<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

### Respuesta
En **C** es habitual construir estructuras más complejas **componiendo otras más simples**. Este enfoque se suele describir como una relación *“tiene-un”* o *“tiene-varios”*. Por ejemplo, una línea puede entenderse como un objeto que **tiene dos puntos**, mientras que cada punto **tiene dos coordenadas**: `x` e `y`. Esta forma de organizar los datos permite modelar mejor los elementos del problema y facilita reutilizar estructuras ya definidas.

Para representar esta situación se puede definir primero una estructura `Punto` con dos coordenadas. Después se define una estructura `Linea` que **contiene dos puntos**, representando el inicio y el final de la línea. De esta forma se observa claramente la composición: la línea está formada por puntos.

También es posible definir funciones que operen con estas estructuras. Una función puede calcular la **distancia entre dos puntos** usando la fórmula de distancia euclídea, mientras que otra puede calcular la **longitud de una línea** reutilizando la función anterior, ya que una línea está definida precisamente por sus dos puntos extremos.

```c
#include <stdio.h>
#include <math.h>

struct Punto {
    double x;
    double y;
};

struct Linea {
    struct Punto p1;
    struct Punto p2;
};

double distancia(struct Punto a, struct Punto b) {
    return sqrt((a.x - b.x)*(a.x - b.x) + (a.y - b.y)*(a.y - b.y));
}

double longitudLinea(struct Linea l) {
    return distancia(l.p1, l.p2);
}

int main() {
    struct Linea l = { {0,0}, {3,4} };
    printf("Longitud de la linea: %.2f\n", longitudLinea(l));
    return 0;
}
```

En este ejemplo se aprecia cómo una estructura puede **contener otras estructuras**, representando relaciones de composición entre los datos. Además, las funciones permiten encapsular operaciones habituales, como calcular distancias o longitudes, lo que mejora la claridad y reutilización del código.


## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### Respuesta
En **Java**, la composición también permite construir objetos más complejos a partir de otros más simples. En este caso, una línea puede modelarse como un objeto que **tiene dos puntos**, mientras que cada punto contiene sus coordenadas `x` e `y`. A diferencia de C, el lenguaje proporciona mecanismos de **ocultación de información**, lo que permite controlar cómo se accede y modifica el estado interno de los objetos.

Para garantizar la **inmutabilidad**, los atributos de las clases pueden declararse como `private` y `final`. De esta forma, una vez creado el objeto, sus valores no pueden modificarse. La clase `Punto` contendrá las coordenadas y ofrecerá un método para calcular la distancia a otro punto. Por su parte, la clase `Linea` estará compuesta por dos objetos `Punto`, representando el inicio y el final de la línea.

Gracias a este diseño, tanto los puntos como la línea permanecen **inalterables tras su creación**. No se proporcionan métodos para modificar sus atributos, lo que asegura que una línea siempre representará el mismo segmento entre dos puntos. Este tipo de diseño es habitual en programación orientada a objetos cuando se desea representar entidades geométricas o valores matemáticos.

```java
class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distancia(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distancia(p2);
    }
}
```


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta
La **multiplicidad** en una relación de composición indica **cuántas instancias de una clase pueden estar asociadas con instancias de otra clase**. Es un concepto muy utilizado en el modelado orientado a objetos, especialmente en diagramas de clases, para expresar cuántos objetos participan en una relación. En otras palabras, describe si un objeto contiene uno, varios o ningún objeto de otro tipo.

En el ejemplo de las clases `Linea` y `Punto`, una línea está definida por **dos puntos concretos**, que representan su inicio y su final. Por tanto, la multiplicidad **de `Linea` a `Punto` es 2**, ya que cada línea está compuesta exactamente por dos puntos. Esto refleja la idea de que la línea *tiene dos* puntos y que esos puntos forman parte de su estructura.

