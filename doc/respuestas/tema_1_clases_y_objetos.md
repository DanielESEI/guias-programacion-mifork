
# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

Las cuatro características fundamentales de la programación orientada a objetos son: **encapsulación**, **herencia**, **polimorfismo** y **abstracción**.

La **encapsulación** consiste en agrupar datos (atributos) y funciones (métodos) que operan sobre esos datos dentro de una misma unidad llamada clase, ocultando los detalles internos de implementación y exponiendo solo lo necesario mediante interfaces públicas. Esto permite proteger la integridad de los datos y facilita el mantenimiento del código.

La **herencia** permite crear nuevas clases basadas en clases existentes, heredando sus atributos y métodos. Esto promueve la reutilización de código y establece relaciones jerárquicas entre clases. El **polimorfismo** permite que objetos de diferentes clases respondan de manera distinta a la misma llamada de método, facilitando la escritura de código más genérico y flexible.

Finalmente, la **abstracción** consiste en modelar entidades del mundo real mediante clases, identificando las características y comportamientos esenciales mientras se omiten los detalles irrelevantes. Esto simplifica la complejidad del sistema al trabajar con conceptos de alto nivel.

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

Cuatro lenguajes populares que soportan programación orientada a objetos son:

- **Java**: Lenguaje puramente orientado a objetos (casi todo es un objeto), multiplataforma gracias a la JVM.
- **Python**: Lenguaje multiparadigma que soporta POO de forma natural y flexible.
- **C++**: Extensión de C que añade soporte para POO, permitiendo combinar programación estructurada y orientada a objetos.
- **C#**: Lenguaje orientado a objetos desarrollado por Microsoft para la plataforma .NET, similar a Java en muchos aspectos.

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

La **programación estructurada** es un paradigma que surgió en los años 60-70 para mejorar la claridad y calidad del código. Se basa en el uso de estructuras de control bien definidas (secuencia, selección con `if/switch`, e iteración con bucles `for/while`) y evita el uso de instrucciones `goto`. Este enfoque facilita la lectura, comprensión y mantenimiento del código al seguir un flujo de ejecución más predecible. Lenguajes como C implementan este paradigma de forma efectiva.

La **programación modular** es una evolución que propone dividir el programa en módulos o unidades independientes, cada uno con una funcionalidad específica y bien definida. Cada módulo encapsula datos y funciones relacionadas, exponiendo solo lo necesario a través de interfaces públicas. Esto permite desarrollar, probar y mantener cada módulo de forma independiente, facilitando el trabajo en equipo y la reutilización de código.

Ambos paradigmas sentaron las bases para la POO: la programación estructurada aportó las estructuras de control que se usan dentro de los métodos, mientras que la programación modular introdujo conceptos de encapsulación y separación de responsabilidades que evolucionaron hacia las clases y objetos.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

Un objeto en programación orientada a objetos se define por tres elementos fundamentales: **identidad**, **estado** y **comportamiento**.

La **identidad** es lo que distingue a un objeto de todos los demás, incluso si tienen el mismo estado. Es similar a la dirección de memoria de una variable en C, algo único que identifica al objeto durante su existencia. El **estado** está determinado por los valores de sus atributos (variables miembro), que pueden cambiar a lo largo del tiempo. Por ejemplo, un objeto de tipo coche podría tener como estado: color rojo, velocidad 60 km/h, motor encendido.

El **comportamiento** se define mediante los métodos (funciones miembro) que puede ejecutar el objeto. Estos métodos pueden consultar o modificar el estado del objeto, o interactuar con otros objetos. Siguiendo el ejemplo del coche, sus comportamientos podrían incluir: acelerar, frenar, girar, etc. Estos tres elementos trabajan juntos para representar entidades del mundo real en el software.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta

Una **clase** es una plantilla o molde que define la estructura y comportamiento que tendrán los objetos creados a partir de ella. Define qué atributos tendrá el objeto (sus datos) y qué métodos podrá ejecutar (sus funciones). Es similar a cómo en C se define un `struct` que especifica los campos que tendrán las variables de ese tipo, pero además incluye las funciones que operan sobre esos datos.

Una clase **no es lo mismo** que un objeto. La clase es la definición abstracta, mientras que el objeto es una entidad concreta creada en memoria a partir de esa clase. Una **instancia** es precisamente eso: un objeto concreto creado a partir de una clase. Por ejemplo, se puede tener la clase `Coche` (la plantilla) y crear varias instancias como `miCoche` y `tuCoche` (objetos concretos en memoria).

No todos los lenguajes orientados a objetos manejan el concepto de clase. Existen lenguajes basados en **prototipos** como JavaScript (aunque las versiones modernas añadieron clases como sintaxis), donde los objetos se crean clonando otros objetos existentes en lugar de instanciar clases. Sin embargo, la mayoría de lenguajes POO populares (Java, C++, C#, Python) sí utilizan el paradigma basado en clases.

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**?? 

### Respuesta

En la mayoría de lenguajes orientados a objetos, los objetos se almacenan en una región de memoria dinámica llamada **heap** (montículo), similar a cuando se usa `malloc()` en C. Al crear un objeto con `new`, se reserva memoria en el heap y se devuelve una referencia (similar a un puntero) a esa ubicación. Esta memoria persiste hasta que el objeto es explícitamente liberado o es eliminado por el recolector de basura.

No es igual en todos los lenguajes. En C++, el programador tiene control total: puede crear objetos en la pila (stack) como variables locales, o en el heap usando `new`, siendo responsable de liberarlos con `delete`. En Java, todos los objetos se crean obligatoriamente en el heap mediante `new`, y las variables locales solo almacenan referencias a esos objetos. En C#, existen los tipos valor (`struct`) que se almacenan en la pila, y los tipos referencia (`class`) que van al heap.

La **recolección de basura** (garbage collection) es un mecanismo automático que gestiona la memoria liberando objetos que ya no son accesibles desde el programa. A diferencia de C donde hay que usar `free()` manualmente, lenguajes como Java, Python o C# incorporan un recolector que periódicamente identifica objetos sin referencias y los elimina, evitando pérdidas de memoria (memory leaks) y liberando al programador de esta responsabilidad. Esto simplifica el desarrollo pero puede introducir pausas impredecibles en la ejecución.

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

Un **método** es una función que pertenece a una clase y define el comportamiento de los objetos de esa clase. A diferencia de las funciones tradicionales de C que existen independientemente, los métodos están asociados a una clase específica y operan sobre los datos (atributos) de los objetos. Cuando se invoca un método sobre un objeto, este tiene acceso implícito a los atributos de ese objeto concreto.

La **sobrecarga de métodos** (method overloading) es la capacidad de definir múltiples métodos con el mismo nombre dentro de una clase, siempre que tengan diferentes firmas (distinto número o tipo de parámetros). El compilador determina qué versión del método invocar según los argumentos proporcionados. Por ejemplo, una clase `Calculadora` podría tener varios métodos `sumar(int a, int b)`, `sumar(double a, double b)` y `sumar(int a, int b, int c)`, y el compilador elegiría el apropiado según los parámetros usados en cada llamada.

Esta característica mejora la legibilidad del código al permitir usar nombres descriptivos consistentes para operaciones similares, en lugar de crear nombres artificialmente diferentes como `sumarEnteros()`, `sumarReales()`, etc. Java soporta sobrecarga de métodos, mientras que C no permite tener múltiples funciones con el mismo nombre.

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

```java
class Punto {
    double x;
    double y;
    
    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}
```

La clase `Punto` define dos atributos `x` e `y` de tipo `double` que representan las coordenadas del punto. El método `calculaDistanciaAOrigen()` calcula la distancia euclidiana desde el punto hasta el origen (0,0) usando el teorema de Pitágoras. `Math.sqrt()` es una función de la biblioteca estándar de Java que calcula la raíz cuadrada.

Ejemplo de uso:

```java
public class PruebaPunto {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3.0;
        p.y = 4.0;
        
        double distancia = p.calculaDistanciaAOrigen();
        System.out.println("La distancia al origen es: " + distancia);
    }
}
```

En este ejemplo se crea una instancia de `Punto` usando `new Punto()`, se asignan valores a sus atributos `x` e `y`, y se invoca el método `calculaDistanciaAOrigen()` que devuelve 5.0 (ya que $\sqrt{3^2 + 4^2} = \sqrt{25} = 5$).

## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

El punto de entrada de un programa en Java es el método `public static void main(String[] args)`. Cuando se ejecuta un programa Java, la JVM (Java Virtual Machine) busca este método específico en la clase indicada y comienza la ejecución desde ahí. Es el equivalente a la función `main()` de C, pero con la particularidad de que debe estar dentro de una clase.

La palabra clave `static` indica que un método o atributo pertenece a la clase en sí, no a las instancias individuales. Esto significa que se puede invocar sin necesidad de crear un objeto. Por ejemplo, `Math.sqrt()` es un método estático que se llama directamente desde la clase `Math` sin crear una instancia. El método `main` debe ser `static` porque la JVM lo ejecuta antes de crear cualquier objeto. No solo se usa para `main`: se pueden crear otros métodos y atributos estáticos cuando se necesita funcionalidad o datos compartidos por todas las instancias de una clase.

Cuando se combina `static` con `final`, se crean **constantes de clase**. `final` significa que el valor no puede cambiar después de su inicialización. Por ejemplo, `public static final double PI = 3.14159;` crea una constante que pertenece a la clase (no a instancias individuales) y cuyo valor es inmutable. Esto es útil para definir valores que son iguales para todos los objetos y que no deben modificarse, similar a `#define` o `const` en C.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

Para compilar un programa Java desde línea de comandos se usa `javac NombreArchivo.java`, que genera archivos `.class`. Para ejecutar se usa `java NombreClase` (sin extensión). Por ejemplo: `javac PruebaPunto.java` genera `PruebaPunto.class` y `Punto.class`, luego `java PruebaPunto` ejecuta el programa. Es importante que el nombre del archivo coincida con el nombre de la clase pública que contiene.

Java es un lenguaje **semicompilado**. A diferencia de C que compila directamente a código máquina nativo del procesador, Java compila a un formato intermedio llamado **bytecode**. Este bytecode no es código máquina de ningún procesador específico, sino instrucciones para la **Máquina Virtual de Java** (JVM). Los archivos `.class` contienen este bytecode, que es portátil entre diferentes sistemas operativos y arquitecturas.

La **JVM** es un programa que interpreta y ejecuta el bytecode. Actúa como una capa de abstracción entre el programa Java y el sistema operativo real. Esto permite que el mismo archivo `.class` se ejecute sin modificaciones en Windows, Linux, Mac, etc., siempre que tengan una JVM instalada. Es el principio "write once, run anywhere" de Java.

El bytecode es más compacto y rápido de interpretar que el código fuente original, pero más lento que código máquina nativo. Las JVM modernas incluyen compiladores JIT (Just-In-Time) que convierten bytecode a código nativo durante la ejecución para mejorar el rendimiento.

## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

`new` es el operador que crea una nueva instancia de una clase, reservando memoria en el heap para el objeto. Es equivalente a `malloc()` en C, pero además de reservar memoria, inicializa el objeto llamando a su constructor. Devuelve una referencia (similar a un puntero) al objeto recién creado en memoria.

Un **constructor** es un método especial que se ejecuta automáticamente cuando se crea un objeto con `new`. Su propósito es inicializar el estado del objeto, asignando valores iniciales a sus atributos. El constructor tiene el mismo nombre que la clase y no especifica tipo de retorno (ni siquiera `void`). Si no se define ningún constructor, Java proporciona automáticamente uno por defecto sin parámetros que inicializa los atributos con valores predeterminados (0 para números, `null` para referencias, etc.).

Ejemplo de la clase `Empleado` con constructor:

```java
class Empleado {
    String dni;
    String nombre;
    String apellidos;
    
    // Constructor
    Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}

// Uso:
Empleado emp = new Empleado("12345678A", "Juan", "García López");
```

En este ejemplo, el constructor recibe tres parámetros y los asigna a los atributos del objeto. Al usar `new Empleado(...)`, se crea el objeto y se inicializan sus datos en una sola operación.

## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

`this` es una referencia implícita al objeto actual sobre el cual se está ejecutando un método. Es similar a un puntero al objeto mismo, permitiendo acceder a sus atributos y métodos. Se usa principalmente cuando hay ambigüedad entre nombres de parámetros y atributos, o cuando se necesita pasar el objeto actual como argumento a otro método.

No se llama igual en todos los lenguajes orientados a objetos. En Java y C++ se usa `this`, en Python se usa `self` (aunque es solo una convención, técnicamente puede tener cualquier nombre), y en algunos lenguajes como Smalltalk no es necesario especificarlo explícitamente. Sin embargo, el concepto es universal: siempre existe una forma de referirse al objeto actual.

Ejemplo de uso de `this` en la clase `Punto`:

```java
class Punto {
    double x;
    double y;
    
    // Constructor con parámetros del mismo nombre que los atributos
    Punto(double x, double y) {
        this.x = x;  // this.x es el atributo, x es el parámetro
        this.y = y;  // this.y es el atributo, y es el parámetro
    }
    
    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }
}
```

En el constructor, `this.x` se refiere al atributo del objeto, mientras que `x` (sin `this`) se refiere al parámetro. Sin `this`, habría ambigüedad y los atributos no se inicializarían correctamente.

## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

```java
class Punto {
    double x;
    double y;
    
    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }
    
    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
    
    double distanciaA(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

El método `distanciaA` recibe como parámetro otro objeto de tipo `Punto` y calcula la distancia euclidiana entre el punto actual (`this`) y el punto recibido (`otro`). Para ello, calcula las diferencias en las coordenadas x e y, y aplica el teorema de Pitágoras: $d = \sqrt{(x_1 - x_2)^2 + (y_1 - y_2)^2}$.

Ejemplo de uso:

```java
Punto p1 = new Punto(1.0, 2.0);
Punto p2 = new Punto(4.0, 6.0);

double dist = p1.distanciaA(p2);  // Calcula distancia entre p1 y p2
System.out.println("Distancia: " + dist);  // Resultado: 5.0
```

En este ejemplo, `p1.distanciaA(p2)` calcula la distancia desde p1 hasta p2. Dentro del método, `this` se refiere a `p1` y `otro` se refiere a `p2`.

## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

En Java, todos los parámetros se pasan **por valor**, pero esto puede resultar confuso al principio. Cuando se pasa un objeto como `Punto`, lo que realmente se copia es la **referencia** (similar a copiar un puntero en C), no el objeto completo. Esto significa que si dentro del método se modifican los atributos del objeto (`otro.x = 10;`), estos cambios **sí afectan** al objeto original fuera del método, porque ambas referencias apuntan al mismo objeto en memoria.

Sin embargo, si se intenta cambiar la propia referencia dentro del método (`otro = new Punto(0, 0);`), esto **no afecta** a la variable fuera del método, porque solo se está modificando la copia local de la referencia. Es como si en C se pasara un puntero por valor: se puede modificar el contenido apuntado (`*p = 5`), pero no se puede hacer que el puntero original apunte a otra dirección.

Para tipos primitivos como `int`, `double`, `boolean`, etc., se pasa una copia del valor. Si se modifica el parámetro dentro del método (`n = 100;`), este cambio **no afecta** a la variable original fuera del método. Solo existe la copia local del valor, similar al comportamiento tradicional de C.

En resumen: en Java los objetos se pasan "por referencia de valor" (la referencia se copia) y los tipos primitivos se pasan por valor (el valor se copia). Las modificaciones a los atributos de objetos sí persisten, pero las reasignaciones de parámetros no.

## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

`toString()` es un método especial en Java que devuelve una representación textual del objeto en forma de `String`. Es llamado automáticamente cuando se intenta imprimir un objeto con `System.out.println()` o cuando se concatena un objeto con una cadena. Todas las clases en Java heredan este método de la clase `Object`, pero el comportamiento por defecto solo muestra el nombre de la clase y la dirección de memoria, lo cual no es muy útil.

Por eso es común **sobrescribir** (override) este método para proporcionar una representación más informativa del objeto. Al implementar `toString()` personalizado, se puede controlar exactamente qué información del objeto se muestra y en qué formato. Esto facilita enormemente la depuración y el logging del programa.

Ejemplo de `toString()` en la clase `Punto`:

```java
class Punto {
    double x;
    double y;
    
    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }
    
    @Override
    public String toString() {
        return "Punto(" + x + ", " + y + ")";
    }
}

