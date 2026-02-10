
# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

Las cuatro características fundamentales de la programación orientada a objetos son: **abstracción**, **encapsulación**, **herencia** y **polimorfismo**.

**La abstracción** consiste

La **encapsulación** consiste en agrupar datos (atributos) y funciones (métodos) que operan sobre esos datos dentro de una misma unidad llamada clase, ocultando los detalles internos de implementación y exponiendo solo lo necesario mediante interfaces públicas. Esto permite proteger la integridad de los datos y facilita el mantenimiento del código.

La **herencia** permite crear nuevas clases basadas en clases existentes, heredando sus atributos y métodos. Esto promueve la reutilización de código y establece relaciones jerárquicas entre clases. El **polimorfismo** permite que objetos de diferentes clases respondan de manera distinta a la misma llamada de método, facilitando la escritura de código más genérico y flexible.

Finalmente, la **abstracción** consiste en modelar entidades del mundo real mediante clases, identificando las características y comportamientos esenciales mientras se omiten los detalles irrelevantes. Esto simplifica la complejidad del sistema al trabajar con conceptos de alto nivel.

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Dinámicos
- **JavaScript** (alto nivel, sin clases)
- **Python**: Lenguaje multiparadigma que soporta POO de forma natural y flexible.

### Compilados (Programas Grandes)
**Recolectores de Basura** (GC)

- **Java**: Lenguaje puramente orientado a objetos (casi todo es un objeto), multiplataforma gracias a la JVM.
- **C#**: Lenguaje orientado a objetos desarrollado por Microsoft para la plataforma .NET, similar a Java en muchos aspectos.

**Sin Recolectores de Basura** (No GC)

- **C++**: Extensión de C que añade soporte para POO, permitiendo combinar programación estructurada y orientada a objetos.
- **Rust**


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

Anterior a estes tipos se programaba en **Ensamblador**, el cual permitía la *Secuencia* (seguir un orden) y el salto arbitrario *Goto* (JMP, salto sin resultado = 0).

La **programación estructurada** es un paradigma que surgió en los años 60-70 para mejorar la claridad y calidad del código. Se basa en el uso de estructuras de control modernas (secuencia, selección con `if/switch`, e iteración con bucles `for/while`) y evita el uso de instrucciones `goto`. Este enfoque facilita la lectura, comprensión y mantenimiento del código al seguir un flujo de ejecución más predecible. Lenguajes como C implementan este paradigma de forma efectiva.

La **programación modular** es una evolución que propone dividir el programa en módulos o unidades independientes, cada uno con una funcionalidad específica y bien definida. Cada módulo encapsula datos y funciones relacionadas, exponiendo solo lo necesario a través de interfaces públicas. Esto permite desarrollar, probar y mantener cada módulo de forma independiente, facilitando el trabajo en equipo y la reutilización de código. Agrupar código para facilitar su uso por otros programas en otros módulos.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

Un objeto en programación orientada a objetos se define por tres elementos fundamentales: **identidad**, **estado** y **comportamiento**.

La **identidad (dirección de memoria)** es lo que distingue a un objeto de todos los demás, incluso si tienen el mismo estado (nombre). Es similar a la dirección de memoria de una variable en C, algo único que identifica al objeto durante su existencia. 

El **estado (nombre)** está determinado por los valores de sus atributos (variables miembro), que pueden cambiar a lo largo del tiempo. Por ejemplo, un objeto de tipo coche podría tener como estado: color rojo, velocidad 60 km/h, motor encendido. Las variables de una *struct* se definen como **campos**.

El **comportamiento** se define mediante los métodos (funciones) que todos los objetos de esa clase pueden hacer. Estos métodos pueden consultar o modificar el estado del objeto, o interactuar con otros objetos. Siguiendo el ejemplo del coche, sus comportamientos podrían incluir: acelerar, frenar, girar, etc. Estos tres elementos trabajan juntos para representar entidades del mundo real en el software.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase? 

Una **clase** es una plantilla o molde que define la estructura y comportamiento que tendrán los objetos creados a partir de ella. Define qué atributos tendrá el objeto (sus datos) y qué métodos podrá ejecutar (sus funciones). Es similar a cómo en C se define un `struct` que especifica los campos que tendrán las variables de ese tipo, pero además incluye las funciones que operan sobre esos datos.

Una clase **no es lo mismo** que un objeto. La clase es la definición abstracta, mientras que el objeto es una entidad concreta (ciertos atributos) creada en memoria a partir de esa clase (variables). Una **instancia** es precisamente eso: un objeto concreto creado a partir de una clase. Por ejemplo, se puede tener la clase `Coche` (la plantilla) y crear varias instancias como `miCoche` y `tuCoche` (objetos concretos en memoria).

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**?? 

En la mayoría de lenguajes orientados a objetos, los objetos se almacenan en una región de memoria dinámica llamada **heap** (montículo), similar a cuando se usa `malloc()` en C. Al crear un objeto con `new`, se reserva memoria en el heap y se devuelve una referencia (similar a un puntero) a esa ubicación. Esta memoria persiste hasta que el objeto es explícitamente liberado o es eliminado por el recolector de basura.

No es igual en todos los lenguajes. En C++, el programador tiene control total: puede crear objetos en la pila (stack) como variables locales, o en el heap usando `new`, siendo responsable de liberarlos con `delete`. En Java, todos los objetos se crean obligatoriamente en el heap mediante `new`, y las variables locales solo almacenan referencias a esos objetos. En C#, existen los tipos valor (`struct`) que se almacenan en la pila, y los tipos referencia (`class`) que van al heap.

