<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="b7c24-101">В этом упражнении вы будете расширяем приложение из предыдущего упражнения для поддержки проверки подлинности с помощью Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b7c24-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="b7c24-102">Это необходимо для получения необходимого маркера доступа OAuth для вызова Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="b7c24-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="b7c24-103">На этом этапе в приложение будет интегрирована библиотека [msal – node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) .</span><span class="sxs-lookup"><span data-stu-id="b7c24-103">In this step you will integrate the [msal-node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) library into the application.</span></span>

1. <span data-ttu-id="b7c24-104">Создайте файл с именем **. env** в корневом каталоге приложения и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="b7c24-104">Create a new file named **.env** in the root of your application, and add the following code.</span></span>

    :::code language="ini" source="../demo/graph-tutorial/example.env":::

    <span data-ttu-id="b7c24-105">Замените `YOUR_CLIENT_SECRET_HERE` идентификатором приложения на портале регистрации приложений и замените на `YOUR_APP_SECRET_HERE` созданный секрет клиента.</span><span class="sxs-lookup"><span data-stu-id="b7c24-105">Replace `YOUR_CLIENT_SECRET_HERE` with the application ID from the Application Registration Portal, and replace `YOUR_APP_SECRET_HERE` with the client secret you generated.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="b7c24-106">Если вы используете систему управления версиями (например, Git), то теперь будет полезно исключить файл **env** из системы управления версиями, чтобы избежать непреднамеренного утечки идентификатора и пароля приложения.</span><span class="sxs-lookup"><span data-stu-id="b7c24-106">If you're using source control such as git, now would be a good time to exclude the **.env** file from source control to avoid inadvertently leaking your app ID and password.</span></span>

1. <span data-ttu-id="b7c24-107">Откройте файл **./app.js** и добавьте следующую строку в начало файла, чтобы загрузить **env** -файл.</span><span class="sxs-lookup"><span data-stu-id="b7c24-107">Open **./app.js** and add the following line to the top of the file to load the **.env** file.</span></span>

    ```javascript
    require('dotenv').config();
    ```

## <a name="implement-sign-in"></a><span data-ttu-id="b7c24-108">Реализация входа</span><span class="sxs-lookup"><span data-stu-id="b7c24-108">Implement sign-in</span></span>

1. <span data-ttu-id="b7c24-109">Откройте строку `var indexRouter = require('./routes/index');` в файле **./app.js**.</span><span class="sxs-lookup"><span data-stu-id="b7c24-109">Locate the line `var indexRouter = require('./routes/index');` in **./app.js**.</span></span> <span data-ttu-id="b7c24-110">Вставьте следующий код **перед** строкой.</span><span class="sxs-lookup"><span data-stu-id="b7c24-110">Insert the following code **before** that line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="MsalInitSnippet":::

    <span data-ttu-id="b7c24-111">Этот код инициализирует библиотеку msal – node с ИДЕНТИФИКАТОРом и паролем приложения.</span><span class="sxs-lookup"><span data-stu-id="b7c24-111">This code initializes the msal-node library with the app ID and password for the app.</span></span>

