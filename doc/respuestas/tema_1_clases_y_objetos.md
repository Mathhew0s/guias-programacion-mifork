# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una
Las cuatro características básicas de la programación orientada a objetos son abstracción, encapsulación, herencia y polimorfismo. 
Estas propiedades permiten organizar el código en torno a entidades que combinan datos y comportamiento, facilitando la comprensión
del sistema y mejorando su mantenimiento. Cada una aporta un mecanismo concreto para estructurar el software de forma más modular y reutilizable.

La abstracción consiste en representar un concepto mediante una clase que expone únicamente los aspectos esenciales y oculta los detalles
internos innecesarios. Gracias a esto, se trabaja con modelos simplificados que reflejan con claridad las entidades del programa, evitando
que la complejidad de la implementación interfiera en su uso.

La encapsulación agrupa datos y métodos dentro de una clase y controla su acceso mediante modificadores de visibilidad. Con este principio 
se garantiza que el estado del objeto solo pueda modificarse a través de interfaces bien definidas, lo que reduce errores, evita dependencias
indeseadas y mejora la modularidad del sistema.

La herencia permite crear nuevas clases basadas en otras existentes, reutilizando atributos y comportamientos sin duplicar código. Esto facilita
la organización jerárquica y la especialización de conceptos. Por su parte, el polimorfismo hace posible que un mismo método produzca comportamientos
distintos según el objeto que lo implemente, ofreciendo mayor flexibilidad y permitiendo trabajar con interfaces comunes de manera uniforme.

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

Entre los lenguajes más utilizados que soportan programación orientada a objetos se encuentran Java, que aplica este paradigma de manera estricta y
resulta muy común en aplicaciones empresariales y en el desarrollo para Android. Junto a él destaca C++, que combina características de bajo nivel 
con un modelo orientado a objetos, siendo habitual en sistemas, motores gráficos y software de alto rendimiento.

Otro lenguaje ampliamente extendido es Python, que ofrece un enfoque orientado a objetos sencillo y flexible, permitiendo mezclar este paradigma con
otros sin perder claridad. También es relevante C#, una opción moderna dentro del ecosistema .NET, utilizada en aplicaciones de escritorio, web y en 
el desarrollo de videojuegos a través de motores como Unity.

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

La programación estructurada es un paradigma que organiza el código mediante estructuras de control claras, como secuencia, selección y repetición. Su
objetivo es evitar el uso desordenado de saltos como *goto*, obteniendo un flujo de ejecución más predecible y fácil de seguir. Gracias a esta organización,
el programa se vuelve más legible y resulta más sencillo localizar y corregir errores.

La programación modular lleva este enfoque un paso más allá al dividir el programa en módulos independientes que agrupan funciones relacionadas dentro de 
una misma unidad lógica. Cada módulo expone solo lo necesario a través de una interfaz, ocultando sus detalles internos. Esto favorece la reutilización del 
código, reduce las dependencias entre partes del sistema y facilita tanto el mantenimiento como el trabajo colaborativo entre distintos desarrolladores.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

Un objeto se define mediante tres elementos fundamentales: estado, comportamiento e identidad. Estos componentes permiten representar una entidad completa
dentro del sistema, combinando los datos que almacena y las operaciones que puede realizar.

El estado está formado por los atributos o propiedades que contienen la información del objeto. Estos valores pueden cambiar durante la ejecución del programa
y determinan la situación concreta del objeto en cada momento.

El comportamiento corresponde a los métodos que el objeto puede ejecutar. A través de ellos se especifican las acciones que puede realizar, cómo interactúa con
otros objetos y de qué forma puede modificar su propio estado.

La identidad distingue a un objeto de cualquier otro, incluso aunque compartan el mismo estado y comportamiento. Esta identidad es única y permite referirse de
manera inequívoca a cada instancia dentro de la memoria o del sistema donde se está ejecutando.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

