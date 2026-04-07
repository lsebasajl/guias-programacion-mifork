<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuesta
En orientación a objetos, la **herencia** es un mecanismo mediante el cual una clase (subclase) adquiere las propiedades y comportamientos de otra (superclase). Esto se expresa con la relación **“A es-un B”**: por ejemplo, un *Artillero es-un Soldado* y un *Zapador es-un Soldado*. Esta relación implica que los objetos de las subclases pueden tratarse como objetos de la superclase, respetando su contrato general. No se trata solo de reutilizar código, sino de modelar correctamente jerarquías conceptuales.

La primera implicación es la **compatibilidad de tipos**: cualquier instancia de una subclase puede usarse donde se espera una instancia de la superclase. Esto permite, por ejemplo, almacenar objetos de distintos tipos concretos en una misma estructura común (como un array de `Soldado`) y tratarlos de forma uniforme. La segunda implicación es la **herencia de estado y comportamiento**: la subclase hereda los atributos y métodos accesibles de la superclase, pudiendo utilizarlos o ampliarlos. Así, tanto el Artillero como el Zapador comparten el atributo `nombre` y el método `saludar()`.

A continuación se muestra un ejemplo sencillo en Java que ilustra ambas ideas. Se define una clase base `Soldado` con un atributo privado `nombre` y un método `saludar()`. Luego se crean dos subclases, cada una con su propio estado adicional y métodos específicos:

```java
class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

class Artillero extends Soldado {
    private int numeroCohetes;

    public Artillero(String nombre, int numeroCohetes) {
        super(nombre);
        this.numeroCohetes = numeroCohetes;
    }

    public int getNumeroCohetes() {
        return numeroCohetes;
    }
}

class Zapador extends Soldado {
    private int numeroMinas;

    public Zapador(String nombre, int numeroMinas) {
        super(nombre);
        this.numeroMinas = numeroMinas;
    }

    public int getNumeroMinas() {
        return numeroMinas;
    }
}
```

Gracias a la compatibilidad de tipos, se puede crear un array de `Soldado` que contenga tanto artilleros como zapadores, y recorrerlo invocando el método común `saludar()` sin preocuparse del tipo concreto de cada objeto:

```java
public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];

        ejercito[0] = new Artillero("Carlos", 5);
        ejercito[1] = new Zapador("Luis", 3);
        ejercito[2] = new Artillero("Ana", 7);

        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}
```

Este ejemplo muestra cómo la herencia permite compartir código (estado y comportamiento) y, al mismo tiempo, tratar objetos distintos de forma uniforme gracias a la compatibilidad de tipos.


## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta
Al crear un objeto de una subclase (por ejemplo, un `Artillero` o un `Zapador`), **se ejecutan varios constructores**, uno por cada nivel de la jerarquía. En primer lugar se ejecuta el constructor de la **superclase** (`Soldado`) y, a continuación, el de la **subclase** concreta. Este orden es obligatorio porque antes de inicializar la parte específica del objeto, debe estar correctamente inicializada la parte heredada. Por tanto, al instanciar un `Artillero`, se ejecutan dos constructores: primero el de `Soldado` y luego el de `Artillero`.

La palabra clave `super` dentro de un constructor se utiliza para **invocar explícitamente el constructor de la superclase**. Permite pasar los parámetros necesarios para inicializar los atributos heredados. Además, esta llamada a `super(...)` debe ser **la primera instrucción del constructor** de la subclase. Si no se escribe, Java intenta insertar automáticamente una llamada a `super()` sin parámetros, siempre que exista un constructor sin argumentos en la clase base.

Si la clase base **no tiene un constructor sin parámetros accesible**, entonces **es obligatorio llamar a `super(...)` de forma explícita** desde el constructor de la subclase, proporcionando los argumentos adecuados. En caso contrario, el código no compilará, ya que Java no podrá inicializar correctamente la parte heredada. Esto obliga a que el programador tenga en cuenta cómo se construyen los objetos de la superclase y garantice su correcta inicialización desde las subclases.