En la dirección contraria, es decir, **de `Punto` a `Linea`**, la multiplicidad puede interpretarse como **0..*** (cero o más). Un punto podría no pertenecer a ninguna línea, o podría utilizarse como extremo de muchas líneas diferentes. Por ejemplo, un mismo punto en un plano podría ser compartido por varias líneas que parten de él. De esta forma, mientras que una línea siempre necesita exactamente dos puntos, un punto puede estar asociado con cualquier número de líneas.


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta
En orientación a objetos, la **composición** describe una relación en la que un objeto está formado por otros objetos. Sin embargo, esta relación puede tener distintos niveles de dependencia entre las partes. Por ello suele distinguirse entre **composición fuerte** y **composición débil**, dependiendo de hasta qué punto los objetos contenidos dependen del objeto que los contiene.

La **composición fuerte** implica una relación muy estrecha entre los objetos. En este caso, los objetos componentes **no tienen sentido fuera del objeto que los contiene** y su ciclo de vida depende completamente de él. Cuando el objeto principal se crea, normalmente se crean también sus componentes, y cuando el objeto principal se destruye, sus componentes también desaparecen. A este tipo de relación es a lo que normalmente se denomina **composición propiamente dicha**.

Por otro lado, la **composición débil** ocurre cuando un objeto utiliza o contiene referencias a otros objetos, pero estos **pueden existir de manera independiente**. Es decir, su ciclo de vida no depende necesariamente del objeto que los utiliza. En este caso, los objetos pueden compartirse entre distintas estructuras o seguir existiendo aunque el objeto que los referenciaba deje de existir. Este tipo de relación suele denominarse **asociación** o **agregación**.

La diferencia principal, por tanto, está en el **ciclo de vida de los objetos**. En la composición fuerte existe una dependencia total entre el contenedor y sus partes, mientras que en la composición débil los objetos relacionados mantienen una existencia independiente y simplemente están vinculados entre sí.

Un ejemplo sencillo de **composición fuerte** puede encontrarse en una clase `Casa` que contiene varias `Habitacion`. En este caso, las habitaciones forman parte de la casa y normalmente **no tienen sentido como entidades independientes dentro del modelo del programa**. Si la casa deja de existir, también desaparecen sus habitaciones. Por tanto, el ciclo de vida de las habitaciones depende completamente del de la casa, lo que representa una **composición propiamente dicha**.

```java
class Habitacion {
    private final String nombre;

    public Habitacion(String nombre) {
        this.nombre = nombre;
    }
}

class Casa {
    private final Habitacion salon;
    private final Habitacion cocina;

    public Casa() {
        this.salon = new Habitacion("Salon");
        this.cocina = new Habitacion("Cocina");
    }
}
```

Un ejemplo de **composición débil (agregación o asociación)** puede observarse en una relación entre `Equipo` y `Jugador`. Un equipo tiene jugadores, pero los jugadores **pueden existir independientemente del equipo**. Un jugador podría cambiar de equipo o incluso no pertenecer a ninguno en un momento dado. En este caso, el ciclo de vida de los jugadores **no depende del equipo**, por lo que se trata de una agregación.

```java
class Jugador {
    private String nombre;

    public Jugador(String nombre) {
        this.nombre = nombre;
    }
}

class Equipo {
    private Jugador jugador;

    public Equipo(Jugador jugador) {
        this.jugador = jugador;
    }
}
```

En resumen, en la **composición fuerte** las partes dependen completamente del objeto que las contiene (como las habitaciones de una casa), mientras que en la **composición débil o agregación** los objetos pueden existir y reutilizarse independientemente, aunque estén relacionados con otros (como los jugadores de un equipo).


## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta
Cuando una clase utiliza otra únicamente dentro de métodos, por ejemplo **recibiéndola como parámetro, devolviéndola como resultado, creándola con `new` dentro de un método o utilizándola como variable local**, no se considera una relación de composición. En estos casos se habla de **dependencia** entre clases. La dependencia indica que una clase **necesita usar otra para realizar alguna operación**, pero no la mantiene como parte de su estado interno.

La diferencia principal es que en la **composición** una clase contiene objetos de otra clase como **atributos**, formando parte de su estructura. Esto implica una relación más estable en el diseño del programa, ya que los objetos están asociados durante toda la vida del objeto que los contiene. En cambio, en una **dependencia**, el uso de la otra clase es normalmente **temporal**, limitado a la ejecución de un método concreto.

