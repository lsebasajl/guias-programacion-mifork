<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Respuesta
La programación orientada a objetos (POO) se basa en cuatro características principales: **encapsulamiento, herencia, polimorfismo y abstracción**.  

El **encapsulamiento** consiste en agrupar datos y funciones que manipulan esos datos dentro de una misma unidad, llamada clase, y en restringir el acceso directo a los datos desde fuera de la clase. Esto permite proteger la información y controlar cómo se modifica, esto quiere decir que por ejemplo variables y metodos no puedes ser accesibles desde calquier punto del código.

La **herencia** permite crear nuevas clases basadas en otras existentes (clases padres), aprovechando sus atributos y métodos sin necesidad de reescribirlos. Esto facilita la reutilización de código y la creación de jerarquías de clases.

El **polimorfismo** hace posible que un mismo método o función pueda comportarse de manera distinta según el objeto que lo invoque. Esto se logra, por ejemplo, mediante la redefinición de métodos en clases derivadas.

La **abstracción** consiste en definir únicamente los aspectos esenciales de un objeto, ignorando los detalles internos, de manera que se pueda trabajar con conceptos generales sin preocuparse por la implementación concreta.

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta
Algunos lenguajes populares que soportan programación orientada a objetos son **Java, C++, Python y C#**.  

Java es un lenguaje puramente orientado a objetos, en el que prácticamente todo se define mediante clases y objetos. C++ combina la programación estructurada con la orientada a objetos, permitiendo elegir el estilo según la necesidad. Python es un lenguaje interpretado que facilita la creación de clases y objetos de manera flexible, con una sintaxis sencilla. C# está diseñado para la plataforma .NET y sigue de manera estricta los principios de la POO, incluyendo encapsulamiento, herencia y polimorfismo.


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta
La **programación estructurada** es un paradigma en el que los programas se organizan mediante secuencias de instrucciones, estructuras de control (como bucles y condicionales) y subrutinas. Su objetivo principal es evitar el uso excesivo de saltos incondicionales y facilitar la comprensión del flujo del programa.

La **programación modular** es una evolución de la estructurada que propone dividir un programa en módulos independientes o funciones bien definidas, cada uno con una responsabilidad concreta. Esto facilita la reutilización, el mantenimiento y la depuración, ya que cada módulo puede desarrollarse y probarse de manera aislada, sin que pueda interferir directamente en el programa principal.

Ambos paradigmas se centran en la organización del código, pero carecen de conceptos como objetos, clases o herencia que caracterizan a la POO. Sin embargo, la programación modular sentó las bases para estructurar el código de manera que después pueda integrarse en clases y objetos.


## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta
Un objeto en programación orientada a objetos se define por **atributos, métodos y estado**.  

Los **atributos** son las propiedades o características que describen al objeto. Por ejemplo, un objeto de tipo `Coche` podría tener atributos como `color`, `marca` y `modelo`. Los **métodos** son las funciones que definen el comportamiento del objeto; en el caso del `Coche`, métodos como `arrancar()` o `acelerar()`.  

El **estado** de un objeto es la combinación de los valores de sus atributos en un momento determinado, es decir, cuando instanciamos una clase y le asignamos sus valores iniciales. Cada objeto puede tener un estado distinto aunque comparta la misma clase, lo que permite que cada instancia funcione de manera independiente de las demás.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta
Una **clase** es un modelo o plantilla que define la estructura y comportamiento de un conjunto de objetos similares. Incluye atributos y métodos, pero no representa un objeto concreto en sí mismo. Tambien se puede entender como el esqueleto de algo del mundo real.  

Un **objeto** es una entidad concreta creada a partir de una clase; se le conoce también como **instancia** de esa clase. Mientras que la clase define cómo es y qué puede hacer, el objeto es el ejemplar real que existe en memoria y que puede interactuar con otros objetos, y que ademas, es totalmente independiente a otras instancias, cada instancia es unica.

No todos los lenguajes orientados a objetos requieren explícitamente el concepto de clase. Por ejemplo, en JavaScript los objetos pueden crearse directamente sin clases, utilizando prototipos. En cambio, en Java o C# la clase es un elemento obligatorio para definir un objeto.


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta
Los objetos normalmente se almacenan en la **memoria dinámica** (heap), mientras que las variables locales y llamadas a funciones se ubican en la **memoria stack**. Esto permite que los objetos y algunos datos tengan un tiempo de vida más largo y no se destruyan automáticamente al salir de un bloque de código.  

