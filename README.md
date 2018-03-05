# Ejemplo de Webpack

En esta pequeña guía encontrarán lo necesario para empezar a utilizar 
Webpack. Go!!!

## ¿Qué se va a utilizar?

### Babel

Anteriormente haz visto que _Babel_ es una herramienta que transforma 
tu código JS de última generación a un JS que se pueda entender en los 
navegadores actuales o `node.js`. Para este caso de ES6 a ES5, por
ahora...
> En el LMS hay algunos ejemplos de esto

Al momento de instalar _Babel_ deberás agregar todos los plugins 
(complementos que sirven para agregar una función nueva y específica) 
necesarios para que funcione correctamente. Puedes ver que en la 
[documentación](https://babeljs.io/docs/plugins/) se detallan varios 
plugins, entonces si necesitas usar todo el estándar tendrías que 
instalar cada uno e imáginate hacer eso, sería demasiado. 

Por eso hay un **preset** (paquete especial) que incluye todo el 
estándar que se llama `env`.

También necesitarás instalar el preset de `react` así no se necesitará
importar el transformador de `jsx`.

Y todo esto lo vas a integrar con Webpack.

### Webpack

_Webpack_ es un empaquetador de módulos. Pero, ¿qué es eso? Bueno, es 
tomar todos los módulos que se usan en tu proyecto y luego lo agrupa en 
un solo paquete. 

En la página de [Webpack](https://webpack.js.org/) puedes ver que hay
muchos archivos JS, CSS, etc; que hacen de un sitio web muy grande.
Entonces _Webpack_ toma esos archivos y los convierte, minimiza, 
optimiza empaquetándolos a uno solo o cuántos desees.

Entonces con esto "transformamos" nuestro código del entorno de 
 _desarrollo_ a un entorno de _producción_.

## A empezar!!

### Requerimientos previos

Deberás tener instalado `node.js` y `npm`. Si por algún motivo no lo 
tienes, puedes descargarlo desde su [página web](https://nodejs.org/es/).
Recuerda que `npm` se instalará de manera automática. Luego, si deseas, 
verifica las versiones descargadas con los siguientes comandos:

        node --version
        npm --version
        
Vas a instalar varias dependencias y para ello puedes utilizar`npm` o 
`yarn`. Yo sé que te gusta `npm`, entonces no se diga más =)

### Pasos

1. Crearás tu `package.json` y completarás los campos solicitados:
        
        npm init

2. Instalarás las dependencias, hay 2 tipos:
    > **Desarrollo**: para fase de desarrollo: `--save-dev`. Ej: 
    webpack, babel, etc.
    
    > **Producción**: propias del proyecto: `--save`. Ej: react, 
    bootstrap, etc.   

    Se pueden instalar de a uno o varias a la vez. Empezarás con las de 
    desarrollo:
    
    2.1. Tener `webpack` de forma global
        
    2.2. `webpack`
    
    2.3. `webpack-cli`
        
    2.4. `webpack-dev-server`: servidor de desarrollo
       
       npm install -g webpack 
        
       npm install --save-dev webpack webpack-cli webpack-dev-server
       
3. Instalarás Babel:
    
    3.1. `babel-cli`: incluye el core de babel, el cual es necesario, de
    lo contrario no funcionaría babel
    
    3.2. `babel-loader`: cargador, nos permitirá usar babel en webpack
    
    3.3. `babel-preset-env`: permite usar el nuevo estándar de ES6
    
    3.4. `babel-preset-react`: permite usar react

       npm install --save-dev babel-cli babel-loader babel-preset-env babel-preset-react
       
4. Si usas estilos, webpack ofrece un cargador para los estilos; así 
usarás un CSS específico para cada componente y podrás crear un CSS final:

       npm install --save-dev css-loader style-loader
       
5. Webpack se encargará de crear el ambiente de producción, por lo que 
hará una copia de tus carpetas al `public` o `dist` de forma
automática con:

        npm install --save-dev html-webpack-plugin
        
6. Ahora, para que se abra el navegador usarás:

        npm install --save-dev open-browser-webpack-plugin
        
7. Dependencias de producción: `react` y `react-dom`

        npm install --save react react-dom
        
Deberá quedar así:

```json
"devDependencies": {
    "babel-cli": "^6.26.0",
    "babel-loader": "^7.1.3",
    "babel-preset-env": "^1.6.1",
    "babel-preset-react": "^6.24.1",
    "css-loader": "^0.28.10",
    "html-webpack-plugin": "^3.0.4",
    "open-browser-webpack-plugin": "0.0.5",
    "style-loader": "^0.20.2",
    "webpack": "^4.1.0",
    "webpack-dev-server": "^3.1.0"
  },
  "dependencies": {
    "react": "^16.2.0",
    "react-dom": "^16.2.0"
  }
```
        
### Completando configuración de Babel

No olvides crear el archivo `.babelrc`

```json
{
  "presets": [
    "env",
    "react"
  ]
}
``` 

## Completa archivos

### Crea tu archivo `index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Ejemplo Webpack</title>
  </head>
  <body>
    <div class="root" id="root"></div>
    <script src="main.js"></script>
  </body>
</html>
```

### Crea tu archivo `main.js`

Prueba inicial

```js
var great = 'Hola promo 2017-2';
alert(great);
```

Ejemplo más elaborado:

```js
var coders = [
  {
    name: 'miriam',
    promo: '2017-1'
  },
  {
    name: 'lily',
    promo: '2017-1'
  }
];

console.log(coders);
```

### Archivo de configuración

```js

const { resolve } = require('path');

// importamos webpack
const webpack = require('webpack');
// ... y los plugins que habiamos instalado
// declarando los plugins...
const OpenBrowserPlugin = require('open-browser-webpack-plugin');
const HtmlWebpackPlugin = require('html-webpack-plugin');

const config = {
  entry: [
    './index.js' // archivo de entrada
  ],
  output: { // archivos de salida
    path: __dirname, // ruta donde va a estar el archivo, si deseo que sea aqui mismo pongo _dirname
    filename: 'build.js', // archivo de salida
    publicPath:'/'
  },
  module: {
    rules: [
      {
	test: /\.js$/, // todos los `js`
	loaders: [
	  'babel-loader', // los procesamos con el loader de `babel`
	],
	exclude: /node_modules/, // ignoramos archivos dentro de node_modules
      }, // luego
      {
	test: /\.css$/, // todos los archivos `csss`
	use: [
	  { loader: "style-loader" }, // primero creamos un tag `style`
	  { loader: "css-loader" } // y le injetamos el `css`
	]
      }
    ]
  },
  plugins: [
    // llamando los plugins
    new OpenBrowserPlugin({ // abre el navegador
      url: 'http://localhost:8080'
    }),
    new HtmlWebpackPlugin({
      template: './index.html',
      filename: 'index.html', // nombre del archivo
      inject: 'body'
    })
  ],
  devServer: { // configuracion del servidor local
    contentBase: resolve(__dirname, 'build'),
    publicPath: '/'
  }
}

module.export = config;

```
