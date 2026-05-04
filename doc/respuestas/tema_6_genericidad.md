<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

### Respuesta
En C, se puede usar un array de `void*` (punteros genéricos) para almacenar referencias a cualquier tipo de dato. La desventaja es que se pierde la información de tipo y el programador debe gestionar manualmente el casting y la memoria. Por ejemplo:
```c
void* almacen[10];
int a = 5;
double b = 3.14;
almacen[0] = &a;
almacen[1] = &b;
// Para recuperar: int *pa = (int*)almacen[0];
```

En Java, se puede usar un array de `Object`, ya que todas las clases heredan de `Object`. Esto permite alojar cualquier objeto, pero también requiere downcasting explícito al recuperar los elementos:
```java
Object[] almacen = new Object[10];
almacen[0] = "Hola";
almacen[1] = 42;       // autoboxing de Integer
String s = (String) almacen[0];
```
Ambos enfoques son formas primitivas de lograr cierta genericidad, pero sin seguridad de tipos en tiempo de compilación.

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica?

### Respuesta
La programación genérica es un paradigma que permite escribir código (clases, interfaces o métodos) parametrizado por tipos, de modo que una misma implementación pueda funcionar con diferentes tipos de datos respetando la seguridad de tipos en tiempo de compilación. Su objetivo es aumentar la reutilización del código sin sacrificar el chequeo estático de tipos, evitando así los errores que pueden surgir al hacer casting explícito.

El ejemplo anterior (usar `void*` o `Object`) es un precursor o una forma básica y no segura de programación genérica. Aunque permite alojar cualquier tipo, no proporciona seguridad de tipos: el compilador no puede detectar si se está extrayendo un valor con un tipo incorrecto, lo que puede provocar fallos en tiempo de ejecución (como una `ClassCastException` en Java o comportamiento indefinido en C). Por tanto, es programación genérica en un sentido muy primitivo, carente de las ventajas que ofrecen los mecanismos modernos como los *generics* de Java o las *plantillas* de C++.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas.

### Respuesta
El principal problema es la pérdida total de información de tipo en tiempo de compilación. Al usar `Object` (o `void*` en C), cualquier tipo de objeto puede ser almacenado sin que el compilador verifique la consistencia. Esto significa que se puede introducir por error un tipo incorrecto en la estructura y el compilador no lo detectará. Al recuperar los elementos, el programador debe realizar un downcasting explícito al tipo esperado, y si el objeto almacenado no es de ese tipo, se produce una `ClassCastException` en tiempo de ejecución (o un comportamiento indefinido/crash en C).

Además, el código se vuelve más verboso y propenso a errores, ya que cada extracción requiere un casting que oscurece la lógica de negocio. No se puede aprovechar el sistema de tipos para expresar restricciones, como "esta lista solo debe contener cadenas". Esto va en contra de los principios de la orientación a objetos moderna, que buscan detectar errores lo antes posible (idealmente en compilación) y hacer el código más autodocumentado.

## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**?

### Respuesta
Los parámetros de tipo (también llamados variables de tipo) son identificadores que actúan como "marcadores" o "comodines" para un tipo concreto que se especificará en el momento de usar una clase, interfaz o método genérico. Se declaran entre los símbolos `<` y `>`, justo después del nombre de la clase o del método. Por ejemplo, en `class Caja<T>`, la `T` es un parámetro de tipo que representa un tipo desconocido en el momento de definir la clase. Cuando se instancia la clase (por ejemplo, `Caja<String>`), ese parámetro se sustituye por un tipo concreto (`String`).

Los parámetros de tipo permiten que una misma clase trabaje con diferentes tipos manteniendo la seguridad de tipos. El compilador verifica que todas las operaciones sean compatibles con el tipo que se suministre en cada instanciación. Convencionalmente se usan nombres de una sola letra mayúscula: `E` para elementos, `K` para claves, `V` para valores, `T` para tipo, etc. Gracias a los parámetros de tipo, no es necesario hacer casting al extraer elementos de una estructura genérica, ya que el compilador conoce el tipo concreto.

## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta
En Java, usando `ArrayList`:
```java
import java.util.ArrayList;
import java.util.List;

public class EjemploJava {
    public static void main(String[] args) {
        List<String> lista = new ArrayList<>();
        lista.add("Hola");
        lista.add("Mundo");
        // lista.add(42); // Error de compilación
        
        for (String s : lista) {
            System.out.println(s.toUpperCase()); // sin casting
        }
    }
}
```

En C++, usando `std::vector`:
```cpp
#include <iostream>
#include <vector>
#include <string>

int main() {
    std::vector<std::string> lista;
    lista.push_back("Hola");
    lista.push_back("Mundo");
    // lista.push_back(42); // Error de compilación
    
    for (const std::string& s : lista) {
        std::cout << s << std::endl;
    }
    return 0;
}
```
En ambos lenguajes, al declarar la estructura con el parámetro de tipo `String`, el compilador garantiza que solo se puedan añadir cadenas, y al extraer elementos no se necesita casting explícito porque el tipo devuelto ya es `String`.

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta
El tratamiento es radicalmente diferente. En C++, cada instanciación de una plantilla con tipos distintos genera código completamente independiente mediante un proceso llamado **instanciación de plantillas**. El compilador toma la plantilla, sustituye los parámetros de tipo por los tipos concretos proporcionados y compila ese código como si se hubiera escrito manualmente para cada combinación de tipos. Esto produce múltiples versiones del mismo código, lo que puede aumentar el tamaño del binario pero ofrece máximo rendimiento y posibilidades avanzadas (como especialización).

En Java, el compilador aplica **type erasure (borrado de tipos)**. Cuando se compila código con genéricos, el compilador verifica la corrección de tipos y luego elimina toda la información genérica, reemplazando los parámetros de tipo por su límite superior (generalmente `Object` si no hay restricción) e inserta los casting necesarios. En tiempo de ejecución, un `ArrayList<String>` y un `ArrayList<Integer>` son ambos `ArrayList` sin información sobre el parámetro de tipo. Esto fue diseñado para mantener compatibilidad con versiones anteriores de Java (pre-generics). No son lo mismo: C++ genera código múltiple, Java usa un único código con borrado.

## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`.

### Respuesta
```java
public class Par<T, U> {
    private T primero;
    private U segundo;
    
    public Par(T primero, U segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }
    
    public T getPrimero() { return primero; }
    public U getSegundo() { return segundo; }
}

// Ejemplo de uso
public class Estadisticas {
    public static Par<Double, Double> calcularMediaYDesviacion(double[] datos) {
        double suma = 0.0;
        for (double d : datos) suma += d;
        double media = suma / datos.length;
        
        double sumaCuadrados = 0.0;
        for (double d : datos) sumaCuadrados += Math.pow(d - media, 2);
        double desviacion = Math.sqrt(sumaCuadrados / datos.length);
        
        return new Par<>(media, desviacion);
    }
    
    public static void main(String[] args) {
        double[] valores = {2.0, 4.0, 6.0, 8.0};
        Par<Double, Double> resultado = calcularMediaYDesviacion(valores);
        System.out.println("Media: " + resultado.getPrimero());
        System.out.println("Desviación: " + resultado.getSegundo());
    }
}
```
La clase `Par` permite almacenar dos valores de tipos potencialmente diferentes. La función devuelve un `Par<Double, Double>`, pero se podrían usar tipos distintos según necesidad.

## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo.

