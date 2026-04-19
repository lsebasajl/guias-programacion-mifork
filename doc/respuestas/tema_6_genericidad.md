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
Una forma clásica de simular “genericidad” sin usar genéricos reales consiste en almacenar referencias de tipo genérico (como `void*` en C o `Object` en Java) dentro de una estructura basada en arrays. La idea es que el array no guarda directamente valores de un tipo concreto, sino direcciones o referencias a datos, permitiendo así almacenar cualquier tipo. Esto aporta flexibilidad, pero pierde seguridad de tipos, ya que el compilador no puede verificar qué tipo real se está almacenando o recuperando.

En C, se puede implementar una estructura que almacene punteros `void*`. Por ejemplo, un array dinámico donde cada posición guarda la dirección de un dato de cualquier tipo. El problema es que, al recuperar el dato, se debe hacer un *casting* manual al tipo original, lo cual puede provocar errores si no se controla correctamente:

```c
#include <stdio.h>

#define MAX 10

typedef struct {
    void* datos[MAX];
    int size;
} Lista;

void insertar(Lista* l, void* elemento) {
    l->datos[l->size++] = elemento;
}

int main() {
    Lista l = { .size = 0 };

    int a = 5;
    float b = 3.14;

    insertar(&l, &a);
    insertar(&l, &b);

    printf("%d\n", *(int*)l.datos[0]);
    printf("%f\n", *(float*)l.datos[1]);

    return 0;
}
```

En Java, el equivalente sería usar un array de tipo `Object`, ya que todas las clases heredan de `Object`. Esto permite almacenar cualquier objeto, aunque también obliga a realizar *casting* al recuperar los elementos. A diferencia de C, Java sí mantiene cierta seguridad en tiempo de ejecución (lanzando excepciones si el casting es incorrecto), pero no en tiempo de compilación:

```java
class Lista {
    private Object[] datos;
    private int size;

    public Lista(int capacidad) {
        datos = new Object[capacidad];
        size = 0;
    }

    public void insertar(Object elemento) {
        datos[size++] = elemento;
    }

    public Object obtener(int i) {
        return datos[i];
    }
}

public class Main {
    public static void main(String[] args) {
        Lista l = new Lista(10);

        l.insertar(5);
        l.insertar(3.14);

        int a = (int) l.obtener(0);
        double b = (double) l.obtener(1);

        System.out.println(a);
        System.out.println(b);
    }
}
```

Este enfoque permite simular contenedores genéricos, pero presenta limitaciones importantes: no hay verificación de tipos en compilación, se requiere *casting* explícito y existe riesgo de errores en ejecución. Estas desventajas son precisamente las que resuelven los genéricos en Java, proporcionando seguridad de tipos sin perder flexibilidad.


## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### Respuesta
La **programación genérica** es un paradigma que permite definir algoritmos y estructuras de datos de forma independiente del tipo concreto de los datos con los que trabajan. En lugar de escribir una versión distinta de una estructura (por ejemplo, una lista) para enteros, otra para reales, otra para objetos, etc., se define una única versión parametrizada por tipos. De esta forma, el compilador puede reutilizar el mismo código para distintos tipos, manteniendo además la seguridad de tipos en tiempo de compilación.

En lenguajes como Java, esto se materializa mediante los *genéricos* (`<T>`), que permiten especificar el tipo de dato en el momento de uso, evitando conversiones manuales (*casting*) y posibles errores en ejecución. En C++, este concepto se implementa mediante *templates*. La idea clave es que el código es más reutilizable, más seguro y más expresivo, ya que el tipo forma parte explícita de la definición de la estructura o del algoritmo.

El ejemplo anterior basado en `void*` en C o `Object` en Java **no es realmente programación genérica**, sino una simulación básica de ella. Aunque permite almacenar distintos tipos de datos en una misma estructura, no proporciona seguridad de tipos en tiempo de compilación ni evita el uso de *casting*. Por tanto, se pierde una de las principales ventajas de la programación genérica.

