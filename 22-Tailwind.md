# Instalación y uso de Tailwind CSS en Angular

## 1. Introducción

Tailwind CSS es un framework CSS que permite diseñar interfaces utilizando clases directamente en el HTML, sin necesidad de escribir tanto CSS tradicional.

Ejemplo:

```html
<button class="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700">
  Mi botón
</button>
```

En esta guía instalaremos **Tailwind CSS 4** en un proyecto Angular.

## 2. Crear un proyecto Angular

Si todavía no tienes un proyecto Angular, puedes crearlo ejecutando:

```bash
ng new mi-proyecto
```

Accede a la carpeta del proyecto:

```bash
cd mi-proyecto
```

## 3. Instalar Tailwind CSS

Ejecuta el siguiente comando:

```bash
npm install tailwindcss @tailwindcss/postcss postcss
```

## 4. Configurar PostCSS

Crea un archivo llamado `.postcssrc.json` en la raíz del proyecto Angular.

Añade el siguiente contenido:

```json
{
  "plugins": {
    "@tailwindcss/postcss": {}
  }
}
```

## 5. Configurar Tailwind CSS

Abre el archivo:

```text
src/styles.css
```

Añade la siguiente línea:

```css
@import "tailwindcss";
```

Con esto, Tailwind CSS queda configurado para utilizarse en los componentes Angular.

No necesitas crear un archivo `tailwind.config.js` para una instalación básica de Tailwind 4.

## 6. Utilizar Tailwind en Angular

Abre el archivo HTML principal de tu aplicación:

```text
src/app/app.html
```

En proyectos Angular con una estructura tradicional también puedes encontrarlo como `app.component.html`.

Añade este ejemplo:

```html
<div class="min-h-screen bg-gray-100 flex items-center justify-center p-6">

  <div class="bg-white rounded-xl shadow-lg p-8 max-w-md w-full">

    <h1 class="text-3xl font-bold text-gray-800 mb-4">
      Hola Angular + Tailwind
    </h1>

    <p class="text-gray-600 mb-6">
      Mi primera aplicación utilizando Tailwind CSS.
    </p>

    <button
      class="bg-blue-600 hover:bg-blue-700 text-white font-semibold px-6 py-3 rounded-lg transition-colors">
      Empezar
    </button>

  </div>

</div>
```

## 7. Ejecutar el proyecto

Inicia el servidor de desarrollo:

```bash
ng serve
```

Abre el navegador en:

```text
http://localhost:4200
```

Si todo está configurado correctamente, verás una tarjeta centrada con un botón azul.

## 8. Clases de Tailwind más utilizadas

| Categoría | Clase | Función |
|---|---|---|
| Texto | `text-xl` | Texto grande |
| Texto | `font-bold` | Negrita |
| Texto | `text-gray-600` | Color gris |
| Fondo | `bg-blue-500` | Fondo azul |
| Espaciado | `p-4` | Padding |
| Espaciado | `m-4` | Margin |
| Bordes | `rounded-lg` | Bordes redondeados |
| Sombras | `shadow-lg` | Sombra grande |
| Flexbox | `flex` | Activa Flexbox |
| Flexbox | `justify-center` | Centrado horizontal |
| Flexbox | `items-center` | Centrado vertical |
| Grid | `grid grid-cols-3` | Grid de tres columnas |
| Responsive | `md:flex` | Flex a partir del breakpoint md |

## 9. Diseño responsive

Tailwind permite adaptar los diseños a diferentes tamaños de pantalla.

Ejemplo:

```html
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">

  <div class="bg-blue-500 p-6 rounded-lg text-white">
    Tarjeta 1
  </div>

  <div class="bg-green-500 p-6 rounded-lg text-white">
    Tarjeta 2
  </div>

  <div class="bg-red-500 p-6 rounded-lg text-white">
    Tarjeta 3
  </div>

</div>
```

Resultado:

- Móvil: 1 columna.
- Tablet (`md`): 2 columnas.
- Escritorio (`lg`): 3 columnas.

## 10. Personalizar colores

En Tailwind CSS 4 puedes definir colores personalizados desde `src/styles.css`:

```css
@import "tailwindcss";

@theme {
  --color-primary: #2563eb;
  --color-secondary: #16a34a;
  --color-accent: #f59e0b;
}
```

Después puedes utilizarlos directamente en los componentes:

```html
<button class="bg-primary text-white px-4 py-2 rounded-lg">
  Mi botón personalizado
</button>
```

## 11. Resumen de instalación

Los pasos necesarios para instalar Tailwind CSS 4 en Angular son:

1. Crear un proyecto Angular.
2. Instalar las dependencias de Tailwind.
3. Crear el archivo `.postcssrc.json`.
4. Añadir `@import "tailwindcss";` en `src/styles.css`.
5. Utilizar las clases de Tailwind en los componentes HTML.
6. Ejecutar `ng serve`.

## 12. Documentación oficial

- Angular: https://angular.dev/
- Tailwind CSS: https://tailwindcss.com/docs
- Instalación de Tailwind con Angular: https://tailwindcss.com/docs/installation/framework-guides/angular

**Recomendación:** utiliza Tailwind CSS 4 para proyectos nuevos, ya que simplifica considerablemente la configuración.
