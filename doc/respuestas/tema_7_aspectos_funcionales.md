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
Un **puntero a función** es una variable que almacena la dirección de memoria de una función, permitiendo invocarla de forma indirecta. En C, las funciones no son objetos como en Java, pero sí pueden manipularse mediante punteros, lo que resulta útil para implementar comportamientos flexibles como callbacks, tablas de funciones o estrategias intercambiables. Su sintaxis puede parecer compleja al principio, ya que combina la declaración de punteros con la firma de la función (tipo de retorno y parámetros).

Desde un punto de vista conceptual, un puntero a función permite desacoplar la llamada a una función de su nombre concreto, lo que introduce cierto grado de abstracción similar al polimorfismo en lenguajes orientados a objetos. En lugar de invocar directamente una función, se invoca a través del puntero, lo que permite cambiar dinámicamente qué función se ejecuta. Esto se acerca a la idea de pasar “comportamientos” como parámetros, algo común en programación funcional.

A continuación se muestra un ejemplo en C donde se define una función que convierte una cadena a mayúsculas. Se crea un puntero a dicha función llamado `aMayusculas` y se invoca a través de él:

```c
#include <stdio.h>
#include <ctype.h>

void convertirAMayusculas(char *cadena) {
    for (int i = 0; cadena[i] != '\0'; i++) {
        cadena[i] = toupper(cadena[i]);
    }
}

int main() {
    char texto[] = "Hola mundo";

    // Declaración del puntero a función
    void (*aMayusculas)(char *);

    // Asignación de la función al puntero
    aMayusculas = convertirAMayusculas;

    // Invocación de la función a través del puntero
    aMayusculas(texto);

    printf("%s\n", texto);

    return 0;
}
```

En este ejemplo, el puntero `aMayusculas` tiene el mismo tipo que la función `convertirAMayusculas`, es decir, una función que recibe un `char*` y no devuelve valor (`void`). Tras asignarle la dirección de la función, se puede invocar como si fuera la propia función. Este mecanismo resulta especialmente útil en C para implementar patrones de diseño básicos sin necesidad de soporte nativo de orientación a objetos.


## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta
Una **función lambda** es una función anónima (sin nombre) que puede definirse de forma breve y utilizarse como valor, es decir, puede asignarse a variables, pasarse como argumento o devolverse como resultado. Su principal objetivo es permitir expresar comportamientos de manera concisa, evitando la necesidad de declarar métodos completos cuando solo se requiere una operación sencilla. Este concepto está muy relacionado con la programación funcional y aporta flexibilidad similar a la que se obtiene con punteros a función en C, pero con una sintaxis más segura y expresiva.

En lenguajes modernos, las funciones lambda facilitan el uso de funciones como “ciudadanos de primera clase”. Esto permite escribir código más declarativo y cercano al “qué hacer” en lugar del “cómo hacerlo”. En el caso de Java, las lambdas se apoyan en interfaces funcionales (interfaces con un único método abstracto), mientras que en JavaScript las funciones son directamente valores, lo que simplifica su uso. A continuación se muestran ejemplos en ambos lenguajes donde se define una lambda que transforma una cadena a mayúsculas y se asigna a una variable local llamada `aMayusculas`.

**Ejemplo en JavaScript:**

```javascript
const aMayusculas = (cadena) => cadena.toUpperCase();

let texto = "Hola mundo";
texto = aMayusculas(texto);

console.log(texto);
```

**Ejemplo en Java (usando Function<String, String>):**

```java
import java.util.function.Function;

public class EjemploLambda {
    public static void main(String[] args) {
        Function<String, String> aMayusculas = (cadena) -> cadena.toUpperCase();

        String texto = "Hola mundo";
        texto = aMayusculas.apply(texto);

        System.out.println(texto);
    }
}
```

En ambos casos, la variable `aMayusculas` referencia a una función lambda que recibe una cadena y devuelve su versión en mayúsculas. La principal diferencia es que en Java se requiere especificar el tipo funcional (`Function<String, String>`) y utilizar el método `apply` para invocarla, mientras que en JavaScript la invocación se realiza de forma directa como cualquier función.

Se pone `String` dos veces porque el tipo `Function<T, R>` en Java es **genérico con dos parámetros de tipo**: el primero (`T`) representa el tipo de entrada (argumento) y el segundo (`R`) representa el tipo de salida (valor de retorno). Es decir, `Function<T, R>` modela una función que recibe un valor de tipo `T` y devuelve un valor de tipo `R`.

## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta
El **paradigma funcional** es un estilo de programación en el que los programas se construyen principalmente mediante la **composición de funciones**. En lugar de centrarse en estados mutables y secuencias de instrucciones (como en C) o en objetos que encapsulan datos y comportamiento (como en Java clásico), se pone el énfasis en funciones puras: aquellas que, para unos mismos argumentos, siempre devuelven el mismo resultado y no producen efectos secundarios. Esto favorece un código más predecible, fácil de razonar y más sencillo de paralelizar, ya que se evita la dependencia de estados compartidos.

A algunos lenguajes orientados a objetos como Java (especialmente a partir de Java 8) se les denomina **multi-paradigma** porque permiten combinar distintos estilos de programación en un mismo lenguaje. Aunque Java sigue siendo principalmente orientado a objetos (clases, herencia, encapsulación), desde Java 8 incorpora características del paradigma funcional, como expresiones lambda, referencias a métodos y el uso de streams. Esto permite escribir código más declarativo y conciso sin abandonar el modelo de objetos, eligiendo el enfoque más adecuado según el problema.

Decir que las funciones son **“ciudadanos de primera clase”** significa que pueden tratarse igual que otros valores del lenguaje (como enteros o cadenas). Es decir, pueden almacenarse en variables, pasarse como argumentos a otras funciones y devolverse como resultado. En C esto se consigue mediante punteros a función, mientras que en lenguajes como JavaScript o en Java con lambdas se hace de forma más directa y segura. Esta característica es clave en el paradigma funcional, ya que permite construir abstracciones más flexibles y reutilizables basadas en el comportamiento.

**Ejemplo en Java**
Un ejemplo sencillo de funciones como “ciudadanos de primera clase” en Java consiste en **pasar una función como parámetro** a otro método. Para ello se utiliza una interfaz funcional como `Function<T, R>`, lo que permite recibir una lambda y aplicarla dentro del método. De esta forma, el comportamiento no está fijado de antemano, sino que se puede cambiar dinámicamente.

```java
import java.util.function.Function;

public class Ejemplo {
    
    // Método que recibe una función como parámetro
    public static String procesar(String texto, Function<String, String> f) {
        return f.apply(texto);
    }

    public static void main(String[] args) {
        // Se define una lambda y se guarda en una variable
        Function<String, String> aMayusculas = s -> s.toUpperCase();

        // Se pasa la función como argumento
        String resultado = procesar("Hola mundo", aMayusculas);

        // O tambien se puede pasar de forma literal
        String resultado = procesar("Hola mundo", s -> s.toUpperCase());

        System.out.println(resultado);
    }
}
```

En este ejemplo, la variable `aMayusculas` almacena una función (lambda), lo que demuestra que puede tratarse como un valor. Posteriormente, dicha función se pasa al método `procesar`, que la utiliza sin conocer su implementación concreta. Esto ilustra cómo en Java las funciones pueden almacenarse, pasarse y ejecutarse dinámicamente, cumpliendo la idea de “ciudadanos de primera clase”.


## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta
La **sintaxis básica de una función lambda en Java** sigue la forma general `(parámetros) -> expresión` o `(parámetros) -> { bloque de código }`. A la izquierda de la flecha (`->`) se indican los parámetros de entrada, y a la derecha se define el cuerpo de la función. Si el cuerpo es una única expresión, su valor se devuelve automáticamente sin necesidad de usar `return`; en cambio, si se utiliza un bloque con llaves `{}`, entonces sí es necesario usar `return` cuando se quiera devolver un resultado.

En cuanto a los parámetros, se pueden escribir con o sin tipo explícito, ya que el compilador suele inferirlos a partir del contexto (por ejemplo, del tipo de la interfaz funcional). Si hay un solo parámetro, incluso pueden omitirse los paréntesis. Sin embargo, si no hay parámetros, es obligatorio usar `()`. Esta flexibilidad permite que las lambdas sean más concisas que las clases anónimas tradicionales, que requerían mucha más sintaxis para lograr lo mismo.

Por ejemplo, una lambda que recibe una cadena y devuelve su longitud podría escribirse como `(String s) -> s.length()` o simplemente `s -> s.length()`. Si se necesitara más lógica, podría usarse un bloque: `(s) -> { int n = s.length(); return n; }`. En todos los casos, la lambda debe ajustarse a una **interfaz funcional** (como `Function<T, R>`), lo que determina el número y tipo de parámetros, así como el tipo de retorno. Esto garantiza la compatibilidad con el sistema de tipos de Java.

