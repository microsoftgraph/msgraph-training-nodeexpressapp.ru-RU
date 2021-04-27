<!-- markdownlint-disable MD002 MD041 -->

В этом упражнении вы будете включать microsoft Graph в приложение. Для этого приложения вы будете использовать [библиотеку microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) для звонков в Microsoft Graph.

## <a name="get-calendar-events-from-outlook"></a>Получение событий календаря из Outlook

1. Откройте **./graph.js** и добавьте следующую функцию внутри `module.exports` .

    :::code language="javascript" source="../demo/graph-tutorial/graph.js" id="GetCalendarViewSnippet":::

    Давайте посмотрим, что делает этот код.

    - Вызывается URL-адрес `/me/calendarview`.
    - Метод `header` добавляет загон в запрос, в результате чего время начала и окончания возвращается в часовом `Prefer: outlook.timezone` поясе пользователя.
    - Метод задает параметры и `query` `startDateTime` `endDateTime` параметры представления календаря.
    - Метод ограничивает поля, возвращенные для каждого события, только теми, которые будут `select` фактически использовать представление.
    - Метод `orderby` сортировать результаты к началу.
    - Метод ограничивает результаты до `top` 50 событий.

1. Создайте новый файл в **каталоге маршрутов с** именемcalendar.jsи **добавьте** следующий код.

    ```javascript
    const router = require('express-promise-router')();
    const graph = require('../graph.js');
    const addDays = require('date-fns/addDays');
    const formatISO = require('date-fns/formatISO');
    const startOfWeek = require('date-fns/startOfWeek');
    const zonedTimeToUtc = require('date-fns-tz/zonedTimeToUtc');
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
          const timeZoneId = iana.findIana(user.timeZone)[0];
          console.log(`Time zone: ${timeZoneId.valueOf()}`);

          // Calculate the start and end of the current week
          // Get midnight on the start of the current week in the user's timezone,
          // but in UTC. For example, for Pacific Standard Time, the time value would be
          // 07:00:00Z
          var weekStart = zonedTimeToUtc(startOfWeek(new Date()), timeZoneId.valueOf());
          var weekEnd = addDays(weekStart, 7);
          console.log(`Start: ${formatISO(weekStart)}`);

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
                formatISO(weekStart),
                formatISO(weekEnd),
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

1. Обновление **./app.js** для использования этого нового маршрута. Добавьте следующую **строку перед** `var app = express();` строкой.

    ```javascript
    var calendarRouter = require('./routes/calendar');
    ```

1. Добавьте следующую **строку после** `app.use('/auth', authRouter);` строки.

    ```javascript
    app.use('/calendar', calendarRouter);
    ```

1. Перезапустите сервер. Войдите и щелкните **ссылку Календарь** в панели nav. Если все работает надлежащим образом, в календаре пользователя должен появиться дамп событий в формате JSON.

## <a name="display-the-results"></a>Отображение результатов

Теперь вы можете добавить представление для отображения результатов в более понятной пользователям форме.

1. Добавьте следующий код в **./app.js после** `app.set('view engine', 'hbs');` строки.

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="FormatDateSnippet":::

    Это реализует помощник [handlebars](http://handlebarsjs.com/#helpers) для формата даты ISO 8601, возвращенной корпорацией Майкрософт Graph во что-то более удобное для человека.

1. Создайте новый файл в **каталоге ./views** с именем **calendar.hbs** и добавьте следующий код.

    :::code language="html" source="../demo/graph-tutorial/views/calendar.hbs" id="LayoutSnippet":::

    Это позволяет повторять коллекцию событий и добавить для каждого из них строку таблицы.

1. Теперь обнови маршрут **в ./routes/calendar.js** для использования этого представления. Замените существующий маршрут следующим кодом.

    :::code language="javascript" source="../demo/graph-tutorial/routes/calendar.js" id="GetRouteSnippet" highlight="33-36,49,51-54,61":::

1. Сохраните изменения, перезапустите сервер и вопишитесь в приложение. Щелкните **ссылку Календарь,** и теперь приложение должно отрисовки таблицы событий.

    ![Снимок экрана с таблицей событий](./images/add-msgraph-01.png)
