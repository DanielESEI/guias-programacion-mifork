
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

### Respuesta

El polimorfismo es la capacidad de tratar objetos de distintas clases derivadas a través de una referencia del tipo base, manteniendo un mismo "mensaje" (la misma llamada a método) pero obteniendo comportamientos distintos según el objeto real. En términos prácticos, permite escribir código más general y reutilizable: en vez de preguntar constantemente "de qué tipo exacto es", se llama al método común y cada clase responde como corresponda.

La sobreescritura (overriding) es el mecanismo por el que una subclase redefine un método heredado de su clase base con la misma firma. Gracias a eso, cuando se invoca ese método mediante una referencia del tipo padre, se ejecuta la versión de la subclase si el objeto real pertenece a ella. Esa combinación entre herencia + sobreescritura + llamada por referencia base es la base del polimorfismo en Java.


## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta

La ligadura dinámica (enlace tardío) significa que la selección del método a ejecutar no se decide completamente al compilar, sino en tiempo de ejecución, según el tipo real del objeto. Es lo que hace posible que una referencia de tipo base invoque implementaciones diferentes en subclases. Por eso está directamente ligada al polimorfismo: sin ligadura dinámica, las llamadas no cambiarían de comportamiento según el objeto concreto.

En C++, este comportamiento debe declararse explícitamente con métodos `virtual` (y suele usarse `override` en subclases para verificar). Si un método no es virtual, la resolución suele ser estática y se llama a la versión asociada al tipo de la referencia/puntero. En Java, los métodos de instancia son virtuales por defecto (salvo casos como `final`, `private` o `static`), por lo que el enlace dinámico es el comportamiento normal y no hace falta marcar `virtual`.

En Python también existe despacho dinámico de métodos de forma natural, porque el lenguaje es dinámico y no obliga a declarar tipos estáticos de referencia como Java o C++. En la práctica, al llamar a un método se busca la implementación en el objeto real y su jerarquía de clases en tiempo de ejecución. Es decir, el efecto polimórfico aparece "por defecto", aunque con reglas de resolución propias de Python.


## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta

En este ejemplo se define una clase base `Soldado` con el método `saludar`, y dos subclases (`Zapador` y `Artillero`) que pueden redefinirlo. Se hace que `Zapador` sustituya completamente el comportamiento para que se observe claramente la sobreescritura. Después se guarda una mezcla de objetos en un array de tipo `Soldado[]`.

Al recorrer ese array y ejecutar `saludar()`, no se usa siempre la versión del padre: se ejecuta la implementación correspondiente al tipo real de cada objeto. Ese es exactamente el efecto del polimorfismo con enlace dinámico.

```java
class Soldado {
	public void saludar() {
		System.out.println("Soldado: saludo reglamentario.");
	}
}

class Zapador extends Soldado {
	@Override
	public void saludar() {
		System.out.println("Zapador: preparado para abrir brecha.");
	}
}

class Artillero extends Soldado {
	@Override
	public void saludar() {
		System.out.println("Artillero: pieza lista para fuego.");
	}
}

public class DemoPolimorfismo {
	public static void main(String[] args) {
		Soldado[] peloton = {
			new Soldado(),
			new Zapador(),
			new Artillero(),
			new Zapador()
		};

		for (Soldado s : peloton) {
			s.saludar();
		}
	}
}
```


## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta

Sí, al sobreescribir un método en una subclase se puede reutilizar primero la implementación de la clase base y luego ampliar o modificar su efecto. En Java, eso se hace con la palabra clave `super`, que permite acceder al método heredado inmediato.

En el caso de `Zapador`, se puede llamar a `super.saludar()` para mantener el saludo común del `Soldado`, y a continuación añadir el mensaje específico del zapador. De esa forma no se sustituye por completo el método, sino que se extiende su comportamiento.

