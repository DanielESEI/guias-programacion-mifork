
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

La encapsulación **(protección)** busca agrupar datos y operaciones dentro de una clase, y la **ocultación** limita el acceso directo al estado interno. Se pretende que el uso del objeto se haga mediante una interfaz pública, sin depender de cómo se guarda o calcula internamente la información.

Entre las **ventajas de la ocultación se incluyen:** menor acoplamiento, mantenimiento más sencillo y posibilidad de cambiar la implementación sin romper el código que usa la clase. También se reduce la probabilidad de errores por manipulación indebida del estado y se facilita el cumplimiento de invariantes.
  
        - Evita estados no válidos de mis objetos.
        - Evita dependencias desde fuera que no quiero.


## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

La **interfaz pública** es el conjunto de **miembros** accesibles desde fuera de la clase, principalmente métodos y, en algunos casos, constantes. Es la "cara" que el objeto ofrece para ser utilizado por otros componentes. *(lo q no está oculto)*

Se relaciona con la ocultación de información porque todo lo que no forma parte de esa interfaz queda protegido. Así, el acceso al estado interno se controla y se evita depender de detalles de implementación.


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

La **interfaz pública** define el contrato de uso de la clase y condiciona al resto del programa. Un diseño cuidadoso evita exponer detalles innecesarios y facilita el mantenimiento a largo plazo.

No suele ser fácil cambiarla, ya que muchos módulos pueden depender de ella. Modificarla implica revisar y adaptar el código cliente, por lo que conviene considerarla estable. *Si se cambia tiene más consecuencias que cualquier cambio en la parte oculta.*


## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

Las **invariantes de clase** son condiciones que deben cumplirse siempre sobre el estado interno de un objeto. Por ejemplo, que un contador no sea negativo o que una coordenada esté en un rango válido.

La ocultación de información ayuda porque obliga a modificar el estado a través de métodos que pueden validar esas condiciones. Se evita que código externo viole la consistencia interna.

**Condiciones que los objetos de esa clase deben cumplir para ser válidos y durante toda la vida del objeto.**
        -> Cuenta Bancaria *debe tener siempre saldo positivo*
        -> Persona *debe tener edad >= 0*
        -> Rectángulo *debe tener ancho y alto > 0*

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

Se puede definir una clase `Punto` con atributos privados y métodos públicos. Así se protege el estado y se expone solo la funcionalidad necesaria mediante la interfaz pública.

La **interfaz pública** estaría formada por el constructor y los métodos públicos (por ejemplo, `calcularDistanciaOrigen` y los getters). `public` permite el acceso desde cualquier clase, mientras que `private` restringe el acceso al interior de la propia clase.

````java
public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    public double getX() { return x; }
    public double getY() { return y; }
}
````


## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

En Java, los modificadores  de acceso se pueden aplicar a clases, atributos, métodos y constructores. En el caso de clases de nivel superior, solo se permite `public` o el acceso por defecto (package-private).

`private` se utiliza en miembros internos (atributos, métodos y constructores) para limitar el acceso al interior de la clase. En clases internas (nested), también es posible aplicar `private`.

**Public (clases, atributos y métodos)**
**Private (clases internas, atributos y métodos)** 

## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

Además de pública y privada, suelen existir visibilidades intermedias que restringen el acceso a un paquete, módulo o jerarquía de herencia. Estas opciones permiten un control más fino sin exponer completamente los miembros.

En Java existen `public`, `protected`, `private` y el acceso por defecto (package-private). En otros lenguajes, como C++ o C#, también hay visibilidades equivalentes, aunque con diferencias en paquetes, módulos o ensamblados.

**protected**, sólo se ve desde "subclases" *(Tema de Herecia)*
**package-private** o sin modificar, solo se ve desde el paqueta


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

Los **miembros privados** están ocultos para otras clases, pero sí son accesibles desde métodos de la misma clase, incluso al referirse a otra instancia. Esto permite que un objeto de la misma clase pueda acceder a los campos privados de otro objeto de esa clase.

En el siguiente método, se accede a `otro.x` y `otro.y` aunque sean privados, porque el acceso ocurre dentro de la propia clase `Punto`.

````java
public class Punto {
    private double x;
    private double y;

    // ...existing code...

    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
````
**a), está oculto para código de otras clases**


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

**"Getter" y "Setter"** permiten dar acceso a atributos privados para obtener su valor o cambiarlo.


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

No, esto no es ciberseguridad. Es facilitar una promoción con menos bugs.

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

Un **miembro de instancia** pertenece a cada objeto y puede tener valores distintos en cada uno. Un **miembro de clase** (estático) pertenece a la clase en sí y se comparte entre todas las instancias.

Los **miembros de clase** también pueden ocultarse usando `private` o `protected`. La visibilidad no depende de si el miembro es de instancia o de clase. *(está asociado a cada instancia; no son compartidos)*


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

A veces: 
    - Un constructor auxiliar oculto, llamado desde otros conductores públicos
    - Cuando quiero controlar el nº de instancias


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

Los miembros de clase se indican con la palabra clave `static`. Se accede a ellos con el nombre de la clase, y se comparten entre todas las instancias.

En el ejemplo, se actualizan los máximos en el constructor. Se ofrecen getters estáticos para consultarlos.

````java
public class Punto {
    private double x;
    private double y;

    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }

    public static double getMaxX() { return maxX; }
    public static double getMaxY() { return maxY; }

    // ...existing code...
}
````


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

Un **método factoría** es un método estático que devuelve una instancia de la clase. En este caso, recibe dos coordenadas, las redondea y devuelve un nuevo `Punto`.

Sí, se debe usar `static` porque es un método de clase (factoría) que construye instancias sin necesidad de tener ya un objeto.

