<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
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

### Respuesta
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
public double calcularDistanciaAPunto(Punto otro) {
    double diffX = this.x - otro.x; // Acceso directo a otro.x (privado)
    double diffY = this.y - otro.y; // Acceso directo a otro.y (privado)
    return Math.sqrt(diffX * diffX + diffY * diffY);
}
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

### Respuesta


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta
