Interactuando Con El Nodo
===

Al final de este tutorial, le pediremos que cree su propia interfaz de usuario para interactuar con su blockchain. Mientras tanto, podemos usar la interfaz de usuario de [Polkadot-JS](https://polkadot.js.org), que es una interfaz generalizada y se adapta a su nodo personalizado.

Abre **Chrome** y navega a:

https://polkadot.js.org/apps/

Para apuntar la interfaz de usuario a su nodo local, necesita ajustar la **Configuración**. Simplemente seleccione 'Nodo local (127.0.0.1:9944)' en el menú desplegable:

```
Settings > remote node/endpoint to connect to > Local Node (127.0.0.1:9944)
```

![An image of the settings in Polkadot-JS Apps UI](../../0/assets/polkadot-js-settings.png)

Después de presionar **Save & Reload** (guardar y volver a cargar), debe notar que la interfaz de usuario de Polkadot-JS cobra vida.
Tenga en cuenta que debe tener su blockchain `./target/substratekitties --dev` en funcionamiento mientras navega por la interfaz de usuario de Polkadot.

Toma en cuenta que más adelante en la sección "Registro de una estructura personalizada" de la Sección 1 > Visualización de una estructura, importaremos un archivo JSON con definiciones de nuevos tipos de datos.

Vayamos a la sección **Transfer** y hagamos una transacción. La cuenta predeterminada llamada "Alice" se financia previamente con una tonelada de *Unidades*.

Genera una transacción para compartirle algo a "Bob". Debería ver una confirmación cuando la transacción se haya completado, y el saldo de Bob también se actualizará.

![First Transfer in Polkadot-JS Apps UI](../../0/assets/first-transfer.png)

Aquí puedo ver con qué rapidez configuramos, ejecutamos e interactuamos con nuestra propia cadena de manera local.

---
**Aprenda Más**

La [interfaz de usuario de Substrate](https://github.com/paritytech/substrate-ui) que se ha creado utilizando la [biblioteca oo7-substrate](https://github.com/paritytech/oo7/tree/master/packages/oo7-substrate). Esta es una interfaz de front-end alternativa a la [de Polkadot-JS Apps](https://github.com/polkadot-js/apps) que sirve para interactuar con la cadena de bloques desarrollada con Substrate.

Al crear su propia interfaz gráfica, puede consultar la [Documentación de la API de Polkadot-JS](https://polkadot.js.org/api/) y la [Documentación de la API de oo7](https://paritytech.github.io/oo7/).