### Respuesta
```java
import java.util.Random;

public class EjemploMetodoGenerico {
    private static Random rand = new Random();
    
    // Versión sin genéricos (con Object)
    public static Object seleccionaUnoObject(Object a, Object b) {
        return rand.nextBoolean() ? a : b;
    }
    
    // Versión genérica
    public static <T> T seleccionaUnoGenerico(T a, T b) {
        return rand.nextBoolean() ? a : b;
    }
    
    public static void main(String[] args) {
        String s1 = "Hola";
        String s2 = "Mundo";
        
        // Con Object: necesita downcasting y permite mezclar tipos
        Object obj = seleccionaUnoObject(s1, s2);
        String resultadoObj = (String) obj; // downcasting explícito OBLIGATORIO
        // seleccionaUnoObject("Hola", 42); // COMPILA aunque sean tipos distintos!
        
        // Con genérico: sin downcasting y tipos forzados a ser iguales
        String resultadoGen = seleccionaUnoGenerico(s1, s2); // sin casting
        // seleccionaUnoGenerico("Hola", 42); // ERROR de compilación
    }
}
```
La diferencia clave: (i) con genéricos no se necesita downcasting porque el tipo de retorno es `T` (que se infiere como `String`), (ii) el compilador exige que ambos argumentos sean del mismo tipo `T`, lo que evita mezclar tipos incompatibles accidentalmente. La versión con `Object` permite mezclar tipos y obliga al programador a recordar qué tipo devolvió.

## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta
Sí, se pueden establecer restricciones usando la palabra clave `extends`. Por ejemplo, `<T extends Number>` significa que `T` debe ser `Number` o una subclase suya (`Integer`, `Double`, etc.). Así el compilador sabe que cualquier objeto de tipo `T` tiene los métodos de `Number` (como `doubleValue()`).

Solución 1 (sin generics, usando `Number`):
```java
public class PuntoNumber {
    private Number x, y;
    public PuntoNumber(Number x, Number y) { this.x = x; this.y = y; }
    public Number getX() { return x; }
    public Number getY() { return y; }
    public double calcularDistanciaA(PuntoNumber otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx*dx + dy*dy);
    }
}
```

Solución 2 (con generics):
```java
public class PuntoGenerico<T extends Number> {
    private T x, y;
    public PuntoGenerico(T x, T y) { this.x = x; this.y = y; }
    public T getX() { return x; }
    public T getY() { return y; }
    public double calcularDistanciaA(PuntoGenerico<T> otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx*dx + dy*dy);
    }
}
```
Respecto al type erasure en la solución con genéricos, el compilador reemplaza `T` por su límite superior, que es `Number`. Tras la compilación, `PuntoGenerico` se convierte en `PuntoGenerico` con `Number` en lugar de `T`, pero los casting se insertan automáticamente.

## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta
Con la solución de `Number`, sí se puede crear un punto con coordenadas de tipos diferentes, por ejemplo `new PuntoNumber(5, 3.14)` donde `5` es `Integer` y `3.14` es `Double`. Ambos son subtipos de `Number`, por lo que es válido. En este caso, `getX()` devuelve `Number`, un tipo abstracto, y el programador necesitaría hacer downcasting o llamar a métodos como `doubleValue()` para operar con el valor concreto.

En la solución con genéricos (`PuntoGenerico<T extends Number>`), NO se puede mezclar tipos distintos porque el parámetro `T` es único para ambas coordenadas. Así, `new PuntoGenerico<Integer>(5, 3.14)` no compila porque el segundo argumento es `Double` y se espera `Integer`. Esto refuerza el chequeo de tipos: o ambas son `Integer`, o ambas `Double`, etc. `getX()` devuelve exactamente el tipo concreto `T` (p.ej., `Integer` o `Double`), no `Number`. Por tanto, la versión con genéricos proporciona más información de tipo y evita casting, pero impone uniformidad en las coordenadas. Cada enfoque tiene sus ventajas según el caso de uso.

## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ... 
} 
```

### Respuesta
Para eliminar `instanceof` y downcasting, se puede usar un tipo paramétrico en la interfaz, indicando que cada punto concreto solo debe calcular distancia con otros puntos del mismo subtipo. Esto se logra mediante el patrón **CRTP (Curiously Recurring Template Pattern)** o su equivalente en Java con genéricos: la interfaz recibe como parámetro el subtipo concreto.

```java
public interface Punto<T extends Punto<T>> {
    double distanciaA(T p);
}

public class Punto2D implements Punto<Punto2D> {
    private final double x, y;
    public Punto2D(double x, double y) { this.x = x; this.y = y; }
    
