<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

### Respuesta
El polimorfismo es un concepto de la programación orientada a objetos que permite que una misma referencia de tipo general (por ejemplo, una clase base) pueda comportarse como distintos tipos concretos en tiempo de ejecución. En otras palabras, se puede invocar el mismo método sobre objetos de distintas clases y obtener comportamientos diferentes según el tipo real del objeto. Esto permite escribir código más general y reutilizable, reduciendo la necesidad de duplicar lógica para cada tipo concreto.

Su utilidad principal es la flexibilidad y extensibilidad del diseño y del programa. Gracias al polimorfismo, se pueden tratar objetos distintos de forma uniforme siempre que compartan una misma interfaz o clase base, lo cual facilita trabajar con colecciones de objetos heterogéneos y aplicar estructuras más genéricas. Esto es especialmente importante en Java, donde se suele programar contra interfaces o clases abstractas en lugar de clases concretas.

La sobreescritura de métodos (method overriding) es el mecanismo que permite que el polimorfismo funcione en tiempo de ejecución. Consiste en que una subclase redefine un método de su clase padre con la misma firma (mismo nombre, mismos parámetros y mismo tipo de retorno compatible) para proporcionar una implementación específica y no tener que editar codigo ya existente. Cuando se llama a ese método a través de una referencia de la clase padre, se ejecuta la versión de la subclase, lo que permite el comportamiento polimórfico.


## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta
La **ligadura dinámica (o enlace tardío)** consiste en que la decisión sobre qué implementación de un método se ejecuta no se toma en tiempo de compilación, sino en tiempo de ejecución. Es decir, cuando se invoca un método sobre una referencia, el sistema determina en ese momento el tipo real del objeto y ejecuta la versión correspondiente del método. Esto permite que una misma llamada de método tenga comportamientos distintos según el objeto concreto.

La relación con el polimorfismo es directa, ya que la ligadura dinámica es el mecanismo que lo hace posible en lenguajes basados en objetos. Gracias a ella, una referencia de tipo general puede apuntar a objetos de distintas subclases, y la llamada al método adecuado se resuelve automáticamente en ejecución. Sin enlace tardío, el polimorfismo quedaría limitado o sería imposible de implementar de forma real.

En Java, la ligadura dinámica se aplica por defecto en los métodos de instancia (excepto métodos estáticos, privados o finales), sin necesidad de indicarlo explícitamente. Esto significa que el programador no debe hacer nada especial para activarla: basta con usar herencia y sobrescritura. En C++, en cambio, la ligadura dinámica no es la opción por defecto; es necesario usar la palabra clave `virtual` en la clase base para habilitarla, y así permitir que se resuelva la llamada en tiempo de ejecución.

En Python, el comportamiento es dinámico por naturaleza, ya que es un lenguaje interpretado y con tipado dinámico. Todos los métodos se resuelven en tiempo de ejecución automáticamente, por lo que el polimorfismo se obtiene sin necesidad de palabras clave específicas como `virtual`. Esto hace que el polimorfismo en Python sea más implícito, mientras que en C++ es más explícito y controlado, y en Java es intermedio, ya que se activa por defecto pero dentro de un sistema de tipos estático.


## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta
Se puede modelar una clase base `Soldado` con un método `saludar()`, que será redefinido en las subclases. En este caso, `Zapador` y `Artillero` heredan de `Soldado`, y `Zapador` sobrescribe completamente el comportamiento del método, mientras que `Artillero` puede mantener el comportamiento original o redefinirlo también si se desea. Esto permite observar cómo una misma llamada produce resultados distintos según el tipo real del objeto.

El polimorfismo se aprecia al trabajar con una referencia de tipo `Soldado` que puede apuntar a objetos de distintas subclases. Al recorrer una estructura de este tipo, la llamada al método `saludar()` se resuelve en tiempo de ejecución gracias a la ligadura dinámica, ejecutándose la versión correspondiente a cada objeto concreto. Esto evita tener que comprobar manualmente el tipo del objeto con estructuras condicionales.

