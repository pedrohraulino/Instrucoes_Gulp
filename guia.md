**Guia Detalhado para Configurar Gulp:**

### **Passo 1:** Inicialize o Projeto NPM
Para começar, execute o seguinte comando para inicializar um novo projeto npm:

```bash
npm init --yes
```

Este comando criará um arquivo `package.json` com as configurações padrão.

### **Passo 2:** Instale Gulp e Plugins
Instale o Gulp e os plugins necessários para processar Sass:

```bash
npm install --save-dev gulp gulp-sass sass
```

### **Passo 3:** Configure o `.gitignore`
Evite que as pastas `node_modules` e `dist` sejam versionadas. Adicione-as ao arquivo `.gitignore`.

### **Passo 4:** Crie o Arquivo `gulpfile.js`
Crie um arquivo chamado `gulpfile.js` na raiz do seu projeto.

### **Passo 5:** Adicione Tarefas ao `package.json`
Edite o arquivo `package.json` e adicione scripts para as tarefas de desenvolvimento e build:

```json
"scripts": {
  "dev": "gulp watch",
  "build": "gulp",
  "test": "echo \"Error: no test specified\" && exit 1"
}
```

### **Passo 6:** Estrutura de Pastas para Sass
Organize a estrutura de pastas para os arquivos Sass:

```
project
│   gulpfile.js
│   package.json
└───src
    └───styles
        └───main.scss
```

### **Passo 7:** Adicione Funções ao `gulpfile.js`
No `gulpfile.js`, adicione funções para processar os arquivos Sass:

```javascript
const gulp = require('gulp');
const sass = require('gulp-sass')(require('sass'));

function styles() {
    return gulp.src('./src/styles/*.scss')
        .pipe(sass({ outputStyle: 'compressed' }))
        .pipe(gulp.dest('./dist/css'));
}

exports.default = gulp.parallel(styles);
```

### **Passo 8:** Crie a Tarefa Watch
Para automatizar as atualizações, adicione uma tarefa de watch ao `gulpfile.js`:

```javascript
exports.watch = function() {
    gulp.watch('./src/styles/*.scss', gulp.parallel(styles));
}
```

Execute com:

```bash
npm run watch
```

### **Passo 9:** Use o Caminho Correto no `index.html`
Certifique-se de usar o caminho correto (`dist/css/main.css`) no arquivo `index.html` para a folha de estilos.

### **Passo 10:** Instale o Compressor de Imagens
Instale o gulp-imagemin para otimizar imagens:

```bash
npm install --save-dev gulp-imagemin@7.1.0
```

Adicione ao `gulpfile.js`:

```javascript
const imagemin = require('gulp-imagemin');

function images() {
    return gulp.src('./src/img/**/*')
        .pipe(imagemin())
        .pipe(gulp.dest('./dist/img'));
}

exports.default = gulp.parallel(styles, images);
```

### **Passo 11:** Comprima Código JavaScript
Instale o gulp-uglify para comprimir o código JavaScript:

```bash
npm install --save-dev gulp-uglify
```

Adicione ao `gulpfile.js`:

```javascript
const uglify = require('gulp-uglify');

function scripts() {
    return gulp.src('./src/scripts/*.js')
        .pipe(uglify())
        .pipe(gulp.dest('./dist/js'));
}

exports.default = gulp.parallel(styles, images, scripts);

// Adicione ao watch
gulp.watch('./src/scripts/*.js', gulp.parallel(scripts));
```

Agora, execute `npm run build` para construir o projeto e `npm run watch` para assistir a alterações automaticamente. Seu ambiente Gulp está pronto para otimizar Sass, imagens e JavaScript em seu projeto.

O seu arquivo gulp deve ter essa estrutura apos todas as configurações:

```javascript
const gulp = require('gulp');
const sass = require('gulp-sass')(require('sass'));
const imagemin = require('gulp-imagemin');
const uglify = require('gulp-uglify');

function styles() {
    return gulp.src('./src/styles/*.scss')
        .pipe(sass({ outputStyle: 'compressed' }))
        .pipe(gulp.dest('./dist/css'));
}

function images() { 
    return gulp.src('./src/img/**/*')
        .pipe(imagemin())
        .pipe(gulp.dest('./dist/img'));
}

function scripts() {
    return gulp.src('./src/scripts/*.js')
        .pipe(uglify())
        .pipe(gulp.dest('./dist/js'));
}

exports.default = gulp.parallel(styles, images, scripts);

exports.watch = function() {
    gulp.watch('./src/styles/*.scss', gulp.parallel(styles));
    gulp.watch('./src/scripts/*.js', gulp.parallel(scripts));
}
