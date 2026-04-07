
## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuestag

En orientación a objetos, la herencia modela relaciones de especialización: si se afirma que A es-un B, entonces A puede tratarse como B. Dicho de otra forma, una subclase representa un caso particular de su superclase. Esta idea tiene dos efectos prácticos muy importantes. Primero, compatibilidad de tipos: una referencia del tipo general puede apuntar a objetos de tipos más concretos. Segundo, herencia de estado y comportamiento: la subclase reutiliza atributos y métodos de la superclase, y además puede añadir los suyos.

En Java, si `Artillero` y `Zapador` heredan de `Soldado`, ambos comparten el estado común (el nombre) y el comportamiento común (`saludar()`), pero cada uno añade su especialidad (cohetes o minas). Gracias a la compatibilidad de tipos, se puede tener un único array de `Soldado` con objetos de distintos subtipos y pedir el saludo a todos sin distinguir de qué subtipo es cada objeto.

```java
class Soldado {
	private String nombre;

	public Soldado(String nombre) {
		this.nombre = nombre;
	}

	public void saludar() {
		System.out.println("Hola, soy " + nombre);
	}
}

class Artillero extends Soldado {
	private int numeroCohetes;

	public Artillero(String nombre, int numeroCohetes) {
		super(nombre);
		this.numeroCohetes = numeroCohetes;
	}

	public int getNumeroCohetes() {
		return numeroCohetes;
	}
}

class Zapador extends Soldado {
	private int numeroMinas;

	public Zapador(String nombre, int numeroMinas) {
		super(nombre);
		this.numeroMinas = numeroMinas;
	}

	public int getNumeroMinas() {
		return numeroMinas;
	}
}

public class Demo {
	public static void main(String[] args) {
		Soldado[] peloton = {
			new Artillero("Ana", 5),
			new Zapador("Luis", 3),
			new Artillero("Marta", 8)
		};

		for (Soldado s : peloton) {
			s.saludar();
		}
	}
}
```


## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta

Al crear un objeto de subclase se ejecutan varios constructores, uno por cada nivel de herencia, desde la clase más general hasta la más concreta. Si se crea un `Artillero`, primero se inicializa la parte `Soldado` y después la parte `Artillero`. Ese orden garantiza que el estado común quede preparado antes de inicializar lo específico.

Dentro de un constructor, `super(...)` invoca el constructor de la superclase. En Java, esa llamada debe ser la primera instrucción del constructor de la subclase. Si no se escribe nada, el compilador intenta insertar `super()` automáticamente. Por ello, si la clase base no tiene constructor sin parámetros visible, hay que llamar explícitamente a `super` con los parámetros correctos; en caso contrario, el código no compila.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta

Sí, los atributos privados de la superclase forman parte de la instancia real de la subclase en memoria. Un objeto `Artillero` contiene tanto la parte heredada de `Soldado` (por ejemplo, `nombre`) como la parte propia (`numeroCohetes`). No son dos objetos separados: es un único objeto con una zona de estado heredada y otra específica.

Ahora bien, que ese estado exista en memoria no significa acceso directo desde el código de la subclase. Si `nombre` es `private` en `Soldado`, `Artillero` no puede usar `nombre` directamente; deberá acceder mediante métodos públicos/protegidos de `Soldado`. Por ejemplo, `saludar()` puede usar `nombre` porque está definido en la propia `Soldado`, pero `Artillero` no podría hacer `System.out.println(nombre)` si sigue siendo privado.

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta

La compatibilidad de tipos hace que el código sea extensible porque permite programar contra la abstracción (`Soldado`) y no contra cada subtipo concreto. Cuando se recorre un array de `Soldado` para pedir saludos, ese algoritmo no depende de si hay artilleros, zapadores u otros tipos nuevos que se añadan después.

Si más adelante se incorpora un subtipo `Medico`, el bucle de saludos no se toca. Solo se crea la nueva clase y se añaden instancias al mismo array. Esta propiedad reduce cambios en código existente y disminuye el riesgo de introducir errores al ampliar funcionalidades.

```java
class Medico extends Soldado {
	private int botiquines;

	public Medico(String nombre, int botiquines) {
		super(nombre);
		this.botiquines = botiquines;
	}

	public int getBotiquines() {
		return botiquines;
	}
}

Soldado[] peloton = {
	new Artillero("Ana", 5),
	new Zapador("Luis", 3),
	new Medico("Clara", 2)
};

for (Soldado s : peloton) {
	s.saludar(); // No cambia aunque aparezca Medico
}
```


## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta

Sí, en Java una referencia del supertipo puede apuntar a un objeto real de un subtipo. Es la base del polimorfismo y suele llamarse `upcasting` cuando se pasa de subtipo a supertipo, normalmente de forma implícita y segura. Por ejemplo, `Soldado s = new Artillero("Ana", 5);`.

Con esa referencia de tipo `Soldado`, solo se pueden invocar miembros visibles en `Soldado`. Aunque el objeto real sea `Artillero`, no se puede llamar directamente a `getNumeroCohetes()` porque ese método no existe en el tipo estático `Soldado`. Para acceder a miembros del subtipo, se usa `downcasting` (de supertipo a subtipo), que requiere comprobación previa para evitar errores de ejecución.

`instanceof` sirve para verificar el tipo real antes del cast. Así se evita `ClassCastException`. En el recorrido de un array de `Soldado`, se puede preguntar si cada elemento es un `Artillero`; si lo es, se convierte y se leen los cohetes.

```java
Soldado[] peloton = {
	new Artillero("Ana", 5),
	new Zapador("Luis", 3),
	new Artillero("Marta", 8)
};

for (Soldado s : peloton) {
	s.saludar();

	if (s instanceof Artillero) {
		Artillero a = (Artillero) s; // downcasting seguro tras instanceof
		System.out.println("Cohetes disponibles: " + a.getNumeroCohetes());
	}
}
```


## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta

El acceso `protected` en Java permite que un miembro sea accesible desde la propia clase, desde sus subclases y desde clases del mismo paquete. Es un punto intermedio entre `private` (solo clase propietaria) y `public` (acceso total). Se usa cuando se desea mantener cierta ocultación, pero permitiendo extensión controlada por herencia.

Si se declara `nombre` como `protected` en `Soldado`, `Zapador` puede usarlo directamente en un método propio, por ejemplo para informar de quién coloca minas. Esto funciona, pero conviene aplicar `protected` con prudencia, porque expone más detalle interno a las subclases.

```java
class Soldado {
	protected String nombre;

	public Soldado(String nombre) {
		this.nombre = nombre;
	}
}

class Zapador extends Soldado {
	private int numeroMinas;

	public Zapador(String nombre, int numeroMinas) {
		super(nombre);
		this.numeroMinas = numeroMinas;
	}

	public void ponerMinas() {
		System.out.println(nombre + " ha colocado " + numeroMinas + " minas.");
	}
}
```


## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta

No en todos los lenguajes orientados a objetos existe necesariamente una clase base única para todo. Depende del diseño del lenguaje y de su sistema de tipos. En algunos lenguajes hay jerarquías más flexibles o incluso tipos primitivos fuera de la jerarquía de objetos.

En Java sí existe una raíz común para las clases: `java.lang.Object`. Toda clase hereda directa o indirectamente de `Object`, aunque no se escriba explícitamente `extends Object`. Por eso todos los objetos comparten métodos básicos como `toString()`, `equals()` y `hashCode()`.


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta

La herencia múltiple consiste en que una clase pueda heredar implementación y estado de más de una superclase al mismo tiempo. Esto aporta flexibilidad, pero también puede generar ambigüedades, por ejemplo cuando dos superclases definen métodos con la misma firma y comportamiento distinto.

En Java no existe herencia múltiple de clases: una clase solo puede extender a una clase (`extends` único). Lo que sí permite Java es implementar múltiples interfaces (`implements A, B, C`), que modelan capacidades. Desde Java 8, las interfaces pueden tener métodos `default`, pero las reglas de resolución de conflictos siguen evitando el modelo clásico de herencia múltiple de estado.


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta

Una excepción no controlada en Java se define heredando de `RuntimeException`. Si se desea aportar más contexto del fallo, puede componerse con un objeto de dominio, por ejemplo `Usuario`, para identificar exactamente qué entidad provocó el error. De este modo se mantiene información de negocio junto al mensaje técnico.

Además, para preservar trazabilidad, conviene ofrecer un constructor con causa (`Throwable cause`). Así, el error original puede encadenarse y no se pierde detalle para depuración. El siguiente ejemplo define `UsuarioNoEncontradoException` con ambos constructores.

