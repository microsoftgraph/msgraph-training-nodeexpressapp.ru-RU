<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="23726-101">В этом упражнении вы будете использовать [Express](http://expressjs.com/) для создания веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="23726-101">In this exercise you will use [Express](http://expressjs.com/) to build a web app.</span></span>

1. <span data-ttu-id="23726-102">Откройте CLI, перейдите в каталог, в котором у вас есть права на создание файлов, и запустите следующую команду, чтобы создать новое приложение Express, которое использует [handlebars](http://handlebarsjs.com/) в качестве двигателя отрисовки.</span><span class="sxs-lookup"><span data-stu-id="23726-102">Open your CLI, navigate to a directory where you have rights to create files, and run the following command to create a new Express app that uses [Handlebars](http://handlebarsjs.com/) as the rendering engine.</span></span>

    ```Shell
    npx express-generator@4.16.1 --hbs graph-tutorial
    ```

    <span data-ttu-id="23726-103">Генератор Express создает новый каталог под названием `graph-tutorial` и создает приложение Express.</span><span class="sxs-lookup"><span data-stu-id="23726-103">The Express generator creates a new directory called `graph-tutorial` and scaffolds an Express app.</span></span>

1. <span data-ttu-id="23726-104">Перейдите в `graph-tutorial` каталог и введите следующую команду для установки зависимостей.</span><span class="sxs-lookup"><span data-stu-id="23726-104">Navigate to the `graph-tutorial` directory and enter the following command to install dependencies.</span></span>

    ```Shell
    npm install
    ```

1. <span data-ttu-id="23726-105">Запустите следующую команду, чтобы обновить пакеты Node с сообщаемой уязвимостью.</span><span class="sxs-lookup"><span data-stu-id="23726-105">Run the following command to update Node packages with reported vulnerabilities.</span></span>

    ```Shell
    npm audit fix
    ```

1. <span data-ttu-id="23726-106">Запустите следующую команду для обновления версии Express и других зависимостей.</span><span class="sxs-lookup"><span data-stu-id="23726-106">Run the following command to update the version of Express and other dependencies.</span></span>

    ```Shell
    npm install express@4.17.1 http-errors@1.8.0 morgan@1.10.0 debug@4.3.1 hbs@4.1.2
    ```

1. <span data-ttu-id="23726-107">Чтобы запустить локальный веб-сервер, используйте следующую команду.</span><span class="sxs-lookup"><span data-stu-id="23726-107">Use the following command to start a local web server.</span></span>

    ```Shell
    npm start
    ```

1. <span data-ttu-id="23726-108">Откройте браузер и перейдите по адресу `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="23726-108">Open your browser and navigate to `http://localhost:3000`.</span></span> <span data-ttu-id="23726-109">Если все работает, вы увидите сообщение "Добро пожаловать в экспресс".</span><span class="sxs-lookup"><span data-stu-id="23726-109">If everything is working, you will see a "Welcome to Express" message.</span></span> <span data-ttu-id="23726-110">Если вы не видите это сообщение, проверьте руководство по началу [работы Express.](http://expressjs.com/starter/generator.html)</span><span class="sxs-lookup"><span data-stu-id="23726-110">If you don't see that message, check the [Express getting started guide](http://expressjs.com/starter/generator.html).</span></span>

## <a name="install-node-packages"></a><span data-ttu-id="23726-111">Установка пакетов node</span><span class="sxs-lookup"><span data-stu-id="23726-111">Install Node packages</span></span>

<span data-ttu-id="23726-112">Прежде чем двигаться дальше, установите дополнительные пакеты, которые вы будете использовать позже:</span><span class="sxs-lookup"><span data-stu-id="23726-112">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="23726-113">[dotenv](https://github.com/motdotla/dotenv) для загрузки значений из файла .env.</span><span class="sxs-lookup"><span data-stu-id="23726-113">[dotenv](https://github.com/motdotla/dotenv) for loading values from a .env file.</span></span>
- <span data-ttu-id="23726-114">[date-fns](https://github.com/date-fns/date-fns) для форматирования значений даты и времени.</span><span class="sxs-lookup"><span data-stu-id="23726-114">[date-fns](https://github.com/date-fns/date-fns) for formatting date/time values.</span></span>
- <span data-ttu-id="23726-115">[windows-iana](https://github.com/rubenillodo/windows-iana) для перевода имен Windows часового пояса в IANA.</span><span class="sxs-lookup"><span data-stu-id="23726-115">[windows-iana](https://github.com/rubenillodo/windows-iana) for translating Windows time zone names to IANA time zone IDs.</span></span>
- <span data-ttu-id="23726-116">[подключение-вспышка](https://github.com/jaredhanson/connect-flash) для сообщений об ошибках вспышки в приложении.</span><span class="sxs-lookup"><span data-stu-id="23726-116">[connect-flash](https://github.com/jaredhanson/connect-flash) to flash error messages in the app.</span></span>
- <span data-ttu-id="23726-117">[экспресс-сеанс](https://github.com/expressjs/session) для хранения значений в сеансе сервера в памяти.</span><span class="sxs-lookup"><span data-stu-id="23726-117">[express-session](https://github.com/expressjs/session) to store values in an in-memory server-side session.</span></span>
- <span data-ttu-id="23726-118">[маршрутизатор express-promise-router,](https://github.com/express-promise-router/express-promise-router) позволяющий обработчику маршрутов возвращать обещание.</span><span class="sxs-lookup"><span data-stu-id="23726-118">[express-promise-router](https://github.com/express-promise-router/express-promise-router) to allow route handlers to return a Promise.</span></span>
- <span data-ttu-id="23726-119">[экспресс-валидатор](https://github.com/express-validator/express-validator) для анализа и проверки данных форм.</span><span class="sxs-lookup"><span data-stu-id="23726-119">[express-validator](https://github.com/express-validator/express-validator) for parsing and validating form data.</span></span>
- <span data-ttu-id="23726-120">[msal-node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) для проверки подлинности и получения маркеров доступа.</span><span class="sxs-lookup"><span data-stu-id="23726-120">[msal-node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) for authenticating and getting access tokens.</span></span>
- <span data-ttu-id="23726-121">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) для звонков в Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="23726-121">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>
- <span data-ttu-id="23726-122">[isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) для полифайла для получения узла.</span><span class="sxs-lookup"><span data-stu-id="23726-122">[isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) to polyfill the fetch for Node.</span></span> <span data-ttu-id="23726-123">Для библиотеки требуется извлечение `microsoft-graph-client` полифайла.</span><span class="sxs-lookup"><span data-stu-id="23726-123">A fetch polyfill is required for the `microsoft-graph-client` library.</span></span> <span data-ttu-id="23726-124">Дополнительные сведения [см. Graph вики клиентской библиотеки Microsoft Graph JavaScript.](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required)</span><span class="sxs-lookup"><span data-stu-id="23726-124">See the [Microsoft Graph JavaScript client library wiki](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) for more information.</span></span>

1. <span data-ttu-id="23726-125">Запустите следующую команду в CLI.</span><span class="sxs-lookup"><span data-stu-id="23726-125">Run the following command in your CLI.</span></span>

    ```Shell
    npm install dotenv@8.2.0 date-fns@2.21.1 date-fns-tz@1.1.4 connect-flash@0.1.1 express-validator@6.10.0
    npm install express-session@1.17.1 express-promise-router@4.1.0 isomorphic-fetch@3.0.0
    npm install @azure/msal-node@1.0.2 @microsoft/microsoft-graph-client@2.2.1 windows-iana@5.0.1
    ```

    > [!TIP]
    > <span data-ttu-id="23726-126">Windows пользователи могут получить следующее сообщение об ошибке при попытке установить эти пакеты на Windows.</span><span class="sxs-lookup"><span data-stu-id="23726-126">Windows users may get the following error message when trying to install these packages on Windows.</span></span>
    >
    > ```Shell
    > gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
    > ```
    >
    > <span data-ttu-id="23726-127">Чтобы устранить ошибку, запустите следующую команду, чтобы установить инструменты сборки Windows с помощью окна терминала повышенного уровня (Администратор), которое устанавливает средства сборки VS и Python.</span><span class="sxs-lookup"><span data-stu-id="23726-127">To resolve the error, run the following command to install the Windows Build Tools using an elevated (Administrator) terminal window which installs the VS Build Tools and Python.</span></span>
    >
    > ```Shell
    > npm install --global --production windows-build-tools
    > ```

1. <span data-ttu-id="23726-128">Обнови приложение для использования `connect-flash` программного обеспечения и `express-session` среднего программного обеспечения.</span><span class="sxs-lookup"><span data-stu-id="23726-128">Update the application to use the `connect-flash` and `express-session` middleware.</span></span> <span data-ttu-id="23726-129">Откройте **./app.js** и добавьте следующее `require` утверждение в верхнюю часть файла.</span><span class="sxs-lookup"><span data-stu-id="23726-129">Open **./app.js** and add the following `require` statement to the top of the file.</span></span>

    ```javascript
    const session = require('express-session');
    const flash = require('connect-flash');
    const msal = require('@azure/msal-node');
    ```

1. <span data-ttu-id="23726-130">Добавьте следующий код сразу после `var app = express();` строки.</span><span class="sxs-lookup"><span data-stu-id="23726-130">Add the following code immediately after the `var app = express();` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SessionSnippet":::

## <a name="design-the-app"></a><span data-ttu-id="23726-131">Проектирование приложения</span><span class="sxs-lookup"><span data-stu-id="23726-131">Design the app</span></span>

<span data-ttu-id="23726-132">В этом разделе будет реализован пользовательский интерфейс приложения.</span><span class="sxs-lookup"><span data-stu-id="23726-132">In this section you will implement the UI for the app.</span></span>

1. <span data-ttu-id="23726-133">Откройте **./views/layout.hbs** и замените все содержимое следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="23726-133">Open **./views/layout.hbs** and replace the entire contents with the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/layout.hbs" id="LayoutSnippet":::

    <span data-ttu-id="23726-134">Этот код добавляет [Bootstrap](http://getbootstrap.com/) для простого стиля и [Font Awesome](https://fontawesome.com/) для некоторых простых значков.</span><span class="sxs-lookup"><span data-stu-id="23726-134">This code adds [Bootstrap](http://getbootstrap.com/) for simple styling, and [Font Awesome](https://fontawesome.com/) for some simple icons.</span></span> <span data-ttu-id="23726-135">Он также определяет глобальную макет с панели nav.</span><span class="sxs-lookup"><span data-stu-id="23726-135">It also defines a global layout with a nav bar.</span></span>

1. <span data-ttu-id="23726-136">Откройте **таблицы ./public/stylesheets/style.css** и замените все содержимое следующим образом.</span><span class="sxs-lookup"><span data-stu-id="23726-136">Open **./public/stylesheets/style.css** and replace its entire contents with the following.</span></span>

    :::code language="css" source="../demo/graph-tutorial/public/stylesheets/style.css":::

1. <span data-ttu-id="23726-137">Откройте **./views/index.hbs** и замените его содержимое следующим.</span><span class="sxs-lookup"><span data-stu-id="23726-137">Open **./views/index.hbs** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/index.hbs" id="IndexSnippet":::

1. <span data-ttu-id="23726-138">Откройте **./routes/index.js** и замените существующий код следующим.</span><span class="sxs-lookup"><span data-stu-id="23726-138">Open **./routes/index.js** and replace the existing code with the following.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/index.js" id="IndexRouterSnippet" highlight="6-10":::

1. <span data-ttu-id="23726-139">Сохраните все изменения и перезапустите сервер.</span><span class="sxs-lookup"><span data-stu-id="23726-139">Save all of your changes and restart the server.</span></span> <span data-ttu-id="23726-140">Теперь приложение должно выглядеть совсем по-другому.</span><span class="sxs-lookup"><span data-stu-id="23726-140">Now, the app should look very different.</span></span>

    ![Снимок экрана: обновленная домашняя страница](./images/create-app-01.png)