Una clase es una plantilla o modelo que define los atributos y métodos que compartirán los objetos creados a partir de ella. En esencia, describe qué datos pueden
almacenar esos objetos y qué operaciones pueden llevar a cabo. Gracias a esta abstracción, es posible representar conceptos del programa de forma coherente y 
organizada.

Una clase no es lo mismo que un objeto. La clase actúa solo como definición, mientras que el objeto es una entidad concreta creada a partir de dicha definición. 
Al crear un objeto, se asignan valores reales a los atributos descritos en la clase y se reserva espacio en memoria, motivo por el cual se considera que un objeto
es una instancia de una clase.

El término instancia se refiere precisamente al resultado de crear un objeto basado en una clase. Cada instancia posee su propio estado, aunque comparta estructura
y comportamiento con otras instancias. Este concepto permite trabajar con múltiples objetos derivados del mismo modelo sin que interfieran entre sí.

No todos los lenguajes orientados a objetos utilizan el concepto de clase. Lenguajes como Java, C++ o C# dependen completamente de ellas, mientras que otros, 
como JavaScript, emplean un sistema basado en prototipos. En estos casos, los objetos pueden heredar directamente de otros objetos sin necesitar una definición 
formal de clase.

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

En lenguajes como Java, los objetos suelen almacenarse en una zona de memoria llamada heap. Este espacio está destinado a datos cuyo tiempo de vida no puede 
conocerse en tiempo de compilación, por lo que los objetos creados dinámicamente residen allí hasta que dejan de ser necesarios. En cambio, las variables locales 
y las referencias a esos objetos suelen almacenarse en la pila de ejecución, que es más limitada y sigue el orden de llamadas a métodos.

Este comportamiento no es igual en todos los lenguajes. En C++, por ejemplo, los objetos pueden almacenarse tanto en la pila como en el heap, según cómo se creen:
como variables automáticas o mediante operadores como new. Lenguajes como Python también gestionan sus objetos en memoria dinámica, aunque la organización concreta 
depende de la implementación del intérprete o de la máquina virtual. Cada lenguaje establece sus propias reglas sobre cómo y dónde se almacenan los datos.

La recolección de basura es un mecanismo automático que libera memoria ocupada por objetos que ya no son accesibles desde ninguna parte del programa. Este sistema 
analiza las referencias activas y elimina aquellos objetos que han quedado sin uso, evitando fugas de memoria y reduciendo la necesidad de gestión manual. Es habitual 
en lenguajes como Java o Python, mientras que otros, como C o C++, requieren que el programador gestione la memoria de forma explícita.


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

Un método es una función definida dentro de una clase que representa una acción o comportamiento asociado a los objetos creados a partir de ella. A través de los
métodos, los objetos pueden operar sobre sus propios datos internos e interactuar con otros elementos del sistema. Su propósito es encapsular operaciones que 
tengan sentido dentro del contexto de la clase y proporcionar una interfaz ordenada para manipular el estado del objeto o realizar cálculos relacionados.

La sobrecarga de métodos consiste en definir varios métodos con el mismo nombre dentro de una misma clase, siempre que se diferencien en el tipo o la cantidad de 
sus parámetros. Este mecanismo permite ofrecer distintas formas de realizar una misma operación sin recurrir a nombres diferentes. Con ello se mantiene una 
interfaz más coherente y se facilita la lectura del código al agrupar comportamientos similares bajo un mismo identificador.


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

A continuación se muestra una clase sencilla llamada Punto con dos atributos de visibilidad por defecto, x e y, y un método `calculaDistanciaOrigen` que 
devuelve la distancia euclidiana al origen. Tras la clase, se incluye un ejemplo con un método main para crear una instancia, asignar valores y mostrar el 
resultado por consola.

class Punto {
    int x; // visibilidad por defecto (package-private)
    int y; // visibilidad por defecto (package-private)

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

public class Main {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3;
        p.y = 4;
        System.out.println("Distancia al origen: " + p.calculaDistanciaAOrigen());
    }
}




## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

