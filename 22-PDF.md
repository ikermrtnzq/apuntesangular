# 22 - Generar documentos PDF

## Instalar las librerías

Para generar documentos PDF necesitamos instalar las siguientes librerías:

```bash
npm install jspdf jspdf-autotable --save
```

## Generar un documento PDF

En el componente donde queremos generar el PDF, creamos el método `generarDocumento()`:

```typescript
generarDocumento(): void {
    let documento: jsPDF = new jsPDF();

    documento.setFontSize(18);
    documento.text("Mi documento", 10, 10);

    const headers = [["Nombre", "Email", "Edad"]];

    const data = [
        ["Diana", "diana@gmail.com", 44],
        ["Mari", "mari@gmail.com", 20],
        ["Patricia", "patricia@gmail.com", 80],
    ];

    autoTable(documento, {
        head: headers,
        body: data,
        startY: 20
    });

    documento.save("personas.pdf");
}
```

### Explicación

1. **Crear el documento:**

   ```typescript
   let documento: jsPDF = new jsPDF();
   ```

   Creamos un nuevo documento PDF.

2. **Establecer el tamaño de letra:**

   ```typescript
   documento.setFontSize(18);
   ```

   Indicamos el tamaño de la fuente.

3. **Añadir texto:**

   ```typescript
   documento.text("Mi documento", 10, 10);
   ```

   Añadimos texto al PDF indicando sus coordenadas.

4. **Crear las cabeceras de la tabla:**

   ```typescript
   const headers = [["Nombre", "Email", "Edad"]];
   ```

5. **Crear los datos:**

   ```typescript
   const data = [
       ["Diana", "diana@gmail.com", 44],
       ["Mari", "mari@gmail.com", 20],
       ["Patricia", "patricia@gmail.com", 80],
   ];
   ```

6. **Añadir la tabla al documento:**

   ```typescript
   autoTable(documento, {
       head: headers,
       body: data,
       startY: 20
   });
   ```

   Utilizamos `autoTable()` para insertar una tabla con cabeceras y datos. `startY` indica la posición vertical donde comienza.

7. **Guardar el PDF:**

   ```typescript
   documento.save("personas.pdf");
   ```

   Generamos y descargamos el archivo `personas.pdf`.

## Llamar al método desde HTML

Luego, en un botón llamamos a la función:

```html
<button (click)="generarDocumento()">
    Generar PDF
</button>
```