Una lambda seria esta: `Function<T, R> EjemploLambda = (String s) -> s.length()`

## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta
Para recibir una función como parámetro y utilizarla dentro de un método, basta con definir dicho método de forma que acepte una referencia a función (en JavaScript) o una interfaz funcional (en Java). Este enfoque permite desacoplar el “qué se hace” (la transformación) del “cuándo se hace” (la llamada al método), lo que aumenta la reutilización y flexibilidad del código. Es una aplicación directa de tratar funciones como valores.

En JavaScript, como las funciones son valores nativos del lenguaje, el método puede recibir directamente una función como argumento y ejecutarla sin necesidad de tipos adicionales. La lambda `aMayusculas` se pasa al método `transformar`, que simplemente la invoca con la cadena recibida:

```javascript
function transformar(texto, funcion) {
    return funcion(texto);
}

const aMayusculas = (cadena) => cadena.toUpperCase();

let resultado = transformar("Hola mundo", aMayusculas);

console.log(resultado);
```

En Java, se utiliza una interfaz funcional como `Function<String, String>` para representar la función transformadora. El método `transformar` recibe tanto la cadena como la función, y la invoca mediante el método `apply`. De este modo, se logra un comportamiento equivalente al de JavaScript, pero con tipado estático:

```java
import java.util.function.Function;

public class Ejemplo {

    public static String transformar(String texto, Function<String, String> funcion) {
        return funcion.apply(texto);
    }

    public static void main(String[] args) {
        Function<String, String> aMayusculas = s -> s.toUpperCase();

        String resultado = transformar("Hola mundo", aMayusculas);

        System.out.println(resultado);
    }
}
```

En ambos casos, el método `transformar` no conoce la lógica concreta de la transformación, sino que la recibe como parámetro. Esto permite reutilizar el mismo método con distintas funciones (por ejemplo, convertir a minúsculas, invertir la cadena, etc.), lo que ilustra claramente la potencia de las funciones como “ciudadanos de primera clase”.


## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta
Para invocar el método `transformar` pasando directamente una función lambda, no es necesario definir previamente una variable como `aMayusculas`. En su lugar, la lambda se escribe **en el propio momento de la llamada**, lo que resulta útil cuando la función solo se va a utilizar una vez. Este estilo hace el código más compacto y refuerza la idea de que las funciones pueden crearse “al vuelo”.

En JavaScript, la lambda que invierte la cadena puede definirse directamente como argumento del método `transformar`. Para invertir una cadena, se suele convertir en array, invertirlo y volver a unirlo:

```javascript
function transformar(texto, funcion) {
    return funcion(texto);
}

let resultado = transformar("Hola mundo", (cadena) => {
    return cadena.split("").reverse().join("");
});

console.log(resultado);
```

En Java, se sigue el mismo enfoque, pero utilizando la interfaz funcional `Function<String, String>`. La lambda se pasa directamente como segundo argumento del método `transformar`, sin necesidad de almacenarla previamente en una variable:

```java
import java.util.function.Function;

public class Ejemplo {

    public static String transformar(String texto, Function<String, String> funcion) {
        return funcion.apply(texto);
    }

    public static void main(String[] args) {
        String resultado = transformar("Hola mundo", s -> {
            return new StringBuilder(s).reverse().toString();
        });

        System.out.println(resultado);
    }
}
```

En ambos casos, la función de inversión se define justo en el momento de la llamada, lo que permite expresar transformaciones puntuales de forma directa. Este patrón es muy común en programación funcional, ya que evita la creación de funciones auxiliares cuando no es necesario reutilizarlas.


## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta
Un **cierre** o *closure* es una función que “captura” variables de su entorno léxico, es decir, puede acceder a variables definidas fuera de ella incluso después de que ese contexto haya terminado. En el caso de las funciones lambda, esto significa que pueden utilizar variables locales del método donde se definen sin necesidad de recibirlas como parámetros. De este modo, la función no solo depende de sus argumentos, sino también del contexto en el que fue creada.

