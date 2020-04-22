<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="96d9f-101">В этом упражнении вы будете использовать [Express](http://expressjs.com/) для создания веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="96d9f-101">In this exercise you will use [Express](http://expressjs.com/) to build a web app.</span></span>

1. <span data-ttu-id="96d9f-102">Откройте подсистему CLI, перейдите к каталогу, в котором у вас есть права на создание файлов, и выполните следующую команду, чтобы создать новое приложение Express, использующее [handlebars](http://handlebarsjs.com/) в качестве обработчика визуализации.</span><span class="sxs-lookup"><span data-stu-id="96d9f-102">Open your CLI, navigate to a directory where you have rights to create files, and run the following command to create a new Express app that uses [Handlebars](http://handlebarsjs.com/) as the rendering engine.</span></span>

    ```Shell
    npx express-generator@4.16.1 --hbs graph-tutorial
    ```

    <span data-ttu-id="96d9f-103">Генератор Express создает новый каталог с именем `graph-tutorial` и формированием шаблонов для приложения Express.</span><span class="sxs-lookup"><span data-stu-id="96d9f-103">The Express generator creates a new directory called `graph-tutorial` and scaffolds an Express app.</span></span>

1. <span data-ttu-id="96d9f-104">Перейдите в `graph-tutorial` каталог и введите следующую команду, чтобы установить зависимости.</span><span class="sxs-lookup"><span data-stu-id="96d9f-104">Navigate to the `graph-tutorial` directory and enter the following command to install dependencies.</span></span>

    ```Shell
    npm install
    ```

1. <span data-ttu-id="96d9f-105">Выполните следующую команду, чтобы обновить пакеты узлов с обнаруженными уязвимостями.</span><span class="sxs-lookup"><span data-stu-id="96d9f-105">Run the following command to update Node packages with reported vulnerabilities.</span></span>

    ```Shell
    npm audit fix
    ```

1. <span data-ttu-id="96d9f-106">Используйте следующую команду для запуска локального веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="96d9f-106">Use the following command to start a local web server.</span></span>

    ```Shell
    npm start
    ```

1. <span data-ttu-id="96d9f-107">Откройте браузер и перейдите по адресу `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="96d9f-107">Open your browser and navigate to `http://localhost:3000`.</span></span> <span data-ttu-id="96d9f-108">Если все работает, вы увидите сообщение "Добро пожаловать в Express".</span><span class="sxs-lookup"><span data-stu-id="96d9f-108">If everything is working, you will see a "Welcome to Express" message.</span></span> <span data-ttu-id="96d9f-109">Если вы не видите это сообщение, ознакомьтесь с [руководством Express Приступая к началу работы](http://expressjs.com/starter/generator.html).</span><span class="sxs-lookup"><span data-stu-id="96d9f-109">If you don't see that message, check the [Express getting started guide](http://expressjs.com/starter/generator.html).</span></span>

## <a name="install-node-packages"></a><span data-ttu-id="96d9f-110">Установка пакетов узлов</span><span class="sxs-lookup"><span data-stu-id="96d9f-110">Install Node packages</span></span>

<span data-ttu-id="96d9f-111">Прежде чем переходить, установите несколько дополнительных пакетов, которые будут использоваться позже:</span><span class="sxs-lookup"><span data-stu-id="96d9f-111">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="96d9f-112">[дотенв](https://github.com/motdotla/dotenv) для загрузки значений из файла env.</span><span class="sxs-lookup"><span data-stu-id="96d9f-112">[dotenv](https://github.com/motdotla/dotenv) for loading values from a .env file.</span></span>
- <span data-ttu-id="96d9f-113">[время для форматирования](https://github.com/moment/moment/) значений даты и времени.</span><span class="sxs-lookup"><span data-stu-id="96d9f-113">[moment](https://github.com/moment/moment/) for formatting date/time values.</span></span>
- <span data-ttu-id="96d9f-114">сообщение об ошибке [Connect-Flash](https://github.com/jaredhanson/connect-flash) to Flash в приложении.</span><span class="sxs-lookup"><span data-stu-id="96d9f-114">[connect-flash](https://github.com/jaredhanson/connect-flash) to flash error messages in the app.</span></span>
- <span data-ttu-id="96d9f-115">[Express — сеанс](https://github.com/expressjs/session) для хранения значений в серверном сеансе в памяти на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="96d9f-115">[express-session](https://github.com/expressjs/session) to store values in an in-memory server-side session.</span></span>
- <span data-ttu-id="96d9f-116">[Passport — Azure — AD](https://github.com/AzureAD/passport-azure-ad) для проверки подлинности и получения маркеров доступа.</span><span class="sxs-lookup"><span data-stu-id="96d9f-116">[passport-azure-ad](https://github.com/AzureAD/passport-azure-ad) for authenticating and getting access tokens.</span></span>
- <span data-ttu-id="96d9f-117">[Simple — OAuth2](https://github.com/lelylan/simple-oauth2) для управления маркерами.</span><span class="sxs-lookup"><span data-stu-id="96d9f-117">[simple-oauth2](https://github.com/lelylan/simple-oauth2) for token management.</span></span>
- <span data-ttu-id="96d9f-118">[Microsoft — Graph — клиент](https://github.com/microsoftgraph/msgraph-sdk-javascript) для совершения звонков в Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="96d9f-118">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>
- <span data-ttu-id="96d9f-119">[исоморфик — получение](https://github.com/matthew-andrews/isomorphic-fetch) для заполнения для узла.</span><span class="sxs-lookup"><span data-stu-id="96d9f-119">[isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) to polyfill the fetch for Node.</span></span> <span data-ttu-id="96d9f-120">Для `microsoft-graph-client` библиотеки необходима получающая заливка.</span><span class="sxs-lookup"><span data-stu-id="96d9f-120">A fetch polyfill is required for the `microsoft-graph-client` library.</span></span> <span data-ttu-id="96d9f-121">Для получения дополнительных сведений посетите [вики-сайт клиентской библиотеки JavaScript для Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) .</span><span class="sxs-lookup"><span data-stu-id="96d9f-121">See the [Microsoft Graph JavaScript client library wiki](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) for more information.</span></span>

1. <span data-ttu-id="96d9f-122">Выполните следующую команду в командной панели CLI.</span><span class="sxs-lookup"><span data-stu-id="96d9f-122">Run the following command in your CLI.</span></span>

    ```Shell
    npm install dotenv@8.2.0 moment@2.24.0 connect-flash@0.1.1 express-session@1.17.0 isomorphic-fetch@2.2.1
    npm install passport-azure-ad@4.2.1 simple-oauth2@3.3.0 @microsoft/microsoft-graph-client@2.0.0
    ```

    > [!TIP]
    > <span data-ttu-id="96d9f-123">При попытке установить эти пакеты в Windows пользователи Windows могут получить следующее сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="96d9f-123">Windows users may get the following error message when trying to install these packages on Windows.</span></span>
    >
    > ```Shell
    > gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
    > ```
    >
    > <span data-ttu-id="96d9f-124">Чтобы устранить эту ошибку, выполните следующую команду, чтобы установить средства построения Windows с помощью окна терминала с повышенными привилегиями (администратора), которое устанавливает инструменты VS Build и Python.</span><span class="sxs-lookup"><span data-stu-id="96d9f-124">To resolve the error, run the following command to install the Windows Build Tools using an elevated (Administrator) terminal window which installs the VS Build Tools and Python.</span></span>
    >
    > ```Shell
    > npm install --global --production windows-build-tools
    > ```

1. <span data-ttu-id="96d9f-125">Обновите приложение, чтобы оно `connect-flash` использовалось по `express-session` промежуточному программному отношению.</span><span class="sxs-lookup"><span data-stu-id="96d9f-125">Update the application to use the `connect-flash` and `express-session` middleware.</span></span> <span data-ttu-id="96d9f-126">Откройте `./app.js` файл и добавьте приведенный ниже `require` оператор в начало файла.</span><span class="sxs-lookup"><span data-stu-id="96d9f-126">Open the `./app.js` file and add the following `require` statement to the top of the file.</span></span>

    ```javascript
    var session = require('express-session');
    var flash = require('connect-flash');
    ```

1. <span data-ttu-id="96d9f-127">Добавьте приведенный ниже код сразу после `var app = express();` строки.</span><span class="sxs-lookup"><span data-stu-id="96d9f-127">Add the following code immediately after the `var app = express();` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SessionSnippet":::

## <a name="design-the-app"></a><span data-ttu-id="96d9f-128">Проектирование приложения</span><span class="sxs-lookup"><span data-stu-id="96d9f-128">Design the app</span></span>

<span data-ttu-id="96d9f-129">В этом разделе будет реализован пользовательский интерфейс приложения.</span><span class="sxs-lookup"><span data-stu-id="96d9f-129">In this section you will implement the UI for the app.</span></span>

1. <span data-ttu-id="96d9f-130">Откройте `./views/layout.hbs` файл и замените все его содержимое приведенным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="96d9f-130">Open the `./views/layout.hbs` file and replace the entire contents with the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/layout.hbs" id="LayoutSnippet":::

    <span data-ttu-id="96d9f-131">В этом коде добавляется [Начальная](http://getbootstrap.com/) загрузка для простых стилей и [Шрифт Awesome](https://fontawesome.com/) для некоторых простых значков.</span><span class="sxs-lookup"><span data-stu-id="96d9f-131">This code adds [Bootstrap](http://getbootstrap.com/) for simple styling, and [Font Awesome](https://fontawesome.com/) for some simple icons.</span></span> <span data-ttu-id="96d9f-132">Он также определяет глобальную структуру с помощью панели навигации.</span><span class="sxs-lookup"><span data-stu-id="96d9f-132">It also defines a global layout with a nav bar.</span></span>

1. <span data-ttu-id="96d9f-133">Откройте `./public/stylesheets/style.css` и замените все содержимое приведенным ниже.</span><span class="sxs-lookup"><span data-stu-id="96d9f-133">Open `./public/stylesheets/style.css` and replace its entire contents with the following.</span></span>

    :::code language="css" source="../demo/graph-tutorial/public/stylesheets/style.css":::

1. <span data-ttu-id="96d9f-134">Откройте `./views/index.hbs` файл и замените его содержимое на приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="96d9f-134">Open the `./views/index.hbs` file and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/index.hbs" id="IndexSnippet":::

1. <span data-ttu-id="96d9f-135">Откройте `./routes/index.js` файл и замените существующий код приведенным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="96d9f-135">Open the `./routes/index.js` file and replace the existing code with the following.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/index.js" id="IndexRouterSnippet" highlight="6-10":::

1. <span data-ttu-id="96d9f-136">Сохраните все изменения и перезапустите сервер.</span><span class="sxs-lookup"><span data-stu-id="96d9f-136">Save all of your changes and restart the server.</span></span> <span data-ttu-id="96d9f-137">Теперь приложение должно выглядеть по-другому.</span><span class="sxs-lookup"><span data-stu-id="96d9f-137">Now, the app should look very different.</span></span>

    ![Снимок экрана с переработанной домашней страницей](./images/create-app-01.png)