Ejemplo en Java:

```java
class Soldado {
    public void saludar() {
        System.out.println("Soy un soldado genérico");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("Soy un zapador, especialista en ingeniería");
    }
}

class Artillero extends Soldado {
    public void saludar() {
        System.out.println("Soy un artillero, manejo la artillería");
    }
}

public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];

        ejercito[0] = new Soldado();
        ejercito[1] = new Zapador();
        ejercito[2] = new Artillero();

        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}
```


## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta
Sí, cuando un método es sobrescrito en una subclase, es posible invocar la implementación de la clase base y construir a partir de ella. Esto permite reutilizar parte del comportamiento original en lugar de sustituirlo completamente, lo cual es útil cuando se quiere extender o modificar ligeramente la funcionalidad heredada en lugar de redefinirla por completo.

En Java, esta invocación al método de la superclase se realiza mediante la palabra clave `super`. De esta forma, dentro del método sobrescrito se puede llamar a `super.metodo()` para ejecutar la versión original definida en la clase padre y después añadir comportamiento adicional en la subclase. Esto encaja con el principio de reutilización de código propio de la herencia.

En el caso del `Zapador`, se puede hacer que primero ejecute el saludo del `Soldado` y después añada su mensaje específico:

```java
class Soldado {
    public void saludar() {
        System.out.println("Soy un soldado genérico");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        super.saludar(); // llamada al método de la clase base
        System.out.println("ZAPADOR A SUS ORDENES");
    }
}
```


## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta
Al sobreescribir un método en Java, la firma del método debe mantenerse prácticamente igual que en la clase base. Esto implica que el nombre del método y la lista de parámetros deben ser exactamente los mismos, incluyendo el orden y los tipos de los parámetros. No es posible cambiar los tipos de los parámetros al sobrescribir, ya que eso no sería una redefinición del mismo método, sino la creación de uno nuevo.

Respecto al tipo de retorno, este debe ser el mismo o un subtipo (covariante) del tipo de retorno original. Es decir, si la clase base devuelve un tipo `A`, la subclase puede devolver un tipo más específico que herede de `A`, pero no uno más general ni incompatible. Esto mantiene la coherencia del polimorfismo y asegura que el código que usa la clase base siga funcionando correctamente.

La diferencia entre sobreescritura (overriding) y sobrecarga (overloading) es que la sobreescritura ocurre entre una clase base y una subclase, manteniendo la misma firma del método para redefinir su comportamiento, mientras que la sobrecarga ocurre dentro de la misma clase y consiste en definir varios métodos con el mismo nombre pero distintos parámetros. En el overriding se usa polimorfismo en tiempo de ejecución, mientras que en el overloading la decisión se toma en tiempo de compilación.

La anotación `@Override` se utiliza para indicar explícitamente que un método está siendo sobrescrito de una clase superior. Su uso no es obligatorio, pero es altamente recomendable porque permite al compilador detectar errores comunes, como cambios accidentales en la firma del método (por ejemplo, un error en el nombre o en los parámetros), lo que evitaría que realmente se esté sobrescribiendo el método. De este modo, mejora la seguridad y claridad del código.


## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta
Sí, en Java el polimorfismo se utiliza prácticamente desde el principio, aunque no siempre se mencione explícitamente con ese nombre. Desde los primeros ejercicios en los que se redefine el método `toString()`, ya se está aplicando polimorfismo mediante sobreescritura, ya que dicho método pertenece originalmente a la clase `Object` y es redefinido en las clases propias para cambiar su comportamiento.

De forma similar, al sobreescribir `equals()` también se está usando polimorfismo, porque se está proporcionando una implementación específica de un método heredado de la clase base `Object`. Cuando el sistema llama a `equals()` sobre una referencia de tipo `Object`, en realidad se ejecuta la versión correspondiente al tipo real del objeto gracias a la ligadura dinámica.

