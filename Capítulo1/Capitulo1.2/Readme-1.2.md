## Taller Instalar Angular CLI 1.2

## Pasos para instalar Visual Studio Code:

1. Descargar Visual Studio Code:

    Dirígete a la página oficial de Visual Studio Code: [Link](https://code.visualstudio.com/)

    Selecciona la versión compatible con tu sistema operativo (Windows, macOS, o Linux).

2. Instalar:

    Una vez descargado el archivo, ábrelo y sigue las instrucciones del instalador. Acepta los términos y condiciones, y selecciona las opciones que prefieras (como crear un acceso directo en el escritorio).

3. Configuración básica:

    Abre Visual Studio Code y configura tu entorno preferido. Puedes ajustar el tema (claro u oscuro) y personalizar la interfaz según tus necesidades.

    Instala las extensiones necesarias para Angular, como Angular Essentials o Angular Snippets, para facilitar el desarrollo.

[visual studio](../../images/Captura%20de%20pantalla%202024-09-22%20171815.png)

## Instalar Angular CLI

1. Una vez que tengas Node.js, abre una terminal (o el símbolo del sistema) y ejecuta el siguiente comando para instalar Angular CLI globalmente:

    ```
    npm install -g @angular/cli
    ```

    ![img](../../images/img-1.png)

2. Verifica si quedo bien instalado angular

    Ejecuta en el cmd

    ```
    ng version
    ```

    ![img](../../images/img-5.png)

3. Crear una nueva aplicación

    Usa Angular CLI para generar una nueva aplicación. En la terminal, navega a la carpeta donde deseas crear tu proyecto y ejecuta:

    ![img](../../images/img-2.png)

    Reemplaza nombre-de-tu-aplicacion on el nombre que desees. Durante la creación, 
    Angular CLI te preguntará si deseas incluir ciertas características (como el enrutamiento). Responde según tus preferencias.

4. Navegar al directorio de la aplicación

    Después de que Angular CLI haya creado tu proyecto, navega a la carpeta del proyecto:

    ![img](../../images/img-3.png)

5. Iniciar el servidor de desarrollo

    Ahora, inicia el servidor de desarrollo para ver tu aplicación en acción:

    ![img](../../images/img-4.png)

    Por defecto, la aplicación estará disponible en

    [LocalHost](http://localhost:4200/)

    Abre esa dirección en tu navegador y deberías ver la página de inicio de tu nueva aplicación Angular.

6. Para pausar el servidor le das clic encima de la terminal y oprimes Crontol + c

