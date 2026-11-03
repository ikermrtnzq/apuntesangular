# 23 - Interceptors

## ¿Qué son los interceptores?

Los interceptores permiten intervenir en las peticiones HTTP para realizar acciones antes o después de que se procesen.

- No pueden modificar directamente una petición, pero sí clonarla y sustituirla.
- No son selectivos por defecto: se activan para todas las peticiones.
- Se declaran dentro de `Providers`.

## Instalación y creación

Instalar el paquete indicado en los apuntes:

```bash
npm.cmd install @angular/animations@^21
npm.cmd install ngx-toastr
```

Crear un interceptor:

```bash
ng g interceptor interceptors/error
```

Después debemos indicar en la configuración de la aplicación que las peticiones pasarán por los interceptores.

## Interceptor para controlar errores

En este caso, cuando se produce un error, mostramos un mensaje y redirigimos a un componente concreto.

Para realizar acciones utilizamos `.pipe()` y `catchError()`:

```typescript
import {
  HttpErrorResponse,
  HttpInterceptorFn
} from '@angular/common/http';

import { inject, Injector } from '@angular/core';
import { catchError, throwError } from 'rxjs';
import { ToastrService } from 'ngx-toastr';
import { Router } from '@angular/router';

export const errorInterceptor: HttpInterceptorFn = (req, next) => {

  const _router = inject(Router);
  const injector = inject(Injector);

  return next(req).pipe(

    catchError((error: HttpErrorResponse) => {

      const toastService = injector.get(ToastrService);

      toastService.error(error.message, 'Error');

      _router.navigate(['/error']);

      console.log("tipo de error: " + error.status);
      console.log("mensaje: " + error.message);

      return throwError(() => error);

    })

  );
};
```

## Interceptor para añadir un TOKEN

También podemos utilizar interceptores para las peticiones que necesitan un token de autenticación.

El token se recupera de `sessionStorage` y se añade a la cabecera `Authorization`:

```typescript
import { HttpInterceptorFn } from '@angular/common/http';

export const authTokenInterceptor: HttpInterceptorFn = (req, next) => {

  const token = sessionStorage.getItem("TOKEN");

  if (token) {
    let peticion = req.clone({
      setHeaders: {
        Authorization: "bearer " + token
      }
    });

    return next(peticion);
  }

  return next(req);
};
```

## Controlar qué peticiones necesitan TOKEN

Para indicar si una petición debe saltarse el interceptor del token utilizamos `HttpContext`.

Creamos un contexto llamado `SKIP_TOKEN`:

```typescript
import { HttpContextToken } from "@angular/common/http";

export const SKIP_TOKEN = new HttpContextToken<boolean>(() => false);
```

En el interceptor de autenticación comprobamos si la petición tiene ese contexto:

```typescript
import { HttpInterceptorFn } from '@angular/common/http';
import { SKIP_TOKEN } from '../utils/token.context';

export const authTokenInterceptor: HttpInterceptorFn = (req, next) => {

  if (req.context.get(SKIP_TOKEN)) {
    return next(req);
  }

  const token = sessionStorage.getItem("TOKEN");

  if (token) {
    let peticion = req.clone({
      setHeaders: {
        Authorization: "bearer " + token
      }
    });

    return next(peticion);
  }

  return next(req);
};
```

Cuando una petición no necesita token, le pasamos `SKIP_TOKEN` con valor `true`:

```typescript
async getEquipos(): Promise<Equipo[]> {
  let request = "api/equipos";
  let url = environment.urlEquipos + request;

  return firstValueFrom(
    this._http.get<Equipo[]>(url, {
      context: new HttpContext().set(SKIP_TOKEN, true)
    })
  );
}
```

## Utilizar varios interceptores

Para que coexistan varios interceptores, los indicamos dentro de `withInterceptors()`:

```typescript
provideHttpClient(
  withInterceptors([
    authTokenInterceptor,
    errorInterceptor
  ])
);
```
