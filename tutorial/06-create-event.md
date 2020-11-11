<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="5736e-101">В этом разделе мы добавим возможность создания событий в календаре пользователя.</span><span class="sxs-lookup"><span data-stu-id="5736e-101">In this section you will add the ability to create events on the user's calendar.</span></span>

## <a name="create-a-new-event-form"></a><span data-ttu-id="5736e-102">Создание новой формы события</span><span class="sxs-lookup"><span data-stu-id="5736e-102">Create a new event form</span></span>

1. <span data-ttu-id="5736e-103">Создайте новый файл в каталоге **./views** с именем **невевент. ХБС** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="5736e-103">Create a new file in the **./views** directory named **newevent.hbs** and add the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/newevent.hbs" id="NewEventFormSnippet":::

1. <span data-ttu-id="5736e-104">Добавьте следующий код в файл **./раутес/calendar.js** перед `module.exports = router;` строкой.</span><span class="sxs-lookup"><span data-stu-id="5736e-104">Add the following code to the **./routes/calendar.js** file before the `module.exports = router;` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/calendar.js" id="GetEventFormSnippet":::

<span data-ttu-id="5736e-105">Это реализует форму для ввода данных пользователем и отображает ее.</span><span class="sxs-lookup"><span data-stu-id="5736e-105">This implements a form for user input and renders it.</span></span>

## <a name="create-the-event"></a><span data-ttu-id="5736e-106">Создание события</span><span class="sxs-lookup"><span data-stu-id="5736e-106">Create the event</span></span>

1. <span data-ttu-id="5736e-107">Откройте **graph.js** и добавьте указанную ниже функцию `module.exports` .</span><span class="sxs-lookup"><span data-stu-id="5736e-107">Open **./graph.js** and add the following function inside `module.exports`.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/graph.js" id="CreateEventSnippet":::

    <span data-ttu-id="5736e-108">Этот код использует поля формы для создания объекта события Graph, а затем отправляет запрос POST в `/me/events` конечную точку, чтобы создать событие в календаре пользователя по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="5736e-108">This code uses the form fields to create a Graph event object, then sends a POST request to the `/me/events` endpoint to create the event on the user's default calendar.</span></span>

1. <span data-ttu-id="5736e-109">Добавьте следующий код в файл **./раутес/calendar.js** перед `module.exports = router;` строкой.</span><span class="sxs-lookup"><span data-stu-id="5736e-109">Add the following code to the **./routes/calendar.js** file before the `module.exports = router;` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/calendar.js" id="PostEventFormSnippet":::

    <span data-ttu-id="5736e-110">Этот код проверяет и проверит входные данные формы, а затем вызывает ее `graph.createEvent` для создания события.</span><span class="sxs-lookup"><span data-stu-id="5736e-110">This code validates and sanitized the form input, then calls `graph.createEvent` to create the event.</span></span> <span data-ttu-id="5736e-111">После завершения вызова он перенаправляется обратно в представление календаря.</span><span class="sxs-lookup"><span data-stu-id="5736e-111">It redirects back to the calendar view after the call completes.</span></span>

1. <span data-ttu-id="5736e-112">Сохраните изменения и перезапустите приложение.</span><span class="sxs-lookup"><span data-stu-id="5736e-112">Save your changes and restart the app.</span></span> <span data-ttu-id="5736e-113">Щелкните элемент навигации по **календарю** , а затем нажмите кнопку **создать событие** .</span><span class="sxs-lookup"><span data-stu-id="5736e-113">Click the **Calendar** nav item, then click the **Create event** button.</span></span> <span data-ttu-id="5736e-114">Введите значения и нажмите кнопку **создать**.</span><span class="sxs-lookup"><span data-stu-id="5736e-114">Fill in the values and click **Create**.</span></span> <span data-ttu-id="5736e-115">Приложение возвращается в представление календаря после создания нового события.</span><span class="sxs-lookup"><span data-stu-id="5736e-115">The app returns to the calendar view once the new event is created.</span></span>

    ![Снимок экрана с формой создания события](images/create-event-01.png)