El punto de entrada en un programa en Java es el método `main`, cuya firma debe ser exactamente `public static void main(String[] args)`. Este método es invocado 
por la máquina virtual Java al iniciar la ejecución del programa y marca el inicio del flujo de instrucciones. Si una clase no contiene un método con esta firma, 
no puede ejecutarse de manera independiente.

La palabra clave `static` indica que un atributo o método pertenece a la clase y no a sus instancias. Esto permite usarlo sin necesidad de crear un objeto. En el 
caso del método `main`, se utiliza porque la JVM debe poder ejecutarlo sin disponer previamente de ninguna instancia creada. Sin embargo, `static` puede emplearse 
en cualquier método o atributo cuyo comportamiento no dependa del estado concreto de un objeto.

La combinación de `static` con `final` se utiliza habitualmente para declarar constantes. Un atributo marcado como `static final` pertenece a la clase y, además, 
no puede modificarse después de su inicialización. Esto garantiza que su valor sea único, inmutable y accesible desde cualquier parte del programa sin necesidad de
crear objetos, cumpliendo así la función típica de las constantes simbólicas.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

Para compilar un programa Java desde la línea de comandos se utiliza el compilador `javac`. El código fuente debe guardarse en un archivo con extensión `.java`, y 
desde la carpeta donde se encuentre basta ejecutar `javac NombreArchivo.java`. Si no hay errores, se generarán uno o varios archivos con extensión `.class`. Para 
ejecutar el programa se usa el comando `java`, indicando solo el nombre de la clase que contiene el método `main`, por ejemplo `java Main`, sin añadir la extensión.

Java es un lenguaje compilado, pero la compilación no produce código máquina específico de un procesador. En lugar de eso, el compilador genera un formato 
intermedio llamado bytecode. Este bytecode es independiente de la plataforma y es el mismo en cualquier sistema operativo, lo que hace que Java se considere a la 
vez compilado e interpretado, ya que no se ejecuta directamente sobre el hardware.

La máquina virtual de Java (JVM) es la encargada de interpretar y ejecutar el bytecode. Actúa como una capa intermedia entre el programa y el sistema operativo, 
permitiendo que el mismo archivo `.class` se ejecute en distintas plataformas siempre que exista una JVM compatible. Esta característica es la que da lugar al 
principio 
*write once, run anywhere*.

Los archivos `.class` contienen el bytecode generado por el compilador. Se trata de un conjunto de instrucciones diseñadas para ser procesadas por la JVM, que puede
interpretarlas o recompilarlas en tiempo de ejecución mediante técnicas como la compilación JIT. Gracias a esto, los programas Java combinan portabilidad con un rendimiento
cercano al de lenguajes compilados directamente a código máquina.


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

La palabra clave `new` es el operador que crea una instancia en memoria a partir de una clase. Al usar `new`, se reserva espacio (habitualmente en el heap), se inicializa el objeto 
ejecutando su constructor y se devuelve una referencia a la nueva instancia, que puede guardarse en una variable para su uso.

Un constructor es un método especial cuyo objetivo es inicializar correctamente las nuevas instancias. Se invoca automáticamente tras `new`, comparte nombre con la 
clase y no declara tipo de retorno. En él se establecen los valores iniciales de los atributos y se garantiza que el objeto empiece en un estado válido. Si no se 
define ninguno, el compilador genera uno por defecto sin parámetros, siempre que no existan otros constructores definidos.

Ejemplo de clase `Empleado` con un constructor que recibe DNI, nombre y apellidos, y un `main` de uso:

class Empleado {
    String dni;
    String nombre;
    String apellidos;

    // Constructor que obliga a crear el empleado con datos completos
    Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }

    public static void main(String[] args) {
        Empleado e = new Empleado("12345678A", "Ana", "García López");
        System.out.println(e.dni + " - " + e.nombre + " " + e.apellidos);
    }
}

## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

