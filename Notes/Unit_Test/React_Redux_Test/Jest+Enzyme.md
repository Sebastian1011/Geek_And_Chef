# Jest + Enzyme
> demo: https://github.com/Sebastian1011/jest-enzyme-react-test

### the series

* project structure
* setup jest and enzyme
* mock data
* test component and connected component
* test redux

---

### Project structure

* React 16.X
* Redux
* Saga
* React-Router 4.X
* Typescript 3.X


![Project structure](../../../assets/images/jest-enzyme-project.png)

### setup jest and enzyme

* Jest, a test runner
* Enzyme, a test utility for React from Airbnb
  
1. config jest:  `npm i -D jest @types/jest babel-jest babel-polyfill identity-obj-proxy`

   jest.config.js
   ```
   module.exports = {
       // auto clear mock calls and instances between every test
       clearMocks: true,
       // the directory where jest should output its coverage files
       coverageDirectory: "<rootDir>/reports/coverage",
       // An array of regexp pattern strings used to skip coverage collection
       coveragePathIgnorePatterns: ["/node_modules/", "configs"],
       // Use this configuration option to add custom reporters to Jest
       reports: [
           "default",
           [
               "./node_modules/jest-html-reporter",
               {
                   pageTitle: "Report name",
                   outputPath: "reports/index.html",
                   includeFailureMsg: true,
                   customScriptPath: "report-script.js"
               }
           ]
       ],
       // An array of regexp pattern strings that are matched against all test paths, matched tests are skipped
       testPathIgnorePatterns: [
           "/node_modules/", "configs"
       ],
       // test root path
       roots: ["tests", "src"],
       // setup jest 
       setupFiles: ["raf/polyfill", "<rootDir>/configs/test-shim.js"],
       setupTestFrameworkScriptFile: "<rootDir>/configs/test-setup.ts",
       // transform typescript
       transform: {
           ".(ts|tsx)":"<rootDir>/node_modules
       },
       // An array of test regex strings
       testMatch: [
           "<rootDir>/tests/**/*.(test|spec).+(ts|tsx|js)",
           "<rootDir>/**/*.(test|spec).+(ts|tsx|js)
       ],
       // proxy to load scss css
       moduleNameMapper: {
           	"\\.(jpg|jpeg|png|gif|eot|otf|webp|svg|ttf|woff|woff2|mp4|webm|wav|mp3|m4a|aac|oga)$":
			"<rootDir>/jest/assetsTransformer.js",
		    "\\.(css|less|scss|sass)$": "identity-obj-proxy"
       },
       moduleFileExtensions: ["ts", "tsx", "js"],
       moduleDirectories: ["scr", "tests", "node_modules"]

   }
   ```

after test, a coverage report and test report will export to `./reports` 

2. config enzyme: `npm i -D enzyme @types/enzyme enzyme-adapter-react-16 @types/enzyme-adapter-react-16 enzyme-to-json`

3. setup typescript: `npm i -D typescript`
   tsconfig.json
   ```
   {
       "compilerOptions": {
           "baseUrl": "./",
           "outDir": "./build",
           "sourceMap": true,
           "noImplicitAny": true,
           "removeComments": true,
           "allowSyntheticDefaultImports": true,
           "experimentalDecorators": true,
           "emitDecoratorMetadata": true,
           "esModuleInterop": true,
           "strictPropertyInitialization": false,
           "module": "es6",
           "target": "esnext",
           "jsx": "react",
           "pretty": true,
           "moduleResolution": "node",
           "strict": true,
           "skipLibCheck": true,
           "paths": {
                "redux": ["typings/redux.d.ts"]
            },
            "lib": [
                "dom",
                "es2016",
                "es2017"
            ]
        },
        "include": [
            "./src/**/*", "tests/config.ts"
        ],
        "exclude": [
            "node_modules",
            "build",
            "build-dev",
            "configs",
            "script"
        ]
   }
   ```
4. setup babel: `npm i -D babel-preset-env babel-preset-es2015 babel-preset-latest babel-preset-react babel-preset-state-0 babel-runtime babel-plugin-import babel-plugin-transform-decorators-legacy babel-plugin-transform-runtime react-hot-loader`

   .babelrc
   ```
   {
    [
        "env",
        {
            "targets": {
            "browsers": "> 1%",
            "uglify": true
            },
            "useBuiltIns": true,
            "modules": false
        }
        ],
        "latest",
        "es2015",
        "react",
        "stage-0",
        "es2017"
    ],
    "retainLines": true,
    "plugins": [
        "react-hot-loader/babel",
        "transform-runtime",
        "transform-decorators-legacy",
        "transform-class-properties"
    ]
   }
   ```


### mockStore

```
    import { createStore, Store, combineReducer } from 'redux';
    import { reducer1, reducer2, reducer3 } from '../scr/reducers/rootReducer';

    export const configureStore = (preloadState?: any): Store => {
        return createStore(combineReducer({
            reducer1,
            reducer2,
            reducer3
        }), preloadState);
    }

    const data = {
        ...
    }

    export const mockStore = configureStore(data);

    export default  mockStore;
```