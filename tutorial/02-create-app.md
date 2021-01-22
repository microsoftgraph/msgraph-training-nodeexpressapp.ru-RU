<!-- markdownlint-disable MD002 MD041 -->

В этом упражнении вы будете использовать [Express](http://expressjs.com/) для создания веб-приложения.

1. Откройте CLI, перейдите в каталог, в котором у вас есть права на создание файлов, и запустите следующую команду, чтобы создать приложение Express, использующее [Handlebars](http://handlebarsjs.com/) в качестве обработщика отрисовки.

    ```Shell
    npx express-generator@4.16.1 --hbs graph-tutorial
    ```

    Генератор Express создает новый каталог, называемый приложением Express, и создает `graph-tutorial` скаолдолды.

1. Перейдите в `graph-tutorial` каталог и введите следующую команду для установки зависимостей.

    ```Shell
    npm install
    ```

1. Чтобы обновить пакеты Node с помощью сообщений об уязвимостях, запустите следующую команду.

    ```Shell
    npm audit fix
    ```

1. Чтобы обновить версию Express и других зависимостей, запустите следующую команду.

    ```Shell
    npm install express@4.17.1 http-errors@1.8.0 morgan@1.10.0 debug@4.1.1
    ```

1. Используйте следующую команду для запуска локального веб-сервера.

    ```Shell
    npm start
    ```

1. Откройте браузер и перейдите по адресу `http://localhost:3000`. Если все работает, вы увидите сообщение "Добро пожаловать в Express". Если вы не видите это сообщение, ознакомьтесь с руководством [по началу работы с Express.](http://expressjs.com/starter/generator.html)

## <a name="install-node-packages"></a>Установка пакетов node

Прежде чем двигаться дальше, установите некоторые дополнительные пакеты, которые вы будете использовать позже:

- [dotenv](https://github.com/motdotla/dotenv) для загрузки значений из ENV-файла.
- [момент](https://github.com/moment/moment/) для форматирования значений даты и времени.
- [windows-iana](https://github.com/rubenillodo/windows-iana) для перевода имен часового пояса Windows в IANA.
- [connect-flash](https://github.com/jaredhanson/connect-flash) к сообщениям об ошибках флэш-накопителя в приложении.
- [express-session](https://github.com/expressjs/session) для хранения значений в сеансе на стороне сервера в памяти.
- [express-promise-router,](https://github.com/express-promise-router/express-promise-router) позволяющий обработчику маршрутов возвращать обещание.
- [express-validator](https://github.com/express-validator/express-validator) для анализа и проверки данных формы.
- [msal-node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) для проверки подлинности и получения маркеров доступа.
- [uuid](https://github.com/uuidjs/uuid) используется msal-node для создания GUID.
- [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) для звонков в Microsoft Graph.
- [isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) to polyfill the fetch for Node. Для библиотеки требуется извлечение `microsoft-graph-client` полифайла. Дополнительные сведения см. в вики-сайте клиентской библиотеки [JavaScript Microsoft Graph.](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required)

1. Запустите следующую команду в CLI.

    ```Shell
    npm install dotenv@8.2.0 moment@2.29.1 moment-timezone@0.5.31 connect-flash@0.1.1 express-session@1.17.1 express-promise-router@4.0.1 isomorphic-fetch@3.0.0
    npm install @azure/msal-node@1.0.0-beta.0 @microsoft/microsoft-graph-client@2.1.1 windows-iana@4.2.1 express-validator@6.6.1 uuid@8.3.1
    ```

    > [!TIP]
    > При попытке установить эти пакеты в Windows пользователи Windows могут получить следующее сообщение об ошибке.
    >
    > ```Shell
    > gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
    > ```
    >
    > Чтобы устранить ошибку, запустите следующую команду, чтобы установить средства сборки Windows с помощью окна терминала с повышенными уровнями (администратора), в котором устанавливаются средства сборки VS и Python.
    >
    > ```Shell
    > npm install --global --production windows-build-tools
    > ```

1. Обновим приложение, чтобы использовать по `connect-flash` `express-session` середине и по середине. Откройте **файл ./app.js** добавьте в верхнюю часть файла `require` следующую выписку.

    ```javascript
    const session = require('express-session');
    const flash = require('connect-flash');
    const msal = require('@azure/msal-node');
    ```

1. Добавьте следующий код сразу после `var app = express();` строки.

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SessionSnippet":::

## <a name="design-the-app"></a>Проектирование приложения

В этом разделе мы реализуем пользовательский интерфейс приложения.

1. Откройте **./views/layout.hbs** и замените все содержимое следующим кодом.

    :::code language="html" source="../demo/graph-tutorial/views/layout.hbs" id="LayoutSnippet":::

    В этом коде [добавляется Bootstrap](http://getbootstrap.com/) для простого стиля, а для некоторых простых значков [font : "Отличный".](https://fontawesome.com/) Он также определяет глобальный макет с панели nav.

1. Откройте **./public/stylesheets/style.css** и замените все его содержимое следующим:

    :::code language="css" source="../demo/graph-tutorial/public/stylesheets/style.css":::

1. Откройте **./views/index.hbs** и замените его содержимое на следующее.

    :::code language="html" source="../demo/graph-tutorial/views/index.hbs" id="IndexSnippet":::

1. Откройте **./routes/index.js** и замените существующий код следующим кодом.

    :::code language="javascript" source="../demo/graph-tutorial/routes/index.js" id="IndexRouterSnippet" highlight="6-10":::

1. Сохраните все изменения и перезапустите сервер. Теперь приложение должно выглядеть совершенно иначе.

    ![Снимок экрана с измененной домашней страницей](./images/create-app-01.png)