No todos los lenguajes manejan la memoria de la misma manera. Por ejemplo, en C++ es necesario gestionar manualmente la memoria con `new` y `delete`, mientras que en Java y Python la gestión de memoria es automática y los objetos que ya no se utilizan se liberan mediante un proceso llamado **recolección de basura (garbage collection)**.  

La **recolección de basura** consiste en detectar objetos que ya no tienen referencias activas y liberar automáticamente la memoria que ocupaban, evitando fugas de memoria y simplificando la gestión del programa.

### **Extra**
### Memoria en Programación Orientada a Objetos

### Stack (Pila)
- Almacena **variables locales** y llamadas a funciones.
- Funciona como **LIFO** (Last In, First Out).
- Memoria se libera **automáticamente** al salir del bloque o función.
- Muy **rápida**, pero de **vida corta** y tamaño limitado.

### Heap (Montículo/Dinámica)
- Almacena **objetos y datos de larga duración**.
- Acceso más **lento** que la stack.
- Vida **independiente del bloque de código** donde se creó.
- En Java, la memoria se libera automáticamente mediante **recolección de basura**.
- Permite que varios objetos existan simultáneamente y mantengan su estado.

### Diferencia clave
- **Stack:** temporal, rápida, limitada.
- **Heap:** larga duración, más flexible, compartible entre funciones y objetos.


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta
Un **método** es una función definida dentro de una clase que describe un comportamiento que pueden realizar los objetos de esa clase. Permite que los objetos actúen sobre sus propios atributos o interactúen con otros objetos. Por ejemplo, un método `mover()` en una clase `Robot` podría modificar la posición del robot en un espacio virtual.  

La **sobrecarga de métodos** es la capacidad de definir varios métodos con el mismo nombre dentro de una misma clase, pero que se diferencien por el número o tipo de parámetros. Esto permite que un objeto pueda ejecutar comportamientos similares adaptados a distintas situaciones. Por ejemplo, un método `imprimir()` podría aceptar tanto un texto como un número, ejecutando acciones ligeramente diferentes según el tipo de dato recibido.  

La sobrecarga facilita la legibilidad y reutilización del código, evitando la necesidad de inventar nombres diferentes para funciones que conceptualmente realizan la misma operación.

Para esto hay que saber muy bien que metodo estamos llamando y con que parametros, para no tener errores que quizas se puede pasar por alto.

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta
```java
// Definición de la clase Punto
class Punto {
    double x;  // atributo x
    double y;  // atributo y

    // Constructor para inicializar x e y
    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Método que calcula la distancia al origen (0,0)
    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

// Clase principal para probar Punto
public class Main {
    public static void main(String[] args) {
        Punto p = new Punto(3, 4);             // Crear instancia con constructor
        System.out.println(p.calculaDistanciaAOrigen()); // Mostrar distancia
    }
}
```


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta
El **punto de entrada** en un programa en Java es siempre el método `main`, que tiene la firma:  

```java
public static void main(String[] args)
```

Cuando se ejecuta un programa, la máquina virtual de Java comienza la ejecución por el método `main`, que sirve como **puerta de entrada** a toda la aplicación. Sin un método `main` definido correctamente, el programa no puede iniciarse.  

La palabra clave `static` indica que el método o atributo pertenece a la **clase** y no a una instancia u objeto concreto. Esto significa que se puede usar directamente con el **nombre de la clase**, sin crear un objeto. Por eso el método `main` es `static`, porque la JVM necesita ejecutarlo antes de que exista cualquier objeto de la clase.  

`static` no se usa sólo en `main`; también puede aplicarse a **atributos y métodos comunes a toda la clase**, como contadores o utilidades que no dependen de los objetos.  

Cuando se combina `static` con `final`, se obtiene un **valor o método que pertenece a la clase y no puede modificarse**. Por ejemplo, se usan `static final` para **constantes**, de manera que todos los objetos de la clase compartan el mismo valor y este no pueda cambiar durante la ejecución.


## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta
En Java, los programas se escriben en archivos con extensión `.java`. Para **compilar** un programa desde la línea de comandos se utiliza el comando `javac`, seguido del nombre del archivo:  

```bash
javac MiPrograma.java
```

Este comando genera un archivo con extensión `.class`, que contiene el **byte-code**, un código intermedio que no es directamente ejecutable por el procesador, pero que puede ser interpretado por la **Java Virtual Machine (JVM)**. Para ejecutar el programa, se usa el comando `java` seguido del nombre de la clase principal sin extensión:

```bash
java MiPrograma
```

Java es un lenguaje **compilado e interpretado**. Se compila a **byte-code** mediante `javac`, y luego ese byte-code se interpreta o ejecuta por la **JVM**, que actúa como una capa entre el programa y el sistema operativo. Esto permite que un mismo programa Java se ejecute en cualquier sistema que tenga instalada la JVM, sin modificar el código fuente.

