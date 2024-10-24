# Práctica 15. Despliegue y configuración de aplicaciones Angular: Preparación para producción 

## Objetivo de la práctica

- Desplegar una aplicación Angular de manera efectiva y configurar entornos adecuados para el desarrollo, pruebas y producción.
- Aprender sobre las opciones de compilación de Angular, cómo ajustar la configuración para diferentes entornos y la importancia de herramientas como Angular DevTools para optimizar el rendimiento y la depuración de tu aplicación.
- Garantizar que tu aplicación funcione correctamente en diversas configuraciones y que esté lista para el uso en producción.

## Duración aproximada
- 40 minutos.

## Instrucciones

1. Construcción de la aplicación.
-  Antes de desplegar la aplicación, es necesario crear una versión optimizada para producción. Angular ofrece un comando para esto:

```bash

ng build --prod
```

- Este comando genera una carpeta dist/ que contiene archivos optimizados (HTML, CSS, JavaScript) listos para ser desplegados.

2. Configuración del servidor.

- El siguiente paso es configurar el servidor donde se alojará la aplicación. Puedes usar servidores web comunes como:

- Apache
- Nginx
- Servidores en la nube como AWS, Heroku, Firebase, entre otros.

3. Copiar archivos al servidor.

- Copiar el contenido de la carpeta dist/ al directorio raíz del servidor. Esto puede hacerse mediante FTP, SCP, o mediante herramientas de integración continua.

4. Configuración de rutas (si es necesario).

- Si la aplicación Angular utiliza el enrutador, es importante asegurarse de que el servidor esté configurado para redirigir todas las rutas a index.html. Esto se debe a que Angular maneja las rutas del lado del -  

- cliente. Por ejemplo, en Nginx, podrías tener una configuración como esta:

```nginx

location / {
    try_files $uri $uri/ /index.html;
}
```

5. Probar la aplicación.

-  Una vez desplegada, es fundamental probar la aplicación en el entorno de producción para asegurarte de que todo funcione correctamente. Verificar las rutas, la interacción con APIs y la carga de recursos.

6. Monitoreo y mantenimiento.

- Después del despliegue, es recomendable monitorear el rendimiento de la aplicación y el uso de recursos. Herramientas como Google Analytics, Sentry o incluso herramientas de monitoreo de servidores pueden ayudar a mantener la aplicación en buen estado.


## Angular Compile Options

1. Compilación JIT vs AOT.

- Angular permite dos modos de compilación:

- *JIT (Just-in-Time)*: Compila los archivos TypeScript en el navegador al momento de ejecutar la aplicación. Es útil durante el desarrollo porque permite ver los cambios en tiempo real, pero no es óptimo para producción debido a su mayor tamaño y tiempo de carga.

- *AOT (Ahead-of-Time)*: Compila la aplicación en tiempo de construcción, lo que significa que se generan archivos JavaScript optimizados antes de que la aplicación se ejecute. Esto resulta en un mejor rendimiento, menores tamaños de archivos y una verificación de errores más temprana.

- Para activar AOT, utilizar el siguiente comando al construir la aplicación:

```bash

ng build --prod --aot
```

2. Options en el angularCompilerOptions.

- En el archivo tsconfig.app.json, puedes encontrar la sección angularCompilerOptions, que permite configurar diferentes aspectos del compilador. Algunas opciones comunes son:

- *enableIvy*: Permite habilitar o deshabilitar Ivy, el motor de renderizado de Angular. Ivy mejora el rendimiento y el tamaño del paquete.

- *fullTemplateTypeCheck*: Activa la verificación completa de tipos en las plantillas, ayudando a detectar errores de tipo en las plantillas de Angular.

- *strictTemplates*: Habilita una verificación más estricta de las plantillas, lo que ayuda a detectar problemas de tipos y errores en tiempo de compilación.

3. Optimización de la aplicación.

- Las Compile Options también pueden influir en cómo se optimiza la aplicación:

- *enableResourceInlining*: Permite incluir recursos (como CSS y HTML) en los archivos generados, lo que reduce la cantidad de solicitudes HTTP.

- *preserveSymlinks*: Ayuda en la resolución de dependencias, especialmente en proyectos monorepo donde se pueden tener enlaces simbólicos.

4. Errores y advertencias en tiempo de compilación.

- Configurar adecuadamente las opciones del compilador puede ayudar a detectar errores antes de que se ejecuten en el navegador. Usar opciones como fullTemplateTypeCheck y strictTemplates puede mejorar la calidad del código y reducir la cantidad de errores en producción.

5. Ejemplo de configuración.

- Aquí tienes un ejemplo de cómo se podría configurar angularCompilerOptions en tsconfig.app.json:

```json

{
  "angularCompilerOptions": {
    "enableIvy": true,
    "fullTemplateTypeCheck": true,
    "strictTemplates": true,
    "enableResourceInlining": true
  }
}
```

## Configuración de Entornos en Aplicaciones Angular

1. Archivos de entorno.

-  Angular permite crear múltiples archivos de entorno en la carpeta src/environments. Por defecto, Angular incluye dos archivos:

- *environment.ts*: Configuración para el entorno de desarrollo.
- *environment.prod.ts*: Configuración para el entorno de producción.

- Puedes agregar más archivos si necesitas configuraciones para otros entornos (por ejemplo, staging, testing).

2. Estructura de los archivos de entorno.

- Cada archivo de entorno exporta un objeto que contiene las configuraciones específicas. Aquí hay un ejemplo básico:

```typescript

// environment.ts (Desarrollo)
export const environment = {
  production: false,
  apiUrl: 'http://localhost:3000/api',
  featureFlag: false
};

// environment.prod.ts (Producción)
export const environment = {
  production: true,
  apiUrl: 'https://api.ejemplo.com',
  featureFlag: true
};

```

3. Uso de los archivos de entorno.

- Para usar las configuraciones del entorno en tus componentes o servicios, simplemente importa el objeto environment correspondiente. Por ejemplo:

```typescript

import { environment } from '../environments/environment';

const apiUrl = environment.apiUrl;

```

- Esto permite que la aplicación utilice la configuración correcta según el entorno en el que se esté ejecutando.

4. Construcción de la aplicación.

- Al construir la aplicación, Angular selecciona automáticamente el archivo de entorno apropiado según el comando utilizado. Por ejemplo:

- Para desarrollo:

```bash

ng serve
```

- Para producción:

```bash

ng build --prod
```

- Angular reemplazará el archivo environment.ts con environment.prod.ts durante el proceso de construcción.

5. Configuración de múltiples entornos.

-  Si necesitas más entornos (como staging), puedes crear otro archivo, por ejemplo, environment.staging.ts. Luego, en el archivo angular.json, debes añadir una configuración adicional para que Angular sepa qué archivo usar:

```json

"configurations": {
  "production": {
    "fileReplacements": [
      {
        "replace": "src/environments/environment.ts",
        "with": "src/environments/environment.prod.ts"
      }
    ]
  },
  "staging": {
    "fileReplacements": [
      {
        "replace": "src/environments/environment.ts",
        "with": "src/environments/environment.staging.ts"
      }
    ]
  }
}
```

- Y para construir para staging, puedes usar:

```bash

ng build --configuration=staging
```

6. Buenas prácticas.

- *No Exponer información sensible*: Asegurarse de no incluir credenciales o información sensible en los archivos de entorno.
- *Variables de entorno en el servidor*: Para configuraciones que no deben estar en el código fuente, considerar el uso de variables de entorno en el servidor donde se despliega la aplicación.
- *Separar Configuraciones*: Mantener las configuraciones bien organizadas y separadas por entorno para evitar confusiones.

## Angular DevTools

1. Instalación.

- Angular DevTools está disponible como una extensión para navegadores como Chrome y Firefox. Para instalarla:

- *Chrome*: Buscar "Angular DevTools" en la Chrome Web Store e instalarla directamente.
- *Firefox*: También está disponible en el sitio de complementos de Firefox.

2. Interfaz de usuario.

- Una vez instalada, acceder a Angular DevTools desde las herramientas de desarrollo del navegador (presionando F12 o Ctrl + Shift + I). Aparecerá una pestaña específica llamada "Angular".

3. Características principales.

a. *Inspección de componentes*

- Angular DevTools permite inspeccionar la estructura de componentes de tu aplicación. Puedes ver:

- *Componentes*: Una lista de todos los componentes en la aplicación.
- *Propiedades*: Ver y editar las propiedades de los componentes en tiempo real.
- *Árbol de Componentes*: Visualiza cómo se relacionan los componentes entre sí.

b. *Seguimiento de cambios*

- La herramienta muestra los cambios en el estado de los componentes. Puedes ver cuándo y dónde se producen las actualizaciones, lo que facilita la identificación de problemas relacionados con el ciclo de detección de cambios de Angular.

c. *Análisis de rendimiento*

- Angular DevTools proporciona herramientas para medir el rendimiento de la aplicación. Se pueden identificar componentes que están consumiendo muchos recursos o que tienen un tiempo de renderizado elevado. Esto es crucial para optimizar la experiencia del usuario.

d. *Depuración de servicios*

- La extensión permite explorar y depurar los servicios inyectados en los componentes, facilitando la verificación de su estado y funcionamiento.

4. Uso de la herramienta.

*Inspeccionar componentes*: Selecciona un componente en el árbol para ver sus propiedades, métodos y el estado actual.

*Medir rendimiento*: Usa las herramientas de rendimiento para grabar y analizar el ciclo de vida de los componentes, identificando cuellos de botella.

*Editores en tiempo real*: Realiza cambios en las propiedades de los componentes y observar los resultados en tiempo real.

5. Mejoras en el ciclo de desarrollo.

- Angular DevTools simplifica muchas tareas comunes en el desarrollo de aplicaciones Angular, como:

*Debugging*: Facilita la identificación de errores y el seguimiento de problemas en el código.

*Optimización*: Ayuda a encontrar áreas de mejora en el rendimiento, lo que se traduce en una mejor experiencia para el usuario final.

*Aprendizaje*: Proporciona una forma visual para comprender cómo funcionan los componentes y servicios en Angular, lo que es especialmente útil para desarrolladores nuevos.
