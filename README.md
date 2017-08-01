# ES6 Tutorial

Tutorial ES5 a ES6

# 1\. Instalar la App de ejemplo:

```
git clone https://github.com/kotehector/ES5-to-ES6-tutorial.git
```

# 2\. Configuración de Babel:

2.1 Crear package.json en el root del proyecto. Lanzamos `npm init`

2.2 Instalamos babel-cli y babel-core lanzando:

```javascript
  npm install babel-cli babel-core --save-dev
```

2.3\. Instalamos el preset para ECMAScript 2015:

```javascript
  npm install babel-preset-es2015 --save-dev
```

2.4\. Instalamos un pequeño servidor local:

```javascript
  npm install http-server --save-dev
```

2.5\. Editamos el `package.json` la sección `scripts: {}` y le añadimos:

```javascript
  "scripts": {
    "babel": "babel --presets es2015 js/main.js -o build/main.bundle.js",
    "start": "http-server"
  }
```

2.6\. Creamos la carpeta `build`en el root.

# 3\. Construír proyecto y levantar el servidor:

3.1 Lanzar babel y compilar el main.js:

```javascript
  npm run babel
```

3.2 Substituimos en el `ìndex.html` el `js/main.js` por el archivo que acabamos de crear: `build/main.bundle.js` con `babel`.

3.3 Comparamos el `js/main.js` con `build/main.bundle.js`y vemos que de momento no hay ningún cambio porque en el `js/main.js` no hay nada con ECMA6.

# 4\. Usar variable `let`:

- Las variables declaradas con `var`son "function-scoped".
- Las declaradas con `let`son "block-scoped", solo existen en el bloque de código donde fueron definidas.

  4.1 Cambia en el `main.js` todas las variables `var` por `let`.

  4.2 Lanzamos `npm run babel`

