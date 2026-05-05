# Rust: Datos, variables y tipos: por qué existen y cómo usarlos

> Si ya leíste los documentos de Java y C#, este te va a resultar familiar en la intención
> pero diferente en casi todo lo demás. Rust no comparte la misma filosofía de diseño
> que Java o C#. Tomó decisiones más radicales, y entender por qué es la clave
> para que el lenguaje tenga sentido.

---

## El mismo problema, una respuesta diferente

Un programa recibe información, la transforma y produce un resultado.
Eso no cambia.

Lo que sí cambia es cuánta responsabilidad le das al programador y cuánta
se queda en el lenguaje. Java y C# tienen un recolector de basura: un proceso
que corre en segundo plano y libera la memoria que ya no se usa.
Rust no tiene recolector de basura. En cambio, tiene un sistema de reglas
que el compilador verifica antes de ejecutar el programa, para garantizar
que la memoria se use correctamente sin necesitar un proceso extra en tiempo de ejecución.

Eso es lo que hace a Rust distinto. Y es lo que explica por qué su sintaxis
y sus reglas existen.

---

## La analogía: el almacén con reglas estrictas de préstamo

En Java y C#, el almacén tiene un empleado que recorre los pasillos y recoge
las cajas que nadie está usando. Eso tiene un costo: el empleado interrumpe
el trabajo cada cierto tiempo para hacer su ronda (eso se llama pausa del GC,
garbage collector).

En Rust, no hay empleado. En cambio, el almacén tiene reglas escritas
en la pared que todos deben cumplir:

1. Cada caja tiene **un único dueño** a la vez.
2. Cuando el dueño se va, la caja se destruye automáticamente.
3. Puedes **prestar** la caja a alguien para que la lea, pero no puede modificarla mientras la estés usando tú.
4. Puedes **prestar** la caja para que la modifiquen, pero solo a una persona a la vez.

El compilador de Rust es el vigilante que verifica que nadie rompa estas reglas.
Si las rompes, el programa no compila. Ese es el trato: a cambio de seguir las reglas,
Rust garantiza que el programa no va a tener errores de memoria en tiempo de ejecución.

Esta analogía no es decorativa. Cada parte de ella tiene un nombre técnico
que aparecerá en el código: ownership, borrowing y lifetimes.

---

## ¿Qué es un dato en Rust?

Igual que en Java y C#: cualquier valor que el programa necesita recordar o transformar.

La diferencia es que en Rust cada dato tiene, además de un tipo, un **dueño**.
Y el compilador rastrea ese dueño en todo momento.

---

## Variables: la etiqueta sobre la caja

```rust
let edad = 25;
```

Rust infiere el tipo automáticamente. En este caso deduce `i32` (entero de 32 bits)
porque es el tipo entero por defecto. Si quieres ser explícito:

```rust
let edad: i32 = 25;
```

| Parte | Qué le dice a Rust |
|---|---|
| `let` | "declara una nueva variable en este alcance" |
| `edad` | "su nombre es `edad`" |
| `: i32` | "su tipo es entero de 32 bits" (opcional si Rust lo puede inferir) |
| `= 25` | "su valor inicial es 25" |

**La diferencia más importante respecto a Java y C#:** las variables en Rust
son **inmutables por defecto**. Si declaras una variable con `let`, no puedes
cambiarle el valor después.

```rust
let edad = 25;
edad = 26; // error de compilación: no puedes reasignar una variable inmutable
```

Para permitir que una variable cambie, debes declararlo explícitamente con `mut`:

```rust
let mut edad = 25;
edad = 26; // ahora sí funciona
```

Esto no es un capricho. Rust te obliga a ser explícito sobre qué datos van a cambiar
y cuáles no, porque esa información le permite al compilador hacer verificaciones
de seguridad más precisas.

---

## Tipos de datos en Rust

Rust también distingue entre tipos simples y tipos compuestos,
pero los organiza de forma diferente a Java y C#.

---

### Tipos escalares: un solo valor

Son los tipos más básicos. Cada uno guarda un valor único.

**Enteros:**

| Tipo | Bits | Con signo | Rango aproximado |
|---|---|---|---|
| `i8` | 8 | sí | -128 a 127 |
| `i32` | 32 | sí | -2 mil millones a 2 mil millones |
| `i64` | 64 | sí | rango muy grande |
| `u8` | 8 | no | 0 a 255 |
| `u32` | 32 | no | 0 a 4 mil millones |
| `usize` | arquitectura | no | índices y tamaños |

En Java y C# siempre tienes signo en los enteros básicos. Rust te da la opción
de elegir entre enteros con signo (`i` de integer) y sin signo (`u` de unsigned).
Eso importa cuando sabes que un valor nunca va a ser negativo, como un índice o un tamaño.

**Decimales:**

| Tipo | Bits | Ejemplo |
|---|---|---|
| `f32` | 32 | `3.14_f32` |
| `f64` | 64 | `3.14` (por defecto) |

**Booleano:**

```rust
let activo: bool = true;
```

**Carácter:**

```rust
let inicial: char = 'N';
```

