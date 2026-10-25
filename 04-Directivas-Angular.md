
# DIRECTIVAS EN ANGULAR

## 1. ¿Qué son las directivas?

Las directivas permiten trabajar con el HTML y utilizar código para controlar qué se muestra, recorrer arrays y modificar los estilos.

## 2. Directiva *ngIf

La directiva `*ngIf` permite mostrar contenido dependiendo de una condición.

Podemos utilizar `else` para mostrar otro contenido cuando la condición no se cumple.

```html
<div *ngIf="sports.length < 5; else condicionelse">
    <p style="color: red;">Hay menos de 5 deportes</p>
</div>

<ng-template #condicionelse>
    <p style="color: green;">Hay más de 5 deportes</p>
</ng-template>
```

En este ejemplo, si `sports.length` es menor que 5, se muestra el primer mensaje. En caso contrario, se muestra el contenido de `condicionelse`.

## 3. Directiva *ngFor

La directiva `*ngFor` permite recorrer los elementos de un array y mostrarlos en HTML.

```html
<ul>
    <li *ngFor="let deporte of sports">
        {{ deporte }}
    </li>
</ul>
```

En este ejemplo, recorremos el array `sports` y mostramos cada deporte.

## 4. Utilizar ng-container y ng-template

Para secciones de código largas, podemos utilizar `ng-container` junto con `ng-template`.

```html
<ng-container *ngTemplateOutlet="plantilla"></ng-container>

<ng-template #plantilla>
    <p style="color: green;">MUCHO CODIGO.....</p>
</ng-template>
```

- `ng-container` permite incluir el contenido de una plantilla.
- `ng-template` define la plantilla que queremos utilizar.

## 5. Sintaxis moderna: @if y @else

También podemos utilizar `@if` y `@else` para trabajar con condiciones.

```html
@if (sports.length < 5) {
    <p style="color: green;">Hay más de 5 deportes</p>
} @else {
    <p style="color: red;">Hay menos de 5 deportes</p>
}
```

Esta sintaxis permite mostrar un contenido u otro dependiendo de la condición.

## 6. Sintaxis moderna: @for

La estructura `@for` permite recorrer los elementos de un array.

```html
@for (sport of sports; track sport; let i = $index) {
    <li>{{ i }} - {{ sport }}</li>
} @empty {
    <p style="color: red;">Array Vacio</p>
}
```

- `sport`: elemento actual del array.
- `track sport`: identifica los elementos del recorrido.
- `$index`: permite obtener la posición del elemento.
- `@empty`: permite mostrar contenido cuando el array está vacío.

## 7. Cambiar estilos según una condición

Podemos modificar los estilos de los elementos dependiendo de una condición.

### 7.1. Estilos en línea

En este ejemplo, los números pares se muestran en verde y los impares en rojo.

```html
@for (num of numbers; track num; let i = $index) {
    <p [style.color]="num % 2 == 0 ? 'green' : 'red'">
        {{ num }}
    </p>
}
```

### 7.2. Utilizar clases CSS

Podemos asignar clases dependiendo de una condición.

```html
@for (num of numbers; track num) {
    <li [class.par]="num % 2 == 0"
        [class.impar]="num % 2 == 1">
        {{ num }}
    </li>
}
```

En este caso:
- Si el número es par, se aplica la clase `par`.
- Si el número es impar, se aplica la clase `impar`.

### 7.3. Utilizar ngClass

También podemos utilizar `ngClass` para asignar las clases mediante un objeto.

```html
@for (num of numbers; track num) {
    <li [ngClass]="{
        par: num % 2 == 0,
        impar: num % 2 == 1
    }">
        {{ num }}
    </li>
}
```

En este ejemplo, las clases se aplican según el resultado de las condiciones.
