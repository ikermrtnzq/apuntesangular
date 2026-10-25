
# MODELOS Y COMUNICACIÓN ENTRE COMPONENTES EN ANGULAR

## 1. Modelos en Angular

Un modelo es un objeto declarado con sus propiedades y su tipado. En Angular, los modelos son clases, como en Java.

### 1.1. Crear un modelo Producto

Creamos la clase `Producto` con sus propiedades y su constructor.

```typescript
export class Producto {
    public nombre: string;
    public imagen: string;
    public precio: number;

    constructor(name: string, img: string, price: number) {
        this.imagen = img;
        this.nombre = name;
        this.precio = price;
    }
}
```

En este ejemplo, el modelo tiene tres propiedades:

- `nombre`: de tipo `string`.
- `imagen`: de tipo `string`.
- `precio`: de tipo `number`.

### 1.2. Utilizar el modelo en otro componente

Podemos crear un producto dentro de otro componente.

```typescript
export class ListaProductosComponent {
    public producto: Producto;

    constructor() {
        this.producto = new Producto(
            "batido",
            "https://www.istockphoto.com/es/fotos/batido-de-leche",
            4.97
        );
    }
}
```

### 1.3. Declarar las propiedades directamente en el constructor

También podemos declarar las propiedades directamente en los parámetros del constructor.

```typescript
export class Producto {
    constructor(
        public nombre: string,
        public imagen: string,
        public precio: number
    ) {
    }
}
```

## 2. Comunicación entre componentes

La comunicación entre componentes se realiza de dos formas:

- **De padre a hijo:** mediante propiedades (`@Input`).
- **De hijo a padre:** mediante métodos y `@Output`.

## 3. Comunicación de padre a hijo

En el componente padre tenemos un array de coches que vamos pasando al componente hijo.

### 3.1. Crear el array de coches en el componente padre

```typescript
export class PadreCochesComponent {
    public coches: Array<Coche>;

    constructor() {
        this.coches = [
            new Coche("Ford", "mustang", 0, 45),
            new Coche("volkswagen", "escarabajo", 0, 10),
            new Coche("lamborgini", "diablo", 0, 75)
        ];
    }
}
```

### 3.2. Enviar los objetos al componente hijo

En el HTML del componente padre, recorremos el array y enviamos cada coche al hijo mediante una propiedad.

```html
@for (coche of coches; track coche) {
    <app-hijo-coche-component [car]="coche"></app-hijo-coche-component>
}
```

En este ejemplo:

- `coches`: es el array de coches del padre.
- `coche`: representa cada coche del recorrido.
- `[car]="coche"`: envía el objeto al componente hijo.

### 3.3. Recibir los datos en el componente hijo

En el componente hijo utilizamos `@Input` para recibir el objeto enviado por el padre.

```typescript
import { Component, Input } from '@angular/core';

export class HijoCocheComponent {
    @Input() car!: Coche;
}
```

### 3.4. Mostrar los datos del coche en HTML

Una vez recibido el objeto, podemos consultar sus propiedades desde el HTML del hijo.

```html
<h1>{{ car.marca }} {{ car.modelo }}</h1>
```

En este caso, mostramos la marca y el modelo del coche.

## 4. Comunicación de hijo a padre

Para enviar información desde el componente hijo al componente padre, utilizaremos variables `@Output`.

Según los apuntes, dicha variable será un método `emit`, como en Vue.

**Este apartado todavía no está desarrollado de forma práctica en el documento.**