En Java, las lambdas pueden capturar variables locales, pero con una restricción importante: dichas variables deben ser **finales o efectivamente finales** (es decir, no se modifican después de su inicialización). Esto garantiza seguridad y evita problemas con el manejo de memoria y concurrencia. A pesar de esta limitación, se pueden construir comportamientos muy expresivos aprovechando este mecanismo.

A continuación se muestra una modificación del ejemplo anterior. Se define una variable local `sufijo` fuera de la lambda, y la función lambda la utiliza para concatenarla a la cadena de entrada:

```java
import java.util.function.Function;

public class Ejemplo {

    public static String transformar(String texto, Function<String, String> funcion) {
        return funcion.apply(texto);
    }

    public static void main(String[] args) {
        String sufijo = "!!!"; // variable local capturada

        String resultado = transformar("Hola mundo", s -> {
            return s + sufijo;
        });

        System.out.println(resultado);
    }
}
```

En este ejemplo, la lambda `s -> s + sufijo` accede a la variable `sufijo` definida en el método `main`, lo que demuestra el concepto de cierre. Aunque `sufijo` no es un parámetro de la función, queda “capturada” por la lambda, formando parte de su contexto.


## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta
La diferencia fundamental es que una **función lambda** no solo representa una dirección de código ejecutable, sino que también puede ir acompañada de un **contexto (cierre)**, mientras que un **puntero a función en C** únicamente almacena la dirección de una función concreta. En C, las funciones son entidades independientes del entorno donde se utilizan; no pueden “recordar” variables locales del contexto en el que se pasan. En cambio, una lambda puede capturar variables externas (como se ha visto con `sufijo`), lo que le permite comportarse de forma más flexible y expresiva.

Otra diferencia importante está en el nivel de abstracción y seguridad. En C, los punteros a función son una herramienta de bajo nivel: no hay verificación avanzada más allá de que la firma coincida, y su uso puede ser propenso a errores si no se manejan correctamente. En Java, las lambdas están integradas en el sistema de tipos mediante **interfaces funcionales**, lo que garantiza en tiempo de compilación que la función cumple con la firma esperada. Además, la sintaxis de las lambdas es más concisa que la declaración de punteros a función, lo que mejora la legibilidad.

Por último, desde el punto de vista conceptual, las lambdas acercan Java al paradigma funcional, permitiendo tratar funciones como valores con comportamiento asociado y contexto. En C, aunque los punteros a función permiten cierta flexibilidad (como pasar funciones como parámetros), no se alcanza el mismo nivel de expresividad ni de integración con el lenguaje. En resumen, las lambdas son una evolución más potente y segura de la idea básica que representan los punteros a función.


## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta
En este caso se está aplicando un patrón típico del paradigma funcional: una función que **devuelve otra función**. La idea es que `crearDescuento(porcentaje)` genere una función “descuento” personalizada, donde el porcentaje queda fijado en el momento de creación. Esa función devuelta puede aplicarse después a cualquier cantidad.

En Java, esto se implementa usando `Function<Double, Double>`, donde la función resultante recibe un precio y devuelve el precio con descuento aplicado. El valor `porcentaje` no se pasa cuando se usa el descuento, sino cuando se crea, lo que permite construir “descuentos parametrizados”.

```java
import java.util.function.Function;

public class Ejemplo {

    // Función que devuelve otra función (closure)
    public static Function<Double, Double> crearDescuento(double porcentaje) {
        return precio -> precio - (precio * porcentaje / 100);
    }

    public static void main(String[] args) {

        // Se crean dos funciones descuento distintas
        Function<Double, Double> descuento10 = crearDescuento(10);
        Function<Double, Double> descuento20 = crearDescuento(20);

        double precio = 100.0;

        System.out.println(descuento10.apply(precio)); // 90.0
        System.out.println(descuento20.apply(precio)); // 80.0
    }
}
```

La **closure** en este caso se produce porque la lambda `precio -> precio - (precio * porcentaje / 100)` está capturando la variable local `porcentaje`, que pertenece al contexto de `crearDescuento`. Aunque `crearDescuento` ya haya terminado su ejecución, la función devuelta sigue teniendo acceso a ese valor, ya que queda “cerrado” dentro de la lambda.

Esto permite que cada función descuento conserve su propio estado (10% o 20%), sin necesidad de almacenarlo explícitamente en un objeto como ocurriría en programación orientada a objetos.


## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta
Una **interfaz funcional** en Java es una interfaz que contiene **exactamente un único método abstracto**, es decir, un único método sin implementación. Este método es el que define el “contrato funcional” que podrá ser implementado por una función lambda. Gracias a esto, el compilador puede inferir automáticamente a qué tipo pertenece una lambda en función del contexto donde se utiliza.

Este concepto es clave en Java porque permite que las lambdas tengan un tipo bien definido en un lenguaje de tipado estático. En lugar de crear una clase anónima completa, la lambda se asocia directamente a una interfaz funcional concreta, como `Function<T, R>`, `Consumer<T>` o `Predicate<T>`, entre otras. Esto permite mantener la seguridad de tipos sin perder la concisión del paradigma funcional.

En cuanto a los **requisitos**, una interfaz funcional debe cumplir principalmente con que solo tenga **un método abstracto**. Puede, sin embargo, contener varios métodos `default` o `static`, ya que estos tienen implementación y no cuentan como abstractos. Además, es recomendable (aunque no obligatorio) usar la anotación `@FunctionalInterface`, que indica al compilador que se desea que la interfaz cumpla este contrato y genera un error si se añade más de un método abstracto.

Un ejemplo sería `Function<Double, Double>`, cuya única operación abstracta es `apply(T t)`, lo que permite que cualquier lambda con una firma compatible pueda ser tratada como una implementación de esa interfaz.


## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta
Una **interfaz funcional** puede definirse manualmente siempre que cumpla la condición de tener **un único método abstracto**. En este caso, se crea una interfaz llamada `Transformador`, cuyo objetivo es representar cualquier función que reciba un `String` y devuelva otro `String`, es decir, una transformación de cadenas.

Esta interfaz actúa como el “tipo” de la función lambda, permitiendo que cualquier expresión lambda compatible pueda asignarse a una variable de ese tipo. Además, al ser una interfaz funcional, puede utilizarse directamente con lambdas sin necesidad de clases anónimas.

```java
@FunctionalInterface
interface Transformador {
    String transformar(String texto);
}
```

Una vez definida la interfaz, puede usarse para declarar variables que apunten a funciones lambda. En este caso, se puede crear una lambda que convierta una cadena a mayúsculas o que realice cualquier otra transformación.

```java
public class Ejemplo {

    public static void main(String[] args) {

        Transformador aMayusculas = s -> s.toUpperCase();

        Transformador invertir = s -> new StringBuilder(s).reverse().toString();

        System.out.println(aMayusculas.transformar("Hola mundo"));
        System.out.println(invertir.transformar("Hola mundo"));
    }
}
```

En este ejemplo, `Transformador` actúa como el tipo de la función lambda. Cada lambda implementa el método `transformar`, lo que permite tratar las funciones como objetos que se pueden almacenar en variables y ejecutar cuando sea necesario. Esto refuerza la idea de que en Java las lambdas están fuertemente tipadas a través de interfaces funcionales.


## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta
Para hacer la interfaz funcional más flexible, se pueden utilizar **genéricos**, de forma que el transformador no esté limitado a `String`, sino que pueda convertir entre cualquier tipo de datos. Esto permite reutilizar la misma abstracción para múltiples conversiones, manteniendo la seguridad de tipos en tiempo de compilación.

Una interfaz funcional genérica define dos parámetros de tipo: uno para el tipo de entrada y otro para el tipo de salida. De este modo, el método abstracto puede transformar un objeto de un tipo `T` a otro tipo `R`.

```java
@FunctionalInterface
interface Transformador<T, R> {
    R transformar(T valor);
}
```

Con esta definición, se puede crear cualquier tipo de transformador. Por ejemplo, uno que convierta un `Double` en un `Integer` redondeando el valor:

```java
public class Ejemplo {

    public static void main(String[] args) {

        Transformador<Double, Integer> redondear = d -> (int) Math.round(d);

        System.out.println(redondear.transformar(4.3)); // 4
        System.out.println(redondear.transformar(4.7)); // 5
    }
}
```

En este ejemplo, la lambda `d -> (int) Math.round(d)` implementa la conversión de `Double` a `Integer`. La interfaz genérica permite que el compilador verifique que el tipo de entrada y salida son coherentes, evitando errores en tiempo de ejecución.

