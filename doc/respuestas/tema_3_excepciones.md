<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

### Respuesta
En C no existen excepciones como en Java, por lo que el control de errores debe diseñarse explícitamente. Si se define una función `raiz(float x)` que solo admite números positivos, el error (recibir un número negativo) debe indicarse de alguna forma al código que llama a la función. Como el usuario debe ser informado desde fuera de la función, esta no debería imprimir directamente el error, sino comunicarlo mediante algún mecanismo convencional.

Una primera opción consiste en devolver un valor especial que indique error. Por ejemplo, se puede devolver `-1` (si se decide que no es un resultado válido) o utilizar `NAN` (Not a Number), definido en `<math.h>`. El código que llama a la función debe comprobar ese valor antes de usar el resultado.

**Ejemplo**
```c
#include <stdio.h>
#include <math.h>

float raiz(float x) {
    if (x < 0) {
        return -1;  // valor especial que indica error
    }
    return sqrt(x);
}

int main() {
    float resultado = raiz(-9);

    if (resultado == -1) {
        printf("Error: no se puede calcular la raíz de un número negativo.\n");
    } else {
        printf("Resultado: %f\n", resultado);
    }

    return 0;
}
```

Una segunda opción más robusta consiste en devolver un código de estado y pasar el resultado mediante un parámetro por referencia (puntero). De esta forma, la función devuelve `0` si todo va bien y `1` si ocurre un error. Este diseño separa claramente el valor calculado del estado de la operación.

**Ejemplo**
```c
#include <stdio.h>
#include <math.h>

int raiz(float x, float *resultado) {
    if (x < 0) {
        return 1;  // código de error
    }
    *resultado = sqrt(x);
    return 0;  // sin error
}

int main() {
    float resultado;
    int estado = raiz(-9, &resultado);

    if (estado != 0) {
        printf("Error: no se puede calcular la raíz de un número negativo.\n");
    } else {
        printf("Resultado: %f\n", resultado);
    }

    return 0;
}
```

En ambos casos, el control del error se realiza fuera de la función `raiz`, que únicamente comunica que algo ha ido mal. La diferencia principal es que la primera opción mezcla resultado y error en el mismo valor devuelto, mientras que la segunda separa claramente el resultado del código de estado, lo que suele considerarse una solución más clara y segura.


## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta
Una **excepción** es un mecanismo de control de errores que permite señalar que durante la ejecución de un programa ha ocurrido una situación anómala o inesperada. A diferencia de los métodos tradicionales en C (como devolver códigos de error), una excepción interrumpe el flujo normal de ejecución y transfiere el control a un bloque especial preparado para gestionar ese problema. En lenguajes como Java, las excepciones forman parte del propio modelo del lenguaje.

El objetivo principal de las excepciones es separar el código que realiza la lógica principal del programa del código que gestiona los errores. Cuando se implementa una función, el programador puede lanzar una excepción si se detecta una condición inválida (por ejemplo, recibir un parámetro incorrecto). De esta forma, la función no necesita decidir cómo informar al usuario; simplemente comunica que ha ocurrido un problema.

Cuando se llama a una función, el programador utiliza excepciones para capturar y tratar esos errores de forma estructurada. Esto permite reaccionar adecuadamente (mostrar un mensaje, intentar una alternativa, finalizar el programa, etc.) sin mezclar continuamente comprobaciones de error con la lógica normal. En resumen, las excepciones se utilizan para hacer más claro, organizado y seguro el control de errores en programas.



## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta
En Java el control de errores puede realizarse mediante excepciones. En este caso, si el método que calcula la raíz recibe un número negativo, en lugar de devolver un valor especial como en C, puede lanzar una excepción. De esta manera, el método solo se encarga de detectar el error y comunicarlo, mientras que el control se realiza desde fuera, por ejemplo, en el `main`.

Se puede definir una clase `Calculadora` que contenga el método `raiz`. Si el parámetro es negativo, se lanza una excepción de tipo `IllegalArgumentException`, que es apropiada cuando un método recibe un argumento no válido. El método `main` será el encargado de capturar esa excepción usando un bloque `try-catch`.

