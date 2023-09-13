# Deprecated

You can now easily achieve the same result by using Electron
[`ipcMain.handle` and `ipcRenderer.invoke`](https://www.electronjs.org/docs/latest/api/ipc-renderer#ipcrendererinvokechannel-args).

# electron-ipcp

Easier asynchronous communication on top of Electron ipc communication using promises.

`ipc` + `promise` => `ipc + p` => `ipcp`.

## Problem being solved

Sometimes you just need to ask something to the `main process` and wait for the response.  
`electron-ipcp` makes this task easier.

## Install

If you use the [two package.json structure](https://www.electron.build/tutorials/two-package-structure), it might look like this:

- install as devDependency in `package.json`
- install as dependency in `app/package.json`

## Usage

### In renderer process

```js
import { ipcpRenderer } from 'electron-ipcp'

// async/await style
async function askSomethingToMain() {
  const sumFromMain = await ipcpRenderer.sendMain('sumInMain', 1, 2)
}

// or promise style
function askSomethingToMain() {
  ipcpRenderer.sendMain('sumInMain', 1, 2).then((sumFromMain) => {})
}
```

### In main process

```js
const { ipcpMain } = require('electron-ipcp')

ipcpMain.on('sumInMain', (event, a, b) => {
  event.respond(a + b)
})
```

### How it works

A randomly named communication channel is created and the module provides a `respond` method that use the same channel.

If you need to access the original `electron event`, it is available.

```js
event = {
  respond, //method to respond to renderer
  originalEvent, // the original electron event
}
```
