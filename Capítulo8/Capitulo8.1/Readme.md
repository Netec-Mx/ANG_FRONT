# Práctica 8.1 Routing en Angular

## Objetivo de la práctica

- Aprender a implementar el enrutamiento en una aplicación Angular.
  
## Duración aproximada
- 20 minutos.

## Instrucciones
- *Instalación del módulo de enrutamiento*

- Para utilizar el enrutamiento en tu aplicación Angular, asegúrate de que el módulo de enrutamiento esté importado. Si creaste tu proyecto usando Angular CLI, este paso ya debería estar configurado. Si no, puedes instalarlo de la siguiente manera:

```bash

ng add @angular/router
```

- *Configuración básica de rutas*

1. Importar el módulo de rutas:
    
`- En tu archivo principal app.module.ts, importar RouterModule y Routes:

```typescript

import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';

const routes: Routes = [
  { path: '', redirectTo: '/home', pathMatch: 'full' },
  { path: 'home', component: HomeComponent },
  { path: 'about', component: AboutComponent },
  // Otras rutas aquí
];

@NgModule({
  declarations: [
    // tus componentes
  ],
  imports: [
    RouterModule.forRoot(routes),
    // otros módulos
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

2. Creación de componentes:

- Asegúrate de tener los componentes que deseas en las rutas. Por ejemplo, HomeComponent y AboutComponent.

```bash

ng generate component home
ng generate component about 
```

3. Uso del router outlet:

- En tu app.component.html, agregar el <router-outlet> donde deseas que se carguen los componentes de las rutas:

```html

<nav>
  <a routerLink="/home">Home</a>
  <a routerLink="/about">About</a>
</nav>
<router-outlet></router-outlet>
```

- *Navegación Programática*

- Para navegar programáticamente entre rutas, utilizar el router de Angular. Aquí hay un ejemplo de cómo hacerlo en un componente:

```typescript
import { Component } from '@angular/core';
import { Router } from '@angular/router';

@Component({
  selector: 'app-home',
  template: `<button (click)="goToAbout()">Go to About</button>`
})
export class HomeComponent {
  constructor(private router: Router) {}

  goToAbout() {
    this.router.navigate(['/about']);
  }
}
```

- *Parámetros de Ruta*

- Angular permite pasar parámetros a través de las rutas. Por ejemplo:

1. Definir una ruta con parámetros:

```typescript

const routes: Routes = [
  { path: 'user/:id', component: UserComponent },
];

```

2. Acceder a los parámetros en el componente:

- Utiliza ActivatedRoute para acceder a los parámetros:

```typescript

import { Component, OnInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router';

@Component({
  selector: 'app-user',
  template: `<p>User ID: {{ userId }}</p>`
})
export class UserComponent implements OnInit {
  userId!: string;

  constructor(private route: ActivatedRoute) {}

  ngOnInit() {
    this.userId = this.route.snapshot.paramMap.get('id')!;
  }
}

```

- *Rutas Hijas*

- Para crear rutas anidadas, puedes definir rutas hijas:

```typescript

const routes: Routes = [
  {
    path: 'products',
    component: ProductsComponent,
    children: [
      { path: 'details/:id', component: ProductDetailsComponent },
      { path: 'reviews', component: ProductReviewsComponent },
    ]
  }
];
```

- No olvides incluir un <router-outlet> en el ProductsComponent para renderizar las rutas hijas.

- *Protección de Rutas*

- Para proteger rutas que requieren autenticación, puedes usar guardias de rutas. Crear una guardia con el siguiente comando:

```bash

ng generate guard auth
```

- En el archivo de la guardia, implementar la lógica de protección:

```typescript

import { Injectable } from '@angular/core';
import { CanActivate, ActivatedRouteSnapshot, RouterStateSnapshot, Router } from '@angular/router';

@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate {
  constructor(private router: Router) {}

  canActivate(
    next: ActivatedRouteSnapshot,
    state: RouterStateSnapshot): boolean {
    const isAuthenticated = false; // Lógica de autenticación

    if (!isAuthenticated) {
      this.router.navigate(['/login']);
      return false;
    }
    return true;
  }
}
```

- Luego, añade la guardia a la ruta:

```typescript

{ path: 'protected', component: ProtectedComponent, canActivate: [AuthGuard] }

```