```java
public class Calculadora {

    public static double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException(
                "No se puede calcular la raíz de un número negativo"
            );
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        try {
            double resultado = Calculadora.raiz(-9);
            System.out.println("Resultado: " + resultado);
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

En este diseño, el método `raiz` no imprime mensajes ni decide cómo actuar ante el error; simplemente lanza la excepción. El método `main`, que está fuera del método que realiza el cálculo, captura la excepción y muestra el mensaje correspondiente. Así se logra una clara separación entre la lógica del cálculo y el control del error, que es precisamente uno de los objetivos del uso de excepciones en Java.


## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta
**Lanzar** una excepción significa generar un objeto de tipo excepción y enviarlo al sistema de ejecución para indicar que ha ocurrido un error. En Java esto se hace con la palabra clave `throw`. En el ejemplo de la raíz cuadrada, cuando el método `raiz` detecta que el número es negativo, ejecuta `throw new IllegalArgumentException(...)`. En ese momento, el flujo normal del método se interrumpe inmediatamente y no se ejecuta ninguna instrucción posterior dentro de ese método.

**Controlar** o **capturar** una excepción significa interceptarla mediante un bloque `try-catch`. El código que puede producir el error se coloca dentro del `try`, y el `catch` define cómo reaccionar si se lanza esa excepción. En el ejemplo anterior, el `main` llama a `Calculadora.raiz(-9)` dentro de un `try`, y el `catch` recoge la `IllegalArgumentException` para mostrar un mensaje. De esta manera, el error se gestiona fuera del método que lo produjo.

Una excepción se **propaga** cuando un método la lanza pero no la captura. En ese caso, la excepción sube al método que lo llamó, y así sucesivamente por la pila de llamadas. Siguiendo el ejemplo, si `raiz` lanza la excepción y `main` no la capturara, la excepción seguiría propagándose hacia arriba. Mientras se propaga, cada método intermedio finaliza inmediatamente sin continuar su ejecución normal. Es decir, se van “desapilando” uno a uno los métodos activos hasta encontrar un `catch` adecuado o hasta que se llegue al final del programa.

Las funciones que no controlan la excepción **no se reanudan después**. Una vez que la excepción abandona un método, este termina abruptamente y no continúa desde donde se quedó. Si nadie captura la excepción, el programa finaliza mostrando un mensaje de error y la traza de la pila (stack trace). Por tanto, la propagación implica la interrupción progresiva de los métodos en la pila hasta que alguien gestione la situación o el programa se detenga.


## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta
**La propagación natural** de las excepciones en Java permite que el control de errores se transfiera automáticamente a la parte del programa que esté preparada para manejarlo, sin necesidad de comprobar explícitamente códigos de error en cada función intermedia. En C, si un error ocurre, cada función de la cadena de llamadas tendría que verificar manualmente un valor de retorno o un puntero de estado para decidir si continuar o devolver un error. Esto aumenta la complejidad y hace que el código sea más propenso a olvidos o errores.

Otra ventaja es que la propagación de excepciones separa claramente la lógica principal de la aplicación de la gestión de errores. Mientras que en C es común intercalar múltiples `if` para chequear errores después de cada llamada, en Java la excepción viaja por la pila hasta que se encuentra un bloque `catch` adecuado. Esto permite escribir métodos más limpios y enfocados solo en la tarea que deben realizar, dejando que la detección y el tratamiento de errores se haga en un nivel superior más apropiado.

Además, la propagación facilita la reutilización y el mantenimiento del código. Si una función se modifica o se reutiliza en otro contexto, no es necesario ajustar cada comprobación de error en todos los llamadores intermedios; la excepción seguirá propagándose hasta un bloque que la gestione correctamente. Esto reduce duplicación de código y disminuye la probabilidad de introducir fallos al modificar la lógica.

En resumen, la propagación natural hace que el manejo de errores sea más consistente, claro y seguro, comparado con C, donde se requiere que cada función intermedia participe activamente en la gestión de códigos de error, aumentando la carga cognitiva y el riesgo de errores.

**Ejemplo**
Un ejemplo pequeño que muestra la **propagación natural de excepciones** en Java puede ser una cadena de métodos donde solo el `main` captura la excepción. Esto demuestra que los métodos intermedios no necesitan comprobar ni manejar el error; la excepción viaja automáticamente por la pila hasta el `catch`.

```java
public class EjemploPropagacion {

    public static void metodo1(double x) {
        metodo2(x);
    }

    public static void metodo2(double x) {
        if (x < 0) {
            throw new IllegalArgumentException("Número negativo no permitido");
        }
        System.out.println("Raíz: " + Math.sqrt(x));
    }

