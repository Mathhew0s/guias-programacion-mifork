<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

En C, la composición se logra anidando estructuras dentro de otras. Para representar una línea formada por dos puntos, se define primero una estructura `Punto` con coordenadas `x` e `y`, y luego una estructura `Linea` que contiene dos instancias de `Punto`. La función `distancia` calcula la separación euclidiana entre dos puntos, mientras que `longitudLinea` utiliza esta función para obtener la longitud total de la línea.

```c
#include <math.h>

struct Punto {
    double x;
    double y;
};

struct Linea {
    struct Punto p1;
    struct Punto p2;
};

double distancia(struct Punto a, struct Punto b) {
    double dx = a.x - b.x;
    double dy = a.y - b.y;
    return sqrt(dx * dx + dy * dy);
}

double longitudLinea(struct Linea l) {
    return distancia(l.p1, l.p2);
}
```

Este enfoque refleja la relación "una línea tiene dos puntos", donde los puntos son parte integrante de la línea. La composición en C es explícita mediante la inclusión directa de las estructuras, sin mecanismos de encapsulación que protejan los datos.

## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.

En Java, la composición se expresa mediante campos que son referencias a otros objetos. Para garantizar inmutabilidad, se declaran los campos como `private final`, y no se proporcionan métodos modificadores. La clase `Punto` recibe sus coordenadas en el constructor y ofrece un método `distancia` que calcula la separación hasta otro punto. La clase `Linea` recibe dos puntos en su constructor y los almacena sin posibilidad de modificación posterior.

```java
public class Punto {
    private final double x;
    private final double y;
    
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }
    
    public double distancia(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

public class Linea {
    private final Punto p1;
    private final Punto p2;
    
    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }
    
    public double longitud() {
        return p1.distancia(p2);
    }
}
```

La inmutabilidad se consigue al no exponer métodos que alteren el estado interno. Una vez creada una línea, sus puntos permanecen fijos, superando así la ausencia de encapsulación que presentaba la versión en C.

## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

La multiplicidad en una relación de composición indica cuántas instancias de una clase están asociadas con una instancia de la otra clase. Se expresa mediante notación de rangos como "1", "0..1", "1..*", etc. En el ejemplo de `Linea` y `Punto`, una línea está compuesta exactamente por dos puntos, mientras que un punto no tiene conocimiento ni referencia a la línea que lo contiene, por lo que la multiplicidad en la dirección inversa es cero.

De `Linea` a `Punto`, la multiplicidad es **2**, puesto que cada línea contiene dos puntos. De `Punto` a `Linea`, la multiplicidad es **0**, ya que un punto no tiene relación con la línea que lo compone; es decir, la relación es unidireccional desde la línea hacia los puntos.

Esta asimetría es común en composiciones donde el objeto compuesto conoce a sus partes, pero las partes no conocen al contenedor, simplificando el mantenimiento y evitando dependencias circulares.

## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

La composición fuerte, también denominada simplemente **composición** en la terminología UML, implica que el ciclo de vida de las partes está ligado al del todo. Si el objeto contenedor es destruido, sus partes también lo son. No existe posibilidad de que una parte pertenezca a varios contenedores simultáneamente. Por el contrario, la composición débil, conocida como **agregación** o asociación compartida, permite que las partes tengan un ciclo de vida independiente y puedan ser compartidas entre distintos contenedores.

En la composición fuerte, la creación y destrucción de las partes son responsabilidad exclusiva del contenedor. En la agregación, las partes preexisten o pueden sobrevivir al contenedor. Esta distinción tiene implicaciones directas en la gestión de memoria y en el diseño de las interfaces: en composición fuerte el constructor del contenedor suele crear las partes internas, mientras que en agregación las partes se reciben como parámetros desde el exterior.

## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

Cuando el uso de una clase se limita a recibirla como parámetro, devolverla desde un método, instanciarla localmente dentro de un método o utilizarla como variable local sin almacenarla como campo de la clase, se está ante una **dependencia**, no ante una composición. La dependencia es la relación más débil entre clases e indica que una clase utiliza temporalmente a otra para cumplir una función específica, pero no mantiene una referencia persistente a ella.

La composición, en cambio, requiere que la clase contenedora posea un campo de referencia al objeto componente, estableciendo así una relación estructural duradera. Esta distinción es importante porque afecta al ciclo de vida y a la complejidad del código: las dependencias temporales no implican responsabilidad sobre la creación o destrucción del objeto utilizado.

## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

**Composición fuerte:** La línea crea internamente sus propios puntos y no los expone al exterior. Cuando la línea desaparece, los puntos quedan sin referencias y son elegibles para el recolector de basura.

```java
public class LineaFuerte {
    private final Punto p1;
    private final Punto p2;
    
    public LineaFuerte(double x1, double y1, double x2, double y2) {
        this.p1 = new Punto(x1, y1);
        this.p2 = new Punto(x2, y2);
    }
    
    public double longitud() {
        return p1.distancia(p2);
    }
}
```

**Composición débil (agregación):** La línea recibe los puntos desde el exterior, que pueden ser compartidos con otras líneas y tener una vida independiente.

```java
public class LineaDebil {
    private final Punto p1;
    private final Punto p2;
    
    public LineaDebil(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }
    
    public double longitud() {
        return p1.distancia(p2);
    }
}
```

En la primera versión, los puntos son creados internamente y su ciclo de vida queda atado al de la línea. En la segunda, los puntos preexisten y pueden seguir siendo utilizados incluso después de que la línea deje de ser referenciada.

## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

En Java, no existe una destrucción explícita de objetos como en lenguajes con gestión manual de memoria. La composición fuerte en Java se manifiesta en que los objetos componentes se vuelven inalcanzables cuando el contenedor deja de ser referenciado, siempre que no existan otras referencias hacia ellos. El recolector de basura se encarga de liberar la memoria asociada en un momento posterior indeterminado.

La ausencia de una destrucción explícita no debilita el concepto de composición fuerte, pues la relación semántica se define por la propiedad del ciclo de vida: los componentes no tienen sentido ni existencia independiente fuera del contenedor. El diseñador expresa esta intención creando los componentes dentro del constructor del contenedor y no exponiéndolos al exterior, asegurando que ninguna otra parte del sistema pueda mantener una referencia independiente a ellos.

## 8. Pon un ejemplo de composición débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

```java
public class Departamento {
    private static final int MAX_PROFESORES = 50;
    private Profesor[] profesores;
    private int numProfesores;
    private Profesor director;
    
    public Departamento(Profesor director) {
        this.profesores = new Profesor[MAX_PROFESORES];
        this.numProfesores = 0;
        this.director = director;
        añadirProfesor(director);
    }
    
    public void añadirProfesor(Profesor p) {
        if (numProfesores >= MAX_PROFESORES) {
            throw new IllegalStateException("No se pueden añadir más profesores, límite alcanzado");
        }
        profesores[numProfesores] = p;
        numProfesores++;
    }
    
    public void eliminarProfesor(int posicion) {
        if (posicion < 0 || posicion >= numProfesores) {
            throw new IllegalArgumentException("Posición inválida");
        }
        Profesor eliminado = profesores[posicion];
        if (eliminado.equals(director)) {
            throw new IllegalStateException("No se puede eliminar al director del departamento");
        }
        for (int i = posicion; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[numProfesores - 1] = null;
        numProfesores--;
    }
    
    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El director no puede ser nulo");
        }
        boolean pertenece = false;
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i].equals(nuevoDirector)) {
                pertenece = true;
                break;
            }
        }
        if (!pertenece) {
            throw new IllegalStateException("El nuevo director debe ser un profesor del departamento");
        }
        this.director = nuevoDirector;
    }
    
    public int getNumProfesores() {
        return numProfesores;
    }
    
    public Profesor getProfesor(int posicion) {
        if (posicion < 0 || posicion >= numProfesores) {
            throw new IllegalArgumentException("Posición inválida");
        }
        return profesores[posicion];
    }
    
    public Profesor getDirector() {
        return director;
    }
}
```

Este diseño mantiene la invariante de que el director es siempre uno de los profesores de la lista. Al eliminar un profesor se impide eliminar al director, y al cambiar el director se verifica que el nuevo pertenezca al conjunto de profesores.

## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Departamento {
    private List<Profesor> profesores;
    private Profesor director;
    
    public Departamento(Profesor director) {
        this.profesores = new ArrayList<>();
        this.director = director;
        añadirProfesor(director);
    }
    
    public void añadirProfesor(Profesor p) {
        profesores.add(p);
    }
    
    public void eliminarProfesor(int posicion) {
        if (posicion < 0 || posicion >= profesores.size()) {
            throw new IllegalArgumentException("Posición inválida");
        }
        Profesor eliminado = profesores.get(posicion);
        if (eliminado.equals(director)) {
            throw new IllegalStateException("No se puede eliminar al director del departamento");
        }
        profesores.remove(posicion);
    }
    
    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El director no puede ser nulo");
        }
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalStateException("El nuevo director debe ser un profesor del departamento");
        }
        this.director = nuevoDirector;
    }
    
    public int getNumProfesores() {
        return profesores.size();
    }
    
    public Profesor getProfesor(int posicion) {
        if (posicion < 0 || posicion >= profesores.size()) {
            throw new IllegalArgumentException("Posición inválida");
        }
        return profesores.get(posicion);
    }
    
    public Profesor getDirector() {
        return director;
    }
}
```

El uso de `List` elimina la necesidad de gestionar manualmente el límite máximo, el contador de elementos, el desplazamiento de elementos al eliminar y la liberación de referencias nulas. Todo esto queda encapsulado en la implementación de `ArrayList`.

Si existiera un método que devolviera toda la lista interna directamente, se rompería la encapsulación, permitiendo que código externo añada o elimine profesores sin pasar por los controles de invariante (como impedir eliminar al director). Para resolverlo, se debe devolver una copia no modificable de la lista mediante `Collections.unmodifiableList(profesores)`, o bien devolver un array copia con `profesores.toArray()`.

## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

```java
public final class Persona {
    private final String nombre;
    private final Persona madre;
    
    public Persona(String nombre, Persona madre) {
        this.nombre = nombre;
        this.madre = madre;
    }
    
    public String getNombre() {
        return nombre;
    }
    
    public Persona getMadre() {
        return madre;
    }
    
    public String lineaMaterna() {
        if (madre == null) {
            return nombre;
        }
        return nombre + " -> " + madre.lineaMaterna();
    }
    
    public static void main(String[] args) {
        Persona abuela = new Persona("Ana", null);
        Persona madre = new Persona("María", abuela);
        Persona nieto = new Persona("Carlos", madre);
        
        System.out.println(nieto.lineaMaterna());
        // Salida: Carlos -> María -> Ana
    }
}
```

La composición recursiva se da cuando un objeto contiene una referencia a otro objeto de su mismo tipo, formando una estructura que puede recorrerse recursivamente. Otros ejemplos clásicos incluyen:

- **Árboles**: un nodo contiene referencias a sus nodos hijos.
- **Listas enlazadas**: un nodo contiene referencia al siguiente nodo.
- **Sistemas de archivos**: una carpeta contiene otras carpetas.
- **Expresiones aritméticas**: una expresión puede contener subexpresiones.

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

Las relaciones de composición bidireccionales son aquellas en las que ambas clases involucradas mantienen una referencia mutua. Cada objeto conoce al otro, permitiendo navegar la relación en ambos sentidos. Esto introduce una mayor complejidad porque hay que mantener la consistencia de ambas referencias, asegurando que si un profesor pertenece a un departamento, el departamento tenga a ese profesor en su lista.

Para implementar esta bidireccionalidad en el ejemplo de `Profesor` y `Departamento`, se debe añadir un campo `departamento` en la clase `Profesor` y actualizarlo cada vez que se establece o modifica la relación. Además, es necesario gestionar cuidadosamente los métodos para mantener la consistencia:

```java
public class Profesor {
    private String nombre;
    private Departamento departamento;
    
    public Profesor(String nombre) {
        this.nombre = nombre;
    }
    
    public void setDepartamento(Departamento d) {
        this.departamento = d;
    }
    
    public Departamento getDepartamento() {
        return departamento;
    }
}

// En Departamento, los métodos deben actualizar la referencia inversa
public void añadirProfesor(Profesor p) {
    profesores.add(p);
    p.setDepartamento(this);
}

public void eliminarProfesor(int posicion) {
    Profesor eliminado = profesores.get(posicion);
    if (eliminado.equals(director)) {
        throw new IllegalStateException("No se puede eliminar al director");
    }
    profesores.remove(posicion);
    eliminado.setDepartamento(null);
}

public void cambiarDirector(Profesor nuevoDirector) {
    // validación...
    this.director = nuevoDirector;
    // la referencia inversa ya existe por pertenecer al departamento
}
```

La bidireccionalidad facilita la navegación pero incrementa el acoplamiento y exige mantener la consistencia en todas las operaciones que afectan a la relación.

