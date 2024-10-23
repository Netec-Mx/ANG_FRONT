# RoutingModule y router-outlet en Angular

## Objetivo de la práctica

-

## Duración aproximada
- minutos

## Instrucciones
1. *Creación del RoutingModule*

- Para crear un RoutingModule en Angular, sigue estos pasos:

 *Importar el RouterModule y Routes:*

- En un nuevo archivo llamado app-routing.module.ts, importar RouterModule y Routes desde @angular/router.

```typescript

import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { HomeComponent } from './home/home.component';
import { AboutComponent } from './about/about.component';

const routes: Routes = [
  { path: '', redirectTo: '/home', pathMatch: 'full' },
  { path: 'home', component: HomeComponent },
  { path: 'about', component: AboutComponent },
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

2. *Incluir el RoutingModule en el AppModule:*

- Asegúrate de importar AppRoutingModule en tu app.module.ts.

```typescript

import { AppRoutingModule } from './app-routing.module';

@NgModule({
  declarations: [
    // tus componentes
  ],
  imports: [
    AppRoutingModule,
    // otros módulos
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

- *router-outlet*

- El router-outlet es una directiva que actúa como un marcador de posición en la plantilla de tu aplicación donde se renderizarán los componentes asociados a las rutas definidas en el RoutingModule. Cada vez que se navega a una ruta específica, Angular reemplaza el contenido del router-outlet con el componente correspondiente.

1. Uso de router-outlet

- *Agregar router-outlet en la plantilla:*

- En tu app.component.html, colocar el router-outlet donde quieras que aparezcan los componentes de las rutas:

```html

<nav>
  <a routerLink="/home">Home</a>
  <a routerLink="/about">About</a>
</nav>
<router-outlet></router-outlet>
```

2. Navegación entre Rutas:

- Cuando el usuario hace clic en los enlaces del menú, Angular actualizará automáticamente el contenido dentro del router-outlet con el componente correspondiente, sin recargar la página.

- *Parámetros de Ruta y router-outlet*

- El router-outlet también puede trabajar con rutas que contienen parámetros. Por ejemplo, si tienes una ruta que muestra detalles de un usuario basado en su ID, puedes definirla así:

```typescript

const routes: Routes = [
  { path: 'user/:id', component: UserDetailComponent },
];

```

Y en el router-outlet, Angular cargará UserDetailComponent cuando se navegue a /user/1, por ejemplo.