    public static void main(String[] args) {
        try {
            metodo1(-4);  // Lanza la excepción desde metodo2
        } catch (IllegalArgumentException e) {
            System.out.println("Error capturado en main: " + e.getMessage());
        }
    }
}
```

En este ejemplo, `metodo2` lanza la excepción, `metodo1` **no la captura**, y el `main` la recibe y la controla. Los métodos intermedios terminan inmediatamente y **no se reanudan**, mostrando cómo la propagación permite centralizar el manejo de errores sin repetir comprobaciones en cada función.


## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta
En orientación a objetos, las excepciones **son objetos** de clases que generalmente heredan de `Exception` o de sus subclases en Java. Esto significa que cada excepción no es solo un valor o código, sino un objeto con propiedades y métodos que pueden contener información adicional sobre el error, como un mensaje descriptivo, la causa original del problema o incluso datos específicos del contexto donde ocurrió la excepción.

Una ventaja importante de que las excepciones sean objetos es que se integra perfectamente con el principio de **encapsulación**. Los detalles internos del error se ocultan dentro del objeto excepción y solo se expone lo necesario a través de métodos públicos, como `getMessage()` o `getCause()`. De esta forma, el código que captura la excepción no necesita conocer la lógica interna de cómo se generó el error, solo interactúa con la interfaz pública de la excepción, manteniendo separada la lógica de negocio de la gestión de errores.

Además, al ser objetos, es posible **crear excepciones personalizadas** definiendo nuevas clases que extiendan `Exception` o `RuntimeException`. Esto permite modelar situaciones de error específicas del dominio de la aplicación y diferenciarlas de las excepciones genéricas del lenguaje. Por ejemplo, se podría crear una excepción `NumeroNegativoException` para manejar exclusivamente errores relacionados con valores negativos en operaciones matemáticas, haciendo que el código sea más claro y autodescriptivo.

```java
class NumeroNegativoException extends Exception {
    public NumeroNegativoException(String mensaje) {
        super(mensaje);
    }
}
```

Con esta técnica, se puede lanzar y capturar `NumeroNegativoException` en lugar de usar excepciones genéricas, manteniendo el control del error encapsulado y específico.


## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta
En Java, las excepciones como objetos encapsulan información que no estaba disponible en el diseño de C con códigos de error. Esto permite que el manejador de errores obtenga datos completos sobre lo que ocurrió sin que el método que lanza la excepción tenga que exponer sus detalles internos. Por ejemplo, cada objeto excepción contiene un **mensaje descriptivo**, accesible mediante `getMessage()`, que explica la causa del error. Esto es equivalente a la notificación de error en C, pero mucho más estructurado y seguro, porque no depende de valores mágicos ni de convenciones externas.

Otra información útil que encapsula una excepción es la **traza de la pila** (stack trace), que se puede obtener con `printStackTrace()`. Esta traza muestra la secuencia exacta de métodos que se llamaron antes de que ocurriera la excepción, incluyendo el método que la lanzó y todos los métodos intermedios (sirve para depurar). En C, esta información no existe de forma automática; habría que implementarla manualmente, lo que es más propenso a errores y difícil de mantener. Con Java, el manejador puede utilizarla para depuración, registro o diagnóstico sin interferir con la lógica del programa principal.

Además, cualquier excepción puede encapsular un **objeto causa** (`getCause()`), que permite enlazar excepciones cuando una falla depende de otra. Esto es especialmente útil en aplicaciones complejas donde un error de bajo nivel provoca uno de alto nivel, y el manejador puede acceder a ambos contextos para decidir cómo reaccionar. En resumen, la encapsulación de información en objetos excepción aporta claridad, seguridad y un acceso estructurado a los datos del error, facilitando un manejo más preciso que los códigos de error simples de C.

Un ejemplo sencillo que muestra cómo se puede usar `getCause()` en Java consiste en encadenar excepciones. Por ejemplo, si un método de cálculo lanza un error y otro método de nivel superior lo detecta y lanza una excepción propia, se puede pasar la excepción original como causa. Esto permite que el manejador final conozca ambos errores y trace la secuencia completa del problema.

```java
class NumeroNegativoException extends Exception {
    public NumeroNegativoException(String mensaje, Throwable causa) {
        super(mensaje, causa);
    }
}

public class EjemploCausa {

    public static void raiz(double x) throws NumeroNegativoException {
        if (x < 0) {
            throw new IllegalArgumentException("Número negativo en raiz");
        }
        System.out.println("Raíz: " + Math.sqrt(x));
    }