La referencia `this` identifica al objeto actual dentro del código de una clase. Se utiliza para acceder a sus atributos y métodos desde el interior de la propia 
instancia y resulta útil para resolver ambigüedades de nombres entre parámetros y campos. También puede emplearse para encadenar constructores o pasar la referencia
del objeto actual a otros métodos.

No todos los lenguajes la nombran igual. En Java y C# se usa `this`, en Python se utiliza el parámetro convencional `self`, y en VB.NET aparece como `Me`. Aunque 
el nombre cambie, el propósito es el mismo: referirse explícitamente a la instancia en curso.

Ejemplo de uso en la clase Punto, resolviendo la ambigüedad entre atributos y parámetros y reutilizando métodos internos:

```java
class Punto {
    int x;
    int y;

    Punto(int x, int y) {
        // this.x y this.y son los atributos de la instancia;
        // x e y (sin this) son los parámetros del constructor.
        this.x = x;
        this.y = y;
    }

    void mover(int x, int y) {
        this.x = x;
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        // Se usa this para dejar claro que se accede a los campos de la instancia
        return Math.sqrt(this.x * this.x + this.y * this.y);
        // Alternativa: return Math.hypot(this.x, this.y);
    }
}
```


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

Se puede añadir un método `distanciaA` que reciba otro objeto de la clase Punto y calcule la distancia euclidiana entre ambos puntos. El cálculo se basa en 
obtener las diferencias en x e y y aplicar la raíz cuadrada de la suma de los cuadrados. La implementación utiliza `this` para referirse a la instancia actual 
y acceder a sus campos.

A continuación se muestra la clase con el nuevo método y un ejemplo breve de uso:

```java
class Punto {
    int x;
    int y;

    Punto(int x, int y) {
        this.x = x;
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
        // Alternativa: return Math.hypot(this.x, this.y);
    }

    // Nuevo método solicitado
    double distanciaA(Punto otro) {
        int dx = this.x - otro.x;
        int dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
        // Alternativa: return Math.hypot(dx, dy);
    }

    public static void main(String[] args) {
        Punto a = new Punto(3, 4);
        Punto b = new Punto(0, 0);
        System.out.println("Distancia entre a y b: " + a.distanciaA(b)); // 5.0
    }
}
```



## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado 
## como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se 
## modificase dentro de la función? 

En Java, el paso de parámetros se realiza siempre por valor. Cuando se pasa un objeto como un `Punto`, lo que se copia es el valor de la referencia, no el objeto 
completo. Tanto el parámetro del método como la variable original apuntan al mismo objeto en memoria, por lo que si dentro del método se modifican los atributos 
del `Punto`, esos cambios se reflejan también fuera del método.

Sin embargo, si dentro del método se asigna una nueva referencia al parámetro, por ejemplo `p = new Punto(...)`, esa reasignación solo afecta a la referencia local.
La variable original del llamador sigue apuntando al objeto inicial, de modo que no cambia lo que ocurre fuera del método.

Cuando el parámetro es un tipo primitivo como `int`, lo que se copia es simplemente su valor. Cualquier modificación realizada dentro del método actúa únicamente 
sobre esa copia local, sin alterar el valor original de la variable en el código que hizo la llamada.

En resumen: cuando se pasa un objeto, modificar su estado interno afecta al exterior; cuando se pasa un tipo primitivo, los cambios quedan aislados dentro del método.

## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

`toString()` es un método heredado de la clase base `Object` que devuelve una representación textual de la instancia. Su propósito es ofrecer una descripción legible
del estado del objeto, útil para depuración, registro y salida por consola. Muchas operaciones de impresión lo invocan de forma implícita al convertir un objeto a cadena.

Este concepto existe en otros lenguajes con nombres similares o equivalentes. En C# se usa `ToString()`, en Python se emplea `__str__` (y `__repr__` para una representación
más formal), y en JavaScript puede definirse `toString()` en prototipos u objetos. En todos los casos, la idea es proporcionar una cadena que describa el objeto de forma 
humana o útil para diagnóstico.

Ejemplo de implementación de `toString()` en la clase Punto:

