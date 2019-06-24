Almacenar un Valor
===

Ahora que tiene declarada su variable de almacenamiento en el motor de ejecución, puede crear una función para agregarle un valor.

## Declarar una Función Pública

Necesita definir funciones del motor de ejecución que inserten y modifiquen los valores de almacenamiento. Esto se puede hacer dentro del macro `decl_module!`, el cuál declara todos los puntos de entrada que maneja su módulo.

Aquí hay un ejemplo de la declaración de una función pública:

```rust
// Importe lo sigiente: 
//
// use support::{dispatch::Result, StorageValue};
// use system::ensure_signed;
decl_module! {
    pub struct Module<T: Trait> for enum Call where origin: T::Origin {

        fn my_function(origin, input_bool: bool) -> Result {
            let _sender = ensure_signed(origin)?;

            <MyBool<T>>::put(input_bool);
            
            Ok(())
        }
    }
}
```

## Estructura de Una Función

Las funciones públicas de un módulo, siempre deben tomar la forma:

```rust
fn foo(origin, bar: Bar, baz: Baz, ...) -> Result;
```

### Origen (Origin)

El primer argumento de estas funciones siempre es `origin`. `origin` contiene información sobre el origen de la llamada. Esto generalmente se divide en tres grupos:

- Llamadas públicas que estén firmadas por una cuenta externa.
- Llamadas de raíz que solo pueden ser realizadas por el sistema de gobierno.
- Llamadas inherentes que solo los autores de bloque y los validadores pueden realizar.

Consulte la definición de [Orign](https://substrate.dev/docs/en/overview/glossary#origin) en el Glosario de Substrate.

### Resultado (Result)

Además, estas funciones deben devolver el tipo [`Result`](https://crates.parity.io/srml_support/dispatch/result/index.html) proveniente del módulo `support::dispatch`. Esto significa que una llamada exitosa siempre devolverá `Ok(())`, de lo contrario, la lógica debería detectar cualquier error que pueda causar un problema y devolver un `Err()`.

Hay dos cosas extremadamente importantes para recordar:

- NO DEBE ENTRAR EN UNA CONDICIÓN DE PÁNICO: bajo ninguna circunstancia (almacenar, quizás, en caso de que el almacenamiento que se encuentre en un estado de daño irreparable) debe ser una función de pánico.
- NO HAY EFECTOS SECUNDARIOS EN EL ERROR: Esta función debe completarse totalmente y devolver `Ok(())`, o no debe tener efectos secundarios en el almacenamiento y devolver `Err ('Mensaje')`.

Verá un poco de esto más adelante. A lo largo de este tutorial, nos aseguraremos de que se cumplan ambas condiciones y le recordaremos que haga lo mismo.

## Comprobación de un Mensaje Firmado

Como ya se mencionó, el primer argumento en cualquiera de estas funciones de módulo es el "origen". Hay tres llamadas que le serán muy útiles ya que que hacen la comparación por usted y devuelven un resultado conveniente: `ensure_signed`,`ensure_root` y `ensure_inherent`. **Siempre utilizarlas para validar al inicio de su función.**

Podemos usar la función `ensure_signed()` de `system::ensure_signed` para verificar el origen, y "asegurar" que el mensaje esté firmado por una cuenta válida. Incluso podemos derivar la cuenta que firmó el mensaje.

## ¡Su Turno!

Use la plantilla para crear una función `set_value ()` que permitirá al usuario enviar un mensaje firmado e inserte un `u64` en el almacenamiento del motor de ejecución.

<!-- tabs:start -->

#### ** Template **

[embedded-code](../../1/assets/1.3-template.rs ':include :type=code embed-template')

#### ** Solution **

[embedded-code-final](../../1/assets/1.3-finished-code.rs ':include :type=code embed-final')

#### ** Previous Chapter Solution **

[embedded-code](../../1/assets/1.2-finished-code.rs ':include :type=code embed-previous')

<!-- tabs:end -->

---
**Aprenda Más**

Si intenta compilar los ejemplos de código que le mostramos anteriormente sin haber importado las bibliotecas requeridas, obtendrá algunos errores:

```rust
error[E0425]: cannot find function `ensure_signed` in this scope
  --> src/substratekitties.rs:15:27
   |
15 |             let _sender = ensure_signed(origin)?;
   |                           ^^^^^^^^^^^^^ not found in this scope
help: possible candidate is found in another module, you can import it into scope
   |
2  | use system::ensure_signed;
   |
```

Como puede ver, agregamos algunas funciones en nuestro código que aún no habíamos importado en nuestro módulo. Rust realmente puede ayudarnos aquí sugiriendo maneras de resolver estos problemas. Si escuchamos a Rust, entonces podemos simplemente agregar estas declaraciones de 'uso' en la parte superior para que nuestro código se compile nuevamente:

```rust
use system::ensure_signed;
```

Como se mencionó en la sección de "patrones comunes", que Rust será su amigo durante el desarrollo del motor de ejecución, y "principalmente" lo ayudará a superar cualquier problema en su código. En el futuro, intentaremos mencionar cada vez que necesite importar una nueva biblioteca, pero no se preocupe cuando el compilador le envíe algunos errores. En su lugar, adopte las sugerencias que podría darle.

---
