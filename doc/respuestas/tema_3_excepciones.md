 
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

En C, como no existen excepciones, el control de errores debe realizarse mediante mecanismos manuales. Una opción es devolver un valor especial que indique que se produjo un error. Por ejemplo, si la raíz debe devolver un double, se puede usar un valor imposible o distintivo, como -1.0, y documentar que dicho valor representa un error. El código llamador debe comprobarlo explícitamente antes de utilizar el resultado.
 
Otra opción habitual en C es devolver un código de error mediante un parámetro adicional o una variable global como errno. En este caso, la función devuelve el resultado válido por un parámetro y un entero que indica si hubo o no error. Esto separa el valor calculado del indicador de fallo, permitiendo un control más claro desde fuera.

Ejemplo 1 (valor especial):

```java
double raiz(double x) {
    if (x < 0){
         return -1.0
    }
    else{
        return sqrt(x);
    }
}
```

Ejemplo 2 (parámetro de error):

```java
double raiz(double x, int* error) {
    if (x < 0) {
        *error = 1;
        return 0.0;
    }
    *error = 0;
    return sqrt(x);
}
```

---

## 2. Brevemente ¿Qué es una "excepción"? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

Una excepción es un mecanismo que permite interrumpir el flujo normal de un programa cuando ocurre un error o situación inesperada. En lugar de devolver valores especiales, se lanza un evento que puede ser manejado por un bloque específico del código. Esto separa la lógica principal de la gestión de errores.

Los programadores utilizan excepciones para señalar fallos sin complicar la interfaz de las funciones. Desde el punto de vista del código llamador, las excepciones permiten detectar errores sin necesidad de comprobar constantemente códigos de retorno, lo que simplifica la estructura general del programa.

---

## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

En Java, las excepciones permiten indicar errores como pasar un número negativo donde no tiene sentido. En una clase Calculadora, el método puede comprobar si el argumento es válido y, si no lo es, lanzar una excepción. Esto permite que el error sea capturado desde el método main u otro llamador.

El código de control se realiza mediante un bloque try-catch que captura la excepción lanzada por el método raíz. Esto separa la lógica de cálculo del tratamiento del error, haciendo el código más claro y modular.

```java
class Calculadora {
    static double raiz(double x) {
        if (x < 0) throw new IllegalArgumentException("Número negativo");
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        try {
            double r = Calculadora.raiz(-5);
            System.out.println("Resultado: " + r);
        } catch (IllegalArgumentException e) {
            System.out.println("Error detectado: " + e.getMessage());
        }
    }
}
```

---

## 4. ¿Qué es "lanzar" una excepción? ¿Qué es "controlar" o "capturar" una excepción? ¿Qué es que se "propague" una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

Lanzar una excepción significa generar un evento de error mediante la palabra clave throw. Cuando se lanza, el flujo normal se interrumpe y el programa busca un bloque capaz de manejarla. Controlar o capturar una excepción significa atraparla mediante un bloque catch y ejecutar un código específico para gestionar el error.

Si una excepción no se captura en una función, esta se propaga hacia arriba en la pila de llamadas hasta encontrar un manejador adecuado. Las funciones intermedias no se reanudan tras lanzarse la excepción, sino que finalizan abruptamente sin completar su ejecución. Esto permite salir rápidamente de un estado erróneo.

En el ejemplo de la raíz cuadrada, si se llama raíz(-5), se lanza IllegalArgumentException. Si el main tiene un catch adecuado, la excepción se captura allí. Si no existiera ese catch, la excepción seguiría propagándose hasta llegar al sistema de ejecución, finalizando el programa.

---

## 5. ¿Qué ventajas tiene frente a C, la "propagación natural" de las excepciones a través de la pila (stack) de llamadas?

La propagación automática de excepciones evita que cada función tenga que comprobar manualmente el retorno de la función anterior, como en C. Esto reduce la complejidad del código y la cantidad de comprobaciones redundantes, haciendo que el flujo sea más lineal y legible.