El **byte-code** es un conjunto de instrucciones de bajo nivel que la JVM entiende, mientras que los ficheros `.class` son los que contienen ese byte-code. En otras palabras, el `.class` es el equivalente al archivo objeto en C/C++, pero portable entre sistemas gracias a la máquina virtual.


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta
En Java, la palabra clave `new` se utiliza para **crear una nueva instancia de un objeto**. Cuando se ejecuta `new Punto()`, por ejemplo, se reserva memoria en el heap para ese objeto y se inicializan sus atributos, devolviendo una referencia que se guarda en la variable que lo apunta. En C/C++ esto sería similar a usar `malloc` para reservar memoria, pero en Java `new` también llama automáticamente al constructor de la clase.  

Un **constructor** es un método especial de una clase que se ejecuta **exactamente cuando se crea un objeto** y sirve para inicializar sus atributos o realizar tareas de configuración inicial. Su nombre debe coincidir exactamente con el nombre de la clase y **no tiene tipo de retorno**, ni siquiera `void`.  

Por ejemplo, en una clase `Empleado` con atributos `DNI`, `nombre` y `apellidos`, un constructor podría definirse así:  

```java
class Empleado {
    String DNI;
    String nombre;
    String apellidos;

    // Constructor
    Empleado(String DNI, String nombre, String apellidos) {
        this.DNI = DNI;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}
```

Con esto, se puede crear un objeto `Empleado` y asignarle sus datos directamente:

```java
Empleado e = new Empleado("12345678A", "Juan", "Pérez");
```

El constructor garantiza que el objeto `e` ya tenga sus atributos inicializados al momento de crearse, evitando asignaciones posteriores separadas.


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta
## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta

En Java, la referencia **`this`** representa al **objeto actual** desde el que se está ejecutando un método o constructor. Permite distinguir entre los atributos de la clase y los parámetros locales cuando tienen el mismo nombre, y también sirve para pasar la propia instancia como argumento a otros métodos o constructores.  

No todos los lenguajes orientados a objetos usan `this` con el mismo nombre. Por ejemplo, en C++ se utiliza también `this`, mientras que en Python se usa `self`, y en JavaScript `this` funciona de manera similar pero su valor puede cambiar según el contexto de llamada.  

Un ejemplo sencillo de `this` en la clase `Punto` sería el siguiente:  

```java
class Punto {
    double x;
    double y;

    // Constructor usando 'this' para diferenciar atributos y parámetros
    Punto(double x, double y) {
        this.x = x; // atributo x = parámetro x
        this.y = y; // atributo y = parámetro y
    }

    double calculaDistanciaAOrigen() {
        // También se podría usar 'this.x' y 'this.y', aunque no es obligatorio aquí
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }
}

// Uso
Punto p = new Punto(3, 4);
System.out.println(p.calculaDistanciaAOrigen()); // Imprime 5.0
```

En este ejemplo, `this.x` y `this.y` hacen referencia a **los atributos del objeto** `p`, mientras que los nombres `x` e `y` por sí solos corresponden a los **parámetros del constructor**. Sin `this`, Java no podría distinguirlos y daría un conflicto de nombres.


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta
```java
// Nuevo método: distancia a otro punto
double distanciaA(Punto otro) {
    double dx = this.x - otro.x;
    double dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}

//Uso
public class Main {
    public static void main(String[] args) {
        Punto p1 = new Punto(3, 4);
        Punto p2 = new Punto(6, 8);

        System.out.println("Distancia al origen de p1: " + p1.calculaDistanciaAOrigen());
        System.out.println("Distancia entre p1 y p2: " + p1.distanciaA(p2));
    }
}
```

### Explicación rápida

- `distanciaA` recibe un objeto `Punto` llamado `otro`.  
- Se calcula la diferencia en `x` (`dx`) y en `y` (`dy`) entre `this` y `otro`.  
- Se aplica la fórmula de la distancia:  
- Se aplica la fórmula de la distancia: `distancia = sqrt(dx*dx + dy*dy)`  
- `this` hace referencia al **objeto que llama al método** (por ejemplo, `p1`), mientras que `otro` es el punto pasado como argumento (por ejemplo, `p2`).  
- Esto permite calcular la distancia entre **cualquier par de puntos** de manera sencilla y reutilizando la misma clase.


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta
En Java, los **objetos**, como un `Punto`, se pasan a los métodos **por valor de la referencia**. Esto significa que se pasa una copia de la referencia al objeto, no el objeto en sí. Por eso, si dentro del método se modifica un atributo del objeto apuntado por esa referencia, los cambios **afectan al objeto original** fuera del método, porque ambos apuntan al mismo objeto en memoria.  

