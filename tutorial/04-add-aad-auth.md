<!-- markdownlint-disable MD002 MD041 -->

В этом упражнении вы расширим приложение из предыдущего упражнения, чтобы поддерживать проверку подлинности с помощью Azure AD. Это необходимо для получения необходимого маркера доступа OAuth для вызова Microsoft Graph. На этом этапе вы интегрируете [библиотеку msal-node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) в приложение.

1. Создайте файл **с именем ENV** в корне приложения и добавьте следующий код.

    :::code language="ini" source="../demo/graph-tutorial/example.env":::

    Замените ИД приложения на портале регистрации приложений и замените созданный секрет `YOUR_CLIENT_SECRET_HERE` `YOUR_APP_SECRET_HERE` клиента.

    > [!IMPORTANT]
    > Если вы используете управление исходным кодом, например git, пришло бы время исключить **ENV-файл** из системы управления исходным кодом, чтобы избежать случайной утечки ИД приложения и пароля.

1. Откройте **файл ./app.js** и добавьте следующую строку в верхнюю часть файла для загрузки **ENV-файла.**

    ```javascript
    require('dotenv').config();
    ```

## <a name="implement-sign-in"></a>Реализация входов

1. Найдите `var app = express();` строку **в ./app.js**. Вставьте следующий код **после** этой строки.

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="MsalInitSnippet":::

    Этот код инициализирует библиотеку msal-node с помощью кода и пароля приложения.

1. Создайте новый файл в **каталоге ./routes** с именемauth.jsи **добавьте** следующий код.

    ```javascript
    var router = require('express-promise-router')();

    /* GET auth callback. */
    router.get('/signin',
      async function (req, res) {
        const urlParameters = {
          scopes: process.env.OAUTH_SCOPES.split(','),
          redirectUri: process.env.OAUTH_REDIRECT_URI
        };

        try {
          const authUrl = await req.app.locals
            .msalClient.getAuthCodeUrl(urlParameters);
          res.redirect(authUrl);
        }
        catch (error) {
          console.log(`Error: ${error}`);
          req.flash('error_msg', {
            message: 'Error getting auth URL',
            debug: JSON.stringify(error, Object.getOwnPropertyNames(error))
          });
          res.redirect('/');
        }
      }
    );

    router.get('/callback',
      async function(req, res) {
        const tokenRequest = {
          code: req.query.code,
          scopes: process.env.OAUTH_SCOPES.split(','),
          redirectUri: process.env.OAUTH_REDIRECT_URI
        };

        try {
          const response = await req.app.locals
            .msalClient.acquireTokenByCode(tokenRequest);

          // TEMPORARY!
          // Flash the access token for testing purposes
          req.flash('error_msg', {
            message: 'Access token',
            debug: response.accessToken
          });
        } catch (error) {
          req.flash('error_msg', {
            message: 'Error completing authentication',
            debug: JSON.stringify(error, Object.getOwnPropertyNames(error))
          });
        }

        res.redirect('/');
      }
    );

    router.get('/signout',
      async function(req, res) {
        // Sign out
        if (req.session.userId) {
          // Look up the user's account in the cache
          const accounts = await req.app.locals.msalClient
            .getTokenCache()
            .getAllAccounts();

          const userAccount = accounts.find(a => a.homeAccountId === req.session.userId);

          // Remove the account
          if (userAccount) {
            req.app.locals.msalClient
              .getTokenCache()
              .removeAccount(userAccount);
          }
        }

        // Destroy the user's session
        req.session.destroy(function (err) {
          res.redirect('/');
        });
      }
    );

    module.exports = router;
    ```

    Он определяет маршрутизатор с тремя маршрутами: `signin` , `callback` и `signout` .

    Маршрут `signin` вызывает функцию для создания URL-адреса входа, а затем перенаправляет `getAuthCodeUrl` браузер на этот URL-адрес.

    Маршрут `callback` — это маршрут, через который Azure перенаправляет вас после завершения регистрации. Код вызывает `acquireTokenByCode` функцию для обмена кодом авторизации на маркер доступа. После получения маркера он перенаправляется обратно на домашную страницу с маркером доступа во временном значении ошибки. Мы будем использовать его, чтобы убедиться, что вход работает, прежде чем двигаться дальше. Перед тестированием необходимо настроить приложение Express на использование нового маршрутизатора **из ./routes/auth.js.**

    Метод `signout` занося пользователя в журнал и уничтожает сеанс.

