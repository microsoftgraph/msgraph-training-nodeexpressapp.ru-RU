<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="d671b-101">В этом упражнении вы добавите Microsoft Graph в приложение.</span><span class="sxs-lookup"><span data-stu-id="d671b-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="d671b-102">В этом приложении для совершения вызовов в Microsoft Graph будет использоваться клиентская библиотека [Microsoft](https://github.com/microsoftgraph/msgraph-sdk-javascript) Graph.</span><span class="sxs-lookup"><span data-stu-id="d671b-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="d671b-103">Получение событий календаря из Outlook</span><span class="sxs-lookup"><span data-stu-id="d671b-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="d671b-104">Для начала добавьте в `./graph.js` файл новый метод, чтобы получить события из календаря.</span><span class="sxs-lookup"><span data-stu-id="d671b-104">Start by adding a new method to the `./graph.js` file to get the events from the calendar.</span></span> <span data-ttu-id="d671b-105">Добавьте следующую функцию в файле `module.exports` in. `./graph.js`</span><span class="sxs-lookup"><span data-stu-id="d671b-105">Add the following function inside the `module.exports` in `./graph.js`.</span></span>

```js
getEvents: async function(accessToken) {
  const client = getAuthenticatedClient(accessToken);

  const events = await client
    .api('/me/events')
    .select('subject,organizer,start,end')
    .orderby('createdDateTime DESC')
    .get();

  return events;
}
```

<span data-ttu-id="d671b-106">Рассмотрите, что делает этот код.</span><span class="sxs-lookup"><span data-stu-id="d671b-106">Consider what this code is doing.</span></span>

- <span data-ttu-id="d671b-107">URL-адрес, который будет вызываться — это `/me/events`.</span><span class="sxs-lookup"><span data-stu-id="d671b-107">The URL that will be called is `/me/events`.</span></span>
- <span data-ttu-id="d671b-108">`select` Метод ограничит поля, возвращаемые для каждого события, только теми, которые будут реально использоваться в представлении.</span><span class="sxs-lookup"><span data-stu-id="d671b-108">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
- <span data-ttu-id="d671b-109">`orderby` Метод сортирует результаты по дате и времени создания, начиная с самого последнего элемента.</span><span class="sxs-lookup"><span data-stu-id="d671b-109">The `orderby` method sorts the results by the date and time they were created, with the most recent item being first.</span></span>

<span data-ttu-id="d671b-110">Создайте новый файл в `./routes` каталоге с именем `calendar.js`и добавьте приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="d671b-110">Create a new file in the `./routes` directory named `calendar.js`, and add the following code.</span></span>

```js
var express = require('express');
var router = express.Router();
var tokens = require('../tokens.js');
var graph = require('../graph.js');

/* GET /calendar */
router.get('/',
  async function(req, res) {
    if (!req.isAuthenticated()) {
      // Redirect unauthenticated requests to home page
      res.redirect('/')
    } else {
      let params = {
        active: { calendar: true }
      };

      // Get the access token
      var accessToken;
      try {
        accessToken = await tokens.getAccessToken(req);
      } catch (err) {
        res.json(err);
      }

      if (accessToken && accessToken.length > 0) {
        try {
          // Get the events
          var events = await graph.getEvents(accessToken);

          res.json(events.value);
        } catch (err) {
          res.json(err);
        }
      }
    }
  }
);

module.exports = router;
```

<span data-ttu-id="d671b-111">Обновите `./app.js` , чтобы использовать этот новый маршрут.</span><span class="sxs-lookup"><span data-stu-id="d671b-111">Update `./app.js` to use this new route.</span></span> <span data-ttu-id="d671b-112">Добавьте указанную ниже \*\*\*\* строку перед `var app = express();` строкой.</span><span class="sxs-lookup"><span data-stu-id="d671b-112">Add the following line **before** the `var app = express();` line.</span></span>

```js
var calendarRouter = require('./routes/calendar');
```

<span data-ttu-id="d671b-113">Затем добавьте следующую строку **после** `app.use('/auth', authRouter);` строки.</span><span class="sxs-lookup"><span data-stu-id="d671b-113">Then add the following line **after** the `app.use('/auth', authRouter);` line.</span></span>

```js
app.use('/calendar', calendarRouter);
```

<span data-ttu-id="d671b-114">Теперь вы можете протестировать это.</span><span class="sxs-lookup"><span data-stu-id="d671b-114">Now you can test this.</span></span> <span data-ttu-id="d671b-115">Войдите и щелкните ссылку **Календарь** на панели навигации.</span><span class="sxs-lookup"><span data-stu-id="d671b-115">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="d671b-116">Если все работает, вы должны увидеть дамп событий JSON в календаре пользователя.</span><span class="sxs-lookup"><span data-stu-id="d671b-116">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="d671b-117">Отображение результатов</span><span class="sxs-lookup"><span data-stu-id="d671b-117">Display the results</span></span>

