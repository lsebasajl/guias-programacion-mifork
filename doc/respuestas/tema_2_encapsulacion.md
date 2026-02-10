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
La **encapsulación** en Programación Orientada a Objetos busca agrupar en una misma unidad (la clase) los datos que representan el estado de un objeto y las operaciones que pueden actuar sobre dichos datos. Junto con ello, la **ocultación de información** pretende restringir el acceso directo a los detalles internos de implementación, permitiendo que el uso del objeto se realice únicamente a través de una interfaz bien definida.

El objetivo principal es separar el *qué hace* un objeto del *cómo lo hace*. De esta forma, se evita que otras partes del programa dependan de detalles internos que podrían cambiar con el tiempo. Esta idea resulta especialmente importante para alguien con experiencia en C/C++, donde el acceso a estructuras de datos suele ser directo y menos controlado.

Entre las ventajas de la ocultación de información se encuentran: una mayor facilidad de mantenimiento del código, la reducción de errores al evitar usos incorrectos de los datos, una mejor modularidad del sistema y la posibilidad de cambiar la implementación interna sin afectar al código que utiliza la clase.


## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta
La **interfaz pública** de una clase está formada por el conjunto de métodos y atributos accesibles desde fuera de la propia clase. Es la “cara visible” del objeto, es decir, aquello que otras clases pueden utilizar para interactuar con él sin conocer su implementación interna.

La relación con la ocultación de información es directa: solo los elementos declarados como públicos forman parte de la interfaz pública, mientras que el resto se mantiene oculto. De esta manera, se controla qué operaciones están permitidas y cómo se puede interactuar con el estado del objeto.

Gracias a esta separación, el programador puede modificar la representación interna o los algoritmos usados por la clase sin necesidad de cambiar el código cliente, siempre que la interfaz pública se mantenga estable.


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta
La **interfaz pública** de una clase actúa como un contrato con el resto del programa. Una vez que otras clases comienzan a depender de ella, cualquier cambio puede provocar errores en cascada. Por ello, debe diseñarse con cuidado, pensando en su uso presente y futuro.

Cambiar la interfaz pública no suele ser sencillo, especialmente en proyectos grandes o cuando la clase es utilizada en muchos puntos del código. Eliminar métodos, cambiar firmas o modificar comportamientos visibles puede obligar a revisar y adaptar numerosas partes del sistema.

Por este motivo, una buena práctica consiste en exponer únicamente lo necesario, manteniendo la interfaz lo más simple y estable posible, y ocultando todo aquello que no deba ser usado directamente.


## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta
Las **invariantes de clase** son condiciones que deben cumplirse siempre para que los objetos de una clase se consideren válidos. Definen reglas sobre el estado interno del objeto, como rangos de valores permitidos o relaciones entre atributos.

La ocultación de información ayuda a mantener estas invariantes porque impide que el estado interno sea modificado libremente desde fuera de la clase. Al forzar el acceso a los datos a través de métodos controlados, se puede comprobar y garantizar que cualquier cambio respeta las condiciones establecidas.

De este modo, la clase se protege a sí misma frente a usos incorrectos y se asegura que todos sus objetos permanezcan en un estado coherente a lo largo de su vida.


## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta
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
}
```
En este ejemplo, los atributos `x` e `y` están declarados como `private`, lo que significa que solo pueden ser accedidos desde dentro de la propia clase. Esto impide que otras clases modifiquen directamente las coordenadas del punto.

La interfaz pública de la clase `Punto` está formada por su constructor y por el método `calcularDistanciaAOrigen`. El modificador `public` indica que estos elementos son accesibles desde cualquier otra clase, mientras que `private` restringe el acceso únicamente al interior de la clase.


## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta
En Java, los modificadores de visibilidad como `public` y `private` pueden aplicarse a clases, atributos, métodos y constructores. Sin embargo, existen ciertas restricciones, como el hecho de que una clase de nivel superior no puede ser `private`.

Cuando se aplican a atributos, métodos o constructores, controlan desde dónde pueden ser utilizados. `public` permite el acceso desde cualquier parte del programa, mientras que `private` limita el acceso a la propia clase.

Este control fino de la visibilidad es una de las herramientas principales que ofrece Java para implementar correctamente la encapsulación y la ocultación de información.


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta
Además de la visibilidad pública y privada, muchos lenguajes orientados a objetos incluyen niveles intermedios de visibilidad. En Java, existen cuatro niveles: `public`, `protected`, visibilidad por defecto (paquete) y `private`.

El modificador `protected` permite el acceso desde clases del mismo paquete o desde subclases, mientras que la visibilidad por defecto restringe el acceso al mismo paquete. Esto ofrece mayor flexibilidad a la hora de diseñar jerarquías de clases y módulos.

En otros lenguajes, como C++, también existen niveles como `protected`, aunque las reglas exactas pueden variar. En todos los casos, el objetivo es proporcionar mecanismos para controlar el acceso y reforzar la encapsulación.


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta
Los **miembros de instancia** son los atributos y métodos que pertenecen a **cada objeto concreto** creado a partir de una clase. No pertenecen a la clase en general, sino a cada instancia individual. Por ejemplo, si se define una clase `Punto` con atributos `x` e `y`, cada objeto `Punto` creado tendrá sus propios valores de `x` e `y`, distintos de los de otros objetos.

Cuando se dice que estos miembros son **privados**, se indica que **no pueden ser accedidos directamente desde fuera de la clase**. Es decir, otra clase no puede leer ni modificar esos atributos usando una referencia al objeto. Esta restricción es la base de la ocultación de información, ya que protege el estado interno del objeto frente a usos incorrectos o no controlados.

Un aspecto importante es que los miembros de instancia privados **no están ocultos para otras instancias de la misma clase**. Un método definido dentro de la clase puede acceder a los atributos privados de cualquier objeto de esa clase. Esto ocurre porque el modificador `private` limita el acceso por **clase**, no por objeto individual.

En resumen, los miembros de instancia privados están ocultos para el código externo a la clase, pero son accesibles desde cualquier método de la propia clase, incluso cuando dicho método trabaja con otra instancia distinta del mismo tipo.

Los miembros de instancia privados están ocultos para **otras clases**, pero no para **otras instancias de la misma clase**. Esto significa que un método de una clase puede acceder a los atributos privados de cualquier objeto de esa misma clase.

```java
public double calcularDistanciaAPunto(Punto otro) {
    double dx = this.x - otro.x;
    double dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}
```


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta
Los métodos *getter* y *setter* son métodos públicos que permiten leer y modificar, respectivamente, el valor de un atributo privado. Normalmente siguen una convención de nombres, como `getX()` o `setX(double x)`.

Su uso permite mantener los atributos ocultos y, al mismo tiempo, ofrecer un acceso controlado a ellos. De este modo, se pueden añadir validaciones o lógica adicional al leer o modificar un valor.

Gracias a los getters y setters, se refuerza la encapsulación y se facilita el mantenimiento del código sin exponer directamente los atributos.

**Ejemplo**

```java
public class Persona {
    private int edad;

    public int getEdad() {
        return edad;
    }

    public void setEdad(int edad) {
        if (edad >= 0) {
            this.edad = edad;
        }
    }
}
```

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta
Cuando se habla de **“seguridad”** en el contexto de la ocultación de información, no se hace referencia a la seguridad informática frente a ataques externos o *hackeos*. El término se utiliza en un sentido lógico y estructural.

Se refiere a la protección del estado interno de los objetos frente a usos incorrectos, accidentales o indebidos por parte del propio programa. Al restringir el acceso directo, se reduce la probabilidad de introducir errores.

Por tanto, la ocultación de información mejora la robustez y fiabilidad del software, no su seguridad frente a ataques externos.

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta
Un **miembro de instancia** pertenece a cada objeto concreto creado a partir de una clase, mientras que un **miembro de clase** pertenece a la clase en sí y es compartido por todas sus instancias. En Java, los miembros de clase se declaran con la palabra clave `static`.

Los miembros de instancia almacenan información específica de cada objeto, como las coordenadas de un punto. Los miembros de clase suelen usarse para información común, contadores o constantes.

Los miembros de clase también pueden ocultarse utilizando modificadores de visibilidad como `private`, aplicando los mismos principios de encapsulación que a los miembros de instancia.

**Ejemplo**
```java
public class Contador {
    // Miembro de clase (compartido por todos los objetos)
    private static int totalObjetos = 0;

    // Miembro de instancia (cada objeto tiene su propio valor)
    private int valor;

    public Contador(int valorInicial) {
        this.valor = valorInicial;
        totalObjetos++;
    }

    public int getValor() {
        return valor;
    }