- Ahora la app no funciona, si vemos el error en la consola vemos: `monthlyRate is not defined`.

  4.3 Lo solucionamos declarando `monthlyRate` fuera del ``ìf```.

  4.4 Lanzamos `npm run babel`

- Ahora comprobamos que la app vuelve a funcionar correctamente.

# 5\. Uso de la Desestructuración

- ECMA6 introduce una nueva sintaxis que hace fácil crear objetos basados en variables. También a la inversa, la nueva sintaxis nos permite crear variables basadas en objetos y arrays.
- Vamos a modificar la función `calculateMonthlyPayment` para que devuelva múltiples variables.

  5.1 Crear Objetos desde Variables.

  5.1.1 Modificamos el `return` de la siguiente manera:

  ```javascript
  return { principal, years, rate, monthlyPayment, monthlyRate };
  ```

  - Al pasarlo por Babel:

    ```javascript
    return {
      principal: principal,
      years: years,
      rate: rate,
      monthlyPayment: monthlyPayment,
      monthlyRate: monthlyRate
    };
    ```

  5.2 Crear Variables desde un Objeto usando Desestructuración

  5.2.1 Añadimos en el `ìndex.html`:

  ```html
  <h2>Monthly Payment: <span id="monthlyPayment" class="currency"></span></h2>
  <h3>Monthly Rate: <span id="monthlyRate"></span></h3>
  ```

  5.2.2 Modificamos en el `main.js`. En el evento del botón `calcBtn`, Modificamos la llamada a `calculateMonthlyPayment` de la siguiente manera:

  ```javascript
  let { monthlyPayment, monthlyRate } = calculateMonthlyPayment(principal, years, rate);
  ```

  5.2.3 Añadimos en la útlima línea del `calcBtn` añadimos:

  ```javascript
  document.getElementById("monthlyRate").innerHTML = (monthlyRate * 100).toFixed(2);
  ```

  5.3 Construír y lanzamos el Proyecto: -Lanzamos `npm run babel` y lo comprobamos en el navegador.

# 6\. Usando las funciones con flechas `=>`

- La sintaxis de la función de ECMA6 con flecha es una forma corta de la sintaxis de una función en ECMA5\. El valor the `this` dentro de la función no se altera, es el mismo valor de `this`fuera de la función. No se necesita hacer `var self = this;`para mantener el actual 'scope'.

  6.1\. En `main.js` Debajo de la functión `calculateMonthlyPayment`, añadimos la función `calculateAmortization` definida de la siguiente manera:

  ```javascript
  let calculateAmortization = (principal, years, rate) => {
    let {monthlyRate, monthlyPayment} = calculateMonthlyPayment(principal, years, rate);
    let balance = principal;
    let amortization = [];
    for (let y=0; y<years; y++) {
        let interestY = 0;  //Interest payment for year y
        let principalY = 0; //Principal payment for year y
        for (let m=0; m<12; m++) {
            let interestM = balance * monthlyRate;       //Interest payment for month m
            let principalM = monthlyPayment - interestM; //Principal payment for month m
            interestY = interestY + interestM;
            principalY = principalY + principalM;
            balance = balance - principalM;
        }
        amortization.push({principalY, interestY, balance});
    }
    return {monthlyPayment, monthlyRate, amortization};
  };
  ```

  6.2\. Modificamos la sintaxis de la función `calculateMonthlyPayment`:

  ```javascript
  let calculateMonthlyPayment = (principal, years, rate) => { ... }
  ```

  6.3\. Modificamos la sintaxis del manejador del evento 'click' `calcBtn`:

  ```javascript
  document.getElementById('calcBtn').addEventListener('click', () => { ... });
  ```

  6.4 En el manejador del evento 'click' `calcBtn`, invocamos a `calculateAmortization` en lugar de `calculateMonthlyPayment`:

  ```javascript
  let { monthlyPayment, monthlyRate, amortization } = calculateAmortization(principal, years, rate);
  ```

  6.5\. En la última línea del manejador del evento 'click' `calcBtn`, mostramos el dato de `amortization` por la consola:

  ```javascript
  amortization.forEach(month => console.log(month));
  ```

- Debe quedar así:

  ```javascript
  document.getElementById('calcBtn').addEventListener('click', () => {
    let principal = document.getElementById("principal").value;
    let years = document.getElementById("years").value;
    let rate = document.getElementById("rate").value;
    let {monthlyPayment, monthlyRate, amortization} = calculateAmortization(principal, years, rate);
    document.getElementById("monthlyPayment").innerHTML = monthlyPayment.toFixed(2);
    document.getElementById("monthlyRate").innerHTML = (monthlyRate * 100).toFixed(2);
    amortization.forEach(month => console.log(month));
  });
  ```

  6.6\. Lanzamos el `npm run babel` para reconstruir el Proyecto.

# 7\. Configuración de Webpack.

- En JavaScript los módulos estan disponibles a través de librerías de terceros. ECMA6 añade soporte nativo para módulos. Cuando compilamos una App modular en ECMA6 a ECMA5, el compilador se basa en biblioteca de terceros para implementar módulos en ECMA5\. Webpack y Browserify son dos de las opciones más populares, Babel soporta ambas (y otras). Vamos a usar Webpack.

  7.1\. Configuración de Webpack

  7.1.1\. Instalamos babel-loader y webpack con npm:

  ```javascript
  npm install babel-loader webpack --save-dev
  ```

  7.1.2 Abrimos el **package.json** y añadimos el script **webpack**

  ```javascript
  "scripts": {
    "babel": "babel --presets es2015 js/main.js -o build/main.bundle.js",
    "start": "http-server",
    "webpack": "webpack"
  }
  ```

  7.1.3 Creamos el archivo de configuración de `webpack.config.js`en la raíz del Proyecto con el siguiente código:

  ```javascript
  var path = require('path');
   var webpack = require('webpack');

   module.exports = {
       entry: './js/main.js',
       output: {
           path: path.resolve(__dirname, 'build'),
           filename: 'main.bundle.js'
       },
       module: {
           loaders: [
               {
                   test: /\.js$/,
                   loader: 'babel-loader',
                   query: {
                       presets: ['es2015']
                   }
               }
           ]
       },
       stats: {
           colors: true
       },
       devtool: 'source-map'
   };
  ```

  7.2 Construir el Proyecto usando Webpack

  7.2.1 Lanzamos el siguiente comando `npm run webpack`

## 8. Uso de Módulos

  8.1\. Crear Módulo

  8.1.1\. Creamos un nuevo archivo `js/mortgage.js`

  8.1.2\. Copiamos las funciones `calculateMonthlyPayment` `calculateAmortization`del `main.js` dentro de `mortgage.js`

  8.1.3\. Añadimos la palabra reservada `export` antes de las funciones para hacerlas disponibles como parte de la API pública. `mortgage.js` debe acabar así:

  ```javascript
   export let calculateMonthlyPayment = (principal, years, rate) => {
       let monthlyRate = 0;
       if (rate) {
           monthlyRate = rate / 100 / 12;
       }
       let monthlyPayment = principal * monthlyRate / (1 - (Math.pow(1/(1 + monthlyRate),
               years * 12)));
       return {principal, years, rate, monthlyPayment, monthlyRate};
   };

   export let calculateAmortization = (principal, years, rate) => {
       let {monthlyRate, monthlyPayment} = calculateMonthlyPayment(principal, years, rate);
       let balance = principal;
       let amortization = [];
       for (let y=0; y<years; y++) {
           let interestY = 0;  //Interest payment for year y
           let principalY = 0; //Principal payment for year y
           for (let m=0; m<12; m++) {
               let interestM = balance * monthlyRate;       //Interest payment for month m
               let principalM = monthlyPayment - interestM; //Principal payment for month m
               interestY = interestY + interestM;
               principalY = principalY + principalM;
               balance = balance - principalM;
           }
           amortization.push({principalY, interestY, balance});
       }
       return {monthlyPayment, monthlyRate, amortization};
   };
  ```

  8.2\. Usar un Módulo

  8.2.1\. En el `main.js`, borramos las funciones `calculateMonthlyPayment` y `calculateAmortization`.

  8..2.2\. Añade la sentencia `import` como la primera línea del `main.js` para importar el Módulo `mortgage.js`

  ```javascript
   import * as mortgage from './mortgage';
  ```

  8.2.3\. En el 'calcBtn' modificamos la llamada a la función `calculateAmortization`:

  ```javascript
   let {monthlyPayment, monthlyRate, amortization} = mortgage.calculateAmortization(principal, years, rate);
  ```

  8.3\. Construimos y lanzamos: `npm run webpack`

## 9. Uso de Clases

  - ECMA6 introduce el concepto de Clase disponible tradicionalmente en los lenguajes orientados a Objetos. En ECMA6, la sintaxis de la Claseesta en la cima el existente prototipo basado en herencia. No agrega un nuevo modelo de herencia orientado a Objetos JavaScript.

  9.1\. Usando una Clase.

    9.1.1. En el ```main.js```, borra la sentencia ```import```.

    9.1.2. Añadimos la siguiente definición de Clase:
    ```javascript
