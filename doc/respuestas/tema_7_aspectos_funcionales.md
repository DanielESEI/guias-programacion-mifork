
# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

Las funciones son "ciudadanos de primera clase". Una función es un tipo más:

    - Se puede asignar a una variable.
    - Se puede pasar como parámetro.
    - Se puede devolver una función como retorno de otra.
  
- Closure.
- Expresiones Lambda. *(sintaxis) - no tienen nombre*
- En lenguajes con comprobación estática de tipos: ¿Qué tipo tienen?

Un puntero a una función es una variable que almacena la dirección de memoria de una función en C, permitiendo invocar la función de forma indirecta a través del puntero. Esto es similar a los punteros a datos en C, pero en lugar de apuntar a variables, apuntan a bloques de código ejecutable. Esta característica es fundamental para implementar comportamientos dinámicos, ya que permite pasar funciones como parámetros a otras funciones o asignarlas a variables para ser invocadas posteriormente.

La sintaxis para declarar un puntero a función tiene la forma: `tipo_retorno (*nombre_puntero)(tipo_param1, tipo_param2, ...)`. El nombre del puntero va entre paréntesis y se antepone el operador `*` para indicar que es un puntero. Para invocar la función a través del puntero, se utiliza la notación `(*puntero)(argumentos)` o simplemente `puntero(argumentos)`.

```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>

char* toUppercase(char str[]) {
    for (int i = 0; str[i] != '\0'; i++) {
        str[i] = toupper(str[i]);
    }
    return str;
}

int main() {
    // Declarar puntero a función
    char* (*aMayusculas)(char[]) = toUppercase;
    
    char cadena[] = "hola mundo";
    aMayusculas(cadena);
    printf("%s\n", cadena);  // Imprime: HOLA MUNDO
    
    return 0;
}
```


## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.


Una función lambda es una función anónima (sin nombre) que puede ser definida de forma compacta dentro del código, generalmente en una sola expresión. A diferencia de los punteros a funciones en C que hacen referencia a funciones nombradas previamente definidas, las lambdas permiten definir y usar funciones sin necesidad de declararlas formalmente en otro lugar. Las lambdas son especialmente útiles en lenguajes funcionales o multi-paradigma, ya que permiten escribir código más conciso y expresivo.

En JavaScript, las funciones lambda (llamadas "arrow functions") se definen con la sintaxis `(parametros) => { cuerpo }`. En Java 8 y posteriores, las funciones lambda tienen la sintaxis `(parametros) -> { cuerpo }` y deben asociarse con una interfaz funcional que defina su tipo. Ambas permiten capturar el contexto en el que se definen, lo que se conoce como "closure".

```javascript
// JavaScript
const aMayusculas = str => str.toUpperCase();
console.log(aMayusculas("hola mundo"));  // HOLA MUNDO
```

```java
// Java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        Function<String, String> aMayusculas = str -> str.toUpperCase();
        System.out.println(aMayusculas.apply("hola mundo"));  // HOLA MUNDO
        // apply recibe un String y devuelve un String
        // apply sólo para funciones (no void)
    }
}
```


## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?


El paradigma funcional es un enfoque de programación que trata los programas como la evaluación de funciones matemáticas, enfatizando la inmutabilidad de datos y evitando cambios de estado. Contrasta con el paradigma imperativo (como C, donde se dan instrucciones paso a paso) y el paradigma orientado a objetos (como Java clásico, donde se organizan datos y comportamiento en objetos). En lugar de modificar variables, el paradigma funcional utiliza transformaciones de datos a través de funciones que devuelven nuevos valores.

Java 8 se denomina "multi-paradigma" porque incorporó características funcionales (lambdas, streams, interfaces funcionales) manteniendo su naturaleza orientada a objetos. Esto permite a los desarrolladores elegir qué enfoque es más apropiado para cada problema: usar OOP para modelar entidades del dominio y usar funcionalidad para transformaciones de datos.

