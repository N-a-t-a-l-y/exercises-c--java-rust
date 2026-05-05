# Java: Datos, variables y tipos: por qué existen y cómo usarlos

---

## Antes de escribir una sola línea de código

Cuando aprendes programación, lo primero que te enseñan son variables, tipos y sintaxis.
El problema es que si no sabes *para qué sirve* eso, lo memorias sin entenderlo —
y cuando el lenguaje cambia, el aprendizaje no transfiere.

Este documento empieza desde antes: ¿qué problema resuelve un programa?

---

## ¿Qué hace un programa, en realidad?

Un programa recibe información, la transforma y produce un resultado.

Siempre. Sin excepción.

- Una calculadora recibe números, los opera y devuelve un resultado.
- Una red social recibe texto e imágenes, los guarda y los muestra a otros.
- Un videojuego recibe inputs del teclado, actualiza el estado del mundo y lo dibuja en pantalla.

Esa "información" que viaja dentro del programa se llama **dato**.
Y el problema central que resuelven las variables, los tipos y las estructuras de datos
es uno solo: **¿cómo guardamos, identificamos y manipulamos esos datos mientras el programa corre?**

---

## La analogía: el sistema de cajas de un almacén

Imagina un almacén logístico. Cuando llega mercancía, el almacén necesita:

1. Un **espacio físico** donde guardar cada cosa (la memoria del computador).
2. Una **etiqueta** en cada espacio para poder encontrarlo después (el nombre de la variable).
3. Un **tipo de caja** que determine qué cabe adentro y cuánto pesa (el tipo de dato).
4. A veces, una **estantería numerada** para guardar muchas cosas del mismo tipo (un array).
5. A veces, una **caja compuesta** con compartimientos para guardar un conjunto relacionado (un objeto).
6. Y un **letrero en la entrada** que le dice al almacenista qué tipo de cosas acepta esa zona (los genéricos `<>`).

Esta analogía no es decorativa, cada parte mapea exactamente a algo técnico que veremos.

---

## ¿Qué es un dato?

Un dato es cualquier valor que el programa necesita recordar o transformar:
un número, un texto, una respuesta sí/no, una fecha, una lista de usuarios.

En Java, como en casi cualquier lenguaje, cada dato tiene un **tipo**.
El tipo no es arbitrario: le dice a la máquina cuánta memoria reservar y qué operaciones
son válidas sobre ese valor.

No es lo mismo guardar el número `42` que guardar el texto `"42"`.
El número se puede sumar; el texto, no (al menos no de la misma forma).

---

## Variables: la etiqueta sobre la caja

Una **variable** es un nombre que le ponemos a un espacio en memoria para poder
referirnos a él desde el código.

```java
int edad = 25;
```

Esto le dice a Java tres cosas:

| Parte | Qué le dice a Java |
|---|---|
| `int` | "reserva una caja del tamaño de un entero" |
| `edad` | "llámala `edad` para que yo pueda encontrarla" |
| `= 25` | "y ponle este valor adentro ahora mismo" |

Sin el tipo, Java no sabe cuánta memoria reservar.
Sin el nombre, el programa no puede referirse a ese espacio después.
Sin el valor inicial, la caja existe pero está vacía, en Java eso puede ser un problema
si intentas usarla antes de asignarle algo.

---

## Tipos de datos en Java

Java divide los tipos en dos grandes familias.
Esta distinción es importante porque cambia cómo viaja el dato por el programa.

---

### Familia 1: Tipos primitivos (la caja contiene el valor directamente)

Son los tipos más básicos. La caja guarda el valor en sí, no una referencia a otro lugar.
Son 8 en total; los más comunes:

| Tipo | Qué guarda | Ejemplo | Tamaño |
|---|---|---|---|
| `int` | Enteros (sin decimales) | `42`, `-7`, `0` | 32 bits |
| `long` | Enteros grandes | `9_000_000_000L` | 64 bits |
| `double` | Decimales (punto flotante) | `3.14`, `-0.5` | 64 bits |
| `float` | Decimales (menos precisión) | `3.14f` | 32 bits |
| `boolean` | Solo verdadero o falso | `true`, `false` | 1 bit lógico |
| `char` | Un solo carácter | `'A'`, `'ñ'` | 16 bits |

```java
int puntos = 100;
double precio = 19.99;
boolean activo = true;
char inicial = 'N';
```

**Por qué importa que sean primitivos:** el valor viaja directamente.
Si haces `int b = a;`, Java copia el número. Modificar `b` no afecta a `a`.

---

### Familia 2: Tipos de referencia (la caja guarda una dirección, no el valor)

Aquí la caja no contiene el dato en sí, contiene la *dirección* de donde está el dato real.
Es como si la caja tuviera adentro un papel que dice "ve al pasillo 7, estante 3".

El tipo más común en esta familia es `String`:

```java
String nombre = "John Doe";
```

La variable `nombre` no contiene las letras directamente.
Contiene una referencia al objeto `String` que vive en otra parte de la memoria (el heap).

Esto explica por qué en Java comparas Strings con `.equals()` y no con `==`:
`==` compara si dos variables apuntan al **mismo lugar** en memoria,
no si los textos son iguales.

```java
String a = new String("hola");
String b = new String("hola");

System.out.println(a == b);       // false — apuntan a lugares distintos
System.out.println(a.equals(b));  // true  — el contenido es el mismo
```

---

## Datos compuestos: cuando necesitas más de una caja

A veces un solo valor no alcanza. Necesitas guardar una lista de cosas,
o un conjunto de datos relacionados que describen una entidad.

Para eso existen las **estructuras de datos compuestas**.

---

### Arrays: la estantería numerada `[]`

