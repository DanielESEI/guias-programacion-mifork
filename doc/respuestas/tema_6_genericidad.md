
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

Se puede crear una estructura de datos basada en un array de `Object`, aprovechando que en Java todas las clases heredan de `Object`. Esta es la única forma de alojar elementos heterogéneos en un único array sin usar generics.

```java
public class ContenedorGenerico {
    private Object[] elementos;
    private int tamanio;
    
    public ContenedorGenerico(int capacidad) {
        elementos = new Object[capacidad];
        tamanio = 0;
    }
    
    public void añadir(Object elemento) {
        if (tamanio < elementos.length) {
            elementos[tamanio++] = elemento;
        }
    }
    
    public Object obtener(int indice) {
        return elementos[indice];
    }
}
```

Para usar esta estructura, se extraen los elementos y se realiza un casting manual al tipo específico:

```java
ContenedorGenerico contenedor = new ContenedorGenerico(3);
contenedor.añadir("Texto");
contenedor.añadir(42);
contenedor.añadir(3.14);

String texto = (String) contenedor.obtener(0);
Integer numero = (Integer) contenedor.obtener(1);
Double decimal = (Double) contenedor.obtener(2);
```

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

La programación genérica es un mecanismo que permite escribir código que funcione con múltiples tipos de datos, sin perder la seguridad de tipos en tiempo de compilación. En lugar de trabajar con un tipo base común (como `Object`), se parametrizan los tipos para que el compilador verifique que se usan consistentemente.

El ejemplo anterior no es un verdadero ejemplo de programación genérica, sino un workaround anterior a que existiesen generics. Utiliza `Object` como tipo común y requiere casting manual en tiempo de ejecución, lo cual es propenso a errores. La programación genérica verdadera utiliza parámetros de tipo que se conocen en tiempo de compilación, permitiendo que el compilador realice verificaciones exhaustivas sin necesidad de casting.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 


El mayor problema es que el compilador no puede verificar tipos en tiempo de compilación. Si se intenta recuperar un elemento y se realiza un casting incorrecto, el programa compilará sin problemas pero fallaré en tiempo de ejecución con una excepción `ClassCastException`. Esto hace que los errores de tipo sean difíciles de detectar durante el desarrollo.

Además, el código que utiliza estas estructuras requiere casting explícito y repetitivo, lo que reduce la legibilidad y aumenta la probabilidad de errores. El compilador tampoco puede verificar si los castings son compatibles, ni fuerza que los elementos almacenados sean del tipo esperado, permitiendo mezclar tipos diferentes sin restricción alguna en la misma colección.


## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 


Los parámetros de tipo son variables que representan tipos desconocidos en tiempo de escritura del código, permitiendo que una clase o método funcione con cualquier tipo que se especifique posteriormente. Se declaran entre corchetes angulares `<T>`, donde `T` es una convención para representar un "Type" genérico. De esta forma, una única clase puede adaptarse a trabajar con múltiples tipos específicos sin duplicar código.

Cuando se instancia o utiliza una clase genérica, se especifica el tipo concreto reemplazando el parámetro de tipo. Por ejemplo, `List<String>` utiliza `String` como tipo concreto para el parámetro `T`. El compilador entonces reemplaza todas las referencias a `T` por `String` en ese caso específico, permitiendo que realice verificación de tipos exhaustiva sin necesidad de castings manuales.


## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

**En Java:**

```java
import java.util.ArrayList;

List<String> nombres = new ArrayList<String>();
nombres.add("Alice");
nombres.add("Bob");
nombres.add("Carlos");

for (String nombre : nombres) {
    System.out.println(nombre.toUpperCase()); // Sin casting, tipo conocido
}
```

**En C++:**

```cpp
#include <vector>
#include <iostream>

std::vector<std::string> nombres;
nombres.push_back("Alice");
nombres.push_back("Bob");
nombres.push_back("Carlos");

for (const std::string& nombre : nombres) {
    std::cout << nombre << std::endl; // Sin casting, tipo conocido
}
```

En ambos casos, al especificar `<String>` o `<std::string>`, el compilador verifica que solo se añadan elementos de ese tipo. Cuando se recuperan elementos, se obtienen directamente como `String` o `std::string`, sin necesidad de casting manual. Si se intenta añadir un elemento de tipo diferente, el compilador rechazará el código inmediatamente, previniendo errores de tipo en tiempo de ejecución.


## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

Ambos lenguajes manejan los generics de formas fundamentalmente diferentes. **En C++**, el compilador instancia una nueva copia completa de la clase para cada tipo específico utilizado. Si se crea `vector<int>` y `vector<string>`, el compilador genera dos versiones completamente independientes del código, cada una optimizada para su tipo específico. Este proceso se llama "instanciación de plantillas" y resulta en un ejecutable más grande pero potencialmente más eficiente.