    public static void calculo(double x) throws NumeroNegativoException {
        try {
            raiz(x);
        } catch (IllegalArgumentException e) {
            // Encapsula la excepción original como causa
            throw new NumeroNegativoException("Error en calculo de raíz", e);
        }
    }

    public static void main(String[] args) {
        try {
            calculo(-5);
        } catch (NumeroNegativoException e) {
            System.out.println("Mensaje: " + e.getMessage());
            System.out.println("Causa original: " + e.getCause());
            e.printStackTrace();
        }
    }
}
```

En este ejemplo, `raiz` lanza una `IllegalArgumentException` al recibir un número negativo. El método `calculo` captura esa excepción y lanza una nueva `NumeroNegativoException`, pasando la excepción original como **causa**. En el `main`, el manejador puede acceder al mensaje de la excepción, conocer la causa original con `getCause()` y también imprimir toda la traza de la pila. Esto facilita el diagnóstico del error, mostrando no solo el error de alto nivel sino también el origen exacto del problema.


## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta
En Java, sí se pueden tener **varios bloques** `catch` asociados a un mismo bloque `try`. Esto permite manejar diferentes tipos de excepciones de manera diferenciada. Cada `catch` declara el tipo de excepción que puede capturar, por lo que se puede reaccionar de forma específica según la naturaleza del error. Esta característica es útil cuando un bloque `try` puede lanzar más de un tipo de excepción y se desea aplicar un tratamiento distinto para cada caso.

Cuando se lanza una excepción dentro del `try`, **solo se ejecuta el primer bloque `catch` cuyo tipo coincida con la excepción lanzada**. Los bloques `catch` posteriores no se ejecutan. Por tanto, el flujo de control pasa directamente desde el `try` al bloque `catch` correspondiente y, tras finalizar ese `catch`, continúa después de todo el conjunto `try-catch`.

Es importante ordenar los bloques `catch` de los tipos más específicos a los más generales, porque Java verifica los `catch` en orden y no permite que un bloque más general preceda a uno más específico si ambos podrían coincidir con la misma excepción. Por ejemplo, un `catch (Exception e)` debe ir al final, después de `catch (IllegalArgumentException e)` u otras excepciones derivadas. Esto asegura que cada excepción sea capturada de manera correcta y evita errores de compilación.

```java
try {
    double resultado = Calculadora.raiz(-4);
} catch (IllegalArgumentException e) {
    System.out.println("Error de argumento: " + e.getMessage());
} catch (Exception e) {
    System.out.println("Error general: " + e.getMessage());
}
```

En este ejemplo, si se lanza `IllegalArgumentException`, **solo se ejecuta el primer `catch`**, y el segundo se ignora.


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta
Para garantizar que se ejecuta siempre un código necesario, como cerrar ficheros o liberar recursos, Java ofrece el bloque **`finally`**, que se ejecuta **siempre**, independientemente de si se lanza o se captura una excepción. Esto asegura que las operaciones de limpieza se realicen incluso cuando una excepción provoca la interrupción del flujo normal. El bloque `finally` se coloca después de los bloques `try` y `catch`, y puede acompañarlos o existir solo con `try`.

Cuando se utiliza `try` junto con `catch`, primero se intenta ejecutar el código dentro del `try`. Si ocurre una excepción, se busca un `catch` adecuado y se ejecuta. En cualquier caso, el bloque `finally` se ejecuta después, antes de que el control continúe fuera del bloque o antes de que la excepción siga propagándose. Esto garantiza que los recursos críticos no queden abiertos ni en un estado inconsistente.

```java
import java.io.FileReader;
import java.io.IOException;

public class EjemploFinally {
    public static void main(String[] args) {
        FileReader lector = null;
        try {
            lector = new FileReader("archivo.txt");
            // Operaciones de lectura...
            int c = lector.read();
            System.out.println("Primer carácter: " + c);
        } catch (IOException e) {
            System.out.println("Error al leer el archivo: " + e.getMessage());
        } finally {
            // Este código se ejecuta siempre
            if (lector != null) {
                try {
                    lector.close();
                    System.out.println("Archivo cerrado correctamente");
                } catch (IOException e) {
                    System.out.println("Error al cerrar el archivo");
                }
            }
        }
    }
}
```

También es posible usar `finally` **sin `catch`**, por ejemplo, si solo se desea garantizar la liberación de recursos pero dejar que la excepción se propague hacia arriba.

```java
public class FinallySinCatch {
    public static void metodo() throws Exception {
        try {
            throw new Exception("Error simulado");
        } finally {
            System.out.println("Bloque finally ejecutado antes de propagar la excepción");
        }
    }