```java
class Usuario {
	private final String id;
	private final String nombre;

	public Usuario(String id, String nombre) {
		this.id = id;
		this.nombre = nombre;
	}

	public String getId() {
		return id;
	}

	public String getNombre() {
		return nombre;
	}
}

class UsuarioNoEncontradoException extends RuntimeException {
	private final Usuario usuario;

	public UsuarioNoEncontradoException(Usuario usuario) {
		super("Usuario no encontrado: " + usuario.getId());
		this.usuario = usuario;
	}

	public UsuarioNoEncontradoException(Usuario usuario, Throwable cause) {
		super("Usuario no encontrado: " + usuario.getId(), cause);
		this.usuario = usuario;
	}

	public Usuario getUsuario() {
		return usuario;
	}
}
```


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta

No se recomienda usar herencia solo para reutilizar código porque heredar no significa copiar utilidades: significa afirmar una relación semántica fuerte de tipo es-un. Si esa relación no es real, el modelo queda forzado y aparecen diseños frágiles. Por ejemplo, crear una subclase solo para “aprovechar métodos” puede introducir comportamientos que no tienen sentido para ese nuevo tipo.

Además, la herencia acopla estrechamente subclase y superclase. Cualquier cambio interno en la clase base puede impactar a las derivadas. Si lo único que se busca es reutilización, la composición suele ser más segura: se delega en un objeto colaborador sin comprometer la jerarquía de tipos.


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta

Se favorece la composición frente a la herencia porque la composición permite construir comportamiento combinando objetos con responsabilidades pequeñas y claras. Eso reduce acoplamiento y facilita sustituir componentes sin afectar a la jerarquía de tipos. En términos prácticos, se gana flexibilidad para cambiar implementaciones en tiempo de diseño e incluso en tiempo de ejecución.

Con herencia, la decisión queda más rígida porque la subclase depende de la estructura de la superclase. Con composición, en cambio, una clase puede “tener un” colaborador (por ejemplo, una estrategia de cálculo o validación) y delegar en él. Esto encaja mejor con evolución incremental y pruebas unitarias aisladas.


## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta

Cuando se dice que la herencia rompe la encapsulación, se alude a que la subclase necesita conocer detalles internos de cómo funciona la superclase para extenderla correctamente. Aunque esos detalles no formen parte de la API pública, terminan condicionando el comportamiento heredado. Si la superclase cambia su implementación, la subclase puede romperse incluso sin cambios en firmas públicas.

También ocurre que, para hacer la herencia útil, a menudo se relaja visibilidad (`protected`) y se exponen partes del estado o métodos internos a subclases. Esa exposición reduce el aislamiento del objeto y hace más difícil garantizar invariantes. Por eso se recomienda heredar cuando la relación conceptual es sólida y estable, no como mecanismo genérico de reutilización.


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta

La alternativa por herencia modela una jerarquía donde `Estudiante` y `Trabajador` son tipos de `Persona`. Es clara cuando se desea compartir contrato y comportamiento común bajo una relación es-un. La alternativa por composición modela que ambos tienen unos `DatosPersonales`; aquí la relación es tiene-un y resulta más flexible para reutilizar el mismo bloque de datos en otras clases.

En el diseño por composición solicitado, `Estudiante` y `Trabajador` reciben una instancia de `DatosPersonales` en el constructor y delegan en ella el acceso a nombre y DNI. El siguiente código muestra ambas aproximaciones de forma comparativa.

```java
// Opcion 1: Herencia
class Persona {
	private String dni;
	private String nombre;

	public Persona(String dni, String nombre) {
		this.dni = dni;
		this.nombre = nombre;
	}

	public String getDni() {
		return dni;
	}

	public String getNombre() {
		return nombre;
	}
}

class EstudianteHerencia extends Persona {
	public EstudianteHerencia(String dni, String nombre) {
		super(dni, nombre);
	}
}

class TrabajadorHerencia extends Persona {
	public TrabajadorHerencia(String dni, String nombre) {
		super(dni, nombre);
	}
}

// Opcion 2: Composicion
class DatosPersonales {
	private String dni;
	private String nombre;

	public DatosPersonales(String dni, String nombre) {
		this.dni = dni;
		this.nombre = nombre;
	}

	public String getDni() {
		return dni;
	}

	public String getNombre() {
		return nombre;
	}
}

class Estudiante {
	private DatosPersonales datos;

	public Estudiante(DatosPersonales datos) {
		this.datos = datos;
	}

	public DatosPersonales getDatos() {
		return datos;
	}
}

class Trabajador {
	private DatosPersonales datos;

	public Trabajador(DatosPersonales datos) {
		this.datos = datos;
	}

	public DatosPersonales getDatos() {
		return datos;
	}
}
```
