<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="1e5e7-101">В этом упражнении вы будете использовать [Express](http://expressjs.com/) для создания веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-101">In this exercise you will use [Express](http://expressjs.com/) to build a web app.</span></span>

1. <span data-ttu-id="1e5e7-102">Откройте подсистему CLI, перейдите к каталогу, в котором у вас есть права на создание файлов, и выполните следующую команду, чтобы создать новое приложение Express, использующее [handlebars](http://handlebarsjs.com/) в качестве обработчика визуализации.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-102">Open your CLI, navigate to a directory where you have rights to create files, and run the following command to create a new Express app that uses [Handlebars](http://handlebarsjs.com/) as the rendering engine.</span></span>

    ```Shell
    npx express-generator@4.16.1 --hbs graph-tutorial
    ```

    <span data-ttu-id="1e5e7-103">Генератор Express создает новый каталог с именем `graph-tutorial` и формированием шаблонов для приложения Express.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-103">The Express generator creates a new directory called `graph-tutorial` and scaffolds an Express app.</span></span>

1. <span data-ttu-id="1e5e7-104">Перейдите в `graph-tutorial` каталог и введите следующую команду, чтобы установить зависимости.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-104">Navigate to the `graph-tutorial` directory and enter the following command to install dependencies.</span></span>

    ```Shell
    npm install
    ```

1. <span data-ttu-id="1e5e7-105">Выполните следующую команду, чтобы обновить пакеты узлов с обнаруженными уязвимостями.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-105">Run the following command to update Node packages with reported vulnerabilities.</span></span>

    ```Shell
    npm audit fix
    ```

1. <span data-ttu-id="1e5e7-106">Выполните следующую команду, чтобы обновить версию Express и другие зависимости.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-106">Run the following command to update the version of Express and other dependencies.</span></span>

    ```Shell
    npm install express@4.17.1 http-errors@1.8.0 morgan@1.10.0 debug@4.1.1
    ```

1. <span data-ttu-id="1e5e7-107">Используйте следующую команду для запуска локального веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-107">Use the following command to start a local web server.</span></span>

    ```Shell
    npm start
    ```

1. <span data-ttu-id="1e5e7-108">Откройте браузер и перейдите по адресу `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-108">Open your browser and navigate to `http://localhost:3000`.</span></span> <span data-ttu-id="1e5e7-109">Если все работает, вы увидите сообщение "Добро пожаловать в Express".</span><span class="sxs-lookup"><span data-stu-id="1e5e7-109">If everything is working, you will see a "Welcome to Express" message.</span></span> <span data-ttu-id="1e5e7-110">Если вы не видите это сообщение, ознакомьтесь с [руководством Express Приступая к началу работы](http://expressjs.com/starter/generator.html).</span><span class="sxs-lookup"><span data-stu-id="1e5e7-110">If you don't see that message, check the [Express getting started guide](http://expressjs.com/starter/generator.html).</span></span>

## <a name="install-node-packages"></a><span data-ttu-id="1e5e7-111">Установка пакетов узлов</span><span class="sxs-lookup"><span data-stu-id="1e5e7-111">Install Node packages</span></span>

<span data-ttu-id="1e5e7-112">Прежде чем переходить, установите несколько дополнительных пакетов, которые будут использоваться позже:</span><span class="sxs-lookup"><span data-stu-id="1e5e7-112">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="1e5e7-113">[дотенв](https://github.com/motdotla/dotenv) для загрузки значений из файла env.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-113">[dotenv](https://github.com/motdotla/dotenv) for loading values from a .env file.</span></span>
- <span data-ttu-id="1e5e7-114">[время для форматирования](https://github.com/moment/moment/) значений даты и времени.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-114">[moment](https://github.com/moment/moment/) for formatting date/time values.</span></span>
- <span data-ttu-id="1e5e7-115">[Windows — IANA](https://github.com/rubenillodo/windows-iana) для преобразования названий часовых поясов Windows в идентификаторы IANA часовых поясов.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-115">[windows-iana](https://github.com/rubenillodo/windows-iana) for translating Windows time zone names to IANA time zone IDs.</span></span>
- <span data-ttu-id="1e5e7-116">сообщение об ошибке [Connect-Flash](https://github.com/jaredhanson/connect-flash) to Flash в приложении.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-116">[connect-flash](https://github.com/jaredhanson/connect-flash) to flash error messages in the app.</span></span>
- <span data-ttu-id="1e5e7-117">[Express — сеанс](https://github.com/expressjs/session) для хранения значений в серверном сеансе в памяти на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-117">[express-session](https://github.com/expressjs/session) to store values in an in-memory server-side session.</span></span>
- <span data-ttu-id="1e5e7-118">[Express – Promise — маршрутизатор](https://github.com/express-promise-router/express-promise-router) , позволяющий обработчикам маршрутов возвращать обещание.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-118">[express-promise-router](https://github.com/express-promise-router/express-promise-router) to allow route handlers to return a Promise.</span></span>
- <span data-ttu-id="1e5e7-119">[Express — средство проверки](https://github.com/express-validator/express-validator) для синтаксического анализа и проверки данных формы.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-119">[express-validator](https://github.com/express-validator/express-validator) for parsing and validating form data.</span></span>
- <span data-ttu-id="1e5e7-120">[msal — узел](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) для проверки подлинности и получения маркеров доступа.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-120">[msal-node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) for authenticating and getting access tokens.</span></span>
- <span data-ttu-id="1e5e7-121">[идентификатор UUID](https://github.com/uuidjs/uuid) , используемый узлом msal для создания идентификаторов GUID.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-121">[uuid](https://github.com/uuidjs/uuid) used by msal-node to generate GUIDs.</span></span>
- <span data-ttu-id="1e5e7-122">[Microsoft — Graph — клиент](https://github.com/microsoftgraph/msgraph-sdk-javascript) для совершения звонков в Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-122">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>
- <span data-ttu-id="1e5e7-123">[исоморфик — получение](https://github.com/matthew-andrews/isomorphic-fetch) для заполнения для узла.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-123">[isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) to polyfill the fetch for Node.</span></span> <span data-ttu-id="1e5e7-124">Для библиотеки необходима получающая заливка `microsoft-graph-client` .</span><span class="sxs-lookup"><span data-stu-id="1e5e7-124">A fetch polyfill is required for the `microsoft-graph-client` library.</span></span> <span data-ttu-id="1e5e7-125">Для получения дополнительных сведений посетите [вики-сайт клиентской библиотеки JavaScript для Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) .</span><span class="sxs-lookup"><span data-stu-id="1e5e7-125">See the [Microsoft Graph JavaScript client library wiki](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) for more information.</span></span>

1. <span data-ttu-id="1e5e7-126">Выполните следующую команду в командной панели CLI.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-126">Run the following command in your CLI.</span></span>

    ```Shell
    npm install dotenv@8.2.0 moment@2.29.1 moment-timezone@0.5.31 connect-flash@0.1.1 express-session@1.17.1 isomorphic-fetch@3.0.0
    npm install @azure/msal-node@1.0.0-beta.0 @microsoft/microsoft-graph-client@2.1.1 windows-iana@4.2.1 express-validator@6.6.1 uuid@8.3.1
    ```

    > [!TIP]
    > <span data-ttu-id="1e5e7-127">При попытке установить эти пакеты в Windows пользователи Windows могут получить следующее сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-127">Windows users may get the following error message when trying to install these packages on Windows.</span></span>
    >
    > ```Shell
    > gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
    > ```
    >
    > <span data-ttu-id="1e5e7-128">Чтобы устранить эту ошибку, выполните следующую команду, чтобы установить средства построения Windows с помощью окна терминала с повышенными привилегиями (администратора), которое устанавливает инструменты VS Build и Python.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-128">To resolve the error, run the following command to install the Windows Build Tools using an elevated (Administrator) terminal window which installs the VS Build Tools and Python.</span></span>
    >
    > ```Shell
    > npm install --global --production windows-build-tools
    > ```

1. <span data-ttu-id="1e5e7-129">Обновите приложение, чтобы оно использовалось по промежуточному программному отношению `connect-flash` `express-session` .</span><span class="sxs-lookup"><span data-stu-id="1e5e7-129">Update the application to use the `connect-flash` and `express-session` middleware.</span></span> <span data-ttu-id="1e5e7-130">Откройте **./app.js** и добавьте следующий `require` оператор в начало файла.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-130">Open **./app.js** and add the following `require` statement to the top of the file.</span></span>

    ```javascript
    const session = require('express-session');
    const flash = require('connect-flash');
    const msal = require('@azure/msal-node');
    ```

1. <span data-ttu-id="1e5e7-131">Добавьте приведенный ниже код сразу после `var app = express();` строки.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-131">Add the following code immediately after the `var app = express();` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SessionSnippet":::

## <a name="design-the-app"></a><span data-ttu-id="1e5e7-132">Проектирование приложения</span><span class="sxs-lookup"><span data-stu-id="1e5e7-132">Design the app</span></span>

<span data-ttu-id="1e5e7-133">В этом разделе будет реализован пользовательский интерфейс приложения.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-133">In this section you will implement the UI for the app.</span></span>

1. <span data-ttu-id="1e5e7-134">Откройте **/виевс/лайаут.ХБС** и замените все содержимое приведенным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-134">Open **./views/layout.hbs** and replace the entire contents with the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/layout.hbs" id="LayoutSnippet":::

    <span data-ttu-id="1e5e7-135">В этом коде добавляется [Начальная](http://getbootstrap.com/) загрузка для простых стилей и [Шрифт Awesome](https://fontawesome.com/) для некоторых простых значков.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-135">This code adds [Bootstrap](http://getbootstrap.com/) for simple styling, and [Font Awesome](https://fontawesome.com/) for some simple icons.</span></span> <span data-ttu-id="1e5e7-136">Он также определяет глобальную структуру с помощью панели навигации.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-136">It also defines a global layout with a nav bar.</span></span>

1. <span data-ttu-id="1e5e7-137">Откройте **/публик/стилешитс/стиле.КСС** и замените все содержимое следующим.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-137">Open **./public/stylesheets/style.css** and replace its entire contents with the following.</span></span>

    :::code language="css" source="../demo/graph-tutorial/public/stylesheets/style.css":::

1. <span data-ttu-id="1e5e7-138">Откройте **/виевс/индекс.ХБС** и замените его содержимое приведенным ниже.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-138">Open **./views/index.hbs** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/index.hbs" id="IndexSnippet":::

1. <span data-ttu-id="1e5e7-139">Откройте **/раутес/index.js** и замените имеющийся код следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-139">Open **./routes/index.js** and replace the existing code with the following.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/index.js" id="IndexRouterSnippet" highlight="6-10":::

1. <span data-ttu-id="1e5e7-140">Сохраните все изменения и перезапустите сервер.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-140">Save all of your changes and restart the server.</span></span> <span data-ttu-id="1e5e7-141">Теперь приложение должно выглядеть по-другому.</span><span class="sxs-lookup"><span data-stu-id="1e5e7-141">Now, the app should look very different.</span></span>

    ![Снимок экрана с переработанной домашней страницей](./images/create-app-01.png)
