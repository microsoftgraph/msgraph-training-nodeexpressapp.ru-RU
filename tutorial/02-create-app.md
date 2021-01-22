<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="ddad9-101">В этом упражнении вы будете использовать [Express](http://expressjs.com/) для создания веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="ddad9-101">In this exercise you will use [Express](http://expressjs.com/) to build a web app.</span></span>

1. <span data-ttu-id="ddad9-102">Откройте CLI, перейдите в каталог, в котором у вас есть права на создание файлов, и запустите следующую команду, чтобы создать приложение Express, использующее [Handlebars](http://handlebarsjs.com/) в качестве обработщика отрисовки.</span><span class="sxs-lookup"><span data-stu-id="ddad9-102">Open your CLI, navigate to a directory where you have rights to create files, and run the following command to create a new Express app that uses [Handlebars](http://handlebarsjs.com/) as the rendering engine.</span></span>

    ```Shell
    npx express-generator@4.16.1 --hbs graph-tutorial
    ```

    <span data-ttu-id="ddad9-103">Генератор Express создает новый каталог, называемый приложением Express, и создает `graph-tutorial` скаолдолды.</span><span class="sxs-lookup"><span data-stu-id="ddad9-103">The Express generator creates a new directory called `graph-tutorial` and scaffolds an Express app.</span></span>

1. <span data-ttu-id="ddad9-104">Перейдите в `graph-tutorial` каталог и введите следующую команду для установки зависимостей.</span><span class="sxs-lookup"><span data-stu-id="ddad9-104">Navigate to the `graph-tutorial` directory and enter the following command to install dependencies.</span></span>

    ```Shell
    npm install
    ```

1. <span data-ttu-id="ddad9-105">Чтобы обновить пакеты Node с помощью сообщений об уязвимостях, запустите следующую команду.</span><span class="sxs-lookup"><span data-stu-id="ddad9-105">Run the following command to update Node packages with reported vulnerabilities.</span></span>

    ```Shell
    npm audit fix
    ```

1. <span data-ttu-id="ddad9-106">Чтобы обновить версию Express и других зависимостей, запустите следующую команду.</span><span class="sxs-lookup"><span data-stu-id="ddad9-106">Run the following command to update the version of Express and other dependencies.</span></span>

    ```Shell
    npm install express@4.17.1 http-errors@1.8.0 morgan@1.10.0 debug@4.1.1
    ```

1. <span data-ttu-id="ddad9-107">Используйте следующую команду для запуска локального веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="ddad9-107">Use the following command to start a local web server.</span></span>

    ```Shell
    npm start
    ```

1. <span data-ttu-id="ddad9-108">Откройте браузер и перейдите по адресу `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="ddad9-108">Open your browser and navigate to `http://localhost:3000`.</span></span> <span data-ttu-id="ddad9-109">Если все работает, вы увидите сообщение "Добро пожаловать в Express".</span><span class="sxs-lookup"><span data-stu-id="ddad9-109">If everything is working, you will see a "Welcome to Express" message.</span></span> <span data-ttu-id="ddad9-110">Если вы не видите это сообщение, ознакомьтесь с руководством [по началу работы с Express.](http://expressjs.com/starter/generator.html)</span><span class="sxs-lookup"><span data-stu-id="ddad9-110">If you don't see that message, check the [Express getting started guide](http://expressjs.com/starter/generator.html).</span></span>

## <a name="install-node-packages"></a><span data-ttu-id="ddad9-111">Установка пакетов node</span><span class="sxs-lookup"><span data-stu-id="ddad9-111">Install Node packages</span></span>

<span data-ttu-id="ddad9-112">Прежде чем двигаться дальше, установите некоторые дополнительные пакеты, которые вы будете использовать позже:</span><span class="sxs-lookup"><span data-stu-id="ddad9-112">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="ddad9-113">[dotenv](https://github.com/motdotla/dotenv) для загрузки значений из ENV-файла.</span><span class="sxs-lookup"><span data-stu-id="ddad9-113">[dotenv](https://github.com/motdotla/dotenv) for loading values from a .env file.</span></span>
- <span data-ttu-id="ddad9-114">[момент](https://github.com/moment/moment/) для форматирования значений даты и времени.</span><span class="sxs-lookup"><span data-stu-id="ddad9-114">[moment](https://github.com/moment/moment/) for formatting date/time values.</span></span>
- <span data-ttu-id="ddad9-115">[windows-iana](https://github.com/rubenillodo/windows-iana) для перевода имен часового пояса Windows в IANA.</span><span class="sxs-lookup"><span data-stu-id="ddad9-115">[windows-iana](https://github.com/rubenillodo/windows-iana) for translating Windows time zone names to IANA time zone IDs.</span></span>
- <span data-ttu-id="ddad9-116">[connect-flash](https://github.com/jaredhanson/connect-flash) к сообщениям об ошибках флэш-накопителя в приложении.</span><span class="sxs-lookup"><span data-stu-id="ddad9-116">[connect-flash](https://github.com/jaredhanson/connect-flash) to flash error messages in the app.</span></span>
- <span data-ttu-id="ddad9-117">[express-session](https://github.com/expressjs/session) для хранения значений в сеансе на стороне сервера в памяти.</span><span class="sxs-lookup"><span data-stu-id="ddad9-117">[express-session](https://github.com/expressjs/session) to store values in an in-memory server-side session.</span></span>
- <span data-ttu-id="ddad9-118">[express-promise-router,](https://github.com/express-promise-router/express-promise-router) позволяющий обработчику маршрутов возвращать обещание.</span><span class="sxs-lookup"><span data-stu-id="ddad9-118">[express-promise-router](https://github.com/express-promise-router/express-promise-router) to allow route handlers to return a Promise.</span></span>
- <span data-ttu-id="ddad9-119">[express-validator](https://github.com/express-validator/express-validator) для анализа и проверки данных формы.</span><span class="sxs-lookup"><span data-stu-id="ddad9-119">[express-validator](https://github.com/express-validator/express-validator) for parsing and validating form data.</span></span>
- <span data-ttu-id="ddad9-120">[msal-node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) для проверки подлинности и получения маркеров доступа.</span><span class="sxs-lookup"><span data-stu-id="ddad9-120">[msal-node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) for authenticating and getting access tokens.</span></span>
- <span data-ttu-id="ddad9-121">[uuid](https://github.com/uuidjs/uuid) используется msal-node для создания GUID.</span><span class="sxs-lookup"><span data-stu-id="ddad9-121">[uuid](https://github.com/uuidjs/uuid) used by msal-node to generate GUIDs.</span></span>
- <span data-ttu-id="ddad9-122">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) для звонков в Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="ddad9-122">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>
- <span data-ttu-id="ddad9-123">[isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) to polyfill the fetch for Node.</span><span class="sxs-lookup"><span data-stu-id="ddad9-123">[isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) to polyfill the fetch for Node.</span></span> <span data-ttu-id="ddad9-124">Для библиотеки требуется извлечение `microsoft-graph-client` полифайла.</span><span class="sxs-lookup"><span data-stu-id="ddad9-124">A fetch polyfill is required for the `microsoft-graph-client` library.</span></span> <span data-ttu-id="ddad9-125">Дополнительные сведения см. в вики-сайте клиентской библиотеки [JavaScript Microsoft Graph.](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required)</span><span class="sxs-lookup"><span data-stu-id="ddad9-125">See the [Microsoft Graph JavaScript client library wiki](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) for more information.</span></span>

1. <span data-ttu-id="ddad9-126">Запустите следующую команду в CLI.</span><span class="sxs-lookup"><span data-stu-id="ddad9-126">Run the following command in your CLI.</span></span>

    ```Shell
    npm install dotenv@8.2.0 moment@2.29.1 moment-timezone@0.5.31 connect-flash@0.1.1 express-session@1.17.1 express-promise-router@4.0.1 isomorphic-fetch@3.0.0
    npm install @azure/msal-node@1.0.0-beta.0 @microsoft/microsoft-graph-client@2.1.1 windows-iana@4.2.1 express-validator@6.6.1 uuid@8.3.1
    ```

    > [!TIP]
    > <span data-ttu-id="ddad9-127">При попытке установить эти пакеты в Windows пользователи Windows могут получить следующее сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="ddad9-127">Windows users may get the following error message when trying to install these packages on Windows.</span></span>
    >
    > ```Shell
    > gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
    > ```
    >
    > <span data-ttu-id="ddad9-128">Чтобы устранить ошибку, запустите следующую команду, чтобы установить средства сборки Windows с помощью окна терминала с повышенными уровнями (администратора), в котором устанавливаются средства сборки VS и Python.</span><span class="sxs-lookup"><span data-stu-id="ddad9-128">To resolve the error, run the following command to install the Windows Build Tools using an elevated (Administrator) terminal window which installs the VS Build Tools and Python.</span></span>
    >
    > ```Shell
    > npm install --global --production windows-build-tools
    > ```

1. <span data-ttu-id="ddad9-129">Обновим приложение, чтобы использовать по `connect-flash` `express-session` середине и по середине.</span><span class="sxs-lookup"><span data-stu-id="ddad9-129">Update the application to use the `connect-flash` and `express-session` middleware.</span></span> <span data-ttu-id="ddad9-130">Откройте **файл ./app.js** добавьте в верхнюю часть файла `require` следующую выписку.</span><span class="sxs-lookup"><span data-stu-id="ddad9-130">Open **./app.js** and add the following `require` statement to the top of the file.</span></span>

    ```javascript
    const session = require('express-session');
    const flash = require('connect-flash');
    const msal = require('@azure/msal-node');
    ```

1. <span data-ttu-id="ddad9-131">Добавьте следующий код сразу после `var app = express();` строки.</span><span class="sxs-lookup"><span data-stu-id="ddad9-131">Add the following code immediately after the `var app = express();` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SessionSnippet":::

## <a name="design-the-app"></a><span data-ttu-id="ddad9-132">Проектирование приложения</span><span class="sxs-lookup"><span data-stu-id="ddad9-132">Design the app</span></span>

<span data-ttu-id="ddad9-133">В этом разделе мы реализуем пользовательский интерфейс приложения.</span><span class="sxs-lookup"><span data-stu-id="ddad9-133">In this section you will implement the UI for the app.</span></span>

1. <span data-ttu-id="ddad9-134">Откройте **./views/layout.hbs** и замените все содержимое следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="ddad9-134">Open **./views/layout.hbs** and replace the entire contents with the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/layout.hbs" id="LayoutSnippet":::

    <span data-ttu-id="ddad9-135">В этом коде [добавляется Bootstrap](http://getbootstrap.com/) для простого стиля, а для некоторых простых значков [font : "Отличный".](https://fontawesome.com/)</span><span class="sxs-lookup"><span data-stu-id="ddad9-135">This code adds [Bootstrap](http://getbootstrap.com/) for simple styling, and [Font Awesome](https://fontawesome.com/) for some simple icons.</span></span> <span data-ttu-id="ddad9-136">Он также определяет глобальный макет с панели nav.</span><span class="sxs-lookup"><span data-stu-id="ddad9-136">It also defines a global layout with a nav bar.</span></span>

1. <span data-ttu-id="ddad9-137">Откройте **./public/stylesheets/style.css** и замените все его содержимое следующим:</span><span class="sxs-lookup"><span data-stu-id="ddad9-137">Open **./public/stylesheets/style.css** and replace its entire contents with the following.</span></span>

    :::code language="css" source="../demo/graph-tutorial/public/stylesheets/style.css":::

1. <span data-ttu-id="ddad9-138">Откройте **./views/index.hbs** и замените его содержимое на следующее.</span><span class="sxs-lookup"><span data-stu-id="ddad9-138">Open **./views/index.hbs** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/index.hbs" id="IndexSnippet":::

1. <span data-ttu-id="ddad9-139">Откройте **./routes/index.js** и замените существующий код следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="ddad9-139">Open **./routes/index.js** and replace the existing code with the following.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/index.js" id="IndexRouterSnippet" highlight="6-10":::

1. <span data-ttu-id="ddad9-140">Сохраните все изменения и перезапустите сервер.</span><span class="sxs-lookup"><span data-stu-id="ddad9-140">Save all of your changes and restart the server.</span></span> <span data-ttu-id="ddad9-141">Теперь приложение должно выглядеть совершенно иначе.</span><span class="sxs-lookup"><span data-stu-id="ddad9-141">Now, the app should look very different.</span></span>

    ![Снимок экрана с измененной домашней страницей](./images/create-app-01.png)