    public static int getTotalObjetos() {
        return totalObjetos;
    }
}
```

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta
Sí, en determinados casos tiene sentido que los constructores sean privados. Esto impide que se creen instancias de la clase directamente desde fuera.

Este enfoque se utiliza, por ejemplo, en patrones de diseño como el `Singleton` o en clases que solo ofrecen métodos estáticos. También es común cuando se quiere controlar estrictamente cómo se crean los objetos mediante métodos factoría.

Por tanto, un constructor privado es una herramienta más para reforzar la encapsulación y el control sobre la creación de objetos.


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta
En Java, los miembros de clase se indican mediante la palabra clave `static`. Esto significa que el atributo o método pertenece a la clase y no a una instancia concreta.

**Ejemplo**

```java
public class Punto {
    private double x;
    private double y;

    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }

    public static double getMaxX() {
        return maxX;
    }

    public static double getMaxY() {
        return maxY;
    }
}
```
En este ejemplo, `maxX` y `maxY` son compartidos por todos los objetos `Punto` y permiten conocer los valores máximos utilizados hasta el momento.


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta
**Primero saber que:**
Un **método factoría** es un método de una clase cuya finalidad es **crear y devolver objetos** de esa clase u otras relacionadas. En lugar de usar directamente el constructor con `new`, se llama a este método para obtener la instancia, lo que permite centralizar y controlar la lógica de creación de objetos.

Este tipo de método puede aplicar reglas adicionales, inicializar atributos de forma especial, devolver subtipos distintos o reutilizar objetos existentes. Esto proporciona mayor flexibilidad y facilita el mantenimiento del código.

En Java, los métodos factoría suelen ser `static`, es decir, pertenecen a la clase y no a un objeto concreto, por lo que pueden invocarse sin necesidad de crear primero una instancia. Son una herramienta útil para reforzar la **encapsulación** y separar la creación de objetos de su uso.

**Ejemplo**
```java
public static Punto crearRedondeado(double x, double y) {
    return new Punto(Math.round(x), Math.round(y));
}
```
Sí, se ha utilizado `static` porque un método factoría pertenece a la clase y no a una instancia concreta. De este modo, puede invocarse sin necesidad de crear previamente un objeto `Punto`.

Este tipo de método permite centralizar la lógica de creación de objetos y aplicar reglas adicionales, como el redondeo de valores.


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta
```java
public class Punto {
    private double[] coordenadas = new double[2];

    public Punto(double x, double y) {
        coordenadas[0] = x;
        coordenadas[1] = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(coordenadas[0] * coordenadas[0] +
                         coordenadas[1] * coordenadas[1]);
    }
}
```
La interfaz pública no se ha modificado, ya que el constructor y el método siguen siendo los mismos. El cambio afecta únicamente a la implementación interna.

Este ejemplo ilustra una de las grandes ventajas de la encapsulación: se puede cambiar la representación interna sin afectar al código que usa la clase.


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta
Aunque un atributo tenga getter y setter públicos, no es recomendable declararlo público. Mantenerlo privado permite añadir validaciones, lógica adicional o restricciones sin cambiar la interfaz pública.

La convención más habitual en POO es declarar los atributos como privados y acceder a ellos mediante métodos. Esto refuerza la encapsulación y el control sobre el estado del objeto.

Esta práctica está directamente relacionada con las invariantes de clase, ya que permite garantizar que cualquier modificación del estado respeta las condiciones internas definidas.


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta
Una clase **inmutable** es aquella cuyos objetos no pueden cambiar su estado una vez creados. Esto significa que no existen métodos que modifiquen los atributos internos tras la construcción del objeto.

Un **método modificador** es aquel que altera el estado interno de un objeto. No siempre es un setter clásico, ya que puede modificar varios atributos o realizar cambios más complejos.

Las clases inmutables tienen ventajas importantes: son más seguras, más fáciles de razonar, evitan efectos colaterales y son especialmente útiles en entornos concurrentes.


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta
No es recomendable incluir setters de forma automática o por convención. Cada setter expone la posibilidad de modificar el estado interno del objeto y puede poner en riesgo sus invariantes.

Solo deberían incluirse setters cuando realmente tenga sentido que el estado del objeto cambie desde el exterior y cuando ese cambio pueda controlarse adecuadamente.

Un diseño cuidadoso suele preferir objetos con menos mutabilidad y con interfaces públicas más reducidas y expresivas.


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta
La clase `String` en Java es **inmutable**. Esto significa que una vez creada una cadena, su contenido no puede cambiar.

Al concatenar dos cadenas, no se modifica ninguna de las originales, sino que se crea un nuevo objeto `String`. Esto puede resultar ineficiente si se realizan muchas concatenaciones.

En esos casos, es recomendable utilizar clases como `StringBuilder` o `StringBuffer`, que permiten construir cadenas de forma mutable y más eficiente.

## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta
En POO, los objetos pueden compararse por **identidad** (si son el mismo objeto en memoria) o por **contenido** (si representan el mismo valor lógico). En Java, el operador `==` compara la identidad.

El método `equals` se utiliza para comparar el contenido lógico de los objetos. Por defecto, la implementación de `equals` en `Object` compara identidad, pero suele sobrescribirse.

Las cadenas en Java deben compararse siempre usando `equals`, ya que `==` solo comprueba si ambas referencias apuntan al mismo objeto.

**Ejemplo**
```java
public class Persona {
    private String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    // Sobrescribir equals para comparar por contenido
    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        Persona otra = (Persona) obj;
        return nombre.equals(otra.nombre);
    }
}

