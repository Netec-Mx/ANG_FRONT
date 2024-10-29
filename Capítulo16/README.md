# Práctica 16. Proyecto práctica API rick and morty

## Objetivo de la práctica

- Ver a los personajes realizar una pequeña búsqueda o poder filtrar en el servicio el nombre que deseo buscar.

## Duración aproximada
- 115 minutos.

## Instrucciones

- Pagina donde se encuentra la API
- [Link](https://rickandmortyapi.com/documentation/#get-all-characters)

Ejemplo

- [RickandMorty](./assets/Captura%20de%20pantalla%202024-09-30%20144019.png)

- Requisitos de ambiente

Nodejs v14
Angular v12.0.5


## Instalar nvm y la instalacion del Nodejs y la ver

1. Paso 1: Desinstalar Node.js (si ya está instalado)

- Si tienes una versión diferente de Node.js instalada, desinstálala primero. La forma de hacerlo dependerá de tu sistema operativo:

- Windows: Ir al Panel de Control > Programas > Desinstalar un programa, y buscar Node.js.

- macOS: Puedes usar Homebrew para desinstalarlo:

```bash

brew uninstall node
```

Linux: Usar el gestor de paquetes correspondiente (como apt, yum, etc.) para desinstalar Node.js.

2. Paso 2: Instalar Node.js versión 14.

- *Usar Node Version Manager (NVM)*:

- La forma más fácil de gestionar versiones de Node.js es mediante NVM. Si no lo tienes instalado, puedes hacerlo así:

*Instalar NVM*:

```bash

curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.4/install.sh | bash
```

-  Cerrar y volver a abrir la terminal, o ejecutar:

```bash

source ~/.bashrc
```

- Instalar Node.js versión 14:

```bash

nvm install 14
```

- Usar Node.js versión 14:

```bash

nvm use 14
```

- Verificar la instalación:

```bash

node -v
```

Deberías ver algo como v14.x.x.

- *Instalación manual:*

- Si prefieres no usar NVM, puedes descargar el instalador de Node.js versión 14 directamente desde nodejs.org y seguir las instrucciones de instalación según tu sistema operativo.

3. Paso 3: Verificar la instalación de Angular.
- Una vez que hayas instalado Node.js versión 14, puedes verificar o instalar Angular CLI:

```bash

npm install -g @angular/cli@12.0.5
```

4. Paso 4: Crear un nuevo proyecto
- Ahora puedes crear un nuevo proyecto Angular con la versión deseada:

```bash

ng new rick-and-morty
cd rick-and-morty
```


## Estructura que vamos a manejar


rick-and-morty/<br>
│<br>
|── e2e/<br>
│<br>
├── node_modules/<br>
│<br>
└── src/<br>
    ├── app/<br>
    │   ├── components/<br>
    │   │   ├── pages/<br>
    │   │   │   ├── characters/<br>
    │   │   │   │   ├── character-detail/<br>
    │   │   │   │   │   ├── character-detail.routing.module.ts<br>
    │   │   │   │   │   ├── character-detail.component.html<br>
    │   │   │   │   │   ├── character-detail.component.scss<br>
    │   │   │   │   │   ├── character-detail.component.ts<br>
    │   │   │   │   │   ├── character-detail.component.spec.ts<br>
    │   │   │   │   │   └── character-detail.module.ts<br>
    │   │   │   │   └── character-list/<br>
    │   │   │   │       ├── character-list.routing.module.ts<br>
    │   │   │   │       ├── character-list.component.html<br>
    │   │   │   │       ├── character-list.component.scss<br>
    │   │   │   │       ├── character-list.component.ts<br>
    │   │   │   │       ├── character-list.component.spec.ts<br>
    │   │   │   │       └── character-list.module.ts<br>
    │   │   │   └── home/<br>
    │   │   │       ├── home.routing.module.ts<br>
    │   │   │       ├── home.component.html<br>
    │   │   │       ├── home.component.scss<br>
    │   │   │       ├── home.component.ts<br>
    │   │   │       ├── home.component.spec.ts<br>
    │   │   │       └── home.module.ts<br>
    │   │   └── shared/<br>
    │   │       ├── components/<br>
    │   │       │   ├── form-search/<br>
    │   │       │   │   └── form-search.component.ts<br>
    │   │       │   └── header/<br>
    │   │       │       ├── header.component.html<br>
    │   │       │       ├── header.component.scss<br>
    │   │       │       ├── header.component.ts<br>
    │   │       │       └── header.component.spec.ts<br>
    │   │       ├── interfaces/<br>
    │   │       │   └── character.interfaces.ts<br>
    │   │       ├── models/<br>
    │   │       │   └── trackHttpError.ts<br>
    │   │       └── services/<br>
    │   │           ├── character.service.ts<br>
    │   │           └── character.service.spec.ts<br>
    │   │<br>
    │   ├── app.routing.module.ts<br>
    │   ├── app.component.html<br>
    │   ├── app.component.scss<br>
    │   ├── app.component.ts<br>
    │   ├── app.component.spec.ts<br>
    │   └── app.module.ts<br>
    │<br>
    ├── assets/<br>
    │   └── icons/<br>
    │<br>
    └── environments/<br>
        ├── environment.prod.ts<br>
        └── environment.ts<br>

## Instalacion de bibliotecas externas

- Bootstrap

```bash

npm install bootstrap
```

- jQuery

```bash

npm install jquery
```

- Popper.js

```bash

npm install @popperjs/core
```

- ngx-infinite-scroll

```bash

npm install ngx-infinite-scroll

```

Paso 1: Configurar el proyecto.
- Configurar angular.json

- Agrega Bootstrap y jQuery en la sección de estilos y scripts:
```json

"styles": [
  "src/styles.scss",
  "node_modules/bootstrap/dist/css/bootstrap.min.css"
],
"scripts": [
  "node_modules/jquery/dist/jquery.min.js",
  "node_modules/@popperjs/core/dist/umd/popper.min.js"
]

```

## commands Line para crear Componentes, servicios, module y archivo routing

- Paso 1: Crear un componente.

- Ejemplo de como podriamos crear el componente en la estructura que tenemos con el archivo routing:

```bash

ng generate component app/components/pages/characters/character-list -routing
```

- Paso 2: Crear un módulo.

```bash

ng generate module app/components/pages/characters/characters
```

- Paso 3: Crear un servicio.

```bash

ng generate service app/components/shared/services/character
```






