En consecuencia, dicho ejemplo puede considerarse un antecedente o una aproximación rudimentaria, pero no una implementación real de programación genérica. La verdadera programación genérica implica que el compilador conozca y controle los tipos, algo que no ocurre cuando se utilizan `void*` u `Object` de forma directa.

**Ejemplo**
Una implementación “real” de programación genérica en Java se basa en el uso de **parámetros de tipo** (`<T>`), lo que permite definir estructuras de datos independientes del tipo concreto pero manteniendo seguridad en compilación. En este caso, en lugar de usar `Object`, se define una clase parametrizada, de forma que el tipo se especifica al crear la estructura. Así, el compilador puede comprobar que solo se insertan y recuperan elementos del tipo correcto, eliminando la necesidad de *casting*.

Por ejemplo, se puede definir una lista genérica basada en arrays de la siguiente forma. Aunque internamente Java no permite crear directamente arrays de tipo genérico, se puede usar un array de `Object` y encapsularlo correctamente para mantener la seguridad de tipos hacia el exterior:

```java
class Lista<T> {
    private Object[] datos;
    private int size;

    public Lista(int capacidad) {
        datos = new Object[capacidad];
        size = 0;
    }

    public void insertar(T elemento) {
        datos[size++] = elemento;
    }

    public T obtener(int i) {
        return (T) datos[i];
    }
}
```

El uso de esta clase muestra la principal ventaja frente al uso de `Object`: el tipo queda fijado en el momento de instanciación, y el compilador controla su uso. Por ejemplo:

```java
public class Main {
    public static void main(String[] args) {
        Lista<Integer> listaEnteros = new Lista<>(10);
        listaEnteros.insertar(5);
        listaEnteros.insertar(10);

        int a = listaEnteros.obtener(0); // sin casting explícito

        Lista<String> listaStrings = new Lista<>(10);
        listaStrings.insertar("Hola");
        listaStrings.insertar("Mundo");

        String s = listaStrings.obtener(1);
    }
}
```

En este enfoque, no es posible insertar un `String` en una lista de `Integer`, ya que el error se detectaría en tiempo de compilación. Esto convierte a la programación genérica en una solución más segura y robusta que el uso de `Object`, al tiempo que mantiene la flexibilidad y reutilización del código.


## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### Respuesta
El uso de `void*` en C o `Object` en Java para simular estructuras genéricas presenta un problema fundamental: **se pierde el chequeo de tipos en tiempo de compilación**. Al almacenar cualquier tipo de dato en la misma estructura, el compilador no puede verificar si las operaciones realizadas sobre esos datos son correctas. Esto implica que errores como insertar un tipo inesperado o recuperar un dato con un tipo incorrecto no se detectan hasta la ejecución (o incluso nunca, en C), lo que reduce la fiabilidad del programa.

Otro inconveniente importante es la necesidad de realizar *casting* explícito al recuperar los datos. Este proceso depende completamente del programador, que debe recordar el tipo real de cada elemento almacenado. Si se realiza un *casting* incorrecto, en Java se producirá una excepción en tiempo de ejecución (`ClassCastException`), mientras que en C el comportamiento puede ser indefinido, pudiendo provocar fallos graves o resultados erróneos sin aviso.

Además, este enfoque dificulta la legibilidad y el mantenimiento del código. Al no estar los tipos claramente especificados, resulta más complicado entender qué tipo de datos se espera en cada estructura y cómo deben utilizarse. Esto incrementa la probabilidad de errores al modificar o ampliar el código, especialmente en programas grandes o desarrollados por varios programadores.

En consecuencia, el principal problema es que se sacrifica la seguridad de tipos que ofrece el lenguaje. Frente a esto, la programación genérica real (como los genéricos en Java) permite mantener la flexibilidad sin renunciar al control estático de tipos, evitando muchos de estos errores antes de que el programa se ejecute.


## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### Respuesta
Los **parámetros de tipo** son un mecanismo que permite definir clases, interfaces o métodos de forma **parametrizada por tipos**, es decir, sin fijar de antemano el tipo concreto de los datos con los que van a trabajar. En lugar de usar un tipo genérico como `Object`, se introduce un “marcador” (habitualmente `T`, `E`, `K`, `V`, etc.) que será sustituido por un tipo real cuando se utilice la clase o método. Esto permite escribir código reutilizable sin perder información de tipos.