Quiere decir que las funciones son "ciudadanos de primera clase" que pueden ser tratadas como cualquier otro valor en el lenguaje: pueden ser asignadas a variables, pasadas como parámetros a otras funciones, devueltas como resultados de funciones, y almacenadas en estructuras de datos. Esto es lo que diferencia a un lenguaje verdaderamente funcional de uno que solo tiene funciones como construcciones de control. En JavaScript y Python, las funciones ya eran ciudadanos de primera clase incluso antes de que Java adoptara este concepto formalmente.


## 4. Explica la sintaxis básica de una función lambda en Java.


La sintaxis básica de una función lambda en Java es: `(parametros) -> { cuerpo }`. Los parámetros van entre paréntesis separados por comas, seguidos del operador flecha `->` y finalmente el cuerpo de la función. Si la lambda tiene un solo parámetro, se pueden omitir los paréntesis alrededor del parámetro. Si el cuerpo es una única expresión que devuelve un valor, se pueden omitir las llaves y el `return`, escribiendo directamente la expresión.

Por ejemplo, `(x, y) -> x + y` es una lambda que suma dos números, mientras que `str -> str.length()` es una lambda que obtiene la longitud de una cadena. Si el cuerpo requiere múltiples sentencias, se escriben entre llaves: `(x) -> { int resultado = x * 2; return resultado; }`. La lambda no tiene nombre (es anónima) y su tipo está determinado por el contexto en el que se utiliza, es decir, por la interfaz funcional a la que se asigna.

```java
// Diferentes variantes de sintaxis
Function<Integer, Integer> cuadrado = x -> x * x;  // Un parámetro, sin paréntesis
BiFunction<Integer, Integer, Integer> suma = (x, y) -> x + y;  // Dos parámetros
Consumer<String> imprimir = str -> System.out.println(str);  // Sin retorno
Function<String, Boolean> esVacia = str -> {  // Cuerpo con varias líneas
    return str == null || str.isEmpty();
};
```


## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.


Rebir funciones como parámetros es uno de los principios fundamentales de la programación funcional. Permite crear funciones genéricas que pueden aplicar diferentes transformaciones sin necesidad de conocer detalles específicos de cómo se realizará la transformación. El método `transformar` actúa como una función de orden superior (una función que recibe otras funciones como parámetros), lo que posibilita crear código altamente reutilizable y flexible.

En Java, el método `transformar` debe tener un parámetro de tipo interfaz funcional (en este caso `Function<String, String>`) para poder recibir cualquier función que transforme un String en otro String. En JavaScript, no es necesario declarar el tipo explícitamente, ya que el lenguaje es dinámicamente tipado. La función se invoca dentro del método utilizando la notación correspondiente a cada lenguaje.

```java
// Java
import java.util.function.Function;

public class Transformador {
    public static String transformar(String texto, Function<String, String> funcion) {
        return funcion.apply(texto);
    }
    
    public static void main(String[] args) {
        Function<String, String> aMayusculas = str -> str.toUpperCase();
        System.out.println(transformar("hola", aMayusculas));  // HOLA
    }
}
```

```javascript
// JavaScript
function transformar(texto, funcion) {
    return funcion(texto);
}

const aMayusculas = str => str.toUpperCase();
console.log(transformar("hola", aMayusculas));  // HOLA
```


## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.


Esta es una de las características más poderosas de las funciones lambda: su capacidad de ser definidas "inline" (en línea) directamente en el punto de uso, sin necesidad de asignarlas previamente a una variable. Esto conduce a código más conciso y expresivo, evitando la creación de variables intermedias innecesarias. En lugar de crear primero una función y luego pasarla como parámetro, la lambda se define directamente en el argumento de la llamada al método.

Esta práctica es especialmente útil cuando la función solo se usa una vez, ya que no tiene sentido crear una variable para almacenarla. El compilador o intérprete infiere automáticamente el tipo de la lambda a partir del contexto (en este caso, el parámetro de tipo `Function<String, String>` esperado por el método `transformar`).

