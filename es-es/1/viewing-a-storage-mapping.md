## Valide su Trabajo en la Interfaz de Usuario Polkadot

A pesar de que este código debe compilarse sin errores, ahora sería un buen momento para revisar su trabajo.

Despues de correr:

```bash
./scripts/build.sh
cargo build --release
./target/release/substratekitties purge-chain --dev
```

Podemos iniciar su nodo:

```bash
./target/release/substratekitties --dev
```

Si regresa a la [Interfaz de Usuario de Polkadot-JS](https://polkadot.js.org/apps), debería ver evidencia de los bloques que producen en el nodo.

## Enviar una Transacción

Vaya a la aplicación **Extrinsics** y, utilizando el menú desplegable de la sección para seleccionar:

```
substratekitties > setValue(value)
```

Escribe un valor y presiona `Submit Transaction`:

![Submit a storage mapping in the Polkadot-JS Apps UI](../../1/assets/submit-storage-mapping.png)

## Ver el Almacenamiento

Ahora que ha enviado una transacción para almacenar un valor, debería validar que el valor esté realmente allí.

Vaya a la aplicación **Chain State** y seleccione:

```
kittyStorage > value(AccountId): u64
```

Con la cuenta con la que envió la transacción, consulte el almacenamiento y presione el botón azul `[+]`:

![Query for storage mapping](../../1/assets/view-storage-mapping.png)

¡Debería ver el mismo valor que guardó! Puede intentar esto con varias cuentas y ver que cada usuario puede almacenar su propio valor en el almacenamiento del motor de ejecución.
