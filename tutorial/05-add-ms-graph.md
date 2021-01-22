<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="3d7e2-101">В этом упражнении вы включаете Microsoft Graph в приложение.</span><span class="sxs-lookup"><span data-stu-id="3d7e2-101">In this exercise you will incorporate Microsoft Graph into the application.</span></span> <span data-ttu-id="3d7e2-102">Для этого приложения вы будете использовать клиентскую библиотеку [Microsoft-graph](https://github.com/microsoftgraph/msgraph-sdk-javascript) для вызова Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="3d7e2-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="3d7e2-103">Получить события календаря из Outlook</span><span class="sxs-lookup"><span data-stu-id="3d7e2-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="3d7e2-104">Откройте **./graph.js** и добавьте в него следующую `module.exports` функцию.</span><span class="sxs-lookup"><span data-stu-id="3d7e2-104">Open **./graph.js** and add the following function inside `module.exports`.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/graph.js" id="GetCalendarViewSnippet":::

    <span data-ttu-id="3d7e2-105">Подумайте, что делает этот код.</span><span class="sxs-lookup"><span data-stu-id="3d7e2-105">Consider what this code is doing.</span></span>

    - <span data-ttu-id="3d7e2-106">Будет вызван URL-адрес `/me/calendarview` .</span><span class="sxs-lookup"><span data-stu-id="3d7e2-106">The URL that will be called is `/me/calendarview`.</span></span>
    - <span data-ttu-id="3d7e2-107">Метод `header` добавляет загон в запрос, в результате чего время начала и окончания возвращается в часовом поясе `Prefer: outlook.timezone` пользователя.</span><span class="sxs-lookup"><span data-stu-id="3d7e2-107">The `header` method adds the `Prefer: outlook.timezone` header to the request, causing the start and end times to be returned in the user's time zone.</span></span>
    - <span data-ttu-id="3d7e2-108">Метод `query` задает `startDateTime` `endDateTime` параметры и параметры представления календаря.</span><span class="sxs-lookup"><span data-stu-id="3d7e2-108">The `query` method sets the `startDateTime` and `endDateTime` parameters for the calendar view.</span></span>
    - <span data-ttu-id="3d7e2-109">Этот `select` метод ограничивает поля, возвращаемые для каждого события, только теми полями, которые будут фактически использовать представление.</span><span class="sxs-lookup"><span data-stu-id="3d7e2-109">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
    - <span data-ttu-id="3d7e2-110">Метод `orderby` сортировать результаты по времени начала.</span><span class="sxs-lookup"><span data-stu-id="3d7e2-110">The `orderby` method sorts the results by the start time.</span></span>
    - <span data-ttu-id="3d7e2-111">Этот `top` метод ограничивает результаты до 50 событий.</span><span class="sxs-lookup"><span data-stu-id="3d7e2-111">The `top` method limits the results to 50 events.</span></span>

1. <span data-ttu-id="3d7e2-112">Создайте новый файл в **каталоге ./routes** **с именемcalendar.js** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="3d7e2-112">Create a new file in the **./routes** directory named **calendar.js**, and add the following code.</span></span>

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

1. <span data-ttu-id="3d7e2-113">**Обновим ./app.js,** чтобы использовать этот новый маршрут.</span><span class="sxs-lookup"><span data-stu-id="3d7e2-113">Update **./app.js** to use this new route.</span></span> <span data-ttu-id="3d7e2-114">Добавьте следующую **строку перед** `var app = express();` строкой.</span><span class="sxs-lookup"><span data-stu-id="3d7e2-114">Add the following line **before** the `var app = express();` line.</span></span>

    ```javascript
    var calendarRouter = require('./routes/calendar');
    ```

1. <span data-ttu-id="3d7e2-115">Добавьте следующую **строку после** `app.use('/auth', authRouter);` строки.</span><span class="sxs-lookup"><span data-stu-id="3d7e2-115">Add the following line **after** the `app.use('/auth', authRouter);` line.</span></span>

    ```javascript
    app.use('/calendar', calendarRouter);
    ```

1. <span data-ttu-id="3d7e2-116">Перезапустите сервер.</span><span class="sxs-lookup"><span data-stu-id="3d7e2-116">Restart the server.</span></span> <span data-ttu-id="3d7e2-117">Войдите и щелкните **ссылку "Календарь"** на панели nav.</span><span class="sxs-lookup"><span data-stu-id="3d7e2-117">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="3d7e2-118">Если все работает, в календаре пользователя должен быть дамп событий JSON.</span><span class="sxs-lookup"><span data-stu-id="3d7e2-118">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="3d7e2-119">Отображение результатов</span><span class="sxs-lookup"><span data-stu-id="3d7e2-119">Display the results</span></span>

<span data-ttu-id="3d7e2-120">Теперь можно добавить представление для более удобного отображения результатов.</span><span class="sxs-lookup"><span data-stu-id="3d7e2-120">Now you can add a view to display the results in a more user-friendly manner.</span></span>

1. <span data-ttu-id="3d7e2-121">Добавьте следующий код в **./app.js после** `app.set('view engine', 'hbs');` строки.</span><span class="sxs-lookup"><span data-stu-id="3d7e2-121">Add the following code in **./app.js after** the `app.set('view engine', 'hbs');` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="FormatDateSnippet":::

    <span data-ttu-id="3d7e2-122">При этом реализуется помощник [handlebars](http://handlebarsjs.com/#helpers) для формата даты ISO 8601, возвращаемой Microsoft Graph, в более удобное для человека.</span><span class="sxs-lookup"><span data-stu-id="3d7e2-122">This implements a [Handlebars helper](http://handlebarsjs.com/#helpers) to format the ISO 8601 date returned by Microsoft Graph into something more human-friendly.</span></span>

1. <span data-ttu-id="3d7e2-123">Создайте новый файл в **каталоге ./views** с именем **calendar.hbs** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="3d7e2-123">Create a new file in the **./views** directory named **calendar.hbs** and add the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/calendar.hbs" id="LayoutSnippet":::

    <span data-ttu-id="3d7e2-124">При этом будет цикл через коллекцию событий и добавлена строка таблицы для каждого из них.</span><span class="sxs-lookup"><span data-stu-id="3d7e2-124">That will loop through a collection of events and add a table row for each one.</span></span>

1. <span data-ttu-id="3d7e2-125">Теперь обновим маршрут в **./routes/calendar.js** для использования этого представления.</span><span class="sxs-lookup"><span data-stu-id="3d7e2-125">Now update the route in **./routes/calendar.js** to use this view.</span></span> <span data-ttu-id="3d7e2-126">Замените существующий маршрут следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="3d7e2-126">Replace the existing route with the following code.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/calendar.js" id="GetRouteSnippet" highlight="33-36,49,51-54,61":::

1. <span data-ttu-id="3d7e2-127">Сохраните изменения, перезапустите сервер и вопишите в приложение.</span><span class="sxs-lookup"><span data-stu-id="3d7e2-127">Save your changes, restart the server, and sign in to the app.</span></span> <span data-ttu-id="3d7e2-128">Щелкните **ссылку "Календарь",** и приложение должно отрисовки таблицы событий.</span><span class="sxs-lookup"><span data-stu-id="3d7e2-128">Click on the **Calendar** link and the app should now render a table of events.</span></span>

    ![Снимок экрана с таблицей событий](./images/add-msgraph-01.png)
