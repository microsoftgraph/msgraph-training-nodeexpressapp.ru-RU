<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="2ebcc-101">В этом упражнении вы расширим приложение из предыдущего упражнения, чтобы поддерживать проверку подлинности с помощью Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="2ebcc-102">Это необходимо для получения необходимого маркера доступа OAuth для вызова Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="2ebcc-103">На этом этапе вы интегрируете [библиотеку msal-node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) в приложение.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-103">In this step you will integrate the [msal-node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) library into the application.</span></span>

1. <span data-ttu-id="2ebcc-104">Создайте файл **с именем ENV** в корне приложения и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-104">Create a new file named **.env** in the root of your application, and add the following code.</span></span>

    :::code language="ini" source="../demo/graph-tutorial/example.env":::

    <span data-ttu-id="2ebcc-105">Замените ИД приложения на портале регистрации приложений и замените созданный секрет `YOUR_CLIENT_SECRET_HERE` `YOUR_APP_SECRET_HERE` клиента.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-105">Replace `YOUR_CLIENT_SECRET_HERE` with the application ID from the Application Registration Portal, and replace `YOUR_APP_SECRET_HERE` with the client secret you generated.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="2ebcc-106">Если вы используете управление исходным кодом, например git, пришло бы время исключить **ENV-файл** из системы управления исходным кодом, чтобы избежать случайной утечки ИД приложения и пароля.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-106">If you're using source control such as git, now would be a good time to exclude the **.env** file from source control to avoid inadvertently leaking your app ID and password.</span></span>

1. <span data-ttu-id="2ebcc-107">Откройте **файл ./app.js** и добавьте следующую строку в верхнюю часть файла для загрузки **ENV-файла.**</span><span class="sxs-lookup"><span data-stu-id="2ebcc-107">Open **./app.js** and add the following line to the top of the file to load the **.env** file.</span></span>

    ```javascript
    require('dotenv').config();
    ```

## <a name="implement-sign-in"></a><span data-ttu-id="2ebcc-108">Реализация входов</span><span class="sxs-lookup"><span data-stu-id="2ebcc-108">Implement sign-in</span></span>

1. <span data-ttu-id="2ebcc-109">Найдите `var app = express();` строку **в ./app.js**.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-109">Locate the line `var app = express();` in **./app.js**.</span></span> <span data-ttu-id="2ebcc-110">Вставьте следующий код **после** этой строки.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-110">Insert the following code **after** that line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="MsalInitSnippet":::

    <span data-ttu-id="2ebcc-111">Этот код инициализирует библиотеку msal-node с помощью кода и пароля приложения.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-111">This code initializes the msal-node library with the app ID and password for the app.</span></span>

1. <span data-ttu-id="2ebcc-112">Создайте новый файл в **каталоге ./routes** с именемauth.jsи **добавьте** следующий код.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-112">Create a new file in the **./routes** directory named **auth.js** and add the following code.</span></span>

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

    <span data-ttu-id="2ebcc-113">Он определяет маршрутизатор с тремя маршрутами: `signin` , `callback` и `signout` .</span><span class="sxs-lookup"><span data-stu-id="2ebcc-113">This defines a router with three routes: `signin`, `callback`, and `signout`.</span></span>

    <span data-ttu-id="2ebcc-114">Маршрут `signin` вызывает функцию для создания URL-адреса входа, а затем перенаправляет `getAuthCodeUrl` браузер на этот URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-114">The `signin` route calls the `getAuthCodeUrl` function to generate the login URL, then redirects the browser to that URL.</span></span>

    <span data-ttu-id="2ebcc-115">Маршрут `callback` — это маршрут, через который Azure перенаправляет вас после завершения регистрации.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-115">The `callback` route is where Azure redirects after the signin is complete.</span></span> <span data-ttu-id="2ebcc-116">Код вызывает `acquireTokenByCode` функцию для обмена кодом авторизации на маркер доступа.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-116">The code calls the `acquireTokenByCode` function to exchange the authorization code for an access token.</span></span> <span data-ttu-id="2ebcc-117">После получения маркера он перенаправляется обратно на домашную страницу с маркером доступа во временном значении ошибки.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-117">Once the token is obtained, it redirects back to the home page with the access token in the temporary error value.</span></span> <span data-ttu-id="2ebcc-118">Мы будем использовать его, чтобы убедиться, что вход работает, прежде чем двигаться дальше.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-118">We'll use this to verify that our sign-in is working before moving on.</span></span> <span data-ttu-id="2ebcc-119">Перед тестированием необходимо настроить приложение Express на использование нового маршрутизатора **из ./routes/auth.js.**</span><span class="sxs-lookup"><span data-stu-id="2ebcc-119">Before we test, we need to configure the Express app to use the new router from **./routes/auth.js**.</span></span>

    <span data-ttu-id="2ebcc-120">Метод `signout` занося пользователя в журнал и уничтожает сеанс.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-120">The `signout` method logs the user out and destroys the session.</span></span>

1. <span data-ttu-id="2ebcc-121">Откройте **./app.js** и вставьте следующий код **перед** `var app = express();` строкой.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-121">Open **./app.js** and insert the following code **before** the `var app = express();` line.</span></span>

    ```javascript
    var authRouter = require('./routes/auth');
    ```

1. <span data-ttu-id="2ebcc-122">Вставьте следующий код **после** `app.use('/', indexRouter);` строки.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-122">Insert the following code **after** the `app.use('/', indexRouter);` line.</span></span>

    ```javascript
    app.use('/auth', authRouter);
    ```