```java
// Java
public class Main {
    public static String transformar(String texto, Function<String, String> funcion) {
        return funcion.apply(texto);
    }
    
    public static void main(String[] args) {
        // Lambda inline para invertir la cadena
        System.out.println(transformar("hola", str -> {
            return new StringBuilder(str).reverse().toString();
        }));  // aloh
    }
}
```

```javascript
// JavaScript
function transformar(texto, funcion) {
    return funcion(texto);
}

// Lambda inline para invertir la cadena
console.log(transformar("hola", str => str.split('').reverse().join('')));  // aloh
```


## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.


Un closure (cierre) es la capacidad de una función lambda (o cualquier función anónima) de "capturar" y acceder a variables del contexto en el que fue definida, incluso después de que ese contexto haya desaparecido. Esto permite que la lambda "recuerde" el estado de variables externas en el momento de su creación. En C con punteros a funciones, esto no es posible de forma segura, ya que las funciones no pueden recordar referencias a variables locales. Los closures son fundamentales en lenguajes funcionales y multi-paradigma como Java 8+, JavaScript y Python.

En Java, las variables capturadas por una lambda deben ser "effectivamente finales" (effectively final), lo que significa que no pueden ser modificadas después de su asignación inicial. Esto garantiza que el closure sea seguro y predecible. JavaScript no tiene esta restricción, por lo que permite modificar variables capturadas, lo que puede llevar a comportamientos inesperados si no se tienen cuidado.

```java
// Java
import java.util.function.Function;

public class Main {
    public static String transformar(String texto, Function<String, String> funcion) {
        return funcion.apply(texto);
    }
    
    public static void main(String[] args) {
        String sufijo = " - transformado";  // Variable local
        
        // Lambda que captura la variable 'sufijo' (closure)
        Function<String, String> agregarSufijo = str -> str + sufijo;
        
        System.out.println(transformar("hola", agregarSufijo));  // hola - transformado
    }
}
```


## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?


La diferencia fundamental entre lambdas y punteros a funciones en C radica en la capacidad de los closures. Los punteros a funciones en C solo pueden apuntar a funciones nombradas que están predefinidas en el código, y estas funciones no pueden acceder a variables locales del contexto desde el cual se obtiene el puntero. Las lambdas, por otro lado, son funciones anónimas que pueden ser definidas "on-the-fly" y capturar variables del contexto en el que se crean, formando un closure.

Otra diferencia importante es la sintaxis y la flexibilidad. Los punteros a funciones en C requieren una declaración explícita del tipo completo (tipo de retorno y tipos de parámetros), lo que los hace más verbosos. Las lambdas en Java, JavaScript y otros lenguajes modernos permiten una sintaxis más compacta, y a menudo el tipo puede ser inferido por el compilador o intérprete. Además, los closures hacen que las lambdas sean mucho más expresivas y útiles para crear comportamientos dinámicos, ya que pueden "recordar" el estado del contexto en el que fueron definidas, algo que los punteros a funciones en C no pueden hacer de forma segura.


## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.


Devolver funciones como resultado es otro aspecto clave de la programación funcional y demuestra el poder de los closures. La función `crearDescuento` es una función de orden superior que recibe un parámetro (el porcentaje) y devuelve una nueva función que incorpora ese parámetro en su comportamiento. Cada función devuelta es diferente dependiendo del porcentaje pasado, y "recuerda" ese porcentaje a través del closure, incluso después de que `crearDescuento` haya finalizado su ejecución.

En este ejemplo, el closure es esencial: cada función descuento captura el valor específico del `porcentaje` que fue pasado a `crearDescuento`. Cuando se llama a la función descuento posteriormente, esta utiliza el valor capturado de `porcentaje` que fue congelado en el momento de su creación. Esto permite crear múltiples funciones descuento con diferentes porcentajes, cada una independiente de las otras, almacenando su propio valor de porcentaje.