1. <span data-ttu-id="b7c24-112">Создайте новый файл в каталоге **./раутес** с именем **auth.js** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="b7c24-112">Create a new file in the **./routes** directory named **auth.js** and add the following code.</span></span>

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

    <span data-ttu-id="b7c24-113">Этот параметр определяет маршрутизатор с тремя маршрутами: `signin` , `callback` , и `signout` .</span><span class="sxs-lookup"><span data-stu-id="b7c24-113">This defines a router with three routes: `signin`, `callback`, and `signout`.</span></span>

    <span data-ttu-id="b7c24-114">`signin`Маршрут вызывает `getAuthCodeUrl` функцию для создания URL-адреса входа, а затем перенаправляет браузер на этот URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="b7c24-114">The `signin` route calls the `getAuthCodeUrl` function to generate the login URL, then redirects the browser to that URL.</span></span>

    <span data-ttu-id="b7c24-115">`callback`Маршрут выполняет перенаправление Azure после завершения входа.</span><span class="sxs-lookup"><span data-stu-id="b7c24-115">The `callback` route is where Azure redirects after the signin is complete.</span></span> <span data-ttu-id="b7c24-116">Код вызывает `acquireTokenByCode` функцию для обмена кодом авторизации для маркера доступа.</span><span class="sxs-lookup"><span data-stu-id="b7c24-116">The code calls the `acquireTokenByCode` function to exchange the authorization code for an access token.</span></span> <span data-ttu-id="b7c24-117">После получения маркера он перенаправляется обратно на домашнюю страницу с маркером доступа во временном значении ошибки.</span><span class="sxs-lookup"><span data-stu-id="b7c24-117">Once the token is obtained, it redirects back to the home page with the access token in the temporary error value.</span></span> <span data-ttu-id="b7c24-118">Мы будем использовать эту проверку, чтобы убедиться, что наш вход работает перед переходом.</span><span class="sxs-lookup"><span data-stu-id="b7c24-118">We'll use this to verify that our sign-in is working before moving on.</span></span> <span data-ttu-id="b7c24-119">Перед тестированием необходимо настроить приложение Express для использования нового маршрутизатора с **./раутес/auth.js**.</span><span class="sxs-lookup"><span data-stu-id="b7c24-119">Before we test, we need to configure the Express app to use the new router from **./routes/auth.js**.</span></span>

    <span data-ttu-id="b7c24-120">`signout`Метод записывает пользователя и уничтожает сеанс.</span><span class="sxs-lookup"><span data-stu-id="b7c24-120">The `signout` method logs the user out and destroys the session.</span></span>

1. <span data-ttu-id="b7c24-121">Откройте **app.js** и вставьте приведенный ниже код **перед** `var app = express();` строкой.</span><span class="sxs-lookup"><span data-stu-id="b7c24-121">Open **./app.js** and insert the following code **before** the `var app = express();` line.</span></span>

    ```javascript
    var authRouter = require('./routes/auth');
    ```

1. <span data-ttu-id="b7c24-122">Вставьте следующий код **после** `app.use('/', indexRouter);` строки.</span><span class="sxs-lookup"><span data-stu-id="b7c24-122">Insert the following code **after** the `app.use('/', indexRouter);` line.</span></span>

    ```javascript
    app.use('/auth', authRouter);
    ```

<span data-ttu-id="b7c24-123">Запустите сервер и перейдите к `https://localhost:3000` .</span><span class="sxs-lookup"><span data-stu-id="b7c24-123">Start the server and browse to `https://localhost:3000`.</span></span> <span data-ttu-id="b7c24-124">Нажмите кнопку входа, и вы будете перенаправлены на `https://login.microsoftonline.com` .</span><span class="sxs-lookup"><span data-stu-id="b7c24-124">Click the sign-in button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="b7c24-125">Войдите с помощью учетной записи Майкрософт и согласия с запрошенными разрешениями.</span><span class="sxs-lookup"><span data-stu-id="b7c24-125">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="b7c24-126">Браузер перенаправляется на приложение, отображая маркер.</span><span class="sxs-lookup"><span data-stu-id="b7c24-126">The browser redirects to the app, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="b7c24-127">Получение сведений о пользователе</span><span class="sxs-lookup"><span data-stu-id="b7c24-127">Get user details</span></span>

1. <span data-ttu-id="b7c24-128">Создайте в корневом каталоге проекта новый файл с именем **graph.js** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="b7c24-128">Create a new file in the root of the project named **graph.js** and add the following code.</span></span>

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

    <span data-ttu-id="b7c24-129">При этом экспортируется `getUserDetails` функция, которая использует пакет SDK Microsoft Graph для вызова `/me` конечной точки и возврата результата.</span><span class="sxs-lookup"><span data-stu-id="b7c24-129">This exports the `getUserDetails` function, which uses the Microsoft Graph SDK to call the `/me` endpoint and return the result.</span></span>

1. <span data-ttu-id="b7c24-130">Откройте **/раутес/auth.js** и добавьте приведенные ниже `require` операторы в начало файла.</span><span class="sxs-lookup"><span data-stu-id="b7c24-130">Open **./routes/auth.js** and add the following `require` statements to the top of the file.</span></span>

    ```javascript
    var graph = require('../graph');
    ```

