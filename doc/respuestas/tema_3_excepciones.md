
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.


**Opción 1: Devolver Valor Especial (código de error)**

En C, una estrategia común es devolver un código de error mediante el valor de retorno. Se puede usar un valor especial como -1 o NaN para indicar que ocurrió un error. El llamador debe verificar este valor y manejar el error explícitamente. Este enfoque requiere que el programador recuerde verificar el resultado después de cada llamada a función.

```c
int raiz(double n, double *resultado) {
    if (n < 0) {
        return -1;  // Código de error
    }
    *resultado = sqrt(n);
    return 0;  // Éxito
}

// Uso:
double res;
if (raiz(-4, &res) == -1) {
    printf("Error: número negativo\n");
}
```

**Opción 2: Parámetro adicional para almacenar un código de error**

Otra alternativa es usar una variable global (como `errno` en C estándar) o pasar un parámetro por referencia para indicar el estado del error. El resultado se devuelve normalmente, pero el estado del error se comunica a través de otro mecanismo. Este enfoque tampoco obliga al compilador a revisar errores, dejando la responsabilidad al programador.

```c
int errno_custom = 0;

double raiz(double n) {
    if (n < 0) {
        errno_custom = VALOR_NEGATIVO;
        return 0;
    }
    return sqrt(n);
}

// Uso:
double resultado = raiz(-4);
if (errno_custom != 0) {
    printf("Error: %d\n", errno_custom);
}
```


## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?


Una **excepción** surge en situaciones atípicas, cuando implementamos. Nos pemiten indicar más plenamente el error cuando llamamos. Nos facilita separar la lógica normal de la situación o manejo de la situación errónea.

El programador utiliza excepciones con dos propósitos principales. Cuando implementa funciones, las lanza para indicar que ha ocurrido una situación anómala que no puede ser resuelta localmente (por ejemplo, un parámetro inválido o un recurso no disponible). Cuando llama a funciones, las captura y maneja para tomar decisiones sobre cómo proceder: intentar una alternativa, informar al usuario, liberar recursos o propagar el error hacia arriba. Este enfoque separa claramente la lógica normal de la lógica de manejo de errores.


## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.