public class Main {
    public static void main(String[] args) {
        Persona p1 = new Persona("Ana");
        Persona p2 = new Persona("Ana");
        Persona p3 = p1;

        System.out.println(p1 == p2);      // false, no son el mismo objeto (identidad)
        System.out.println(p1.equals(p2)); // true, tienen el mismo contenido
        System.out.println(p1 == p3);      // true, p3 apunta al mismo objeto que p1
    }
}
```


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta
Las clases *wrapper* son clases que envuelven tipos primitivos para tratarlos como objetos. En Java, ejemplos de wrappers son `Integer`, `Double` o `Boolean`.

Java permite un proceso automático llamado *autoboxing* y *unboxing*, que convierte entre tipos primitivos y sus wrappers de forma transparente.

Estas clases permiten usar valores primitivos en colecciones y aprovechar características de los objetos. No todos los lenguajes tienen tipos primitivos separados; algunos tratan todo como objetos y no necesitan wrappers.

**Ejemplo**
```java
import java.util.ArrayList;

public class WrapperExample {
    public static void main(String[] args) {
        // Lista de enteros usando la clase wrapper Integer
        ArrayList<Integer> numeros = new ArrayList<>();

        // Autoboxing: el int primitivo se convierte automáticamente en Integer
        numeros.add(5);
        numeros.add(10);

        // Unboxing: el Integer se convierte automáticamente en int
        int suma = numeros.get(0) + numeros.get(1);

        System.out.println("Suma: " + suma);
    }
}
```


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta
Un **tipo enumerado** es un tipo de dato que define un conjunto finito y cerrado de valores posibles. Se utiliza cuando un valor solo puede tomar una serie concreta de opciones.

En Java, un enumerado (`enum`) es en realidad una clase especial. Puede tener atributos, métodos y constructores, aunque con ciertas restricciones.

Esto aporta grandes ventajas en encapsulación, ya que el comportamiento asociado a cada valor puede definirse dentro del propio enumerado, evitando errores y valores inválidos.


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado.

### Respuesta
```java
public enum Mes {
    ENERO(1, 31),
    FEBRERO(2, 28),
    MARZO(3, 31),
    ABRIL(4, 30),
    MAYO(5, 31),
    JUNIO(6, 30),
    JULIO(7, 31),
    AGOSTO(8, 31),
    SEPTIEMBRE(9, 30),
    OCTUBRE(10, 31),
    NOVIEMBRE(11, 30),
    DICIEMBRE(12, 31);

    private int ordinal;
    private int dias;

    private Mes(int ordinal, int dias) {
        this.ordinal = ordinal;
        this.dias = dias;
    }

    public int getDias() {
        return dias;
    }

    public int getOrdinal() {
        return ordinal;
    }
}
```

## 24. Añade a la clase `Mes` del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta
```java
public enum Mes {
    ENERO(1, 31),
    FEBRERO(2, 28),
    MARZO(3, 31),
    ABRIL(4, 30),
    MAYO(5, 31),
    JUNIO(6, 30),
    JULIO(7, 31),
    AGOSTO(8, 31),
    SEPTIEMBRE(9, 30),
    OCTUBRE(10, 31),
    NOVIEMBRE(11, 30),
    DICIEMBRE(12, 31);

    private int ordinal;
    private int dias;

    private Mes(int ordinal, int dias) {
        this.ordinal = ordinal;
        this.dias = dias;
    }

    public int getDias() {
        return dias;
    }

    public int getOrdinal() {
        return ordinal;
    }

    // Metodos que pide la pregunta
    public boolean esDePrimavera(boolean hemisferioNorte) {
        return hemisferioNorte ? (ordinal >= 3 && ordinal <= 5)
                               : (ordinal >= 9 && ordinal <= 11);
    }

    public boolean esDeVerano(boolean hemisferioNorte) {
        return hemisferioNorte ? (ordinal >= 6 && ordinal <= 8)
                               : (ordinal == 12 || ordinal <= 2);
    }

    public boolean esDeOtono(boolean hemisferioNorte) {
        return hemisferioNorte ? (ordinal >= 9 && ordinal <= 11)
                               : (ordinal >= 3 && ordinal <= 5);
    }

    public boolean esDeInvierno(boolean hemisferioNorte) {
        return hemisferioNorte ? (ordinal == 12 || ordinal <= 2)
                               : (ordinal >= 6 && ordinal <= 8);
    }
}
```