**Ejemplos**
Cuando la clase base **no dispone de un constructor sin parámetros accesible**, es obligatorio invocar explícitamente a `super(...)` desde la subclase. En caso contrario, el compilador intentará insertar `super()` automáticamente y fallará, ya que ese constructor no existe o no es visible. Esto garantiza que la parte heredada del objeto quede correctamente inicializada, obligando a proporcionar los datos necesarios desde la subclase.

Un ejemplo sencillo es el siguiente. La clase `Soldado` solo tiene un constructor con parámetros, por lo que `Artillero` debe llamar explícitamente a `super(nombre)`:

```java
class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }
}

class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre); // obligatorio, no existe super()
        this.cohetes = cohetes;
    }
}
```

Si se eliminase la llamada a `super(nombre)`, el código no compilaría, porque Java intentaría usar `super()` y no existe. Esto muestra claramente la necesidad de invocar explícitamente al constructor adecuado de la superclase cuando no hay uno por defecto.

En el caso de una jerarquía con **tres niveles**, se ejecuta una cadena de constructores desde la clase más general hasta la más específica. Cada nivel es responsable de inicializar su parte del objeto, llamando al nivel superior. Por ejemplo:

```java
class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        System.out.println("Constructor Soldado");
        this.nombre = nombre;
    }
}

class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        System.out.println("Constructor Artillero");
        this.cohetes = cohetes;
    }
}

class ArtilleroElite extends Artillero {
    private int rango;

    public ArtilleroElite(String nombre, int cohetes, int rango) {
        super(nombre, cohetes);
        System.out.println("Constructor ArtilleroElite");
        this.rango = rango;
    }
}
```

Al crear un objeto `new ArtilleroElite("Ana", 5, 2)`, el orden de ejecución será: primero el constructor de `Soldado`, luego el de `Artillero` y finalmente el de `ArtilleroElite`. Esto refleja cómo la inicialización se propaga desde la superclase más alta hasta la subclase más concreta, asegurando que todas las partes del objeto quedan correctamente construidas.


## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta
En memoria, una instancia de una subclase **incluye también los atributos definidos en su superclase**, incluso si estos son `private`. Es decir, un objeto de tipo `Artillero` o `Zapador` contiene internamente tanto sus propios atributos como los heredados de `Soldado` (por ejemplo, `nombre`). Desde el punto de vista del modelo de memoria, no existen “dos objetos separados”, sino uno solo cuya estructura incluye todas las partes de la jerarquía.

Sin embargo, que esos atributos formen parte del objeto **no implica que puedan ser accedidos directamente desde el código de la subclase** si son `private`. El modificador `private` restringe el acceso únicamente a la propia clase donde se declara. Por tanto, aunque un `Artillero` “tenga” el atributo `nombre` en memoria, no puede acceder a él directamente con `this.nombre`. Para interactuar con ese estado, se deben usar métodos públicos o protegidos definidos en la superclase (por ejemplo, un getter o métodos como `saludar()`).

Se puede ilustrar con el ejemplo:

```java
class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public void mostrarInfo() {
        // System.out.println(nombre); // ERROR: nombre es privado en Soldado
        System.out.println("Soy " + getNombre() + " y tengo " + cohetes + " cohetes");
    }
}
```

En este ejemplo, el atributo `nombre` forma parte del objeto `Artillero` en memoria, pero no puede ser accedido directamente desde la subclase. En su lugar, se utiliza el método `getNombre()` heredado. Esto muestra la diferencia entre **existencia en memoria** y **accesibilidad desde el código**, que viene determinada por las reglas de encapsulación.


## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta
La compatibilidad de tipos en herencia implica una **alta extensibilidad del código**, ya que permite añadir nuevas subclases sin necesidad de modificar el código existente que trabaja con la superclase. Esto se relaciona con la idea de que el código debería estar **abierto a extensión pero cerrado a modificación**: se pueden introducir nuevos comportamientos creando nuevas subclases, sin alterar las estructuras que ya funcionan. Al tratar todos los objetos como instancias de `Soldado`, el código cliente no necesita conocer los detalles de cada tipo concreto.

Gracias a esta compatibilidad, cualquier nueva subclase de `Soldado` podrá utilizarse allí donde se espere un `Soldado`. Por tanto, estructuras como arrays, listas o métodos que operan con `Soldado` seguirán funcionando sin cambios. Esto reduce el acoplamiento y facilita la evolución del sistema, ya que no es necesario reescribir ni duplicar lógica cuando se amplía la jerarquía.

