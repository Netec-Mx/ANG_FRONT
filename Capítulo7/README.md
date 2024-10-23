# Práctica 7. Creación y uso de servicios en Angular

## Objetivos de la práctica
- Crear y utlizar servicios en Angular para gestionar la lógica de negocio y el acceso a datos de tu aplicación.

## Duración aproximada:
- 30 minutos.

## Instrucciones
1. Generar un servicio: Puedes crear un servicio usando Angular CLI:

```bash

ng generate service nombre-del-servicio

```

- Esto creará un archivo con un decorador @Injectable().

2. Definir el servicio: Un servicio típico puede verse así:

```typescript

import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root', // Hace que el servicio sea singleton en toda la aplicación
})
export class MiServicio {
  constructor() {}

  obtenerDatos() {
    return ['dato1', 'dato2', 'dato3'];
  }
}

```

3. Usar el servicio en un componente: Para usar un servicio, debes inyectarlo en el constructor de tu componente:

```typescript

import { Component } from '@angular/core';
import { MiServicio } from './mi-servicio.service';

@Component({
  selector: 'app-mi-componente',
  template: `
    <ul>
      <li *ngFor="let dato of datos">{{ dato }}</li>
    </ul>
  `,
})
export class MiComponente {
  datos: string[];

  constructor(private miServicio: MiServicio) {
    this.datos = this.miServicio.obtenerDatos();
  }
}

```

## Cómo crear un Data Service

1. Generar un Data Service: Usa Angular CLI para crear un servicio:\

```bash

ng generate service data
```

2. Definir el Data Service: Un Data Service típico podría verse así:

```typescript

import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root',
})
export class DataService {
  private apiUrl = 'https://api.example.com/datos';

  constructor(private http: HttpClient) {}

  obtenerDatos(): Observable<any[]> {
    return this.http.get<any[]>(this.apiUrl);
  }

  agregarDato(dato: any): Observable<any> {
    return this.http.post<any>(this.apiUrl, dato);
  }

  eliminarDato(id: number): Observable<any> {
    return this.http.delete<any>(`${this.apiUrl}/${id}`);
  }
}
```

3. Usar el Data Service en un Componente: Ahora puedes inyectar y utilizar el Data Service en tus componentes:

```typescript

import { Component, OnInit } from '@angular/core';
import { DataService } from './data.service';

@Component({
  selector: 'app-datos',
  template: `
    <ul>
      <li *ngFor="let dato of datos">{{ dato.name }}</li>
    </ul>
  `,
})
export class DatosComponent implements OnInit {
  datos: any[] = [];

  constructor(private dataService: DataService) {}

  ngOnInit() {
    this.dataService.obtenerDatos().subscribe((datos) => {
      this.datos = datos;
    });
  }
}
```


## Cómo inyectar un servicio en otro servicio

1. Crear el primer servicio: Supongamos que tienes un servicio que maneja operaciones de autenticación:

```typescript

import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root',
})
export class AuthService {
  isLoggedIn = false;

  login() {
    this.isLoggedIn = true;
  }

  logout() {
    this.isLoggedIn = false;
  }
}
```

2. Crear el segundo servicio: Ahora, vamos a crear un servicio que necesite acceder al (AuthService):

```typescript

import { Injectable } from '@angular/core';
import { AuthService } from './auth.service';

@Injectable({
  providedIn: 'root',
})
export class UserService {
  constructor(private authService: AuthService) {}

  getUserStatus() {
    return this.authService.isLoggedIn;
  }
}
```

3. Usar el servicio inyectado: Ahora puedes utilizar (UserService), que a su vez utiliza (AuthService) para obtener el estado del usuario:

```typescript

import { Component } from '@angular/core';
import { UserService } from './user.service';

@Component({
  selector: 'app-user-status',
  template: `
    <p>User is {{ userStatus ? 'logged in' : 'logged out' }}</p>
  `,
})
export class UserStatusComponent {
  userStatus: boolean;

  constructor(private userService: UserService) {
    this.userStatus = this.userService.getUserStatus();
  }
}
```

## Cómo implementar la comunicación a través de un servicio

1. Crear un servicio: Comienza creando un servicio que manejará los datos compartidos y la comunicación entre los componentes.

```typescript

import { Injectable } from '@angular/core';
import { BehaviorSubject } from 'rxjs';

@Injectable({
  providedIn: 'root',
})
export class DataService {
  private mensajeSource = new BehaviorSubject<string>('Mensaje inicial');
  currentMensaje = this.mensajeSource.asObservable();

  cambiarMensaje(mensaje: string) {
    this.mensajeSource.next(mensaje);
  }
}
```

2. Usar el servicio en el primer componente: En este componente, puedes cambiar el mensaje a través del servicio.

```typescript

import { Component } from '@angular/core';
import { DataService } from './data.service';

@Component({
  selector: 'app-componente1',
  template: `
    <input [(ngModel)]="nuevoMensaje" />
    <button (click)="enviarMensaje()">Enviar Mensaje</button>
  `,
})
export class Componente1 {
  nuevoMensaje: string;

  constructor(private dataService: DataService) {}

  enviarMensaje() {
    this.dataService.cambiarMensaje(this.nuevoMensaje);
  }
}
```

3. Usar el servicio en el segundo componente: Este componente escucha los cambios en el mensaje.

```typescript

import { Component, OnInit } from '@angular/core';
import { DataService } from './data.service';

@Component({
  selector: 'app-componente2',
  template: `<p>Mensaje recibido: {{ mensaje }}</p>`,
})
export class Componente2 implements OnInit {
  mensaje: string;

  constructor(private dataService: DataService) {}

  ngOnInit() {
    this.dataService.currentMensaje.subscribe((mensaje) => {
      this.mensaje = mensaje;
    });
  }
}
```