1. Откройте **./app.js** и вставьте следующий код **перед** `var app = express();` строкой.

    ```javascript
    var authRouter = require('./routes/auth');
    ```

1. Вставьте следующий код **после** `app.use('/', indexRouter);` строки.

    ```javascript
    app.use('/auth', authRouter);
    ```

Запустите сервер и перейдите к `https://localhost:3000` . Нажмите кнопку "Вход" и перенаправите вас на `https://login.microsoftonline.com` . Войдите в систему с помощью учетной записи Майкрософт и согласиться на запрашиваемую разрешения. Браузер перенаправляет пользователя в приложение с отображением маркера.

### <a name="get-user-details"></a>Получить сведения о пользователе

1. Создайте файл в корневой папке проекта с именем **graph.js** добавьте следующий код.

    ```javascript
    var graph = require('@microsoft/microsoft-graph-client');
    require('isomorphic-fetch');

    module.exports = {
      getUserDetails: async function(accessToken) {
        const client = getAuthenticatedClient(accessToken);

        const user = await client
          .api('/me')
          .select('displayName,mail,mailboxSettings,userPrincipalName')
          .get();
        return user;
      },
    };

    function getAuthenticatedClient(accessToken) {
      // Initialize Graph client
      const client = graph.Client.init({
        // Use the provided access token to authenticate
        // requests
        authProvider: (done) => {
          done(null, accessToken);
        }
      });

      return client;
    }
    ```

    При этом экспортируется функция, которая использует microsoft Graph SDK для вызова конечной точки `getUserDetails` `/me` и возврата результата.

1. Откройте **файл ./routes/auth.js** добавьте следующие утверждения в `require` верхнюю часть файла.

    ```javascript
    var graph = require('../graph');
    ```

1. Замените существующий маршрут вызова следующим кодом.

    :::code language="javascript" source="../demo/graph-tutorial/routes/auth.js" id="CallbackSnippet" highlight="13-23":::

    Новый код сохраняет код учетной записи пользователя в сеансе, получает сведения о пользователе из Microsoft Graph и сохраняет его в хранилище пользователя приложения.

1. Перезапустите сервер и войдите в нее. Вы должны вернуться на home-страницу, но пользовательский интерфейс должен измениться, чтобы указать, что вы вписались.

    ![Снимок экрана с домашней страницей после входов](./images/add-aad-auth-01.png)

1. Щелкните аватар пользователя в правом верхнем углу, чтобы получить доступ к ссылке **"Выйти".** При **нажатии** кнопки "Выйти" сеанс сбрасывается и возвращается на домашней странице.

    ![Снимок экрана с выпадающим меню со ссылкой "Выйти"](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a>Хранение и обновление маркеров

На этом этапе приложение имеет маркер доступа, который отправляется в `Authorization` заголовке вызовов API. Это маркер, который позволяет приложению получать доступ к Microsoft Graph от имени пользователя.

Однако этот маркер является кратковременной. Срок действия маркера истекает через час после его выпуска. В этом случае маркер обновления становится полезным. Спецификация OAuth представляет маркер обновления, который позволяет приложению запрашивать новый маркер доступа, не требуя от пользователя повторного входить в систему.

Так как приложение использует пакет msal-node, вам не нужно реализовывать логику хранения маркеров или обновления. Приложение использует кэш маркеров msal-node в памяти по умолчанию, достаточный для примера приложения. Производственные приложения должны [](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-node/docs/configuration.md) предоставлять собственный подключаемый модуль кэширования для сериализации кэша маркеров в безопасном надежном хранилище.
