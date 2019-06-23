Correr un Nodo Personalizado
===

Ahora que ha instalado con éxito todos los requisitos, podemos crear de manera rápida un nodo personalizado de Substrate mediante una plantilla preconfigurada.

Substrate es un proyecto en rápida y constante evolución, lo que significa que por el momento hay cambios introducidos de vez en cuando que pueden requerir una refactorización de nuestro código. Para mejorar la experiencia de desarrollo en este taller, hemos creado una versión de trabajo estable y junto con una interfaz de usuario.

Siempre que comience con este paquete de Substrate, debería poder completar el resto de este tutorial sin problema, pero háganos saber si ese no es el caso. Para obtener el paquete, ejecute el siguiente comando en su directorio de trabajo:

```bash
git clone https://github.com/shawntabrizi/substrate-package
```

El repositorio `substrate-package` contiene dos carpetas:

1. `substrate-node-template`
2. `substrate-ui`

Por el momento no tocaremos la carpeta `substrate-ui` hasta el Capítulo 4 de este taller, pero como su nombre lo indica, incluye una interfaz de usuario preconstruida y personalizable, escrita en [React](https://reactjs.org/).

En su lugar, estaremos trabajando principalmente en la carpeta `substrate-node-template` que contiene un nodo de Substrate básico para comenzar a trabajar.

Vayamos a la carpeta del paquete y cambiemos el nombre del proyecto y de las carpetas del proyecto usando el script `strate-package-rename.sh`:

```bash
cd substrate-package
./substrate-package-rename.sh substratekitties <autor>
```

Luego, vaya a la carpeta ahora llamada "substratekitties" y construya el nodo:

```bash
cd substratekitties
./scripts/init.sh
./scripts/build.sh
cargo build --release
```

Este proceso puede tardar un poco, pero una vez que se hace, debería poder iniciar el nodo al ejecutar:

```bash
./target/release/substratekitties --dev
```

Si has hecho todo bien hasta ahora, debería ver cómo se producen los nuevos bloques.

![An image of the node producing new blocks](../../0/assets/building-blocks.png)

¡Bien, acabas de empezar tu propio blockchain personalizado!

---
**Aprenda Más**

El repositorio `substrate-package` se creó mediante [algunos comandos personalizados](https://github.com/paritytech/substrate-up) proporcionados por el comando `getsubstrate.io` que ejecutamos anteriormente.

Si está iniciando un proyecto nuevo y desea obtener la última versión de Substrate, puede crear su propio paquete con Substrate ejecutando:

```bash
substrate-node-new <nombre_proyecto> <autor>
substrate-ui-new <nombre_proyecto>
```

Como se mencionó anteriormente, la única desventaja de este método es que estos scripts se extraen directamente de diferentes repositorios de GitHub, lo que significa que puede haber momentos de incompatibilidad durante las etapas de rápida evolución en Substrate.

---
