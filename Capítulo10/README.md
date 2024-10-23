## Práctica 10. Creacion de un componente Login

## Objetivo de la práctica

- Crear un componente de inicio de sesión (Login) en Angular.
- Aprender a definir la estructura del componente, gestionar el formulario de autenticación, implementar la validación de datos y manejar la interacción con un servicio de autenticación.
- Entender cómo gestionar la entrada del usuario y autenticarlo de manera efectiva en tu aplicación.

## Duración aproximada
- 40 minutos.

## Instrucciones
- Crear un proyecto nuevo
- y crear un componente nuevo que lo llamaras login, usando el comando CLI para crear componentes

```bash
ng g component login
```

- Estructura de Carpetas

```lua

/src
|-- /app
    |-- /components
        |-- login
            |-- login.component.ts
            |-- login.component.html
            |-- login.component.css
```

1. Código del Componente de Login

- 1. app.module.ts
- Asegúrate de importar ReactiveFormsModule para usar formularios reactivos.

```typescript

import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { ReactiveFormsModule } from '@angular/forms';

import { AppComponent } from './app.component';
import { LoginComponent } from './components/login/login.component';

@NgModule({
  declarations: [
    AppComponent,
    LoginComponent
  ],
  imports: [
    BrowserModule,
    ReactiveFormsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

2. Codigo en el archivo login.component.ts

```typescript

import { Component } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';

@Component({
  selector: 'app-login',
  templateUrl: './login.component.html',
  styleUrls: ['./login.component.css']
})
export class LoginComponent {
  loginForm: FormGroup;

  constructor(private fb: FormBuilder) {
    this.loginForm = this.fb.group({
      email: ['', [Validators.required, Validators.email]],
      password: ['', [Validators.required, Validators.minLength(6)]]
    });
  }

  onSubmit() {
    if (this.loginForm.valid) {
      const formData = this.loginForm.value;
      console.log('Usuario autenticado:', formData);
      // Aquí puedes realizar la lógica de autenticación
    }
  }
}
```

3. Codigo en el archivo login.component.html
```html

<div class="login-container">
  <h2>Iniciar Sesión</h2>
  <form [formGroup]="loginForm" (ngSubmit)="onSubmit()">
    <div>
      <label for="email">Correo Electrónico</label>
      <input id="email" formControlName="email" />
      <div *ngIf="loginForm.get('email')?.invalid && loginForm.get('email')?.touched">
        <small *ngIf="loginForm.get('email')?.errors?.required">El correo es obligatorio.</small>
        <small *ngIf="loginForm.get('email')?.errors?.email">Formato de correo inválido.</small>
      </div>
    </div>
    
    <div>
      <label for="password">Contraseña</label>
      <input type="password" id="password" formControlName="password" />
      <div *ngIf="loginForm.get('password')?.invalid && loginForm.get('password')?.touched">
        <small *ngIf="loginForm.get('password')?.errors?.required">La contraseña es obligatoria.</small>
        <small *ngIf="loginForm.get('password')?.errors?.minlength">Mínimo 6 caracteres.</small>
      </div>
    </div>
    
    <button type="submit" [disabled]="loginForm.invalid">Iniciar Sesión</button>
  </form>
</div>
```

4. Codigo en el archivo login.component.css

```css

