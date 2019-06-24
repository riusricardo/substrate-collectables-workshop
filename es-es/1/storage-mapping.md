Mapa de Almacenamiento
===

El último motor de ejecución solo permitió almacenar un valor único entre todos los usuarios de la cadena de bloques. Al comenzar a pensar en una cadena de coleccionables, tiene sentido agregar soporte para que cada usuario almacene su propio valor.

Para habilitar esto, reemplazará la variable simple con un mapa de almacenamiento.

## Tipos Específicos de Substrate

Antes de pasar a los mapas de almacenamiento, lea sobre algunos tipos específicos de Substrate que estará utilizando.

La plantilla predetrminada del motor de ejecución incluye un grupo de módulos que exponen los tipos que espera obtener en una cadena de bloques. Es posible que incluso se encuentre exponiendo nuevos tipos a otras partes del motor de ejecución a medida que desarrolla más módulos.

En este tutorial solo usaremos 3 tipos específicos de Substrate:

 1. AccountId
 2. Balance
 3. Hash

Su módulo no tiene acceso de forma nativa a estos tipos, pero puede obtener acceso fácilmente al hacer que el `Trait` del módulo herede de los módulos que definen estos tipos. En este caso, el módulo `balances` tiene todo lo que necesita:

```rust
pub trait Trait: balances::Trait {}
```

Puede acceder a estos tipos en cualquier lugar donde haya especificado el genérico `<T: Trait>` usando `T::Type` como lo realizó en el ejemplo anterior.

## Declarar un Mapa de Almacenamiento

Un mapa de almacenamiento le permite poner un par básico (llave, valor) en el almacenamiento del motor de ejecución. Se puede declarar así:

```rust
decl_storage! {
    trait Store for Module<T: Trait> as Example {
        SomeValue get(some_value_getter): map u32 => u32;
        MyValue: map T::AccountId => u32;
    }
}
```

Puede ver que los mapas pueden ser bastante útiles cuando desea representar datos "propios". Dado que podemos mapear un usuario (`AccountId`) a algún valor, como con `MyValue`, podemos mantener en el almacenamiento información sobre ese usuario. Incluso podemos crear una lógica en nuestro motor de ejecución que administre a quién se le permite modificar esos valores, será un patrón común que usaremos en este tutorial.

Para usar un mapa de almacenamiento, deberá importar el tipo `support::StorageMap`.

### Trabajando con un Mapa de Almacenamiento (StorageMap)

Las funciones utilizadas para acceder a un `StorageMap` se encuentran definidas en [el mismo lugar que `StorageValue`](https://crates.parity.io/srml_support/storage/trait.StorageMap.html):

```rust
/// Obtener la llave del prefijo en el almacenamiento.
fn prefix() -> &'static [u8];

/// Obtenga la llave de almacenamiento utilizada para obtener el valor correspondientea ésta.
fn key_for(x: &K) -> Vec<u8>;

/// Verdadero si el valor está definido en el almacenamiento.
fn exists<S: Storage>(key: &K, storage: &S) -> bool {
    storage.exists(&Self::key_for(key)[..])
}

/// Cargue el valor asociado con la respectiva llave del mapa.
fn get<S: Storage>(key: &K, storage: &S) -> Self::Query;

/// Toma el valor respectivo a la llave en mapa y lo elimina después.
fn take<S: Storage>(key: &K, storage: &S) -> Self::Query;

/// Almacena un valor para asociarse con la llave dada del mapa.
fn insert<S: Storage>(key: &K, val: &V, storage: &S) {
    storage.put(&Self::key_for(key)[..], val);
}

/// Elimina el valor respectivo a una llave.
fn remove<S: Storage>(key: &K, storage: &S) {
    storage.kill(&Self::key_for(key)[..]);
}

/// Muta el valor respectivo a una llave.
fn mutate<R, F: FnOnce(&mut Self::Query) -> R, S: Storage>(key: &K, f: F, storage: &S) -> R;
```

Por lo tanto, si desea "insertar" un valor en un mapa de almacenamiento, debe proporcionar la llave y el valor:

```rust
<SomeValue<T>>::insert(key, value);
```

A continuación, puede consultar ese valor con:

```rust
let my_value = <SomeValue<T>>::get(key);
let also_my_value = Self::some_value_getter(key);
```

## ¡Su Turno!

Actualice su ejemplo para ahora almacenar un mapa de un `AccountId` a un` u64`.

<!-- tabs:start -->

#### ** Template **

[embedded-code](../../1/assets/1.4-template.rs ':include :type=code embed-template')

#### ** Solution **

[embedded-code-final](../../1/assets/1.4-finished-code.rs ':include :type=code embed-final')

#### ** Previous Chapter Solution **

[embedded-code-previous](../../1/assets/1.3-finished-code.rs ':include :type=code embed-previous')

<!-- tabs:end -->

---
**Aprenda Más**

[TODO: make this a page]

---
