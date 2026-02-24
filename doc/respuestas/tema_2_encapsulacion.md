# TEMA 2. Encapsulación 

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

### Respuesta
La encapsulación y la ocultación de información buscan agrupar los datos (atributos) y los comportamientos (métodos) que operan sobre esos datos dentro de una unidad llamada clase. Su objetivo principal es proteger el estado interno de un objeto, permitiendo que solo un conjunto definido de operaciones pueda interactuar con él, en lugar de permitir el acceso directo y arbitrario.

Algunas ventajas de la ocultación de información incluyen la reducción del acoplamiento entre componentes del sistema, lo que facilita el mantenimiento y la evolución del código. También previene la corrupción accidental de los datos al forzar su modificación a través de métodos controlados, mejora la seguridad y permite cambiar la implementación interna sin afectar al código externo que la utiliza.

## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta
La interfaz pública de una clase se refiere al conjunto de miembros (métodos y atributos) que se declaran como accesibles desde fuera de la propia clase. Estos miembros definen las operaciones que otros objetos o partes del programa pueden solicitar a un objeto de dicha clase, actuando como un contrato de servicio.

Esta interfaz se relaciona directamente con la ocultación de información porque actúa como la frontera controlada entre el exterior y los detalles internos de la clase. Mientras la interfaz pública expone lo necesario para la interacción, la ocultación mantiene privados los detalles de implementación y los datos internos, asegurando que el acceso solo ocurra a través de los canales definidos y permitidos.

## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta
Hay que diseñar la interfaz pública con cuidado porque define cómo otras partes del sistema interactuarán con la clase. Una interfaz bien diseñada es clara, minimalista y estable, lo que facilita la comprensión y el uso correcto de la clase. Una interfaz deficiente puede llevar a un uso erróneo, código frágil y una alta dependencia entre módulos.

Cambiar la interfaz pública una vez que el código está en uso no es fácil, especialmente si otras clases o módulos dependen de ella. Cualquier modificación, como eliminar un método o cambiar su firma, puede romper el código cliente existente, forzando a realizar modificaciones costosas en cascada. Por ello, la interfaz pública debe ser lo más estable posible.

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta
Las invariantes de clase son condiciones o reglas lógicas sobre el estado interno de un objeto que deben ser verdaderas en todo momento durante la vida útil del objeto, excepto quizás durante la ejecución de un método. Son propiedades que definen un estado consistente y válido para los datos de la instancia.

La ocultación de información ayuda a preservar estas invariantes al restringir el acceso directo a los campos internos. Si los atributos fueran públicos, cualquier parte del código podría modificarlos arbitrariamente, posiblemente llevando al objeto a un estado inconsistente. Al hacerlos privados y forzar todas las modificaciones a través de métodos públicos (como setters con validación), la clase puede garantizar que sus invariantes se mantengan tras cada operación.

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?


```java
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

    public double getX() {
        return x;
    }

    public double getY() {
        return y;
    }
}
```
La interfaz pública de esta clase `Punto` está compuesta por el constructor `Punto(double, double)` y los métodos públicos: `calcularDistanciaAOrigen()`, `getX()` y `getY()`. Estos son los únicos elementos accesibles desde fuera de la clase.

En Java, `public` es un modificador que indica que el miembro (atributo, método, constructor o clase) es accesible desde cualquier otra clase. `Private` es un modificador que restringe el acceso al miembro únicamente al interior de la propia clase en la que está declarado, impidiendo el acceso directo desde el exterior.

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta
En Java, los modificadores `public` y `private` se pueden aplicar a clases (aunque una clase de nivel superior solo puede ser `public` o por defecto), atributos (también llamados campos o variables miembro), constructores y métodos. Estos modificadores controlan el nivel de acceso o visibilidad de dichos elementos desde otras partes del código.

