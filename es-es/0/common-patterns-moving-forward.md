Patrones Comunes
===

## El Compilador de Rust es tu Amigo

Una de las muchas ventajas de usar un lenguaje de programación fuertemente tipado como Rust, es que el compilador puede ser muy útil e incluso sugerir cómo corregir errores del código. Puede obtener más información sobre cómo el compilador de Rust genera sugerencias en [esta publicación](https://jvns.ca/blog/2018/01/13/rust-in-2018--way-easier-to-use/) . Esto será cada vez más obvio entre más desarrolle con Rust. Para comenzar, debe consultar el [Libro de Rust](https://doc.rust-lang.org/book/).

## Actualizar tu Motor de Ejecución

Antes de comenzar a crear un motor de ejecución personalizado, debe estar familiarizado con algunos patrones que le ayudarán a iterar y ejecutar su código.

El código de ejecución de Substrate se compila en dos versiones:

 - Una imagen [WebAssembly](https://webassembly.org/) (Wasm)
 - Un binario ejecutable estándar.

El archivo Wasm se utiliza como parte de la compilación del binario ejecutable, por lo que es importante que siempre compile su imagen Wasm antes de compilar el binario.

El orden debe ser:

```bash
./scripts/build.sh          // Construir Wasm
cargo build --release       // Construir el binario
```

Puede notar que al reiniciar su nodo, la producción de bloques simplemente inicia desde dónde se quedó.
Además, cuando realize cambios en su nodo, los bloques producidos en el pasado por versiones anteriores de su nodo permanecen.

Sin embargo, si sus cambios en el motor de ejecución son significativos, es posible que deba remover todos los bloques anteriores de su cadena utilizando este comando:

```bash
./target/release/substratekitties purge-chain --dev
```

Después de todo esto, podrá reiniciar su nodo nuevamente y verá reflejados los últimos cambios:

```bash
./target/release/substratekitties --dev
```

Recuerde este patrón; Lo va a utilizar mucho.

---

**Aprenda Más**

Siempre debe utilizar la última versión estable de Rust y la nightly.

En el directorio `scripts` existe otro script además de `build.sh`. Este otro debe ejecutarse cada vez que inicie un nuevo proyecto para asegurarnos de instalar y actualizar los requerimientos.

```bash
./scripts/init.sh
```

---