En Java, los parámetros de tipo se declaran entre los símbolos `< >`. Por ejemplo, en una clase `Caja<T>`, la `T` representa un tipo cualquiera que será especificado al crear el objeto. De esta forma, el compilador puede comprobar que solo se usan valores del tipo adecuado, evitando errores y eliminando la necesidad de *casting* explícito al recuperar datos.

```java
class Caja<T> {
    private T valor;

    public void guardar(T valor) {
        this.valor = valor;
    }

    public T obtener() {
        return valor;
    }
}
```

El uso sería el siguiente:

```java
Caja<Integer> cajaInt = new Caja<>();
cajaInt.guardar(10);
int x = cajaInt.obtener();
```

Gracias a los parámetros de tipo, se consigue combinar **flexibilidad y seguridad de tipos**. A diferencia del uso de `Object`, el compilador conoce el tipo concreto en cada uso y puede detectar errores en tiempo de compilación, lo que hace que el código sea más robusto, claro y fácil de mantener.


## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta
En Java, la programación genérica se implementa mediante **generics**, que permiten parametrizar clases como `ArrayList<T>`. Al indicar `String` como parámetro de tipo, se garantiza en tiempo de compilación que solo se podrán insertar cadenas, y que al recuperarlas no será necesario hacer *casting*. Esto asegura tanto la corrección del programa como la claridad del código.

```java
import java.util.ArrayList;

public class Main {
    public static void main(String[] args) {
        ArrayList<String> lista = new ArrayList<>();

        lista.add("Hola");
        lista.add("Mundo");
        lista.add("Genéricos");

        for (String s : lista) {
            System.out.println(s.toUpperCase()); // s es String con seguridad
        }
    }
}
```

En este caso, si se intentara añadir un entero (`lista.add(5);`), el compilador produciría un error. Además, al recorrer la lista, cada elemento ya es de tipo `String`, por lo que se pueden usar directamente sus métodos sin conversiones explícitas.

En C++, el equivalente son los **templates**, que permiten definir estructuras genéricas como `std::vector<T>`. Al instanciar el vector con `std::string`, se obtiene una estructura que solo admite ese tipo, con comprobación en compilación y sin pérdida de eficiencia. A diferencia de Java, en C++ los templates se resuelven completamente en compilación, generando código específico para cada tipo.

```cpp
#include <iostream>
#include <vector>
#include <string>

int main() {
    std::vector<std::string> lista;

    lista.push_back("Hola");
    lista.push_back("Mundo");
    lista.push_back("Templates");

    for (const std::string& s : lista) {
        std::cout << s << std::endl; // s es std::string con seguridad
    }

    return 0;
}
```

En ambos lenguajes, se observa que la programación genérica permite crear estructuras reutilizables sin renunciar al control de tipos. Tanto `generics` en Java como `templates` en C++ evitan errores comunes asociados al uso de tipos genéricos sin control (como `Object` o `void*`), garantizando que cada elemento del contenedor es del tipo especificado (`String` en Java y `std::string` en C++).


## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta
Cuando se instancia una clase con parámetros de tipo, el compilador utiliza esa información para **comprobar que los tipos son correctos** y adaptar el código genérico al tipo concreto indicado. Es decir, si se tiene una clase `Lista<T>` y se crea `Lista<String>`, el compilador verifica que solo se usan `String` en los métodos correspondientes. Sin embargo, la forma en la que esto se implementa internamente **no es igual en todos los lenguajes**.

En Java, el compilador aplica un mecanismo llamado **“type erasure” (borrado de tipos)**. Esto significa que, tras realizar las comprobaciones en tiempo de compilación, el tipo genérico (`T`) se elimina y se sustituye por su límite superior (normalmente `Object`). Por tanto, en tiempo de ejecución no existe información sobre el tipo genérico concreto (`String`, `Integer`, etc.). El compilador añade automáticamente los *casting* necesarios donde hacen falta. Esto explica por qué no se pueden crear arrays de tipos genéricos directamente o por qué no se puede hacer `new T()`.

