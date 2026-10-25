
# HOOKS DEL CICLO DE VIDA Y MÉTODOS EN ANGULAR

## 1. Hooks del ciclo de vida

Los hooks nos permiten saber en qué momento del ciclo de vida se encuentra un componente.

### ngOnInit()

Es el primer método que se ejecuta después del constructor.

- Debemos implementar `OnInit`.
- El constructor se utiliza para inicializar las variables.
- Este método trabaja con las variables ya inicializadas.

Ejemplo:

```typescript
export class HookComponent implements OnInit {

    ngOnInit(): void {
        console.log("OnInit: Despues de constructor");
    }

}
```

### ngDoCheck()

Se ejecuta cada vez que se modifica la representación del componente, según los apuntes.

```typescript
ngDoCheck(): void {
    console.log("DoCheck: cada vez que el component se actualiza");
}
```

### ngOnDestroy()

Se ejecuta cuando el componente padre elimina el componente de su representación.

## 2. Métodos en Angular

Los métodos permiten ejecutar acciones dentro de un componente.

### Métodos que no devuelven ningún valor

Para los métodos que realizan acciones y no devuelven ningún valor, utilizamos `void`.

Ejemplo:

```typescript
cambiarMensaje(): void {
    this.mensaje = "He cambiado hoy es martess";
}
```

Podemos llamar al método desde el HTML utilizando el evento `(click)`:

```html
<h2>{{ mensaje }}</h2>

<button (click)="cambiarMensaje()">
    cambiar el mensaje
</button>
```

### Métodos que devuelven un valor

Si un método devuelve un valor, indicamos el tipo de dato que devuelve.

Ejemplo:

```typescript
nombreMetodoAccion(): string {
    // ACCIONES DEL METODO
    return value;
}
```

En este caso, el método devuelve un valor de tipo `string`.

## 3. Actualización de variables

En Angular, las variables se pueden actualizar desde los métodos del componente.

Por ejemplo, al ejecutar `cambiarMensaje()`, se modifica el valor de `mensaje`:

```typescript
cambiarMensaje(): void {
    this.mensaje = "He cambiado hoy es martess";
}
```
