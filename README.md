## ES6 Tutorial

Tutorial ES5 a ES6

1. Instalar la App de ejemplo:
```git clone https://github.com/kotehector/ES5-to-ES6-tutorial.git```

2. Configuración de Babel:

  2.1 Crear package.json en el root del proyecto. Lanzamos ```npm init```

  2.2 Instalamos babel-cli y babel-core lanzando:
```npm install babel-cli babel-core --save-dev```

  2.3. Instalamos el preset para ECMAScript 2015:
```npm install babel-preset-es2015 --save-dev```

  2.4. Instalamos un pequeño servidor local:
```npm install http-server --save-dev```

  2.5. Editamos el ```package.json``` la sección ```scripts: {}``` y le añadimos:
  ```javascript
  "scripts": {
    "babel": "babel --presets es2015 js/main.js -o build/main.bundle.js",
    "start": "http-server"
  }
  ```
  2.6. Creamos la carpeta ```build```en el root.

3. Construír proyecto y lanzar:

  3.1 Lanzar babel y compilar el main.js
  ```npm run babel```.

  3.2 Substituimos en el ```ìndex.html``` el ```js/main.js``` por el archivo que acabamos de crear: ```build/main.bundle.js``` con ```babel```.

  3.3 Comparamos el ```js/main.js``` con ```build/main.bundle.js```y vemos que de momento no hay ningún cambio porque en el ```js/main.js``` no hay nada con ECMA6.