En cambio, en C++, el compilador utiliza la **instanciación de plantillas (template instantiation)**. En este caso, cuando se usa un template con un tipo concreto (por ejemplo, `std::vector<std::string>`), el compilador genera **una versión específica del código para ese tipo**. Es decir, crea una “copia” de la clase o función adaptada a `std::string`. Esto ocurre completamente en tiempo de compilación, y el tipo concreto sí está presente en el código generado final.

Por tanto, la diferencia clave es que Java reutiliza una única implementación genérica mediante borrado de tipos, mientras que C++ genera implementaciones específicas para cada tipo usado. El enfoque de Java reduce la duplicación de código en el binario, pero pierde información de tipos en ejecución; el de C++ mantiene toda la información de tipos y puede ser más eficiente, aunque puede aumentar el tamaño del código generado.


## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

### Respuesta
Se puede definir una clase genérica con **dos parámetros de tipo** para representar un par de valores potencialmente distintos. En este caso, se suelen usar identificadores como `T` y `U` para indicar que cada componente puede tener un tipo diferente. La clase encapsula ambos valores y proporciona un constructor junto con métodos *getter* para acceder a ellos, manteniendo la seguridad de tipos en todo momento.

```java
class Par<T, U> {
    private T primero;
    private U segundo;

    public Par(T primero, U segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public T getPrimero() {
        return primero;
    }

    public U getSegundo() {
        return segundo;
    }
}
```

A partir de esta clase, se puede construir una función que calcule la media y la desviación típica de un array de `double` y devuelva ambos resultados agrupados en un objeto `Par<Double, Double>`. De este modo, se evita crear una clase específica para el resultado, reutilizando la estructura genérica.

```java
public class Estadisticas {

    public static Par<Double, Double> calcular(double[] datos) {
        double suma = 0.0;
        for (double d : datos) {
            suma += d;
        }
        double media = suma / datos.length;

        double sumaCuadrados = 0.0;
        for (double d : datos) {
            sumaCuadrados += Math.pow(d - media, 2);
        }
        double desviacion = Math.sqrt(sumaCuadrados / datos.length);

        return new Par<>(media, desviacion);
    }

    public static void main(String[] args) {
        double[] valores = {1.0, 2.0, 3.0, 4.0, 5.0};

        Par<Double, Double> resultado = calcular(valores);

        System.out.println("Media: " + resultado.getPrimero());
        System.out.println("Desviación típica: " + resultado.getSegundo());
    }
}
```

En este ejemplo, el uso de genéricos permite que el tipo del par sea conocido en tiempo de compilación (`Double, Double`), evitando conversiones y posibles errores. Además, la clase `Par` es reutilizable para cualquier combinación de tipos, lo que demuestra la flexibilidad de la programación genérica.


## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

### Respuesta
En Java, los métodos también pueden declarar **parámetros de tipo propios**, independientes de la clase en la que se definen. Esto permite escribir funciones genéricas sin necesidad de que toda la clase sea genérica. Un ejemplo típico sería un método `seleccionaUno` que, dados dos objetos, devuelve uno de ellos aleatoriamente. Si se implementa usando `Object`, el método pierde información de tipos y obliga al uso de *casting* al recuperar el resultado.

```java
import java.util.Random;

class Util {

    public static Object seleccionaUno(Object a, Object b) {
        Random r = new Random();
        return r.nextBoolean() ? a : b;
    }

    public static void main(String[] args) {
        Object res = seleccionaUno("Hola", "Mundo");
        String s = (String) res; // requiere casting
    }
}
```

En este caso, (i) es necesario hacer *downcasting* al tipo esperado (`String`), lo cual puede provocar errores en ejecución si el tipo no coincide. Además, (ii) no se fuerza que ambos parámetros sean del mismo tipo: se podría llamar al método con `"Hola"` y `5`, lo que incrementa el riesgo de errores.

