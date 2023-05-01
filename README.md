## installation
clone this repository by running `git clone https://github.com/Flursh/tweego-vite-boilerplate.git` and run `npm install`

## structure
```
├── assets/
├── src/
│   ├── css/
│   │    ├── *.css
│   ├── js/
│   │    ├── *.js
│   │    ├── *.ts
│   ├── main.ts
│   ├── vite-env.d.ts
├── story/
│   ├── dist/
│   ├── modals/
│   ├── passages/
│   ├── setup/
│   ├── UI/
│   ├── head-content.txt
├── tsconfig.json
├── assets/vite.config.js
├── package.json
```
> **Warning**
> for **windows** users, the 'open' command will not work, you will have to modify the package.json file `scripts` section with :
```json
"openStory": "explorer \"index.html\"",
"openTest": "explorer \"test.html\"",
```

### assets
story any static files you want to have access to from your story, like images for example. You link to them from your twee files relatively from the generate file, like `./assets/pie.jpg`

### src
all your pre-compiled `js` and `ts` files should go in the `js` folder and css files should go in `css`. They will get compiled (including compiling typescript) and minified in the `/story/dist/` folder under `/css/style-[hash].css` and `/js/main-[hash].js`;

In order for a file to be compiled, it needs to be imported in `main.ts` or in another file imported in `main.ts`. The order in which the files are imported will be reflected in the output file.

> **Warning**
> Anything that isn't explicitely imported will not be compiled and therefore will not be accessible in the story!

```ts
// main.ts

// JS
import './js/setup';
import './js/inserts';
import './js/inventory';
import './js/saves';
import './js/dialog';

// CSS
import './css/choices.css';
import './css/dialog.css';
import './css/styles.css';
import './css/UI.css';
```

## Commands

```json
{
  "scripts": {
    "start": "npm run build",
    "openStory": "open ./index.html",
    "openTest": "open ./test.html",
    "test": "npm run compile && tweego -o ./test.html ./story -t --head=\"./story/head-content.txt\" && npm run openTest",
    "test:watch": "concurrently \"npm run compile:watch\" \"tweego -o ./test.html ./story --head=\"./story/head-content.txt\" -w -t\" \"npm run openTest\"",
    "build:watch": "concurrently \"npm run compile:watch\" \"tweego -o ./test.html ./story --head=\"./story/head-content.txt\" -w\" \"npm run openStory\"",
    "build": "npm run compile && tweego -o ./index.html ./story --head=\"./story/head-content.txt\" && npm run openStory",
    "compile": "tsc && vite build",
    "compile:watch": "tsc && vite build --watch"
  }
}
```

### build story file
`npm start` : the html file can be found in index.html in the root of the folder

### Test and watch for changes
`npm run test:watch` : recompiles on file change in the `story` and `src` folders. The test file is in test.html

### All commands

- `npm start` : alias of `npm run build`
- `npm run openStory` : opens the `index.html` file from a successful build
- `npm run openTest` : opens the `test.html` file from a successful test
- `npm run test` : 
    - compiles the data in `/src`
    - compiles the `/story` folder to `test.html` in test mode
    - opens `test.html` in a new page in the default browser
- `npm run test:watch` : same as `test` but recompiles on change in either `/story` or `/src`
- `npm run build` : 
    - compiles the data in `/src`
    - compiles the `/story` folder to `index.html`
    - opens `index.html` in a new page in the default browser
- `npm run build:watch` : same as `build` but recompiles on change in either `/story` or `/src`
- `npm run compile` : compiles the `/src` folder
- `npm run compile:watch` : compiles the `/src` folder and eveytime its content changes

### using SASS
To use sass (or stylus, or Less, see the vite documentation), just add the package as a dev dependency with `npm install sass -D` and you can start using `.scss` files, remember that anything that isn't imported will not be available in the compiled css.

`page.scss` :
```scss
@import './footer';

#page {
    padding: 1em 0;
    & header, & article, & footer {
        padding: .5em 2em;
    }
}
```
`main.ts` :
```ts
// css
import "./page.scss";
// continue
```