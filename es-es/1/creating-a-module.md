Creando un Módulo
===

Para empezar, debemos crear un nuevo módulo para nuestro motor de ejecución. Para eso trabajaremos con una plantilla vacía que colocaremos en un nuevo archivo `stratekitties.rs`:

```
substratekitties
|
+-- runtime
    |
    +-- src
        |
        +-- lib.rs
        |
        +-- * substratekitties.rs
	|
	+-- template.rs 
```

> **Nota:** También hay un archivo `template.rs` proporcionado por `substrate-node-template`. En general, puedes comenzar a construir tus nuevos módulos con esa plantilla. Sin embargo, para empezar con algo desde cero,crearemos en un nuevo archivo.

**substratekitties<span>.</span>rs**

```rust
use support::{decl_storage, decl_module};

pub trait Trait: system::Trait {}

decl_storage! {
    trait Store for Module<T: Trait> as KittyStorage {
        // Declarar las variables de almacenamiento junto con sus respectivas funciones de lectura (getter)
    }
}

decl_module! {
    pub struct Module<T: Trait> for enum Call where origin: T::Origin {
        // Declarar las funciones públicas
    }
}
```

Puede ver que esta plantilla nos permite comenzar a escribir las partes más básicas de nuestro módulo, las funciones públicas y el almacenamiento.

Pero antes de empezar a hacerlo, debe incluir este archivo en el motor de ejecución. Para hacer esto hay que agregar el módulo en `lib.rs` ubicado en el mismo directorio.

## Actualizando el Motor de Ejecución

Si observa más de cerca el archivo `lib.rs`, notará que contiene detalles sobre todos los módulos que conforman el motor de ejecución. Para cada módulo, se tiene que:

- Importar el archivo Rust que contiene el módulo.
- Implementar su `Trait`
- Incluir el módulo en el macro `construct_runtime!`

Así que hará lo mismo aquí.

Para incluir el nuevo módulo que creó, tiene que agregar la línea indicada con el comentario (`// Agregar esta línea`) cerca de la parte superior del archivo:

```rust
// `lib.rs`
...
pub type BlockNumber = u64;

pub type Nonce = u64;

// Agregar esta línea
mod substratekitties;
...
```

Como no ha definido nada en el módulo, la implementación `Trait` también es muy simple. Puede incluir esta línea después de las otras implementaciones:

```rust
// `lib.rs`
...
impl sudo::Trait for Runtime {
	type Event = Event;
	type Proposal = Call;
}

// Agregar esta línea
impl substratekitties::Trait for Runtime {}
...
```

Finalmente, puede agregar esta línea al final de la definición `construct_runtime!`:

```rust
// `lib.rs`
...
construct_runtime!(
	pub enum Runtime with Log(InternalLog: DigestItem<Hash, Ed25519AuthorityId>) where
		Block = Block,
		NodeBlock = opaque::Block,
		UncheckedExtrinsic = UncheckedExtrinsic
	{
		System: system::{default, Log(ChangesTrieRoot)},
		Timestamp: timestamp::{Module, Call, Storage, Config<T>, Inherent},
		Consensus: consensus::{Module, Call, Storage, Config<T>, Log(AuthoritiesChange), Inherent},
		Aura: aura::{Module},
		Indices: indices,
		Balances: balances,
		Sudo: sudo,
		// Agregar esta línea
		Substratekitties: substratekitties::{Module, Call, Storage},
	}
);
...
```

Tenga en cuenta que ha agregado tres `tipos` a esta definición (`Module`, `Call`, `Storage`). Todos son producto de los macros definidos en nuestra plantilla.

Hasta este punto, el código es válido y debe poderse construir:

```bash
./scripts/build.sh
cargo build --release
```

## Su Turno!

Si aún no lo ha hecho, siga las instrucciones en esta página para configurar su plantilla `substrate-node-template`. Si completó todo con éxito, debería poder compilar su código con éxito y sin errores:

```bash
./scripts/build.sh
cargo build --release
```

Al final de cada sección en este tutorial, su código debe compilarse sin errores. La mayoría de los cambios a lo largo de este tutorial se llevarán a cabo en el archivo `stratekitties.rs`, posteriormente necesitará actualizar una vez más el archivo `lib.rs`.

¡Ahora es el momento de comenzar a agregar algo de su propia lógica!

<!-- tabs:start -->

#### ** Solution **

[embedded-code](../../1/assets/1.1-finished-code.rs ':include :type=code embed-final')

<!-- tabs:end -->

---
**Aprenda Más**

Revise [aquí](https://substrate.dev/docs/en/runtime/macros/construct_runtime) la documentación del macro `construct_runtime!`.

---