// Uso:
Punto p = new Punto(3.0, 4.0);
System.out.println(p);  // Imprime: Punto(3.0, 4.0)
```

Muchos lenguajes orientados a objetos tienen métodos equivalentes: Python tiene `__str__()` y `__repr__()`, C# tiene `ToString()`, JavaScript tiene `toString()`, etc. Es un patrón común en POO.

## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?

Una clase tiene similitudes con un `struct` de C en el sentido de que ambos agrupan datos relacionados, pero una clase va mucho más allá. El `struct` solo define la estructura de datos (qué campos contiene), mientras que una clase combina datos (atributos) y funciones (métodos) que operan sobre esos datos en una sola unidad cohesiva.

Lo que le falta al `struct` para ser una clase completa incluye: (1) **métodos integrados** - en C las funciones existen separadas de los structs, mientras que en POO los métodos son parte de la clase; (2) **encapsulación** - no hay manera de ocultar campos o controlar el acceso en un struct, todos son públicos por defecto; (3) **constructores y destructores** - no hay inicialización automática garantizada; (4) **herencia** - no se pueden crear structs derivados que hereden campos y funciones de otros structs.

Además, faltan conceptos como sobrecarga de operadores, polimorfismo, métodos virtuales, y toda la maquinaria que permite la POO. En esencia, el `struct` proporciona solo la parte de "datos agrupados" de una clase, pero no el comportamiento, la protección de datos, ni las relaciones jerárquicas que caracterizan a la POO. C++ de hecho reconoce esto: en C++, `struct` y `class` son casi idénticos, la única diferencia es que `struct` tiene visibilidad pública por defecto mientras que `class` tiene privada.

## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta
```c
#include <stdio.h>
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

