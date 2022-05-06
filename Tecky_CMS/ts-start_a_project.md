# Typescript Project Setup

&nbsp;

- create project folder

&nbsp;

- init yarn project

```bash
yarn init -y

```

&nbsp;

- create and configure `.prettierrc`

```text
{
    "trailingComma": "es5",
    "tabWidth": 4,
    "semi": false,
    "singleQuote": true,
    "overrides": [
        {
            "files": ["*.ts", "*.js"],
            "options": {
                "semi": true,
                "tabWidth": 2,
                "singleQuote": false,
                "printWidth": 100
            }
        }
    ]
}

```

&nbsp;

- install ts related packages

```bash
yarn add ts-node @types/node typescript

```

&nbsp;

- create and configure `tsconfig.json`

```json
{
  "compilerOptions": {
    "module": "commonjs",
    "target": "es5",
    "lib": ["es6", "dom"],
    "sourceMap": true,
    "allowJs": true,
    "jsx": "react",
    "esModuleInterop": true,
    "moduleResolution": "node",
    "noImplicitReturns": true,
    "noImplicitThis": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "suppressImplicitAnyIndexErrors": true,
    "noUnusedLocals": true
  },
  "exclude": ["node_modules", "build", "scripts", "index.js"]
}
```

&nbsp;

- create and configure `index.js`

```js
require("ts-node/register");
require("./main");
```

&nbsp;

- create and configure `main.ts`

```ts
console.log("hi");
```

&nbsp;

- create and configure `.gitignore`

```text
node_modules
.DS_Store

.env

```

&nbsp;