````java
public static Punto crearPuntoRedondeado(double x, double y) {
    return new Punto(Math.round(x), Math.round(y));
}
````


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

Gracias a la **ocultación de información**, podemos cambiar la implementación interna sin afectar el código que usa la clase. La interfaz pública se mantiene igual.

````java
public class Punto {
    private double[] coordenadas; // array interno de 2 posiciones

    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.coordenadas = new double[2];
        this.coordenadas[0] = x;
        this.coordenadas[1] = y;
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(coordenadas[0] * coordenadas[0] + 
                         coordenadas[1] * coordenadas[1]);
    }

    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.coordenadas[0] - otro.coordenadas[0];
        double dy = this.coordenadas[1] - otro.coordenadas[1];
        return Math.sqrt(dx * dx + dy * dy);
    }

    public double getX() { return coordenadas[0]; }
    public double getY() { return coordenadas[1]; }

    public static double getMaxX() { return maxX; }
    public static double getMaxY() { return maxY; }

    public static Punto crearPuntoRedondeado(double x, double y) {
        return new Punto(Math.round(x), Math.round(y));
    }
}
````

La **interfaz pública** no cambia: los métodos públicos siguen teniendo las mismas firmas y comportamiento. Solo cambia cómo se almacenan internamente las coordenadas.


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

Si los hago **públicos**: 

    - Si quiero garantizar la invariante de clase.
    - Conversión: 
      - Siempre privado.
      - Emplear métodos de validación
    - Para poder cambiar la interfaz.


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

Es inmutable si su instancia no gana: 
- Método Modificador -> cualquier método que cambia el estado interno (Ej: setter)

Las **clases inmutables tienen ventajas** -> no hacer clases mutables por defecto.


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

No, solo haría clases inmutables.


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

String es inmutable.


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

En POO se pueden comparar objetos por identidad (misma referencia) o por contenido (mismo estado). En Java, `equals` define la comparacion logica; por defecto, hereda el comportamiento de `Object` y compara por identidad. Para comparar contenido, se debe sobrescribir `equals` (y `hashCode`). Las cadenas se comparan con `equals`, no con `==`, porque `==` solo comprueba si es la misma referencia.

Ejemplo:

````java
String a = new String("hola");
String b = new String("hola");

boolean mismaReferencia = (a == b);     // false
boolean mismoContenido = a.equals(b);   // true
````


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

Las clases wrapper son clases que encapsulan tipos primitivos para tratarlos como objetos. En Java, ejemplos son `Integer`, `Double`, `Boolean`, y se usan para colecciones, metodos genericos o cuando se necesita `null`. La conversion puede ser automatica por autoboxing/unboxing, pero sigue existiendo una conversion implicita. No todos los lenguajes tienen tipos primitivos separados (por ejemplo, en Python todo es objeto), por lo que no siempre se necesitan wrappers.

Ejemplo:

````java
List<Integer> numeros = new ArrayList<>();
numeros.add(5);           // autoboxing int -> Integer
int x = numeros.get(0);   // unboxing Integer -> int
````


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

Un tipo enumerado representa un conjunto finito y cerrado de valores posibles. En Java, un `enum` es una clase especial con instancias predefinidas; puede tener atributos, metodos y constructor privado. En terminos de encapsulacion, permite agrupar datos y comportamiento relacionados y evita valores invalidos al restringir las instancias posibles a las del propio enumerado.


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado.

Un `enum` puede definir atributos privados y un constructor para inicializarlos. Cada constante del enum pasa sus datos al constructor. Los metodos publicos exponen los valores de forma controlada.

Ejemplo:

````java
public enum Mes {
    ENERO(1, 31),
    FEBRERO(2, 28),
    MARZO(3, 31),
    ABRIL(4, 30),
    MAYO(5, 31),
    JUNIO(6, 30),
    JULIO(7, 31),
    AGOSTO(8, 31),
    SEPTIEMBRE(9, 30),
    OCTUBRE(10, 31),
    NOVIEMBRE(11, 30),
    DICIEMBRE(12, 31);

    private final int ordinalEnAnio;
    private final int dias;

    Mes(int ordinalEnAnio, int dias) {
        this.ordinalEnAnio = ordinalEnAnio;
        this.dias = dias;
    }

    public int getOrdinalEnAnio() {
        return ordinalEnAnio;
    }

    public int getDias() {
        return dias;
    }
}
````


## 24. Añade a la clase `Mes` del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

Se puede definir cada estacion por conjuntos de meses. En el hemisferio norte se usan las estaciones habituales, y en el hemisferio sur se invierten (verano <-> invierno, primavera <-> otono). Los metodos devuelven `true` si el mes cae dentro del conjunto correspondiente.

Ejemplo:

````java
public enum Mes {
    // ...constantes y campos del ejercicio anterior...

    public boolean esDePrimavera(boolean enHemisferioNorte) {
        return enHemisferioNorte
            ? (this == MARZO || this == ABRIL || this == MAYO)
            : (this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE);
    }

    public boolean esDeVerano(boolean enHemisferioNorte) {
        return enHemisferioNorte
            ? (this == JUNIO || this == JULIO || this == AGOSTO)
            : (this == DICIEMBRE || this == ENERO || this == FEBRERO);
    }

    public boolean esDeOtono(boolean enHemisferioNorte) {
        return enHemisferioNorte
            ? (this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE)
            : (this == MARZO || this == ABRIL || this == MAYO);
    }

    public boolean esDeInvierno(boolean enHemisferioNorte) {
        return enHemisferioNorte
            ? (this == DICIEMBRE || this == ENERO || this == FEBRERO)
            : (this == JUNIO || this == JULIO || this == AGOSTO);
    }
}
````
