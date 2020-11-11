<!-- markdownlint-disable MD002 MD041 -->

В этом упражнении вы добавите в приложение Microsoft Graph. В этом приложении для совершения вызовов в Microsoft Graph будет использоваться [Клиентская библиотека Microsoft](https://github.com/microsoftgraph/msgraph-sdk-javascript) Graph.

## <a name="get-calendar-events-from-outlook"></a>Получение событий календаря из Outlook

1. Откройте **graph.js** и добавьте указанную ниже функцию `module.exports` .

    :::code language="javascript" source="../demo/graph-tutorial/graph.js" id="GetCalendarViewSnippet":::

    Рассмотрите, что делает этот код.

    - URL-адрес, который будет вызываться — это `/me/events` .
    - `select`Метод ограничит поля, возвращаемые для каждого события, только теми, которые будут реально использоваться в представлении.
    - `orderby`Метод сортирует результаты по дате и времени создания, начиная с самого последнего элемента.

1. Создайте новый файл в каталоге **./раутес** с именем **calendar.js** и добавьте следующий код.

    ```javascript
    const router = require('express-promise-router')();
    const graph = require('../graph.js');
    const moment = require('moment-timezone');
    const iana = require('windows-iana');
    const { body, validationResult } = require('express-validator');
    const validator = require('validator');

    /* GET /calendar */
    router.get('/',
      async function(req, res) {
        if (!req.session.userId) {
          // Redirect unauthenticated requests to home page
          res.redirect('/')
        } else {
          const params = {
            active: { calendar: true }
          };

          // Get the user
          const user = req.app.locals.users[req.session.userId];
          // Convert user's Windows time zone ("Pacific Standard Time")
          // to IANA format ("America/Los_Angeles")
          // Moment needs IANA format
          const timeZoneId = iana.findOneIana(user.timeZone);
          console.log(`Time zone: ${timeZoneId.valueOf()}`);

          // Calculate the start and end of the current week
          // Get midnight on the start of the current week in the user's timezone,
          // but in UTC. For example, for Pacific Standard Time, the time value would be
          // 07:00:00Z
          var startOfWeek = moment.tz(timeZoneId.valueOf()).startOf('week').utc();
          var endOfWeek = moment(startOfWeek).add(7, 'day');
          console.log(`Start: ${startOfWeek.format()}`);

          // Get the access token
          var accessToken;
          try {
            accessToken = await getAccessToken(req.session.userId, req.app.locals.msalClient);
          } catch (err) {
            res.send(JSON.stringify(err, Object.getOwnPropertyNames(err)));
            return;
          }

          if (accessToken && accessToken.length > 0) {
            try {
              // Get the events
              const events = await graph.getCalendarView(
                accessToken,
                startOfWeek.format(),
                endOfWeek.format(),
                user.timeZone);

              res.json(events.value);
            } catch (err) {
              res.send(JSON.stringify(err, Object.getOwnPropertyNames(err)));
            }
          }
          else {
            req.flash('error_msg', 'Could not get an access token');
          }
        }
      }
    );

    async function getAccessToken(userId, msalClient) {
      // Look up the user's account in the cache
      try {
        const accounts = await msalClient
          .getTokenCache()
          .getAllAccounts();

        const userAccount = accounts.find(a => a.homeAccountId === userId);

        // Get the token silently
        const response = await msalClient.acquireTokenSilent({
          scopes: process.env.OAUTH_SCOPES.split(','),
          redirectUri: process.env.OAUTH_REDIRECT_URI,
          account: userAccount
        });

        return response.accessToken;
      } catch (err) {
        console.log(JSON.stringify(err, Object.getOwnPropertyNames(err)));
      }
    }

    module.exports = router;
    ```

1. Update **./app.js** , чтобы использовать этот новый маршрут. Добавьте указанную ниже **before** строку перед `var app = express();` строкой.

    ```javascript
    var calendarRouter = require('./routes/calendar');
    ```

1. Добавьте следующую строку **после** `app.use('/auth', authRouter);` строки.

    ```javascript
    app.use('/calendar', calendarRouter);
    ```

1. Перезапустите сервер. Войдите и щелкните ссылку **Календарь** на панели навигации. Если все работает, вы должны увидеть дамп событий JSON в календаре пользователя.

## <a name="display-the-results"></a>Отображение результатов

Теперь вы можете добавить представление для отображения результатов более удобным для пользователя способом.

1. Добавьте следующий код в **./app.js после** `app.set('view engine', 'hbs');` строки.

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="FormatDateSnippet":::

    В этом случае реализуется [вспомогательный модуль handlebars](http://handlebarsjs.com/#helpers) для форматирования даты ISO 8601, возвращаемой Microsoft Graph, в нечто более удобное для человека.

1. Создайте новый файл в каталоге **./views** с именем **Calendar. ХБС** и добавьте приведенный ниже код.

    :::code language="html" source="../demo/graph-tutorial/views/calendar.hbs" id="LayoutSnippet":::

    Это приведет к перебору коллекции событий и добавлению строки таблицы для каждой из них.

1. Теперь обновите маршрут в **/раутес/calendar.js** , чтобы использовать это представление. Замените существующий маршрут следующим кодом.

    :::code language="javascript" source="../demo/graph-tutorial/routes/calendar.js" id="GetRouteSnippet" highlight="33-36,49,51-54,61":::

1. Сохраните изменения, перезапустите сервер и войдите в приложение. Щелкните ссылку **Календарь** , после чего приложение должно отобразить таблицу событий.

    ![Снимок экрана с таблицей событий](./images/add-msgraph-01.png)