Por ejemplo, si una clase `CalculadoraGeometrica` recibe objetos `Punto` como parámetros para calcular una distancia, la clase está **dependiendo** de `Punto`, pero no está compuesta por puntos. Los objetos se usan solo para realizar un cálculo puntual y no se almacenan como atributos de la clase.

```java
class CalculadoraGeometrica {

    public static double distancia(Punto a, Punto b) {
        double dx = a.getX() - b.getX();
        double dy = a.getY() - b.getY();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

En este ejemplo, `CalculadoraGeometrica` **depende de la clase `Punto`** porque utiliza sus objetos en un método, pero no existe una relación de composición, ya que la clase no guarda puntos como parte de su estado interno.


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta
En el ejemplo de `Linea` y `Punto`, la relación puede implementarse de dos maneras según cómo se gestione el **ciclo de vida de los objetos `Punto`**. Si los puntos se crean dentro de la propia clase `Linea` y no pueden existir independientemente de ella, se está ante una **composición fuerte**. En cambio, si los puntos se crean fuera y simplemente se pasan a la línea para que los utilice, se trata de una **composición débil o agregación**, ya que los puntos pueden existir sin la línea.

En la **composición fuerte**, la clase `Linea` es responsable de crear los objetos `Punto`. De esta forma, los puntos quedan ligados al ciclo de vida de la línea: si la línea deja de existir, también dejan de existir los puntos que la componen. Desde el exterior solo se proporcionan las coordenadas necesarias para construir los puntos, pero los objetos `Punto` no se crean fuera de `Linea`.

```java
class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distancia(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(double x1, double y1, double x2, double y2) {
        this.p1 = new Punto(x1, y1);
        this.p2 = new Punto(x2, y2);
    }

    public double longitud() {
        return p1.distancia(p2);
    }
}
```

En la **composición débil (agregación)**, los objetos `Punto` se crean fuera de la clase `Linea` y se pasan como parámetros al constructor. Esto significa que los puntos pueden utilizarse en otros contextos o compartirse entre varias líneas. El ciclo de vida de los puntos ya no depende de la línea, sino del lugar donde se crean.

```java
class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distancia(p2);
    }
}
```

La diferencia entre ambas implementaciones está en **quién crea los objetos `Punto`**. Si la propia `Linea` los crea, existe composición fuerte y los puntos dependen de ella. Si los puntos se crean externamente y solo se pasan a la línea, se habla de composición débil o agregación, ya que los puntos mantienen una existencia independiente.


## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta
En **Java**, cuando existe una **composición fuerte**, el objeto contenedor es responsable de crear los objetos que forman parte de él, pero **no necesita destruirlos explícitamente**. Esto ocurre porque Java dispone de un sistema automático de gestión de memoria llamado **recolección de basura (*garbage collector*)**. Este mecanismo se encarga de liberar automáticamente los objetos que ya no pueden ser utilizados por el programa.

Cuando un objeto `Linea` deja de ser accesible (por ejemplo, cuando ya no existe ninguna referencia a él), el recolector de basura detecta que dicho objeto **ya no puede ser usado**. En ese momento, también observa que los objetos `Punto` contenidos dentro de `Linea` tampoco tienen referencias externas, ya que solo eran accesibles a través de la línea. Como consecuencia, tanto la línea como sus puntos pasan a ser candidatos a ser eliminados de la memoria.

Por esta razón, en el código **no se observa ninguna destrucción explícita** de los objetos `Punto`. A diferencia de lenguajes como **C o C++**, donde es necesario liberar memoria manualmente (`free` o `delete`), en Java esta tarea la realiza automáticamente el sistema de ejecución. El programador únicamente se preocupa de crear los objetos y de diseñar correctamente las relaciones entre ellos.

En consecuencia, en una **composición fuerte en Java**, el ciclo de vida de los objetos componentes sigue estando ligado al del contenedor de forma conceptual. Sin embargo, la destrucción real de los objetos **no se programa manualmente**, sino que se produce automáticamente cuando el recolector de basura detecta que ya no existen referencias activas hacia esos objetos.


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta
Un ejemplo de **composición débil (agregación)** puede darse entre un `Departamento` y varios `Profesor`. Los profesores pueden existir independientemente del departamento y podrían pertenecer a otros contextos, pero el departamento mantiene una colección de ellos. Además, el departamento tiene un **director**, que también es un profesor del propio departamento. Esto crea dos relaciones: una entre el departamento y todos sus profesores, y otra entre el departamento y el profesor que actúa como director.

Para mantener la **invariante de clase**, el director debe formar siempre parte de la lista de profesores. Por ello, al crear el departamento se exige un profesor inicial que será el director y que también se añade automáticamente a la lista de profesores. Asimismo, al cambiar el director se comprueba que el profesor ya pertenece al departamento, y al eliminar un profesor se impide eliminar al director. Si alguna de estas reglas se viola, se lanza una excepción.

La encapsulación se mantiene ocultando el uso interno de un **array `Profesor[]`**. Desde el exterior no se expone el array directamente, sino que se proporcionan métodos para añadir profesores, eliminar uno por posición, consultar cuántos hay y obtener uno por su posición. De esta forma se controla el acceso y se garantiza que siempre se respete la invariante del sistema.

```java
class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}
```

```java
class Departamento {
    private Profesor[] profesores = new Profesor[50];
    private int numProfesores = 0;
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("El director no puede ser null");
        }
        this.director = directorInicial;
        profesores[0] = directorInicial;
        numProfesores = 1;
    }

    public int getNumProfesores() {
        return numProfesores;
    }

    public Profesor getProfesor(int pos) {
        if (pos < 0 || pos >= numProfesores) {
            throw new IndexOutOfBoundsException();
        }
        return profesores[pos];
    }

    public void addProfesor(Profesor p) {
        if (numProfesores >= 50) {
            throw new IllegalStateException("Capacidad maxima alcanzada");
        }
        profesores[numProfesores++] = p;
    }

    public void eliminarProfesor(int pos) {
        if (pos < 0 || pos >= numProfesores) {
            throw new IndexOutOfBoundsException();
        }
        if (profesores[pos] == director) {
            throw new IllegalStateException("No se puede eliminar al director");
        }

        for (int i = pos; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[numProfesores - 1] = null;
        numProfesores--;
    }

    public Profesor getDirector() {
        return director;
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        boolean encontrado = false;
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == nuevoDirector) {
                encontrado = true;
                break;
            }
        }

        if (!encontrado) {
            throw new IllegalArgumentException("El director debe ser profesor del departamento");
        }

        director = nuevoDirector;
    }
}
```

En este diseño, los objetos `Profesor` pueden existir fuera del `Departamento`, lo que refleja la **composición débil**. Al mismo tiempo, las comprobaciones incluidas en los métodos garantizan que el **director siempre pertenezca al conjunto de profesores** y que nunca se elimine accidentalmente de la lista.

**NOTA: metodo eliminarProfesor**
El método `eliminarProfesor` tiene la función de **quitar un profesor de la lista interna del departamento**, pero manteniendo la **invariante** de que el director siempre forma parte de la lista y que la estructura interna siga siendo consistente. La lógica se puede desglosar paso a paso:

1. **Comprobación de posición válida:**

   ```java
   if (pos < 0 || pos >= numProfesores) {
       throw new IndexOutOfBoundsException();
   }
   ```

   Se asegura que la posición indicada esté dentro del rango de profesores actuales. Si no lo está, se lanza una excepción. Esto evita errores de acceso fuera del array.

2. **Protección del director:**

   ```java
   if (profesores[pos] == director) {
       throw new IllegalStateException("No se puede eliminar al director");
   }
   ```

   Antes de eliminar, se verifica si el profesor que se quiere eliminar es el **director**. Como el director debe permanecer en la lista, se lanza una excepción si se intenta eliminarlo. Esto garantiza que la invariante de clase no se rompa.

3. **Desplazamiento de elementos:**

   ```java
   for (int i = pos; i < numProfesores - 1; i++) {
       profesores[i] = profesores[i + 1];
   }
   ```

   Después de marcar al profesor para eliminar, se **recorren los elementos posteriores en el array hacia atrás** para llenar el hueco dejado por el profesor eliminado. Por ejemplo, si se elimina el profesor en la posición 2, el profesor en la posición 3 pasa a la 2, el de la posición 4 pasa a la 3, y así sucesivamente.

4. **Limpieza del último elemento y actualización del contador:**

   ```java
   profesores[numProfesores - 1] = null;
   numProfesores--;
   ```

   Después de desplazar los elementos, la última posición del array queda repetida, por lo que se pone `null` para evitar referencias colgantes. Luego se decrementa `numProfesores` para reflejar que ahora hay un profesor menos.

En resumen, el método **elimina un profesor de manera segura**, mantiene la **estructura contigua del array** y garantiza que el **director nunca se pueda eliminar**, cumpliendo la invariante de la clase.


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta
Al usar **`List<Profesor>`** en Java, se simplifica mucho la gestión de la colección de profesores. La clase `ArrayList` ya se encarga del tamaño dinámico, del desplazamiento de elementos al eliminar y de la comprobación de límites, por lo que no hace falta mantener manualmente un array y un contador (`numProfesores`). Esto significa que **gran parte del código original que manejaba el array primitivo, los desplazamientos y la gestión del tamaño se puede eliminar**.

A continuación se muestra cómo quedaría la clase `Departamento` usando `List` en lugar de un array primitivo:

```java
import java.util.ArrayList;
import java.util.List;