La versión genérica del método soluciona ambos problemas mediante un parámetro de tipo `<T>`. De esta forma, el compilador garantiza que ambos argumentos son del mismo tipo y que el valor devuelto también lo es, eliminando la necesidad de *casting*.

```java
import java.util.Random;

class Util {

    public static <T> T seleccionaUno(T a, T b) {
        Random r = new Random();
        return r.nextBoolean() ? a : b;
    }

    public static void main(String[] args) {
        String s = seleccionaUno("Hola", "Mundo"); // sin casting

        // seleccionaUno("Hola", 5); // error de compilación
    }
}
```

Con este enfoque, (i) se evita completamente el *downcasting*, ya que el compilador conoce el tipo de retorno, y (ii) se obliga a que ambos argumentos sean del mismo tipo (`T`). Esto mejora la seguridad y claridad del código, aprovechando plenamente las ventajas de la programación genérica frente al uso de `Object`.


## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta
Sí, en Java se pueden establecer **restricciones sobre los parámetros de tipo** mediante *bounded types*. Esto se hace usando la palabra clave `extends`, indicando que el tipo genérico debe ser una subclase de otro (por ejemplo, `Number`). De esta forma, el compilador garantiza que los objetos de ese tipo disponen de ciertos métodos (como `doubleValue()`), lo que permite tratarlos como números sin perder completamente la seguridad de tipos.

Una primera solución, sin genéricos, consiste en definir las coordenadas directamente como `Number`. Esto permite almacenar cualquier tipo numérico (`Integer`, `Double`, etc.), pero no distingue entre ellos ni evita mezclas de tipos:

```java
class Punto {
    private Number x;
    private Number y;

    public Punto(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() {
        return x;
    }

    public Number getY() {
        return y;
    }

    public double calcularDistanciaA(Punto otro) {
        double dx = this.x.doubleValue() - otro.x.doubleValue();
        double dy = this.y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

En este caso, se puede crear un punto con `Integer` y otro con `Double`, y el compilador no lo impedirá. Aunque funcional, no hay control fino sobre el tipo concreto de número utilizado.

La segunda solución utiliza genéricos con restricción (`<T extends Number>`), lo que permite mantener la flexibilidad pero obligando a que ambas coordenadas sean del mismo tipo numérico:

```java
class Punto<T extends Number> {
    private T x;
    private T y;

    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() {
        return x;
    }

    public T getY() {
        return y;
    }