Se aplican para definir la interfaz y la encapsulación de una clase. Los elementos `public` forman la interfaz accesible para otros, mientras que los elementos `private` son detalles de implementación ocultos, accesibles solo dentro del cuerpo de la propia clase. No se pueden aplicar directamente, por ejemplo, a variables locales dentro de un método.


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta
Sí, existen más tipos de visibilidad además de pública y privada. En Java, además de `public` y `private`, existen los modificadores `protected` y el modificador por defecto (package-private). `Protected` permite el acceso desde la misma clase, clases del mismo paquete y subclases, mientras que el modificador por defecto permite el acceso solo desde clases del mismo paquete.

En otros lenguajes como C++ y C#, también existen niveles similares de visibilidad, aunque con algunas diferencias específicas. Por ejemplo, C++ tiene `public`, `private`, `protected` y también `friend` para otorgar acceso especial a funciones o clases específicas. Estos niveles adicionales permiten un control más granular sobre la encapsulación y el acceso a los miembros de una clase.

## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta
Los miembros privados de una instancia están ocultos para otras clases, pero NO para otras instancias de la misma clase. En POO, una instancia puede acceder directamente a los miembros privados de otra instancia de la misma clase, ya que el control de acceso se basa en la clase, no en el objeto individual.

Ejemplo con el método `calcularDistanciaAPunto(Punto otro)` en la clase `Punto`:
```
public double calcularDistanciaAPunto(Punto otro) {
    double diffX = this.x - otro.x; // Acceso directo a otro.x (privado)
    double diffY = this.y - otro.y; // Acceso directo a otro.y (privado)
    return Math.sqrt(diffX * diffX + diffY * diffY);
}
```
Aunque `x` e `y` son privados, el método puede acceder directamente a `otro.x` y `otro.y` porque `otro` es una instancia de la misma clase `Punto`. Esto demuestra que la privacidad es a nivel de clase, no a nivel de objeto.

## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta
Los métodos "getter" y "setter" son métodos públicos que proporcionan acceso controlado a los atributos privados de una clase. Un "getter" (o método de acceso) retorna el valor de un atributo privado, mientras que un "setter" (o método de modificación) establece o modifica el valor de un atributo privado, usualmente con alguna validación.

Estos métodos forman parte de la interfaz pública de la clase y son fundamentales para la encapsulación. Permiten mantener los atributos privados mientras se ofrece una forma controlada de leerlos y modificarlos. Los setters pueden incluir lógica de validación para asegurar que los datos asignados cumplan con las reglas de negocio y mantengan las invariantes de clase.

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta
No, cuando hablamos de "seguridad" en el contexto de la ocultación de información, generalmente no nos referimos a seguridad contra hackeos o ataques maliciosos externos. La seguridad aquí se refiere a la protección contra errores y manipulaciones incorrectas dentro del propio código del programa.

La ocultación mejora la seguridad del programa al prevenir que código externo modifique accidentalmente el estado interno de un objeto de maneras inconsistentes o no deseadas. Esto reduce bugs y comportamientos inesperados, haciendo que el programa sea más robusto y confiable. Es una medida de seguridad a nivel de diseño y mantenimiento del código, no una medida de seguridad informática contra atacantes externos.

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta
Un miembro de instancia (no estático) pertenece a cada objeto individual creado a partir de la clase. Cada instancia tiene su propia copia de estos miembros. Un miembro de clase (estático) pertenece a la clase misma y es compartido por todas las instancias de esa clase. Solo existe una copia del miembro estático, independientemente de cuántos objetos se creen.

Sí, los miembros de clase también se pueden ocultar usando los mismos modificadores de acceso (`private`, `protected`, etc.). Un miembro de clase declarado como `private` solo será accesible dentro de la propia clase, tanto desde métodos de instancia como desde métodos estáticos. La ocultación de información aplica igualmente a miembros estáticos para proteger datos compartidos y controlar su acceso a través de una interfaz pública adecuada.


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

Los constructores privados pueden utilizarse cuando se desea controlar estrictamente cómo se crean las instancias de una clase. Esta técnica evita que otros componentes del programa puedan crear objetos de forma libre, obligando a que la inicialización siga un proceso concreto. De esta manera se garantiza que los objetos comiencen siempre en un estado válido o siguiendo una lógica de construcción específica.