class Departamento {
    private final List<Profesor> profesores = new ArrayList<>();
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("El director no puede ser null");
        }
        this.director = directorInicial;
        profesores.add(directorInicial);
    }

    public int getNumProfesores() {
        return profesores.size();
    }

    public Profesor getProfesor(int pos) {
        return profesores.get(pos); // ArrayList ya verifica límites
    }

    public void addProfesor(Profesor p) {
        if (profesores.size() >= 50) {
            throw new IllegalStateException("Capacidad máxima alcanzada");
        }
        profesores.add(p);
    }

    public void eliminarProfesor(int pos) {
        Profesor p = profesores.get(pos);
        if (p == director) {
            throw new IllegalStateException("No se puede eliminar al director");
        }
        profesores.remove(pos); // List se encarga de desplazar elementos
    }

    public Profesor getDirector() {
        return director;
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalArgumentException("El director debe ser profesor del departamento");
        }
        director = nuevoDirector;
    }
}
```

### Qué se ha ahorrado respecto al array

* No hay necesidad de declarar un array de tamaño fijo ni un contador (`numProfesores`).
* No se necesita recorrer manualmente el array para **desplazar elementos** al eliminar un profesor.
* No se necesita asignar `null` a la última posición después de eliminar un elemento.

### Sobre devolver la lista interna

Si se añadiera un método que devolviera todos los profesores, como:

```java
public List<Profesor> getProfesores() {
    return profesores;
}
```

el **problema** es que la lista interna se **expondría directamente**, permitiendo que el código externo pueda modificarla (añadir, eliminar o reemplazar profesores), rompiendo la **encapsulación** y la invariante del director.

Para solucionarlo, se puede devolver **una copia de la lista** o una **vista no modificable**:

```java
import java.util.Collections;

