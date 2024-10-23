# Práctica 1.4. Pasos para migrar a una nueva versión de Angular

## Objetivo de la práctica

- Crear tu primer proyecto con Angular utilizando la CLI. 
- Aprender a generar una nueva aplicación.
- Comprender la estructura de un proyecto Angular.
- Configurar el entorno de desarrollo necesario para comenzar a construir aplicaciones web dinámicas.

## Duración aproximada
- 30 minutos.

## Instrucciones
1. Actualizar Angular CLI y Angular Core

- Primero, necesitas actualizar tanto el Angular CLI como el core de Angular en tu proyecto. Puedes hacerlo utilizando el siguiente comando:

    ```
        ng update @angular/core @angular/cli
    ```

- Este comando actualizará automáticamente las dependencias de Angular a la última versión disponible. Asegúrate de que tu CLI global también esté actualizada:

    ```
        npm install -g @angular/cli@latest
    ```

2. Verificar y resolver las dependencias de terceros

Muchas veces las dependencias externas, como bibliotecas o componentes de terceros, podrían no ser compatibles con la nueva versión de Angular. Para identificar problemas, utiliza el comando:

    ```
        npm outdated
    ```

Esto te mostrará las dependencias que necesitan actualización. Es recomendable buscar versiones compatibles o actualizadas de estas dependencias, o buscar alternativas en caso de que ya no sean compatibles.

3.  Realizar cambios requeridos por migraciones automáticas

- Angular proporciona un conjunto de migraciones automáticas que ajustan tu código a los cambios importantes entre versiones. Durante el proceso de actualización, el comando ng update puede mostrarte pasos adicionales que debes realizar manualmente o cambios automáticos en tu código.'

- Puedes ver detalles específicos de cada migración en la [Angular Update Guide](https://angular.dev/update-guide), la cual te indica los pasos recomendados según la versión de Angular de la que estás migrando.

4. Actualizar TypeScript

- Las nuevas versiones de Angular generalmente requieren versiones actualizadas de TypeScript. Asegúrate de tener la versión recomendada:

    ```
        npm install typescript@latest
    ```

- Revisar el archivo tsconfig.json para asegurarte de que los ajustes de compilación sean compatibles con la nueva versión de TypeScript.

5. Verificar configuraciones y polyfills

- Angular puede descontinuar el soporte para ciertos navegadores (como IE11 en versiones más recientes). Asegúrate de revisar y limpiar configuraciones o polyfills que ya no sean necesarios:

- Elimina polyfills de IE si ya no se requieren en tu proyecto (src/polyfills.ts).
        
- Verificar configuraciones específicas de navegadores en el archivo browserslist.

6. Revisar y actualizar el código obsoleto

    Algunas API pueden haber sido descontinuadas o cambiadas en versiones recientes de Angular. Revisa el código utilizando los siguientes pasos:

- Utilizar ng lint para identificar problemas en el código.
        
- Usar las advertencias proporcionadas por el compilador de Angular para actualizar APIs obsoletas.

7. Pruebas Exhaustivas

- Una vez que hayas actualizado el proyecto, realizar pruebas exhaustivas. Asegúrate de probar tanto las pruebas unitarias como las pruebas end-to-end (E2E), ya que los cambios en el framework pueden afectar el comportamiento de tu aplicación.

   Ejecutar tus pruebas unitarias:

    ```
        ng test
    ```

    Y si tienes pruebas end-to-end configuradas:

    ```
        ng e2e
    ```

    