**En Java**, se emplea un mecanismo opuesto llamado "type erasure" (borrado de tipos). El compilador genera una única versión de la clase genérica, pero durante la compilación reemplaza todos los parámetros de tipo por su tipo base (normalmente `Object`), y añade castings automáticos donde es necesario. En tiempo de ejecución, `List<String>` y `List<Integer>` son exactamente la misma clase, pero el compilador ha verificado que la primera solo contiene `String` y la segunda solo `Integer`. Esta aproximación reduce el tamaño del bytecode pero pierde información de tipos en tiempo de ejecución.


## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 


```java
public class Par<T, U> {
    private final T primero;
    private final U segundo;
    
    public Par(T primero, U segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }
    
    public T getPrimero() {
        return primero;
    }
    
    public U getSegundo() {
        return segundo;
    }
}
```

La clase `Par` declara dos parámetros de tipo independientes, permitiendo almacenar dos valores de tipos diferentes. Un ejemplo de uso sería calcular la media y desviación típica de un array:

```java
public static Par<Double, Double> estadisticas(double[] datos) {
    double media = 0;
    for (double d : datos) media += d;
    media /= datos.length;
    
    double varianza = 0;
    for (double d : datos) varianza += Math.pow(d - media, 2);
    varianza /= datos.length;
    
    return new Par<>(media, Math.sqrt(varianza));
}

// Uso:
Par<Double, Double> resultado = estadisticas(new double[]{1, 2, 3, 4, 5});
System.out.println("Media: " + resultado.getPrimero());
System.out.println("Desv. típica: " + resultado.getSegundo());
```

El compilador verifica que `getPrimero()` devuelve `Double` y `getSegundo()` también, sin necesidad de casting manual. Si alguien intenta asignar el resultado a un tipo diferente, el compilador rechazará el código inmediatamente.


## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

**Versión sin generics (con `Object`):**

```java
public static Object seleccionaUnoSinGenerics(Object a, Object b) {
    return Math.random() < 0.5 ? a : b;
}

// Uso:
Object resultado = seleccionaUnoSinGenerics("texto", 42); // Compila sin problemas
String texto = (String) resultado; // Casting manual requerido
```

**Versión con generics:**

```java
public static <T> T seleccionaUno(T a, T b) {
    return Math.random() < 0.5 ? a : b;
}

// Uso:
String resultado = seleccionaUno("texto1", "texto2"); // Sin casting
Integer numero = seleccionaUno(10, 20); // Sin casting
```

La diferencia fundamental es que con generics, el compilador verifica en tiempo de compilación que ambos parámetros sean del mismo tipo. En la versión sin generics, se puede pasar `"texto"` y `42` sin problemas, y el método devolverá un `Object` del que no se conoce el tipo real. Con generics, el compilador infiere que si se pasa `String` como primer parámetro, el segundo debe ser también `String`, y el retorno ya se conoce como `String` sin necesidad de casting. Esto previene errores de tipo en tiempo de ejecución y elimina la necesidad de castings repetitivos.


## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

**Solución 1: Sin generics (usando `Number`):**

```java
public class PuntoNumber {
    private final Number x, y;
    
    public PuntoNumber(Number x, Number y) {
        this.x = x; this.y = y;
    }
    
    public Number getX() { return x; }
    public Number getY() { return y; }
    
    public double calcularDistanciaA(PuntoNumber otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx*dx + dy*dy);
    }
}
```

**Solución 2: Con generics (bounded type parameter):**

```java
public class Punto<T extends Number> {
    private final T x, y;
    
    public Punto(T x, T y) {
        this.x = x; this.y = y;
    }
    
    public T getX() { return x; }
    public T getY() { return y; }
    
    public double calcularDistanciaA(Punto<T> otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx*dx + dy*dy);
    }
}

// Uso:
Punto<Integer> p1 = new Punto<>(3, 4);
Punto<Double> p2 = new Punto<>(3.0, 4.0);
```

La declaración `<T extends Number>` es una restricción que obliga a que `T` sea `Number` o una subclase suya. Con esto, se pueden usar métodos de `Number` como `doubleValue()`. En la primera solución, los getters devuelven `Number`, perdiendo información específica del tipo. En la segunda, `getX()` devuelve exactamente el tipo concreto usado (`Integer`, `Double`, etc.). 

Respecto al "type erasure", tras la compilación ambas soluciones son equivalentes internamente: `T` se reemplaza por `Number`, y los getters de la clase genérica devuelven `Number` tras el borrado. Sin embargo, el compilador ha insertado castings automáticos en el código cliente, permitiendo que quien usa `Punto<Integer>` reciba directamente un `Integer` de `getX()` sin casting manual.


## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

La solución sin generics **sí permite** crear un punto con coordenadas de tipos diferentes: `new PuntoNumber(3, 4.5)` compila sin problemas porque ambos son aceptables como `Number`. La solución con generics **no lo permite**: al declarar `Punto<Integer> p = new Punto<>(3, 4.5)` el compilador rechaza `4.5` porque espera un `Integer`.

