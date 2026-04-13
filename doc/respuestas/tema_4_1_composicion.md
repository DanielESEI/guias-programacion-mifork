
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.


En C, la composición de estructuras permite crear tipos complejos a partir de otros más simples. Por ejemplo, una estructura `Punto` puede tener dos coordenadas, y una estructura `Linea` puede estar formada por dos puntos. Esto se expresa como "una línea tiene dos puntos".

```c
struct Punto {
    double x;
    double y;
};

struct Linea {
    struct Punto p1;
    struct Punto p2;
};

double distanciaEntrePuntos(struct Punto a, struct Punto b) {
    double dx = a.x - b.x;
    double dy = a.y - b.y;
    return sqrt(dx * dx + dy * dy);
}

double longitudLinea(struct Linea l) {
    return distanciaEntrePuntos(l.p1, l.p2);
}
```

La composición permite modelar relaciones "tiene-un" de forma natural y reutilizar estructuras. Las funciones asociadas calculan la distancia entre puntos y la longitud de una línea, mostrando cómo se combinan los datos.

## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  


En Java, la composición se implementa mediante clases que contienen referencias a otras clases. Se puede definir una clase `Punto` inmutable, cuyos atributos no cambian tras la construcción, y una clase `Linea` que contiene dos puntos, también inmutable.

```java
public final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distanciaA(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

    public double getX() { return x; }
    public double getY() { return y; }
}

public final class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distanciaA(p2);
    }

    public Punto getP1() { return p1; }
    public Punto getP2() { return p2; }
}
```

La composición en Java permite encapsular y proteger el estado interno, garantizando la inmutabilidad y evitando errores por modificaciones accidentales.

## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.


**Multiplicidad** de A y B (ej: entre línea y punto) en notación UML.

- 1 Línea se relaciona como mínimo con 2 Puntos y como máximo con 2 Puntos.
- 1 Punto se relaciona como mínimo con 0 Líneas y como máximo con muchas Líneas.

La multiplicidad describe cuántas instancias de un tipo están asociadas a otra en una relación de composición. En el ejemplo, una `Linea` está compuesta por exactamente dos `Punto`, mientras que un `Punto` puede formar parte de ninguna, una o varias líneas.

De `Linea` a `Punto`, la multiplicidad es 2 (cada línea tiene dos puntos). De `Punto` a `Linea`, la multiplicidad es 0..n (un punto puede estar en cero o muchas líneas). Esto permite modelar relaciones "uno a varios" o "muchos a muchos" según el contexto.

## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.


### Composición Fuerte vs Débil
 
- **Fuerte** -> El contenedor (ej: línea) es el que crea los objetos que contiene (ej: Punto) y estos no viven más allá del contenedor.
    - El **ciclo de vida** está vinculado al contenedor.
- **Débil** -> El contenedor y el contenido tienen ciclos de vida independientes (ej: los objetos Punto pueden vivir sin estar en objetos línea).

La composición fuerte implica que el ciclo de vida de los objetos contenidos está ligado al del contenedor; si el contenedor se destruye, los objetos contenidos también. En la composición débil, los objetos pueden existir independientemente del contenedor.

La composición fuerte se denomina "composición" propiamente dicha, mientras que la débil se conoce como "asociación" o "agregación". En la fuerte, el contenedor es responsable de crear y destruir los objetos; en la débil, solo mantiene referencias.

La más habitual es la débil, como el ejemplo de la clase.

En **UML** usamos *"rombos"* para expresar que el contenedor es básicamente un contenedor.

## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?


Ejemplos de *"dependencia"*, NO Composición; ej: Punto depende de String y StringBuilder...

Cuando una clase utiliza otra como parámetro, variable local o la instancia dentro de un método, se habla de "dependencia" y no de composición. La dependencia indica que una clase necesita otra para realizar alguna operación, pero no la incorpora como parte de su estructura interna.

La composición implica que una clase contiene referencias a otras como parte de su estado, mientras que la dependencia es más transitoria y limitada al ámbito de métodos o funciones.


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

Composicón Débil ya hecho.

**COMPOSICION FUERTE**
```java
class Lines {
    private Punto p1;
    private Punto p2;

    public Linea (double x1, double x2, double y1, double y2) {
        this.p1 = new Punto(x1, y1);
        this.p2 = new Punto(x2, y2);
    }

    public Punto getP1() return this.p1; // No se puede hacer (PROHIBIDO)
    public double getX1() return this.p1.getX();
}

```

## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?


En Java, la vida de Punto termina cuando es inaccesible y en el ejemplo, ocurre cuando línea deja de serlo a su vez. Por tanto, cuando línea "es basura", también lo serán sus puntos y serán eliminados de memoria por el recolector de basura.

En Java, la gestión de memoria se realiza mediante el recolector de basura. Cuando el contenedor (por ejemplo, `Linea`) deja de ser accesible, sus objetos internos (`Punto`) también pueden ser eliminados si no hay otras referencias. No es necesario destruir explícitamente los objetos.

Esto ocurre porque Java no tiene destructores manuales como C++; la eliminación es automática cuando no hay referencias. Por eso, la composición fuerte se basa en la propiedad de referencia, no en la destrucción manual.

## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.


Se puede implementar la relación usando un array privado y controlando el acceso mediante métodos públicos. El director debe ser uno de los profesores del departamento y la clase debe lanzar excepciones si se viola la invariante.

- Hay 2 composiciones débiles.
- No se expone al array al exterior (imposible garantizar invariante de clase)
- En los métodos que gestionan el departamento se controla que no se viole la invariante de clase.

```java
public class Profesor {
    private final String nombre;
    public Profesor(String nombre) { this.nombre = nombre; }
    public String getNombre() { return nombre; }
}

public class Departamento {
    private final Profesor[] profesores = new Profesor[50];
    private int numProfesores = 0;
    private Profesor director;

    public Departamento(Profesor director) {
        this.director = director;
        profesores[0] = director;
        numProfesores = 1;
    }

    public void addProfesor(Profesor p) {
        if (numProfesores >= 50) throw new IllegalStateException("Máximo alcanzado");
        profesores[numProfesores++] = p;
    }

    public void removeProfesor(int pos) {
        if (pos < 0 || pos >= numProfesores) throw new IndexOutOfBoundsException();
        if (profesores[pos] == director) throw new IllegalStateException("No se puede eliminar al director");
        for (int i = pos; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[--numProfesores] = null;
    }

    public int getNumProfesores() { return numProfesores; }
    public Profesor getProfesor(int pos) {
        if (pos < 0 || pos >= numProfesores) throw new IndexOutOfBoundsException();
        return profesores[pos];
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        boolean encontrado = false;
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == nuevoDirector) {
                encontrado = true;
                break;
            }
        }
        if (!encontrado) throw new IllegalArgumentException("El director debe ser profesor del departamento");
        director = nuevoDirector;
    }

    public Profesor getDirector() { return director; }
}
```

La encapsulación se mantiene y las invariantes se controlan mediante excepciones.

## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?


Usando `List`, se simplifica la gestión de profesores, eliminando la necesidad de controlar el tamaño y el desplazamiento manual de elementos. Además, se puede devolver una copia de la lista para evitar exponer la estructura interna.

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Departamento {
    private final List<Profesor> profesores = new ArrayList<>();
    private Profesor director;

    public Departamento(Profesor director) {
        this.director = director;
        profesores.add(director);
    }

    public void addProfesor(Profesor p) {
        profesores.add(p);
    }

    public void removeProfesor(int pos) {
        Profesor p = profesores.get(pos);
        if (p == director) throw new IllegalStateException("No se puede eliminar al director");
        profesores.remove(pos);
    }

    public int getNumProfesores() { return profesores.size(); }
    public Profesor getProfesor(int pos) { return profesores.get(pos); }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (!profesores.contains(nuevoDirector)) throw new IllegalArgumentException("El director debe ser profesor del departamento");
        director = nuevoDirector;
    }

    public Profesor getDirector() { return director; }

    public List<Profesor> getProfesores() {
        return Collections.unmodifiableList(profesores);
    }
}
```

Al devolver la lista interna directamente, se podría romper la encapsulación si el cliente modifica la lista. Para evitarlo, se devuelve una copia o una vista no modificable.

Con List<Profesor>...
1. No cambia la interfaz pública.
2. Es más fácil implementar algunos métodos, delegando en métodos de List.
3. Si se devuelve, hay que devolver una copia, para proteger la invariante de clase.

## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.


La composición recursiva se da cuando una clase contiene referencias a instancias de sí misma. En el caso de `Persona`, se puede definir como inmutable y con una madre, que es otra `Persona`.

- Composiciones **reflexivas o recursivas**.
- Composiciones **bidireccionales**.

```java
public final class Persona {
    private final String nombre;
    private final Persona madre;

    public Persona(String nombre, Persona madre) {
        this.nombre = nombre;
        this.madre = madre;
    }

    public String getNombre() { return nombre; }
    public Persona getMadre() { return madre; }
}

public class Main {
    public static void main(String[] args) {
        Persona abuela = new Persona("Ana", null);
        Persona madre = new Persona("Beatriz", abuela);
        Persona nieto = new Persona("Carlos", madre);
        System.out.println(nieto.getNombre() + " -> " +
            nieto.getMadre().getNombre() + " -> " +
            nieto.getMadre().getMadre().getNombre());
    }
}
```

Otros ejemplos clásicos de composición recursiva son árboles (cada nodo tiene hijos que son nodos), listas enlazadas, y estructuras de directorios en sistemas de archivos.

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?


Las relaciones de composición bidireccionales implican que ambos objetos mantienen referencias entre sí. Por ejemplo, un `Profesor` tiene un campo para su `Departamento` y el `Departamento` mantiene una lista de `Profesor`.

Para implementarlo, al añadir un profesor al departamento, se debe actualizar el campo del profesor para que apunte al departamento, y viceversa. Es importante evitar inconsistencias y ciclos de referencia, y gestionar correctamente la actualización de ambos lados de la relación.

Las bidireccionales exigen programar cuidadosamente para mantener la consistencia.
P.ej: Si añado un profesor al departamento, debo actualizar la inferencia al Departamento desde Profesor.