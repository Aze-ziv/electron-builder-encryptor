# electron-builder-encryptor [![npm](https://img.shields.io/npm/v/electron-builder-encryptor.svg)](https://npmjs.com/package/electron-builder-encryptor)

[![ci](https://github.com/dusionlike/electron-builder-encryptor/actions/workflows/ci.yml/badge.svg)](https://github.com/dusionlike/electron-builder-encryptor/actions/workflows/ci.yml)

English | [简体中文](./README-zh-CN.md)

simple electron package encryption tool

## Examples

- [electron-builder-encryptor-demo](https://github.com/dusionlike/electron-builder-encryptor/tree/main/playground)

## Features

- 🐵 Very easy to use, just add a few lines of code
- 💚 Framework agnostic, easy to add to your existing projects
- 🤷‍♂️ It only takes effect when packaging with `electron-builder`, and does not affect development and debugging
- 🔒 Use [bytenode](https://github.com/bytenode/bytenode) to encrypt the main process and custom method to encrypt the renderer process
- 👀 Prevent tampering with the `app.asar` file

## Usage

```bash
npm i electron-builder-encryptor -D

# These 4 libraries need to be added to the project
npm i adm-zip
npm i bytenode
npm i mime
npm i original-fs
```

Add `afterPack` to `electron-builder` configuration

```json5
// package.json
{
    "build": {
        "asar": true,
        "afterPack": "node_modules/electron-builder-encryptor"
    }
}
```

or

```json5
// electron-builder.json
{
    "asar": true,
    "afterPack": "node_modules/electron-builder-encryptor"
}
```

in mian process

```js
if (!app.isPackaged) {
    // address before packing
    mainWindow.loadFile('renderer/index.html')
} else {
    // address after packing
    mainWindow.loadURL('myclient://apps/index.html')
}
```

> Note: The renderer process that needs to be encrypted must be placed in the `renderer` folder under the directory where the entry file `main.js` is located

## Configuration

```ts
// encryptor.config.ts or encryptor.config.js
import { defineConfig } from 'electron-builder-encryptor'

export default defineConfig({
    /**
     * encryption key
     */
    key: 'xxx000777',
})
```

all configuration

```ts
export declare interface UserConfig {
    /**
     * encryption key
     */
    key?: string
    /**
     * renderer protocol scheme
     * @default 'myclient'
     */
    protocol?: string
    /**
     * electron custom schemes to be registered with options.
     * @default
     * {standard: true, secure: true, bypassCSP: true, allowServiceWorkers: true, supportFetchAPI: true, corsEnabled: true, stream: true}
     */
    privileges?: Privileges
    /**
     * The encrypted storage path of the rendering process, with the program execution directory as the root node
     */
    rendererOutPath?: string
}
```

## License

[MIT](./LICENSE) License
