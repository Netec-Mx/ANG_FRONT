## Caso Práctico: Routing en Angular

- *Escenario*

- Imaginemos que estamos desarrollando una aplicación para una tienda en línea. La aplicación tendrá las siguientes características:

1. Una página de inicio que muestra productos destacados.

2. Una página de productos donde los usuarios pueden ver todos los productos disponibles.

3. Una página de detalles de producto que muestra información específica sobre un producto seleccionado.

4. Una página de contacto.

- *Estructura del Proyecto*

- Antes de comenzar, la estructura del proyecto podría ser la siguiente:

```lua

src/
|-- app/
|   |-- components/
|   |   |-- home/
|   |   |-- products/
|   |   |-- product-detail/
|   |   |-- contact/
|   |-- app-routing.module.ts
|   |-- app.component.ts
|   |-- app.module.ts
|-- assets/
|-- environments/

```

1. *Paso 1: Crear los Componentes*

- Primero, generaremos los componentes necesarios utilizando Angular CLI:

```bash

ng generate component components/home
ng generate component components/products
ng generate component components/product-detail
ng generate component components/contact
```

2. Paso 2: Configurar el AppRoutingModule

- En el archivo app-routing.module.ts, configuraremos las rutas de la aplicación.

```typescript

import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { HomeComponent } from './components/home/home.component';
import { ProductsComponent } from './components/products/products.component';
import { ProductDetailComponent } from './components/product-detail/product-detail.component';
import { ContactComponent } from './components/contact/contact.component';

const routes: Routes = [
  { path: '', redirectTo: '/home', pathMatch: 'full' },
  { path: 'home', component: HomeComponent },
  { path: 'products', component: ProductsComponent },
  { path: 'product/:id', component: ProductDetailComponent },
  { path: 'contact', component: ContactComponent },
  { path: '**', redirectTo: '/home' } // Ruta no encontrada
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

- *Detalles de las Rutas*

- Ruta Vacía (''): Redirige a la página de inicio (/home).

- Ruta de Productos (/products): Muestra todos los productos.

- Ruta de Detalle de Producto (/product/:id): Muestra los detalles de un producto específico, donde :id es un parámetro dinámico.

- Ruta de Contacto (/contact): Muestra la página de contacto.

- Ruta No Encontrada ('**'): Redirige a la página de inicio en caso de que la ruta no sea válida.

3. Paso 3: Implementar el Router Outlet

- En el archivo app.component.html, añadiremos un <router-outlet> para que Angular sepa dónde renderizar los componentes según la ruta activa.

```html

<nav>
  <a routerLink="/home">Home</a>
  <a routerLink="/products">Products</a>
  <a routerLink="/contact">Contact</a>
</nav>
<router-outlet></router-outlet>
```

4. Paso 4: Acceso a Parámetros de Ruta

- En el componente ProductDetailComponent, accedemos al parámetro de ruta id para mostrar los detalles del producto.

```typescript

import { Component, OnInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router';

@Component({
  selector: 'app-product-detail',
  template: `
    <h2>Detalles del Producto</h2>
    <p>ID del Producto: {{ productId }}</p>
  `
})
export class ProductDetailComponent implements OnInit {
  productId!: string;

  constructor(private route: ActivatedRoute) {}

  ngOnInit() {
    this.route.paramMap.subscribe(params => {
      this.productId = params.get('id')!;
    });
  }
}
```

5. Paso 5: Navegación entre Componentes

- En el componente ProductsComponent, podemos añadir enlaces para navegar a los detalles de cada producto. Para este ejemplo, consideraremos un producto con ID 1.

```html

<h2>Lista de Productos</h2>
<ul>
  <li><a [routerLink]="['/product', 1]">Producto 1</a></li>
  <li><a [routerLink]="['/product', 2]">Producto 2</a></li>
</ul>
```

6. Paso 6: Estilo y Diseño

- Agrega estilos en styles.css o en los estilos de cada componente para mejorar la presentación visual de la aplicación.