Este enfoque se emplea en patrones como singleton, donde solo puede existir una única instancia de la clase. También se usa en métodos factoría, donde la clase ofrece métodos estáticos que devuelven instancias configuradas de una manera determinada. En estos escenarios, ocultar el constructor facilita el mantenimiento de invariantes y evita usos incorrectos.


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

Los miembros de clase se indican utilizando la palabra clave static. Un atributo o método estático pertenece a la clase y no a una instancia concreta, por lo que su valor se comparte entre todos los objetos creados. Esto resulta útil para almacenar información global, como estadísticas o valores acumulados entre múltiples instancias.

En la clase Punto, se pueden añadir miembros estáticos que registren el valor máximo de x e y establecido hasta el momento. Cada vez que se construya un nuevo Punto, se actualizarán estos límites si las nuevas coordenadas los superan.

```java
class Punto {
    int x;
    int y;

    static int maxX = Integer.MIN_VALUE;
    static int maxY = Integer.MIN_VALUE;

    Punto(int x, int y) {
        this.x = x;
        this.y = y;

        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }
}
```

---

## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`?

Un método factoría es un método estático encargado de crear instancias aplicando una lógica previa antes de invocar al constructor. En este caso, se desea redondear las coordenadas al entero más cercano, por lo que el método realiza esa operación y devuelve un nuevo Punto con los valores ajustados.

Este método debe ser static porque pertenece a la clase y no a objetos individuales. Así puede usarse sin necesidad de crear una instancia previa.

```java
static Punto desdeDouble(double x, double y) {
    int rx = (int) Math.round(x);
    int ry = (int) Math.round(y);
    return new Punto(rx, ry);
}
```

---

## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

Cambiar la implementación interna sin modificar la interfaz pública permite evolucionar la clase sin afectar a los usuarios que ya la utilizan. En este caso se sustituye la representación interna mediante dos atributos por un array, pero se mantienen los métodos públicos getX y getY para garantizar compatibilidad.

Este enfoque ilustra el objetivo de la encapsulación: los detalles internos pueden variar siempre que la interfaz pública se mantenga estable.

```java
class Punto {
    private final double[] coords = new double[2];

    Punto(double x, double y) {
        coords[0] = x;
        coords[1] = y;
    }

    double getX() { return coords[0]; }
    double getY() { return coords[1]; }
}
```

---

## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

Aunque pueda parecer que usar getters y setters implica que un atributo podría ser público, esto no es recomendable. Declarar atributos públicos elimina el control sobre el estado interno del objeto y permite modificaciones sin restricciones, lo que resulta contrario al principio de encapsulación.

La convención habitual en Java es declarar todos los atributos como privados y proporcionar getters y setters solo cuando sea necesario. Esto permite añadir validaciones, mantener invariantes de clase y evitar estados incoherentes. Las invariantes representan condiciones que deben cumplirse siempre, y la encapsulación facilita preservarlas.

---

## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

Una clase es inmutable si su estado no puede cambiar después de construir la instancia. Esto implica que todos los atributos deben inicializarse en el constructor y no existir métodos que alteren esos valores. Las instancias permanecen siempre en el mismo estado, lo cual simplifica su uso y reduce errores.

Un método modificador es aquel que altera el estado interno del objeto. No todos los métodos modificadores son setters, pues un método puede modificar atributos de manera indirecta. En una clase inmutable, ninguno de estos métodos puede existir.

Las clases inmutables presentan ventajas importantes: son más seguras en entornos concurrentes, más sencillas de razonar y menos propensas a errores por cambios inesperados. Por estos motivos, su uso suele ser recomendado cuando sea posible.

---

## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

No, no es recomendable incluir setters de manera automática. Permitir la modificación indiscriminada de atributos puede romper invariantes internas y generar estados inconsistentes. Los setters deben añadirse solo cuando exista una necesidad clara de modificar un valor después de crear el objeto.

