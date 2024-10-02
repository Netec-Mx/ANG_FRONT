## Proyecto practica API rick and morty

- La idea de este proyecto es poder ver los personajes y realizar una pequena busqueda o poder filtrar en el servicio el nombre que deseo buscar

- Pagina donde se encuentra la api
- [Link](https://rickandmortyapi.com/documentation/#get-all-characters)

Ejemplo

- [RickandMorty](./assets/Captura%20de%20pantalla%202024-09-30%20144019.png)

- Requisitos de ambiente

Nodejs v14
Angular v12.0.5


## instalar nvm y la instalacion del Nodejs y la ver

1. Paso 1: Desinstalar Node.js (si ya está instalado)

- Si tienes una versión diferente de Node.js instalada, desinstálala primero. La forma de hacerlo dependerá de tu sistema operativo:

- Windows: Ve al Panel de Control > Programas > Desinstalar un programa, y busca Node.js.

- macOS: Puedes usar Homebrew para desinstalarlo:

```bash

brew uninstall node
```

Linux: Usa el gestor de paquetes correspondiente (como apt, yum, etc.) para desinstalar Node.js.

2. Paso 2: Instalar Node.js versión 14

- *Usar Node Version Manager (NVM)*:

- La forma más fácil de gestionar versiones de Node.js es mediante NVM. Si no lo tienes instalado, puedes hacerlo así:

*Instalar NVM*:

```bash

curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.4/install.sh | bash
```

- Luego, cierra y vuelve a abrir tu terminal, o ejecuta:

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

- *Instalación Manual:*

- Si prefieres no usar NVM, puedes descargar el instalador de Node.js versión 14 directamente desde nodejs.org y seguir las instrucciones de instalación según tu sistema operativo.

3. Paso 3: Verificar la Instalación de Angular
- Una vez que hayas instalado Node.js versión 14, puedes verificar o instalar Angular CLI:

```bash

npm install -g @angular/cli@12.0.5
```

4. Paso 4: Crear un Nuevo Proyecto
- Ahora puedes crear un nuevo proyecto Angular con la versión deseada:

```bash

ng new rick-and-morty
cd rick-and-morty
```


## Estructura que vamos a manejar


rick-and-morty/
│
├── e2e/
│
├── node_modules/
│
└── src/
    ├── app/
    │   ├── components/
    │   │   ├── pages/
    │   │   │   ├── characters/
    │   │   │   │   ├── character-detail/
    │   │   │   │   │   ├── character-detail.routing.module.ts
    │   │   │   │   │   ├── character-detail.component.html
    │   │   │   │   │   ├── character-detail.component.scss
    │   │   │   │   │   ├── character-detail.component.ts
    │   │   │   │   │   ├── character-detail.component.spec.ts
    │   │   │   │   │   └── character-detail.module.ts
    │   │   │   │   └── character-list/
    │   │   │   │       ├── character-list.routing.module.ts
    │   │   │   │       ├── character-list.component.html
    │   │   │   │       ├── character-list.component.scss
    │   │   │   │       ├── character-list.component.ts
    │   │   │   │       ├── character-list.component.spec.ts
    │   │   │   │       └── character-list.module.ts
    │   │   │   └── home/
    │   │   │       ├── home.routing.module.ts
    │   │   │       ├── home.component.html
    │   │   │       ├── home.component.scss
    │   │   │       ├── home.component.ts
    │   │   │       ├── home.component.spec.ts
    │   │   │       └── home.module.ts
    │   │   └── shared/
    │   │       ├── components/
    │   │       │   ├── form-search/
    │   │       │   │   └── form-search.component.ts
    │   │       │   └── header/
    │   │       │       ├── header.component.html
    │   │       │       ├── header.component.scss
    │   │       │       ├── header.component.ts
    │   │       │       └── header.component.spec.ts
    │   │       ├── interfaces/
    │   │       │   └── character.interfaces.ts
    │   │       ├── models/
    │   │       │   └── trackHttpError.ts
    │   │       └── services/
    │   │           ├── character.service.ts
    │   │           └── character.service.spec.ts
    │   │
    │   ├── app.routing.module.ts
    │   ├── app.component.html
    │   ├── app.component.scss
    │   ├── app.component.ts
    │   ├── app.component.spec.ts
    │   └── app.module.ts
    │
    ├── assets/
    │   └── icons/
    │
    └── environments/
        ├── environment.prod.ts
        └── environment.ts

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

Paso 1: Configurar el Proyecto
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

- Paso 1: Crear un Componente

- Ejemplo de como podriamos crear el componente en la estructura que tenemos con el archivo routing

```bash

ng generate component app/components/pages/characters/character-list -routing
```

- Paso 2: Crear un Módulo

```bash

ng generate module app/components/pages/characters/characters
```

- Paso 3: Crear un Servicio

```bash

ng generate service app/components/shared/services/character
```






















