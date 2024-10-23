# Reorganización de Componentes en Angular

## Objetivo de la práctica

-

## Duración aproximada
- minutos

## Instrucciones

- *Estructura de Proyecto Angular*

- Una buena estructura de carpetas facilita la navegación y el mantenimiento del proyecto. A continuación, se presenta una estructura de carpetas recomendada:

```lua

src/
|-- app/
|   |-- components/
|   |   |-- header/
|   |   |   |-- header.component.ts
|   |   |   |-- header.component.html
|   |   |   |-- header.component.css
|   |   |
|   |   |-- footer/
|   |   |-- sidebar/
|   |   |
|   |-- pages/
|   |   |-- home/
|   |   |-- about/
|   |   |
|   |-- services/
|   |-- models/
|   |-- app.component.ts
|   |-- app.module.ts
|-- assets/
|-- environments/
```

- *Componentes*

1. Componentes Reutilizables

- Los componentes que se utilizan en múltiples lugares, como encabezados y pies de página, deben estar en una carpeta específica de componentes.

- *Ejemplo*: HeaderComponent

```typescript

import { Component } from '@angular/core';

@Component({
  selector: 'app-header',
  templateUrl: './header.component.html',
  styleUrls: ['./header.component.css']
})
export class HeaderComponent {}
```

- Uso en otro componente:

```html

<app-header></app-header>
<router-outlet></router-outlet>
<app-footer></app-footer>
```

2. Componentes de Página
- Los componentes que representan páginas específicas deben agruparse en una carpeta llamada pages. Esto ayuda a distinguir entre componentes de interfaz de usuario y componentes de estructura.

*Ejemplo*: HomeComponent

```typescript

import { Component } from '@angular/core';

@Component({
  selector: 'app-home',
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.css']
})
export class HomeComponent {}
```

- *Uso de Módulos*

- Angular permite la creación de módulos, lo que mejora la organización del código y la carga perezosa. Agrupa componentes relacionados en un módulo específico.

- Ejemplo: Módulo de Productos

```typescript

import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { ProductListComponent } from './product-list/product-list.component';
import { ProductDetailComponent } from './product-detail/product-detail.component';

@NgModule({
  declarations: [
    ProductListComponent,
    ProductDetailComponent
  ],
  imports: [
    CommonModule
  ],
  exports: [
    ProductListComponent,
    ProductDetailComponent
  ]
})
export class ProductsModule {}
