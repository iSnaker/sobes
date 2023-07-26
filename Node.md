## NodeJs

### В чем разница между setTimeout vs process.nexttick и setImmediate?
setImmediate(cb) выполняет callback перед остальными событиями в очереди, по этому оно выполнится раньше
setTimeout(fn,0). process.nextTick() будет обработан выполнения текущего события, не смотря на состояния
фазы очереди событий(event loop).

    setTimeout(() => console.log('setTimeout'), 0)
    setImmediate(() => console.log('setImmediate'))
    process.nextTick(() => console.log('nextTick'))
    // will print nextTick setTimeout setImmediate



### Как автоматически перезапустить сервер при сбое или перезагрузке системы?
Чтобы автоматически перезапустить сервер после перезагрузке системы используйте Linux утилиту  'upstart' .
Чтобы автоматически перезапустить сервер после сбоя используйте forever или pm2.


### В чем разница между Node.js и JavaScript?
Node.js - это интерпретатор, а также среда для JavaScript, которая в основном используется для доступа или
реализации любых неблокирующих процедур для любого типа ОС. Здесь работает движок Google Chrome.
Принимая во внимание, что JavaScript - это язык программ, который используется для любых задач на стороне клиента
для интернет-приложений. Здесь работает движок Firefox, Safari, Google Chrome и т. Д.