<span data-ttu-id="2ebcc-123">Запустите сервер и перейдите к `https://localhost:3000` .</span><span class="sxs-lookup"><span data-stu-id="2ebcc-123">Start the server and browse to `https://localhost:3000`.</span></span> <span data-ttu-id="2ebcc-124">Нажмите кнопку "Вход" и перенаправите вас на `https://login.microsoftonline.com` .</span><span class="sxs-lookup"><span data-stu-id="2ebcc-124">Click the sign-in button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="2ebcc-125">Войдите в систему с помощью учетной записи Майкрософт и согласиться на запрашиваемую разрешения.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-125">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="2ebcc-126">Браузер перенаправляет пользователя в приложение с отображением маркера.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-126">The browser redirects to the app, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="2ebcc-127">Получить сведения о пользователе</span><span class="sxs-lookup"><span data-stu-id="2ebcc-127">Get user details</span></span>

1. <span data-ttu-id="2ebcc-128">Создайте файл в корневой папке проекта с именем **graph.js** добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-128">Create a new file in the root of the project named **graph.js** and add the following code.</span></span>

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

    <span data-ttu-id="2ebcc-129">При этом экспортируется функция, которая использует microsoft Graph SDK для вызова конечной точки `getUserDetails` `/me` и возврата результата.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-129">This exports the `getUserDetails` function, which uses the Microsoft Graph SDK to call the `/me` endpoint and return the result.</span></span>

1. <span data-ttu-id="2ebcc-130">Откройте **файл ./routes/auth.js** добавьте следующие утверждения в `require` верхнюю часть файла.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-130">Open **./routes/auth.js** and add the following `require` statements to the top of the file.</span></span>

    ```javascript
    var graph = require('../graph');
    ```

1. <span data-ttu-id="2ebcc-131">Замените существующий маршрут вызова следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-131">Replace the existing callback route with the following code.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/auth.js" id="CallbackSnippet" highlight="13-23":::

    <span data-ttu-id="2ebcc-132">Новый код сохраняет код учетной записи пользователя в сеансе, получает сведения о пользователе из Microsoft Graph и сохраняет его в хранилище пользователя приложения.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-132">The new code saves the user's account ID in the session, gets the user's details from Microsoft Graph, and saves it in the app's user storage.</span></span>

1. <span data-ttu-id="2ebcc-133">Перезапустите сервер и войдите в нее.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-133">Restart the server and go through the sign-in process.</span></span> <span data-ttu-id="2ebcc-134">Вы должны вернуться на home-страницу, но пользовательский интерфейс должен измениться, чтобы указать, что вы вписались.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-134">You should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

    ![Снимок экрана с домашней страницей после входов](./images/add-aad-auth-01.png)

1. <span data-ttu-id="2ebcc-136">Щелкните аватар пользователя в правом верхнем углу, чтобы получить доступ к ссылке **"Выйти".**</span><span class="sxs-lookup"><span data-stu-id="2ebcc-136">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="2ebcc-137">При **нажатии** кнопки "Выйти" сеанс сбрасывается и возвращается на домашней странице.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-137">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

    ![Снимок экрана с выпадающим меню со ссылкой "Выйти"](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a><span data-ttu-id="2ebcc-139">Хранение и обновление маркеров</span><span class="sxs-lookup"><span data-stu-id="2ebcc-139">Storing and refreshing tokens</span></span>

<span data-ttu-id="2ebcc-140">На этом этапе приложение имеет маркер доступа, который отправляется в `Authorization` заголовке вызовов API.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-140">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="2ebcc-141">Это маркер, который позволяет приложению получать доступ к Microsoft Graph от имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-141">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="2ebcc-142">Однако этот маркер является кратковременной.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-142">However, this token is short-lived.</span></span> <span data-ttu-id="2ebcc-143">Срок действия маркера истекает через час после его выпуска.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-143">The token expires an hour after it is issued.</span></span> <span data-ttu-id="2ebcc-144">В этом случае маркер обновления становится полезным.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-144">This is where the refresh token becomes useful.</span></span> <span data-ttu-id="2ebcc-145">Спецификация OAuth представляет маркер обновления, который позволяет приложению запрашивать новый маркер доступа, не требуя от пользователя повторного входить в систему.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-145">The OAuth specification introduces a refresh token, which allows the app to request a new access token without requiring the user to sign in again.</span></span>

<span data-ttu-id="2ebcc-146">Так как приложение использует пакет msal-node, вам не нужно реализовывать логику хранения маркеров или обновления.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-146">Because the app is using the msal-node package, you do not need to implement any token storage or refresh logic.</span></span> <span data-ttu-id="2ebcc-147">Приложение использует кэш маркеров msal-node в памяти по умолчанию, достаточный для примера приложения.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-147">The app uses the default msal-node in-memory token cache, which is sufficient for a sample application.</span></span> <span data-ttu-id="2ebcc-148">Производственные приложения должны [](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-node/docs/configuration.md) предоставлять собственный подключаемый модуль кэширования для сериализации кэша маркеров в безопасном надежном хранилище.</span><span class="sxs-lookup"><span data-stu-id="2ebcc-148">Production applications should provide their own [caching plugin](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-node/docs/configuration.md) to serialize the token cache in a secure, reliable storage medium.</span></span>