Se puede ilustrar añadiendo un nuevo tipo, por ejemplo `Medico`, que también es un `Soldado` pero tiene su propia funcionalidad específica:

```java
class Medico extends Soldado {
    private int botiquines;

    public Medico(String nombre, int botiquines) {
        super(nombre);
        this.botiquines = botiquines;
    }

    public int getBotiquines() {
        return botiquines;
    }
}
```

El código que trabaja con un conjunto de soldados **no necesita modificarse** para soportar este nuevo tipo. Simplemente se pueden añadir instancias de `Medico` al mismo array de `Soldado` y todo seguirá funcionando igual:

```java
public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[4];

        ejercito[0] = new Artillero("Carlos", 5);
        ejercito[1] = new Zapador("Luis", 3);
        ejercito[2] = new Artillero("Ana", 7);
        ejercito[3] = new Medico("Marta", 2); // nuevo tipo

        for (Soldado s : ejercito) {
            s.saludar(); // no se modifica este código
        }
    }
}
```

Esto demuestra que la extensibilidad se consigue sin tocar el código existente: el bucle que invoca `saludar()` funciona igual para cualquier subtipo de `Soldado`. La compatibilidad de tipos permite así escribir código más general, reutilizable y fácil de mantener.


## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta
En Java, es posible que una **referencia del supertipo apunte a un objeto real de un subtipo**. Por ejemplo, una variable de tipo `Soldado` puede referenciar tanto a un `Artillero` como a un `Zapador`. Esto es consecuencia directa de la compatibilidad de tipos en herencia. Sin embargo, con esa referencia solo se pueden invocar directamente los métodos que estén definidos en `Soldado` (aunque el comportamiento concreto pueda ser el de la subclase si hay sobrescritura).

No es posible invocar directamente métodos específicos del subtipo (por ejemplo, `getNumeroCohetes()`) usando una referencia de tipo `Soldado`, ya que el compilador solo conoce los métodos declarados en la clase `Soldado`. Para acceder a métodos propios del subtipo, es necesario realizar un **downcasting**, es decir, convertir explícitamente la referencia al tipo más específico. El **upcasting** es el proceso inverso (y automático), en el que un objeto de subtipo se trata como su supertipo (por ejemplo, asignar un `Artillero` a una variable `Soldado`).

El operador `instanceof` permite comprobar en tiempo de ejecución si un objeto es instancia de una clase concreta (o de alguna de sus subclases). Esto es útil para realizar downcasting de forma segura, evitando errores en ejecución. Sin esta comprobación, un casting incorrecto produciría una excepción (`ClassCastException`).

A continuación se muestra un ejemplo recorriendo un array de `Soldado`. Si el objeto real es un `Artillero`, se hace un casting y se accede a su información específica:

```java
for (Soldado s : ejercito) {
    s.saludar();

    if (s instanceof Artillero) {
        Artillero a = (Artillero) s; // downcasting
        System.out.println("Tiene " + a.getNumeroCohetes() + " cohetes");
    }
}
```

En este código, el array puede contener distintos tipos de soldados, pero mediante `instanceof` se identifica cuándo el objeto real es un `Artillero`. Entonces se realiza el downcasting para poder acceder a su método específico. Esto ilustra cómo combinar polimorfismo (trato común) con acceso a comportamientos particulares cuando es necesario.


## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta
El acceso **protegido** (`protected`) en herencia significa que un atributo o método es accesible **desde la propia clase, desde sus subclases y desde otras clases del mismo paquete**. A diferencia de `private`, que restringe el acceso únicamente a la clase donde se declara, `protected` permite que las subclases utilicen directamente ese estado o comportamiento heredado. Esto facilita la reutilización en jerarquías de herencia, manteniendo cierto nivel de encapsulación (no es completamente público).

En Java, se implementa utilizando la palabra clave `protected` delante del atributo o método. De esta forma, las subclases pueden acceder directamente a dichos miembros sin necesidad de getters o setters. Sin embargo, sigue siendo una buena práctica usarlo con cuidado, ya que expone parte de la implementación interna a las subclases, aumentando el acoplamiento entre ellas y la superclase.

