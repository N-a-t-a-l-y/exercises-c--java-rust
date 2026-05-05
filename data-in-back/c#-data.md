# C#: Datos, variables y tipos: por qué existen y cómo usarlos

> Si ya leíste el documento de Java, este te va a resultar familiar en la lógica
> pero diferente en los detalles. C# y Java comparten muchas ideas porque ambos
> aprendieron del mismo lenguaje base (C), pero tomaron decisiones distintas.
> Las diferencias importan, y este documento las señala explícitamente.

---

## El mismo problema, otro lenguaje

Un programa recibe información, la transforma y produce un resultado.
Eso no cambia entre lenguajes.

Lo que cambia es cómo cada lenguaje decide expresar esas ideas:
cómo nombra los tipos, cómo organiza el código, qué decisiones toma por ti
y cuáles te deja tomar a ti.

C# es un lenguaje desarrollado por Microsoft, diseñado para correr sobre la plataforma **.NET**.
Es fuertemente tipado, igual que Java, lo que significa que cada dato tiene un tipo
declarado y el compilador verifica que lo uses correctamente antes de ejecutar el programa.

---

## La analogía: el sistema de cajas de un almacén

La misma analogía que en Java aplica aquí, porque el problema es el mismo.

1. Un **espacio físico** donde guardar cada cosa → la memoria del computador.
2. Una **etiqueta** en cada espacio para encontrarlo → el nombre de la variable.
3. Un **tipo de caja** que determina qué cabe adentro → el tipo de dato.
4. Una **estantería numerada** para muchas cosas del mismo tipo → un array.
5. Una **caja compuesta** con compartimientos para datos relacionados → una clase u objeto.
6. Un **letrero en la entrada** que le dice al compilador qué tipo acepta → los genéricos `<>`.

Lo que varía es cómo C# construye cada una de estas piezas.

---

## ¿Qué es un dato en C#?

Igual que en Java: cualquier valor que el programa necesita recordar o transformar.
Un número, un texto, una respuesta sí/no, una fecha, una lista de usuarios.

Cada dato tiene un **tipo**, y ese tipo le dice al compilador:
- Cuánta memoria reservar.
- Qué operaciones son válidas.
- Cómo debe interpretarse el valor.

---

## Variables: la etiqueta sobre la caja

```csharp
int edad = 25;
```

Esto le dice a C# tres cosas:

| Parte | Qué le dice a C# |
|---|---|
| `int` | "reserva una caja del tamaño de un entero" |
| `edad` | "llámala `edad` para que yo pueda encontrarla" |
| `= 25` | "y ponle este valor adentro ahora mismo" |

Idéntico a Java en la lógica. Diferente en que C# también permite escribir esto:

```csharp
var edad = 25;
```

`var` le pide a C# que **infiera el tipo** solo, mirando el valor que le asignas.
El tipo sigue siendo `int`, no desaparece, pero no tienes que escribirlo tú.
El compilador lo deduce en tiempo de compilación, no en tiempo de ejecución.

> **Cuándo usar `var`:** cuando el tipo es obvio por el contexto (`var lista = new List<string>()`)
> o cuando el nombre del tipo es muy largo. Cuando el tipo aporta información útil al lector,
> es mejor escribirlo explícitamente.

---

## Tipos de datos en C#

C# también divide los tipos en dos familias, pero con una diferencia importante en cómo los nombra.

---

### Familia 1, Tipos de valor (la caja contiene el valor directamente)

En C# se llaman **value types**, equivalentes a los primitivos de Java,
pero con una diferencia conceptual: en C# incluso los `struct` (estructuras propias)
son value types, no solo los tipos básicos.

Los más comunes:

| Tipo C# | Alias .NET | Qué guarda | Ejemplo |
|---|---|---|---|
| `int` | `Int32` | Enteros | `42`, `-7` |
| `long` | `Int64` | Enteros grandes | `9_000_000_000L` |
| `double` | `Double` | Decimales (64 bits) | `3.14` |
| `float` | `Single` | Decimales (32 bits) | `3.14f` |
| `bool` | `Boolean` | Verdadero o falso | `true`, `false` |
| `char` | `Char` | Un carácter | `'A'` |
| `decimal` | `Decimal` | Decimales de alta precisión (financiero) | `19.99m` |

```csharp
int puntos = 100;
double precio = 19.99;
bool activo = true;
char inicial = 'N';
decimal saldo = 1_500_000.50m;
```

**Diferencia con Java:** en C#, `bool` se escribe `bool` (no `boolean`), y existe `decimal`
como tipo específico para cálculos financieros donde los errores de punto flotante son inaceptables.

**Por qué importa que sean value types:** cuando asignas uno a otro, se copia el valor.

```csharp
int a = 10;
int b = a;
b = 99;

Console.WriteLine(a); // 10 — a no cambió
Console.WriteLine(b); // 99
```

---

