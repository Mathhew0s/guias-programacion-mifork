=>
<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

### Respuesta
Un puntero a función es una variable que almacena la dirección de memoria de una función, permitiendo invocar dicha función de forma indirecta a través del puntero. En C, los punteros a función permiten pasar funciones como argumentos a otras funciones, almacenarlas en estructuras de datos o decidir en tiempo de ejecución qué función ejecutar. La sintaxis para declarar un puntero a función incluye el tipo de retorno y los tipos de los parámetros, por ejemplo: `char* (*ptr)(char*)`.

Ejemplo en C:
```c
#include <stdio.h>
#include <ctype.h>
#include <string.h>

char* aMayusculasFuncion(char* cadena) {
    for (int i = 0; cadena[i]; i++) {
        cadena[i] = toupper(cadena[i]);
    }
    return cadena;
}

int main() {
    char* (*aMayusculas)(char*) = aMayusculasFuncion;
    char texto[] = "hola mundo";
    printf("%s\n", aMayusculas(texto));  // Imprime "HOLA MUNDO"
    return 0;
}
```
La variable `aMayusculas` es un puntero a función que apunta a `aMayusculasFuncion` y se invoca exactamente como si fuera la función original.

## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta
Una función lambda (también llamada función anónima) es una función que se define sin nombre, en el lugar donde se va a utilizar, y puede ser asignada a una variable, pasada como argumento o devuelta como resultado. Las lambdas permiten escribir código más conciso y expresivo, especialmente cuando se trabaja con operaciones sobre colecciones o callbacks. A diferencia de los punteros a función tradicionales, las lambdas suelen capturar variables del contexto donde se definen (cierres o closures).

Ejemplo en JavaScript:
```javascript
const aMayusculas = (texto) => texto.toUpperCase();
console.log(aMayusculas("hola mundo"));  // "HOLA MUNDO"
```

Ejemplo en Java:
```java
import java.util.function.Function;

public class EjemploLambda {
    public static void main(String[] args) {
        Function<String, String> aMayusculas = (texto) -> texto.toUpperCase();
        System.out.println(aMayusculas.apply("hola mundo"));  // "HOLA MUNDO"
    }
}
```
En ambos casos, `aMayusculas` es una variable que referencia una función lambda que transforma un `String` en su versión en mayúsculas.

## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta
El paradigma funcional es un estilo de programación que trata la computación como la evaluación de funciones matemáticas, evitando cambios de estado y datos mutables. En este paradigma, las funciones son tratadas como ciudadanos de primera clase, lo que significa que pueden ser asignadas a variables, pasadas como argumentos, devueltas como resultados y almacenadas en estructuras de datos. También se fomenta el uso de funciones puras (sin efectos secundarios) y la inmutabilidad.

Java 8 se considera un lenguaje multi-paradigma porque, además de su naturaleza orientada a objetos, incorporó características funcionales como lambdas, streams y referencias a métodos. Esto permite a los programadores elegir el estilo más adecuado para cada problema. Que las funciones sean "ciudadanos de primera clase" significa que tienen el mismo estatus que otros valores (como enteros o cadenas): se pueden pasar como parámetros, devolver de funciones y asignar a variables, lo que antes en Java no era posible sin usar clases anónimas verbosas. JavaScript siempre ha tenido este concepto, mientras que Java lo adoptó a partir de la versión 8.

## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta
La sintaxis básica de una lambda en Java consta de tres partes: lista de parámetros, el operador flecha `->` y el cuerpo de la función. Si la lambda tiene un solo parámetro, se pueden omitir los paréntesis; si tiene múltiples parámetros o ninguno, los paréntesis son obligatorios. Si el cuerpo consta de una sola expresión, las llaves y la palabra `return` son opcionales (el valor de la expresión se devuelve implícitamente). Si el cuerpo tiene varias sentencias, se deben usar llaves y la sentencia `return` explícita.