Un array es una secuencia de casillas del mismo tipo, numeradas desde 0.

Los corchetes `[]` tienen dos momentos de aparición:

**Momento 1: declarar que algo es un array:**

```java
int[] calificaciones;
```

El `[]` aquí le dice a Java: "esto no es un solo entero, es una fila de enteros".

**Momento 2: acceder a una posición específica:**

```java
calificaciones[0] = 90;   // asigna el valor 90 a la posición 0
int primera = calificaciones[0];  // lee la posición 0
```

El `[]` con un número adentro es como decirle al almacenista:
"ve a la estantería `calificaciones` y trae lo que está en el compartimiento número 0".

**Crear e inicializar un array completo:**

```java
// Las llaves {} aquí inicializan el contenido de la estantería
String[] frutas = {"mango", "fresa", "uva"};

System.out.println(frutas[0]);   // mango
System.out.println(frutas[2]);   // uva
System.out.println(frutas.length); // 3 — cuántas posiciones tiene
```

> **Nota sobre los índices:** el primer elemento siempre es `[0]`, no `[1]`.
> Un array de 3 elementos tiene posiciones `[0]`, `[1]` y `[2]`.
> Acceder a `[3]` en este caso lanza un `ArrayIndexOutOfBoundsException` — Java te avisa que saliste de la estantería.

---

### Objetos y clases — la caja compuesta con compartimientos `{}`

Cuando necesitas agrupar datos relacionados bajo un mismo nombre, usas una **clase**.
Una clase es el molde; un **objeto** es la caja creada a partir de ese molde.

```java
class Producto {
    String nombre;
    double precio;
    int stock;
}
```

Las llaves `{}` aquí definen el contenido del molde: qué compartimientos tiene esta caja.

Para crear un objeto (una caja concreta a partir del molde):

```java
Producto cafe = new Producto();
cafe.nombre = "Café tostado";
cafe.precio = 12500.0;
cafe.stock = 40;

System.out.println(cafe.nombre);  // Café tostado
System.out.println(cafe.precio);  // 12500.0
```

---

### Las llaves `{}`: el delimitador de espacios

En Java, las llaves tienen un solo trabajo conceptual: **marcar dónde empieza y dónde termina un bloque**.

Ese bloque puede ser:

```java
// El cuerpo de una clase
class MiClase {
    // todo lo que pertenece a MiClase vive aquí
}

// El cuerpo de un método
static void saludar() {
    System.out.println("Hola");
}

// El cuerpo de una condición
if (edad >= 18) {
    System.out.println("Mayor de edad");
}

// Los valores iniciales de un array
int[] numeros = {10, 20, 30};
```

Regla simple: **cada `{` tiene exactamente un `}` que lo cierra**.
Cuando un programa falla por símbolo raro, lo primero que revisas es que los pares de llaves estén completos.

---

### Genéricos `<>`: el letrero de tipo en la entrada

Cuando usas estructuras más avanzadas como `ArrayList`,
necesitas decirle a Java qué tipo de datos va a contener.
Para eso existen los ángulos `<>`.

```java
import java.util.ArrayList;

ArrayList<String> nombres = new ArrayList<>();
nombres.add("Dahiana");
nombres.add("Carlos");

System.out.println(nombres.get(0));  // Dahiana
```

El `<String>` no guarda datos: es una **anotación al compilador**.
Le dice: "esta lista solo acepta `String`. Si intentas meter un `int`, avísame antes de correr el programa".

Esto es lo que se llama **genérico** o tipo paramétrico.
La misma estructura `ArrayList` puede trabajar con cualquier tipo, el `<>` especifica cuál.

```java
ArrayList<Integer> edades = new ArrayList<>();   // solo enteros
ArrayList<Double>  precios = new ArrayList<>();   // solo decimales
ArrayList<String>  tags    = new ArrayList<>();   // solo texto
```

> **Por qué `Integer` y no `int`:** Los genéricos en Java solo aceptan tipos de referencia,
> no primitivos. `Integer` es la versión "caja" de `int`, un objeto que envuelve el entero.
> Java hace la conversión automáticamente (autoboxing) cuando haces `.add(5)`.

---

## Los tres símbolos en contexto: resumen técnico

| Símbolo | Nombre | Qué hace técnicamente |
|---|---|---|
| `{}` | Llaves | Delimita un bloque: clase, método, condición o valores iniciales |
| `[]` | Corchetes | Declara un array (en el tipo) o accede a una posición (en el nombre) |
| `<>` | Ángulos | Parámetro de tipo para genéricos — solo le habla al compilador |

La clave: **el mismo símbolo puede tener varios roles según el contexto**.
`{}` en `{10, 20, 30}` inicializa valores. `{}` en `static void f() {}` delimita el cuerpo del método.
No son reglas absolutas del universo — son reglas que Java definió para ese símbolo.


---

## ¿Qué sigue?

Con esto ya tienes el mapa completo de cómo Java organiza y manipula datos:

- **Dato** → la información que el programa necesita.
- **Variable** → el nombre con el que le hablas a ese espacio en memoria.
- **Tipo primitivo** → la caja que contiene el valor directamente.
- **Tipo de referencia** → la caja que apunta a donde vive el dato real.
- **Array** → la estantería numerada para muchos datos del mismo tipo.
- **Clase / objeto** → la caja compuesta para datos relacionados de tipos distintos.
- **Genérico** → el letrero que le dice al compilador qué tipo acepta una estructura.

Los tres símbolos (`[]`, `{}`, `<>`) no son magia, son la sintaxis que Java escogió
para expresar estas ideas. Cuando los ves, pregúntate: ¿este símbolo está declarando algo,
agrupando algo, o anotando un tipo?