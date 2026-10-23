[Ver AquaSur en GitHub Pages](https://irenepm777.github.io/Landing_responsiva_Irene/)

# AquaSur - Landing Responsive con Sass

## Descripción

En esta práctica he retomado **AquaSur**, la landing responsive de un acuario ficticio de Sevilla que desarrollé anteriormente, para rehacer sus estilos utilizando **Sass (SCSS)**.

Mi objetivo ha sido aprovechar las herramientas que ofrece Sass para organizar mejor el código, reducir repeticiones y facilitar su mantenimiento, pero sin modificar el diseño original de la página.

La landing sigue incluyendo sus componentes personalizados de Bootstrap, tres zonas marinas, una galería con carrusel, diseño responsive y modo oscuro.

## 1. Estructura del proyecto

He decidido dividir los estilos en distintos archivos SCSS según su función, en lugar de mantener todo el código en una única hoja de estilos.

Los archivos que empiezan por `_` son **parciales de Sass**, por lo que no se compilan individualmente.

He utilizado `styles.scss` y `dark.scss` como archivos principales para generar las dos hojas CSS que necesita la página.

## 2. Variables

He centralizado los colores, tipografías, rutas de imágenes y breakpoints en `_variables.scss`, para no tener que repetir los mismos valores constantemente.

Por ejemplo:

```scss
$white: #ffffff;
$ink: #08222e;
$primary: #0f5c78;

$font-body: "Work Sans", sans-serif;
$font-heading: "Quicksand", sans-serif;
```

De esta forma, si quiero cambiar un color principal o una tipografía, puedo hacerlo desde un único archivo sin tener que buscar todas sus apariciones.

## 3. Funciones personalizadas

En `_functions.scss` he creado varias funciones usando `@function` y `@return` para hacer cálculos y devolver valores que puedo reutilizar en los estilos.

Una de las más útiles es `rem()`, que convierte valores de píxeles a rem:

```scss
@function rem($px, $base: 16) {
  @return math.div($px, $base) * 1rem;
}
```

Por ejemplo, `rem(20)` devuelve `1.25rem`.

También he definido otras funciones:

- **`bp()`**: obtiene los breakpoints almacenados en un mapa.
- **`shade()`**: permite oscurecer un color.
- **`with-alpha()`**: modifica la transparencia de un color.
- **`tint()`**: mezcla un color con blanco.

Estas funciones me permiten reutilizar operaciones sin tener que escribirlas de nuevo cada vez.

## 4. Funciones nativas de Sass

Además de crear mis propias funciones, he utilizado módulos nativos de Sass mediante `@use`:

- **`sass:math`**: para realizar operaciones matemáticas con `math.div()`.
- **`sass:map`**: para consultar los mapas con `map.get()` y `map.has-key()`.
- **`sass:color`**: para trabajar con colores mediante `color.adjust()`, `color.change()` y `color.mix()`.

Por ejemplo, he creado esta función para gestionar transparencias:

```scss
@function with-alpha($c, $a) {
  @return color.change($c, $alpha: $a);
}
```

La he utilizado en elementos como las sombras de las tarjetas, los bordes del navbar y los textos del footer.

## 5. Mixins

En `_mixins.scss` he definido varios bloques reutilizables mediante `@mixin` y `@include`.

Uno de ellos es `boton()`, que me permite configurar los colores de los botones y sus estados `hover` y `active`.

Por ejemplo, en el botón principal:

```scss
.btn-aqua {
  @include boton(
    $primary,
    $white,
    $primary-hover,
    $primary-active
  );
}
```

También he creado el mixin `up()`, que utilizo para gestionar los breakpoints de la web:

```scss
@mixin up($name) {
  @media (min-width: bp($name)) {
    @content;
  }
}
```

Así puedo incluir las media queries directamente en cada componente, sin tener que agruparlas todas al final del archivo.

He añadido otros mixins para las etiquetas, los títulos de sección, las imágenes de fondo y los elementos decorativos.

## 6. Anidamiento (Nesting)

Otra de las funcionalidades que he aprovechado es el anidamiento de selectores.

Antes tenía que escribir varias reglas independientes para los elementos de un mismo componente. Ahora puedo agruparlas dentro de su selector principal.

Por ejemplo:

```scss
.nav-aqua {
  .nav-link {
    color: $ink;

    &:hover {
      text-decoration: underline;
    }
  }
}
```

También he utilizado `&` para definir pseudoclases como `:hover` y `:active`.

Esto me ha permitido reducir bastante la repetición de selectores y mantener juntos los estilos relacionados.

## 7. Modularidad

Para organizar los archivos he utilizado **`@use` y `@forward`**.

En `_index.scss` he reexportado las variables, funciones y mixins para poder acceder a ellos desde el resto de archivos.

Después, en `styles.scss`, he reunido los distintos componentes:

```scss
@use "base";
@use "botones";
@use "navbar";
@use "home";
@use "cards";
@use "carrusel";
@use "footer";
```

He preferido mantener todos los parciales dentro de una misma carpeta, ya que el proyecto no es especialmente grande y así me resulta más cómodo localizar los archivos.

Aunque estén separados, Sass los combina durante la compilación para generar una única hoja de estilos principal.

## 8. Diseño responsive

He mantenido los mismos breakpoints que tenía la landing original:

| Dispositivo | Breakpoint |
|---|---|
| Móvil | Menos de 768px |
| Tablet | Desde 768px |
| Escritorio | Desde 1200px |

La principal diferencia es que ahora utilizo el mixin `up()` para aplicar los estilos correspondientes a cada tamaño de pantalla.

También he conservado la disposición en zigzag de las tarjetas, la adaptación del navbar y los diferentes tamaños de los elementos.

## 9. Modo oscuro

He mantenido el modo oscuro en un archivo independiente, `dark.scss`.

Los estilos se aplican cuando JavaScript añade la clase `.dark-mode` al `body`.

En esta hoja también he utilizado variables, anidamiento y funciones como `with-alpha()` para gestionar los colores y sus transparencias.

He mantenido el degradado original y los cambios de color de los botones, enlaces y demás elementos.

## 10. Compilación de Sass

Como los navegadores no interpretan SCSS directamente, he tenido que compilar los archivos para convertirlos en CSS estándar.

Para ello he instalado Sass mediante npm:

```powershell
npm.cmd install -g sass
```

Después he compilado las dos hojas principales con estos comandos:

```powershell
sass.cmd scss/styles.scss css/styles.css
sass.cmd scss/dark.scss css/dark.css
```

También he utilizado el modo `--watch` para detectar cambios automáticamente:

```powershell
sass.cmd --watch scss:css
```

Esto me ha permitido seguir trabajando en los archivos SCSS sin tener que ejecutar manualmente la compilación cada vez que modificaba algo.

Los archivos CSS generados son los que finalmente utiliza `index.html`.

## 11. Publicación con GitHub Pages

He realizado toda la práctica en la rama `sass` del repositorio original, manteniendo así el trabajo anterior en su propia rama.

Una vez terminada la refactorización y compilados los estilos, he publicado la landing mediante **GitHub Pages**.

He comprobado que se mantienen los estilos, las imágenes, el diseño responsive, el carrusel y el funcionamiento del modo oscuro.

**Enlace a la landing:**  
https://irenepm777.github.io/Landing_responsiva_Irene/

## Conclusión

Con esta práctica he aprendido a utilizar Sass para organizar los estilos de una forma muuuucho más cómoda que con CSS tradicional.

Lo que más útil me ha parecido ha sido poder dividir el código en archivos independientes, reutilizar estilos mediante mixins y trabajar con funciones para evitar repetir operaciones.

Además, he podido aplicar todo esto sobre una web que ya tenía terminada, así que he visto bastante bien la diferencia entre trabajar directamente con CSS y utilizar un preprocesador.