El método `getX()` en la solución sin generics siempre devuelve `Number`, independientemente de qué tipo específico se haya almacenado. En la solución con generics, `getX()` devuelve exactamente el tipo especificado: `Integer` si se instancia `Punto<Integer>`, o `Double` si se instancia `Punto<Double>`. Esta diferencia es crucial: la solución con generics refuerza la seguridad de tipos, asegurando que ambas coordenadas sean exactamente del mismo tipo, mientras que la solución sin generics sacrifica ese refuerzo a cambio de flexibilidad.


## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.

```java
public interface Punto<T extends Punto<T>> {
    public double distanciaA(T p);
}

public class Punto2D implements Punto<Punto2D> {
    private final double x, y;
    
    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double distanciaA(Punto2D p) {
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2));
    }
}

public class Punto3D implements Punto<Punto3D> {
    private final double x, y, z;
    
    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double distanciaA(Punto3D p) {
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2) + Math.pow(z - p.z, 2));
    }
}
```

Esta técnica se denomina "F-bounded polymorphism" (polimorfismo acotado-F). La interfaz `Punto<T extends Punto<T>>` establece que cada implementación debe especificar el tipo concreto para `T`. Al hacer esto, el compilador verifica que `distanciaA` recibe exactamente un `Punto2D` en la clase `Punto2D`, y un `Punto3D` en `Punto3D`, sin necesidad de castings o comprobaciones con `instanceof`. El código es type-safe desde el compilador, más legible, y evita errores en tiempo de ejecución.


## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

La respuesta es diferente en cada caso. `String[]` **sí es** (es tipo compatible) subtipo de `Object[]` debido a que los arrays son "covariantes": si se declara `Object[] array = new String[5]`, el código compila. Sin embargo, `List<String>` **no es** (compatible) subtipo de `List<Object>`: asignar `List<Object> lista = new ArrayList<String>()` causa error de compilación. Esto es porque las listas genéricas son "invariantes".

La diferencia se ilustra con un problema de tiempo de ejecución en arrays:
```java
Object[] array = new String[3];
array[0] = 123; // Compila, pero ¡error en ejecución!
```
El compilador no puede evitar este error porque confía en que `array` es `Object[]`. Con generics, esto no ocurre:
```java
List<Object> lista = new ArrayList<String>(); // No compila
```

**Covariancia** significa que si `T` es subtipo de `U`, entonces `Generico<T>` es subtipo de `Generico<U>`. **Contravariancia** es lo opuesto: `Generico<U>` es subtipo de `Generico<T>`. **Invariancia** significa que `Generico<T>` y `Generico<U>` no tienen relación de subtipo, incluso si `T` es subtipo de `U`. Java elige invariancia para listas genéricas porque permite la seguridad de tipos: no se puede añadir ni recuperar elementos de forma insegura. Los arrays, por compatibilidad con versiones antiguas de Java, son covariantes, lo que crea la brecha de seguridad demostrada.


## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

Un wildcard (`?`) es un tipo desconocido que permite especificar restricciones sobre qué tipos se aceptan. `List<? extends T>` acepta listas de `T` o cualquier subtipo de `T` (covariancia controlada), permitiendo **leer** elementos con seguridad, pero impidiendo **escribir** (excepto `null`). `List<? super T>` acepta listas de `T` o cualquier supertipo de `T` (contravariancia controlada), permitiendo **escribir** elementos de tipo `T`, pero impidiendo **leer** con seguridad (todo es `Object`).

**Ejemplo 1: Suma de números con `? extends Number`**
```java
public static double sumarNumeros(List<? extends Number> numeros) {
    double suma = 0;
    for (Number n : numeros) {
        suma += n.doubleValue();
    }
    return suma;
}

// Uso:
List<Integer> enteros = Arrays.asList(1, 2, 3);
List<Double> decimales = Arrays.asList(1.5, 2.5);
System.out.println(sumarNumeros(enteros));     // 6.0
System.out.println(sumarNumeros(decimales));   // 4.0
```

**Ejemplo 2: Añadir enteros con `? super Integer`**
```java
public static void añadirEnteros(List<? super Integer> lista, int... valores) {
    for (int valor : valores) {
        lista.add(valor);
    }
}

// Uso:
List<Number> numeros = new ArrayList<>();
List<Integer> enteros = new ArrayList<>();
añadirEnteros(numeros, 1, 2, 3);
añadirEnteros(enteros, 4, 5, 6);
```

En la primera solución se lee de la lista, así que se usa covariancia: acepta listas de cualquier tipo de número. En la segunda se escribe, así que se usa contravariancia: acepta listas de `Integer` o sus supertipo, garantizando que aceptarán `Integer`. Los wildcards permiten escribir métodos más flexibles manteniendo la seguridad de tipos en tiempo de compilación.