La **recolección de basura** (garbage collection) es un mecanismo automático que gestiona la memoria liberando objetos que ya no son accesibles desde el programa. A diferencia de C donde hay que usar `free()` manualmente, lenguajes como Java, Python o C# incorporan un recolector que periódicamente identifica objetos sin referencias y los elimina, evitando pérdidas de memoria (memory leaks) y liberando al programador de esta responsabilidad. Esto simplifica el desarrollo pero puede introducir pausas impredecibles en la ejecución.

**Ventajas de Usar Heap**
- Reserva dinámica, el tamaño se decide en ejecución.
- Lo que está en el Heap, vive más allá que el método o función donde se ha creado.

**Desventajas de Usar Heap**
- Hay que liberarla cuando ya no se necesita -> Manual (difícil de hacer)
                                             -> Automático (Recolector de Basura) 

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

Un **método** es una función que pertenece a una clase y define el comportamiento de los objetos de esa clase. A diferencia de las funciones tradicionales de C que existen independientemente, los métodos están asociados a una clase específica y operan sobre los datos (atributos) de los objetos. Cuando se invoca un método sobre un objeto, este tiene acceso implícito a los atributos de ese objeto concreto.

La **sobrecarga de métodos** (method overloading) es la capacidad de definir múltiples métodos con el mismo nombre dentro de una clase, siempre que tengan diferentes firmas (distinto número o tipo de parámetros). El compilador determina qué versión del método invocar según los argumentos proporcionados. Por ejemplo, una clase `Calculadora` podría tener varios métodos `sumar(int a, int b)`, `sumar(double a, double b)` y `sumar(int a, int b, int c)`, y el compilador elegiría el apropiado según los parámetros usados en cada llamada.

Esta característica mejora la legibilidad del código al permitir usar nombres descriptivos consistentes para operaciones similares, en lugar de crear nombres artificialmente diferentes como `sumarEnteros()`, `sumarReales()`, etc. Java soporta sobrecarga de métodos, mientras que C no permite tener múltiples funciones con el mismo nombre.

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

```java
class Punto {
    double x; // Estad (atributo)
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

La palabra clave `static` indica que un método o atributo pertenece a la clase en sí, no a las instancias individuales (concretas). Esto significa que se puede invocar sin necesidad de crear un objeto. Por ejemplo, `Math.sqrt()` es un método estático que se llama directamente desde la clase `Math` sin crear una instancia. El método `main` debe ser `static` porque la JVM lo ejecuta antes de crear cualquier objeto. No solo se usa para `main`: se pueden crear otros métodos y atributos estáticos cuando se necesita funcionalidad o datos compartidos por todas las instancias de una clase.

Cuando se combina `static` con `final`, se crean **constantes de clase**. `final` significa que el valor no puede cambiar después de su inicialización. Por ejemplo, `public static final double PI = 3.14159;` crea una constante que pertenece a la clase (no a instancias individuales) y cuyo valor es inmutable. Esto es útil para definir valores que son iguales para todos los objetos y que no deben modificarse, similar a `#define` o `const` en C.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

Para compilar un programa Java desde la línea de comandos se usa `javac Archivo.java`, que genera los `.class`. Para ejecutar se usa `java Clase` (sin extensión). Java es compilado a **bytecode**, y la **JVM** es la máquina virtual que ejecuta ese bytecode; por eso los `.class` son portables entre sistemas.

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

`this` (no está disponible en métodos static) es una referencia implícita al objeto actual sobre el cual se está ejecutando un método. Es similar a un puntero al objeto mismo, permitiendo acceder a sus atributos y métodos. Se usa principalmente cuando hay ambigüedad entre nombres de parámetros y atributos, o cuando se necesita pasar el objeto actual como argumento a otro método.

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

En Java, todos los parámetros se pasan **por valor**. En objetos se copia la referencia, por eso modificar atributos sí afecta al objeto original, pero reasignar la **referencia no (solo cambia la copia local)**. En tipos primitivos se copia el valor, así que cambios dentro del método no afectan a la variable original. En otras palabras: se puede cambiar el estado del objeto recibido, pero no cambiar a que referencia apunta la variable externa.

## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

`toString()` devuelve una representación textual del objeto y se puede sobrescribir para mostrar datos utiles al imprimir o registrar. Si no se sobrescribe, hereda la version de `Object`, que normalmente muestra el nombre de la clase y un hash. Hay equivalentes en otros lenguajes (por ejemplo, Python `__str__()`, C# `ToString()`). En `Punto`, podria devolver algo como `Punto(3.0, 4.0)`.

Ejemplo detallado:

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

public class PruebaPunto {
    public static void main(String[] args) {
        Punto p = new Punto(3.0, 4.0);
        System.out.println(p);            // Llama a toString()
        System.out.println(p.toString()); // Misma salida
    }
}
```

## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?

Un `struct` agrupa datos, pero no integra metodos ni encapsulacion. Le faltan control de acceso, constructores/destructores, herencia y polimorfismo; por eso no llega a ser una clase completa. En C++, `struct` y `class` son casi iguales, pero en C puro no existe esa integracion de comportamiento.

## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

Se emula con un `struct` para los datos y funciones que reciben un puntero al `struct`. El `this` pasa a ser un parametro explicito (por ejemplo, `Punto* this`) y al llamar se pasa `&p`. Asi se logra la misma idea de "metodo" pero de forma manual.

Ejemplo detallado:

```c
#include <stdio.h>
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

double calculaDistanciaAOrigen(Punto* this) {
    return sqrt(this->x * this->x + this->y * this->y);
}

int main(void) {
    Punto p;
    p.x = 3.0;
    p.y = 4.0;

    double d = calculaDistanciaAOrigen(&p); // &p es el "this"
    printf("Distancia: %.1f\n", d);        // Imprime 5.0
    return 0;
}
```