Gracias a los genéricos, esta misma interfaz `Transformador<T, R>` puede reutilizarse para muchos otros casos, como `String → Integer`, `Integer → String`, o incluso tipos más complejos, manteniendo siempre la misma estructura funcional.


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta
En Java existen varias **interfaces funcionales predefinidas** en el paquete `java.util.function`, diseñadas precisamente para evitar tener que crear interfaces como `Transformador<T, R>` en la mayoría de casos. Estas interfaces cubren los usos más comunes de programación funcional (transformaciones, condiciones, consumo de datos, etc.), y todas ellas tienen un único método abstracto, por lo que son compatibles con lambdas.

La más general es `Function<T, R>`, que representa una transformación de un tipo a otro. Es equivalente al `Transformador<T, R>` definido anteriormente, ya que recibe un `T` y devuelve un `R`. Junto a ella, hay otras variantes especializadas según el caso de uso.

Las principales interfaces funcionales predefinidas en Java son:

* **`Function<T, R>`**: transforma un valor de tipo `T` en otro de tipo `R`.
* **`BiFunction<T1, T2, R>`**: transforma el valor de los tipos `T1, T2` en otro de tipo `R`.
* **`Consumer<T>`**: recibe un valor de tipo `T` y no devuelve nada (efecto secundario).
* **`Supplier<T>`**: no recibe parámetros y devuelve un valor de tipo `T`.
* **`Predicate<T>`**: recibe un valor de tipo `T` y devuelve un `boolean` (condición lógica).

Estas interfaces permiten expresar de forma estándar la mayoría de patrones funcionales sin necesidad de crear nuevas definiciones.

Un ejemplo de uso de varias de ellas sería:

```java
import java.util.function.*;

public class Ejemplo {

    public static void main(String[] args) {

        // Function: transforma Double -> Integer
        Function<Double, Integer> redondear = d -> (int) Math.round(d);

        // Consumer: consume un valor sin devolver nada
        Consumer<String> imprimir = s -> System.out.println(s);

        // Supplier: genera un valor sin entrada
        Supplier<Double> aleatorio = () -> Math.random();

        // Predicate: evalúa una condición
        Predicate<Integer> esPar = n -> n % 2 == 0;

        System.out.println(redondear.apply(4.6));
        imprimir.accept("Hola");
        System.out.println(aleatorio.get());
        System.out.println(esPar.test(10));
    }
}
```

En este conjunto de interfaces se observa cómo Java estandariza los distintos “roles” que puede tener una función. Esto evita duplicar código como `Transformador<T, R>` y proporciona una base común para trabajar con lambdas en todo el ecosistema del lenguaje.


## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta
El método `forEach` en Java es una forma funcional de recorrer colecciones, sustituyendo al bucle `for` tradicional. En lugar de describir el control de iteración de forma explícita, se pasa una acción (una lambda) que se ejecuta para cada elemento de la lista. Esto encaja con el paradigma funcional, ya que se delega el “qué hacer con cada elemento” en una función.

En el caso de una lista de `Integer`, se puede usar `forEach` junto con una lambda que evalúe si cada número es positivo. Si se cumple la condición, se muestra un mensaje. Este enfoque hace el código más declarativo y compacto.

```java
import java.util.Arrays;
import java.util.List;

public class Ejemplo {

    public static void main(String[] args) {

        List<Integer> numeros = Arrays.asList(-3, 5, 0, 12, -1, 7);

        numeros.forEach(n -> {
            if (n > 0) {
                System.out.println(n + " es positivo");
            }
        });
    }
}
```

En este ejemplo, la lambda `n -> { ... }` actúa como la función que se ejecuta por cada elemento de la lista. El método `forEach` recorre internamente la colección, eliminando la necesidad de escribir un bucle explícito. Esto reduce la verbosidad del código y centra la atención en la lógica de procesamiento de cada elemento en lugar del mecanismo de iteración.


## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta
En la firma de `forEach` en Java se utiliza `Consumer<? super T>` en lugar de `Consumer<T>` por una cuestión de **flexibilidad de tipos** en genéricos. Esto está relacionado con cómo Java trata la **herencia y los wildcards**, y evita restricciones innecesarias al pasar funciones.

La clave está en que `? super T` permite aceptar un consumidor de `T` o de cualquier **supertipo de `T`**. Por ejemplo, si se tiene una `List<Integer>`, el `forEach` puede aceptar un `Consumer<Integer>`, pero también un `Consumer<Number>` o incluso `Consumer<Object>`. Esto amplía las posibilidades de reutilización sin romper la seguridad de tipos.

Esto se explica mediante el principio **PECS**, que significa:

* **Producer Extends**
* **Consumer Super**

Es decir:

* Si una estructura **produce valores** (los devuelve), se usa `extends`.
* Si una estructura **consume valores** (los recibe), se usa `super`.

En `forEach`, la lista **produce elementos** (`T`), pero el `Consumer` **los consume**, por eso se usa `? super T`.

---

### Aplicación al método `transformar`

En el método `transformar`, originalmente se usaba algo como:

```java
Function<T, R>
```

Esto está bien cuando el transformador **consume T y produce R**, pero si se quiere aplicar PECS correctamente, se puede razonar así:

* El tipo `T` es un **input (produce desde fuera hacia la función)** → sería `extends` si fuera parte de una estructura productora.
* El tipo `R` es un **output (resultado producido por la función)**.

Sin embargo, en `Function<T, R>` no se usa wildcard porque la interfaz define ambos roles claramente. Donde sí se aplica PECS es cuando se quiere hacer el método más flexible respecto al tipo de la función.

Un ejemplo más flexible sería permitir transformadores que acepten supertipos del input:

```java
import java.util.function.Function;

public class Ejemplo {

    public static <T, R> R transformar(T valor, Function<? super T, ? extends R> funcion) {
        return funcion.apply(valor);
    }

    public static void main(String[] args) {

        Function<Object, String> f = o -> o.toString();

        String resultado = transformar(123, f);

        System.out.println(resultado);
    }
}
```

---

### Interpretación del PECS en este caso

* `? super T` en el parámetro del `Function` significa:
  la función puede aceptar `T` o cualquier supertipo → más flexibilidad al **consumir entrada**.

* `? extends R` significa:
  el resultado puede ser `R` o un subtipo → más flexibilidad al **producir salida**.

---

En resumen, `PECS` permite escribir APIs genéricas más reutilizables y seguras. En el caso de `forEach`, se aplica directamente porque el `Consumer` solo consume datos. En el caso de `transformar`, se puede aplicar para permitir funciones más generales sin perder seguridad de tipos.


## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta
Las **referencias a métodos** permiten tratar un método ya existente como si fuera una función que puede almacenarse en una variable, pasarse como parámetro o invocarse indirectamente. Son una forma más directa de reutilizar comportamiento sin necesidad de escribir una lambda que simplemente llame a ese método. En JavaScript esto es natural gracias a la flexibilidad de las funciones, mientras que en Java se hace mediante referencias a métodos con `::`.

---

## Ejemplo en JavaScript

En JavaScript, los métodos de un objeto pueden extraerse como funciones, pero es importante mantener el contexto (`this`) para que el método siga accediendo correctamente a los atributos del objeto. Por eso suele usarse `bind`.

```javascript
class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }

    saludar() {
        console.log("Hola, soy " + this.nombre);
    }
}

let p = new Persona("Juan");

// Referencia al método saludar con contexto ligado
let referenciaSaludar = p.saludar.bind(p);

// Invocación a través de la referencia
referenciaSaludar();
```

En este caso, `bind(p)` asegura que el método conserve el contexto del objeto `p`. Sin ello, `this` podría perder su referencia original.

---

## Ejemplo en Java

En Java, las referencias a métodos se integran directamente en el sistema de tipos mediante interfaces funcionales. Se usa el operador `::` para obtener una referencia a un método existente.

```java
import java.util.function.Consumer;

class Persona {
    private String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

public class Ejemplo {

    public static void main(String[] args) {

        Persona p = new Persona("Juan");

        // Referencia al método de instancia
        Consumer<Void> referenciaSaludar = v -> p.saludar();

        // Alternativa más idiomática con referencia a método
        Runnable ref = p::saludar;

        // Invocación
        ref.run();
    }
}
```

---

## Idea clave

En ambos lenguajes, una referencia a método permite **tratar un comportamiento existente como un valor**. En JavaScript esto ocurre de forma natural pero hay que cuidar el contexto (`this`), mientras que en Java se integra formalmente con interfaces funcionales y el operador `::`, lo que garantiza tipado estático y compatibilidad con lambdas.


## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta
En Java existen **cuatro tipos principales de referencias a método**, que permiten reutilizar métodos existentes como si fueran funciones compatibles con interfaces funcionales. Todas ellas se basan en el operador `::` y se diferencian según el contexto desde el que se llama al método.

---

## 1. Referencia a método estático