```java
class Soldado {
	public void saludar() {
		System.out.println("Soldado: saludo reglamentario.");
	}
}

class Zapador extends Soldado {
	@Override
	public void saludar() {
		super.saludar();
		System.out.println("ZAPADOR A SUS ORDENES");
	}
}
```

La palabra clave usada para invocar al método de la clase base es `super`.


## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta

En una sobreescritura real en Java, la firma del método debe mantenerse: mismo nombre y mismos tipos de parámetros (incluido orden). El tipo de retorno debe ser el mismo o uno covariante (un subtipo del original). Además, no se puede reducir visibilidad (por ejemplo, pasar de `public` a `protected`) y no se pueden declarar excepciones comprobadas más amplias que en el método heredado.

La diferencia principal es que `overriding` actúa entre padre e hijo con la misma firma, mientras que `overloading` ocurre en la misma clase (o jerarquía) con el mismo nombre pero lista de parámetros distinta. En sobrecarga, la elección de qué método usar se decide en compilación según argumentos; en sobreescritura, la implementación final depende del objeto real en ejecución.

La anotación `@Override` indica al compilador que se pretende sobreescribir un método heredado. Es recomendable usarla siempre porque evita errores silenciosos: si se comete una errata en el nombre o firma, el compilador avisará inmediatamente en lugar de crear accidentalmente otro método diferente.


## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta

Sí, en Java se empieza a usar polimorfismo muy pronto, aunque al principio no siempre se nombre explícitamente. Cada vez que se hereda de `Object` y se redefine un método como `toString` o `equals`, se está participando en comportamiento polimórfico, porque esos métodos pueden invocarse sobre referencias generales y resolver en tiempo de ejecución la versión concreta de cada clase.

Por ejemplo, cuando se imprime un objeto con `System.out.println(obj)`, internamente se llama a `toString()`. Si una clase lo ha sobreescrito, se usa su versión específica. Lo mismo ocurre con `equals`: dos referencias de tipo general pueden comparar objetos de clases concretas distintas y ejecutar la implementación redefinida. Por tanto, sí: sobreescribir esos métodos ya es usar polimorfismo.


## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta

Una clase abstracta es una clase pensada como base de herencia, que puede contener métodos implementados y métodos sin implementación. Un método abstracto declara únicamente su firma y obliga a que las subclases concretas lo implementen. Por definición, no se pueden crear instancias directas de una clase abstracta.

Para el caso propuesto, `abstract` debe colocarse en la declaración de la clase `Soldado` y también en la declaración del método `atacar`. Las subclases concretas (`Zapador`, `Artillero`, etc.) implementan `atacar` con su comportamiento específico, mientras comparten lo que sí esté implementado en la clase base.

```java
abstract class Soldado {
	public void saludar() {
		System.out.println("Soldado: saludo reglamentario.");
	}

	public abstract void atacar();
}

class Zapador extends Soldado {
	@Override
	public void atacar() {
		System.out.println("Zapador: coloca carga de demolición.");
	}
}

class Artillero extends Soldado {
	@Override
	public void atacar() {
		System.out.println("Artillero: dispara la pieza.");
	}
}
```


## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta

En Java, `final` aplicado a un método impide que dicho método sea sobreescrito en subclases. Aplicado a una clase, impide que esa clase sea heredada. Es una forma de "cerrar" puntos de extensión cuando se quiere preservar comportamiento, seguridad o invariantes de diseño.

Su relación con el polimorfismo es directa: al impedir herencia o sobreescritura, se limita la variación de comportamiento polimórfico en esos elementos. El resto de métodos no `final` de una jerarquía siguen pudiendo participar en despacho dinámico. Un ejemplo muy conocido en la API estándar es `String`, que es una clase `final`.


## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta

Una interfaz en Java define un contrato: qué operaciones debe ofrecer una clase, sin obligar a una implementación concreta (salvo métodos `default` o `static` en versiones modernas). Sirve para modelar capacidades comunes entre clases que pueden no compartir la misma clase base.

