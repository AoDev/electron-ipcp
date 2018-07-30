# electron-ipcp
Easier asynchronous communication on top of Electron ipc communication using promises.

`ipc` + `promise` => `ipc + p` => `ipcp`.

## Problem being solved

Sometimes you just need to ask something to the `main process` and wait for the response.  
`electron-ipcp` makes this task easier.


## Usage

### In renderer process

```js
import {ipcpRenderer} from 'electron-ipcp'

// async/await style
async function askSomethingToMain () {
  const sumFromMain = await ipcpRenderer.sendMain('sumInMain', 1, 2)
}

// or promise style
function askSomethingToMain () {
  ipcpRenderer.sendMain('sumInMain', 1, 2)
    .then((sumFromMain) => { })
}
```

### In main process

```js
const {ipcpMain} = require('electron-ipcp')

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