Ejemplos de sintaxis:
```java
// Sin parámetros
Runnable r = () -> System.out.println("Hola");

// Un parámetro (paréntesis opcional)
Function<String, String> f1 = s -> s.toUpperCase();

// Múltiples parámetros
BinaryOperator<Integer> suma = (a, b) -> a + b;

// Varias sentencias (con llaves y return)
Function<Integer, Integer> cuadrado = (x) -> {
    int resultado = x * x;
    return resultado;
};
```
El compilador infiere los tipos de los parámetros a partir del contexto (interfaz funcional esperada), por lo que no es necesario declararlos explícitamente, aunque se puede hacer si se desea: `(String s) -> s.length()`.

## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta
Tener la capacidad de pasar funciones como parámetros permite crear métodos de orden superior (higher-order functions) que generalizan comportamientos. El método `transformar` recibe un `String` y una función que define cómo transformarlo, aplicando dicha función al texto recibido. Esto separa la lógica de transformación del flujo general, siguiendo el principio de separación de preocupaciones.

Ejemplo en JavaScript:
```javascript
function transformar(texto, transformador) {
    return transformador(texto);
}

const aMayusculas = (t) => t.toUpperCase();
console.log(transformar("hola mundo", aMayusculas));  // "HOLA MUNDO"
```

Ejemplo en Java:
```java
import java.util.function.Function;

public class TransformadorEjemplo {
    public static String transformar(String texto, Function<String, String> transformador) {
        return transformador.apply(texto);
    }
    
    public static void main(String[] args) {
        Function<String, String> aMayusculas = t -> t.toUpperCase();
        System.out.println(transformar("hola mundo", aMayusculas));  // "HOLA MUNDO"
    }
}
```
En ambos casos, el método `transformar` no sabe qué transformación específica se aplicará; simplemente ejecuta la función recibida. Esto demuestra la potencia de tratar funciones como valores.

## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta
Pasar la lambda directamente en la llamada evita tener que declarar una variable intermedia y hace el código más conciso, especialmente cuando la función es simple y se usa solo una vez. Esta técnica es común en programación funcional y permite definir comportamientos "inline" sin necesidad de nombrarlos.

Ejemplo en JavaScript:
```javascript
console.log(transformar("hola mundo", (t) => t.split('').reverse().join('')));  // "odnum aloh"
```

Ejemplo en Java:
```java
public class TransformadorInline {
    public static String transformar(String texto, Function<String, String> transformador) {
        return transformador.apply(texto);
    }
    
    public static void main(String[] args) {
        // Lambda para invertir la cadena, definida directamente en la llamada
        String resultado = transformar("hola mundo", t -> new StringBuilder(t).reverse().toString());
        System.out.println(resultado);  // "odnum aloh"
    }
}
```
Se observa que en Java se usa `StringBuilder.reverse()` para invertir la cadena, y la lambda se escribe directamente como argumento del método `transformar`. No es necesario nombrar la función; el compilador infiere que se trata de una implementación de `Function<String, String>`.

## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga sea concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta
Un closure (cierre) es una función lambda que captura y recuerda el entorno en el que fue definida, incluyendo variables locales que estaban en su ámbito aunque dichas variables ya no existan cuando la lambda se ejecute. En Java, las lambdas pueden capturar variables locales siempre que sean "efectivamente finales" (es decir, que no se modifiquen después de ser inicializadas). El closure permite que la lambda "cierre sobre" esas variables y las utilice incluso fuera del contexto original.

Ejemplo en Java:
```java
import java.util.function.Function;

public class EjemploClosure {
    public static void main(String[] args) {
        String sufijo = " [procesado]";  // variable local efectivamente final
        Function<String, String> concatenarSufijo = (texto) -> texto + sufijo;
        
        System.out.println(concatenarSufijo.apply("Hola"));  // "Hola [procesado]"
        
        // Modificación del ejemplo de transformar con closure
        String prefijo = "INFO: ";
        String resultado = transformar("mensaje", t -> prefijo + t.toUpperCase());
        System.out.println(resultado);  // "INFO: MENSAJE"
    }
    
    public static String transformar(String texto, Function<String, String> transformador) {
        return transformador.apply(texto);
    }
}
```
La lambda `t -> prefijo + t.toUpperCase()` captura la variable `prefijo` definida fuera de ella. Aunque `prefijo` es local al método `main`, la lambda la retiene y la usa cuando se ejecuta dentro de `transformar`. Esto es un closure.

## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta
La diferencia fundamental radica en la capacidad de capturar el entorno (closures). Un puntero a función en C solo almacena la dirección de memoria de una función predefinida, pero no puede capturar variables del contexto donde se define. No existe en C un mecanismo que permita a un puntero a función "recordar" valores de variables locales del ámbito de creación, a menos que se pasen explícitamente como parámetros adicionales. En cambio, una lambda en lenguajes modernos (Java, JavaScript, Python, etc.) puede acceder a variables del ámbito circundante sin necesidad de pasarlas como argumentos.

Otra diferencia es la sintaxis y la verbosidad: los punteros a función en C requieren declaraciones con tipos complejos y generalmente implican definir la función por separado. Las lambdas son anónimas y se escriben en el lugar de uso, lo que las hace más cómodas para operaciones cortas. Además, las lambdas en Java están tipadas mediante interfaces funcionales, mientras que los punteros en C son simplemente direcciones de memoria. Por último, las lambdas pueden lanzar excepciones chequeadas según el contexto, mientras que en C el manejo de errores es diferente.

## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta
Devolver funciones desde otras funciones (funciones de orden superior) permite crear fábricas de comportamientos parametrizados. La función `crearDescuento` recibe un porcentaje y devuelve una lambda que, dado un precio, aplica ese descuento. La lambda captura el `porcentaje` mediante un closure, de modo que cada función descuento recuerda el porcentaje con el que fue creada.

Ejemplo en Java:
```java
import java.util.function.Function;

public class FabricaDescuentos {
    public static Function<Double, Double> crearDescuento(double porcentaje) {
        // La lambda captura 'porcentaje' (closure)
        return precio -> precio * (1 - porcentaje / 100.0);
    }
    
    public static void main(String[] args) {
        Function<Double, Double> descuento10 = crearDescuento(10);
        Function<Double, Double> descuento25 = crearDescuento(25);
        
        double precioOriginal = 100.0;
        System.out.println(descuento10.apply(precioOriginal));  // 90.0
        System.out.println(descuento25.apply(precioOriginal));  // 75.0
    }
}
```
El closure ocurre porque la lambda `precio -> precio * (1 - porcentaje / 100.0)` captura la variable `porcentaje` del parámetro de `crearDescuento`. Cada vez que se invoca `crearDescuento` con un valor distinto, se crea una nueva lambda que "recuerda" ese valor. Sin closures, no sería posible que la función descuento supiera qué porcentaje aplicar a menos que se lo pasáramos como parámetro adicional.

## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta
Una interfaz funcional en Java es una interfaz que contiene exactamente un método abstracto (sin implementar). Puede contener múltiples métodos `default` o `static`, pero solo un método abstracto. Este único método abstracto define la "forma" que debe tener la lambda que se asigna a esa interfaz. Las interfaces funcionales pueden anotarse opcionalmente con `@FunctionalInterface`, que hace que el compilador verifique que cumpla el requisito de un solo método abstracto y genere un error si no es así.

El requisito principal es tener un único método abstracto. Los métodos heredados de `Object` (como `toString`, `equals`, `hashCode`) no cuentan como métodos abstractos a efectos de esta regla, porque toda clase los tiene implícitamente. Por ejemplo, `Runnable` es una interfaz funcional porque solo tiene el método abstracto `run()`. `Comparator<T>` también lo es porque tiene un único método abstracto (`compare`), aunque tiene muchos métodos `default` y `static`. Una lambda en Java debe ser compatible con la interfaz funcional esperada en el contexto: la lista de parámetros y el tipo de retorno de la lambda deben coincidir con los del método abstracto.

## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta
Definir una interfaz funcional manualmente es sencillo y útil para entender cómo funcionan las lambdas detrás de escena. La interfaz `Transformador` declara un único método abstracto `transformar` que recibe un `String` y devuelve otro `String`. Al marcar la interfaz con `@FunctionalInterface`, se asegura que no se añada accidentalmente otro método abstracto.

