# 13. Bootstrap en Angular

## 1. ¿Qué es Bootstrap?

Bootstrap es un framework que permite utilizar estilos y componentes prediseñados para construir interfaces web.

En Angular podemos utilizar Bootstrap para dar estilo a las vistas HTML.

## 2. Instalar Bootstrap mediante CDN

Para utilizar Bootstrap en el proyecto, copiamos las rutas CDN en el archivo `index.html`.

Debemos añadir:

- El enlace CSS (`link`) dentro de la etiqueta `<head>`.
- El script JavaScript (`script`) dentro de la etiqueta `<body>`.

### Enlace CSS

Añadimos dentro de `<head>`:

```html
<link
  href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.8/dist/css/bootstrap.min.css"
  rel="stylesheet"
  integrity="sha384-sRIl4kxILFvY47J16cr9ZwB07vP4J8+LH7qKQnuqkuIAvNWLzeN8tE5YBujZqJLB"
  crossorigin="anonymous"
>
```

### Script JavaScript

Añadimos antes de cerrar la etiqueta `<body>`:

```html
<script
  src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.8/dist/js/bootstrap.bundle.min.js"
  integrity="sha384-FKyoEForCGlyvwx9Hj09JcYn3nv7wiPVlz7YYwJrWVcXK/BmnVDxM+D2scQbITxI"
  crossorigin="anonymous">
</script>
```

## 3. Utilizar Bootstrap en las vistas

Una vez incluidos los recursos, podemos utilizar las clases de Bootstrap directamente en el HTML de los componentes.

Por ejemplo:

```html
<div class="container">
  <h1 class="text-primary">Hola Angular</h1>

  <button class="btn btn-primary">
    Pulsar
  </button>
</div>
```

- `container`: crea un contenedor para el contenido.
- `text-primary`: aplica el color principal al texto.
- `btn`: aplica el estilo base de botón.
- `btn-primary`: aplica el estilo de botón principal.

## 4. Resumen

- Bootstrap permite aplicar estilos y utilizar componentes prediseñados.
- Para utilizarlo mediante CDN, añadimos el CSS y el JavaScript en `index.html`.
- Después podemos utilizar sus clases en el HTML de nuestros componentes.