.login-container {
  max-width: 400px;
  margin: auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

label {
  display: block;
  margin-bottom: 5px;
}

input {
  width: 100%;
  padding: 10px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  width: 100%;
  padding: 10px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:disabled {
  background-color: #ccc;
}
```
## Firebase para Autenticación

1. Crear un Proyecto en Firebase

    1. Ir a Firebase Console:

    - Visitar Firebase Console.
    - Iniciar sesión con tu cuenta de Google.

    2. Crear un nuevo proyecto:

    - Hacer clic en "Agregar proyecto" y seguir las instrucciones.
    - Puedes optar por habilitar Google Analytics o dejarlo desactivado según tus preferencias.

2. Registrar tu Aplicación Web

    1. Añadir una Aplicación Web:

    - Una vez que tu proyecto esté creado, en el panel de control, hacer clic en el icono de la web (</>).
    - Introducir un nombre para tu aplicación y hacer clic en "Registrar aplicación".

    2. Obtener Configuración de Firebase:

    - Firebase te proporcionará un bloque de código con la configuración de Firebase. Lo necesitarás más adelante.

3. Instalar Firebase y AngularFire

- Abrir tu terminal en el directorio de tu proyecto Angular y ejecutar:

```bash

npm install firebase @angular/fire
```

4. Configurar Firebase en Angular

    1. Importar los Módulos Necesarios

    - Abre src/app/app.module.ts y configura AngularFire:

```typescript

import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { ReactiveFormsModule } from '@angular/forms';
import { AngularFireModule } from '@angular/fire';
import { AngularFireAuthModule } from '@angular/fire/auth';
import { environment } from '../environments/environment';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent,
    // Otros componentes...
  ],
  imports: [
    BrowserModule,
    ReactiveFormsModule,
    AngularFireModule.initializeApp(environment.firebase), // Inicializa Firebase
    AngularFireAuthModule // Importa el módulo de autenticación
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

   2. Agregar la Configuración en environment.ts
   - Abrir src/environments/environment.ts y añadir tu configuración de Firebase:

```typescript

export const environment = {
  production: false,
  firebase: {
    apiKey: "TU_API_KEY",
    authDomain: "TU_AUTH_DOMAIN",
    projectId: "TU_PROJECT_ID",
    storageBucket: "TU_STORAGE_BUCKET",
    messagingSenderId: "TU_MESSAGING_SENDER_ID",
    appId: "TU_APP_ID"
  }
};

```
  - Reemplazar los valores con los que obtuviste de la configuración de Firebase

5. Habilitar Métodos de Autenticación
    1. Ir a la sección de Autenticación:

    - En la consola de Firebase, seleccionar "Authentication" en el menú de la izquierda.

    2. Configurar Métodos de inicio de sesión:

    - Hacer clic en "Método de inicio de sesión".
    - Habilitar los métodos que quieras usar (por ejemplo, "Correo electrónico/contraseña", "Google", etc.) y guardar los cambios.

6. Crear un Servicio de Autenticación
    
    - Crear un nuevo servicio para manejar la autenticación.

    1.  Crear el Servicio
    
    - Ejecutar en la terminal:

````bash

ng generate service auth

````

   2.  Implementar el Servicio
    -  Abrir src/app/auth.service.ts y añadir el siguiente código:

```typescript

import { Injectable } from '@angular/core';
import { AngularFireAuth } from '@angular/fire/auth';
import firebase from 'firebase/compat/app';

@Injectable({
  providedIn: 'root'
})
export class AuthService {
  constructor(private afAuth: AngularFireAuth) {}

  // Método para registrar un usuario
  async register(email: string, password: string) {
    return await this.afAuth.createUserWithEmailAndPassword(email, password);
  }

  // Método para iniciar sesión
  async login(email: string, password: string) {
    return await this.afAuth.signInWithEmailAndPassword(email, password);
  }

  // Método para cerrar sesión
  async logout() {
    return await this.afAuth.signOut();
  }
}
```

7. Usar el Servicio en un Componente
    
    - Ahora puedes usar el servicio de autenticación en tu componente de inicio de sesión.

    1. Ejemplo de Componente de Login
    
    - A continuación, te muestro cómo podrías integrar el servicio en tu componente de inicio de sesión:

```typescript

import { Component } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';
import { AuthService } from '../auth.service';

@Component({
  selector: 'app-login',
  templateUrl: './login.component.html',
  styleUrls: ['./login.component.css']
})
export class LoginComponent {
  loginForm: FormGroup;

  constructor(private fb: FormBuilder, private authService: AuthService) {
    this.loginForm = this.fb.group({
      email: ['', [Validators.required, Validators.email]],
      password: ['', [Validators.required, Validators.minLength(6)]]
    });
  }

  async onSubmit() {
    if (this.loginForm.valid) {
      const { email, password } = this.loginForm.value;
      try {
        await this.authService.login(email, password);
        console.log('Inicio de sesión exitoso');
      } catch (error) {
        console.error('Error al iniciar sesión:', error);
      }
    }
  }
}
```


## Obtención del Token al hacer Login.

- Modificación del Servicio de Autenticación
- Primero, vamos a modificar el servicio de autenticación que creamos anteriormente para que devuelva el token después de iniciar sesión.

1. Actualizar auth.service.ts
- Aquí está el código actualizado para incluir la obtención del token:

```typescript

import { Injectable } from '@angular/core';
import { AngularFireAuth } from '@angular/fire/auth';
import firebase from 'firebase/compat/app';

@Injectable({
  providedIn: 'root'
})
export class AuthService {
  constructor(private afAuth: AngularFireAuth) {}

  // Método para registrar un usuario
  async register(email: string, password: string) {
    return await this.afAuth.createUserWithEmailAndPassword(email, password);
  }

  // Método para iniciar sesión
  async login(email: string, password: string): Promise<string | null> {
    const userCredential = await this.afAuth.signInWithEmailAndPassword(email, password);
    const token = await userCredential.user?.getIdToken(); // Obtener el token
    return token; // Devolver el token
  }

  // Método para cerrar sesión
  async logout() {
    return await this.afAuth.signOut();
  }
}

```

2. Uso del Token en el Componente de Login

- Ahora puedes usar el token en tu componente de inicio de sesión después de que el usuario se haya autenticado.

    1. Actualizar login.component.ts
    - Aquí tienes un ejemplo de cómo obtener y usar el token:

```typescript

import { Component } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';
import { AuthService } from '../auth.service';

@Component({
  selector: 'app-login',
  templateUrl: './login.component.html',
  styleUrls: ['./login.component.css']
})
export class LoginComponent {
  loginForm: FormGroup;

  constructor(private fb: FormBuilder, private authService: AuthService) {
    this.loginForm = this.fb.group({
      email: ['', [Validators.required, Validators.email]],
      password: ['', [Validators.required, Validators.minLength(6)]]
    });
  }

  async onSubmit() {
    if (this.loginForm.valid) {
      const { email, password } = this.loginForm.value;
      try {
        const token = await this.authService.login(email, password);
        console.log('Inicio de sesión exitoso. Token:', token);
        // Aquí puedes almacenar el token en el almacenamiento local o en un servicio
        localStorage.setItem('token', token || ''); // Almacenar el token
      } catch (error) {
        console.error('Error al iniciar sesión:', error);
      }
    }
  }
}
```

3. Uso del Token para Autenticación
- Puedes utilizar el token almacenado para autenticar solicitudes a tu backend. Cuando realices una llamada a una API, puedes incluir el token en los encabezados de la solicitud:

```typescript

const token = localStorage.getItem('token');
const headers = new HttpHeaders({
  Authorization: `Bearer ${token}`
});

// Realiza la llamada a la API con los headers
this.httpClient.get('https://api.example.com/secure-data', { headers }).subscribe(data => {
  console.log('Datos seguros:', data);
});

```

## Obtención del Token de Autenticación en Firebase

1. Configuración Inicial:

- Asegúrate de tener tu proyecto de Angular configurado con Firebase y de haber habilitado los métodos de autenticación deseados (como correo electrónico/contraseña).

2. Servicio de Autenticación:

- Crear un servicio que gestione la autenticación y la obtención del token. Este servicio utilizará la biblioteca de AngularFire para interactuar con Firebase.

- Ejemplo de Servicio de Autenticación

```typescript

import { Injectable } from '@angular/core';
import { AngularFireAuth } from '@angular/fire/auth';

@Injectable({
  providedIn: 'root'
})
export class AuthService {
  constructor(private afAuth: AngularFireAuth) {}

  // Método para iniciar sesión y obtener el token
  async login(email: string, password: string): Promise<string | null> {
    const userCredential = await this.afAuth.signInWithEmailAndPassword(email, password);
    const token = await userCredential.user?.getIdToken(); // Obtener el token
    return token; // Devolver el token
  }
}

```

3. Implementación en el Componente de Login
- En tu componente de inicio de sesión, utiliza el servicio para iniciar sesión y obtener el token.

- Ejemplo de Componente de Login

```typescript

import { Component } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';
import { AuthService } from '../auth.service';

@Component({
  selector: 'app-login',
  templateUrl: './login.component.html',
  styleUrls: ['./login.component.css']
})
export class LoginComponent {
  loginForm: FormGroup;

  constructor(private fb: FormBuilder, private authService: AuthService) {
    this.loginForm = this.fb.group({
      email: ['', [Validators.required, Validators.email]],
      password: ['', [Validators.required, Validators.minLength(6)]]
    });
  }

  async onSubmit() {
    if (this.loginForm.valid) {
      const { email, password } = this.loginForm.value;
      try {
        const token = await this.authService.login(email, password);
        console.log('Inicio de sesión exitoso. Token:', token);
        localStorage.setItem('token', token || ''); // Almacenar el token
      } catch (error) {
        console.error('Error al iniciar sesión:', error);
      }
    }
  }
}
```

4. Almacenamiento y Uso del Token
- Una vez que has obtenido el token, puedes almacenarlo en el almacenamiento local del navegador para usarlo en solicitudes futuras.

    1. Almacenamiento del Token

```typescript

localStorage.setItem('token', token || '');
```

   2. Uso del Token para Autenticación de API
    - Cuando necesites realizar solicitudes a un backend, incluye el token en los encabezados de la solicitud:


```typescript

const token = localStorage.getItem('token');
const headers = new HttpHeaders({
  Authorization: `Bearer ${token}`
});

// Realiza la llamada a la API
this.httpClient.get('https://api.example.com/secure-data', { headers }).subscribe(data => {
  console.log('Datos seguros:', data);
});

```


## Enviando Token en cada petición HTTP

1. Crear un Interceptor

    - Primero, crea un interceptor que se encargue de agregar el token a los encabezados de las solicitudes.

    1. Generar el Interceptor
    - Ejecuta el siguiente comando en la terminal:

```bash

ng generate interceptor auth
```

   2. Implementar el Interceptor
    - Abre el archivo auth.interceptor.ts que se ha creado y añade el siguiente código:

```typescript

import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpRequest, HttpHandler, HttpEvent } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable()
export class AuthInterceptor implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    const token = localStorage.getItem('token');

    if (token) {
      const cloned = req.clone({
        setHeaders: {
          Authorization: `Bearer ${token}`
        }
      });
      return next.handle(cloned);
    }
    
    return next.handle(req);
  }
}
```

2. Registrar el Interceptor
    - Ahora, debes registrar el interceptor en tu módulo principal para que Angular lo utilice en todas las solicitudes HTTP.

    1. Actualizar app.module.ts
    - Abre app.module.ts y añade el interceptor en el array de providers:

```typescript

import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { ReactiveFormsModule } from '@angular/forms';
import { HttpClientModule, HTTP_INTERCEPTORS } from '@angular/common/http';
import { AngularFireModule } from '@angular/fire';
import { AngularFireAuthModule } from '@angular/fire/auth';
import { environment } from '../environments/environment';

import { AppComponent } from './app.component';
import { AuthInterceptor } from './auth.interceptor';

@NgModule({
  declarations: [
    AppComponent,
    // Otros componentes...
  ],
  imports: [
    BrowserModule,
    ReactiveFormsModule,
    HttpClientModule, // Asegúrate de importar el módulo HTTP
    AngularFireModule.initializeApp(environment.firebase),
    AngularFireAuthModule
  ],
  providers: [
    { provide: HTTP_INTERCEPTORS, useClass: AuthInterceptor, multi: true }
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

3. Realizar Peticiones HTTP
- Ahora, cada vez que realices una solicitud HTTP en tu aplicación, el interceptor añadirá automáticamente el token de autorización.

    1. Ejemplo de Servicio que Hace Solicitudes HTTP
    - Aquí tienes un ejemplo de cómo podrías hacer una solicitud HTTP utilizando el servicio HttpClient:

```typescript

import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class ApiService {
  private apiUrl = 'https://api.example.com';

  constructor(private http: HttpClient) {}

  getSecureData(): Observable<any> {
    return this.http.get(`${this.apiUrl}/secure-data`);
  }
}
```

4. Uso del Servicio en un Componente
- Puedes utilizar este servicio en un componente para obtener datos seguros:

```typescript

import { Component, OnInit } from '@angular/core';
import { ApiService } from '../api.service';

@Component({
  selector: 'app-secure-data',
  templateUrl: './secure-data.component.html',
  styleUrls: ['./secure-data.component.css']
})
export class SecureDataComponent implements OnInit {
  data: any;

  constructor(private apiService: ApiService) {}

  ngOnInit() {
    this.apiService.getSecureData().subscribe(
      response => {
        this.data = response;
        console.log('Datos seguros:', this.data);
      },
      error => {
        console.error('Error al obtener datos seguros:', error);
      }
    );
  }
}
```

## Componente de Login y Logout en Angular

1. Paso 1: Crear el Componente de Login
- Primero, generemos el componente de Login.

    1. Generar el Componente

    - Ejecutar el siguiente comando en la terminal:

```bash

ng generate component login
```

   2. Implementar la Lógica del Componente
    
    - Abre login.component.ts y añade la lógica para manejar el formulario de inicio de sesión.

```typescript

import { Component } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';
import { AuthService } from '../auth.service';

@Component({
  selector: 'app-login',
  templateUrl: './login.component.html',
  styleUrls: ['./login.component.css']
})
export class LoginComponent {
  loginForm: FormGroup;
  errorMessage: string | null = null;

  constructor(private fb: FormBuilder, private authService: AuthService) {
    this.loginForm = this.fb.group({
      email: ['', [Validators.required, Validators.email]],
      password: ['', [Validators.required, Validators.minLength(6)]]
    });
  }

  async onSubmit() {
    if (this.loginForm.valid) {
      const { email, password } = this.loginForm.value;
      try {
        const token = await this.authService.login(email, password);
        console.log('Inicio de sesión exitoso. Token:', token);
        // Redirige o realiza acciones adicionales después del inicio de sesión
      } catch (error) {
        this.errorMessage = 'Error al iniciar sesión. Intente de nuevo.';
        console.error('Error al iniciar sesión:', error);
      }
    }
  }
}

```

2. Paso 2: Crear el HTML del Componente de Login
- Ahora, crear la interfaz del componente en login.component.html:

```html

<div class="login-container">
  <h2>Iniciar Sesión</h2>
  <form [formGroup]="loginForm" (ngSubmit)="onSubmit()">
    <div>
      <label for="email">Correo Electrónico</label>
      <input id="email" formControlName="email" />
      <div *ngIf="loginForm.get('email')?.invalid && loginForm.get('email')?.touched">
        <small>El correo electrónico es inválido.</small>
      </div>
    </div>

    <div>
      <label for="password">Contraseña</label>
      <input type="password" id="password" formControlName="password" />
      <div *ngIf="loginForm.get('password')?.invalid && loginForm.get('password')?.touched">
        <small>La contraseña es requerida.</small>
      </div>
    </div>

    <button type="submit" [disabled]="loginForm.invalid">Iniciar Sesión</button>
    <div *ngIf="errorMessage">{{ errorMessage }}</div>
  </form>
</div>
```

3. Paso 3: Implementar el Servicio de Autenticación
- Asegúrate de tener un servicio de autenticación que maneje el login y logout. Aquí te dejo un ejemplo simple.

- archivo auth.service.ts

```typescript

import { Injectable } from '@angular/core';
import { AngularFireAuth } from '@angular/fire/auth';

@Injectable({
  providedIn: 'root'
})
export class AuthService {
  constructor(private afAuth: AngularFireAuth) {}

  async login(email: string, password: string): Promise<string | null> {
    const userCredential = await this.afAuth.signInWithEmailAndPassword(email, password);
    const token = await userCredential.user?.getIdToken();
    return token;
  }

  async logout(): Promise<void> {
    await this.afAuth.signOut();
  }
}
```

4. Paso 4: Crear el Componente de Logout
- Ahora, vamos a crear un componente para manejar el cierre de sesión.

    1. Generar el Componente de Logout
    -  Ejecuta el siguiente comando:

```bash

ng generate component logout

```

   2. Implementar la Lógica del Componente de Logout
    - Abre logout.component.ts y añade la lógica para cerrar sesión:

```typescript

import { Component } from '@angular/core';
import { AuthService } from '../auth.service';
import { Router } from '@angular/router';

@Component({
  selector: 'app-logout',
  template: `<button (click)="onLogout()">Cerrar Sesión</button>`
})
export class LogoutComponent {
  constructor(private authService: AuthService, private router: Router) {}

  async onLogout() {
    await this.authService.logout();
    localStorage.removeItem('token'); // Elimina el token almacenado
    this.router.navigate(['/login']); // Redirige al usuario a la página de login
  }
}
```

5. Paso 5: Integrar los Componentes en la Aplicación
- Asegúrate de que los componentes de Login y Logout estén integrados en tu aplicación. Por ejemplo, puedes agregar las rutas en app-routing.module.ts:

```typescript

import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { LoginComponent } from './login/login.component';
import { LogoutComponent } from './logout/logout.component';

const routes: Routes = [
  { path: 'login', component: LoginComponent },
  { path: 'logout', component: LogoutComponent },
  { path: '', redirectTo: '/login', pathMatch: 'full' }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }

```
## Uso de Guardianes en Angular

1. Paso 1: Crear un Guardián
- Para ilustrar cómo utilizar un guardián, vamos a crear un AuthGuard que proteja rutas que requieren autenticación.

    1. Generar el Guardián
    - Ejecutar el siguiente comando en la terminal:

```bash
Copiar código
ng generate guard auth
```

   - Esto generará un nuevo guardián llamado AuthGuard.

    2. Implementar la Lógica del Guardián
    - Abrir auth.guard.ts y añadir la lógica para verificar si el usuario está autenticado:

```typescript
Copiar código
import { Injectable } from '@angular/core';
import { CanActivate, ActivatedRouteSnapshot, RouterStateSnapshot, Router } from '@angular/router';
import { AuthService } from './auth.service'; // Asegúrate de que la ruta sea correcta

@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate {
  constructor(private authService: AuthService, private router: Router) {}

  canActivate(
    next: ActivatedRouteSnapshot,
    state: RouterStateSnapshot): boolean {
    const isAuthenticated = !!localStorage.getItem('token'); // Verifica si hay un token

    if (!isAuthenticated) {
      this.router.navigate(['/login']); // Redirige a login si no está autenticado
      return false; // Bloquea el acceso
    }

    return true; // Permite el acceso
  }
}
```


2. Paso 2: Proteger Rutas con el Guardián

- Ahora que has creado el guardián, debes aplicarlo a las rutas que deseas proteger.

    1. Actualizar app-routing.module.ts
    - Abrir app-routing.module.ts y aplicar el AuthGuard a las rutas necesarias:

```typescript
Copiar código
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { LoginComponent } from './login/login.component';
import { SecureComponent } from './secure/secure.component'; // Un componente que deseas proteger
import { AuthGuard } from './auth.guard'; // Asegúrate de que la ruta sea correcta

const routes: Routes = [
  { path: 'login', component: LoginComponent },
  { path: 'secure', component: SecureComponent, canActivate: [AuthGuard] }, // Ruta protegida
  { path: '', redirectTo: '/login', pathMatch: 'full' }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }

```

3. Paso 3: Probar el Guardián
- Ahora puedes probar la funcionalidad del guardián:

- Intenta acceder a la ruta protegida /secure sin iniciar sesión. Deberías ser redirigido a la página de login.
- Iniciar sesión y acceder nuevamente a la ruta /secure. Deberías poder acceder sin problemas.