    public static void main(String[] args) {
        try {
            metodo();
        } catch (Exception e) {
            System.out.println("Excepción capturada en main: " + e.getMessage());
        }
    }
}
```

En este caso, aunque no haya un `catch` en el método `metodo`, el bloque `finally` se ejecuta **antes de que la excepción salga del método**, asegurando la limpieza o cierre de recursos. Esto demuestra que `finally` es la herramienta de Java para garantizar la ejecución de código crítico frente a excepciones.


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta
Sí, en Java un bloque **`finally` puede ir sin `catch`**, es decir, se puede tener un `try` seguido directamente de `finally`. Esto se utiliza cuando no se desea capturar la excepción en ese nivel, pero sí garantizar que cierto código de limpieza o liberación de recursos se ejecute siempre. La sintaxis válida sería:

```java
try {
    // código que puede lanzar excepción
} finally {
    // código que siempre se ejecuta
}
```

El bloque `finally` **se ejecuta siempre**, tanto si ocurre una excepción como si no ocurre. Incluso si dentro del `try` se produce una excepción que no se captura allí mismo, el `finally` se ejecuta antes de que la excepción se propague hacia el método que llamó. Esto asegura que recursos como ficheros, conexiones de base de datos o memoria temporal se liberen de manera confiable.

Incluso si hay un `return` **dentro del** `try`, el bloque `finally` se ejecuta antes de que la función realmente devuelva el valor. Es decir, el `finally` se ejecuta incluso cuando se interrumpe el flujo normal por un `return`. Esto es útil para garantizar que la limpieza se haga aunque la función termine anticipadamente.

```java
public class FinallyConReturn {
    public static int ejemplo() {
        try {
            System.out.println("Dentro del try");
            return 1;  // retorna un valor, pero el finally se ejecuta antes
        } finally {
            System.out.println("Finalmente siempre se ejecuta");
        }
    }

    public static void main(String[] args) {
        int resultado = ejemplo();
        System.out.println("Resultado devuelto: " + resultado);
    }
}
```

En este ejemplo, se ve que el mensaje del `finally` se imprime antes de que el valor `1` sea retornado al `main`. Esto garantiza que el código crítico dentro de `finally` siempre se ejecute, incluso si se produce una salida anticipada del método por `return` o por una excepción.


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta
En Java, las excepciones se clasifican en **controladas (checked)** y **no controladas (unchecked)** según el momento en que el compilador exige su manejo. Las **excepciones controladas** (todas menos las `RunTimeException`) son aquellas que el compilador **obliga a capturar o declarar con `throws` en la firma del método**. Generalmente representan situaciones que pueden ocurrir de manera previsible y recuperable, como errores de E/S o problemas con ficheros. Por el contrario, las **excepciones no controladas** heredan de `RuntimeException` y representan errores de programación que normalmente no se esperan ni se pueden manejar de manera rutinaria, como errores de índice, punteros nulos o violaciones de lógica.

El papel de `RuntimeException` es servir como base de todas las excepciones no controladas. Esto incluye, por ejemplo, `NullPointerException` o `IndexOutOfBoundsException`. El compilador no obliga a capturarlas ni a declararlas, lo que permite escribir código más limpio, asumiendo que estas situaciones reflejan errores que deberían corregirse durante el desarrollo y no gestionarse rutinariamente en producción.

Algunos ejemplos de **excepciones controladas** típicas son:

* `IOException` – cuando se trabaja con ficheros o streams de datos.
* `FileNotFoundException` – al intentar abrir un fichero que no existe.
* `SQLException` – errores al acceder a bases de datos.
* `ClassNotFoundException` – al cargar clases dinámicamente.

Algunos ejemplos de **excepciones no controladas** típicas son:

* `NullPointerException` – al intentar usar un objeto que es `null`.
* `ArithmeticException` – división por cero.
* `IndexOutOfBoundsException` – acceso fuera de los límites de un array o lista.
* `IllegalArgumentException` – parámetros inválidos pasados a un método.

Situaciones donde suele preferirse **una excepción controlada**:

**1.** Lectura de un fichero que puede no existir (`FileNotFoundException`).
**2.** Acceso a una base de datos (`SQLException`).
**3.** Conversión de un archivo de entrada que puede fallar (`IOException`).
**4.** Carga dinámica de clases externas (`ClassNotFoundException`).

Situaciones donde suele preferirse **una excepción no controlada**:

**1.** Acceder a un índice inválido de un array (`IndexOutOfBoundsException`).
**2.** Operaciones aritméticas imposibles, como dividir por cero (`ArithmeticException`).
**3.** Uso de objetos no inicializados (`NullPointerException`).
**4.** Pasar argumentos inválidos a un método propio (`IllegalArgumentException`).

En resumen, **las controladas se usan cuando el error puede esperarse y recuperarse**, y **las no controladas cuando reflejan errores de programación que deberían corregirse**, dejando que la excepción termine el flujo si no se maneja. Esto permite un diseño más claro y seguro del código.


## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta
En Java, la palabra clave **`throws`** se utiliza en la declaración de un método para indicar que ese método **puede lanzar una o varias excepciones controladas** durante su ejecución. Al usar `throws`, el método no captura la excepción internamente, sino que **propaga la responsabilidad** de gestionarla a quien llame al método. Esto permite que la función que lanza la excepción se concentre en su lógica principal, mientras que el código de manejo del error se define en un nivel superior donde es más apropiado.

Usar `throws` es una alternativa a capturar la excepción dentro del mismo método. En lugar de colocar un bloque `try-catch` interno, se declara que el método puede lanzar la excepción, lo que obliga a los llamadores a decidir cómo tratarla, ya sea capturándola con `try-catch` o propagándola nuevamente con `throws`. Esto favorece la **separación de responsabilidades**, haciendo que el código del método sea más limpio y que el manejo de errores se centralice donde se tenga contexto suficiente para tomar decisiones sobre la recuperación o notificación del error.

Por ejemplo, un método que lee un fichero puede declarar `throws IOException` en lugar de capturarla:

```java
import java.io.FileReader;
import java.io.IOException;