En Rust, `char` representa un carácter Unicode completo (4 bytes), no solo ASCII como en Java.

---

### Tipos compuestos escalares: varios valores juntos, tamaño fijo

Estos tipos agrupan valores sin necesitar memoria dinámica.

**Tupla: valores de tipos distintos, posición fija:**

```rust
let persona: (String, i32, bool) = (String::from("Lucía"), 21, true);

println!("{}", persona.0);  // Lucía
println!("{}", persona.1);  // 21
```

Una tupla es útil cuando quieres devolver varios valores desde una función
sin crear una estructura completa. Accedes a cada posición con un punto y el índice.

**Array: valores del mismo tipo, tamaño fijo en tiempo de compilación:**

```rust
let frutas: [&str; 3] = ["mango", "fresa", "uva"];

println!("{}", frutas[0]);     // mango
println!("{}", frutas[2]);     // uva
println!("{}", frutas.len());  // 3
```

El tipo `[&str; 3]` le dice a Rust dos cosas: que los elementos son `&str`
y que el array tiene exactamente 3 posiciones. Ese tamaño es parte del tipo,
no es una propiedad separada. Un `[&str; 3]` y un `[&str; 5]` son tipos diferentes.

---

## Los corchetes `[]`: dos roles, mismo símbolo

Igual que en Java y C#, los corchetes tienen dos momentos de aparición:

**Momento 1: declarar el tipo de un array:**

```rust
let numeros: [i32; 5];
```

**Momento 2: acceder a una posición:**

```rust
let primero = numeros[0];
```

La lógica es la misma que en los otros lenguajes. Lo que cambia es que en Rust
el tamaño forma parte del tipo, no es una propiedad separada.

---

## Las llaves `{}`: delimitador de bloques y placeholder en macros

En Rust, las llaves tienen el mismo rol principal que en Java y C#:
marcar dónde empieza y termina un bloque.

```rust
fn saludar() {
    println!("Hola");
}

if edad >= 18 {
    println!("Mayor de edad");
}
```

Pero en Rust aparece un segundo uso que no existe en Java ni C#:
las llaves `{}` dentro de strings son placeholders para interpolación.

```rust
let nombre = "Dahiana";
let edad = 21;

println!("Nombre: {}, edad: {}", nombre, edad);
println!("Nombre: {nombre}, edad: {edad}");  // sintaxis más nueva, desde Rust 1.58
```

El `{}` dentro del string le dice al macro `println!` dónde insertar el siguiente valor.
No es el mismo `{}` que abre bloques, aunque el símbolo sea igual.
El contexto (dentro de un string vs fuera) es lo que determina qué rol cumple.

---

## Datos compuestos con estructura: structs

Cuando necesitas agrupar datos relacionados de tipos distintos bajo un mismo nombre,
en Rust usas una **struct**. Es el equivalente a una clase en Java o C#,
pero sin métodos incluidos por defecto.

```rust
struct Producto {
    nombre: String,
    precio: f64,
    stock: u32,
}
```

Las llaves `{}` aquí definen los campos de la estructura.

Para crear una instancia:

```rust
let cafe = Producto {
    nombre: String::from("Café tostado"),
    precio: 12500.0,
    stock: 40,
};

println!("{}", cafe.nombre);   // Café tostado
println!("{}", cafe.precio);   // 12500
```

Las llaves `{}` al crear la instancia inicializan los campos, nombre por nombre.
No hay una lista numerada como en los arrays, sino campos identificados por nombre.

---

## El sistema de tipos de Rust: ownership y borrowing

Aquí es donde Rust se separa completamente de Java y C#.
No es solo sintaxis diferente, es un modelo mental diferente.

---

### Ownership: cada dato tiene un único dueño

Cuando creas un valor en Rust, hay exactamente una variable que lo posee.
Cuando esa variable sale del alcance (del bloque `{}`), el valor se destruye y la memoria se libera.
Eso ocurre automáticamente, sin recolector de basura.

```rust
{
    let nombre = String::from("Nataly");
    println!("{}", nombre);
}
// aquí `nombre` ya no existe. La memoria fue liberada automáticamente.
```

**¿Qué pasa cuando asignas un String a otra variable?**

```rust
let a = String::from("hola");
let b = a;  // la propiedad se mueve a `b`

println!("{}", a);  // error de compilación: `a` ya no es el dueño
```

Esto se llama **move** (movimiento). El valor no se copia, la propiedad se transfiere.
`a` queda inválida. Esto no existe en Java ni C# y es el concepto que más confunde
a quienes llegan de esos lenguajes.

**¿Por qué existe esto?** Porque garantiza que nunca hay dos partes del programa
modificando el mismo dato al mismo tiempo sin saberlo. El compilador lo verifica.

---

### Tipos que sí se copian: Copy

Los tipos escalares simples (`i32`, `f64`, `bool`, `char`) implementan el trait `Copy`.
Eso significa que cuando los asignas a otra variable, se copia el valor,
no se mueve la propiedad.

```rust
let a = 10;
let b = a;  // se copia el valor

println!("{}", a);  // 10, funciona porque i32 implementa Copy
println!("{}", b);  // 10
```

