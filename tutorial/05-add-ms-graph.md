<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="121af-101">В этом упражнении вы будете включать microsoft Graph в приложение.</span><span class="sxs-lookup"><span data-stu-id="121af-101">In this exercise you will incorporate Microsoft Graph into the application.</span></span> <span data-ttu-id="121af-102">Для этого приложения вы будете использовать [библиотеку microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) для звонков в Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="121af-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="121af-103">Получение событий календаря из Outlook</span><span class="sxs-lookup"><span data-stu-id="121af-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="121af-104">Откройте **./graph.js** и добавьте следующую функцию внутри `module.exports` .</span><span class="sxs-lookup"><span data-stu-id="121af-104">Open **./graph.js** and add the following function inside `module.exports`.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/graph.js" id="GetCalendarViewSnippet":::

    <span data-ttu-id="121af-105">Давайте посмотрим, что делает этот код.</span><span class="sxs-lookup"><span data-stu-id="121af-105">Consider what this code is doing.</span></span>

    - <span data-ttu-id="121af-106">Вызывается URL-адрес `/me/calendarview`.</span><span class="sxs-lookup"><span data-stu-id="121af-106">The URL that will be called is `/me/calendarview`.</span></span>
    - <span data-ttu-id="121af-107">Метод `header` добавляет загон в запрос, в результате чего время начала и окончания возвращается в часовом `Prefer: outlook.timezone` поясе пользователя.</span><span class="sxs-lookup"><span data-stu-id="121af-107">The `header` method adds the `Prefer: outlook.timezone` header to the request, causing the start and end times to be returned in the user's time zone.</span></span>
    - <span data-ttu-id="121af-108">Метод задает параметры и `query` `startDateTime` `endDateTime` параметры представления календаря.</span><span class="sxs-lookup"><span data-stu-id="121af-108">The `query` method sets the `startDateTime` and `endDateTime` parameters for the calendar view.</span></span>
    - <span data-ttu-id="121af-109">Метод ограничивает поля, возвращенные для каждого события, только теми, которые будут `select` фактически использовать представление.</span><span class="sxs-lookup"><span data-stu-id="121af-109">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
    - <span data-ttu-id="121af-110">Метод `orderby` сортировать результаты к началу.</span><span class="sxs-lookup"><span data-stu-id="121af-110">The `orderby` method sorts the results by the start time.</span></span>
    - <span data-ttu-id="121af-111">Метод ограничивает результаты до `top` 50 событий.</span><span class="sxs-lookup"><span data-stu-id="121af-111">The `top` method limits the results to 50 events.</span></span>

1. <span data-ttu-id="121af-112">Создайте новый файл в **каталоге маршрутов с** именемcalendar.jsи **добавьте** следующий код.</span><span class="sxs-lookup"><span data-stu-id="121af-112">Create a new file in the **./routes** directory named **calendar.js**, and add the following code.</span></span>

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

1. <span data-ttu-id="121af-113">Обновление **./app.js** для использования этого нового маршрута.</span><span class="sxs-lookup"><span data-stu-id="121af-113">Update **./app.js** to use this new route.</span></span> <span data-ttu-id="121af-114">Добавьте следующую **строку перед** `var app = express();` строкой.</span><span class="sxs-lookup"><span data-stu-id="121af-114">Add the following line **before** the `var app = express();` line.</span></span>

    ```javascript
    var calendarRouter = require('./routes/calendar');
    ```

1. <span data-ttu-id="121af-115">Добавьте следующую **строку после** `app.use('/auth', authRouter);` строки.</span><span class="sxs-lookup"><span data-stu-id="121af-115">Add the following line **after** the `app.use('/auth', authRouter);` line.</span></span>

    ```javascript
    app.use('/calendar', calendarRouter);
    ```

1. <span data-ttu-id="121af-116">Перезапустите сервер.</span><span class="sxs-lookup"><span data-stu-id="121af-116">Restart the server.</span></span> <span data-ttu-id="121af-117">Войдите и щелкните **ссылку Календарь** в панели nav.</span><span class="sxs-lookup"><span data-stu-id="121af-117">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="121af-118">Если все работает надлежащим образом, в календаре пользователя должен появиться дамп событий в формате JSON.</span><span class="sxs-lookup"><span data-stu-id="121af-118">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="121af-119">Отображение результатов</span><span class="sxs-lookup"><span data-stu-id="121af-119">Display the results</span></span>

<span data-ttu-id="121af-120">Теперь вы можете добавить представление для отображения результатов в более понятной пользователям форме.</span><span class="sxs-lookup"><span data-stu-id="121af-120">Now you can add a view to display the results in a more user-friendly manner.</span></span>

1. <span data-ttu-id="121af-121">Добавьте следующий код в **./app.js после** `app.set('view engine', 'hbs');` строки.</span><span class="sxs-lookup"><span data-stu-id="121af-121">Add the following code in **./app.js after** the `app.set('view engine', 'hbs');` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="FormatDateSnippet":::

    <span data-ttu-id="121af-122">Это реализует помощник [handlebars](http://handlebarsjs.com/#helpers) для формата даты ISO 8601, возвращенной корпорацией Майкрософт Graph во что-то более удобное для человека.</span><span class="sxs-lookup"><span data-stu-id="121af-122">This implements a [Handlebars helper](http://handlebarsjs.com/#helpers) to format the ISO 8601 date returned by Microsoft Graph into something more human-friendly.</span></span>

1. <span data-ttu-id="121af-123">Создайте новый файл в **каталоге ./views** с именем **calendar.hbs** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="121af-123">Create a new file in the **./views** directory named **calendar.hbs** and add the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/calendar.hbs" id="LayoutSnippet":::

    <span data-ttu-id="121af-124">Это позволяет повторять коллекцию событий и добавить для каждого из них строку таблицы.</span><span class="sxs-lookup"><span data-stu-id="121af-124">That will loop through a collection of events and add a table row for each one.</span></span>

1. <span data-ttu-id="121af-125">Теперь обнови маршрут **в ./routes/calendar.js** для использования этого представления.</span><span class="sxs-lookup"><span data-stu-id="121af-125">Now update the route in **./routes/calendar.js** to use this view.</span></span> <span data-ttu-id="121af-126">Замените существующий маршрут следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="121af-126">Replace the existing route with the following code.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/calendar.js" id="GetRouteSnippet" highlight="33-36,49,51-54,61":::

1. <span data-ttu-id="121af-127">Сохраните изменения, перезапустите сервер и вопишитесь в приложение.</span><span class="sxs-lookup"><span data-stu-id="121af-127">Save your changes, restart the server, and sign in to the app.</span></span> <span data-ttu-id="121af-128">Щелкните **ссылку Календарь,** и теперь приложение должно отрисовки таблицы событий.</span><span class="sxs-lookup"><span data-stu-id="121af-128">Click on the **Calendar** link and the app should now render a table of events.</span></span>

    ![Снимок экрана с таблицей событий](./images/add-msgraph-01.png)
