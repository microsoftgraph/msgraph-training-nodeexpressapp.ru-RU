<!-- markdownlint-disable MD002 MD041 -->

В этом упражнении вы будете использовать [Express](http://expressjs.com/) для создания веб-приложения.

1. Откройте подсистему CLI, перейдите к каталогу, в котором у вас есть права на создание файлов, и выполните следующую команду, чтобы создать новое приложение Express, использующее [handlebars](http://handlebarsjs.com/) в качестве обработчика визуализации.

    ```Shell
    npx express-generator@4.16.1 --hbs graph-tutorial
    ```

    Генератор Express создает новый каталог с именем `graph-tutorial` и формированием шаблонов для приложения Express.

1. Перейдите в `graph-tutorial` каталог и введите следующую команду, чтобы установить зависимости.

    ```Shell
    npm install
    ```

1. Выполните следующую команду, чтобы обновить пакеты узлов с обнаруженными уязвимостями.

    ```Shell
    npm audit fix
    ```

1. Выполните следующую команду, чтобы обновить версию Express и другие зависимости.

    ```Shell
    npm install express@4.17.1 http-errors@1.8.0 morgan@1.10.0 debug@4.1.1
    ```

1. Используйте следующую команду для запуска локального веб-сервера.

    ```Shell
    npm start
    ```

1. Откройте браузер и перейдите по адресу `http://localhost:3000`. Если все работает, вы увидите сообщение "Добро пожаловать в Express". Если вы не видите это сообщение, ознакомьтесь с [руководством Express Приступая к началу работы](http://expressjs.com/starter/generator.html).

## <a name="install-node-packages"></a>Установка пакетов узлов

Прежде чем переходить, установите несколько дополнительных пакетов, которые будут использоваться позже:

- [дотенв](https://github.com/motdotla/dotenv) для загрузки значений из файла env.
- [время для форматирования](https://github.com/moment/moment/) значений даты и времени.
- [Windows — IANA](https://github.com/rubenillodo/windows-iana) для преобразования названий часовых поясов Windows в идентификаторы IANA часовых поясов.
- сообщение об ошибке [Connect-Flash](https://github.com/jaredhanson/connect-flash) to Flash в приложении.
- [Express — сеанс](https://github.com/expressjs/session) для хранения значений в серверном сеансе в памяти на стороне сервера.
- [Express – Promise — маршрутизатор](https://github.com/express-promise-router/express-promise-router) , позволяющий обработчикам маршрутов возвращать обещание.
- [Express — средство проверки](https://github.com/express-validator/express-validator) для синтаксического анализа и проверки данных формы.
- [msal — узел](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) для проверки подлинности и получения маркеров доступа.
- [идентификатор UUID](https://github.com/uuidjs/uuid) , используемый узлом msal для создания идентификаторов GUID.
- [Microsoft — Graph — клиент](https://github.com/microsoftgraph/msgraph-sdk-javascript) для совершения звонков в Microsoft Graph.
- [исоморфик — получение](https://github.com/matthew-andrews/isomorphic-fetch) для заполнения для узла. Для библиотеки необходима получающая заливка `microsoft-graph-client` . Для получения дополнительных сведений посетите [вики-сайт клиентской библиотеки JavaScript для Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) .

1. Выполните следующую команду в командной панели CLI.

    ```Shell
    npm install dotenv@8.2.0 moment@2.29.1 moment-timezone@0.5.31 connect-flash@0.1.1 express-session@1.17.1 isomorphic-fetch@3.0.0
    npm install @azure/msal-node@1.0.0-beta.0 @microsoft/microsoft-graph-client@2.1.1 windows-iana@4.2.1 express-validator@6.6.1 uuid@8.3.1
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

1. Обновите приложение, чтобы оно использовалось по промежуточному программному отношению `connect-flash` `express-session` . Откройте **./app.js** и добавьте следующий `require` оператор в начало файла.

    ```javascript
    const session = require('express-session');
    const flash = require('connect-flash');
    const msal = require('@azure/msal-node');
    ```

1. Добавьте приведенный ниже код сразу после `var app = express();` строки.

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SessionSnippet":::

## <a name="design-the-app"></a>Проектирование приложения

В этом разделе будет реализован пользовательский интерфейс приложения.

1. Откройте **/виевс/лайаут.ХБС** и замените все содержимое приведенным ниже кодом.

    :::code language="html" source="../demo/graph-tutorial/views/layout.hbs" id="LayoutSnippet":::

    В этом коде добавляется [Начальная](http://getbootstrap.com/) загрузка для простых стилей и [Шрифт Awesome](https://fontawesome.com/) для некоторых простых значков. Он также определяет глобальную структуру с помощью панели навигации.

1. Откройте **/публик/стилешитс/стиле.КСС** и замените все содержимое следующим.

    :::code language="css" source="../demo/graph-tutorial/public/stylesheets/style.css":::

1. Откройте **/виевс/индекс.ХБС** и замените его содержимое приведенным ниже.

    :::code language="html" source="../demo/graph-tutorial/views/index.hbs" id="IndexSnippet":::

1. Откройте **/раутес/index.js** и замените имеющийся код следующим кодом.

    :::code language="javascript" source="../demo/graph-tutorial/routes/index.js" id="IndexRouterSnippet" highlight="6-10":::

1. Сохраните все изменения и перезапустите сервер. Теперь приложение должно выглядеть по-другому.

    ![Снимок экрана с переработанной домашней страницей](./images/create-app-01.png)