Esto es equivalente al comportamiento de los value types en C# y los primitivos en Java.
La diferencia es que en Rust esta distinción tiene un nombre formal y se puede aplicar
a tipos propios.

---

### Borrowing: prestar sin ceder la propiedad

Si quieres usar un dato en otra parte del código sin transferir la propiedad,
puedes **prestar** una referencia con el operador `&`.

```rust
let nombre = String::from("Dahiana");
let longitud = calcular_longitud(&nombre);  // se presta, no se mueve

println!("{} tiene {} letras", nombre, longitud);  // nombre sigue siendo válido

fn calcular_longitud(s: &String) -> usize {
    s.len()
}
```

El `&` aquí no es el operador AND lógico. Es el operador de referencia:
"dame acceso a este dato sin quedarte con él".

**Referencia mutable:** si necesitas que la función modifique el dato prestado:

```rust
let mut mensaje = String::from("Hola");
agregar_texto(&mut mensaje);

fn agregar_texto(s: &mut String) {
    s.push_str(", mundo");
}

println!("{}", mensaje);  // Hola, mundo
```

El `&mut` significa: "te presto este dato y sí puedes modificarlo".
La regla de Rust: solo puede existir una referencia mutable a un dato al mismo tiempo.

---

## Los genéricos `<>` en Rust

Rust también tiene genéricos y se escriben igual que en Java y C#.
El concepto es el mismo: le dicen al compilador qué tipo puede contener una estructura.

```rust
let mut numeros: Vec<i32> = Vec::new();
numeros.push(10);
numeros.push(25);

println!("{}", numeros[0]);  // 10
```

`Vec<i32>` es el equivalente de `List<int>` en C# o `ArrayList<Integer>` en Java.
`Vec` es la estructura dinámica de Rust para listas que crecen en tiempo de ejecución.

A diferencia de Java, Rust sí acepta tipos escalares directamente en los genéricos:
`Vec<i32>` funciona sin necesidad de un wrapper como `Integer`.

La forma más común con inferencia de tipos:

```rust
let mut numeros = vec![10, 25, 30];
println!("{}", numeros[0]);  // 10
```

`vec![]` es un macro que crea un `Vec` inicializado. Los corchetes adentro
son parte de la sintaxis del macro, no del tipo.

---

## Los tres símbolos en contexto: resumen técnico

| Símbolo | Nombre | Qué hace en Rust |
|---|---|---|
| `{}` | Llaves | Delimita un bloque de código, o es un placeholder dentro de strings en macros como `println!` |
| `[]` | Corchetes | Declara el tipo de un array con su tamaño, accede a una posición, o inicializa un array literal |
| `<>` | Ángulos | Parámetro de tipo para genéricos, structs genéricas o funciones genéricas |

---

## Tabla comparativa: Java, C# y Rust

| Concepto | Java | C# | Rust |
|---|---|---|---|
| Entero | `int` | `int` | `i32` |
| Entero sin signo | no existe | `uint` | `u32` |
| Booleano | `boolean` | `bool` | `bool` |
| Decimal | `double` | `double` | `f64` |
| Texto dinámico | `String` | `string` | `String` |
| Texto estático | no existe directo | no existe directo | `&str` |
| Variables mutables | sí por defecto | sí por defecto | requiere `mut` explícito |
| Inferencia de tipo | limitada (`var`) | `var` | siempre disponible |
| Recolector de basura | sí | sí | no, usa ownership |
| Lista dinámica | `ArrayList<Integer>` | `List<int>` | `Vec<i32>` |
| Acceder a lista | `.get(0)` | `[0]` | `[0]` |
| Gestión de memoria | automática (GC) | automática (GC) | ownership en compilación |
| Referencia | no explícita | no explícita | `&` y `&mut` explícitos |

---

## ¿Qué sigue?

Con esto tienes el mapa de cómo Rust organiza y manipula datos:

- **Dato** : la información que el programa necesita.
- **Variable** : el nombre con el que le hablas a ese espacio en memoria.
- **Tipo escalar** : un solo valor, tamaño conocido en compilación (i32, f64, bool, char).
- **Tupla** : varios valores de tipos distintos, acceso por posición.
- **Array** : varios valores del mismo tipo, tamaño fijo y parte del tipo.
- **Vec** : lista dinámica que crece en tiempo de ejecución.
- **Struct** : caja compuesta con campos nombrados, equivalente a una clase sin métodos.
- **Ownership** : cada dato tiene un dueño, cuando el dueño sale del alcance el dato se destruye.
- **Borrowing** : usar un dato sin tomar su propiedad, con `&` (solo lectura) o `&mut` (escritura).
- **Genérico** : el letrero `<T>` que le dice al compilador qué tipo acepta una estructura.

Los tres símbolos (`[]`, `{}`, `<>`) hacen cosas similares a Java y C# en la superficie.
La diferencia real está debajo: Rust razona sobre quién posee cada dato y por cuánto tiempo.
Eso no es una restricción arbitraria, es el mecanismo que le permite garantizar
seguridad de memoria sin costo en tiempo de ejecución.