public class EjemploThrows {
    public static void leerArchivo(String nombre) throws IOException {
        FileReader lector = new FileReader(nombre);
        int c = lector.read();
        System.out.println("Primer carácter: " + c);
        lector.close();
    }

    public static void main(String[] args) {
        try {
            leerArchivo("archivo.txt");  // Aquí se captura la excepción
        } catch (IOException e) {
            System.out.println("Error al leer el archivo: " + e.getMessage());
        }
    }
}
```

En este ejemplo, `leerArchivo` no maneja la excepción dentro de sí mismo, sino que la **propaga** al `main` mediante `throws`. Esto permite que el método se enfoque en la operación de lectura y deja que el llamador decida cómo reaccionar frente al error, en lugar de obligarlo a manejarlo inmediatamente.


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa manejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta
Se puede definir un método que abra un fichero y lea algún dato, **declarando con `throws`** que puede producirse una excepción `IOException` si el fichero no existe o hay un error de E/S. De esta manera, el método no captura la excepción; simplemente la propaga al llamador. Sin embargo, se puede usar un bloque `finally` para garantizar que ciertos recursos, como el cierre del fichero, siempre se realicen, aunque la excepción siga su propagación.

```java
import java.io.FileReader;
import java.io.IOException;

public class LecturaArchivo {

    public static void abrirArchivo(String nombreArchivo) throws IOException {
        FileReader lector = null;
        try {
            lector = new FileReader(nombreArchivo);
            int c = lector.read();
            System.out.println("Primer carácter: " + c);
        } finally {
            // Se ejecuta siempre, incluso si se lanza la excepción
            if (lector != null) {
                try {
                    lector.close();
                    System.out.println("Archivo cerrado en finally");
                } catch (IOException e) {
                    System.out.println("Error al cerrar el archivo");
                }
            }
        }
    }

