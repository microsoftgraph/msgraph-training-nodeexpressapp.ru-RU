<!-- markdownlint-disable MD002 MD041 -->

В этом упражнении вы включаете Microsoft Graph в приложение. Для этого приложения вы будете использовать клиентскую библиотеку [Microsoft-graph](https://github.com/microsoftgraph/msgraph-sdk-javascript) для вызова Microsoft Graph.

## <a name="get-calendar-events-from-outlook"></a>Получить события календаря из Outlook

1. Откройте **./graph.js** и добавьте в него следующую `module.exports` функцию.

    :::code language="javascript" source="../demo/graph-tutorial/graph.js" id="GetCalendarViewSnippet":::

    Подумайте, что делает этот код.

    - Будет вызван URL-адрес `/me/calendarview` .
    - Метод `header` добавляет загон в запрос, в результате чего время начала и окончания возвращается в часовом поясе `Prefer: outlook.timezone` пользователя.
    - Метод `query` задает `startDateTime` `endDateTime` параметры и параметры представления календаря.
    - Этот `select` метод ограничивает поля, возвращаемые для каждого события, только теми полями, которые будут фактически использовать представление.
    - Метод `orderby` сортировать результаты по времени начала.
    - Этот `top` метод ограничивает результаты до 50 событий.

1. Создайте новый файл в **каталоге ./routes** **с именемcalendar.js** и добавьте следующий код.

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

1. **Обновим ./app.js,** чтобы использовать этот новый маршрут. Добавьте следующую **строку перед** `var app = express();` строкой.

    ```javascript
    var calendarRouter = require('./routes/calendar');
    ```

1. Добавьте следующую **строку после** `app.use('/auth', authRouter);` строки.

    ```javascript
    app.use('/calendar', calendarRouter);
    ```

1. Перезапустите сервер. Войдите и щелкните **ссылку "Календарь"** на панели nav. Если все работает, в календаре пользователя должен быть дамп событий JSON.

## <a name="display-the-results"></a>Отображение результатов

Теперь можно добавить представление для более удобного отображения результатов.

1. Добавьте следующий код в **./app.js после** `app.set('view engine', 'hbs');` строки.

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="FormatDateSnippet":::

    При этом реализуется помощник [handlebars](http://handlebarsjs.com/#helpers) для формата даты ISO 8601, возвращаемой Microsoft Graph, в более удобное для человека.

1. Создайте новый файл в **каталоге ./views** с именем **calendar.hbs** и добавьте следующий код.

    :::code language="html" source="../demo/graph-tutorial/views/calendar.hbs" id="LayoutSnippet":::

    При этом будет цикл через коллекцию событий и добавлена строка таблицы для каждого из них.

1. Теперь обновим маршрут в **./routes/calendar.js** для использования этого представления. Замените существующий маршрут следующим кодом.

    :::code language="javascript" source="../demo/graph-tutorial/routes/calendar.js" id="GetRouteSnippet" highlight="33-36,49,51-54,61":::

1. Сохраните изменения, перезапустите сервер и вопишите в приложение. Щелкните **ссылку "Календарь",** и приложение должно отрисовки таблицы событий.

    ![Снимок экрана с таблицей событий](./images/add-msgraph-01.png)
