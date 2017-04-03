# api documentation for  [app-module-path (v2.2.0)](https://github.com/patrick-steele-idem/app-module-path-node)  [![npm package](https://img.shields.io/npm/v/npmdoc-app-module-path.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-app-module-path) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-app-module-path.svg)](https://travis-ci.org/npmdoc/node-npmdoc-app-module-path)
#### Simple module to add additional directories to the Node module search for top-level app modules

[![NPM](https://nodei.co/npm/app-module-path.png?downloads=true)](https://www.npmjs.com/package/app-module-path)

[![apidoc](https://npmdoc.github.io/node-npmdoc-app-module-path/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-app-module-path_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-app-module-path/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-app-module-path/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-app-module-path/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Patrick Steele-Idem",
        "email": "pnidem@gmail.com"
    },
    "bugs": {
        "url": "https://github.com/patrick-steele-idem/app-module-path-node/issues"
    },
    "dependencies": {},
    "description": "Simple module to add additional directories to the Node module search for top-level app modules",
    "devDependencies": {
        "chai": "^3.5.0",
        "mocha": "^3.2.0"
    },
    "directories": {},
    "dist": {
        "shasum": "641aa55dfb7d6a6f0a8141c4b9c0aa50b6c24dd5",
        "tarball": "https://registry.npmjs.org/app-module-path/-/app-module-path-2.2.0.tgz"
    },
    "ebay": {},
    "gitHead": "e0942e24b37ab7e7259ea4c822d157d8d8aa3787",
    "homepage": "https://github.com/patrick-steele-idem/app-module-path-node",
    "keywords": [
        "modules",
        "path",
        "node",
        "extend",
        "resolve"
    ],
    "license": "BSD-2-Clause",
    "main": "lib/index.js",
    "maintainers": [
        {
            "name": "mlrawlings",
            "email": "ml.rawlings@gmail.com"
        },
        {
            "name": "philidem",
            "email": "phillip.idem@gmail.com"
        },
        {
            "name": "pnidem",
            "email": "pnidem@gmail.com"
        }
    ],
    "name": "app-module-path",
    "optionalDependencies": {},
    "publishConfig": {
        "registry": "https://registry.npmjs.org/"
    },
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git+https://github.com/patrick-steele-idem/app-module-path-node.git"
    },
    "scripts": {
        "test": "node test/test.js && ./node_modules/mocha/bin/mocha test/test2.js"
    },
    "version": "2.2.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module app-module-path](#apidoc.module.app-module-path)
1.  [function <span class="apidocSignatureSpan">app-module-path.</span>addPath (path, parent)](#apidoc.element.app-module-path.addPath)
1.  [function <span class="apidocSignatureSpan">app-module-path.</span>enableForDir (dir)](#apidoc.element.app-module-path.enableForDir)
1.  [function <span class="apidocSignatureSpan">app-module-path.</span>removePath (path)](#apidoc.element.app-module-path.removePath)



# <a name="apidoc.module.app-module-path"></a>[module app-module-path](#apidoc.module.app-module-path)

#### <a name="apidoc.element.app-module-path.addPath"></a>[function <span class="apidocSignatureSpan">app-module-path.</span>addPath (path, parent)](#apidoc.element.app-module-path.addPath)
- description and source-code
```javascript
function addPath(path, parent) {
    // Anable app-module-path to work under any directories that are explicitly added
    enableForDir(path);

    function addPathHelper(targetArray) {
        path = nodePath.normalize(path);
        if (targetArray && targetArray.indexOf(path) === -1) {
            targetArray.push(path);
        }
    }

    path = nodePath.normalize(path);

    if (appModulePaths.indexOf(path) === -1) {
        appModulePaths.push(path);
        // Enable the search path for the current top-level module
        if (require.main) {
            addPathHelper(require.main.paths);
        }

        parent = parent || module.parent;

        // Also modify the paths of the module that was used to load the app-module-paths module
        // and all of it's parents
        while(parent && parent !== require.main) {
            addPathHelper(parent.paths);
            parent = parent.parent;
        }
    }
}
```
- example usage
```shell
...

'npm install app-module-path --save'

## Usage
'''javascript
// ***IMPORTANT**: The following line should be added to the very
//                 beginning of your main script!
require('app-module-path').addPath(baseDir);
'''

__IMPORTANT:__
The search path should be modified before any modules are loaded!

__Example:__
...
```

#### <a name="apidoc.element.app-module-path.enableForDir"></a>[function <span class="apidocSignatureSpan">app-module-path.</span>enableForDir (dir)](#apidoc.element.app-module-path.enableForDir)
- description and source-code
```javascript
function enableForDir(dir) {
    allowedDirs[dir] = true;
}
```
- example usage
```shell
...

## Explicitly enabling a directory/package

By default, 'app-module-path' will not attempt to resolve app modules from a directory that is found to be within a 'node_modules
' directory. This behavior can be changed by explicitly enabling 'app-module-path' to work for descendent modules of a specific
directory. For example:

'''javascript
var packageDir = path.dirname(require.resolve('installed-module-allowed'));
require('../').enableForDir(packageDir);
'''


### ES5

'''javascript
require('app-module-path/register');
...
```

#### <a name="apidoc.element.app-module-path.removePath"></a>[function <span class="apidocSignatureSpan">app-module-path.</span>removePath (path)](#apidoc.element.app-module-path.removePath)
- description and source-code
```javascript
function removePath(path) {
    function removePathHelper(targetArray) {
        path = nodePath.normalize(path);
        if (!targetArray) return;
        var index = targetArray.indexOf(path);
        if (index === -1) return;
        targetArray.splice(index, 1);
    }

    var parent;
    path = nodePath.normalize(path);
    var index = appModulePaths.indexOf(path);

    if (index > -1) {
        appModulePaths.splice(index, 1);
        // Enable the search path for the current top-level module
        if (require.main) removePathHelper(require.main.paths);
        parent = module.parent;

        // Also modify the paths of the module that was used to load the app-module-paths module
        // and all of it's parents
        while(parent && parent !== require.main) {
            removePathHelper(parent.paths);
            parent = parent.parent;
        }
    }
}
```
- example usage
```shell
n/a
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
