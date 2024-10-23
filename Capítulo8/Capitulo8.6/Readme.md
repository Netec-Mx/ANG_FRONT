## 8.6. Child Routes en Angular

- *Estructura de Rutas Hijas*

- Al definir rutas hijas, puedes anidar rutas dentro de otras rutas. Esto es especialmente útil cuando tienes un componente que actúa como contenedor y necesitas cargar diferentes componentes en función de la navegación del usuario.


- *Ejemplo de Configuración de Rutas Hijas*

- Supongamos que tienes un módulo de Admin donde quieres incluir rutas para gestionar usuarios y productos. Aquí tienes cómo puedes definir estas rutas:

```typescript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { AdminComponent } from './admin/admin.component';
import { UserManagementComponent } from './admin/user-management/user-management.component';
import { ProductManagementComponent } from './admin/product-management/product-management.component';

const routes: Routes = [
  {
    path: 'admin',
    component: AdminComponent,
    children: [
      { path: 'users', component: UserManagementComponent },
      { path: 'products', component: ProductManagementComponent },
    ]
  }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

- *Uso del Router Outlet*

- Para que las rutas hijas se rendericen correctamente, debes usar un <router-outlet> dentro del componente contenedor (en este caso, AdminComponent).

- *Ejemplo de Componente Contenedor*

```html
<!-- admin.component.html -->
<h2>Admin Dashboard</h2>
<nav>
  <a routerLink="users">Manage Users</a>
  <a routerLink="products">Manage Products</a>
</nav>
<router-outlet></router-outlet>
```

- *Navegación a Rutas Hijas*

- Para navegar a rutas hijas, puedes utilizar el objeto Router. La navegación se realiza especificando la ruta del componente contenedor y luego la ruta hija.

- *Ejemplo de Navegación*

```typescript
import { Component } from '@angular/core';
import { Router } from '@angular/router';

@Component({
  selector: 'app-dashboard',
  template: `<button (click)="goToUsers()">Manage Users</button>`
})
export class DashboardComponent {
  constructor(private router: Router) {}

  goToUsers() {
    this.router.navigate(['admin', 'users']);
  }
}
```

- *Parámetros de Ruta en Rutas Hijas*
- Las rutas hijas también pueden tener parámetros. Por ejemplo, si deseas gestionar un usuario específico, puedes definir una ruta hija que incluya un parámetro de ID:

```typescript
{ path: 'users/:id', component: UserDetailComponent }
```

- Y navegar a esa ruta:

```typescript
this.router.navigate(['admin', 'users', userId]);
```

- *Protección de Rutas Hijas*

- Al igual que las rutas principales, puedes aplicar guardias a las rutas hijas para proteger el acceso a componentes específicos.

- *Ejemplo de Guardia*

- Si tienes una guardia que verifica si el usuario tiene permisos de administrador, puedes aplicarla a las rutas hijas:

```typescript
{
  path: 'admin',
  component: AdminComponent,
  canActivate: [AdminGuard],
  children: [
    { path: 'users', component: UserManagementComponent, canActivate: [UserGuard] },
    { path: 'products', component: ProductManagementComponent },
  ]
}
```



