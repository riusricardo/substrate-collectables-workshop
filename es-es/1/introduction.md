Introducción
===

En esta sección, aprenderá los conceptos básicos para crear un motor de ejecución personalizado:

- Cómo utilizar el almacenamiento en su motor de ejecución.
- Cómo exponer funciones del motor de ejecución (Runtime).
- Cómo utilizar la interfaz de usuario de Polkadot-JS para interactuar con su motor de ejecución.

## ¿Qué es un Motor de Ejecución (Runtime)?

En resumen, el motor de ejecución ([*Runtime*](https://substrate.dev/docs/en/overview/glossary#runtime)) es la lógica de ejecución para la creación y validación de bloques, a veces denominada como la función de transición de estado ([**STF** ](https://substrate.dev/docs/en/overview/glossary#stf-state-transition-function), por sus siglas en ingles). En [Substrate](https://substrate.dev/docs/en/overview/glossary#substrate), esto se almacena en la cadena de bloques como un binario de WebAssembly, en un formato ejecutable e independiente de la implementación. Otros sistemas tienden a expresarlo solo en un formato legible para las personas (por ejemplo, Ethereum) o nada en absoluto (por ejemplo, Bitcoin).

## ¿Qué es un Módulo?

El motor de ejecución de su blockchain contiene múltiples características y funcionalidades que trabajan en conjunto para impulsar su blockchain. 

Por ejemplo:
- Administración de cuentas
- Saldos de fichas (tokens)
- Gobernanza
- Actualizaciones del Runtime
- y más...

[Aquí](https://github.com/paritytech/substrate/tree/master/srml) encontrará todos los módulos proporcionados en el código base para que los incluya fácilmente en su motor de ejecución. Este conjunto de módulos provistos por Substrate se conocen como Substrate Runtime Module Library ([**SRML**](https://substrate.dev/docs/en/overview/glossary#srml-substrate-runtime-module-library)).

Con esta biblioteca de módulos, puede crear e incluir fácilmente nuevos módulos en su motor de ejecución. ¡Eso es lo que haremos en este tutorial!

## Rust

En este momento, el desarrollo de Substrate y el motor de ejecución utilizan el [lenguaje de programación Rust](https://www.parity.io/why-rust/).

Este tutorial **no** es un curso para aprender Rust, pero repasará algunas de las diferencias básicas que puede encontrar al seguir esta guía en comparación con la programación en otros lenguajes.

### Propiedad (Ownership) y Préstamo (Borrowing)

De los [documentos de Rust](https://doc.rust-lang.org/book/ownership.html):

> La propiedad es la característica más exclusiva de Rust, y le permite a Rust garantizar la seguridad de la memoria sin necesidad de un recolector de basura.
>
> - Cada valor en Rust tiene una variable que se llama propietario.
> - Solo puede haber un dueño (propietario) a la vez.
> - Cuando el propietario queda fuera del alcance, el valor se eliminará.

Verá a lo largo del tutorial que agregaremos un símbolo ampersand (`&`) delante de algunas variables, lo que implica que estamos tomando prestado el valor. Esto es útil si necesitamos reutilizar el valor varias veces a lo largo de una función.

Básicamente, impide cometer errores estúpidos en el manejo de la memoria, así que agradece si el compilador Rust te aconseja que no hagas una cosa determinada.

### Rasgos (Traits)

De los [documentos de Rust](https://doc.rust-lang.org/book/traits.html):

> Rasgos abstractos sobre el comportamiento común en diferentes definiciones de tipos.

Si está familiarizado con las interfaces, los Rasgos (Traits) son [la noción única de Rust de una interfaz](https://blog.rust-lang.org/2015/05/11/traits.html).

### Errores Recuperables con tipo resultado (Result)

Más adelante aprenderá que las funciones del módulo deben devolver el tipo `Result`, el cuál nos permite manejar los errores dentro de nuestras funciones. Si la operación fué exitosa, el valor de devuelto (`Result`) es `Ok()`, en caso de error es `Err()`.

A lo largo de este tutorial usaremos el operador de signo de interrogación (`?`) al final de las funciones que devuelven un `Result`. Al llamar a una función como esta, por ejemplo, `my_function ()?`, Rust simplemente expande el código a:

```rust
match my_function() {
    Ok(value) => value,
    Err(msg) => return Err(msg),
}
```

[Aquí](https://doc.rust-lang.org/book/ch09-02-recoverable-errors-with-result.html)puede obtener más información.

Los errores típicos de esta clase son:

```
error[E0382]: borrow of moved value: `s`
error[E0382]: use of moved value: `s`
error[E0502]: cannot borrow `s` as immutable because it is also borrowed as mutable
error[E0499]: cannot borrow `s` as mutable more than once at a time
```

Por ejemplo, el siguiente código no compilará:

```rust
fn main() {
    let mut s = String::from("hello");
    let t1 = &s;      // t1 es una referencia inmutable a la cadena de texto (String).
    let t2 = &mut s;  // t2 es una referencia mutable a la cadena de texto (String).
    t2.push_str(&t1); // Queremos concatenar t2 con t1.
                      //
		      // Esto no sirve ya que ambos son referencias a la misma cadena subyacente.
}
```
Error de préstamo:

```
error[E0502]: cannot borrow `s` as mutable because it is also borrowed as immutable
 --> src/main.rs:4:14
  |
3 |     let t1 = &s;
  |              -- immutable borrow occurs here
4 |     let t2 = &mut s;
  |              ^^^^^^ mutable borrow occurs here
5 |     t2.push_str(&t1);
  |                 --- immutable borrow later used here
```

Una solución fácil para este error en particular es clonar la cadena en lugar de tener otra referencia a ella.

```rust
fn main() {
    let mut s = String::from("hello");
    let t1 = s.clone();
    let t2 = &mut s;
    t2.push_str(&t1);
}
```

Funciona!

### Macros

De los [documentos de Rust](https://doc.rust-lang.org/book/macros.html):

> Mientras que las funciones y los tipos se abstraen en el código, las macros se abstraen a un nivel sintáctico.

Dicho de manera más sencilla, las macros son código que escriben código, generalmente para simplificar o hacer que el código sea más legible.

Substrate utiliza muchas macros a lo largo del proceso de desarrollo del Runtime, y son bastante específicos en la sintaxis que admiten y bastante limitados en los errores que devuelven.

---
**Aprenda Más**

[TODO: make this a page]

---