Un diseño más seguro consiste en limitar los setters, favorecer atributos inmutables y permitir modificaciones únicamente a través de métodos bien controlados que mantengan la consistencia del objeto.

---

## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

La clase String en Java es inmutable. Esto significa que cada operación que parezca modificar una cadena, como concatenar dos strings, crea realmente una nueva instancia sin alterar las originales. Aunque esto mejora la seguridad y el rendimiento en cachés internas, puede generar un gran número de objetos temporales si se realizan muchas concatenaciones consecutivas.

Para concatenaciones repetidas o construcción incremental de textos, se recomienda usar StringBuilder, que permite modificar la cadena de manera eficiente sin crear nuevas instancias en cada operación. Una vez terminado el proceso, se puede obtener un String final mediante toString.

---

## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java?

Los objetos pueden compararse por identidad o por contenido. La identidad se verifica con el operador ==, que comprueba si dos referencias apuntan al mismo objeto en memoria. Para comparar contenido, Java utiliza el método equals, que debe sobrescribirse en las clases que necesiten comparación estructural.

Por defecto, equals hereda la implementación de Object, que compara identidad. Para que dos objetos sean considerados iguales por su contenido, es necesario redefinir equals. En el caso de las cadenas String, la comparación debe hacerse siempre mediante equals, ya que == solo comprobaría si ambas referencias apuntan al mismo objeto.

---

## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers?

Las clases wrapper encapsulan valores primitivos y permiten tratarlos como objetos. En Java existen wrappers como Integer, Double o Boolean, que permiten usar valores primitivos en colecciones genéricas o en contextos donde solo pueden emplearse objetos.

La conversión entre primitivo y wrapper puede realizarse de manera automática mediante autoboxing y unboxing. Esto simplifica el código, aunque requiere atención porque los wrappers ocupan más memoria y pueden contener null. No todos los lenguajes necesitan wrappers, ya que algunos no distinguen entre tipos primitivos y objetos.

---

## 22. En POO ¿Qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

Un tipo enumerado define un conjunto limitado de valores con significado dentro de un dominio concreto. Esto proporciona claridad y evita errores derivados del uso de valores numéricos o constantes sueltas.

En Java, un enumerado es una clase especial con instancias predefinidas. Puede tener atributos privados, métodos y constructores, lo que permite encapsular comportamiento y datos asociados a cada valor. Este diseño mejora la seguridad y hace que el código sea más expresivo y fácil de mantener.

Los enumerados permiten encapsular detalles internos y evitar valores no válidos, lo que aumenta la robustez del programa.

---

## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`.

```java
enum Mes {
    ENERO(31, 1),
    FEBRERO(28, 2),
    MARZO(31, 3),
    ABRIL(30, 4),
    MAYO(31, 5),
    JUNIO(30, 6),
    JULIO(31, 7),
    AGOSTO(31, 8),
    SEPTIEMBRE(30, 9),
    OCTUBRE(31, 10),
    NOVIEMBRE(30, 11),
    DICIEMBRE(31, 12);

    private final int dias;
    private final int numero;

    Mes(int dias, int numero) {
        this.dias = dias;
        this.numero = numero;
    }

    int getDias() { return dias; }
    int getNumero() { return numero; }

    boolean esDePrimavera(boolean norte) {
        return norte ? (numero >= 3 && numero <= 5)
                     : (numero >= 9 && numero <= 11);
    }

    boolean esDeVerano(boolean norte) {
        return norte ? (numero >= 6 && numero <= 8)
                     : (numero == 12 || numero <= 2);
    }

    boolean esDeOtono(boolean norte) {
        return norte ? (numero >= 9 && numero <= 11)
                     : (numero >= 3 && numero <= 5);
    }

    boolean esDeInvierno(boolean norte) {
        return norte ? (numero == 12 || numero <= 2)
                     : (numero >= 6 && numero <= 8);
    }
}
```