Se refiere directamente a un método de clase sin necesidad de instanciar el objeto. Es equivalente a una función global en C.

```java
import java.util.function.Function;

public class Ejemplo {

    public static String aMayusculas(String s) {
        return s.toUpperCase();
    }

    public static void main(String[] args) {

        Function<String, String> ref = Ejemplo::aMayusculas;

        System.out.println(ref.apply("hola"));
    }
}
```

---

## 2. Referencia a constructor

Permite crear objetos usando una referencia funcional.

```java
import java.util.function.Supplier;

class Persona {
    public Persona() {
        System.out.println("Persona creada");
    }
}

public class Ejemplo {

    public static void main(String[] args) {

        Supplier<Persona> ref = Persona::new;

        Persona p = ref.get();
    }
}
```

---

## 3. Referencia a método de instancia de un objeto concreto

Se usa cuando ya existe una instancia y se quiere referenciar uno de sus métodos.

```java
import java.util.function.Consumer;

class Persona {
    public void saludar() {
        System.out.println("Hola");
    }
}

public class Ejemplo {

    public static void main(String[] args) {

        Persona p = new Persona();

        Runnable ref = p::saludar;

        ref.run();
    }
}
```

---

## 4. Referencia a método de instancia de un tipo (cualquier instancia)

Se usa cuando el método se aplica a cualquier objeto del tipo, y el receptor se pasa como parámetro implícito.

```java
import java.util.function.Function;

public class Ejemplo {

    public static void main(String[] args) {

        Function<String, Integer> ref = String::length;

        System.out.println(ref.apply("hola"));
    }
}
```

---

## Idea general

Estos cuatro tipos permiten cubrir prácticamente todos los casos de reutilización de métodos como funciones:

* **Estático** → pertenece a la clase
* **Constructor** → crea objetos
* **Instancia concreta** → usa un objeto específico
* **Instancia genérica** → se aplica a cualquier objeto del tipo

Esto hace que Java pueda integrar programación funcional sin perder su modelo orientado a objetos.


## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta
Se puede usar `Collections.sort` de forma funcional pasando un **comparador como lambda**, lo que permite expresar directamente la lógica de ordenación sin necesidad de clases auxiliares. En este caso se ordena una lista de `Persona` primero por edad y, si hay empate, por nombre en orden alfabético.

---

## Versión 1: comparación manual en la lambda

En esta versión se implementa la lógica de comparación “a mano”, devolviendo un entero según el resultado de las comparaciones:

```java
import java.util.*;

class Persona {
    String nombre;
    int edad;

    Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }
}

public class Ejemplo {

    public static void main(String[] args) {

        List<Persona> lista = new ArrayList<>();

        lista.add(new Persona("Ana", 30));
        lista.add(new Persona("Luis", 25));
        lista.add(new Persona("Carlos", 30));
        lista.add(new Persona("Bea", 25));

        Collections.sort(lista, (p1, p2) -> {
            if (p1.edad != p2.edad) {
                return p1.edad - p2.edad;
            } else {
                return p1.nombre.compareTo(p2.nombre);
            }
        });

        for (Persona p : lista) {
            System.out.println(p.nombre + " - " + p.edad);
        }
    }
}
```

---

## Versión 2: usando `Comparator`

En esta versión se aprovecha la API de `Comparator`, que ya proporciona métodos auxiliares para encadenar criterios de ordenación de forma más declarativa.

```java
import java.util.*;

class Persona {
    String nombre;
    int edad;

    Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }
}

public class Ejemplo {

    public static void main(String[] args) {

        List<Persona> lista = new ArrayList<>();

        lista.add(new Persona("Ana", 30));
        lista.add(new Persona("Luis", 25));
        lista.add(new Persona("Carlos", 30));
        lista.add(new Persona("Bea", 25));

        Comparator<Persona> comp = Comparator
                .comparingInt((Persona p) -> p.edad)
                .thenComparing(p -> p.nombre);

        Collections.sort(lista, comp);

        for (Persona p : lista) {
            System.out.println(p.nombre + " - " + p.edad);
        }
    }
}
```

---

## Idea clave

En la primera versión se expresa explícitamente la lógica de comparación, mientras que en la segunda se usa `Comparator` para **componer criterios de ordenación de forma funcional y declarativa**. Ambas usan programación funcional, pero la segunda es más expresiva, reutilizable y cercana al estilo moderno de Java.
