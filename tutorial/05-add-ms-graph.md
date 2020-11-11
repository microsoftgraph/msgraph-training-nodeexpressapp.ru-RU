<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="c6a78-101">В этом упражнении вы добавите в приложение Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="c6a78-101">In this exercise you will incorporate Microsoft Graph into the application.</span></span> <span data-ttu-id="c6a78-102">В этом приложении для совершения вызовов в Microsoft Graph будет использоваться [Клиентская библиотека Microsoft](https://github.com/microsoftgraph/msgraph-sdk-javascript) Graph.</span><span class="sxs-lookup"><span data-stu-id="c6a78-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="c6a78-103">Получение событий календаря из Outlook</span><span class="sxs-lookup"><span data-stu-id="c6a78-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="c6a78-104">Откройте **graph.js** и добавьте указанную ниже функцию `module.exports` .</span><span class="sxs-lookup"><span data-stu-id="c6a78-104">Open **./graph.js** and add the following function inside `module.exports`.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/graph.js" id="GetCalendarViewSnippet":::

    <span data-ttu-id="c6a78-105">Рассмотрите, что делает этот код.</span><span class="sxs-lookup"><span data-stu-id="c6a78-105">Consider what this code is doing.</span></span>

    - <span data-ttu-id="c6a78-106">URL-адрес, который будет вызываться — это `/me/events` .</span><span class="sxs-lookup"><span data-stu-id="c6a78-106">The URL that will be called is `/me/events`.</span></span>
    - <span data-ttu-id="c6a78-107">`select`Метод ограничит поля, возвращаемые для каждого события, только теми, которые будут реально использоваться в представлении.</span><span class="sxs-lookup"><span data-stu-id="c6a78-107">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
    - <span data-ttu-id="c6a78-108">`orderby`Метод сортирует результаты по дате и времени создания, начиная с самого последнего элемента.</span><span class="sxs-lookup"><span data-stu-id="c6a78-108">The `orderby` method sorts the results by the date and time they were created, with the most recent item being first.</span></span>

1. <span data-ttu-id="c6a78-109">Создайте новый файл в каталоге **./раутес** с именем **calendar.js** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="c6a78-109">Create a new file in the **./routes** directory named **calendar.js** , and add the following code.</span></span>

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

1. <span data-ttu-id="c6a78-110">Update **./app.js** , чтобы использовать этот новый маршрут.</span><span class="sxs-lookup"><span data-stu-id="c6a78-110">Update **./app.js** to use this new route.</span></span> <span data-ttu-id="c6a78-111">Добавьте указанную ниже **before** строку перед `var app = express();` строкой.</span><span class="sxs-lookup"><span data-stu-id="c6a78-111">Add the following line **before** the `var app = express();` line.</span></span>

    ```javascript
    var calendarRouter = require('./routes/calendar');
    ```

1. <span data-ttu-id="c6a78-112">Добавьте следующую строку **после** `app.use('/auth', authRouter);` строки.</span><span class="sxs-lookup"><span data-stu-id="c6a78-112">Add the following line **after** the `app.use('/auth', authRouter);` line.</span></span>

    ```javascript
    app.use('/calendar', calendarRouter);
    ```

1. <span data-ttu-id="c6a78-113">Перезапустите сервер.</span><span class="sxs-lookup"><span data-stu-id="c6a78-113">Restart the server.</span></span> <span data-ttu-id="c6a78-114">Войдите и щелкните ссылку **Календарь** на панели навигации.</span><span class="sxs-lookup"><span data-stu-id="c6a78-114">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="c6a78-115">Если все работает, вы должны увидеть дамп событий JSON в календаре пользователя.</span><span class="sxs-lookup"><span data-stu-id="c6a78-115">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="c6a78-116">Отображение результатов</span><span class="sxs-lookup"><span data-stu-id="c6a78-116">Display the results</span></span>

<span data-ttu-id="c6a78-117">Теперь вы можете добавить представление для отображения результатов более удобным для пользователя способом.</span><span class="sxs-lookup"><span data-stu-id="c6a78-117">Now you can add a view to display the results in a more user-friendly manner.</span></span>

1. <span data-ttu-id="c6a78-118">Добавьте следующий код в **./app.js после** `app.set('view engine', 'hbs');` строки.</span><span class="sxs-lookup"><span data-stu-id="c6a78-118">Add the following code in **./app.js after** the `app.set('view engine', 'hbs');` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="FormatDateSnippet":::

    <span data-ttu-id="c6a78-119">В этом случае реализуется [вспомогательный модуль handlebars](http://handlebarsjs.com/#helpers) для форматирования даты ISO 8601, возвращаемой Microsoft Graph, в нечто более удобное для человека.</span><span class="sxs-lookup"><span data-stu-id="c6a78-119">This implements a [Handlebars helper](http://handlebarsjs.com/#helpers) to format the ISO 8601 date returned by Microsoft Graph into something more human-friendly.</span></span>

1. <span data-ttu-id="c6a78-120">Создайте новый файл в каталоге **./views** с именем **Calendar. ХБС** и добавьте приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="c6a78-120">Create a new file in the **./views** directory named **calendar.hbs** and add the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/calendar.hbs" id="LayoutSnippet":::

    <span data-ttu-id="c6a78-121">Это приведет к перебору коллекции событий и добавлению строки таблицы для каждой из них.</span><span class="sxs-lookup"><span data-stu-id="c6a78-121">That will loop through a collection of events and add a table row for each one.</span></span>

1. <span data-ttu-id="c6a78-122">Теперь обновите маршрут в **/раутес/calendar.js** , чтобы использовать это представление.</span><span class="sxs-lookup"><span data-stu-id="c6a78-122">Now update the route in **./routes/calendar.js** to use this view.</span></span> <span data-ttu-id="c6a78-123">Замените существующий маршрут следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="c6a78-123">Replace the existing route with the following code.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/calendar.js" id="GetRouteSnippet" highlight="33-36,49,51-54,61":::

1. <span data-ttu-id="c6a78-124">Сохраните изменения, перезапустите сервер и войдите в приложение.</span><span class="sxs-lookup"><span data-stu-id="c6a78-124">Save your changes, restart the server, and sign in to the app.</span></span> <span data-ttu-id="c6a78-125">Щелкните ссылку **Календарь** , после чего приложение должно отобразить таблицу событий.</span><span class="sxs-lookup"><span data-stu-id="c6a78-125">Click on the **Calendar** link and the app should now render a table of events.</span></span>

    ![Снимок экрана с таблицей событий](./images/add-msgraph-01.png)