public List<Profesor> getProfesores() {
    return Collections.unmodifiableList(new ArrayList<>(profesores));
}
```

De esta forma, quien llame al método puede leer la lista, pero **no puede modificar la lista interna del departamento**, garantizando que la invariante de clase siga siendo válida.


## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta
En Java, una **composición recursiva** ocurre cuando una clase contiene referencias a objetos de su **propia clase**, permitiendo estructuras jerárquicas o en cadena. Al igual que las excepciones pueden encerrar causas (otra excepción), este patrón permite modelar relaciones como árboles genealógicos, estructuras de nodos o listas enlazadas. Cada objeto “contiene” otro objeto de la misma clase, y así sucesivamente, formando una estructura recursiva.

A continuación se muestra un ejemplo sencillo de una clase `Persona` **inmutable**, que tiene como atributo a la madre, que también es una `Persona`. Para garantizar la inmutabilidad, todos los atributos son `final` y `private`, y no existen métodos que los modifiquen tras la construcción del objeto:

```java
class Persona {
    private final String nombre;
    private final Persona madre;

    public Persona(String nombre, Persona madre) {
        this.nombre = nombre;
        this.madre = madre;
    }

    public String getNombre() {
        return nombre;
    }

    public Persona getMadre() {
        return madre;
    }
}
```

En un **`main`**, se puede crear una familia empezando por la abuela, luego la madre y finalmente el nieto. Cada persona referencia a su madre, formando una cadena de composición recursiva:

```java
public class Main {
    public static void main(String[] args) {
        Persona abuela = new Persona("Abuela", null); // No tiene madre registrada
        Persona madre = new Persona("Madre", abuela);
        Persona nieto = new Persona("Nieto", madre);

        System.out.println(nieto.getNombre() + " tiene madre: " + nieto.getMadre().getNombre());
        System.out.println("Y su abuela es: " + nieto.getMadre().getMadre().getNombre());
    }
}
```

La salida sería:

```
Nieto tiene madre: Madre
Y su abuela es: Abuela
```

Algunos **ejemplos clásicos de composiciones recursivas** en programación incluyen:

1. **Excepciones con causa** (`Throwable cause`), donde una excepción puede contener otra.
2. **Estructuras de árboles** (por ejemplo, un nodo que tiene hijos de tipo `Nodo`).
3. **Listas enlazadas** (`Node next` en una lista simple o doblemente enlazada).
4. **Directorios y archivos en un sistema de ficheros**, donde un directorio contiene otros directorios o archivos.

Este patrón es muy útil para representar jerarquías, relaciones familiares, dependencias encadenadas o cualquier estructura donde un objeto pueda contener objetos de su mismo tipo.


## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta
Las relaciones de composición **bidireccionales** se producen cuando **ambas clases conocen y mantienen referencias mutuas entre sí**. Es decir, no solo el contenedor conoce a los componentes, sino que cada componente también sabe a qué contenedor pertenece. Esto permite navegar la relación en **ambas direcciones**, pero también introduce cierta complejidad: se debe garantizar que ambas referencias se mantengan consistentes para que la estructura no quede incoherente.

En términos de implementación, esto implica que, además de que el `Departamento` tenga una lista de `Profesor` y un director, **cada `Profesor` también tendría un atributo que apunte al departamento al que pertenece**. Cada vez que se añade un profesor al departamento, se debe actualizar la referencia del profesor; si se elimina o cambia de departamento, también se debe actualizar la referencia inversa. Esto asegura que ambas direcciones de la relación estén sincronizadas.

Por ejemplo, en el caso de `Profesor` y `Departamento`, se añadiría un atributo `departamento` en la clase `Profesor` y se modificarían los métodos de `Departamento` para mantener la consistencia:

```java
class Profesor {
    private final String nombre;
    private Departamento departamento; // referencia al departamento

    public Profesor(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }

    public Departamento getDepartamento() {
        return departamento;
    }

    void setDepartamento(Departamento depto) { // package-private para controlar desde Departamento
        this.departamento = depto;
    }
}
```

```java
class Departamento {
    private final List<Profesor> profesores = new ArrayList<>();
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        this.director = directorInicial;
        profesores.add(directorInicial);
        directorInicial.setDepartamento(this);
    }

    public void addProfesor(Profesor p) {
        profesores.add(p);
        p.setDepartamento(this); // actualizar referencia inversa
    }

    public void eliminarProfesor(int pos) {
        Profesor p = profesores.get(pos);
        if (p == director) {
            throw new IllegalStateException("No se puede eliminar al director");
        }
        profesores.remove(pos);
        p.setDepartamento(null); // limpiar referencia inversa
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalArgumentException("El director debe ser profesor del departamento");
        }
        director = nuevoDirector;
    }
}
```

De esta manera, cada profesor **sabe a qué departamento pertenece**, y el departamento **mantiene la lista de profesores**, logrando así una relación **bidireccional consistente**.

Un aspecto importante de este diseño es **controlar la visibilidad** del método `setDepartamento` para que los cambios solo se realicen desde el departamento, evitando inconsistencias desde código externo.