```java
public class Calculadora {
    public double raiz(double n) throws IllegalArgumentException {
        if (n < 0) {
            throw new IllegalArgumentException("El número debe ser positivo");
        }
        return Math.sqrt(n);
    }
}

public class Main {
    public static void main(String[] args) {
        Calculadora calc = new Calculadora();
        try {
            double resultado = calc.raiz(-4);
            System.out.println("Raíz: " + resultado);
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

En Java, la clase `Calculadora` contiene el método `raiz()` que lanza una excepción cuando el número es negativo. El método `main()` llama a este método dentro de un bloque `try-catch`, capturando la excepción del tipo `IllegalArgumentException` si ocurre. Si el número es válido, el programa continúa normalmente. Si es negativo, el flujo de ejecución salta al bloque `catch`, permitiendo manejar el error de manera controlada sin que el programa se bloquee.


## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.


**Lanzar una excepción** significa crear un objeto de excepción e interrumpir el flujo normal del programa, indicando que algo ha salido mal. Se hace con la instrucción `throw`. Por ejemplo, cuando `raiz(-4)` se ejecuta y el número es negativo, se lanza una excepción: `throw new IllegalArgumentException("El número debe ser positivo")`.

**Controlar o capturar una excepción** significa interceptar esa excepción en un bloque `catch` e incluir código para manejarla. En el ejemplo del `main()`, el `try-catch` captura la excepción lanzada por `raiz()`, evitando que el programa termine abruptamente.

**Propagación de una excepción** es el proceso donde la excepción viaja hacia arriba a través de la pila de llamadas. Cuando `raiz()` lanza la excepción, esta no se maneja dentro de `raiz()`, así que sube a `main()`. Si `main()` tampoco la captura (sin `try-catch`), subiría al nivel superior y así sucesivamente hasta que alguien la capture o el programa termine.

**En la pila de llamadas:** si `main()` está arriba y llama a `raiz()`, cuando ocurre el error, `raiz()` no se reanuda. Su ejecución se detiene, se lanza la excepción, y el control pasa directamente a `main()`. Si `main()` tiene `try-catch`, maneja el error; si no, el programa termina. **Las funciones que no caturan la excepción no se reanudan**, simplemente desaparecen del stack cuando la excepción pasa por ellas.


## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?


En C, cada función debe verificar explícitamente el código de error devuelto por la función que llamó y propagarlo hacia arriba, lo que requiere escribir código repetitivo y propenso a errores en múltiples niveles. El programador debe recordar verificar cada valor de retorno, y es fácil olvidar estos controles, dejando errores sin manejar.

La propagación natural de excepciones en Java elimina esta necesidad. Cuando una excepción es lanzada, viaja automáticamente hacia arriba sin necesidad de código explícito en cada nivel. Solo las funciones que deseen manejar un error específico necesitan incluir un bloque `try-catch`; las demás pueden ignorarla deliberadamente permitiendo que continúe propagándose. Esto resulta en código más limpio, más legible y con menos posibilidad de olvidar error handling, ya que el compilador informa sobre excepciones controladas no manejadas.


## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?


Sí, en Java las excepciones son objetos que heredan de la clase `Exception` (o `RuntimeException`). Esto significa que una excepción es una instancia de una clase con propiedades y métodos, no simplemente un código numérico como en C. Esta característica trae importantes ventajas desde el punto de vista de la encapsulación.

La encapsulación de excepciones como objetos permite que estas contengan información estructurada y accesible sobre el error: el tipo específico indica qué clase de problema ocurrió, el mensaje proporciona detalles, y se pueden agregar propiedades adicionales personalizadas. El código que captura la excepción puede acceder a esta información de manera segura y tipada. *(pueden tener métodos y crear clases excepción)*

Sí, es totalmente posible crear excepciones personalizadas extendiendo `Exception` o `RuntimeException`. Por ejemplo, se podría crear `RaizNegativaException` heredando de `Exception`, permitiendo manejar errores particulares del dominio de la aplicación de manera específica. Esto mejora la claridad del código, pues los errores específicos pueden ser capturados y manejados de formas diferentes según su tipo.


## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?


Todo objeto excepción en Java encapsula información esencial que es muy valiosa para el debugging y el manejo del error. La información más importante incluye: el **mensaje descriptivo** (accesible con `getMessage()`) que explica qué salió mal, el **tipo de la excepción** (que identifica la categoría del error), el **stack trace** (accesible con `printStackTrace()`) que muestra la secuencia exacta de llamadas que llevaron al error, indicando ficheros y números de línea exactos.

    a) Un mensaje (getMessage())
    b) La traza de la pila (getStackTrace o printStackTrace)
    c) Opcionalmente, la "causa", es otra excepción que es la verdadera causa

Esta información estructurada es fundamental cuando se desarrolla un programa en Java. En C, si se usa solo un código de error numérico, el programador debe mantener documentación externa sobre qué significa cada número y no obtiene automáticamente la traza de la pila. En Java, simplemente imprimiendo o inspeccionando el objeto excepción, el desarrollador obtiene un panorama completo de qué sucedió y dónde. Además, el compilador garantiza que esta información siempre está disponible, mientras que en C es fácil perder datos sobre el contexto del error.


## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta

Sí, es perfectamente válido y común tener múltiples bloques `catch` después de un mismo bloque `try`. Cada bloque `catch` puede capturar un tipo diferente de excepción, permitiendo manejar distintos errores de formas diferentes.

```java
try {
    double resultado = calc.raiz(numero);
    int valor = Integer.parseInt(texto);
    System.out.println(resultado / valor);
} catch (IllegalArgumentException e) {
    System.out.println("Raíz no válida: " + e.getMessage());
} catch (NumberFormatException e) {
    System.out.println("Formato de número incorrecto");
} catch (ArithmeticException e) {
    System.out.println("División por cero");
}
```

Regarding execution: **solo se ejecuta un bloque `catch`**, el primero que coincida con el tipo de excepción lanzada. Java examina los bloques `catch` en orden de arriba hacia abajo y ejecuta el primero cuyo tipo sea compatible con la excepción. Una vez que se ejecuta un `catch`, el resto se saltan. Esto es importante porque el orden importa: las excepciones más específicas deben estar antes que las más generales.


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta

La solución es usar el bloque `finally`. Este bloque se ejecuta **siempre**, sin importar si ocurrió una excepción o no, y se ejecuta **antes** de que la excepción continúe propagándose. Es ideal para garantizar que se cierren archivos, conexiones a bases de datos, o se liberen otros recursos críticos.

**Ejemplo con `catch` y `finally`:**

```java
try {
    BufferedReader file = new BufferedReader(new FileReader("datos.txt"));
    String linea = file.readLine();
} catch (IOException e) {
    System.out.println("Error al leer: " + e.getMessage());
} finally {
    // Se ejecuta siempre
    if (file != null) {
        file.close();  // Garantizado que se cierra
    }
}
```

**Ejemplo sin `catch`, solo `try-finally`:**

```java
try {
    BufferedReader file = new BufferedReader(new FileReader("datos.txt"));
    String linea = file.readLine();
    // Si hay error, se lanza la excepción...
} finally {
    // ...pero ANTES se ejecuta esto
    if (file != null) {
        file.close();  // Se cierra incluso if there is an exception
    }
    // Luego la excepción continúa propagándose hacia arriba
}
```

En ambos casos, el código de limpieza en `finally` se garantiza que se ejecutará, proporcionando una forma segura de manejar recursos sin importar qué camino tome la ejecución.


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta

Sí, el bloque `finally` puede ir sin `catch`. La estructura válida es `try-finally` o `try-catch-finally`, pero no es obligatorio tener `catch`. Si solo hay `try-finally`, el bloque `finally` se ejecuta siempre antes de que la excepción se propague hacia arriba.

El bloque `finally` se ejecuta **siempre**, independientemente de si ocurrió una excepción o no. Tanto si el código del `try` se completa exitosamente como si se lanza una excepción, el `finally` se ejecuta sin faltar.

Incluso si hay un `return` en el `try`, el bloque `finally` se ejecuta **antes** de que la función retorne. Esto es muy importante:

```java
public static int ejemplo() {
    try {
        return 5;  // Intención de retornar
    } finally {
        System.out.println("Esto se ejecuta primero");  // Se imprime antes del return
    }
}
```

En este caso, se imprime el mensaje del `finally` antes de que la función retorne el valor 5. Esto garantiza que cualquier código de limpieza se ejecute incluso si hay un `return` prematuro en el `try`.


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta

Las **excepciones controladas** (checked exceptions) son aquellas cuya captura es obligatoria. Heredan de `Exception` y el compilador de Java obliga a que sean capturadas con `try-catch` o declaradas con `throws`. Las **excepciones no controladas** (unchecked exceptions) heredan de `RuntimeException` y no obliga el compilador a capturarlas; el programador decide si hacerlo o no.

`RuntimeException` es la clase base de todas las excepciones no controladas. Su propósito es representar errores que típicamente indican bugs en el código (como acceder a un índice fuera de rango) más que condiciones externas inesperadas.

**Ejemplos de excepciones típicas:**
- Controlada: `IOException` (archivo no encontrado), `FileNotFoundException`
- No controlada: `NullPointerException`, `ArrayIndexOutOfBoundsException`, `IllegalArgumentException`

Excepción personalizada controlada:
```java
public class RaizNegativaException extends Exception {
    public RaizNegativaException(String msg) { super(msg); }
}
```

Excepción personalizada no controlada:
```java
public class ParámetroInválidoException extends RuntimeException {
    public ParámetroInválidoException(String msg) { super(msg); }
}
```

**Situaciones donde usar excepciones controladas:**
- Lectura/escritura de ficheros (`IOException`)
- Conexiones de red que pueden fallar
- Acceso a bases de datos
- Parsing de formatos externos

**Situaciones donde usar excepciones no controladas:**
- Validación de parámetros (valor nulo, rango inválido)
- Errores lógicos en el código (índice fuera de rango)
- Precondiciones incumplidas
- Estados inconsistentes detectados en tiempo de ejecución


## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta

`throws` es una cláusula en la firma de un método que indica que dicho método puede lanzar una excepción controlada sin capturarla. Cuando un método declara `throws IOException`, está comunicando al compilador y a los llamadores que es responsabilidad de estos últimos manejar la excepción si desean.

Es una alternativa a capturar porque el compilador de Java requiere que las excepciones controladas sean manejadas de alguna manera: o bien se capturan con `try-catch`, o bien se declaran con `throws`. Un método no puede ignorar una excepción controlada sin hacer una de estas dos cosas. Por lo tanto, `throws` es una forma válida de cumplir con esta obligación, delegando responsabilidad al llamador.

```java
public void leerArchivo(String ruta) throws IOException {
    BufferedReader br = new BufferedReader(new FileReader(ruta));
    String linea = br.readLine();  // FileReader lanza IOException
    // No capturamos, solo declaramos que se puede lanzar
}
```

Esta alternativa es útil cuando el método en cuestión no tiene forma sensata de manejar el error (por ejemplo, es una función de bajo nivel que no sabe qué hacer con un archivo no encontrado), por lo que prefiere que sea el código de nivel superior quien decida cómo proceder.


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta

```java
public class LectorArchivos {
    // El método declara que puede lanzar IOException
    public String leerPrimerapLinea(String ruta) throws IOException {
        BufferedReader br = null;
        try {
            // BufferedReader y FileReader pueden lanzar IOException
            br = new BufferedReader(new FileReader(ruta));
            return br.readLine();
            // Si ocurre excepción aquí, se propaga hacia arriba
        } finally {
            // Se ejecuta siempre, incluso si hay excepción
            if (br != null) {
                br.close();  // Cierra el recurso antes de que la excepción se propague
            }
        }
    }
}