```java
@FunctionalInterface
public interface Transformador {
    String transformar(String texto);
}

// Uso de la interfaz funcional creada manualmente
public class EjemploTransformadorManual {
    public static void main(String[] args) {
        Transformador aMayusculas = texto -> texto.toUpperCase();
        Transformador invertir = texto -> new StringBuilder(texto).reverse().toString();
        
        System.out.println(aMayusculas.transformar("hola"));  // "HOLA"
        System.out.println(invertir.transformar("hola"));     // "aloh"
    }
}
```
Aunque se podría haber usado `Function<String, String>` de la biblioteca estándar, crear una interfaz personalizada puede ser útil para dar nombres semánticos más claros en dominios específicos. La lambda `texto -> texto.toUpperCase()` es compatible porque coincide con el método `transformar`: recibe un `String` y devuelve un `String`.

## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta
Hacer la interfaz genérica permite reutilizar el mismo concepto para cualquier par de tipos de entrada y salida. La interfaz `Transformador<T, R>` declara un método `transformar` que recibe un `T` y devuelve una `R`. Esto es exactamente lo mismo que la interfaz `Function<T, R>` de Java, pero creada manualmente para ilustrar el concepto.

```java
@FunctionalInterface
public interface Transformador<T, R> {
    R transformar(T entrada);
}

// Ejemplo de uso: redondear un Double a Integer
public class EjemploTransformadorGenerico {
    public static void main(String[] args) {
        Transformador<Double, Integer> redondear = d -> (int) Math.round(d);
        
        Double valor = 3.67;
        Integer resultado = redondear.transformar(valor);
        System.out.println(resultado);  // 4
    }
}
```
El transformador `redondear` recibe un `Double` y devuelve un `Integer`. La lambda `d -> (int) Math.round(d)` es compatible porque el parámetro de entrada es `Double` y el cuerpo devuelve `Integer` (con el casting explícito). Gracias a los genéricos, `Transformador` puede usarse para cualquier conversión de tipos, como `String` a `Integer`, `LocalDate` a `String`, etc.

## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta
Java 8 introdujo el paquete `java.util.function` con una colección de interfaces funcionales predefinidas para cubrir los casos de uso más comunes. Esto evita que los programadores tengan que definir sus propias interfaces cada vez. Las principales son:

- **`Function<T, R>`**: Recibe un argumento de tipo `T` y devuelve un resultado de tipo `R`. (Equivalente a `Transformador<T,R>`).
- **`Predicate<T>`**: Recibe un argumento de tipo `T` y devuelve un `boolean`. Se usa para filtros.
- **`Consumer<T>`**: Recibe un argumento de tipo `T` y no devuelve resultado (`void`). Se usa para acciones con efectos secundarios.
- **`Supplier<T>`**: No recibe argumentos y devuelve un resultado de tipo `T`. Se usa para generadores o fábricas.
- **`UnaryOperator<T>`**: Subinterfaz de `Function<T, T>` donde entrada y salida son del mismo tipo.
- **`BinaryOperator<T>`**: Extiende `BiFunction<T,T,T>`, recibe dos argumentos del mismo tipo y devuelve un resultado de ese tipo.
- **`BiFunction<T, U, R>`**: Recibe dos argumentos de tipos `T` y `U`, devuelve `R`.

También existen versiones especializadas para tipos primitivos (`IntFunction`, `DoublePredicate`, `LongConsumer`, etc.) que evitan el autoboxing. Estas interfaces forman la base de la programación funcional en Java.

## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta
El método `forEach` de la interfaz `Iterable` (y por tanto de `List`) recibe un `Consumer<? super T>` y aplica ese consumidor a cada elemento de la colección. Es una alternativa funcional al bucle `for` tradicional que separa el "qué hacer" (la acción) del "cómo recorrer" (la iteración). El código resulta más declarativo y a menudo más legible, especialmente cuando la acción es simple.