Por ejemplo, si se recibe un `Punto` y se cambia `p.x = 10` dentro del método, la instancia original que se pasó verá ese cambio reflejado. Sin embargo, si dentro del método se asigna la referencia a un **nuevo objeto**, eso no afecta al objeto original fuera del método, porque solo se está cambiando la copia de la referencia local.  

En cambio, los tipos primitivos como `int`, `double` o `boolean` se pasan **por valor**, es decir, se copia el valor directamente al parámetro del método. Si se modifica ese parámetro dentro del método, **no se altera la variable original** fuera del método, ya que el método solo trabaja con la copia.  

En resumen: los objetos permiten modificar sus atributos dentro del método porque se pasa la referencia por valor, mientras que los tipos primitivos permanecen inalterados, ya que se pasa una copia del valor.


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta
En Java, el método **`toString()`** se utiliza para obtener una **representación en forma de texto** de un objeto. Cada clase hereda este método de la clase `Object`, y por defecto devuelve algo poco legible, como el nombre de la clase y un código de memoria. Sobrescribir `toString()` permite que los objetos muestren información más clara y útil al imprimirlos o concatenarlos en cadenas de texto.  

Otros lenguajes orientados a objetos tienen conceptos similares, aunque no siempre con el mismo nombre. Por ejemplo, en C++ se puede sobrecargar el operador `<<` para flujos de salida, y en Python existe el método `__str__()` para convertir un objeto en cadena. La idea siempre es la misma: proporcionar una **representación textual del objeto**.  

Un ejemplo de `toString()` en la clase `Punto` podría ser:  

```java
class Punto {
    double x;
    double y;

    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Sobrescribir el método toString
    @Override
    public String toString() {
        return "Punto(" + x + ", " + y + ")";
    }
}

// Uso
public class Main {
    public static void main(String[] args) {
        Punto p = new Punto(3, 4);
        System.out.println(p); // Imprime: Punto(3.0, 4.0)
    }
}
```
En este ejemplo, al imprimir `p`, Java llama automáticamente a `toString()` y muestra los valores de `x` e `y` de manera legible, en lugar de mostrar información técnica sobre la clase y la referencia.


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?

### Respuesta
Una **clase en Java** y un `struct` en C tienen similitudes: ambos sirven para **agrupar datos relacionados** bajo un mismo tipo, y ambos permiten crear múltiples variables de ese tipo. Sin embargo, un `struct` en C únicamente puede contener **atributos**, es decir, datos, y no puede contener directamente **métodos** o comportamientos.  

Para que un `struct` se parezca a una clase, le faltarían varias características de la programación orientada a objetos: **métodos para operar sobre los datos**, **constructores para inicializar los valores**, **encapsulamiento** (controlar la visibilidad de los datos) y **herencia/polimorfismo** para relacionar tipos entre sí.  

En otras palabras, mientras que un `struct` es simplemente un contenedor de datos, una **clase define tanto los datos como el comportamiento** de esos datos, y permite que cada variable de ese tipo (cada instancia) tenga su propio estado y funciones asociadas. Esto convierte a las variables de tipo clase en **objetos completos**, no solo en bloques de memoria con datos.  


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta
En C no existen clases ni objetos, pero se puede **emular una clase usando un `struct` para los datos** y funciones externas que reciban un puntero al `struct` para operar sobre ellos. En este caso, el puntero cumple el papel de `this`, ya que permite que la función acceda a los atributos de una instancia concreta.  

Por ejemplo, la clase `Punto` en Java se podría emular así en C:  

```c
#include <stdio.h>
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

// Función que calcula la distancia al origen
double calculaDistanciaAOrigen(Punto *p) {
    return sqrt(p->x * p->x + p->y * p->y);
}

int main() {
    Punto p1 = {3, 4};
    printf("Distancia al origen: %.2f\n", calculaDistanciaAOrigen(&p1));
    return 0;
}
```

**En este ejemplo:**
- `Punto` es un `struct` que contiene los atributos `x` e `y`.  
- La función `calculaDistanciaAOrigen` recibe un **puntero a `Punto`**, que actúa como `this` en Java, permitiendo acceder a los atributos específicos de esa instancia.  
- Cada `struct` creado puede tener sus propios valores, igual que un objeto en Java, pero **no tiene métodos integrados**: las funciones son externas y se les pasa explícitamente la instancia.  
- En resumen, `this` desaparece como palabra reservada, pero se emula usando un puntero al `struct` para indicar sobre qué instancia se está operando.