// Función que emula el método
double calculaDistanciaAOrigen(Punto* this) {
    return sqrt(this->x * this->x + this->y * this->y);
}

int main() {
    Punto p;
    p.x = 3.0;
    p.y = 4.0;
    
    double dist = calculaDistanciaAOrigen(&p);
    printf("Distancia: %f\n", dist);
    return 0;
}
```

Para emular una clase en C, se define un `struct` con los atributos y funciones separadas que reciben un puntero al struct. Lo que en POO es implícito (`this`), en C debe hacerse **explícito**: el primer parámetro de la función es un puntero a la estructura, que representa el objeto sobre el que opera. Esta es la clave: `this` no desaparece, simplemente se convierte en un parámetro explícito.

De hecho, así es aproximadamente cómo los compiladores de C++ implementan internamente los métodos: los convierten en funciones normales que reciben un puntero oculto `this` como primer argumento. La diferencia es que en POO este mecanismo está integrado en el lenguaje, proporcionando sintaxis más limpia y verificación de tipos en tiempo de compilación.

Esta emulación muestra que la POO no es "magia", sino una abstracción del lenguaje que organiza código y datos de forma más natural y segura. Lenguajes como C pueden simular algunos aspectos de POO, pero requieren disciplina manual del programador para mantener las convenciones, mientras que lenguajes POO las hacen cumplir automáticamente.