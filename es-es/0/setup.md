Instalación y Configuración
===

Substrate proporciona un comando simple para preparar su máquina e iniciar el desarrollo:

```bash
curl https://getsubstrate.io -sSf | bash -s -- --fast
```

Este comando descargará e instalará diferentes dependencias como: Rust, git y más ... Si está interesado, puede consultar los comandos yendo [a la url](https://getsubstrate.io).

> Tenga en cuenta que estamos usando la bandera `--fast` para omitir la construcción del ejecutable de `substrate` y `subkey`. Estos no son necesarios para completar este taller, sin embargo, pueden ser requeridos en otros tutoriales / guías de Substrate.

Si está intentando desarrollar en un sistema operativo incompatible con este script (por ejemplo, Windows), puede consultar las instrucciones de configuración [en el repositorio de Sustrate](https://github.com/paritytech/substrate#61-hacking-on-substrate).

Mientras espera a que Substrate y sus dependencias asociadas (por ejemplo, Rust, etc.) se descargen e instalen, hay otros programas que no están incluidos en el paquete y deben instalarse en su entorno de desarrollo:

 - [Node + NPM](https://nodejs.org/en/download/)
 - [Yarn](https://yarnpkg.com)
 - [Visual Studio Code](https://code.visualstudio.com/) o el entorno de desarrollo de su preferencia.
 - [Chrome](https://www.google.com/chrome/) o un explorador basado en Chromium.

---

**Aprenda Más**

La interfaz de usuario utiliza WebSockets y una conexión cifrada para conectarse a la instancia del nodo local de Substrate. La mayoría de los navegadores no permiten este tipo de conexión por razones de seguridad y privacidad, solo Chrome permite esta conexión _si se está conectando a localhost_. Es por eso que estamos usando Chrome en este taller. Si desea conectarse al navegador utilizando una computadora diferente en la red, debe ser a través de una conexión segura.

Si desea utilizar un navegador diferente a lo largo de este taller, deberá clonar y ejecutar la [Interfaz de Usuario de aplicaciones para Polkadot.js](https://github.com/polkadot-js/apps) localmente, y así pueda utilizar una conexión no segura.

---