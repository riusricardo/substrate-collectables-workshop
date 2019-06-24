Creando Variables de Almacenamiento
===

Intente agregar algo de lógica simple en su motor de ejecución: una función que almacena una variable.

Para hacer esto, primero necesitaremos definir una variable de almacenamiento para un [**Storage Item**](https://substrate.dev/docs/en/overview/glossary#storage-items) en el macro [**`decl_storage!`**](https://crates.parity.io/srml_support_procedural/macro.decl_storage.html). Esto permite un uso seguro de la base de datos de Substrate. También permite mantener las variables accesibles en todos los bloques.

## Declarar una Variable de Almacenamiento

Substrate soporta de forma nativa todos los tipos primitivos disponibles en Rust (`bool`, `u8`, `u32`, etc...) y también algunos tipos personalizados específicos de Substrate (`AccountId`, `Balance`, `Hash`, [ y más](https://polkadot.js.org/api/types/) ...)

Puede declarar una variable de almacenamiento simple:

```rust
decl_storage! {
    trait Store for Module<T: Trait> as Example {
        MyU32: u32;
        MyBool get(my_bool_getter): bool;
    }
}
```

Aquí ha definido dos variables: un `u32` y un `bool` con una función de lectura (getter) llamada `my_bool_getter`. El parámetro `get` es opcional, pero si lo agrega a su variable de almacenamiento, expondrá una función getter con el nombre especificado (`fn getter_name () -> Type`).

Para almacenar estas variables de almacenamiento, necesita importar el módulo `support::StorageValue`.

### Trabajar con una Variable de Almacenamiento

Las funciones utilizadas para acceder a un `StorageValue` se definen en [`srml_support::storage`](https://crates.parity.io/srml_support/storage/trait.StorageValue.html):

```rust
/// Obtener la llave de almacenamiento.
fn key() -> &'static [u8];

/// Es verdadero si el valor está definido en el almacenamiento.
fn exists<S: Storage>(storage: &S) -> bool {
    storage.exists(Self::key())
}

/// Cargue el valor de la variable de almacenamiento.
fn get<S: Storage>(storage: &S) -> Self::Query;

/// Toma un valor del almacenamiento y lo elimina después.
fn take<S: Storage>(storage: &S) -> Self::Query;

/// Almacena un valor bajo esta llave en la variable de almacenamiento.
fn put<S: Storage>(val: &T, storage: &S) {
    storage.put(Self::key(), val)
}

/// Muta este valor.
fn mutate<R, F: FnOnce(&mut Self::Query) -> R, S: Storage>(f: F, storage: &S) -> R;

/// Borrar el valor del almacenamiento.
fn kill<S: Storage>(storage: &S) {
    storage.kill(Self::key())
}
```

Entonces, si quiere "poner" el valor de `MyU32`, puede escribir:

```rust
<MyU32<T>>::put(1337);
```

Si quisiera "obtener" el valor de `MyBool`, podría escribir:

```rust
let my_bool = <MyBool<T>>::get();
let also_my_bool = Self::my_bool_getter();
```

En la siguiente sección aprenderá a integrar estas llamadas en su módulo.

## ¡Su Turno!

Cree una variable de almacenamiento llamada `Value` que almacene un `u64`.

Asegúrese de importar las bibliotecas requeridas por el compilador. Su código debe compilar con éxito.

<!-- tabs:start -->

#### ** Template **

[embedded-code](../../1/assets/1.2-template.rs ':include :type=code embed-template')

#### ** Solution **

[embedded-code-final](../../1/assets/1.2-finished-code.rs ':include :type=code embed-final')

#### ** Previous Chapter Solution **

[embedded-code-previous](../../1/assets/1.1-finished-code.rs ':include :type=code embed-previous')

<!-- tabs:end -->