Otra ventaja importante es que en C es fácil olvidar controlar un error, mientras que en Java, si una excepción controlada no se captura o no se declara en throws, el compilador obliga a gestionarla. Esto hace que el tratamiento de errores sea más seguro y menos propenso a fallos.

---

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

En la mayoría de lenguajes orientados a objetos, una excepción es un objeto. Esto permite encapsular información relevante sobre el error, como mensajes, causas internas o datos adicionales relacionados con la situación en la que ocurrió.

El hecho de que sean objetos permite definir excepciones personalizadas que extiendan las clases existentes. De esta forma, se pueden crear excepciones que representen errores específicos del dominio del problema, mejorando la claridad y el control del programa.

---

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué información esencial lleva cualquier objeto excepción que es muy útil tener cuando se llega a un manejador?

Una excepción en Java contiene un mensaje descriptivo que ayuda a entender qué ha ocurrido. También mantiene información sobre la pila de llamadas en el momento del error, lo que permite rastrear fácilmente en qué parte del programa se originó.

Además, algunas excepciones pueden contener otras excepciones como causa interna. Esto facilita comprender errores complejos que se originan en niveles más bajos del código, lo que resulta mucho más informativo que un simple código numérico o valor de retorno como en C.

---

## 8. En Java, sobre el bloque "try-catch", ¿se pueden tener más de un bloque catch? ¿cuántos bloques catch se ejecutan?

En Java es posible tener varios bloques catch consecutivos para manejar distintos tipos de excepciones. Esto permite un tratamiento más específico, asignando un comportamiento particular a cada tipo de error esperado.

Cuando se lanza una excepción, se ejecuta únicamente el primer bloque catch cuya clase coincida con la excepción lanzada. No se ejecutan los demás, ya que el error queda manejado en ese punto.

---

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con finally, tanto con catch como sin él.

El bloque finally permite ejecutar código que debe realizarse tanto si ocurre una excepción como si no. Esto es útil para liberar recursos, cerrar archivos o desconectar conexiones sin depender del flujo normal del programa.

El bloque finally se ejecuta siempre, incluso si la excepción no es capturada. Esto garantiza que los recursos se manejen correctamente, independientemente del éxito o fallo del proceso.

Ejemplo con catch:

```java
try {
    int x = Integer.parseInt("abc");
} catch (NumberFormatException e) {
    System.out.println("Error de formato");
} finally {
    System.out.println("Liberando recursos");
}
```

Ejemplo sin catch:

```java
try {
    int x = Integer.parseInt("abc");
} finally {
    System.out.println("Esto también se ejecuta");
}
```

---

## 10. En Java, el bloque finally puede ir sin catch? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un return en medio del try?

El bloque finally puede aparecer sin un bloque catch. Esto permite realizar tareas finales sin necesidad de capturar la excepción en el mismo sitio donde se produce.

El código de finally se ejecuta siempre, tanto si ocurre como si no ocurre una excepción. Incluso si hay un return dentro del try, el bloque finally se ejecuta antes de que la función devuelva el valor. Esta garantía lo hace adecuado para liberar recursos independientemente del flujo del programa.

---

## 11. En Java, qué son las excepciones "controladas" y las "no controladas"? ¿Qué papel juega RuntimeException? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

Las excepciones controladas son aquellas que el compilador exige capturar o declarar en throws. Representan errores esperables, como problemas de entrada/salida o ausencia de archivos. Las no controladas derivan de RuntimeException y representan errores de programación, como índices fuera de rango o argumentos inválidos.

RuntimeException agrupa errores que suelen indicar fallos lógicos. Como no requieren ser capturadas explícitamente, su uso señala que el problema debe resolverse corrigiendo el código llamador y no simplemente recuperándose de él.

Situaciones para excepciones controladas:
1. Archivo no encontrado  
2. Error de lectura/escritura  
3. Conexión de red fallida  
4. Acceso denegado

Situaciones para excepciones no controladas:
1. Divisiones por cero  
2. Argumentos inválidos en un método  
3. Acceso fuera de rango a un array  
4. Violación de invariantes internas

---