    public double calcularDistanciaA(Punto<T> otro) {
        double dx = this.x.doubleValue() - otro.x.doubleValue();
        double dy = this.y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

Aquí, el compilador impide mezclar tipos distintos: un `Punto<Integer>` solo puede operar con otro `Punto<Integer>`. Además, se conoce el tipo concreto en cada uso, lo que mejora la seguridad y claridad del código.

Respecto al **type erasure**, en ambos casos el tipo genérico `T` se elimina en tiempo de compilación y se sustituye por su límite superior, es decir, `Number`. Por tanto, tras la compilación, la versión genérica se comporta internamente como si utilizara `Number`, aunque en tiempo de compilación se haya beneficiado del chequeo de tipos más estricto.


## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta
En la solución sin genéricos, donde las coordenadas se declaran como `Number`, sí es posible asignar tipos distintos a cada coordenada. Es decir, puede crearse un `Punto` con `Integer` en `x` y `Double` en `y` sin que el compilador lo impida, ya que ambos tipos son subtipos de `Number`. Esto aporta flexibilidad, pero reduce el control sobre la consistencia del tipo de los datos.

```java
Punto p = new Punto(3, 4.5); // Integer y Double mezclados
```

En este caso, el método `getX()` devuelve un `Number`, independientemente de si internamente es un `Integer`, `Double` u otro subtipo. Por tanto, el tipo de retorno es siempre genérico en tiempo de compilación, y es el programador quien debe realizar conversiones si necesita un tipo más específico.

```java
Number x = p.getX();
```

En cambio, en la solución con genéricos (`Punto<T extends Number>`), el tipo `T` se fija en el momento de la creación del objeto. Esto implica que ambas coordenadas deben ser del mismo tipo concreto, por lo que no se permite mezclar `Integer` y `Double` en el mismo punto. El compilador fuerza esta coherencia, reforzando el chequeo de tipos.

```java
Punto<Integer> p1 = new Punto<>(3, 4);   // correcto
// Punto<Integer> p2 = new Punto<>(3, 4.5); // error de compilación
```

En este caso, `getX()` devuelve exactamente el tipo `T`, es decir, el tipo concreto con el que se ha instanciado la clase. Por ejemplo:

```java
Integer x = p1.getX();
```

Por tanto, la diferencia clave es que en la versión sin genéricos el retorno es siempre `Number` (menos específico y con necesidad potencial de conversiones), mientras que en la versión con genéricos el retorno es el tipo concreto `T`, lo que elimina *casting* y refuerza el control de tipos en compilación.


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
El problema del diseño original es que la interfaz `Punto` no impone ningún tipo sobre el propio punto que recibe el método `distanciaA`. Esto obliga a comprobar en ejecución si el objeto es de tipo `Punto2D` o `Punto3D`, usando `instanceof`, y posteriormente a realizar *downcasting*. Este enfoque rompe el polimorfismo y elimina el chequeo de tipos en compilación, trasladando los errores al tiempo de ejecución.

Una solución con **generics** consiste en parametrizar la interfaz con el tipo del propio punto. De esta forma, se garantiza que cada implementación solo puede calcular distancia con puntos del mismo tipo, eliminando la necesidad de comprobaciones dinámicas.

```java
public interface Punto<T> {
    double distanciaA(T p);
}
```

A partir de aquí, cada clase concreta fija su propio tipo como parámetro de la interfaz. Esto asegura coherencia entre el tipo de la clase y el tipo del parámetro del método.

```java
public class Punto2D implements Punto<Punto2D> {
    private final double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double distanciaA(Punto2D p) {
        return Math.sqrt(Math.pow(x - p.x, 2)
                       + Math.pow(y - p.y, 2));
    }
}
```

De forma análoga, la clase 3D queda completamente separada y tipada:

```java
public class Punto3D implements Punto<Punto3D> {
    private final double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double distanciaA(Punto3D p) {
        return Math.sqrt(Math.pow(x - p.x, 2)
                       + Math.pow(y - p.y, 2)
                       + Math.pow(z - p.z, 2));
    }
}
```

Con este diseño, el compilador garantiza que no es posible llamar `distanciaA` entre tipos incompatibles:

```java
Punto2D a = new Punto2D(1, 2);
Punto2D b = new Punto2D(3, 4);
a.distanciaA(b); // correcto

Punto3D c = new Punto3D(1, 2, 3);
// a.distanciaA(c); // ERROR de compilación
```

En esta versión, (i) se elimina completamente el `instanceof` y el *downcasting*, ya que el tipo correcto está garantizado en compilación, y (ii) se refuerza el polimorfismo de forma segura: cada implementación trabaja exclusivamente con su propio tipo de punto. Esto hace el diseño más robusto, claro y alineado con la programación genérica real.


## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta
No, **`List<String>` no es subtipo de `List<Object>`**, aunque `String` sí lo sea de `Object`. En cambio, **`String[] sí es subtipo de `Object[]`**. Esta diferencia se debe a cómo Java trata la seguridad de tipos en genéricos frente a arrays.

En el caso de los **arrays**, Java los considera *covariantes*. Es decir, si `String` es subtipo de `Object`, entonces `String[]` también lo es de `Object[]`. Esto permite escribir cosas como:

```java
Object[] arr = new String[3];
arr[0] = "Hola";
arr[1] = 10; // error en tiempo de ejecución
```

Aquí aparece un problema importante: el error no se detecta en compilación, sino en ejecución, lanzando una `ArrayStoreException`. Esto ocurre porque el array real sigue siendo de `String[]`, y no puede almacenar un `Integer`, aunque la referencia sea `Object[]`. Por tanto, la covarianza en arrays puede romper la seguridad de tipos en tiempo de ejecución.

En cambio, en los **genéricos de Java**, como `List<T>`, el sistema es *invariante*. Esto significa que aunque `String` sea subtipo de `Object`, **`List<String>` no es subtipo de `List<Object>`**. Esto evita exactamente el problema anterior:

```java
List<Object> lista = new ArrayList<String>(); // ERROR de compilación
```

Esta restricción es intencionada: si se permitiera, se podría hacer lo siguiente:

```java
List<String> strings = new ArrayList<>();
List<Object> objs = strings;   // (si fuera válido)
objs.add(10);                  // rompería la lista de Strings
```

Esto muestra que permitir la relación subtipada entre contenedores rompería la seguridad de tipos.

A partir de esto se definen tres conceptos:

* **Covarianza**: si `A` es subtipo de `B`, entonces `C<A>` es subtipo de `C<B>`. Permite “leer” tipos más específicos desde estructuras más generales. Ejemplo: arrays en Java (`String[] → Object[]`), aunque es inseguro en escritura.

* **Contravarianza**: si `A` es subtipo de `B`, entonces `C<B>` es subtipo de `C<A>`. Se usa cuando el tipo solo se “consume” (entrada). En Java se expresa con `? super T`.

* **Invarianza**: no existe relación entre `C<A>` y `C<B>` aunque `A` sea subtipo de `B`. Es el caso de los genéricos en Java (`List<T>`), y garantiza máxima seguridad de tipos evitando mezclas peligrosas.

En resumen, los arrays son covariantes y pueden producir errores en tiempo de ejecución, mientras que los genéricos son invariantes para garantizar seguridad en compilación.


## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta
Un **wildcard (`?`)** en Java es un *comodín* que se usa en tipos genéricos para indicar “algún tipo desconocido”, permitiendo flexibilizar el sistema de tipos sin perder del todo la seguridad. Se utiliza cuando no interesa fijar exactamente el parámetro de tipo, sino imponer restricciones sobre él.

La diferencia entre `List<? extends T>` y `List<? super T>` está en cómo se permite usar la lista:

* `List<? extends T>` (**covarianza acotada**) significa “una lista de algún subtipo de T”. Se usa cuando se quiere **leer** valores de la estructura, pero no modificarla de forma segura, ya que no se conoce el tipo exacto.
* `List<? super T>` (**contravarianza acotada**) significa “una lista de algún supertipo de T”. Se usa cuando se quiere **escribir** valores de tipo T en la estructura, garantizando que siempre son compatibles.

---

### (i) Suma de números con `? extends`

Aquí se recibe una lista de cualquier subtipo de `Number` (`Integer`, `Double`, etc.), pero solo se necesita leer sus valores:

```java
import java.util.List;

class Util {

    public static double sumar(List<? extends Number> lista) {
        double suma = 0.0;

        for (Number n : lista) {
            suma += n.doubleValue();
        }

        return suma;
    }
}
```

En este caso se usa `? extends Number` porque interesa aceptar listas de distintos tipos numéricos, pero solo para **leerlos**. No se puede añadir elementos a la lista (excepto `null`), ya que el tipo concreto no es conocido.

---

### (ii) Añadir números con `? super`

Aquí se recibe una lista donde se pueden insertar enteros de forma segura, ya que la lista está garantizada como supertipo de `Integer`:

```java
import java.util.List;

class Util {

    public static void añadirEnteros(List<? super Integer> lista) {
        lista.add(1);
        lista.add(2);
        lista.add(3);
    }
}
```

En este caso se usa `? super Integer` porque interesa **escribir** valores `Integer` en la lista. Puede ser una `List<Integer>`, `List<Number>` o `List<Object>`, pero siempre es seguro añadir `Integer`.

---

### Resumen conceptual

* `? extends T` → **solo lectura segura** (covarianza)
* `? super T` → **solo escritura segura** (contravarianza)
* Regla práctica clásica: **PECS (Producer Extends, Consumer Super)**

Esto permite recuperar parte de la flexibilidad de la covarianza y contravarianza en Java, manteniendo la seguridad de tipos que se pierde en estructuras completamente covariantes como los arrays.
