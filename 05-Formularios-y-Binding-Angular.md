
# FORMULARIOS Y BINDING EN ANGULAR

## 1. Formularios en Angular

Para trabajar con formularios, debemos activar e importar `FormsModule` en el módulo principal.

En `app-module.ts`, importamos el componente del formulario y añadimos `FormsModule` a `imports`.

```typescript
import { FormsComponent } from '../components/formsbindingcomponent/forms.binding.component';

imports: [
    BrowserModule,
    FormsModule
]
```

## 2. Métodos para recoger datos de un formulario

En los apuntes se explican dos métodos:

- **Binding:** conecta un elemento HTML con una variable del componente.
- **Referencia a un objeto:** permite acceder al elemento HTML desde TypeScript.

## 3. Binding con ngModel

El binding permite conectar una variable de TypeScript con un elemento HTML. Los cambios se sincronizan en ambos sentidos.

### Componente TypeScript

```typescript
export class TablaMultiplicarComponent {
    public numero: number;
    public mensaje: string;
    public tabla: Array<number>;

    recibirDatos(): void {
        this.mensaje = "recibido";
        this.tabla = [];

        for (let index = 0; index <= 10; index++) {
            let aux = 0;
            aux = this.numero * index;
            this.tabla.push(aux);
        }
    }

    constructor() {
        this.mensaje = "";
        this.numero = 0;
        this.tabla = [];
    }
}
```

El método `recibirDatos()` calcula la tabla de multiplicar del número recibido y guarda los resultados en el array `tabla`.

### HTML del formulario

```html
<form #formtabla="ngForm" (ngSubmit)="recibirDatos()">
    <label>Numero </label>

    <input
        type="number"
        [(ngModel)]="numero"
        name="cajaNum"
        #cajaNum="ngModel"
    >

    <button>Enviar datos</button>
</form>
```

Elementos importantes:

- `#formtabla="ngForm"`: identifica el formulario.
- `(ngSubmit)="recibirDatos()"`: ejecuta el método al enviar el formulario.
- `[(ngModel)]="numero"`: conecta el input con la variable `numero`.
- `name="cajaNum"`: asigna un nombre al campo.
- `#cajaNum="ngModel"`: crea una referencia al control de Angular.

## 4. Referencia a un objeto HTML

Otra forma de recoger datos consiste en acceder directamente al elemento HTML desde TypeScript mediante `@ViewChild` y `ElementRef`.

### Componente TypeScript

```typescript
export class TablaMultiplicar2Component {
    public tabla: Array<number>;

    @ViewChild("cajanumero") cajanumero: ElementRef;

    constructor() {
        this.tabla = [];
        this.cajanumero = new ElementRef(0);
    }

    recibirDatos() {
        let numero = this.cajanumero.nativeElement.value;

        this.tabla = [];

        for (let num = 0; num <= 10; num++) {
            let aux = 0;
            aux = parseInt(numero) * num;
            this.tabla.push(aux);
        }
    }
}
```

El método `recibirDatos()` obtiene el valor del input mediante:

```typescript
let numero = this.cajanumero.nativeElement.value;
```

Después, convierte el valor a número y calcula la tabla de multiplicar.

### HTML del formulario

```html
<form #tablaForm="ngForm" (ngSubmit)="recibirDatos()">
    <label>Numero 1</label>

    <input type="number" #cajanumero>

    <button>Sumar</button>
</form>
```

## 5. Identificación de los formularios

En los apuntes se indica que los formularios utilizan referencias con `#` para identificarlos.

Por ejemplo:

```html
<form #tablaForm="ngForm" (ngSubmit)="recibirDatos()">
```

Se utiliza `ngForm` en el formulario para evitar que se produzca la recarga de la página al enviarlo.

## 6. Inicialización de variables

Angular requiere inicializar las variables para evitar errores.

Si utilizamos `@ViewChild` y queremos indicar que una propiedad se inicializará posteriormente, podemos utilizar el signo `!`.

Ejemplo:

```typescript
@ViewChild("cajanumero1") cajanumero1!: ElementRef;
```