// Quien llama debe manejar la excepción
public static void main(String[] args) {
    LectorArchivos lector = new LectorArchivos();
    try {
        String linea = lector.leerPrimerapLinea("noexiste.txt");
        System.out.println(linea);
    } catch (IOException e) {
        System.out.println("Error: " + e.getMessage());
    }
}
```

En este ejemplo, `leerPrimerapLinea()` declara con `throws IOException` que no va a manejar la excepción localmente. Sin embargo, el bloque `finally` garantiza que se cierre el archivo incluso si ocurre un error, antes de que la excepción se propague hacia `main()`, que ya sí la captura.


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta

Sí, técnicamente es posible declarar una excepción no controlada en `throws`. Java permite escribir `throws RuntimeException` en una firma de método, sin embargo, el compilador no lo requiere ni lo obliga, pues `RuntimeException` no es obligatoria de capturar. La pregunta es si tiene sentido hacerlo.

Tecnicamente no es necesário que el método llamador ponga `try-catch` para excepciones no controladas, pero sí puede hacerlo si lo desea. El compilador no lo exigirá. El sentido de hacerlo sería documentativo y pragmático: al incluir `throws RuntimeException` en la firma, el programador está documentando que el método *puede* lanzar esa excepción, informando así a los usuarios del método que debería considerarse capturarla. Es una forma de comunicación explícita sobre el comportamiento del método.

```java
public double raiz(double n) throws IllegalArgumentException {
    if (n < 0) {
        throw new IllegalArgumentException("Número negativo");
    }
    return Math.sqrt(n);
}