Ejemplo:
```java
import java.util.Arrays;
import java.util.List;

public class EjemploForEach {
    public static void main(String[] args) {
        List<Integer> numeros = Arrays.asList(-5, 3, 0, 10, -2, 7);
        
        numeros.forEach(n -> {
            if (n > 0) {
                System.out.println(n + " es positivo");
            }
        });
    }
}
```
Salida:
```
3 es positivo
10 es positivo
7 es positivo
```
La lambda `n -> { if (n > 0) System.out.println(...); }` actúa como un `Consumer<Integer>` que consume cada entero. Comparado con un bucle `for (int n : numeros) { ... }`, el `forEach` enfatiza la operación sobre el elemento más que el mecanismo de iteración, y permite cambiar fácilmente la estrategia de iteración (por ejemplo, a paralelo con `parallelStream().forEach`).

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta
La firma `void forEach(Consumer<? super T> action)` usa `? super T` en lugar de `Consumer<T>` para permitir mayor flexibilidad. Si se usara `Consumer<T>`, solo se podría pasar un consumidor que acepte exactamente objetos de tipo `T`. Con `Consumer<? super T>`, se puede pasar un consumidor que acepte cualquier supertipo de `T` (por ejemplo, `Consumer<Object>` para una lista de `Integer`). Esto es útil porque un consumidor que sabe manejar `Object` también puede manejar `Integer` (principio de sustitución de Liskov). El wildcard `? super T` hace que el tipo sea **contravariante** para el parámetro de entrada del consumidor.

**PECS** es un acrónimo que significa **Producer Extends, Consumer Super**. Es una regla para elegir wildcards en genéricos:
- Si un tipo paramétrico **produce** elementos (los entrega), se usa `? extends T`. Permite leer como `T` pero no escribir.
- Si un tipo paramétrico **consume** elementos (los recibe), se usa `? super T`. Permite escribir elementos de tipo `T` pero leer solo como `Object`.

Aplicando PECS al método `transformar`: originalmente se definía como `transformar(String texto, Function<String, String> f)`. Si se quisiera más generalidad, el transformador debe **consumir** un `String` y **producir** un `String`. Por tanto, el transformador es un productor de resultados, y según PECS debería usarse `? extends String` en el tipo de retorno del transformador, pero la interfaz `Function<T, R>` ya tiene el productor en `R`. Para el parámetro de entrada del transformador, si se quisiera consumir supertipos, se usaría `? super String`. Así, una firma más flexible sería:
```java
public static <T, R> R transformar(T entrada, Function<? super T, ? extends R> transformador)
```
Esto permite pasar un `Function<Object, String>` donde se espera `Function<String, String>` (consumidor más general) y permite que el retorno sea de un subtipo de `R`.

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta
Las referencias a métodos (method references) son una forma abreviada de escribir lambdas cuando la lambda simplemente llama a un método existente. Permiten un código más conciso y legible, especialmente cuando se trabaja con APIs funcionales. Tanto en JavaScript como en Java, se puede obtener una referencia a un método de un objeto concreto, aunque la sintaxis difiere entre lenguajes.

Ejemplo en JavaScript:
```javascript
class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }
    saludar() {
        console.log(`Hola, soy ${this.nombre}`);
    }
}

const persona = new Persona("Ana");
const referenciaSaludo = persona.saludar.bind(persona);  // bind necesario para preservar 'this'
referenciaSaludo();  // "Hola, soy Ana"
```

Ejemplo en Java:
```java
import java.util.function.Supplier;

class Persona {
    private String nombre;
    public Persona(String nombre) { this.nombre = nombre; }
    public void saludar() { System.out.println("Hola, soy " + nombre); }
}

public class ReferenciaMetodo {
    public static void main(String[] args) {
        Persona persona = new Persona("Ana");
        Runnable referenciaSaludo = persona::saludar;  // referencia a método de instancia
        referenciaSaludo.run();  // "Hola, soy Ana"
    }
}
```
En Java, `persona::saludar` crea una referencia al método `saludar` del objeto `persona` concreto, y se asigna a `Runnable` porque `saludar` tiene firma `void()` (sin parámetros). Al invocar `run()`, se ejecuta el método en el objeto `persona`. No se necesita `bind` porque la referencia ya está vinculada a la instancia.

## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta
Java soporta cuatro tipos de referencias a métodos:

1. **Referencia a método estático**: `Clase::metodoEstatico`
2. **Referencia a método de instancia sobre una instancia concreta**: `objeto::metodoInstancia`
3. **Referencia a método de instancia sobre cualquier instancia** (tipo contenedor): `Clase::metodoInstancia`
4. **Referencia a constructor**: `Clase::new`

Ejemplos completos:
```java
import java.util.function.Function;
import java.util.function.Supplier;
import java.util.Arrays;
import java.util.List;

public class TiposReferenciasMetodo {
    // 1. Método estático
    public static String aMayusculas(String s) { return s.toUpperCase(); }
    
    // 2. Método de instancia (para cualquier instancia)
    public String anexarPunto(String s) { return s + "."; }
    
    static class Persona {
        String nombre;
        Persona(String nombre) { this.nombre = nombre; }
        public String getNombre() { return nombre; }
    }
    
    public static void main(String[] args) {
        // 1. Referencia a método estático
        Function<String, String> mayus = TiposReferenciasMetodo::aMayusculas;
        
        // 2. Referencia a método de instancia sobre instancia concreta
        TiposReferenciasMetodo obj = new TiposReferenciasMetodo();
        Function<String, String> conPunto = obj::anexarPunto;
        
        // 3. Referencia a método de instancia sobre cualquier instancia
        // (el primer parámetro se convierte en el receptor)
        Function<Persona, String> obtenNombre = Persona::getNombre;
        
        // 4. Referencia a constructor
        Supplier<Persona> creaPersonaVacia = Persona::new; // asume constructor sin args
        Function<String, Persona> creaPersonaConNombre = Persona::new; // constructor con String
        
        List<String> nombres = Arrays.asList("Ana", "Luis");
        nombres.stream().map(Persona::new).forEach(p -> System.out.println(p.getNombre()));
    }
}
```
Cada tipo de referencia se convierte en una lambda específica. La referencia a método estático y a constructor son las más directas. La referencia a método de instancia sobre cualquier instancia (tipo `Clase::metodo`) es particularmente útil en streams, donde el primer parámetro de la lambda se convierte en el receptor del método.

## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta
Ordenar listas con lambdas y con los métodos de `Comparator` muestra la expresividad del estilo funcional. La primera versión implementa manualmente la comparación con una lambda que devuelve un entero siguiendo el contrato de `Comparator`. La segunda versión usa los métodos estáticos y por defecto de `Comparator` (`comparingInt`, `thenComparing`), que son más declarativos y menos propensos a errores.

Versión 1 (lambda manual):
```java
import java.util.*;

class Persona {
    String nombre;
    int edad;
    Persona(String nombre, int edad) { this.nombre = nombre; this.edad = edad; }
    @Override public String toString() { return nombre + "(" + edad + ")"; }
}

public class OrdenacionPersonas {
    public static void main(String[] args) {
        List<Persona> personas = Arrays.asList(
            new Persona("Ana", 25),
            new Persona("Luis", 30),
            new Persona("Ana", 20),
            new Persona("Carlos", 25)
        );
        
        // Versión 1: lambda manual
        Collections.sort(personas, (p1, p2) -> {
            int porEdad = Integer.compare(p1.edad, p2.edad);
            if (porEdad != 0) return porEdad;
            return p1.nombre.compareTo(p2.nombre);
        });
        System.out.println(personas);  // [Ana(20), Ana(25), Carlos(25), Luis(30)]
        
        // Versión 2: usando Comparator
        List<Persona> personas2 = Arrays.asList(
            new Persona("Ana", 25),
            new Persona("Luis", 30),
            new Persona("Ana", 20),
            new Persona("Carlos", 25)
        );
        personas2.sort(Comparator.comparingInt((Persona p) -> p.edad)
                                 .thenComparing(p -> p.nombre));
        System.out.println(personas2);  // mismo resultado
    }
}
```
La versión con `Comparator` es más legible porque expresa la intención ("comparar por edad y luego por nombre") en lugar de los detalles de implementación de la comparación. Además, `comparingInt` evita el autoboxing y `thenComparing` encadena comparadores de forma natural. Esta segunda versión se considera más idiomática en Java moderno.