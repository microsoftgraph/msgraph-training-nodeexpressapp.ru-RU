<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="a8ad4-101">В этом упражнении вы будете использовать [Express](http://expressjs.com/) для создания веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="a8ad4-101">In this exercise you will use [Express](http://expressjs.com/) to build a web app.</span></span> <span data-ttu-id="a8ad4-102">Если вы еще не установили генератор Express, вы можете установить его из интерфейса командной строки (CLI) с помощью следующей команды.</span><span class="sxs-lookup"><span data-stu-id="a8ad4-102">If you don't already have the Express generator installed, you can install it from your command-line interface (CLI) with the following command.</span></span>

```Shell
npm install express-generator -g
```

<span data-ttu-id="a8ad4-103">Откройте подсистему CLI, перейдите к каталогу, в котором у вас есть права на создание файлов, и выполните следующую команду, чтобы создать новое приложение Express, использующее [handlebars](http://handlebarsjs.com/) в качестве обработчика визуализации.</span><span class="sxs-lookup"><span data-stu-id="a8ad4-103">Open your CLI, navigate to a directory where you have rights to create files, and run the following command to create a new Express app that uses [Handlebars](http://handlebarsjs.com/) as the rendering engine.</span></span>

```Shell
express --hbs graph-tutorial
```

<span data-ttu-id="a8ad4-104">Генератор Express создает новый каталог с именем `graph-tutorial` и формированием шаблонов для приложения Express.</span><span class="sxs-lookup"><span data-stu-id="a8ad4-104">The Express generator creates a new directory called `graph-tutorial` and scaffolds an Express app.</span></span> <span data-ttu-id="a8ad4-105">Перейдите к новому каталогу и введите следующую команду, чтобы установить зависимости.</span><span class="sxs-lookup"><span data-stu-id="a8ad4-105">Navigate to this new directory and enter the following command to install dependencies.</span></span>

```Shell
npm install
```

<span data-ttu-id="a8ad4-106">После выполнения этой команды используйте следующую команду для запуска локального веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="a8ad4-106">Once that command completes, use the following command to start a local web server.</span></span>

```Shell
npm start
```

<span data-ttu-id="a8ad4-107">Откройте браузер и перейдите по адресу `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="a8ad4-107">Open your browser and navigate to `http://localhost:3000`.</span></span> <span data-ttu-id="a8ad4-108">Если все работает, вы увидите сообщение "Добро пожаловать в Express".</span><span class="sxs-lookup"><span data-stu-id="a8ad4-108">If everything is working, you will see a "Welcome to Express" message.</span></span> <span data-ttu-id="a8ad4-109">Если вы не видите это сообщение, ознакомьтесь с [руководством Express Приступая к началу работы](http://expressjs.com/starter/generator.html).</span><span class="sxs-lookup"><span data-stu-id="a8ad4-109">If you don't see that message, check the [Express getting started guide](http://expressjs.com/starter/generator.html).</span></span>

<span data-ttu-id="a8ad4-110">Прежде чем переходить, установите несколько дополнительных драгоценных камней, которые будут использоваться позже:</span><span class="sxs-lookup"><span data-stu-id="a8ad4-110">Before moving on, install some additional gems that you will use later:</span></span>

- <span data-ttu-id="a8ad4-111">[дотенв](https://github.com/motdotla/dotenv) для загрузки значений из файла env.</span><span class="sxs-lookup"><span data-stu-id="a8ad4-111">[dotenv](https://github.com/motdotla/dotenv) for loading values from a .env file.</span></span>
- <span data-ttu-id="a8ad4-112">[время для форматирования](https://github.com/moment/moment/) значений даты и времени.</span><span class="sxs-lookup"><span data-stu-id="a8ad4-112">[moment](https://github.com/moment/moment/) for formatting date/time values.</span></span>
- <span data-ttu-id="a8ad4-113">сообщение об ошибке [Connect-Flash](https://github.com/jaredhanson/connect-flash) to Flash в приложении.</span><span class="sxs-lookup"><span data-stu-id="a8ad4-113">[connect-flash](https://github.com/jaredhanson/connect-flash) to flash error messages in the app.</span></span>
- <span data-ttu-id="a8ad4-114">[Express — сеанс](https://github.com/expressjs/session) для хранения значений в серверном сеансе в памяти на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="a8ad4-114">[express-session](https://github.com/expressjs/session) to store values in an in-memory server-side session.</span></span>
- <span data-ttu-id="a8ad4-115">[Passport — Azure — AD](https://github.com/AzureAD/passport-azure-ad) для проверки подлинности и получения маркеров доступа.</span><span class="sxs-lookup"><span data-stu-id="a8ad4-115">[passport-azure-ad](https://github.com/AzureAD/passport-azure-ad) for authenticating and getting access tokens.</span></span>
- <span data-ttu-id="a8ad4-116">[Simple — OAuth2](https://github.com/lelylan/simple-oauth2) для управления маркерами.</span><span class="sxs-lookup"><span data-stu-id="a8ad4-116">[simple-oauth2](https://github.com/lelylan/simple-oauth2) for token management.</span></span>
- <span data-ttu-id="a8ad4-117">[Microsoft — Graph — клиент](https://github.com/microsoftgraph/msgraph-sdk-javascript) для совершения звонков в Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="a8ad4-117">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>

<span data-ttu-id="a8ad4-118">Выполните следующую команду в командной панели CLI.</span><span class="sxs-lookup"><span data-stu-id="a8ad4-118">Run the following command in your CLI.</span></span>

```Shell
npm install dotenv@8.1.0 moment@2.24.0 connect-flash@0.1.1 express-session@1.16.2
npm install passport-azure-ad@4.1.0 simple-oauth2@2.4.0 @microsoft/microsoft-graph-client@1.7.0
```

> [!TIP]
> <span data-ttu-id="a8ad4-119">При попытке установить эти пакеты в Windows пользователи Windows могут получить следующее сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="a8ad4-119">Windows users may get the following error message when trying to install these packages on Windows.</span></span>
>
> ```Shell
> gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
> ```
>
> <span data-ttu-id="a8ad4-120">Чтобы устранить эту ошибку, выполните следующую команду, чтобы установить средства построения Windows с помощью окна терминала с повышенными привилегиями (администратора), которое устанавливает инструменты VS Build и Python.</span><span class="sxs-lookup"><span data-stu-id="a8ad4-120">To resolve the error, run the following command to install the Windows Build Tools using an elevated (Administrator) terminal window which installs the VS Build Tools and Python.</span></span>
>
> ```Shell
> npm install --global --production windows-build-tools
> ```

<span data-ttu-id="a8ad4-121">Теперь обновите приложение, чтобы оно `connect-flash` использовалось по `express-session` промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="a8ad4-121">Now update the application to use the `connect-flash` and `express-session` middleware.</span></span> <span data-ttu-id="a8ad4-122">Откройте `./app.js` файл и добавьте приведенный ниже `require` оператор в начало файла.</span><span class="sxs-lookup"><span data-stu-id="a8ad4-122">Open the `./app.js` file and add the following `require` statement to the top of the file.</span></span>

```js
var session = require('express-session');
var flash = require('connect-flash');
```

<span data-ttu-id="a8ad4-123">Добавьте приведенный ниже код сразу после `var app = express();` строки.</span><span class="sxs-lookup"><span data-stu-id="a8ad4-123">Add the following code immediately after the `var app = express();` line.</span></span>

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

## <a name="design-the-app"></a><span data-ttu-id="a8ad4-124">Проектирование приложения</span><span class="sxs-lookup"><span data-stu-id="a8ad4-124">Design the app</span></span>

<span data-ttu-id="a8ad4-125">Начните с создания глобального макета для приложения.</span><span class="sxs-lookup"><span data-stu-id="a8ad4-125">Start by creating the global layout for the app.</span></span> <span data-ttu-id="a8ad4-126">Откройте `./views/layout.hbs` файл и замените все его содержимое приведенным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="a8ad4-126">Open the `./views/layout.hbs` file and replace the entire contents with the following code.</span></span>

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

<span data-ttu-id="a8ad4-127">В этом коде добавляется [Начальная](http://getbootstrap.com/) загрузка для простых стилей и [Шрифт Awesome](https://fontawesome.com/) для некоторых простых значков.</span><span class="sxs-lookup"><span data-stu-id="a8ad4-127">This code adds [Bootstrap](http://getbootstrap.com/) for simple styling, and [Font Awesome](https://fontawesome.com/) for some simple icons.</span></span> <span data-ttu-id="a8ad4-128">Он также определяет глобальную структуру с помощью панели навигации.</span><span class="sxs-lookup"><span data-stu-id="a8ad4-128">It also defines a global layout with a nav bar.</span></span>

<span data-ttu-id="a8ad4-129">Теперь откройте `./public/stylesheets/style.css` и замените все содержимое приведенным ниже.</span><span class="sxs-lookup"><span data-stu-id="a8ad4-129">Now open `./public/stylesheets/style.css` and replace its entire contents with the following.</span></span>

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

<span data-ttu-id="a8ad4-130">Теперь обновите страницу по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a8ad4-130">Now update the default page.</span></span> <span data-ttu-id="a8ad4-131">Откройте `./views/index.hbs` файл и замените его содержимое на приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="a8ad4-131">Open the `./views/index.hbs` file and replace its contents with the following.</span></span>

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

<span data-ttu-id="a8ad4-132">Откройте `./routes/index.js` файл и замените существующий код приведенным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="a8ad4-132">Open the `./routes/index.js` file and replace the existing code with the following.</span></span>

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

<span data-ttu-id="a8ad4-133">Сохраните все изменения и перезапустите сервер.</span><span class="sxs-lookup"><span data-stu-id="a8ad4-133">Save all of your changes and restart the server.</span></span> <span data-ttu-id="a8ad4-134">Теперь приложение должно выглядеть по-другому.</span><span class="sxs-lookup"><span data-stu-id="a8ad4-134">Now, the app should look very different.</span></span>

![Снимок экрана с переработанной домашней страницей](./images/create-app-01.png)
