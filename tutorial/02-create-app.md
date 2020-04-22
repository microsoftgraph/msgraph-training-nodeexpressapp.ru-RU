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

1. Используйте следующую команду для запуска локального веб-сервера.

    ```Shell
    npm start
    ```

1. Откройте браузер и перейдите по адресу `http://localhost:3000`. Если все работает, вы увидите сообщение "Добро пожаловать в Express". Если вы не видите это сообщение, ознакомьтесь с [руководством Express Приступая к началу работы](http://expressjs.com/starter/generator.html).

## <a name="install-node-packages"></a>Установка пакетов узлов

Прежде чем переходить, установите несколько дополнительных пакетов, которые будут использоваться позже:

- [дотенв](https://github.com/motdotla/dotenv) для загрузки значений из файла env.
- [время для форматирования](https://github.com/moment/moment/) значений даты и времени.
- сообщение об ошибке [Connect-Flash](https://github.com/jaredhanson/connect-flash) to Flash в приложении.
- [Express — сеанс](https://github.com/expressjs/session) для хранения значений в серверном сеансе в памяти на стороне сервера.
- [Passport — Azure — AD](https://github.com/AzureAD/passport-azure-ad) для проверки подлинности и получения маркеров доступа.
- [Simple — OAuth2](https://github.com/lelylan/simple-oauth2) для управления маркерами.
- [Microsoft — Graph — клиент](https://github.com/microsoftgraph/msgraph-sdk-javascript) для совершения звонков в Microsoft Graph.
- [исоморфик — получение](https://github.com/matthew-andrews/isomorphic-fetch) для заполнения для узла. Для `microsoft-graph-client` библиотеки необходима получающая заливка. Для получения дополнительных сведений посетите [вики-сайт клиентской библиотеки JavaScript для Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) .

1. Выполните следующую команду в командной панели CLI.

    ```Shell
    npm install dotenv@8.2.0 moment@2.24.0 connect-flash@0.1.1 express-session@1.17.0 isomorphic-fetch@2.2.1
    npm install passport-azure-ad@4.2.1 simple-oauth2@3.3.0 @microsoft/microsoft-graph-client@2.0.0
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

1. Обновите приложение, чтобы оно `connect-flash` использовалось по `express-session` промежуточному программному отношению. Откройте `./app.js` файл и добавьте приведенный ниже `require` оператор в начало файла.

    ```javascript
    var session = require('express-session');
    var flash = require('connect-flash');
    ```

1. Добавьте приведенный ниже код сразу после `var app = express();` строки.

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SessionSnippet":::

## <a name="design-the-app"></a>Проектирование приложения

В этом разделе будет реализован пользовательский интерфейс приложения.

1. Откройте `./views/layout.hbs` файл и замените все его содержимое приведенным ниже кодом.

    :::code language="html" source="../demo/graph-tutorial/views/layout.hbs" id="LayoutSnippet":::

    В этом коде добавляется [Начальная](http://getbootstrap.com/) загрузка для простых стилей и [Шрифт Awesome](https://fontawesome.com/) для некоторых простых значков. Он также определяет глобальную структуру с помощью панели навигации.

1. Откройте `./public/stylesheets/style.css` и замените все содержимое приведенным ниже.

    :::code language="css" source="../demo/graph-tutorial/public/stylesheets/style.css":::

1. Откройте `./views/index.hbs` файл и замените его содержимое на приведенный ниже код.

    :::code language="html" source="../demo/graph-tutorial/views/index.hbs" id="IndexSnippet":::

1. Откройте `./routes/index.js` файл и замените существующий код приведенным ниже кодом.

    :::code language="javascript" source="../demo/graph-tutorial/routes/index.js" id="IndexRouterSnippet" highlight="6-10":::

1. Сохраните все изменения и перезапустите сервер. Теперь приложение должно выглядеть по-другому.

    ![Снимок экрана с переработанной домашней страницей](./images/create-app-01.png)
