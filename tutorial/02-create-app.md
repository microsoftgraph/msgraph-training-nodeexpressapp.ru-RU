<!-- markdownlint-disable MD002 MD041 -->

В этом упражнении вы будете использовать [Express](http://expressjs.com/) для создания веб-приложения. Если вы еще не установили генератор Express, вы можете установить его из интерфейса командной строки (CLI) с помощью следующей команды.

```Shell
npm install express-generator -g
```

Откройте подсистему CLI, перейдите к каталогу, в котором у вас есть права на создание файлов, и выполните следующую команду, чтобы создать новое приложение Express, использующее [handlebars](http://handlebarsjs.com/) в качестве обработчика визуализации.

```Shell
express --hbs graph-tutorial
```

Генератор Express создает новый каталог с именем `graph-tutorial` и формированием шаблонов для приложения Express. Перейдите к новому каталогу и введите следующую команду, чтобы установить зависимости.

```Shell
npm install
```

После выполнения этой команды используйте следующую команду для запуска локального веб-сервера.

```Shell
npm start
```

Откройте браузер и перейдите по адресу `http://localhost:3000`. Если все работает, вы увидите сообщение "Добро пожаловать в Express". Если вы не видите это сообщение, ознакомьтесь с [руководством Express Приступая к началу работы](http://expressjs.com/starter/generator.html).

Прежде чем переходить, установите несколько дополнительных драгоценных камней, которые будут использоваться позже:

- [дотенв](https://github.com/motdotla/dotenv) для загрузки значений из файла env.
- [время для форматирования](https://github.com/moment/moment/) значений даты и времени.
- сообщение об ошибке [Connect-Flash](https://github.com/jaredhanson/connect-flash) to Flash в приложении.
- [Express — сеанс](https://github.com/expressjs/session) для хранения значений в серверном сеансе в памяти на стороне сервера.
- [Passport — Azure — AD](https://github.com/AzureAD/passport-azure-ad) для проверки подлинности и получения маркеров доступа.
- [Simple — OAuth2](https://github.com/lelylan/simple-oauth2) для управления маркерами.
- [Microsoft — Graph — клиент](https://github.com/microsoftgraph/msgraph-sdk-javascript) для совершения звонков в Microsoft Graph.
- [исоморфик — получение](https://github.com/matthew-andrews/isomorphic-fetch) для заполнения для узла. Для `microsoft-graph-client` библиотеки необходима получающая заливка. Для получения дополнительных сведений посетите [вики-сайт клиентской библиотеки JavaScript для Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) .

Выполните следующую команду в командной панели CLI.

```Shell
npm install dotenv@8.2.0 moment@2.24.0 connect-flash@0.1.1 express-session@1.17.0 isomorphic-fetch@2.2.1
npm install passport-azure-ad@4.2.0 simple-oauth2@3.1.0 @microsoft/microsoft-graph-client@2.0.0
```

> [!TIP]
> При попытке установить эти пакеты в Windows пользователи Windows могут получить следующее сообщение об ошибке.
>
> ```Shell
> gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
> ```
>
> Чтобы устранить эту ошибку, выполните следующую команду, чтобы установить средства построения Windows с помощью окна терминала с повышенными привилегиями (администратора), которое устанавливает инструменты VS Build и Python.
>
> ```Shell
> npm install --global --production windows-build-tools
> ```

Теперь обновите приложение, чтобы оно `connect-flash` использовалось по `express-session` промежуточного слоя. Откройте `./app.js` файл и добавьте приведенный ниже `require` оператор в начало файла.

```js
var session = require('express-session');
var flash = require('connect-flash');
```

Добавьте приведенный ниже код сразу после `var app = express();` строки.

```js
// Session middleware
// NOTE: Uses default in-memory session store, which is not
// suitable for production
app.use(session({
  secret: 'your_secret_value_here',
  resave: false,
  saveUninitialized: false,
  unset: 'destroy'
}));

// Flash middleware
app.use(flash());

// Set up local vars for template layout
app.use(function(req, res, next) {
  // Read any flashed errors and save
  // in the response locals
  res.locals.error = req.flash('error_msg');

  // Check for simple error string and
  // convert to layout's expected format
  var errs = req.flash('error');
  for (var i in errs){
    res.locals.error.push({message: 'An error occurred', debug: errs[i]});
  }

  next();
});
```

## <a name="design-the-app"></a>Проектирование приложения

Начните с создания глобального макета для приложения. Откройте `./views/layout.hbs` файл и замените все его содержимое приведенным ниже кодом.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Node.js Graph Tutorial</title>

    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.1/css/bootstrap.min.css"
      integrity="sha384-WskhaSGFgHYWDcbwN70/dfYBj47jz9qbsMId/iRN3ewGhXQFZCSftd1LZCfmhktB" crossorigin="anonymous">
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.1.0/css/all.css"
      integrity="sha384-lKuwvrZot6UHsBSfcMvOkWwlCMgc0TaWr+30HWe3a4ltaBwTZhyTEggF5tJv8tbt" crossorigin="anonymous">
    <link rel='stylesheet' href='/stylesheets/style.css' />
    <script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.3/umd/popper.min.js"
      integrity="sha384-ZMP7rVo3mIykV+2+9J3UJ46jBk0WLaUAdn689aCwoqbBJiSnjAK/l8WvCWPIPm49" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.1.1/js/bootstrap.min.js"
      integrity="sha384-smHYKdLADwkXOn1EmN1qk/HfnUcbVRZyYmZ4qpPea6sjB/pTJ0euyQp0Mk8ck+5T" crossorigin="anonymous"></script>
  </head>

  <body>
    <nav class="navbar navbar-expand-md navbar-dark fixed-top bg-dark">
      <div class="container">
        <a href="/" class="navbar-brand">Node.js Graph Tutorial</a>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarCollapse"
          aria-controls="navbarCollapse" aria-expanded="false" aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarCollapse">
          <ul class="navbar-nav mr-auto">
            <li class="nav-item">
              <a href="/" class="nav-link{{#if active.home}} active{{/if}}">Home</a>
            </li>
            {{#if user}}
              <li class="nav-item" data-turbolinks="false">
                <a href="/calendar" class="nav-link{{#if active.calendar}} active{{/if}}">Calendar</a>
              </li>
            {{/if}}
          </ul>
          <ul class="navbar-nav justify-content-end">
            <li class="nav-item">
              <a class="nav-link" href="https://developer.microsoft.com/graph/docs/concepts/overview" target="_blank">
                <i class="fas fa-external-link-alt mr-1"></i>Docs
              </a>
            </li>
            {{#if user}}
              <li class="nav-item dropdown">
                <a class="nav-link dropdown-toggle" data-toggle="dropdown" href="#" role="button" aria-haspopup="true"
                  aria-expanded="false">
                  {{#if user.avatar}}
                    <img src="{{ user.avatar }}" class="rounded-circle align-self-center mr-2" style="width: 32px;">
                  {{else}}
                    <i class="far fa-user-circle fa-lg rounded-circle align-self-center mr-2" style="width: 32px;"></i>
                  {{/if}}
                </a>
                <div class="dropdown-menu dropdown-menu-right">
                  <h5 class="dropdown-item-text mb-0">{{ user.displayName }}</h5>
                  <p class="dropdown-item-text text-muted mb-0">{{ user.email }}</p>
                  <div class="dropdown-divider"></div>
                  <a href="/auth/signout" class="dropdown-item">Sign Out</a>
                </div>
              </li>
            {{else}}
              <li class="nav-item">
                <a href="/auth/signin" class="nav-link">Sign In</a>
              </li>
            {{/if}}
          </ul>
        </div>
      </div>
    </nav>
    <main role="main" class="container">
      {{#each error}}
        <div class="alert alert-danger" role="alert">
          <p class="mb-3">{{ this.message }}</p>
          {{#if this.debug }}
            <pre class="alert-pre border bg-light p-2"><code>{{ this.debug }}</code></pre>
          {{/if}}
        </div>
      {{/each}}

      {{{body}}}
    </main>
  </body>
</html>
```

В этом коде добавляется [Начальная](http://getbootstrap.com/) загрузка для простых стилей и [Шрифт Awesome](https://fontawesome.com/) для некоторых простых значков. Он также определяет глобальную структуру с помощью панели навигации.

Теперь откройте `./public/stylesheets/style.css` и замените все содержимое приведенным ниже.

```css
body {
  padding-top: 4.5rem;
}

.alert-pre {
  word-wrap: break-word;
  word-break: break-all;
  white-space: pre-wrap;
}
```

Теперь обновите страницу по умолчанию. Откройте `./views/index.hbs` файл и замените его содержимое на приведенный ниже код.

```html
<div class="jumbotron">
  <h1>Node.js Graph Tutorial</h1>
  <p class="lead">This sample app shows how to use the Microsoft Graph API to access Outlook and OneDrive data from Node.js</p>
  {{#if user}}
    <h4>Welcome {{ user.displayName }}!</h4>
    <p>Use the navigation bar at the top of the page to get started.</p>
  {{else}}
    <a href="/auth/signin" class="btn btn-primary btn-large">Click here to sign in</a>
  {{/if}}
</div>
```

Откройте `./routes/index.js` файл и замените существующий код приведенным ниже кодом.

```js
var express = require('express');
var router = express.Router();

/* GET home page. */
router.get('/', function(req, res, next) {
  let params = {
    active: { home: true }
  };

  res.render('index', params);
});

module.exports = router;
```

Сохраните все изменения и перезапустите сервер. Теперь приложение должно выглядеть по-другому.

![Снимок экрана с переработанной домашней страницей](./images/create-app-01.png)