Se puede modificar el ejemplo de `Soldado` para que el atributo `nombre` sea `protected`, permitiendo que `Zapador` lo utilice directamente en uno de sus métodos específicos:

```java
class Soldado {
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

class Zapador extends Soldado {
    private int numeroMinas;

    public Zapador(String nombre, int numeroMinas) {
        super(nombre);
        this.numeroMinas = numeroMinas;
    }

    public void ponerMina() {
        System.out.println(nombre + " ha colocado una mina");
    }
}
```

En este ejemplo, `Zapador` accede directamente al atributo `nombre` heredado gracias a que es `protected`. Si fuese `private`, esto no sería posible y habría que recurrir a un método público o protegido en `Soldado`. Esto ilustra cómo `protected` permite compartir detalles internos entre clases relacionadas por herencia, manteniendo un equilibrio entre encapsulación y reutilización.


## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta
En muchos lenguajes orientados a objetos existe el concepto de una **clase base común para todos los objetos**, pero no es una regla universal. Depende del lenguaje y de su modelo de tipos. Algunos lenguajes tienen una jerarquía completamente unificada donde todo hereda de una clase raíz, mientras que otros (especialmente los que combinan programación estructurada y orientación a objetos, como C++) no obligan a que todas las clases compartan un ancestro común explícito.

En Java, sí existe una clase base universal: **`Object`** (`java.lang.Object`). Todas las clases en Java heredan directa o indirectamente de esta clase, incluso aunque no se indique explícitamente con `extends`. Esto significa que cualquier objeto en Java es también un `Object`, lo que permite tratarlos de forma uniforme. Por ejemplo, se pueden usar referencias de tipo `Object` para almacenar cualquier instancia de cualquier clase.

Como consecuencia, todas las clases heredan ciertos métodos comunes definidos en `Object`, como `toString()`, `equals()` o `hashCode()`. Esto proporciona una base común de comportamiento y facilita operaciones generales sobre objetos. Por tanto, en Java la jerarquía de herencia es única y tiene una raíz bien definida, lo que simplifica el modelo de tipos y favorece la interoperabilidad entre clases.


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta
La **herencia múltiple** es un mecanismo mediante el cual una clase puede **heredar de más de una superclase al mismo tiempo**. Esto permitiría, por ejemplo, que una clase combine directamente estado y comportamiento de varias clases base distintas. Aunque puede resultar potente, también introduce problemas como ambigüedades (por ejemplo, si dos superclases tienen métodos con el mismo nombre) y mayor complejidad en el diseño.

En Java, **no existe la herencia múltiple de clases**. Es decir, una clase solo puede extender de una única superclase (`extends`). Esta decisión se tomó para evitar problemas como el llamado *problema del diamante*, donde no está claro qué implementación debe heredarse cuando varias clases comparten un ancestro común. De esta forma, Java mantiene un modelo de herencia más simple y predecible.

Sin embargo, Java sí permite una forma alternativa de reutilización múltiple mediante **interfaces**. Una clase puede implementar múltiples interfaces (`implements`), lo que le permite heredar contratos (métodos abstractos) de varias fuentes. Desde versiones modernas de Java, las interfaces pueden incluir métodos `default` con implementación, lo que introduce cierta capacidad similar a la herencia múltiple, pero con reglas bien definidas para evitar conflictos.


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta
En Java, las excepciones son objetos que forman parte de una jerarquía cuya raíz es `Throwable`. Las **excepciones no controladas** (unchecked) son aquellas que heredan de `RuntimeException` y no obligan a ser declaradas ni capturadas explícitamente. Crear excepciones personalizadas permite modelar errores de dominio de forma más expresiva, incluyendo información relevante sobre el contexto en el que se produjo el problema.

En este caso, se define una excepción `UsuarioNoEncontradoException` que es no controlada (extiende de `RuntimeException`) y que además está **compuesta con un objeto `Usuario`**, de forma que se pueda conocer qué usuario provocó el error. También se sobrecargan los constructores para permitir opcionalmente incluir una **causa** (`Throwable cause`), lo cual es útil para encadenar excepciones y no perder información sobre el origen del fallo.