    public static void main(String[] args) {
        try {
            abrirArchivo("archivoInexistente.txt");
        } catch (IOException e) {
            System.out.println("Excepción capturada en main: " + e.getMessage());
        }
    }
}
```

En este ejemplo:

* La firma del método `abrirArchivo` incluye `throws IOException`, lo que indica que **no maneja la excepción internamente**.
* El bloque `finally` asegura que el fichero se cierre siempre, independientemente de si se lanza la excepción.
* La excepción se captura finalmente en el `main`, que es el nivel adecuado para decidir cómo reaccionar ante un archivo inexistente.

Esto combina la **propagación de excepciones** con la **garantía de limpieza de recursos**, una práctica típica en Java para operaciones de E/S.


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta
Sí, en Java **se puede incluir una excepción no controlada (unchecked), como `RuntimeException`, en la cláusula `throws`**, aunque no es obligatorio. El compilador no exige que las excepciones que heredan de `RuntimeException` se declaren ni que se capturen, porque estas suelen representar errores de programación que deberían corregirse, como `NullPointerException` o `ArithmeticException`. Por lo tanto, un método puede declarar `throws RuntimeException` solo como información adicional para quien lo lea, pero no cambia el comportamiento del compilador.

El método llamador **no está obligado a capturar una excepción no controlada**. Puede usar `try-catch` si desea anticiparse a un posible error en tiempo de ejecución y manejarlo de forma controlada, pero normalmente no se hace. Esto tiene sentido en situaciones donde se quiere dar un tratamiento específico a errores de lógica previsibles, aunque sean errores de programación, sin obligar al código a lidiar con ellos en todos los casos. Por ejemplo, un método que valida parámetros podría lanzar `IllegalArgumentException` y el llamador podría capturarlo si desea ofrecer un mensaje más amigable al usuario.

```java
public class EjemploRuntime {

    public static void dividir(int a, int b) throws ArithmeticException {
        if (b == 0) {
            throw new ArithmeticException("División por cero");
        }
        System.out.println("Resultado: " + (a / b));
    }

