<!-- markdownlint-disable MD002 MD041 -->

В этом упражнении вы будете расширяем приложение из предыдущего упражнения для поддержки проверки подлинности с помощью Azure AD. Это необходимо для получения необходимого маркера доступа OAuth для вызова Microsoft Graph. На этом этапе вы интегрирует библиотеку [Passport – Azure AD AD](https://github.com/AzureAD/passport-azure-ad) в приложение.

Создайте новый файл с именем `.env` File в корневом каталоге приложения и добавьте следующий код.

```text
OAUTH_APP_ID=YOUR_APP_ID_HERE
OAUTH_APP_PASSWORD=YOUR_APP_PASSWORD_HERE
OAUTH_REDIRECT_URI=http://localhost:3000/auth/callback
OAUTH_SCOPES='profile offline_access user.read calendars.read'
OAUTH_AUTHORITY=https://login.microsoftonline.com/common/
OAUTH_ID_METADATA=v2.0/.well-known/openid-configuration
OAUTH_AUTHORIZE_ENDPOINT=oauth2/v2.0/authorize
OAUTH_TOKEN_ENDPOINT=oauth2/v2.0/token
```

Замените `YOUR APP ID HERE` идентификатором приложения на портале регистрации приложений и замените `YOUR APP SECRET HERE` созданным паролем.

> [!IMPORTANT]
> Если вы используете систему управления версиями (например, Git), то в дальнейшем будет полезно исключить `.env` файл из системы управления версиями, чтобы избежать непреднамеренного утечки идентификатора и пароля приложения.

Откройте `./app.js` и добавьте указанную ниже строку в начало файла, чтобы загрузить `.env` файл.

```js
require('dotenv').config();
```

## <a name="implement-sign-in"></a>Реализация входа

Расположите строку `var indexRouter = require('./routes/index');` в `./app.js`файле. Вставьте следующий код **перед** строкой.

```js
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
    return done(new Error("No OID found in user profile."), null);
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

Этот код инициализирует библиотеку [Passport. js](http://www.passportjs.org/) для использования `passport-azure-ad` библиотеки и настраивает ее с помощью идентификатора приложения и пароля для приложения.

Теперь передавайте `passport` объект в приложение Express. Расположите строку `app.use('/', indexRouter);` в `./app.js`файле. Вставьте следующий код **перед** строкой.

```js
// Initialize passport
app.use(passport.initialize());
app.use(passport.session());
```

Создайте новый файл в `./routes` каталоге `auth.js` и добавьте указанный ниже код.

```js
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

Этот параметр определяет маршрутизатор с тремя маршрутами `signin`: `callback`,, `signout`и.

`signin` Маршрут вызывает `passport.authenticate` метод, в результате чего приложение перенаправляется на страницу входа в Azure.

`callback` Маршрут выполняет перенаправление Azure после завершения входа. Код вызывает `passport.authenticate` метод еще раз, в результате чего `passport-azure-ad` стратегия запрашивает маркер доступа. После получения маркера вызывается следующий обработчик, который перенаправляет обратно на домашнюю страницу с маркером доступа во временном значении ошибки. Мы будем использовать эту проверку, чтобы убедиться, что наш вход работает перед переходом. Перед тестированием необходимо настроить приложение Express для использования нового маршрутизатора `./routes/auth.js`.

`signout` Метод записывает пользователя и уничтожает сеанс.

Вставьте следующий код **перед** `var app = express();` строкой.

```js
var authRouter = require('./routes/auth');
```

Затем вставьте следующий код **после** `app.use('/', indexRouter);` строки.

```js
app.use('/auth', authRouter);
```

Запустите сервер и перейдите к `https://localhost:3000`. Нажмите кнопку входа, и вы будете перенаправлены на `https://login.microsoftonline.com`. Войдите с помощью учетной записи Майкрософт и согласия с запрошенными разрешениями. Браузер перенаправляется на приложение, отображая маркер.

### <a name="get-user-details"></a>Получение сведений о пользователе

Начните с создания нового файла, в котором будет храниться весь набор вызовов Microsoft Graph. Создайте в корневом каталоге проекта новый файл `graph.js` и добавьте указанный ниже код.

```js
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

При этом экспортируется `getUserDetails` функция, которая использует пакет SDK Microsoft Graph для вызова `/me` конечной точки и возврата результата.

Обновите `signInComplete` метод в `/app.js` методе, чтобы вызвать эту функцию. Сначала добавьте приведенные ниже `require` операторы в начало файла.

```js
var graph = require('./graph');
```

Замените имеющуюся функцию `signInComplete` указанным ниже кодом.

```js
async function signInComplete(iss, sub, profile, accessToken, refreshToken, params, done) {
  if (!profile.oid) {
    return done(new Error("No OID found in user profile."), null);
  }

  try{
    const user = await graph.getUserDetails(accessToken);

    if (user) {
      // Add properties to profile
      profile['email'] = user.mail ? user.mail : user.userPrincipalName;
    }
  } catch (err) {
    done(err, null);
  }

  // Save the profile and tokens in user storage
  users[profile.oid] = { profile, accessToken };
  return done(null, users[profile.oid]);
}
```

Новый код обновляет `profile` предоставленное паспортом значение, чтобы добавить `email` свойство, используя данные, возвращенные Microsoft Graph.

Наконец, добавьте код для `./app.js` загрузки профиля пользователя в `locals` свойство ответа. Это сделает его доступным для всех представлений в приложении.

Добавьте **после** `app.use(passport.session());` строки следующую строку.

```js
app.use(function(req, res, next) {
  // Set the authenticated user in the
  // template locals
  if (req.user) {
    res.locals.user = req.user.profile;
  }
  next();
});
```

## <a name="storing-the-tokens"></a>Сохранение маркеров

Теперь, когда вы можете получить маркеры, следует реализовать способ их хранения в приложении. В настоящее время приложение сохраняет необработанный маркер доступа в хранилище пользователя в памяти. Так как это пример приложения, для простоты вы по-прежнему можете хранить их там. Реальное приложение использует более надежное решение для безопасного хранения, например базу данных.

Однако хранение только маркера доступа не позволяет проверить срок действия или обновить маркер. Чтобы включить эту возможность, обновите пример, чтобы создать оболочку для маркеров `AccessToken` объекта из `simple-oauth2` библиотеки.

`./app.js`Сначала добавьте следующий код **перед** `signInComplete` функцией.

```js
// Configure simple-oauth2
const oauth2 = require('simple-oauth2').create({
  client: {
    id: process.env.OAUTH_APP_ID,
    secret: process.env.OAUTH_APP_PASSWORD
  },
  auth: {
    tokenHost: process.env.OAUTH_AUTHORITY,
    authorizePath: process.env.OAUTH_AUTHORIZE_ENDPOINT,
    tokenPath: process.env.OAUTH_TOKEN_ENDPOINT
  }
});
```

Затем обновите `signInComplete` функцию, чтобы она была `AccessToken` создана из необработанных маркеров, переданных в хранилище пользователя. Замените имеющуюся функцию `signInComplete` указанным ниже кодом.

```js
async function signInComplete(iss, sub, profile, accessToken, refreshToken, params, done) {
  if (!profile.oid) {
    return done(new Error("No OID found in user profile."), null);
  }

  try{
    const user = await graph.getUserDetails(accessToken);

    if (user) {
      // Add properties to profile
      profile['email'] = user.mail ? user.mail : user.userPrincipalName;
    }
  } catch (err) {
    done(err, null);
  }

  // Create a simple-oauth2 token from raw tokens
  let oauthToken = oauth2.accessToken.create(params);

  // Save the profile and tokens in user storage
  users[profile.oid] = { profile, oauthToken };
  return done(null, users[profile.oid]);
}
```

Обновите `callback` маршрут в `./routes/auth.js` , чтобы удалить `req.flash` перенаправление вручную, и укажите для `successRedirect` `passport.authenticate`параметра значение. `callback` Маршрут должен выглядеть так, как показано ниже.

```js
router.post('/callback',
  function(req, res, next) {
    passport.authenticate('azuread-openidconnect',
      {
        response: res,
        failureRedirect: '/',
        failureFlash: true,
        successRedirect: '/'
      }
    )(req,res,next);
  }
);
```

Перезапустите сервер и пройдите процесс входа. Необходимо вернуться на домашнюю страницу, но пользовательский интерфейс должен измениться, чтобы показать, что вы вошли в систему.

![Снимок экрана домашней страницы после входа](./images/add-aad-auth-01.png)

Щелкните аватар пользователя в правом верхнем углу, чтобы получить доступ к ссылке **выхода** . При нажатии кнопки **выйти** сбрасывается сеанс и возвращается на домашнюю страницу.

![Снимок экрана с раскрывающимся меню со ссылкой "выйти"](./images/add-aad-auth-02.png)

## <a name="refreshing-tokens"></a>Обновление маркеров

На этом шаге приложение имеет маркер доступа, который отправляется в `Authorization` заголовке вызовов API. Это маркер, который позволяет приложению получать доступ к Microsoft Graph от имени пользователя.

Однако этот маркер кратковременно используется. Срок действия маркера истечет через час после его выдачи. В этом случае маркер обновления становится полезен. Маркер обновления позволяет приложению запросить новый маркер доступа, не требуя от пользователя повторного входа.

Для этого создайте новый файл в корневой папке проекта `tokens.js` , в котором хранятся функции управления маркерами. Добавьте в него указанный ниже код.

```js
module.exports = {
  getAccessToken: async function(req) {
    if (req.user) {
      // Get the stored token
      var storedToken = req.user.oauthToken;

      if (storedToken) {
        if (storedToken.expired()) {
          // refresh token
          var newToken = await storedToken.refresh();

          // Update stored token
          req.user.oauthToken = newToken;
          return newToken.token.access_token;
        }

        // Token still valid, just return it
        return storedToken.token.access_token;
      }
    }
  }
};
```

Этот метод сначала проверяет, истек ли срок действия маркера доступа, или закройте его до истечения срока действия. Если это так, то он использует маркер обновления для получения новых токенов, затем обновляет кэш и возвращает новый маркер доступа. Этот метод используется, когда необходимо получить маркер доступа из хранилища.