```java
class Punto {
    int x;
    int y;

    Punto(int x, int y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public String toString() {
        return "(" + x + ", " + y + ")";
    }

    public static void main(String[] args) {
        Punto p = new Punto(3, 4);
        // toString() se invoca implícitamente en la concatenación
        System.out.println("Punto: " + p);       // imprime: Punto: (3, 4)
        // y también puede llamarse de forma explícita
        System.out.println(p.toString());         // imprime: (3, 4)
    }
}
```

## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


Un `struct` en C se parece a una clase únicamente en que ambos permiten agrupar varios datos bajo un mismo tipo compuesto. En un `struct` se pueden definir campos 
relacionados, igual que en una clase se definen atributos. Sin embargo, la similitud termina ahí, porque un `struct` no incorpora ninguno de los mecanismos propios
de la programación orientada a objetos.

Lo primero que falta es la capacidad de incluir métodos. En C, un `struct` solo almacena datos, mientras que una clase combina datos y comportamiento dentro de una
misma unidad. Esta unión es esencial en la orientación a objetos, donde los métodos definen cómo se manipula el estado y cómo interactúa una instancia con otras.

Tampoco existe en C la noción de instancia como entidad con identidad o comportamiento propio. Una variable de tipo `struct` es simplemente un bloque de memoria con
campos, sin constructores, destructores ni inicialización automática. A diferencia de una clase, no dispone de mecanismos como `this`, ni permite trabajar con herencia,
polimorfismo o llamadas implícitas al crear o destruir objetos.

Además, un `struct` carece de encapsulación. En C no existen modificadores de acceso, de modo que todos los campos son accesibles sin restricciones. En las clases, 
en cambio, es posible controlar qué información es pública y cuál permanece oculta, protegiendo el estado interno. Por todo esto, aunque un `struct` pueda recordar 
superficialmente a una clase, no puede desempeñar el papel que esta tiene en el paradigma orientado a objetos.

## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? 
## ¿Qué ha pasado con `this`?

Para emular una clase en C se puede definir un `struct` que agrupe los datos y, por separado, funciones que operen sobre ese `struct`. La función que calcula la 
distancia al origen recibirá el `struct` por valor o un puntero a él como parámetro explícito. Así se separan datos y comportamiento, pero se logra una organización
similar a la de una clase sencilla.

En este enfoque no existe `this` como referencia implícita al objeto actual. Al no haber métodos dentro del `struct`, la referencia al “objeto” debe pasarse como 
argumento a las funciones. Si se usa un puntero, ese primer parámetro cumple el papel de `this` de forma explícita y permite leer o modificar los campos.

```c
#include <stdio.h>
#include <math.h>

typedef struct {
    int x;
    int y;
} Punto;

/* Versión por valor: no modifica el Punto */
double distancia_a_origen(Punto p) {
    return sqrt((double)p.x * p.x + (double)p.y * p.y);
}

/* Versión por puntero: el primer parámetro hace de “this” explícito */
double distancia_a_origen_ptr(const Punto* p) {
    return sqrt((double)p->x * p->x + (double)p->y * p->y);
}

/* Ejemplo de “método” que modifica el estado: mover el punto */
void mover(Punto* p, int dx, int dy) {
    p->x += dx;
    p->y += dy;
}

int main(void) {
    Punto a = {3, 4};
    printf("Distancia (por valor): %.1f\n", distancia_a_origen(a));          // 5.0
    printf("Distancia (por puntero): %.1f\n", distancia_a_origen_ptr(&a));   // 5.0

    mover(&a, -3, -4);
    printf("Tras mover, distancia: %.1f\n", distancia_a_origen_ptr(&a));     // 0.0
    return 0;
}
```

Si se quisieran “métodos” adicionales, se definirían funciones que reciban un puntero a `Punto` para modificar sus campos. Esto hace explícito que la responsabilidad
de pasar la referencia recae en el llamador y que no existe invocación ligada al objeto como en lenguajes orientados a objetos.
