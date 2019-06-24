Almacenando una Estructura
===

Si pensó en que fué genial que todos tuvieran su propio número, ahora intentemos darle gatitos digitales a todos.

Primero debe definir qué propiedades tienen los gatitos. Para esto usará una estructura (`struct`), y luego aprenderá cómo almacenar estas estructuras personalizadas en el motor de ejecución.

## Definiendo una Estructura Personalizada

Puede definir una estructura personalizada para su motor de ejecución de la siguiente manera:

```rust
#[derive(Encode, Decode, Default, Clone, PartialEq)]
#[cfg_attr(feature = "std", derive(Debug))]
pub struct MyStruct<A, B> {
    some_number: u32,
    some_generic: A,
    some_other_generic: B,
}
```

Para usar los traits personalizados `Encode` y `Decode`, necesitará importarlos desde el crate `parity_codec`:

```rust
use parity_codec::{Encode, Decode};
```

Esto debería parecer bastante normal en comparación con la definición de estructuras en otros lenguajes. Sin embargo, notará dos rarezas en esta declaración que son específicas para el desarrollo del motor de ejecución...

### Utilizando Genéricos

Notará que se definió la estructura utilizando un genérico como uno de los tipos que almacenamos. Esto será importante cuando intente utilizar tipos personalizados de Substrate como `AccountId` o `Balance` dentro de la estructura, ya que tendrá que pasar estos tipos cada vez que use la estructura.

Entonces, si quisiéra almacenar un `Balance` en `some_generic` y `Hash` en `some_other_generic`, deberá definir el elemento de almacenamiento de esta manera:

```rust
decl_storage! {
    trait Store for Module<T: Trait> as Example {
        MyItem: map T::AccountId => MyStruct<T::Balance, T::Hash>;
    }
}
```

Para mayor claridad en el tutorial, se definirá un tipo genérico de `T::AccountId` como `AccountId` o `T::Balance` como `Balance`.

### Derive Macro

La otra cosa que notará es `# [derive (...)]` en la parte superior. Este es un atributo proporcionado por el compilador de Rust que permite implementaciones básicas de algunos `Traits`. La segunda línea, `# [cfg_attr (feature = "std", derive (Debug))]` hace lo mismo para el trait `Debug`, pero solo cuando se usan las bibliotecas "estándar", es decir, cuando se compilan los binarios nativos y no el wasm.  [Aquí] (https://doc.rust-lang.org/rust-by-example/trait/derive.html) puede obtener más información sobre esto. Para efectos de este tutorial, puede tratarlo como magia.

## Estructura Personalizada en una Función del Módulo

Ahora que ha inicializado su estructura personalizada en el motor de ejecución, puede insertar valores y modificarlos.

Este es un ejemplo de cómo crear e insertar una estructura en el almacenamiento utilizando una función (ej. `create_struct`):

```rust
decl_module! {
    pub struct Module<T: Trait> for enum Call where origin: T::Origin {
        fn create_struct(origin, value: u32, balance: T::Balance, hash: T::Hash) -> Result {
            let sender = ensure_signed(origin)?;

            let new_struct = MyStruct {
                some_number: value,
                some_generic: balance,
                some_other_generic: hash,
            };

            <MyItem<T>>::insert(sender, new_struct);
            Ok(())
        }
    }
}
```

## ¡Su Turno!

Actualice el motor de ejecución del mapa de almacenamiento para almacenar una estructura `Kitty` en lugar de un u64.

Un `Kitty` debe tener las siguientes propiedades:

 - `id` : `Hash`
 - `dna` : `Hash`
 - `price` : `Balance`
 - `gen` : `u64`

Hemos creado para usted un esqueleto de la función `create_kitty()`, pero necesitará agregar la lógica. Incluya el código para crear un nuevo gato (`new_kitty`) usando el objeto `Kitty` y almacene ese objeto en su motor de ejecución.

Para inicializar `T::Hash` y` T::Balance` puede usar:

```rust
let hash_of_zero = <T as system::Trait>::Hashing::hash_of(&0);

let my_zero_balance = <T::Balance as As<u64>>::sa(0);
```

También tendra que comenzar `gen` como `0`.

<!-- tabs:start -->

#### ** Template **

[embedded-code](../../1/assets/1.6-template.rs ':include :type=code embed-template')

#### ** Solution **

[embedded-code-final](../../1/assets/1.6-finished-code.rs ':include :type=code embed-final')

#### ** Previous Chapter Solution **

[embedded-code-previous](../../1/assets/1.5-finished-code.rs ':include :type=code embed-previous')

<!-- tabs:end -->

---

**Aprenda Más**

### Cadenas de Texto en Substrate

¡Se podría haber imaginado que una de las propiedades que agregaría para sus gatitos sería un nombre! Después de todo, ¿quién no nombra las cosas que ama?

Substrate no soporta directamente cadenas de texto (`Strings`). El almacenamiento en el motor de ejecución está ahí para almacenar el estado de la lógica de negocio. No es para almacenar datos generales que necesita la interfaz de usuario. Si realmente necesita almacenar algunos datos arbitrarios en su motor de ejecución, puede crear un bytearray (`Vec<u8>`). Sin embargo, lo más lógico es almacenar un hash en un servicio como IPFS para luego utilizarlo y obtener Datos para su interfaz de usuario. Esto está actualmente fuera del alcance de este taller, pero lo podría agregar más adelante para admitir metadatos adicionales acerca de su gatito.

---
