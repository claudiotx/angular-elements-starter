# AngularElements Boilerplate

This project can be used as a basis to start any angular elements project.
Enterprise Grade.
You will build a library that works within our Angular applications, and also outside of them.
The library will be usable in multiple different projects

## Trivia

Create an Angular Component and use it anywhere, i.e. Vanilla JS and HTML, Angular, React, Vue, etc.
Distribute to NPM your components/libraries/packages in a framework agnostic way.
The NPM Package will both:
a) follow Angular package standards
b) bundled as framework-agnostic bundle able to be used anywhere

## File Structure

Two main folders: `projects` and `src`.

```
├── angular.json **workspace configuration, build configurations for each library in ./projects/**
├── package.json
├── projects/ **all libraries**
│   └── library1/
│       ├── CHANGELOG.md
│       ├── package.json
│       ├── src/
│       │   ├── elements/ **code responsible for using and building the library code with Angular elements.**
│       │   │   ├── environments/
│       │   │   │   ├── environment.prod.ts
│       │   │   │   └── environment.ts
│       │   │   ├── index.html
│       │   │   ├── index.ts
│       │   │   ├── main.ts
│       │   │   ├── polyfills.ts
│       │   │   ├── library1.module.ts
│       │   │   └── public_api.ts
│       │   ├── index.ts
│       │   ├── lib/ **libraries source code**
│       │   │   ├── index.ts
│       │   │   ├── my-component/
│       │   │   └── library1.module.ts
│       │   └── public_api.ts
│       ├── tsconfig.elements.json
│       └── tsconfig.lib.json
├── src/ **testing application**
│   ├── app/
│   ├── environments/
│   ├── index.html
│   ├── main.ts
│   └── tsconfig.app.json
└── tsconfig.json
```

## Build Configurations (angular.json)

Angular Package Format (APF)

### NPM Library (projects/library1)

Project Type: "library"
Builder: ng-packagr
Own `tsconfig.lib.json`
Own AppModule

### Elements as an Angular Application

ProjectType and builder configurations are standard.
Own `tsconfig.elements.json`
Own AppModule ** for Angular elements-specific libraries, polyfills, and web-element definitions when you need them.

## Build
Build the Angular Elements Application
`ng build --prod library1-elements`

Library will make use of these files: runtime.js, polyfills.js, main.js

Script to concatenate the 3 files:

```js
const fs = require('fs-extra');
const concat = require('concat');

function handleErr(err) {
    if (err) {
        return console.error(err);
    }
    console.log('success!');
}

(async function build() {
  const library1Files = [
      './dist/library1-elements-tmp/runtime.js',
      './dist/library1-elements-tmp/polyfills.js',
      './dist/library1-elements-tmp/main.js'
    ];

  await fs.ensureDir('./dist/library1-client/elements');
  await concat(library1Files, './dist/library1-client/elements/library1.js');
  await fs.remove('./dist/library1-elements-tmp', handleErr);
})();
```

### Using the component
<my-component my-cool-input=”foo” …></my-component>

### Refs
[1] https://angular.io/guide/elements
[2] https://angular.io/guide/workspace-config#angular-workspace-configuration
[3] https://goo.gl/jB3GVv
[4] https://github.com/ng-packagr/ng-packagr
[5] https://github.com/angular/angular/issues/24556
