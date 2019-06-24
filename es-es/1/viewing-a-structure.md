Viendo una Estructura
===

Ahora que ha configurado el motor de ejecución para hacer gatitos (`kitties`), ¡puede revisar su trabajo!

Introdujo una estructura personalizada en su cadena, aún cuando la interfaz de usuario de Polkadot-JS Apps es muy buena para adaptarse a sus cambios, en esta situación, debe darle una pista sobre cómo deserializar los datos estructurados.

>RECORDATORIO: recuerde limpiar su cadena para que comience de nuevo cuando interactúe con la interfaz de usuario:
>
> ```
> ./scripts/build.sh
> cargo build --release
> ./target/release/substratekitties purge-chain --dev
> ./target/release/substratekitties --dev
> ```

## Registro de una Estructura personalizado

Afortunadamente, la interfaz de usuario de Polkadot-JS Apps proporciona una forma muy sencilla de importar estructuras personalizadas para que la página pueda decodificar la información correctamente.

Vuelva a la aplicación  **Settings**. En la sección **Developer** , puede cargar un archivo JSON con su estructura personalizada o agregarlo manualmente a través de un editor de código. Copie y pegue este objeto JSON en el editor de código y presione `Save`.

```
{
    "Kitty": {
        "id": "H256",
        "dna": "H256",
        "price": "Balance",
        "gen": "u64"
    }
}
```

## Creando un Kitty

Now we can go and create a new kitty. In the **Extrinsics** app, go to:

```
substratekitties > createKitty()
```

Una vez que presione `submit`, deberá ver que la transacción se ejecuta:

![Image of creating a kitty in the Polkadot-JS Apps UI](../../1/assets/creating-a-kitty.png)

## Viendo un Kitty

Finalmente, podemos ingresar a la aplicación **Chain State** y ver nuestro gatito almacenado al seleccionar:

```
kittyStorage > ownedKitty(AccountId): Kitty
```

Luego, seleccione un usuario que haya llamado a la función `createKitty ()`. Entonces deberías poder ver las propiedades individuales del objeto `Kitty`:

![Image of viewing a kitty object in the Polkadot UI](../../1/assets/view-kitty.png)

---
**Aprenda Más**

[TODO: make this a page]

---