### Tipos de valor anulables (nullable value types)

En Java, los tipos primitivos **nunca** pueden ser `null`.
En C#, puedes hacer que cualquier value type acepte `null` con el operador `?`:

```csharp
int  edadNormal   = 25;     // solo acepta enteros
int? edadAnulable = null;   // acepta enteros o null
```

Esto es útil cuando un dato puede no existir todavía, por ejemplo, la edad de un usuario
que aún no ha completado su perfil. En Java tendrías que usar `Integer` (el wrapper) para esto.

---

### Familia 2: Tipos de referencia (la caja guarda una dirección)

Aquí la caja no contiene el dato directamente, contiene la dirección de donde vive el dato.
Igual que en Java: es el papel que dice "ve al pasillo 7, estante 3".

El tipo más común es `string` (con minúscula en C#):

```csharp
string nombre = "Dahiana";
```

**Diferencia importante con Java:** en C#, el operador `==` en `string` ya compara el contenido,
no la referencia. El compilador lo sobreescribió para hacer lo que uno esperaría naturalmente.

```csharp
string a = "hola";
string b = "hola";

Console.WriteLine(a == b);        // true  — compara contenido (C# lo resuelve por ti)
Console.WriteLine(a.Equals(b));   // true  — también funciona, explícito
```

> Esto es una diferencia de diseño entre los dos lenguajes. Java expone la distinción
> referencia/valor y te obliga a manejarla. C# la esconde en el caso de `string`
> para que la experiencia sea más natural. Ninguna decisión es "mejor", son filosofías distintas.

---

## Tipos de datos: el sistema de tipos unificado de C#

Una característica que distingue a C# de Java: en C# **todo es un objeto**, incluso los value types.

Esto significa que puedes llamar métodos directamente sobre un entero:

```csharp
int numero = -42;
Console.WriteLine(numero.ToString());       // "-42"
Console.WriteLine(Math.Abs(numero));        // 42
Console.WriteLine(42.ToString("D5"));       // "00042"
```

En Java, los tipos primitivos (`int`, `double`) no tienen métodos, tienes que usar
sus wrappers (`Integer`, `Double`) para eso. En C#, el compilador hace ese puente
automáticamente gracias al sistema de tipos unificado.

---

## Datos compuestos: cuando necesitas más de una caja

---

### Arrays: la estantería numerada `[]`

Igual que en Java: una secuencia de casillas del mismo tipo, numeradas desde 0.
Los corchetes `[]` tienen los mismos dos momentos:

**Momento 1 — declarar que algo es un array:**

```csharp
int[] calificaciones;
```

**Momento 2 — acceder a una posición:**

```csharp
calificaciones[0] = 90;
int primera = calificaciones[0];
```

**Crear e inicializar un array completo:**

```csharp
string[] frutas = {"mango", "fresa", "uva"};

Console.WriteLine(frutas[0]);        // mango
Console.WriteLine(frutas[2]);        // uva
Console.WriteLine(frutas.Length);    // 3
```

**Diferencia con Java:** en C# la propiedad es `Length` con L mayúscula (en Java es `length` minúscula).
Pequeño detalle, pero es el tipo de cosa que te hace perder tiempo si no lo sabes.

---

### Clases y objetos: la caja compuesta `{}`

Cuando necesitas agrupar datos relacionados, usas una clase:

```csharp
class Producto {
    public string Nombre;
    public double Precio;
    public int Stock;
}
```

Las llaves `{}` definen el contenido del molde, exactamente igual que en Java.

**Diferencia:** en C# las convenciones de nombres usan **PascalCase** para propiedades y métodos
(`Nombre`, `ObtenerPrecio`), mientras que Java usa **camelCase** (`nombre`, `obtenerPrecio`).
No es una regla del lenguaje, es una convención de la comunidad, pero es tan universal
que ignorarla hace que el código parezca escrito por alguien que no conoce el ecosistema.

Crear un objeto:

```csharp
var cafe = new Producto();
cafe.Nombre = "Café tostado";
cafe.Precio = 12500.0;
cafe.Stock = 40;

Console.WriteLine(cafe.Nombre);  // Café tostado
Console.WriteLine(cafe.Precio);  // 12500
```

**Properties: una mejora de C# sobre los campos directos:**

En C# es más idiomático usar **properties** en lugar de campos públicos.
Una property tiene un getter y un setter implícitos:

```csharp
class Producto {
    public string Nombre { get; set; }
    public double Precio { get; set; }
    public int Stock { get; set; }
}
```

El `{ get; set; }` es la sintaxis que C# usa para decir: "este campo tiene lectura y escritura".
Es más que decoración, internamente genera métodos que permiten añadir lógica después
sin cambiar la interfaz pública de la clase.

---

### Las llaves `{}`: el delimitador de espacios

En C#, igual que en Java, las llaves tienen un solo trabajo conceptual:
**marcar dónde empieza y dónde termina un bloque**.

```csharp
// El cuerpo de una clase
class MiClase {
    // todo lo que pertenece a MiClase vive aquí
}

// El cuerpo de un método
static void Saludar() {
    Console.WriteLine("Hola");
}

// El cuerpo de una condición
if (edad >= 18) {
    Console.WriteLine("Mayor de edad");
}

// Los valores iniciales de un array
int[] numeros = {10, 20, 30};
```

Regla: **cada `{` tiene exactamente un `}` que lo cierra**.

**Lo que C# añade, expresiones de colección (C# 12+):**

```csharp
// Sintaxis moderna equivalente para arrays
int[] numeros = [10, 20, 30];
```

Los corchetes `[]` aquí cumplen el mismo rol que `{}` en la inicialización tradicional.
Es una adición reciente del lenguaje, si ves código más antiguo, siempre encontrarás `{}`.

---

### Genéricos `<>`: el letrero de tipo en la entrada

En C#, los genéricos funcionan igual que en Java conceptualmente:
le dicen al compilador qué tipo puede contener una estructura.

```csharp
using System.Collections.Generic;

List<string> nombres = new List<string>();
nombres.Add("Dahiana");
nombres.Add("Carlos");

Console.WriteLine(nombres[0]);  // Dahiana
```

**Diferencia clave con Java:** en C# puedes acceder a los elementos de una `List<T>`
con corchetes `[0]` igual que en un array. En Java necesitarías `.get(0)`.
Esto es posible porque C# implementa un indexador en la clase `List<T>`.

```csharp
List<int>     edades  = new List<int>();      // acepta int directamente (no Integer)
List<double>  precios = new List<double>();
List<string>  tags    = new List<string>();
```

**Diferencia importante con Java:** en C# los genéricos sí aceptan tipos de valor directamente.
No necesitas `List<Integer>`, puedes escribir `List<int>` y funciona.
En Java esto no es posible porque los genéricos solo aceptan tipos de referencia.

**Con `var` (la forma más común en código real):**

```csharp
var nombres = new List<string>();
```

El compilador sabe que `nombres` es `List<string>` porque lee el lado derecho.

---

## Los tres símbolos en contexto: resumen técnico

| Símbolo | Nombre | Qué hace en C# |
|---|---|---|
| `{}` | Llaves | Delimita un bloque: clase, método, condición, valores iniciales, o getter/setter |
| `[]` | Corchetes | Declara un array, accede a una posición, o inicializa colecciones (C# 12+) |
| `<>` | Ángulos | Parámetro de tipo para genéricos — solo le habla al compilador |

La diferencia más importante respecto a Java:
- `[]` en C# también funciona para acceder a `List<T>` (en Java necesitas `.get()`).
- `bool` en C# (no `boolean`).
- Los genéricos aceptan `int` directamente (no `Integer`).
- `string` con `==` compara contenido (en Java compara referencia).

---

## Tabla comparativa: Java vs C#

| Concepto | Java | C# |
|---|---|---|
| Entero | `int` | `int` |
| Booleano | `boolean` | `bool` |
| Texto | `String` (mayúscula) | `string` (minúscula) |
| Comparar strings | `.equals()` | `==` funciona |
| Inferencia de tipo | no existe (`var` llegó en Java 10, limitada) | `var` desde C# 3 |
| Nullable value type | usar wrapper `Integer` | `int?` |
| Lista dinámica | `ArrayList<Integer>` | `List<int>` |
| Acceder a lista | `.get(0)` | `[0]` |
| Largo de array | `.length` (minúscula) | `.Length` (mayúscula) |
| Convención de nombres | camelCase para métodos | PascalCase para métodos |
| Interpolación de strings | `"Hola " + nombre` | `$"Hola {nombre}"` |

---

## ¿Qué sigue?

Con esto ya tienes el mapa de cómo C# organiza y manipula datos:

- **Dato** → la información que el programa necesita.
- **Variable** → el nombre con el que le hablas a ese espacio en memoria.
- **Value type** → la caja que contiene el valor directamente (int, double, bool...).
- **Reference type** → la caja que apunta a donde vive el dato real (string, clases...).
- **Nullable type** → un value type que también puede ser null (`int?`).
- **Array** → la estantería numerada para muchos datos del mismo tipo.
- **Clase con properties** → la caja compuesta con `{ get; set; }`.
- **Genérico** → el letrero `<T>` que le dice al compilador qué tipo acepta la estructura.

Los tres símbolos (`[]`, `{}`, `<>`) hacen lo mismo que en Java en esencia,
pero C# tomó decisiones distintas en los detalles: `[0]` funciona en listas,
`bool` sin `ean`, `int` directo en genéricos, `==` compara contenido en strings.

Esos detalles no son errores de diseño, son decisiones conscientes de cada lenguaje.
Conocerlas es lo que te permite moverte entre lenguajes sin que el cambio te cueste el doble.