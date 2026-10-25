
# PARÁMETROS EN LAS RUTAS Y NAVEGACIÓN EN ANGULAR

## 1. Pasar parámetros por las rutas

Podemos pasar parámetros a un componente a través de la URL.

Para ello, definimos la ruta indicando el parámetro con `:`.

Ejemplo:

```typescript
{ path: "doble/:numero", component: NumeroDobleComponent }
```

En este caso, `numero` es el parámetro que recibiremos desde la URL.

## 2. Recibir parámetros con ActivatedRoute

Para recoger los parámetros de una ruta, utilizamos `ActivatedRoute` y `Params`.

Los parámetros se reciben en el método `ngOnInit()` mediante una suscripción.

```typescript
import { ActivatedRoute, Params } from '@angular/router';

@Component(...)
export class NumeroDobleComponent implements OnInit {

    public doble!: number;
    public numero!: number;

    constructor(private _activeRoute: ActivatedRoute) {
        this.doble = 0;
    }

    ngOnInit(): void {
        this._activeRoute.params.subscribe((parametros: Params) => {
            if (parametros['numero'] != null) {
                this.numero = parseInt(parametros['numero']);
                this.doble = this.numero * 2;
            }
        });
    }
}
```

En este ejemplo:

- `ActivatedRoute` permite acceder a los parámetros de la ruta.
- `params.subscribe()` permite recibir los parámetros.
- `parametros['numero']` obtiene el valor del parámetro `numero`.
- `parseInt()` convierte el valor a un número entero.
- `this.doble` almacena el doble del número recibido.

## 3. Navegar a una ruta con parámetros mediante RouterLink

Podemos pasar un parámetro desde un enlace utilizando `routerLink`.

```html
<li>
    <a [routerLink]="['/doble/7']">Doble con params</a>
</li>
```

En este ejemplo, navegamos a la ruta `doble` pasando el valor `7`.

## 4. Navegar mediante Router

También podemos navegar a otras rutas desde TypeScript utilizando `Router`.

Primero, importamos `Router`:

```typescript
import { ActivatedRoute, Params, Router } from '@angular/router';
```

Después, lo inyectamos en el constructor:

```typescript
constructor(
    private _activeRoute: ActivatedRoute,
    private _router: Router
) {
    this.doble = 0;
}
```

### 4.1. Volver a la página principal

Creamos un método que navega a la ruta principal:

```typescript
goHome(): void {
    this._router.navigate(["/"]);
}
```

Podemos llamar al método desde HTML:

```html
<button (click)="goHome()"></button>
```

### 4.2. Navegar pasando un parámetro

Podemos crear un método que reciba un número y navegue a la ruta `doble` pasando ese valor.

```typescript
goDoble(num: number): void {
    this._router.navigate(["/doble", num]);
}
```

Desde HTML, llamamos al método indicando el número:

```html
<button (click)="goDoble(99)">go</button>
```

## 5. Crear enlaces dinámicos con @for

Podemos generar varios enlaces mediante un bucle `@for` y pasar diferentes parámetros en cada uno.

```html
@for (item of [].constructor(3); track $index) {
    <a [routerLink]="['/doble', $index + 5]">
        Doble {{$index + 5}}
    </a>
}
```

En este ejemplo, se generan enlaces con los números 5, 6 y 7.

## 6. Utilizar ChangeDetectorRef

En los apuntes se utiliza `ChangeDetectorRef` para actualizar la vista cuando se reciben parámetros de la ruta.

Importamos `ChangeDetectorRef`:

```typescript
import {
    Component,
    OnInit,
    ChangeDetectorRef
} from '@angular/core';
```

Lo inyectamos en el constructor:

```typescript
constructor(
    private _activeRoute: ActivatedRoute,
    private _router: Router,
    private _cdr: ChangeDetectorRef
) {}
```

Dentro de `ngOnInit()`, utilizamos `detectChanges()` después de actualizar las variables:

```typescript
ngOnInit(): void {
    this._activeRoute.params.subscribe((parametros: Params) => {
        if (parametros['numero'] != null) {
            this.numero = parseInt(parametros['numero']);
            this.doble = this.numero * 2;
            this._cdr.detectChanges();
        }
    });
}
```

El método `detectChanges()` se utiliza en este ejemplo para detectar los cambios y actualizar la vista.