## 12. ¿Qué es y para qué se usa throws? ¿Por qué es alternativa a capturar una excepción controlada?

La cláusula throws indica que un método puede producir una excepción que no desea o no puede manejar. En lugar de capturarla, permite que la excepción se propague hacia arriba en la cadena de llamadas. Esto deja al código llamador la responsabilidad de decidir cómo actuar ante ese error.

Usar throws es una alternativa válida a capturar excepciones cuando el método no tiene suficiente información para decidir qué hacer o cuando se prefiere que niveles superiores gestionen el fallo.

---

## 13. Pon un ejemplo en Java de firma de método que incluya throws, de una función que abre un fichero pero que declara que no le interesa manejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del finally.

Este ejemplo muestra un método que abre un archivo y deja que FileNotFoundException se propague. El bloque finally garantiza el cierre del recurso aunque ocurra un error.

```java
void leerArchivo(String ruta) throws FileNotFoundException {
    Scanner sc = null;
    try {
        sc = new Scanner(new File(ruta));
        System.out.println(sc.nextLine());
    } finally {
        if (sc != null) sc.close();
    }
}
```

---

## 14. ¿Podemos poner en throws excepciones no controladas, como RuntimeException? ¿Debería el método llamador entonces poner try-catch en ese caso? ¿Qué sentido tendría?

Es posible poner excepciones no controladas en throws, aunque normalmente no es necesario. La filosofía de RuntimeException es que indica errores de programación y no requiere captura obligatoria. Declararlas explícitamente puede ayudar a documentar comportamientos excepcionales, pero no se espera que el código llamador las controle de inmediato.

El método llamador no está obligado a poner try-catch para estas excepciones. Solo tendría sentido capturarlas si se quiere evitar que el programa termine abruptamente, aunque su aparición suele implicar que existe un fallo lógico que debe corregirse.

---

## 15. ¿Cuándo se recomienda usar excepciones controladas, como IOException, y cuándo no controladas como IllegalArgumentException? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

Las excepciones controladas se usan cuando el error es previsible y recuperable, como fallos de entrada/salida o ausencia de recursos externos. En cambio, las no controladas representan errores de programación que deberían corregirse dentro del código antes de ejecutarlo en producción.

No todos los lenguajes distinguen entre excepciones controladas y no controladas. Muchos, como C++ o Python, solo tienen un tipo de excepción equivalente a las no controladas de Java. En esos lenguajes, lo más habitual es lanzar excepciones no controladas.

---

## 16. ¿Tiene sentido lanzar excepciones dentro del catch? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

Lanzar una excepción dentro del catch puede ser útil cuando se desea transformar un error de bajo nivel en otro de nivel superior. Esto permite mejorar la claridad y la coherencia de la aplicación al encapsular detalles internos en una excepción más adecuada para el contexto.

También es posible relanzar la misma excepción capturada. Esto es útil cuando se necesita realizar acciones adicionales antes de propagar el error, como registrar información o liberar recursos. Después de esas tareas, se permite que la excepción siga su camino.

Ejemplo de lanzar nueva excepción:

```java
catch (IOException e) {
    throw new RuntimeException("Error leyendo configuración");
}
```

Ejemplo de relanzar la misma excepción:

```java
catch (IOException e) {
    logError(e);
    throw e;
}
```

---

## 17. ¿En qué consiste que una excepción sea la "causa" de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

Una excepción puede incluir otra como causa para reflejar mejor la cadena de errores. Esto permite encapsular errores internos en otros más adecuados para la lógica del programa. La excepción de alto nivel conserva información sobre el error original, facilitando la depuración.

Cuando una excepción con causa se imprime por pantalla, normalmente se muestra también la traza de la excepción interna. Esto permite comprender en qué punto exacto se produjo el error inicial.

```java
try {
    leerArchivo("datos.txt");
} catch (FileNotFoundException e) {
    throw new MiExcepcion("No se pudo cargar el archivo requerido", e);
}
```

En este ejemplo, MiExcepcion mostrará en la salida tanto su propio mensaje como la traza del FileNotFoundException que originó el problema.