1. <span data-ttu-id="b7c24-131">Замените существующий маршрут обратного вызова на следующий код.</span><span class="sxs-lookup"><span data-stu-id="b7c24-131">Replace the existing callback route with the following code.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/auth.js" id="CallbackSnippet" highlight="13-23":::

    <span data-ttu-id="b7c24-132">Новый код сохраняет идентификатор учетной записи пользователя в сеансе, получает сведения о пользователе из Microsoft Graph и сохраняет его в хранилище пользователя приложения.</span><span class="sxs-lookup"><span data-stu-id="b7c24-132">The new code saves the user's account ID in the session, gets the user's details from Microsoft Graph, and saves it in the app's user storage.</span></span>

1. <span data-ttu-id="b7c24-133">Перезапустите сервер и пройдите процесс входа.</span><span class="sxs-lookup"><span data-stu-id="b7c24-133">Restart the server and go through the sign-in process.</span></span> <span data-ttu-id="b7c24-134">Необходимо вернуться на домашнюю страницу, но пользовательский интерфейс должен измениться, чтобы показать, что вы вошли в систему.</span><span class="sxs-lookup"><span data-stu-id="b7c24-134">You should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

    ![Снимок экрана домашней страницы после входа](./images/add-aad-auth-01.png)

1. <span data-ttu-id="b7c24-136">Щелкните аватар пользователя в правом верхнем углу, чтобы получить доступ к ссылке **выхода** .</span><span class="sxs-lookup"><span data-stu-id="b7c24-136">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="b7c24-137">При нажатии кнопки **выйти** сбрасывается сеанс и возвращается на домашнюю страницу.</span><span class="sxs-lookup"><span data-stu-id="b7c24-137">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

    ![Снимок экрана с раскрывающимся меню со ссылкой "выйти"](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a><span data-ttu-id="b7c24-139">Хранение и обновление маркеров</span><span class="sxs-lookup"><span data-stu-id="b7c24-139">Storing and refreshing tokens</span></span>

<span data-ttu-id="b7c24-140">На этом шаге приложение имеет маркер доступа, который отправляется в `Authorization` заголовке вызовов API.</span><span class="sxs-lookup"><span data-stu-id="b7c24-140">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="b7c24-141">Это маркер, который позволяет приложению получать доступ к Microsoft Graph от имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="b7c24-141">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="b7c24-142">Однако этот маркер кратковременно используется.</span><span class="sxs-lookup"><span data-stu-id="b7c24-142">However, this token is short-lived.</span></span> <span data-ttu-id="b7c24-143">Срок действия маркера истечет через час после его выдачи.</span><span class="sxs-lookup"><span data-stu-id="b7c24-143">The token expires an hour after it is issued.</span></span> <span data-ttu-id="b7c24-144">В этом случае маркер обновления становится полезен.</span><span class="sxs-lookup"><span data-stu-id="b7c24-144">This is where the refresh token becomes useful.</span></span> <span data-ttu-id="b7c24-145">В спецификации OAuth представлен маркер обновления, который позволяет приложению запросить новый маркер доступа, не требуя от пользователя повторного входа.</span><span class="sxs-lookup"><span data-stu-id="b7c24-145">The OAuth specification introduces a refresh token, which allows the app to request a new access token without requiring the user to sign in again.</span></span>

<span data-ttu-id="b7c24-146">Так как приложение использует пакет msal-Node, не требуется реализовывать логику хранения или обновления маркеров.</span><span class="sxs-lookup"><span data-stu-id="b7c24-146">Because the app is using the msal-node package, you do not need to implement any token storage or refresh logic.</span></span> <span data-ttu-id="b7c24-147">Приложение использует используемый по умолчанию кэш маркеров msal-узла в памяти, который достаточно для примера приложения.</span><span class="sxs-lookup"><span data-stu-id="b7c24-147">The app uses the default msal-node in-memory token cache, which is sufficient for a sample application.</span></span> <span data-ttu-id="b7c24-148">Рабочие приложения должны предоставить собственный [подключаемый модуль кэширования](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-node/docs/configuration.md) для сериализации кэша маркеров на надежном, надежном носителе.</span><span class="sxs-lookup"><span data-stu-id="b7c24-148">Production applications should provide their own [caching plugin](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-node/docs/configuration.md) to serialize the token cache in a secure, reliable storage medium.</span></span>
