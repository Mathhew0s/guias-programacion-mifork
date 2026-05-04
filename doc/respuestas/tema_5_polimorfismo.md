<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

### Respuesta
El polimorfismo es la capacidad de un objeto de adoptar múltiples formas, de modo que una misma referencia (o llamada a método) pueda comportarse de manera diferente según el tipo real del objeto que se esté utilizando. En programación orientada a objetos, el polimorfismo permite escribir código más genérico y reutilizable, ya que se puede tratar a objetos de distintas clases hijas como si fueran de la clase base, y al invocar un método se ejecutará la versión correspondiente al objeto real (no al tipo de la referencia). Esto facilita la extensibilidad y el mantenimiento del software.

La sobreescritura (overriding) es el mecanismo que permite a una subclase redefinir un método heredado de su superclase, proporcionando una implementación específica para ese método. Para que exista sobreescritura, el método en la subclase debe tener el mismo nombre, misma lista de parámetros y un tipo de retorno compatible (covariante). La sobreescritura es la base del polimorfismo de inclusión, ya que es la herramienta que permite que un método se comporte de forma distinta según la subclase concreta.

## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta
La ligadura dinámica (o enlace tardío) es el proceso mediante el cual la llamada a un método se asocia con el código del método en tiempo de ejecución, no en tiempo de compilación. Esto permite que el programa decida qué versión de un método ejecutar según el tipo real del objeto que se encuentra en la referencia, no según el tipo declarado de la referencia. El polimorfismo depende directamente de la ligadura dinámica, pues sin ella no sería posible que una referencia de tipo base ejecute el método de la subclase concreta.

La forma de indicar la ligadura dinámica depende del lenguaje. En Java, por defecto todos los métodos no estáticos, no privados y no finales usan ligadura dinámica; no hay que indicarlo explícitamente. En C++, los métodos deben declararse como `virtual` para que usen enlace tardío; si no se usa `virtual`, la ligadura es estática (en tiempo de compilación). En Python, todos los métodos son dinámicos por defecto (como en Java), sin necesidad de una palabra clave especial. Así, Java y Python ofrecen polimorfismo sin anotaciones adicionales, mientras que en C++ el programador debe especificar `virtual` para obtener el mismo comportamiento.

## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta
```java
class Soldado {
    public void saluda() {
        System.out.println("Soldado saludando.");
    }
}

class Zapador extends Soldado {
    @Override
    public void saluda() {
        System.out.println("Zapador: ¡A sus órdenes!");
    }
}

class Artillero extends Soldado {
    // No sobreescribe saluda, usa el de Soldado
}

public class PruebaPolimorfismo {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];
        ejercito[0] = new Soldado();
        ejercito[1] = new Zapador();
        ejercito[2] = new Artillero();

        for (Soldado s : ejercito) {
            s.saluda();  // Ligadura dinámica: ejecuta la versión real
        }
    }
}
```
La salida sería:
```
Soldado saludando.
Zapador: ¡A sus órdenes!
Soldado saludando.
```
Aunque todas las referencias son de tipo `Soldado`, al llamar a `saluda` se ejecuta el método del objeto real: `Zapador` usa su propia versión, mientras que `Artillero` hereda la del padre. Así se demuestra el polimorfismo.

## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta
Sí, es posible invocar el método de la clase base dentro de la sobreescritura. Esto se logra mediante la palabra clave `super`, que permite acceder a miembros (métodos o atributos) de la superclase directa. De esta forma, la subclase puede extender o modificar parcialmente el comportamiento heredado, reutilizando la lógica de la clase base y añadiendo funcionalidad adicional.

El ejemplo modificado para `Zapador` quedaría así:
```java
class Zapador extends Soldado {
    @Override
    public void saluda() {
        super.saluda();  // Invoca al método de Soldado
        System.out.println("ZAPADOR A SUS ORDENES");
    }
}
```
Al llamar a `saluda` sobre un `Zapador`, primero se ejecuta `super.saluda()` (que imprime "Soldado saludando.") y luego se agrega el nuevo mensaje. La palabra clave empleada es `super`.

## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta
Para sobreescribir un método en Java, la lista de parámetros debe ser idéntica (mismo número, tipos y orden). El tipo de retorno debe ser el mismo o un subtipo del tipo de retorno del método original (retorno covariante). Además, el método sobreescritor no puede tener un nivel de acceso más restrictivo (por ejemplo, no se puede hacer `private` si el original era `public`), y no puede lanzar excepciones más amplias o nuevas excepciones chequeadas que las declaradas en el método base.

La sobreescritura (overriding) redefine un método existente en la jerarquía de herencia, con la misma firma, para proporcionar una implementación específica de una subclase. La sobrecarga (overloading) consiste en definir múltiples métodos con el mismo nombre pero diferente lista de parámetros (número, tipo u orden), dentro de la misma clase; no requiere herencia y no es polimórfica en tiempo de ejecución. La anotación `@Override` indica al compilador que se pretende sobreescribir un método de la superclase. Es recomendable usarla siempre porque detecta errores comunes, como escribir mal el nombre o los parámetros, y mejora la legibilidad del código, dejando clara la intención de sobreescribir.

## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta
Sí, se emplea polimorfismo desde etapas muy tempranas, aunque a menudo no se mencione explícitamente con ese nombre. Cuando se sobreescribe el método `toString()` (heredado de `Object`) para que un objeto devuelva una representación adecuada, ya se está usando polimorfismo: al imprimir el objeto con `System.out.println(objeto)`, el compilador genera una llamada a `toString()` y, gracias a la ligadura dinámica, se ejecuta la versión sobrescrita de la clase concreta. Lo mismo ocurre al sobreescribir `equals(Object o)` para personalizar la igualdad lógica.

De hecho, toda clase en Java hereda de `Object`, y la capacidad de que una referencia de tipo `Object` pueda invocar métodos como `toString` y que se ejecute la versión de la subclase real es un ejemplo puro de polimorfismo. Por tanto, desde los primeros ejercicios donde se redefine `toString` en una clase `Persona` o `CuentaBancaria`, el estudiante está aplicando el concepto de polimorfismo sin saberlo.

## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta
Una clase abstracta es una clase que no puede ser instanciada; su propósito es servir como base para otras clases. Un método abstracto es un método declarado sin implementación (solo la firma), y obliga a las subclases concretas a proporcionar dicha implementación. Para declarar un método abstracto se usa la palabra clave `abstract` en la firma. Si una clase contiene al menos un método abstracto, la clase completa debe ser declarada abstracta.

No se pueden crear instancias de una clase abstracta mediante `new`, porque su implementación está incompleta. El ejemplo sería:
```java
abstract class Soldado {
    public void saluda() {
        System.out.println("Soldado saludando.");
    }
    public abstract void atacar();  // método abstracto
}

class Zapador extends Soldado {
    @Override
    public void atacar() {
        System.out.println("Zapador coloca una carga explosiva.");
    }
}

class Artillero extends Soldado {
    @Override
    public void atacar() {
        System.out.println("Artillero dispara el cañón.");
    }
}
```
La palabra `abstract` debe colocarse: en la declaración de la clase (`abstract class Soldado`) y en la declaración del método (`public abstract void atacar();`). Las subclases no abstractas deben implementar `atacar`.

## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta
En Java, `final` aplicado a un método impide que las subclases puedan sobreescribir ese método. Aplicado a una clase, impide que se pueda heredar de ella (es decir, no se pueden crear subclases). `final` también se usa en variables para hacerlas constantes, pero en el contexto del polimorfismo interesa su efecto sobre métodos y clases. Al hacer un método `final`, se elimina la posibilidad de que una subclase proporcione una versión polimórfica diferente; el método se liga estáticamente en tiempo de compilación si es privado o final, aunque en la práctica la JVM aún puede optimizarlo. Al hacer una clase `final`, se bloquea por completo la herencia, por lo que no puede haber polimorfismo de subtipado a partir de ella.

La relación con el polimorfismo es que `final` restringe o anula las capacidades polimórficas. Por ejemplo, si una clase se declara `final`, no se podrán tener referencias polimórficas a subclases (porque no existen). Si un método es `final`, las subclases no pueden cambiar su comportamiento mediante sobreescritura. La API estándar de Java contiene varias clases `final`: `String`, `Integer`, `Math`, `System`, etc. `String` es `final` para garantizar su inmutabilidad y seguridad, impidiendo que alguien cree una subclase que altere su comportamiento esencial.

## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta
Una interfaz en Java es un tipo de referencia que define un contrato: un conjunto de métodos abstractos (sin implementación) que las clases que la implementan deben proporcionar. A partir de Java 8, las interfaces pueden incluir métodos `default` y `static` con implementación, pero su esencia sigue siendo la de especificar un comportamiento esperado. Las interfaces permiten el polimorfismo de forma similar a las clases abstractas, pero con la diferencia fundamental de que una clase puede implementar múltiples interfaces, mientras que solo puede heredar de una clase (sea abstracta o concreta). Además, las interfaces no tienen estado (no admiten atributos de instancia, solo constantes estáticas).

Comparadas con las clases abstractas, las interfaces son más flexibles para definir capacidades transversales (por ejemplo, `Comparable`, `Serializable`). Una clase abstracta puede tener métodos concretos y atributos de instancia, lo que permite compartir código entre jerarquías, pero limita a una única herencia. Sí, una clase en Java puede implementar más de una interfaz: `class MiClase implements InterfaceA, InterfaceB, InterfaceC`. Esto es la base para lograr un comportamiento similar a la herencia múltiple, pero sin los problemas de ambigüedad (Java resuelve conflictos con reglas claras).

## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta
```java
abstract class Punto {
    public abstract double calcularDistanciaA(Punto otro);
}

class Punto2D extends Punto {
    private double x, y;
    public Punto2D(double x, double y) { this.x = x; this.y = y; }
    @Override
    public double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto2D) {
            Punto2D p2 = (Punto2D) otro; // downcasting explícito
            double dx = this.x - p2.x;
            double dy = this.y - p2.y;
            return Math.sqrt(dx*dx + dy*dy);
        } else {
            throw new IllegalArgumentException("Se esperaba un Punto2D");
        }
    }
}

class Punto3D extends Punto {
    private double x, y, z;
    public Punto3D(double x, double y, double z) { this.x = x; this.y = y; this.z = z; }
    @Override
    public double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto3D) {
            Punto3D p3 = (Punto3D) otro;
            double dx = this.x - p3.x;
            double dy = this.y - p3.y;
            double dz = this.z - p3.z;
            return Math.sqrt(dx*dx + dy*dy + dz*dz);
        } else {
            throw new IllegalArgumentException("Se esperaba un Punto3D");
        }
    }
}

class Linea {
    private Punto p1, p2;
    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }
    public double longitud() {
        // Polimorfismo: se usará la implementación concreta de calcularDistanciaA
        return p1.calcularDistanciaA(p2);
    }
}
```
La clase `Linea` trabaja con referencias de tipo `Punto`, sin conocer si son 2D o 3D. Al llamar a `longitud()`, se ejecuta dinámicamente el método `calcularDistanciaA` de la subclase real, que internamente usa `instanceof` y downcasting para acceder a las coordenadas específicas. Esto respeta el polimorfismo y permite manejar líneas en diferentes dimensiones.

## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta
La herencia de interfaces en Java se refiere a la capacidad que tiene una interfaz de extender una o más interfaces padre mediante la palabra clave `extends`. Al extender, la subinterfaz hereda todos los métodos abstractos (y también los métodos `default` y `static`) de las interfaces base, y puede añadir nuevos métodos. Esto permite construir jerarquías de contratos, donde una interfaz más específica amplía a una más genérica.

Sí, existe la herencia múltiple de interfaces: una interfaz puede extender varias interfaces a la vez, separadas por comas (`interface C extends A, B`). Esto es legal porque las interfaces solo declaran comportamientos sin estado, y Java resuelve posibles conflictos de métodos `default` mediante reglas explícitas (la subinterfaz debe sobrescribir el método conflictivo o prevalecer la de la interfaz más específica).

Ejemplo:
```java
interface Fichero {
    String leerContenido();
}

interface FicheroEscribible extends Fichero {
    void escribirContenido(String contenido);
    void eliminar();
}
```
Cualquier clase que implemente `FicheroEscribible` deberá proporcionar los métodos `leerContenido`, `escribirContenido` y `eliminar`. La herencia de interfaces facilita el diseño de APIs extensibles y promueve la reutilización de contratos sin los problemas de la herencia múltiple de clases.