A continuación se muestra un ejemplo completo:

```java
class Usuario {
    private String nombre;

    public Usuario(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;

    public UsuarioNoEncontradoException(Usuario usuario) {
        super("Usuario no encontrado: " + usuario.getNombre());
        this.usuario = usuario;
    }

    public UsuarioNoEncontradoException(Usuario usuario, Throwable cause) {
        super("Usuario no encontrado: " + usuario.getNombre(), cause);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}
```

En este diseño, la excepción incluye tanto un mensaje descriptivo como el objeto `Usuario` implicado, accesible mediante un getter. Además, la sobrecarga del constructor permite conservar la causa original del error, lo que resulta fundamental para depuración y trazabilidad. De este modo, se combinan herencia (de `RuntimeException`) y composición (con `Usuario`) para construir una excepción rica en información.


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta
No se debe usar herencia únicamente para **reutilizar código** porque la herencia implica **una relación semántica fuerte entre clases**. La frase “A es un B” debe tener sentido conceptual en el dominio del problema. Si se usa herencia solo para copiar métodos y atributos, se fuerza una jerarquía que puede no reflejar la realidad, generando acoplamiento innecesario y limitando la flexibilidad futura del código. Esto puede llevar a problemas cuando se necesiten cambios en la superclase: todas las subclases heredarán esos cambios, aunque no tengan relación lógica con ellos.

Además, heredar atributos y métodos de forma indebida rompe la **encapsulación**, ya que las subclases quedan atadas a la implementación de la superclase. Si el objetivo es solo compartir funcionalidad, se puede optar por **composición**, creando clases que contengan instancias de otras y deleguen comportamiento. La composición es más flexible, permite cambiar los componentes sin afectar a toda la jerarquía y respeta mejor los principios de diseño orientado a objetos como **Open/Closed** y **Single Responsibility**.

Por ejemplo, si se quisiera reutilizar la capacidad de saludar de `Soldado` en otra clase que no es un soldado, sería más adecuado crear un objeto `Saludador` y que la clase lo **contenga** y lo use, en lugar de forzar herencia. Así se reutiliza código sin introducir relaciones semánticas incorrectas ni dependencias rígidas entre clases. La herencia debe reflejar **un “es-un” real**, no solo servir como mecanismo de ahorro de líneas de código.


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta
Se recomienda **favorecer la composición frente a la herencia** porque la composición ofrece **mayor flexibilidad y menor acoplamiento** entre clases. Al usar composición, un objeto contiene instancias de otras clases y delega comportamientos a ellas, en lugar de depender de la implementación de una superclase. Esto permite cambiar o reemplazar componentes internos sin afectar a toda la jerarquía, mientras que la herencia fija de manera rígida la relación “es-un”, haciendo difícil modificar o extender el comportamiento sin afectar a todas las subclases existentes.

Otro motivo es que la composición respeta mejor los principios de diseño orientado a objetos, como **Open/Closed** y **Single Responsibility**. Una clase puede enfocarse en su responsabilidad principal y delegar tareas específicas a objetos internos especializados, evitando la sobrecarga de la superclase y la proliferación de métodos innecesarios en la jerarquía. La herencia, en cambio, tiende a introducir dependencias implícitas: cualquier cambio en la superclase puede afectar inesperadamente a todas las subclases.

En términos de reutilización de código, la composición permite **compartir funcionalidad sin forzar relaciones semánticas que no existen**. Por ejemplo, si se quisiera que una clase `Ingeniero` pueda “saludar” igual que un `Soldado`, no hace falta que `Ingeniero` herede de `Soldado`; basta con que contenga un objeto `Saludador` y delegue la operación. Esto evita relaciones “es-un” artificiales y mantiene el diseño más modular y fácil de mantener a medida que el sistema crece.

En resumen, la composición favorece **flexibilidad, encapsulación y modularidad**, mientras que la herencia debe reservarse para modelar relaciones naturales y semánticas de tipo “A es-un B”.


## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta
Cuando se dice que la **herencia rompe la encapsulación**, se refiere a que las subclases **quedan expuestas a la implementación interna de la superclase**. Al heredar atributos y métodos, la subclase puede depender de detalles que originalmente estaban destinados a ser internos, incluso si solo se querían reutilizar ciertas funcionalidades. Esto crea un acoplamiento fuerte entre la superclase y la subclase: cambios en la superclase pueden afectar directamente a todas las subclases, aunque no tengan relación lógica con los cambios.

Por ejemplo, si un `Soldado` tiene un atributo `nombre` y un método `saludar()`, cualquier subclase como `Artillero` o `Zapador` hereda esos elementos. Aunque la intención fuera solo aprovechar `saludar()`, la subclase queda obligada a mantener la consistencia con la forma en que `Soldado` maneja `nombre`. Si en el futuro se cambia la implementación de `nombre` en `Soldado` (por ejemplo, se pasa a almacenarlo encriptado), todas las subclases deben adaptarse, rompiendo el principio de encapsulación de manera indirecta.

En contraste, la **composición** preserva la encapsulación, porque el objeto interno se maneja mediante métodos públicos o protegidos, y la clase que lo contiene solo interactúa a través de su interfaz. Por ejemplo, un objeto `Saludador` podría encargarse de mostrar mensajes, y cualquier clase que lo use no necesita conocer cómo guarda o formatea el nombre. De esta forma, los detalles internos permanecen ocultos y cualquier cambio dentro del componente no rompe el código de las clases que lo utilizan.

En resumen, la herencia **expone implícitamente detalles internos de la superclase**, mientras que la composición permite reutilizar funcionalidad **sin comprometer la encapsulación**, reduciendo riesgos y manteniendo un diseño más robusto.


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta
Se puede modelar la relación de **persona, estudiante y trabajador** de dos formas: usando herencia y usando composición. Cada enfoque refleja decisiones de diseño distintas y tiene implicaciones sobre la flexibilidad y el acoplamiento.

**Opción 1: Herencia**
Se crea una superclase `Persona` que contiene los atributos comunes `dni` y `nombre`. `Estudiante` y `Trabajador` extienden de `Persona` y heredan estos atributos. Este enfoque sigue la relación semántica “Estudiante es una Persona” y “Trabajador es una Persona”.

```java
class Persona {
    private String dni;
    private String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() { return dni; }
    public String getNombre() { return nombre; }
}

class Estudiante extends Persona {
    private String carrera;

    public Estudiante(String dni, String nombre, String carrera) {
        super(dni, nombre);
        this.carrera = carrera;
    }

    public String getCarrera() { return carrera; }
}

class Trabajador extends Persona {
    private String puesto;

    public Trabajador(String dni, String nombre, String puesto) {
        super(dni, nombre);
        this.puesto = puesto;
    }

    public String getPuesto() { return puesto; }
}
```

**Opción 2: Composición**
Se crea una clase `DatosPersonales` que encapsula los atributos `dni` y `nombre`. Tanto `Estudiante` como `Trabajador` **contienen** un objeto de tipo `DatosPersonales` en lugar de heredarlo. Esto permite reutilizar los datos sin imponer una relación “es-un” que podría no ser necesaria en otros contextos.

```java
class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() { return dni; }
    public String getNombre() { return nombre; }
}

class Estudiante {
    private DatosPersonales datos;
    private String carrera;

    public Estudiante(DatosPersonales datos, String carrera) {
        this.datos = datos;
        this.carrera = carrera;
    }

    public DatosPersonales getDatos() { return datos; }
    public String getCarrera() { return carrera; }
}

class Trabajador {
    private DatosPersonales datos;
    private String puesto;

    public Trabajador(DatosPersonales datos, String puesto) {
        this.datos = datos;
        this.puesto = puesto;
    }

    public DatosPersonales getDatos() { return datos; }
    public String getPuesto() { return puesto; }
}
```

Con la composición, los datos personales pueden reutilizarse en otros contextos sin forzar la jerarquía “Persona”. En cambio, la herencia es más natural si existe una relación clara “es-un” y se espera que muchas operaciones comunes se compartan entre todas las personas. La elección depende de **si se necesita reflejar un vínculo semántico fuerte (herencia)** o **solo reutilizar estado y comportamiento (composición)**.