Se parecen a las clases abstractas en que ambas pueden expresar abstracción, pero no son equivalentes. Una clase abstracta permite mantener estado de instancia y lógica compartida más rica; una interfaz prioriza el contrato común desacoplado. En Java, una clase solo puede heredar de una clase, pero sí puede implementar múltiples interfaces, lo que aporta una forma de herencia múltiple de comportamiento contractual.


## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta

Se puede modelar `Punto` como clase abstracta con el método `calcularDistanciaA(Punto otro)`. Cada subtipo (`Punto2D`, `Punto3D`) implementa el cálculo correcto de distancia euclídea según su dimensión. Para cumplir la condición pedida, se usa `instanceof` para validar compatibilidad y luego *downcasting* para acceder a las coordenadas del subtipo concreto.

La clase `Linea` no necesita conocer si trabaja con 2D o 3D: recibe dos referencias `Punto` y delega el cálculo en polimorfismo con `inicio.calcularDistanciaA(fin)`. Así, el mismo código de `Linea` funciona para distintas dimensiones, siempre que ambos extremos sean del mismo subtipo.

```java
abstract class Punto {
	public abstract double calcularDistanciaA(Punto otro);
}

class Punto2D extends Punto {
	private final double x;
	private final double y;

	public Punto2D(double x, double y) {
		this.x = x;
		this.y = y;
	}

	@Override
	public double calcularDistanciaA(Punto otro) {
		if (!(otro instanceof Punto2D)) {
			throw new IllegalArgumentException("Se esperaba un Punto2D");
		}
		Punto2D p = (Punto2D) otro;
		double dx = this.x - p.x;
		double dy = this.y - p.y;
		return Math.sqrt(dx * dx + dy * dy);
	}
}

class Punto3D extends Punto {
	private final double x;
	private final double y;
	private final double z;

	public Punto3D(double x, double y, double z) {
		this.x = x;
		this.y = y;
		this.z = z;
	}

	@Override
	public double calcularDistanciaA(Punto otro) {
		if (!(otro instanceof Punto3D)) {
			throw new IllegalArgumentException("Se esperaba un Punto3D");
		}
		Punto3D p = (Punto3D) otro;
		double dx = this.x - p.x;
		double dy = this.y - p.y;
		double dz = this.z - p.z;
		return Math.sqrt(dx * dx + dy * dy + dz * dz);
	}
}

class Linea {
	private final Punto inicio;
	private final Punto fin;

	public Linea(Punto inicio, Punto fin) {
		this.inicio = inicio;
		this.fin = fin;
	}

	public double longitud() {
		return inicio.calcularDistanciaA(fin);
	}
}
```


## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta

La herencia de interfaces en Java consiste en que una interfaz puede extender otra para ampliar su contrato. De esta manera, una interfaz hija incluye automáticamente todos los métodos de la interfaz padre y añade nuevos métodos. Sí existe herencia múltiple de interfaces: una interfaz puede extender varias interfaces, y una clase puede implementar varias interfaces a la vez.

En el ejemplo, `Fichero` define la operación básica de lectura. Luego `FicheroEscribible` extiende `Fichero` y añade operaciones de escritura y borrado. Cualquier clase que implemente `FicheroEscribible` queda obligada a implementar los tres métodos.

```java
interface Fichero {
	String leerContenido();
}

interface FicheroEscribible extends Fichero {
	void escribirContenido(String contenido);
	void eliminar();
}

class FicheroMemoria implements FicheroEscribible {
	private String contenido = "";
	private boolean eliminado = false;

	@Override
	public String leerContenido() {
		if (eliminado) {
			throw new IllegalStateException("El fichero está eliminado");
		}
		return contenido;
	}

	@Override
	public void escribirContenido(String contenido) {
		if (eliminado) {
			throw new IllegalStateException("No se puede escribir: fichero eliminado");
		}
		this.contenido = contenido;
	}

	@Override
	public void eliminar() {
		eliminado = true;
		contenido = "";
	}
}
```