// El llamador PUEDE capturar, pero no está obligado
Double resultado = raiz(-5);  // Sin try-catch, válido

// O puede capturar para evitar el crash
try {
    resultado = raiz(-5);
} catch (IllegalArgumentException e) {
    resultado = 0.0;
}
```

En resumen, es más una convención de documentación que una obligación del lenguaje.


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta

Se recomienda **excepciones controladas** (`IOException`) para condiciones que representan situaciones **externas o del sistema** que pueden ocurrir incluso en código correcto: archivo no encontrado, conexión de red caída, permiso denegado. Estas son situaciones que un programa robusto debe anticipar y manejar. Se recomienda **excepciones no controladas** (`IllegalArgumentException`) para condiciones que representan **errores lógicos o de uso incorrecto**: parámetros inválidos, violación de precondiciones, estado inconsistente. Estas sugieren un bug en el código del llamador.

No todos los lenguajes tienen esta distinción. Java es relativamente único al forzar el manejo de excepciones controladas. C++ tiene excepciones pero todas son opcionales de capturar. Python tiene excepciones pero sin distinción obligatoria entre tipos. En lenguajes como C# (similar a Java), también existe la distinción pero no es tan estricta.

En lenguajes que solo tienen una opción, **la más habitual es la variante "no controlada"** (por defecto, el programador elige si capturar o no). Esto es más pragmático y menos restrictivo, aunque requiere mayor disciplina del programador para manejar errores correctamente. Java fue innovador al forzar el manejo de excepciones controladas, lo que aumenta la robustez pero también la verbosidad del código.


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta

Sí, tiene perfecto sentido lanzar excepciones dentro de un `catch`. El `catch` es solo un bloque de código normal, por lo que se pueden lanzar nuevas excepciones si la situación lo requiere. Esto permite transformar un tipo de excepción en otro, hacer logging y relanzar, o lanzar excepciones completamente nuevas basadas en el error capturado.

**Ejemplo 1: Lanzar una excepción diferente dentro del `catch`**

```java
try {
    BufferedReader br = new BufferedReader(new FileReader("config.txt"));
    String config = br.readLine();
} catch (IOException e) {
    // Captura IOException pero lanza una personalizada
    throw new ConfigurationException("No se pudo cargar la configuración", e);
}
```

**Ejemplo 2: Relanzar la misma excepción capturada**

```java
try {
    BufferedReader br = new BufferedReader(new FileReader("datos.txt"));
    String datos = br.readLine();
} catch (IOException e) {
    System.out.println("Error detectado, registrando...");
    // Log del error localmente
    registrarError(e);
    // Relanza la misma excepción para que otros niveles la manejen
    throw e;
}
```

Relanzar la misma excepción tiene sentido cuando el método quiere realizar algún manejo local (loguear, actualizar estado, liberar recursos) pero reconoce que no puede resolver el problema completamente y necesita que un nivel superior lo maneje. Es un patrón común de logging + delegación.


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

Que una excepción sea la **"causa"** de otra significa que la primera excepción está encadenada dentro de la segunda. Cuando se captura una excepción de bajo nivel (por ejemplo, `IOException`) y se desea relanzar como una excepción personalizada de nivel superior (por ejemplo, `DataLoadException`), se puede pasar la excepción original como causa, creando una cadena que preserva la información original.

Java proporciona dos formas de hacerlo. En el constructor de la excepción se puede pasarla como argumento, o se puede usar `initCause()`:

```java
public class DataLoadException extends Exception {
    public DataLoadException(String msg, Throwable causa) {
        super(msg, causa);  // La excepción original es la causa
    }
}

public class DataLoader {
    public Data cargarDatos(String archivo) throws DataLoadException {
        try {
            BufferedReader br = new BufferedReader(new FileReader(archivo));
            return parsearDatos(br);
        } catch (IOException e) {
            // Captura IOException de bajo nivel
            // La encapsula en una nueva excepción personalizada
            throw new DataLoadException("Fallo al cargar datos de: " + archivo, e);
        }
    }
}
```

**Sí, cuando una excepción sale por pantalla (se imprime sin ser capturada), si tiene una causa, esta se ve.** El método `printStackTrace()` automáticamente imprime la cadena completa de causas. Se vería algo como:

```
DataLoadException: Fallo al cargar datos de: archivo.txt
    at DataLoader.cargarDatos(...)
Caused by: java.io.FileNotFoundException: archivo.txt
    at java.io.FileInputStream.open(...)
```

Esto es extremadamente útil para debugging, pues permite ver tanto el error de alto nivel ("no se pudieron cargar datos") como el error de bajo nivel que lo causó ("archivo no encontrado").

