
# INTRODUCCIÓN A ANGULAR

Angular es un lenguaje SPA. Utiliza TypeScript y podemos trabajar con componentes `standalone` o sin ellos.

## 1. Instalar Angular CLI

Para instalar Angular CLI ejecutamos:

```bash
npm install -g @angular/cli@latest
```

Solo es necesario instalarlo en el equipo. Luego, en VS Code, se instala la extensión **Angular Schematics**.

## 2. Crear un proyecto Angular

Para crear un proyecto llamado `primerangular`, ejecutamos:

```bash
ng new primerangular --standalone=false --routing=false
```

Durante la creación del proyecto, según los apuntes, seleccionamos:

1. Escribir `n`.
2. Seleccionar CSS.
3. Volver a escribir `n`.
4. Seleccionar `none`.

## 3. Ejecutar el proyecto

Entramos en la carpeta del proyecto y ejecutamos:

```bash
cd primerangular
ng serve
```

## 4. Componentes en Angular

En Angular, los componentes separan la vista y la funcionalidad:

- **HTML:** contiene la vista.
- **TypeScript (TS):** contiene la funcionalidad.
- **CSS:** contiene los estilos.

Los componentes se organizan en carpetas.

## 5. Module

En los proyectos de tipo `standalone=false` tenemos un archivo principal llamado `app-module.ts` (normalmente `app.module.ts`), que organiza cómo está construido el proyecto, sus dependencias, el routing y otras características.

## 6. Organización de los componentes

- Tendremos una carpeta para los componentes y otra carpeta para cada componente.
- Los nombres deben escribirse en minúsculas.
- Cada componente debe tener un nombre de dos palabras, según los apuntes.