```java
import java.util.function.Function;

public class Main {
    public static Function<Double, Double> crearDescuento(double porcentaje) {
        return precio -> precio * (1 - porcentaje / 100);
    }
    
    public static void main(String[] args) {
        Function<Double, Double> descuento10 = crearDescuento(10);
        Function<Double, Double> descuento20 = crearDescuento(20);
        
        double precio = 100.0;
        System.out.println("Precio original: " + precio);
        System.out.println("Con 10% descuento: " + descuento10.apply(precio));  // 90.0
        System.out.println("Con 20% descuento: " + descuento20.apply(precio));  // 80.0
    }
}
```


## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?


Una interfaz funcional es una interfaz de Java que define exactamente un método abstracto. Este método abstracto define la firma (parámetros y tipo de retorno) que debe tener cualquier lambda que se asigne a esa interfaz. Las interfaces funcionales actúan como tipos para las lambdas, permitiendo que Java verifique en tiempo de compilación que una lambda es compatible con el contexto donde se usa. Aunque una interfaz funcional puede tener múltiples métodos concretos (como métodos estáticos o métodos `default`), solo debe tener un método abstracto.

Los requisitos para una interfaz funcional son: (1) debe ser una interfaz de Java, (2) debe tener exactamente un método abstracto no heredado, (3) puede tener métodos `default` y métodos `static`, y (4) debe anotarse opcionalmente con `@FunctionalInterface` para que el compilador verifique que cumple con la definición. Cuando una lambda se asigna a una interfaz funcional, el compilador verifica que los parámetros y el tipo de retorno de la lambda coincidan con los del método abstracto de la interfaz.

```java
// Interfaz funcional correcta
@FunctionalInterface // opcional
public interface Transformador {
    String transformar(String entrada);
}

// Uso con lambda
Transformador mayusculas = str -> str.toUpperCase();
System.out.println(mayusculas.transformar("hola"));  // HOLA
```


## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).


Crear interfaces funcionales personalizadas es útil cuando se necesita un comportamiento específico que no es proporcionado por las interfaces funcionales predefinidas de Java. Al definir una interfaz funcional con nombre significativo, el código se vuelve más legible y autodocumentado, ya que el nombre comunica claramente la intención de la interfaz. Para la transformación de cadenas, una interfaz `Transformador` es mucho más expresiva que usar directamente `Function<String, String>`.

La interfaz se define con un único método abstracto que especifica la firma esperada. La anotación `@FunctionalInterface` es opcional pero recomendada, ya que le indica al compilador que esta interfaz es funcional y genera un error si se modifica de forma que viole esta condición. Cualquier lambda que cumpla con la firma del método abstracto puede asignarse a una variable del tipo de la interfaz.

```java
@FunctionalInterface
public interface Transformador {
    String transformar(String entrada);
}

public class Main {
    public static void main(String[] args) {
        Transformador mayusculas = str -> str.toUpperCase();
        Transformador minusculas = str -> str.toLowerCase();
        
        System.out.println(mayusculas.transformar("hola"));  // HOLA
        System.out.println(minusculas.transformar("HOLA"));  // hola
    }
}
```


## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.