    @Override
    public double distanciaA(Punto2D p2d) {
        return Math.sqrt(Math.pow(x - p2d.x, 2) + Math.pow(y - p2d.y, 2));
    }
}

public class Punto3D implements Punto<Punto3D> {
    private final double x, y, z;
    public Punto3D(double x, double y, double z) { this.x = x; this.y = y; this.z = z; }
    
    @Override
    public double distanciaA(Punto3D p3d) {
        return Math.sqrt(Math.pow(x - p3d.x, 2) + Math.pow(y - p3d.y, 2) + Math.pow(z - p3d.z, 2));
    }
}
```
Ahora cada clase implementa `Punto<MismaClase>`, por lo que el parámetro del método `distanciaA` es exactamente el tipo concreto. Ya no se necesita `instanceof` ni downcasting, y el compilador garantiza que no se pase un `Punto3D` a un `Punto2D`. Esto eleva la seguridad a tiempo de compilación.

## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta
En Java, `String[]` es subtipo de `Object[]` (covariancia en arrays). Esto significa que se puede asignar un array de `String` a una variable de tipo `Object[]`. Sin embargo, esto puede generar un `ArrayStoreException` en tiempo de ejecución si se intenta almacenar un elemento de tipo incorrecto (por ejemplo, `objArray[0] = new Integer(42)` cuando `objArray` realmente apunta a un `String[]`). Los arrays tienen "memoria" de su tipo componente en tiempo de ejecución y verifican cada inserción.

En cambio, `List<String>` NO es subtipo de `List<Object>`; los genéricos en Java son **invariantes**. Aunque `String` herede de `Object`, `List<String>` y `List<Object>` son tipos distintos y no compatibles. Esto evita el problema de los arrays: si se permitiera, se podría añadir un `Integer` a un `List<Object>` que realmente fuese un `List<String>`. Como los genéricos sufren type erasure, no pueden comprobar el tipo en tiempo de ejecución, por lo que la prohibición en compilación es necesaria para garantizar la seguridad.

Definiciones:
- **Covariante**: `A<T2>` es subtipo de `A<T1>` si `T2` es subtipo de `T1` (arrays en Java).
- **Contravariante**: `A<T2>` es subtipo de `A<T1>` si `T1` es subtipo de `T2` (parámetros de método en ciertos contextos).
- **Invariante**: `A<T1>` y `A<T2>` no tienen relación de subtipado aunque `T1` y `T2` la tengan (los genéricos básicos de Java).

## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta
Un wildcard (`?`) es un símbolo que representa un "tipo desconocido" en el uso de tipos genéricos. Permite expresar relaciones de subtipado más flexibles que la invariancia por defecto. `List<? extends T>` significa "una lista de algún tipo que es subtipo de `T`" (incluyendo `T` mismo). Esto permite leer elementos como `T`, pero no se pueden añadir (excepto `null`). Se usa cuando se necesita extraer datos de una estructura. `List<? super T>` significa "una lista de algún tipo que es supertipo de `T`" (incluyendo `T`). Permite añadir elementos de tipo `T` (o subtipos), pero lo que se lee es de tipo `Object`. Se usa cuando se necesita insertar datos en una estructura.

Ejemplo (i) con `? extends`:
```java
public static double suma(List<? extends Number> lista) {
    double total = 0.0;
    for (Number n : lista) {
        total += n.doubleValue();
    }
    return total;
}
// Puede llamarse con List<Integer>, List<Double>, etc.
```

Ejemplo (ii) con `? super`:
```java
public static void agregarEnteros(List<? super Integer> lista, int cantidad) {
    for (int i = 0; i < cantidad; i++) {
        lista.add(i); // Integer es subtipo de ? super Integer
    }
}
// Puede llamarse con List<Integer>, List<Number>, List<Object>
```
El wildcard `? extends` permite covariancia (leer), mientras que `? super` permite contravariancia (escribir). Esta es la regla PECS (*Producer Extends, Consumer Super*).