    public static void main(String[] args) {
        // No es obligatorio capturar, pero se puede
        try {
            dividir(10, 0);
        } catch (ArithmeticException e) {
            System.out.println("Capturada: " + e.getMessage());
        }
    }
}
```

En este ejemplo, el método `dividir` declara `throws ArithmeticException` (unchecked), pero incluso si se eliminara la cláusula `throws`, el código seguiría compilando y funcionando igual. La ventaja de declararla es **documentar intencionalmente que el método puede producir esta excepción**, ayudando al mantenedor del código a entender los posibles riesgos y facilitando decisiones sobre manejo opcional con `try-catch`.


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta
Se recomienda usar **excepciones controladas** cuando el error es **previsible y recuperable** durante la ejecución normal del programa, y cuando se espera que el llamador pueda reaccionar de manera razonable. Por ejemplo, errores de E/S como `IOException`, `FileNotFoundException` o problemas de conexión a bases de datos son situaciones donde es habitual que el código que llama pueda intentar una acción alternativa, notificar al usuario o reintentar la operación. Estas excepciones obligan al programador a pensar en el manejo de errores, promoviendo código más robusto frente a condiciones externas.

Por el contrario, las **excepciones no controladas**, como `IllegalArgumentException`, `NullPointerException` o `ArithmeticException`, se usan cuando el error **refleja una violación de la lógica interna del programa o de los contratos de los métodos**. No se espera que el llamador pueda recuperarse de ellas de manera rutinaria; más bien, indican fallos de programación que deberían corregirse durante el desarrollo. Este tipo de excepciones evita sobrecargar el código con comprobaciones constantes de errores que nunca deberían ocurrir si el programa está bien diseñado.

No todos los lenguajes ofrecen ambas opciones. Por ejemplo, C y C++ tradicional no tienen excepciones controladas; se gestionan errores mediante códigos de retorno o punteros especiales. En estos lenguajes, **la opción más habitual es un enfoque tipo "no controlada"** implícito, es decir, el código decide si ignora, comprueba o propaga un error, pero no existe un mecanismo obligatorio de captura como `checked exceptions` de Java. Por ello, la disciplina del programador y el diseño cuidadoso de funciones y valores de retorno son esenciales para evitar fallos silenciosos o propagación de errores inadvertidos.

En resumen, las **excepciones controladas** son útiles para errores externos previsibles y recuperables, mientras que las **no controladas** se usan para fallos de lógica internos. En lenguajes que solo tienen un tipo de manejo de errores (como C/C++), se tiende a usar códigos de retorno y propagación manual, que actúa de forma equivalente a las no controladas en cuanto a no obligar al llamador a capturarlas.


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta
Sí, tiene sentido **lanzar excepciones dentro de un bloque `catch`**, ya que a veces el manejador detecta que no puede resolver completamente el problema o que debe notificar un error más apropiado a un nivel superior. Esto permite transformar o **encapsular** la excepción original en otra más específica del contexto, o simplemente propagarla para que un método superior decida cómo manejarla.

También es posible **relanzar la misma excepción capturada**, usando `throw` dentro del `catch`. Esto se hace cuando el bloque `catch` realiza alguna acción adicional —como registrar el error, liberar recursos o imprimir un mensaje— pero no puede resolver la situación y desea que la excepción continúe propagándose hacia arriba. Esto es útil para no ocultar el error mientras se registra o se realiza limpieza parcial.

**Ejemplo 1: Lanzar una nueva excepción desde el catch**

```java
public static void ejemploTransformacion() throws Exception {
    try {
        int resultado = 10 / 0;
    } catch (ArithmeticException e) {
        // Transformar la excepción a un tipo más general o específico de la aplicación
        throw new Exception("Error en operación matemática", e);
    }
}
```

En este caso, el `catch` captura un `ArithmeticException` pero lanza una nueva excepción con un mensaje más descriptivo, pasando la original como causa (`e`).
En este caso, será la causa.

**Ejemplo 2: Relanzar la misma excepción capturada**

```java
public static void ejemploRethrow() throws ArithmeticException {
    try {
        int resultado = 10 / 0;
    } catch (ArithmeticException e) {
        System.out.println("Se detectó división por cero, registrando...");
        throw e;  // Relanzar la misma excepción
    }
}
```

Aquí, el bloque `catch` realiza un registro y luego relanza la **misma excepción** para que el llamador pueda decidir cómo manejarla. Esto tiene sentido cuando se necesita **combinar logging o limpieza parcial** con propagación, sin ocultar el error original.

**Ejemplo 3: Lanzar otra excepcion totalmente nueva**

```java
public static void ejemploTransformacion() throws Exception {
    try {
        int resultado = 10 / 0;
    } catch (ArithmeticException e) {
        // Lanzar una excepcion totalmente nueva
        throw new AplicationException("Error");
    }
}
```

En resumen, lanzar una excepción dentro del `catch` permite **modificar, encapsular o relanzar** la situación de error, mientras que relanzar la misma excepción es útil para **mantener la trazabilidad y dejar que un nivel superior gestione el error**.



## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta
Que una excepción sea la **causa** de otra significa que una excepción de nivel superior encapsula a otra de nivel inferior, estableciendo un **vínculo entre el error original y el error que se comunica al llamador**. Esto permite mantener la trazabilidad del problema: el manejador de nivel superior puede conocer no solo que ocurrió un error, sino también qué lo originó. En Java, esto se hace pasando la excepción original como parámetro al constructor de la nueva excepción usando `Throwable cause`.

Por ejemplo, si un método de bajo nivel lanza `IOException` al leer un fichero, un método de nivel superior podría capturarla y lanzar una excepción personalizada `ErrorLecturaArchivoException`, pasando la `IOException` como causa:

```java
class ErrorLecturaArchivoException extends Exception {
    public ErrorLecturaArchivoException(String mensaje, Throwable causa) {
        super(mensaje, causa);
    }
}

public class EjemploCausa {

    public static void leerArchivo(String nombre) throws ErrorLecturaArchivoException {
        try {
            throw new java.io.IOException("Fichero no encontrado");
        } catch (java.io.IOException e) {
            // Encapsular la excepción original como causa
            throw new ErrorLecturaArchivoException("Error al procesar el archivo", e);
        }
    }

    public static void main(String[] args) {
        try {
            leerArchivo("archivoInexistente.txt");
        } catch (ErrorLecturaArchivoException e) {
            System.out.println("Mensaje: " + e.getMessage());
            System.out.println("Causa original: " + e.getCause());
            e.printStackTrace();
        }
    }
}
```

Cuando se imprime la excepción con `printStackTrace()`, **se muestra la causa**. La salida incluye primero la excepción de alto nivel (`ErrorLecturaArchivoException`) y, debajo, la excepción original (`IOException`) con su propia traza. Esto permite ver toda la cadena de errores desde el nivel superior hasta el error inicial, facilitando la depuración y comprensión de la secuencia exacta de fallos que provocaron la excepción final. Se puede ver rapidamente con el metodo `getCause()`

En resumen, la relación de causa permite **encapsular errores de bajo nivel** en excepciones de alto nivel, manteniendo toda la información sobre el origen del problema y ofreciendo mayor claridad al manejar errores complejos.