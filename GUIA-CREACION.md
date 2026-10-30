# 20 - Comandos de Angular

Comandos que aparecen en mis apuntes de Angular (`ANGULAR(6).docx`).

## Instalación

Instalar Angular CLI de forma global:

```bash
npm install -g @angular/cli@latest
```

## Crear un proyecto

Crear el proyecto `primerangular` sin standalone y sin routing:

```bash
ng new primerangular --standalone=false --routing=false
```

Durante la creación, según los apuntes:
1. Escribir `n`.
2. Seleccionar `css`.
3. Volver a escribir `n`.
4. Seleccionar `none`.

## Ejecutar el servidor

```bash
ng serve
```

## Generar componentes

Crear el componente `cineComponent` dentro de `components`:

```bash
ng g c components/cineComponent
```

Crear un componente en `src/components`, omitiendo su importación automática:

```bash
ng g c cineComponent --path src/components --skip-import
```

## Instalar una dependencia

Instalar `xlsx` y guardarlo como dependencia del proyecto:

```bash
npm install xlsx --save
```

## Generar environments

```bash
ng g environments
```
