<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="08b9e-101">В этом упражнении вы будете расширяем приложение из предыдущего упражнения для поддержки проверки подлинности с помощью Azure AD.</span><span class="sxs-lookup"><span data-stu-id="08b9e-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="08b9e-102">Это необходимо для получения необходимого маркера доступа OAuth для вызова Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="08b9e-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="08b9e-103">На этом этапе вы интегрирует библиотеку [Passport – Azure AD AD](https://github.com/AzureAD/passport-azure-ad) в приложение.</span><span class="sxs-lookup"><span data-stu-id="08b9e-103">In this step you will integrate the [passport-azure-ad](https://github.com/AzureAD/passport-azure-ad) library into the application.</span></span>

1. <span data-ttu-id="08b9e-104">Создайте новый файл с именем `.env` File в корневом каталоге приложения и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="08b9e-104">Create a new file named `.env` file in the root of your application, and add the following code.</span></span>

    :::code language="ini" source="../demo/graph-tutorial/.env.example":::

    <span data-ttu-id="08b9e-105">Замените `YOUR APP ID HERE` идентификатором приложения на портале регистрации приложений и замените `YOUR APP SECRET HERE` созданным паролем.</span><span class="sxs-lookup"><span data-stu-id="08b9e-105">Replace `YOUR APP ID HERE` with the application ID from the Application Registration Portal, and replace `YOUR APP SECRET HERE` with the password you generated.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="08b9e-106">Если вы используете систему управления версиями (например, Git), то в дальнейшем будет полезно исключить `.env` файл из системы управления версиями, чтобы избежать непреднамеренного утечки идентификатора и пароля приложения.</span><span class="sxs-lookup"><span data-stu-id="08b9e-106">If you're using source control such as git, now would be a good time to exclude the `.env` file from source control to avoid inadvertently leaking your app ID and password.</span></span>

1. <span data-ttu-id="08b9e-107">Откройте `./app.js` и добавьте указанную ниже строку в начало файла, чтобы загрузить `.env` файл.</span><span class="sxs-lookup"><span data-stu-id="08b9e-107">Open `./app.js` and add the following line to the top of the file to load the `.env` file.</span></span>

    ```javascript
    require('dotenv').config();
    ```

## <a name="implement-sign-in"></a><span data-ttu-id="08b9e-108">Реализация входа</span><span class="sxs-lookup"><span data-stu-id="08b9e-108">Implement sign-in</span></span>

1. <span data-ttu-id="08b9e-109">Расположите строку `var indexRouter = require('./routes/index');` в `./app.js`файле.</span><span class="sxs-lookup"><span data-stu-id="08b9e-109">Locate the line `var indexRouter = require('./routes/index');` in `./app.js`.</span></span> <span data-ttu-id="08b9e-110">Вставьте следующий код **перед** строкой.</span><span class="sxs-lookup"><span data-stu-id="08b9e-110">Insert the following code **before** that line.</span></span>

    ```javascript
    var passport = require('passport');
    var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

    // Configure passport

    // In-memory storage of logged-in users
    // For demo purposes only, production apps should store
    // this in a reliable storage
    var users = {};

    // Passport calls serializeUser and deserializeUser to
    // manage users
    passport.serializeUser(function(user, done) {
      // Use the OID property of the user as a key
      users[user.profile.oid] = user;
      done (null, user.profile.oid);
    });

    passport.deserializeUser(function(id, done) {
      done(null, users[id]);
    });

    // Callback function called once the sign-in is complete
    // and an access token has been obtained
    async function signInComplete(iss, sub, profile, accessToken, refreshToken, params, done) {
      if (!profile.oid) {
        return done(new Error("No OID found in user profile."));
      }

      // Save the profile and tokens in user storage
      users[profile.oid] = { profile, accessToken };
      return done(null, users[profile.oid]);
    }

    // Configure OIDC strategy
    passport.use(new OIDCStrategy(
      {
        identityMetadata: `${process.env.OAUTH_AUTHORITY}${process.env.OAUTH_ID_METADATA}`,
        clientID: process.env.OAUTH_APP_ID,
        responseType: 'code id_token',
        responseMode: 'form_post',
        redirectUrl: process.env.OAUTH_REDIRECT_URI,
        allowHttpForRedirectUrl: true,
        clientSecret: process.env.OAUTH_APP_PASSWORD,
        validateIssuer: false,
        passReqToCallback: false,
        scope: process.env.OAUTH_SCOPES.split(' ')
      },
      signInComplete
    ));
    ```

    <span data-ttu-id="08b9e-111">Этот код инициализирует библиотеку [Passport. js](http://www.passportjs.org/) для использования `passport-azure-ad` библиотеки и настраивает ее с помощью идентификатора приложения и пароля для приложения.</span><span class="sxs-lookup"><span data-stu-id="08b9e-111">This code initializes the [Passport.js](http://www.passportjs.org/) library to use the `passport-azure-ad` library, and configures it with the app ID and password for the app.</span></span>

1. <span data-ttu-id="08b9e-112">Расположите строку `app.use('/', indexRouter);` в `./app.js`файле.</span><span class="sxs-lookup"><span data-stu-id="08b9e-112">Locate the line `app.use('/', indexRouter);` in `./app.js`.</span></span> <span data-ttu-id="08b9e-113">Вставьте следующий код **перед** строкой.</span><span class="sxs-lookup"><span data-stu-id="08b9e-113">Insert the following code **before** that line.</span></span>

    ```javascript
    // Initialize passport
    app.use(passport.initialize());
    app.use(passport.session());
    ```

1. <span data-ttu-id="08b9e-114">Создайте новый файл в `./routes` каталоге `auth.js` и добавьте указанный ниже код.</span><span class="sxs-lookup"><span data-stu-id="08b9e-114">Create a new file in the `./routes` directory named `auth.js` and add the following code.</span></span>

    ```javascript
    var express = require('express');
    var passport = require('passport');
    var router = express.Router();

    /* GET auth callback. */
    router.get('/signin',
      function  (req, res, next) {
        passport.authenticate('azuread-openidconnect',
          {
            response: res,
            prompt: 'login',
            failureRedirect: '/',
            failureFlash: true,
            successRedirect: '/'
          }
        )(req,res,next);
      }
    );

    router.post('/callback',
      function(req, res, next) {
        passport.authenticate('azuread-openidconnect',
          {
            response: res,
            failureRedirect: '/',
            failureFlash: true
          }
        )(req,res,next);
      },
      function(req, res) {
        // TEMPORARY!
        // Flash the access token for testing purposes
        req.flash('error_msg', {message: 'Access token', debug: req.user.accessToken});
        res.redirect('/');
      }
    );

    router.get('/signout',
      function(req, res) {
        req.session.destroy(function(err) {
          req.logout();
          res.redirect('/');
        });
      }
    );

    module.exports = router;
    ```

    <span data-ttu-id="08b9e-115">Этот параметр определяет маршрутизатор с тремя маршрутами `signin`: `callback`,, `signout`и.</span><span class="sxs-lookup"><span data-stu-id="08b9e-115">This defines a router with three routes: `signin`, `callback`, and `signout`.</span></span>

    <span data-ttu-id="08b9e-116">`signin` Маршрут вызывает `passport.authenticate` метод, в результате чего приложение перенаправляется на страницу входа в Azure.</span><span class="sxs-lookup"><span data-stu-id="08b9e-116">The `signin` route calls the `passport.authenticate` method, causing the app to redirect to the Azure login page.</span></span>

    <span data-ttu-id="08b9e-117">`callback` Маршрут выполняет перенаправление Azure после завершения входа.</span><span class="sxs-lookup"><span data-stu-id="08b9e-117">The `callback` route is where Azure redirects after the signin is complete.</span></span> <span data-ttu-id="08b9e-118">Код вызывает `passport.authenticate` метод еще раз, в результате чего `passport-azure-ad` стратегия запрашивает маркер доступа.</span><span class="sxs-lookup"><span data-stu-id="08b9e-118">The code calls the `passport.authenticate` method again, causing the `passport-azure-ad` strategy to request an access token.</span></span> <span data-ttu-id="08b9e-119">После получения маркера вызывается следующий обработчик, который перенаправляет обратно на домашнюю страницу с маркером доступа во временном значении ошибки.</span><span class="sxs-lookup"><span data-stu-id="08b9e-119">Once the token is obtained, the next handler is called, which redirects back to the home page with the access token in the temporary error value.</span></span> <span data-ttu-id="08b9e-120">Мы будем использовать эту проверку, чтобы убедиться, что наш вход работает перед переходом.</span><span class="sxs-lookup"><span data-stu-id="08b9e-120">We'll use this to verify that our sign-in is working before moving on.</span></span> <span data-ttu-id="08b9e-121">Перед тестированием необходимо настроить приложение Express для использования нового маршрутизатора `./routes/auth.js`.</span><span class="sxs-lookup"><span data-stu-id="08b9e-121">Before we test, we need to configure the Express app to use the new router from `./routes/auth.js`.</span></span>

    <span data-ttu-id="08b9e-122">`signout` Метод записывает пользователя и уничтожает сеанс.</span><span class="sxs-lookup"><span data-stu-id="08b9e-122">The `signout` method logs the user out and destroys the session.</span></span>

1. <span data-ttu-id="08b9e-123">Откройте `./app.js` и вставьте следующий код **перед** `var app = express();` строкой.</span><span class="sxs-lookup"><span data-stu-id="08b9e-123">Open `./app.js` and insert the following code **before** the `var app = express();` line.</span></span>

    ```javascript
    var authRouter = require('./routes/auth');
    ```

1. <span data-ttu-id="08b9e-124">Вставьте следующий код **после** `app.use('/', indexRouter);` строки.</span><span class="sxs-lookup"><span data-stu-id="08b9e-124">Insert the following code **after** the `app.use('/', indexRouter);` line.</span></span>

    ```javascript
    app.use('/auth', authRouter);
    ```

<span data-ttu-id="08b9e-125">Запустите сервер и перейдите к `https://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="08b9e-125">Start the server and browse to `https://localhost:3000`.</span></span> <span data-ttu-id="08b9e-126">Нажмите кнопку входа, и вы будете перенаправлены на `https://login.microsoftonline.com`.</span><span class="sxs-lookup"><span data-stu-id="08b9e-126">Click the sign-in button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="08b9e-127">Войдите с помощью учетной записи Майкрософт и согласия с запрошенными разрешениями.</span><span class="sxs-lookup"><span data-stu-id="08b9e-127">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="08b9e-128">Браузер перенаправляется на приложение, отображая маркер.</span><span class="sxs-lookup"><span data-stu-id="08b9e-128">The browser redirects to the app, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="08b9e-129">Получение сведений о пользователе</span><span class="sxs-lookup"><span data-stu-id="08b9e-129">Get user details</span></span>

1. <span data-ttu-id="08b9e-130">Создайте в корневом каталоге проекта новый файл `graph.js` и добавьте указанный ниже код.</span><span class="sxs-lookup"><span data-stu-id="08b9e-130">Create a new file in the root of the project named `graph.js` and add the following code.</span></span>

    ```javascript
    var graph = require('@microsoft/microsoft-graph-client');
    require('isomorphic-fetch');

    module.exports = {
      getUserDetails: async function(accessToken) {
        const client = getAuthenticatedClient(accessToken);

        const user = await client.api('/me').get();
        return user;
      }
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

    <span data-ttu-id="08b9e-131">При этом экспортируется `getUserDetails` функция, которая использует пакет SDK Microsoft Graph для вызова `/me` конечной точки и возврата результата.</span><span class="sxs-lookup"><span data-stu-id="08b9e-131">This exports the `getUserDetails` function, which uses the Microsoft Graph SDK to call the `/me` endpoint and return the result.</span></span>

1. <span data-ttu-id="08b9e-132">Откройте `/app.js` и добавьте приведенные `require` ниже операторы в начало файла.</span><span class="sxs-lookup"><span data-stu-id="08b9e-132">Open `/app.js` and add the following `require` statements to the top of the file.</span></span>

    ```javascript
    var graph = require('./graph');
    ```

1. <span data-ttu-id="08b9e-133">Замените имеющуюся функцию `signInComplete` указанным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="08b9e-133">Replace the existing `signInComplete` function with the following code.</span></span>

    ```javascript
    async function signInComplete(iss, sub, profile, accessToken, refreshToken, params, done) {
      if (!profile.oid) {
        return done(new Error("No OID found in user profile."));
      }

      try{
        const user = await graph.getUserDetails(accessToken);

        if (user) {
          // Add properties to profile
          profile['email'] = user.mail ? user.mail : user.userPrincipalName;
        }
      } catch (err) {
        return done(err);
      }

      // Save the profile and tokens in user storage
      users[profile.oid] = { profile, accessToken };
      return done(null, users[profile.oid]);
    }
    ```

    <span data-ttu-id="08b9e-134">Новый код обновляет `profile` предоставленное паспортом значение, чтобы добавить `email` свойство, используя данные, возвращенные Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="08b9e-134">The new code updates the `profile` provided by Passport to add an `email` property, using the data returned by Microsoft Graph.</span></span>

1. <span data-ttu-id="08b9e-135">Добавьте **после** `app.use(passport.session());` строки следующую строку.</span><span class="sxs-lookup"><span data-stu-id="08b9e-135">Add the following **after** the `app.use(passport.session());` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="AddProfileSnippet":::

    <span data-ttu-id="08b9e-136">Этот код загружает профиль пользователя в `locals` свойство ответа.</span><span class="sxs-lookup"><span data-stu-id="08b9e-136">This code loads the user profile into the `locals` property of the response.</span></span> <span data-ttu-id="08b9e-137">Это сделает его доступным для всех представлений в приложении.</span><span class="sxs-lookup"><span data-stu-id="08b9e-137">This will make it available to all of the views in the app.</span></span>

## <a name="storing-the-tokens"></a><span data-ttu-id="08b9e-138">Сохранение маркеров</span><span class="sxs-lookup"><span data-stu-id="08b9e-138">Storing the tokens</span></span>

<span data-ttu-id="08b9e-139">Теперь, когда вы можете получить маркеры, следует реализовать способ их хранения в приложении.</span><span class="sxs-lookup"><span data-stu-id="08b9e-139">Now that you can get tokens, it's time to implement a way to store them in the app.</span></span> <span data-ttu-id="08b9e-140">В настоящее время приложение сохраняет необработанный маркер доступа в хранилище пользователя в памяти.</span><span class="sxs-lookup"><span data-stu-id="08b9e-140">Currently, the app is storing the raw access token in the in-memory user storage.</span></span> <span data-ttu-id="08b9e-141">Так как это пример приложения, для простоты вы по-прежнему можете хранить их там.</span><span class="sxs-lookup"><span data-stu-id="08b9e-141">Since this is a sample app, for simplicity's sake, you'll continue to store them there.</span></span> <span data-ttu-id="08b9e-142">Реальное приложение использует более надежное решение для безопасного хранения, например базу данных.</span><span class="sxs-lookup"><span data-stu-id="08b9e-142">A real-world app would use a more reliable secure storage solution, like a database.</span></span>

<span data-ttu-id="08b9e-143">Однако хранение только маркера доступа не позволяет проверить срок действия или обновить маркер.</span><span class="sxs-lookup"><span data-stu-id="08b9e-143">However, storing just the access token doesn't allow you to check expiration or refresh the token.</span></span> <span data-ttu-id="08b9e-144">Чтобы включить эту возможность, обновите пример, чтобы создать оболочку для маркеров `AccessToken` объекта из `simple-oauth2` библиотеки.</span><span class="sxs-lookup"><span data-stu-id="08b9e-144">In order to enable that, update the sample to wrap the tokens in an `AccessToken` object from the `simple-oauth2` library.</span></span>

1. <span data-ttu-id="08b9e-145">Откройте `./app.js` и добавьте следующий код **перед** `signInComplete` функцией.</span><span class="sxs-lookup"><span data-stu-id="08b9e-145">Open `./app.js` and add the following code **before** the `signInComplete` function.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="ConfigureOAuth2Snippet":::

1. <span data-ttu-id="08b9e-146">Замените имеющуюся функцию `signInComplete` указанным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="08b9e-146">Replace the existing `signInComplete` function with the following.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SignInCompleteSnippet" highlight="17-18, 21":::

1. <span data-ttu-id="08b9e-147">Замените существующий маршрут `./routes/auth.js` обратного вызова следующим.</span><span class="sxs-lookup"><span data-stu-id="08b9e-147">Replace the existing callback route in  `./routes/auth.js` with the following.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/auth.js" id="CallbackRouteSnippet" highlight="17-18":::

1. <span data-ttu-id="08b9e-148">Перезапустите сервер и пройдите процесс входа.</span><span class="sxs-lookup"><span data-stu-id="08b9e-148">Restart the server and go through the sign-in process.</span></span> <span data-ttu-id="08b9e-149">Необходимо вернуться на домашнюю страницу, но пользовательский интерфейс должен измениться, чтобы показать, что вы вошли в систему.</span><span class="sxs-lookup"><span data-stu-id="08b9e-149">You should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

    ![Снимок экрана домашней страницы после входа](./images/add-aad-auth-01.png)

1. <span data-ttu-id="08b9e-151">Щелкните аватар пользователя в правом верхнем углу, чтобы получить доступ к ссылке **выхода** .</span><span class="sxs-lookup"><span data-stu-id="08b9e-151">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="08b9e-152">При нажатии кнопки **выйти** сбрасывается сеанс и возвращается на домашнюю страницу.</span><span class="sxs-lookup"><span data-stu-id="08b9e-152">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

    ![Снимок экрана с раскрывающимся меню со ссылкой "выйти"](./images/add-aad-auth-02.png)

## <a name="refreshing-tokens"></a><span data-ttu-id="08b9e-154">Обновление маркеров</span><span class="sxs-lookup"><span data-stu-id="08b9e-154">Refreshing tokens</span></span>

<span data-ttu-id="08b9e-155">На этом шаге приложение имеет маркер доступа, который отправляется в `Authorization` заголовке вызовов API.</span><span class="sxs-lookup"><span data-stu-id="08b9e-155">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="08b9e-156">Это маркер, который позволяет приложению получать доступ к Microsoft Graph от имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="08b9e-156">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="08b9e-157">Однако этот маркер кратковременно используется.</span><span class="sxs-lookup"><span data-stu-id="08b9e-157">However, this token is short-lived.</span></span> <span data-ttu-id="08b9e-158">Срок действия маркера истечет через час после его выдачи.</span><span class="sxs-lookup"><span data-stu-id="08b9e-158">The token expires an hour after it is issued.</span></span> <span data-ttu-id="08b9e-159">В этом случае маркер обновления становится полезен.</span><span class="sxs-lookup"><span data-stu-id="08b9e-159">This is where the refresh token becomes useful.</span></span> <span data-ttu-id="08b9e-160">Маркер обновления позволяет приложению запросить новый маркер доступа, не требуя от пользователя повторного входа.</span><span class="sxs-lookup"><span data-stu-id="08b9e-160">The refresh token allows the app to request a new access token without requiring the user to sign in again.</span></span>

1. <span data-ttu-id="08b9e-161">Создайте новый файл в корневой папке проекта, `tokens.js` в котором хранятся функции управления маркерами.</span><span class="sxs-lookup"><span data-stu-id="08b9e-161">Create a new file in the root of the project named `tokens.js` to hold token management functions.</span></span> <span data-ttu-id="08b9e-162">Добавьте в него указанный ниже код.</span><span class="sxs-lookup"><span data-stu-id="08b9e-162">Add the following code.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/tokens.js" id="TokensSnippet":::

<span data-ttu-id="08b9e-163">Этот метод сначала проверяет, истек ли срок действия маркера доступа, или закройте его до истечения срока действия.</span><span class="sxs-lookup"><span data-stu-id="08b9e-163">This method first checks if the access token is expired or close to expiring.</span></span> <span data-ttu-id="08b9e-164">Если это так, то он использует маркер обновления для получения новых токенов, затем обновляет кэш и возвращает новый маркер доступа.</span><span class="sxs-lookup"><span data-stu-id="08b9e-164">If it is, then it uses the refresh token to get new tokens, then updates the cache and returns the new access token.</span></span> <span data-ttu-id="08b9e-165">Этот метод используется, когда необходимо получить маркер доступа из хранилища.</span><span class="sxs-lookup"><span data-stu-id="08b9e-165">You'll use this method whenever you need to get the access token out of storage.</span></span>