<span data-ttu-id="d671b-118">Теперь вы можете добавить представление для отображения результатов более удобным для пользователя способом.</span><span class="sxs-lookup"><span data-stu-id="d671b-118">Now you can add a view to display the results in a more user-friendly manner.</span></span> <span data-ttu-id="d671b-119">Сначала добавьте следующий код `./app.js` **после** `app.set('view engine', 'hbs');` строки.</span><span class="sxs-lookup"><span data-stu-id="d671b-119">First, add the following code in `./app.js` **after** the `app.set('view engine', 'hbs');` line.</span></span>

```js
var hbs = require('hbs');
var moment = require('moment');
// Helper to format date/time sent by Graph
hbs.registerHelper('eventDateTime', function(dateTime){
  return moment(dateTime).format('M/D/YY h:mm A');
});
```

<span data-ttu-id="d671b-120">В этом случае реализуется [вспомогательный модуль handlebars](http://handlebarsjs.com/#helpers) для форматирования даты ISO 8601, возвращаемой Microsoft Graph, в нечто более удобное для человека.</span><span class="sxs-lookup"><span data-stu-id="d671b-120">This implements a [Handlebars helper](http://handlebarsjs.com/#helpers) to format the ISO 8601 date returned by Microsoft Graph into something more human-friendly.</span></span>

<span data-ttu-id="d671b-121">Создайте новый файл в `./views` каталоге `calendar.hbs` и добавьте указанный ниже код.</span><span class="sxs-lookup"><span data-stu-id="d671b-121">Create a new file in the `./views` directory named `calendar.hbs` and add the following code.</span></span>

```html
<h1>Calendar</h1>
<table class="table">
  <thead>
    <tr>
      <th scope="col">Organizer</th>
      <th scope="col">Subject</th>
      <th scope="col">Start</th>
      <th scope="col">End</th>
    </tr>
  </thead>
  <tbody>
    {{#each events}}
      <tr>
        <td>{{this.organizer.emailAddress.name}}</td>
        <td>{{this.subject}}</td>
        <td>{{eventDateTime this.start.dateTime}}</td>
        <td>{{eventDateTime this.end.dateTime}}</td>
      </tr>
    {{/each}}
  </tbody>
</table>
```

<span data-ttu-id="d671b-122">Это приведет к перебору коллекции событий и добавлению строки таблицы для каждой из них.</span><span class="sxs-lookup"><span data-stu-id="d671b-122">That will loop through a collection of events and add a table row for each one.</span></span> <span data-ttu-id="d671b-123">Теперь обновите маршрут в `./routes/calendar.js` поле, чтобы использовать это представление.</span><span class="sxs-lookup"><span data-stu-id="d671b-123">Now update the route in `./routes/calendar.js` to use this view.</span></span> <span data-ttu-id="d671b-124">Замените существующий маршрут следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="d671b-124">Replace the existing route with the following code.</span></span>

```js
router.get('/',
  async function(req, res) {
    if (!req.isAuthenticated()) {
      // Redirect unauthenticated requests to home page
      res.redirect('/')
    } else {
      let params = {
        active: { calendar: true }
      };

      // Get the access token
      var accessToken;
      try {
        accessToken = await tokens.getAccessToken(req);
      } catch (err) {
        req.flash('error_msg', {
          message: 'Could not get access token. Try signing out and signing in again.',
          debug: JSON.stringify(err)
        });
      }

      if (accessToken && accessToken.length > 0) {
        try {
          // Get the events
          var events = await graph.getEvents(accessToken);
          params.events = events.value;
        } catch (err) {
          req.flash('error_msg', {
            message: 'Could not fetch events',
            debug: JSON.stringify(err)
          });
        }
      }

      res.render('calendar', params);
    }
  }
);
```

<span data-ttu-id="d671b-125">Сохраните изменения, перезапустите сервер и войдите в приложение.</span><span class="sxs-lookup"><span data-stu-id="d671b-125">Save your changes, restart the server, and sign in to the app.</span></span> <span data-ttu-id="d671b-126">Щелкните ссылку **Календарь** , после чего приложение должно отобразить таблицу событий.</span><span class="sxs-lookup"><span data-stu-id="d671b-126">Click on the **Calendar** link and the app should now render a table of events.</span></span>

![Снимок экрана с таблицей событий](./images/add-msgraph-01.png)