Al combinar interfaces funcionales con generics, se logra crear abstracciones altamente reutilizables que pueden trabajar con cualquier tipo de datos. La interfaz `Transformador<T, R>` con dos parámetros de tipo permite transformar de un tipo `T` a un tipo `R`, proporcionando una solución genérica para cualquier conversión de tipos. Esto es mucho más flexible que tener interfaces separadas para cada combinación de tipos, y sigue el principio DRY (Don't Repeat Yourself).

La sintaxis para declarar una interfaz funcional genérica es similar a cualquier otra interfaz genérica: se incluyen los parámetros de tipo entre ángulos después del nombre de la interfaz. El método abstracto utiliza estos parámetros de tipo para definir su firma. Al usar la interfaz, se especifican los tipos concretos que se desean, y el compilador verifica que la lambda sea compatible con esos tipos.

```java
@FunctionalInterface
public interface Transformador<T, R> {
    R transformar(T entrada);
}

public class Main {
    public static void main(String[] args) {
        // Transformador que convierte Double a Integer
        Transformador<Double, Integer> redondear = num -> Math.round(num).intValue();
        
        System.out.println(redondear.transformar(3.7));   // 4
        System.out.println(redondear.transformar(2.3));   // 2
        
        // Transformador que convierte String a Integer
        Transformador<String, Integer> longitud = str -> str.length();
        System.out.println(longitud.transformar("hola"));  // 4
    }
}
```


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta

Java proporciona un conjunto de interfaces funcionales predefinidas en el paquete `java.util.function` para cubrir los casos de uso más comunes, evitando que los programadores tengan que crear sus propias interfaces funcionales. Estas interfaces están diseñadas para ser genéricas y reutilizables, con nombres que comunican claramente su propósito. Las más importantes son: `Function<T, R>` (transforma T a R), `Consumer<T>` (consume un valor T sin devolver nada), `Supplier<T>` (suministra un valor T sin recibir parámetros), `Predicate<T>` (prueba si T cumple una condición, devuelve boolean), y `BiFunction<T, U, R>` (transforma dos valores T y U a R).

Otras interfaces funcionales útiles incluyen `BiConsumer<T, U>` (consume dos valores), `BiPredicate<T, U>` (prueba con dos valores), `UnaryOperator<T>` (transforma T a T), y `BinaryOperator<T>` (combina dos valores T a T). También existen variantes especializadas para tipos primitivos como `IntFunction<R>`, `DoubleConsumer`, etc., que evitan el auto-boxing y son más eficientes. Utilizar estas interfaces predefinidas es preferible a crear nuevas interfaces, a menos que se necesite un nombre específico que comunique mejor la intención del código.

```java
import java.util.function.*;

public class Main {
    public static void main(String[] args) {
        Function<String, Integer> longitud = str -> str.length();
        Consumer<String> imprimir = str -> System.out.println(str);
        Supplier<Double> aleatorio = () -> Math.random();
        Predicate<Integer> esPositivo = num -> num > 0;
        BiFunction<Integer, Integer, Integer> suma = (a, b) -> a + b;
        
        System.out.println(longitud.apply("hola"));        // 4
        imprimir.accept("mundo");                           // mundo
        System.out.println(aleatorio.get());                 // número aleatorio
        System.out.println(esPositivo.test(5));              // true
        System.out.println(suma.apply(3, 4));                // 7
    }
}
```


## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta

El método `forEach` es una alternativa funcional al bucle `for` tradicional que permite recorrer una colección pasando una función (lambda) que se aplicará a cada elemento. En lugar de escribir el bucle explícitamente con índices o iteradores, `forEach` maneja internamente la iteración y se enfoca en qué hacer con cada elemento. Esto resulta en código más conciso y declarativo, donde se expresa la intención (qué hacer) en lugar de los detalles de implementación (cómo iterar).

El método `forEach` acepta un parámetro de tipo `Consumer<? super T>`, que es una interfaz funcional que recibe un elemento y no devuelve nada. La lambda que se pase a `forEach` se ejecutará una vez por cada elemento de la colección. En el ejemplo, la lambda recibe cada `Integer` y verifica si es positivo, mostrando un mensaje solo en ese caso.

```java
import java.util.Arrays;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<Integer> numeros = Arrays.asList(-3, 5, -1, 8, 0, -2, 7);
        
        // Versión tradicional con for
        for (Integer num : numeros) {
            if (num > 0) {
                System.out.println("Número positivo: " + num);
            }
        }
        
        // Versión funcional con forEach
        numeros.forEach(num -> {
            if (num > 0) {
                System.out.println("Número positivo: " + num);
            }
        });
    }
}
```

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta

**PECS** es un acrónimo que significa "Producer Extends, Consumer Super" y es una regla mnemotécnica para decidir cuándo usar `extends` y `super` en los comodines de genericidad (`?`). La regla establece que si se va a extraer datos de una estructura genérica (es un productor), se debe usar `extends` para especificar un tipo superior. Si se van a insertar datos en una estructura genérica (es un consumidor), se debe usar `super` para especificar un tipo inferior. Esto maximiza la flexibilidad del código permitiendo tanto tipos exactos como tipos más generales.

En el caso de `forEach`, se usa `Consumer<? super T>` porque `forEach` solo está consumiendo (llamando) al consumer pasado, nunca intentando obtener valores de él. Esto permite pasar un `Consumer<Object>` incluso si la lista contiene `String`, ya que un `Consumer<Object>` puede consumir cualquier `String`. Para el método `transformar`, si solo se necesita invocar la función pero no usarla como productor de múltiples valores, podrían mejorarse la firma usando `Function<? super String, ? extends String>` para mayor flexibilidad, aunque típicamente no es necesario en este caso.

```java
import java.util.function.Function;
import java.util.function.Consumer;

public class Main {
    // Método mejorado con PECS
    public static <T, R> R transformar(T entrada, Function<? super T, ? extends R> funcion) {
        return funcion.apply(entrada);
    }
    
    public static void main(String[] args) {
        // Un Consumer<Object> puede usarse con forEach de List<String>
        Consumer<Object> imprimidor = obj -> System.out.println(obj);
        java.util.List<String> palabras = java.util.Arrays.asList("hola", "mundo");
        palabras.forEach(imprimidor);  // Funciona porque Object es supertipo de String
    }
}
```

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta

Una referencia a método es similar a una lambda, pero en lugar de escribir el cuerpo de la función, se hace referencia a un método existente. Esto es útil cuando ya existe un método que realiza exactamente lo que se necesita, evitando escribir una lambda innecesaria. En Java, la sintaxis para obtener una referencia a un método de instancia es `objeto::nombreMetodo`, y se puede asignar a una variable de tipo interfaz funcional compatible. En JavaScript, las funciones son objetos de primera clase, por lo que obtener una referencia a un método es tan simple como asignar el método a una variable.

Las referencias a métodos hacen que el código sea más legible y reutilizable, especialmente cuando se pasan como parámetros a funciones de orden superior. El compilador de Java verifica que el método referenciado sea compatible con la interfaz funcional a la que se asigna, es decir, que tenga los mismos parámetros y tipo de retorno.

```java
// Java
public class Persona {
    private String nombre;
    
    public Persona(String nombre) {
        this.nombre = nombre;
    }
    
    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

public class Main {
    public static void main(String[] args) {
        Persona persona = new Persona("Juan");
        Runnable saludo = persona::saludar;  // Referencia al método
        saludo.run();  // Invoca saludar()  -> Hola, soy Juan
    }
}
```

```javascript
// JavaScript
class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }
    
    saludar() {
        console.log("Hola, soy " + this.nombre);
    }
}

const persona = new Persona("Juan");
const saludo = persona.saludar;  // Referencia al método
// En JavaScript, generalmente se hace: const saludo = () => persona.saludar();
// para mantener el contexto 'this'
persona.saludar();  // Invoca saludar()  -> Hola, soy Juan
```


## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta

Java proporciona cuatro tipos de referencias a métodos, cada una con un propósito diferente. Las referencias a métodos estáticos tienen la sintaxis `Clase::metodoEstatico` y se utilizan para acceder a métodos que no dependen de ninguna instancia. Las referencias a constructores tienen la sintaxis `Clase::new` y crean nuevas instancias de la clase. Las referencias a métodos de instancia concretos tienen la sintaxis `instancia::metodo` y vinculan el método a una instancia específica. Las referencias a métodos de instancia de cualquier instancia tienen la sintaxis `Clase::metodo` y permiten que se especifique la instancia cuando se invoca la referencia.

Esta última variante es especialmente útil cuando se trabaja con colecciones o flujos de datos, ya que permite aplicar un método a cada elemento sin necesidad de crear una lambda para cada elemento. El compilador infiere qué tipo de referencia a método se necesita basándose en el contexto y la interfaz funcional a la que se asigna.

```java
import java.util.function.*;
import java.util.Arrays;
import java.util.List;

public class Calculadora {
    public static int sumarDos(int num) {
        return num + 2;
    }
    
    public String obtenerDescripcion() {
        return "Descripción de la instancia";
    }
}

public class Main {
    public static void main(String[] args) {
        // 1. Referencia a método estático
        Function<Integer, Integer> suma = Calculadora::sumarDos;
        System.out.println(suma.apply(5));  // 7
        
        // 2. Referencia a constructor
        Supplier<Calculadora> creador = Calculadora::new;
        Calculadora calc = creador.get();
        
        // 3. Referencia a método de instancia concreta
        Supplier<String> obtenerDesc = calc::obtenerDescripcion;
        System.out.println(obtenerDesc.get());  // Descripción de la instancia
        
        // 4. Referencia a método de instancia sobre cualquier instancia
        List<String> palabras = Arrays.asList("hola", "mundo", "java");
        palabras.forEach(System.out::println);  // Imprime cada palabra
    }
}
```


## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta

Este ejemplo demuestra cómo usar lambdas con `Collections.sort`, que acepta un `Comparator<T>`, una interfaz funcional que define cómo comparar dos objetos. El método `sort` es un excelente caso de uso para lambdas porque la lógica de comparación es frecuentemente única para cada situación de ordenamiento, y escribirla en línea con una lambda es más legible que crear clases anónimas o métodos auxiliares. El ordenamiento primero por edad y luego por nombre es un patrón común que ilustra cómo manejar criterios de ordenamiento múltiples.

La interfaz `Comparator` proporciona métodos auxiliares como `thenComparing` que facilitan la composición de múltiples criterios de ordenamiento de forma elegante. La primera versión usa una lambda simple con la lógica completa de comparación, mientras que la segunda versión aprovecha los métodos de `Comparator` para crear un comparador más expresivo y reutilizable. Ambas produchucen el mismo resultado, pero la segunda es más idiomática en Java funcional.

```java
import java.util.*;

public class Persona {
    private String nombre;
    private int edad;
    
    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }
    
    public String getNombre() { return nombre; }
    public int getEdad() { return edad; }
    
    @Override
    public String toString() { return nombre + " (" + edad + ")"; }
}

public class Main {
    public static void main(String[] args) {
        List<Persona> personas = Arrays.asList(
            new Persona("Carlos", 25),
            new Persona("Ana", 30),
            new Persona("Bruno", 25),
            new Persona("Diana", 30)
        );
        
        // Versión 1: Lambda manual con lógica de comparación
        List<Persona> personas1 = new ArrayList<>(personas);
        Collections.sort(personas1, (p1, p2) -> {
            int comparaEdad = Integer.compare(p1.getEdad(), p2.getEdad());
            if (comparaEdad != 0) return comparaEdad;
            return p1.getNombre().compareTo(p2.getNombre());
        });
        
        System.out.println("Versión 1 (Lambda manual):");
        personas1.forEach(System.out::println);
        
        // Versión 2: Usando Comparator.comparing y thenComparing
        List<Persona> personas2 = new ArrayList<>(personas);
        Comparator<Persona> comparador = Comparator
            .comparingInt(Persona::getEdad)
            .thenComparing(Persona::getNombre);
        Collections.sort(personas2, comparador);
        
        System.out.println("\nVersión 2 (Comparator):");
        personas2.forEach(System.out::println);
    }
}
// Salida: Bruno (25), Carlos (25), Ana (30), Diana (30)
```