Por tanto, aunque el concepto se estudie de manera más formal más adelante, el polimorfismo está presente desde el inicio del aprendizaje de Java. Cada vez que se sobrescribe un método heredado y se invoca a través de una referencia de la clase base, ya se está aprovechando este mecanismo, incluso en métodos tan básicos como `toString()` o `equals()`.


## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta
Una clase abstracta es una clase que no se puede instanciar directamente y que se utiliza como base para otras clases. Su propósito es definir una estructura común y obligar a las subclases a implementar ciertos comportamientos. Puede contener tanto métodos concretos (con implementación) como métodos abstractos (sin implementación).

Un método abstracto es un método que se declara sin cuerpo, es decir, solo se define su firma, pero no su implementación. Este tipo de método obliga a las clases derivadas a proporcionar una implementación concreta. Si una clase contiene al menos un método abstracto, entonces la clase también debe declararse como abstracta.

No se pueden crear instancias de una clase abstracta, ya que está incompleta por definición. Su función es servir como plantilla para otras clases, no como un objeto directo. Por tanto, solo se pueden instanciar sus subclases concretas que hayan implementado todos sus métodos abstractos.

En el ejemplo, `Soldado` debe declararse como `abstract` porque contiene el método abstracto `atacar()`. Este método se implementa de forma diferente en cada subclase (`Zapador`, `Artillero`, etc.), mientras que el método `saludar()` puede mantenerse común o también ser sobrescrito si se desea. La palabra clave `abstract` se coloca tanto en la clase como en el método abstracto.

```java
abstract class Soldado {
    public void saludar() {
        System.out.println("Soy un soldado genérico");
    }

    public abstract void atacar();
}

class Zapador extends Soldado {
    @Override
    public void atacar() {
        System.out.println("El zapador coloca explosivos");
    }
}

class Artillero extends Soldado {
    @Override
    public void atacar() {
        System.out.println("El artillero dispara el cañón");
    }
}
```


## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta
La palabra clave `final` en Java impone restricciones que limitan la posibilidad de modificación y extensión. Cuando se aplica a una clase, significa que dicha clase no puede ser heredada, por lo que no se pueden crear subclases a partir de ella. Cuando se aplica a un método, significa que ese método no puede ser sobrescrito en ninguna subclase, es decir, su implementación queda fija.

Esto se relaciona directamente con el polimorfismo, ya que lo restringe. El polimorfismo depende de la herencia y de la sobrescritura de métodos para poder ejecutar diferentes comportamientos según el tipo real del objeto. Si una clase es `final`, se elimina la posibilidad de extenderla y, por tanto, de aplicar polimorfismo basado en herencia. Si un método es `final`, se permite la herencia de la clase, pero se impide que ese comportamiento concreto sea modificado en subclases.

Un ejemplo de la API estándar de Java es la clase `String`, que es `final`. Esto significa que no se puede crear una clase que extienda `String`, lo que garantiza que su comportamiento sea seguro, consistente e inmutable. Esta decisión de diseño es importante porque permite optimizaciones internas y evita alteraciones no controladas en un tipo fundamental del lenguaje.


## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta
Las interfaces en Java son un tipo de referencia que define un conjunto de métodos que una clase debe implementar, pero sin proporcionar la implementación de dichos métodos (al menos en el modelo clásico que se suele estudiar al inicio). Su objetivo es establecer un contrato: **cualquier clase que implemente una interfaz se compromete a ofrecer ciertos comportamientos.**

Se parecen a las clases abstractas en que ambas pueden definir métodos que deben ser implementados por las subclases, pero existen diferencias importantes. Una clase abstracta puede tener estado (atributos), constructores y métodos con implementación, mientras que una interfaz tradicional se centra únicamente en definir comportamiento. Además, una clase solo puede heredar de una clase abstracta, pero puede implementar varias interfaces.

