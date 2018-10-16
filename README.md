# hackathon_iu6_2018

### Введение

**Задача**: разобраться с основами построения клинт-серверного приложения.

В данном случае, клиент и сервер будут находиться в одной локальной сети.

Например, ноутбук и Raspberry Pi.

В качестве серверного узла выступает одноплатный компьютер Raspberry Pi (Node.js + Express),
в качестве клиента - браузер на ноутбуке (js + html).

### Часть 1. Установка Node.js на Raspberry Pi

Для начала необходимо по VNC подключиться к Pi.

Далее, откройте терминал и выполните команды для установки Node.js
```sh
sudo apt-get update
sudo apt-get install nodejs
```

Проверьте, установились ли пакеты
```sh
akenoq@AkenoqPC:~$ node -v
v8.4.0
akenoq@AkenoqPC:~$ npm -v
5.3.0
```

Если пакет `npm` (Node.js Package Manager) не установлен и не показывает версию,
то необходимо его установить отдельно командой `sudo apt-get install npm`

### Часть 2. Запуск hello-сервера на Raspberry Pi

1. Создайте папку `hello`
2. Перейдите в нее и выполните команду `npm init` (все, что предложит, оставлять по дефолту)
В вашей папке появился файл `package.json`, в нем будут храниться зависимости проекта и параметры запуска.
3. Далее необходимо установить веб-фреймворк `Express` для обработки запросов, маршрутизации и раздачи статики.
`npm install express --save` (`--save` сохранит зависимость в `package .json`, в нем появилась строчка `"dependencies": {"express": "^4.16.4"}`)

```json
{
  "name": "hello",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.16.4"
  }
}
```
4. Создайте файл с кодом сервера `index.js`

```js
"use strict";

// подключаем фреймвок express
let express = require('express');
let app = express();

// разрешаем междоменные запросы
app.use(function(req, res, next) {
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
    res.header("Cache-Control", "no-cache, no-store, must-revalidate");
    next();
});

// hello-GET, вывод сообщения в браузере 'Hello Pi'
app.get('/api/hello', function (req, res) {
    res.send('Hello Pi');
});

// слушаем на порту 5055 или другом свободном
let port = process.env.PORT || 5055;
app.listen(port);
console.log("Server works on port " + port);
```
5. Выполните команду для запуска скрипта `node index.js` (в терминале должно появиться сообщение, что сервер работает)
6. Чтобы проверить работу, в браузере перейдите по адресу `http://localhost:5005/api/hello`.
Должно появиться сообщение `Hello Pi`

> Можно в `package.json` добавить скрипт `"start":"node index.js"` и запускать приложение командой `npm start`

> Чтобы выключить сервер, в его терминале используйте сочетание клавиш `Ctrl + C`

### Подготовка к запуску стартовых приложений

Запуск стартовых приложений на клиенте (ноутбук с браузером) и сервере (Raspberry Pi).

На клиентском устройстве необходимо повторить предыдущие шаги из Части 1 и Части 2 (1-3 пункты),
так как в данном случае на клиенте надо будет иметь сервер со статикой для веб-интерфейса.

Скачать примеры из репозитория на клиентский и серверный узел `https://github.com/akenoq/hackathon_iu6_2018.git`

На Raspberry Pi понадобится папка `server`, на клиенте - `client`

##### Raspberry Pi
1. На Raspberry Pi перейдите в папку `server`
2. Для установки зависимостей выполните `npm install`
3. Для запуска сервера выполните `npm start`
4. Сервер готов к запросам

##### Ноутбук
1. На устройстве перейдите в папку `client`
2. Для установки зависимостей выполните `npm install`
3. Для запуска сервера выполните `npm start`
4. Для перехода к веб-интерфейсу в браузере перейдите по адресу сервера `http://localhost:5007/`

> Для тестирования веб-интерфейса поднимите и основной сервер на вашем компьютере

**Задание**: поменять url запросов и порты работы серверов таим образом,
чтобы запросы можно было пересылать с веб-интерфейса на сервер-обработчик Raspberry Pi