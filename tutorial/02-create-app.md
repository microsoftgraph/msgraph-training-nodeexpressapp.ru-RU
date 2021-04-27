<!-- markdownlint-disable MD002 MD041 -->

В этом упражнении вы будете использовать [Express](http://expressjs.com/) для создания веб-приложения.

1. Откройте CLI, перейдите в каталог, в котором у вас есть права на создание файлов, и запустите следующую команду, чтобы создать новое приложение Express, которое использует [handlebars](http://handlebarsjs.com/) в качестве двигателя отрисовки.

    ```Shell
    npx express-generator@4.16.1 --hbs graph-tutorial
    ```

    Генератор Express создает новый каталог под названием `graph-tutorial` и создает приложение Express.

1. Перейдите в `graph-tutorial` каталог и введите следующую команду для установки зависимостей.

    ```Shell
    npm install
    ```

1. Запустите следующую команду, чтобы обновить пакеты Node с сообщаемой уязвимостью.

    ```Shell
    npm audit fix
    ```

1. Запустите следующую команду для обновления версии Express и других зависимостей.

    ```Shell
    npm install express@4.17.1 http-errors@1.8.0 morgan@1.10.0 debug@4.3.1 hbs@4.1.2
    ```

1. Чтобы запустить локальный веб-сервер, используйте следующую команду.

    ```Shell
    npm start
    ```

1. Откройте браузер и перейдите по адресу `http://localhost:3000`. Если все работает, вы увидите сообщение "Добро пожаловать в экспресс". Если вы не видите это сообщение, проверьте руководство по началу [работы Express.](http://expressjs.com/starter/generator.html)

## <a name="install-node-packages"></a>Установка пакетов node

Прежде чем двигаться дальше, установите дополнительные пакеты, которые вы будете использовать позже:

- [dotenv](https://github.com/motdotla/dotenv) для загрузки значений из файла .env.
- [date-fns](https://github.com/date-fns/date-fns) для форматирования значений даты и времени.
- [windows-iana](https://github.com/rubenillodo/windows-iana) для перевода имен Windows часового пояса в IANA.
- [подключение-вспышка](https://github.com/jaredhanson/connect-flash) для сообщений об ошибках вспышки в приложении.
- [экспресс-сеанс](https://github.com/expressjs/session) для хранения значений в сеансе сервера в памяти.
- [маршрутизатор express-promise-router,](https://github.com/express-promise-router/express-promise-router) позволяющий обработчику маршрутов возвращать обещание.
- [экспресс-валидатор](https://github.com/express-validator/express-validator) для анализа и проверки данных форм.
- [msal-node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) для проверки подлинности и получения маркеров доступа.
- [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) для звонков в Microsoft Graph.
- [isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) для полифайла для получения узла. Для библиотеки требуется извлечение `microsoft-graph-client` полифайла. Дополнительные сведения [см. Graph вики клиентской библиотеки Microsoft Graph JavaScript.](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required)

1. Запустите следующую команду в CLI.

    ```Shell
    npm install dotenv@8.2.0 date-fns@2.21.1 date-fns-tz@1.1.4 connect-flash@0.1.1 express-validator@6.10.0
    npm install express-session@1.17.1 express-promise-router@4.1.0 isomorphic-fetch@3.0.0
    npm install @azure/msal-node@1.0.2 @microsoft/microsoft-graph-client@2.2.1 windows-iana@5.0.1
    ```

    > [!TIP]
    > Windows пользователи могут получить следующее сообщение об ошибке при попытке установить эти пакеты на Windows.
    >
    > ```Shell
    > gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
    > ```
    >
    > Чтобы устранить ошибку, запустите следующую команду, чтобы установить инструменты сборки Windows с помощью окна терминала повышенного уровня (Администратор), которое устанавливает средства сборки VS и Python.
    >
    > ```Shell
    > npm install --global --production windows-build-tools
    > ```

1. Обнови приложение для использования `connect-flash` программного обеспечения и `express-session` среднего программного обеспечения. Откройте **./app.js** и добавьте следующее `require` утверждение в верхнюю часть файла.

    ```javascript
    const session = require('express-session');
    const flash = require('connect-flash');
    const msal = require('@azure/msal-node');
    ```

1. Добавьте следующий код сразу после `var app = express();` строки.

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SessionSnippet":::

## <a name="design-the-app"></a>Проектирование приложения

В этом разделе будет реализован пользовательский интерфейс приложения.

1. Откройте **./views/layout.hbs** и замените все содержимое следующим кодом.

    :::code language="html" source="../demo/graph-tutorial/views/layout.hbs" id="LayoutSnippet":::

    Этот код добавляет [Bootstrap](http://getbootstrap.com/) для простого стиля и [Font Awesome](https://fontawesome.com/) для некоторых простых значков. Он также определяет глобальную макет с панели nav.

1. Откройте **таблицы ./public/stylesheets/style.css** и замените все содержимое следующим образом.

    :::code language="css" source="../demo/graph-tutorial/public/stylesheets/style.css":::

1. Откройте **./views/index.hbs** и замените его содержимое следующим.

    :::code language="html" source="../demo/graph-tutorial/views/index.hbs" id="IndexSnippet":::

1. Откройте **./routes/index.js** и замените существующий код следующим.

    :::code language="javascript" source="../demo/graph-tutorial/routes/index.js" id="IndexRouterSnippet" highlight="6-10":::

1. Сохраните все изменения и перезапустите сервер. Теперь приложение должно выглядеть совсем по-другому.

    ![Снимок экрана: обновленная домашняя страница](./images/create-app-01.png)