Efectivamente, una clase puede implementar más de una interfaz en Java, lo cual permite una forma de herencia múltiple de comportamiento. Esto es especialmente útil para combinar distintos “roles” o capacidades en una misma clase sin los problemas de la herencia múltiple de clases. De esta forma, las interfaces son una herramienta clave para el polimorfismo, ya que permiten tratar objetos de distintas clases de forma uniforme si comparten la misma interfaz.


## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta
En este diseño se utiliza una clase abstracta `Punto` que define el comportamiento común, pero obliga a que cada subtipo implemente su propia forma de calcular la distancia. Esto se hace porque la distancia en 2D y en 3D no se calcula exactamente igual, ya que cambia el número de coordenadas implicadas. De este modo, el método `calcularDistanciaA` se declara como abstracto y cada subclase (`Punto2D` y `Punto3D`) proporciona su implementación concreta.

Para garantizar que solo se calculan distancias entre puntos compatibles, se utiliza `instanceof` y posteriormente *downcasting*. Esto permite comprobar en tiempo de ejecución si ambos puntos pertenecen al mismo tipo (por ejemplo, ambos son 2D o ambos 3D) antes de realizar la operación. Aunque este enfoque no es el más elegante en diseño avanzado, es útil para entender cómo funciona la verificación de tipos en tiempo de ejecución junto con el polimorfismo.

La clase `Linea` se diseña para trabajar únicamente con referencias de tipo `Punto`, sin conocer su implementación concreta. Gracias al polimorfismo, puede almacenar dos puntos y calcular su longitud llamando al método `calcularDistanciaA`, dejando que cada objeto decida cómo calcularla. Esto permite que la misma clase funcione tanto con puntos 2D como 3D sin modificaciones.

```java
abstract class Punto {
    public abstract double calcularDistanciaA(Punto p);
}

class Punto2D extends Punto {
    double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double calcularDistanciaA(Punto p) {
        if (!(p instanceof Punto2D)) {
            throw new IllegalArgumentException("Incompatible: no es Punto2D");
        }
        Punto2D otro = (Punto2D) p; // downcasting
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

class Punto3D extends Punto {
    double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double calcularDistanciaA(Punto p) {
        if (!(p instanceof Punto3D)) {
            throw new IllegalArgumentException("Incompatible: no es Punto3D");
        }
        Punto3D otro = (Punto3D) p; // downcasting
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        double dz = this.z - otro.z;
        return Math.sqrt(dx * dx + dy * dy + dz * dz);
    }
}

class Linea {
    private Punto a;
    private Punto b;

    public Linea(Punto a, Punto b) {
        this.a = a;
        this.b = b;
    }

    public double longitud() {
        return a.calcularDistanciaA(b);
    }
}
```


## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta
La herencia de interfaces en Java consiste en que una interfaz puede extender a otra interfaz, heredando sus métodos y pudiendo añadir nuevos. De esta forma, se construyen jerarquías de comportamiento sin necesidad de clases concretas, definiendo contratos cada vez más específicos a partir de otros más generales. Esto permite reutilizar definiciones de métodos y organizar mejor las capacidades que se exigen a las clases.

En Java sí existe la herencia múltiple de interfaces, es decir, una interfaz puede extender varias interfaces al mismo tiempo. Esto no genera los problemas típicos de la herencia múltiple de clases, porque las interfaces no contienen estado ni implementación completa (en el modelo básico), sino únicamente definiciones de métodos. Por tanto, no hay conflicto de ambigüedad en la herencia de comportamiento.

En el ejemplo, se define una interfaz `Fichero` con un método para leer el contenido, y otra interfaz `FicheroEscribible` que extiende `Fichero` y añade operaciones adicionales como escribir y eliminar contenido. Esto muestra cómo una interfaz puede especializar otra ampliando su contrato.

```java
interface Fichero {
    String leer();
}

interface FicheroEscribible extends Fichero {
    void escribir(String contenido);
    void eliminar();
}
```
