
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

La encapsulación busca agrupar datos y operaciones dentro de una clase, y la ocultación limita el acceso directo al estado interno. Se pretende que el uso del objeto se haga mediante una interfaz pública, sin depender de cómo se guarda o calcula internamente la información.

Entre las **ventajas de la ocultación se incluyen:** menor acoplamiento, mantenimiento más sencillo y posibilidad de cambiar la implementación sin romper el código que usa la clase. También se reduce la probabilidad de errores por manipulación indebida del estado y se facilita el cumplimiento de invariantes.


## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

La **interfaz pública** es el conjunto de miembros accesibles desde fuera de la clase, principalmente métodos y, en algunos casos, constantes. Es la "cara" que el objeto ofrece para ser utilizado por otros componentes.

Se relaciona con la ocultación de información porque todo lo que no forma parte de esa interfaz queda protegido. Así, el acceso al estado interno se controla y se evita depender de detalles de implementación.


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

La **interfaz pública** define el contrato de uso de la clase y condiciona al resto del programa. Un diseño cuidadoso evita exponer detalles innecesarios y facilita el mantenimiento a largo plazo.

No suele ser fácil cambiarla, ya que muchos módulos pueden depender de ella. Modificarla implica revisar y adaptar el código cliente, por lo que conviene considerarla estable.


## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

Las **invariantes de clase** son condiciones que deben cumplirse siempre sobre el estado interno de un objeto. Por ejemplo, que un contador no sea negativo o que una coordenada esté en un rango válido.

La ocultación de información ayuda porque obliga a modificar el estado a través de métodos que pueden validar esas condiciones. Se evita que código externo viole la consistencia interna.


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

## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

Además de pública y privada, suelen existir visibilidades intermedias que restringen el acceso a un paquete, módulo o jerarquía de herencia. Estas opciones permiten un control más fino sin exponer completamente los miembros.

En Java existen `public`, `protected`, `private` y el acceso por defecto (package-private). En otros lenguajes, como C++ o C#, también hay visibilidades equivalentes, aunque con diferencias en paquetes, módulos o ensamblados.


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


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

Un **getter** es un método que devuelve el valor de un atributo, y un setter es un método que lo modifica. Se emplean para controlar el acceso al estado interno sin exponer directamente los atributos.

Con ellos se puede validar, transformar o restringir el acceso, manteniendo las invariantes de clase. También permiten cambiar la implementación interna sin afectar al código cliente.


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

No se refiere a seguridad frente a ataques externos, sino a seguridad del diseño y del uso correcto de los objetos. Se trata de evitar que el estado interno se modifique de forma incorrecta desde fuera.

La ocultación de información reduce errores y facilita mantener invariantes, lo que incrementa la robustez del programa. Es una seguridad lógica, no una protección criptográfica o de red.


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

Un **miembro de instancia** pertenece a cada objeto y puede tener valores distintos en cada uno. Un **miembro de clase** (estático) pertenece a la clase en sí y se comparte entre todas las instancias.

Los **miembros de clase** también pueden ocultarse usando `private` o `protected`. La visibilidad no depende de si el miembro es de instancia o de clase.


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

Sí, tiene sentido cuando se quiere controlar la creación de instancias. Por ejemplo, en patrones como Singleton, o cuando se usan métodos factoría.

Un constructor privado impide instanciar desde fuera de la clase. Se obliga a usar métodos públicos controlados para crear objetos.


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

### Respuesta


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado.

### Respuesta


## 24. Añade a la clase `Mes` del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta
