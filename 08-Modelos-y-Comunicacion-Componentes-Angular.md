# 08. Modelos y comunicación entre componentes en Angular

## 1. Modelos en Angular

Un modelo es un objeto declarado con sus propiedades y su tipado. En Angular podemos utilizar clases de TypeScript para representar objetos, de forma similar a Java.

### 1.1. Crear un modelo `Producto`

Creamos la clase `Producto` con sus propiedades y su constructor.

```typescript
export class Producto {
    public nombre: string;
    public imagen: string;
    public precio: number;

    constructor(nombre: string, imagen: string, precio: number) {
        this.imagen = imagen;
        this.nombre = nombre;
        this.precio = precio;
    }
}
```

El modelo tiene tres propiedades:

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

Para utilizar `Producto` desde otro archivo, debemos importarlo desde la ruta donde esté definido.

### 1.3. Declarar las propiedades directamente en el constructor

También podemos declarar las propiedades directamente en los parámetros del constructor.

```typescript
export class Producto {
    constructor(
        public nombre: string,
        public imagen: string,
        public precio: number
    ) {}
}
```

Esta forma permite declarar e inicializar las propiedades sin tener que escribir las asignaciones manualmente.

## 2. Comunicación entre componentes

La comunicación entre componentes se puede realizar en dos direcciones:

- **De padre a hijo:** mediante `@Input`.
- **De hijo a padre:** mediante `@Output` y `EventEmitter`.

## 3. Comunicación de padre a hijo con `@Input`

En el componente padre tenemos un array de coches que enviamos al componente hijo.

### 3.1. Crear el array en el padre

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

### 3.2. Enviar los objetos al hijo

En el HTML del padre recorremos el array y enviamos cada coche al componente hijo.

```html
@for (coche of coches; track coche) {
    <app-hijo-coche-component
        [car]="coche">
    </app-hijo-coche-component>
}
```

- `coches`: array del componente padre.
- `coche`: representa cada elemento del recorrido.
- `[car]="coche"`: envía el objeto al hijo mediante la propiedad `car`.

### 3.3. Recibir los datos en el hijo

En el componente hijo utilizamos `@Input` para recibir el objeto.

```typescript
import { Component, Input } from '@angular/core';

export class HijoCocheComponent {
    @Input() car!: Coche;
}
```

El signo `!` indica a TypeScript que la propiedad se inicializará posteriormente.

### 3.4. Mostrar los datos en el HTML del hijo

```html
<h1>{{ car.marca }} {{ car.modelo }}</h1>
```

Una vez recibido el objeto, podemos acceder a sus propiedades desde la plantilla.

## 4. Comunicación de hijo a padre con `@Output` y `EventEmitter`

Para enviar información desde el hijo al padre, el hijo emite un evento y el padre lo recibe mediante un método.

### 4.1. Crear el método en el padre

En el componente padre declaramos una variable para guardar el mensaje y un método que recibe el dato enviado por el hijo.

```typescript
public mensaje!: string;

seleccionarFavorito(event: string): void {
    this.mensaje = "Su deporte favorito es " + event;
}
```

El método recibe un `string` y construye el mensaje que se mostrará en la vista.

### 4.2. Enviar el deporte al hijo y escuchar el evento

En el HTML del padre:

```html
@for (dep of arrayDeportes; track dep) {
    <app-deporte-component
        (seleccionarFavorito)="seleccionarFavorito($event)"
        [sport]="dep">
    </app-deporte-component>
}
```

Aquí utilizamos dos enlaces:

- `[sport]="dep"`: envía el deporte al componente hijo.
- `(seleccionarFavorito)="seleccionarFavorito($event)"`: escucha el evento emitido por el hijo y llama al método del padre.

`$event` contiene el valor que envía el componente hijo.

### 4.3. Emitir el evento desde el hijo

En el componente hijo importamos `Input`, `Output` y `EventEmitter`.

```typescript
import { Component, Input, Output, EventEmitter } from '@angular/core';

export class DeporteComponent {
    @Input() sport!: string;

    @Output() seleccionarFavorito: EventEmitter<any> =
        new EventEmitter();

    mostrarFavorito(): void {
        this.seleccionarFavorito.emit(this.sport);
    }
}
```

Conceptos importantes:

- `@Input`: recibe información del padre.
- `@Output`: declara un evento que el padre puede escuchar.
- `EventEmitter`: permite emitir el evento.
- `.emit(this.sport)`: envía el deporte al padre.
- `mostrarFavorito()`: método que realiza la emisión del evento.

En este ejemplo se utiliza `EventEmitter<any>`, como en los apuntes. Si el evento siempre envía un texto, se puede utilizar `EventEmitter<string>` para indicar el tipo de dato.

### 4.4. Mostrar el mensaje en el padre

En el HTML del padre mostramos el mensaje recibido:

```html
<h2>{{ mensaje }}</h2>
```

Cuando el hijo emite el deporte favorito, el padre ejecuta `seleccionarFavorito()`, actualiza `mensaje` y Angular muestra el resultado.

## 5. Resumen

| Comunicación | Mecanismo | Función |
|---|---|---|
| Padre → hijo | `@Input` | El hijo recibe datos del padre. |
| Hijo → padre | `@Output` y `EventEmitter` | El hijo emite un evento con información. |
| Recibir el evento | `$event` | Permite acceder al dato emitido. |
| Actualizar el mensaje | Método del padre | Procesa el dato recibido y actualiza la variable. |
