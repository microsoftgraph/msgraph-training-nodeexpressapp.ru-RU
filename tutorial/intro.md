<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="a41ef-101">В этом руководстве рассказывается, как создать веб-приложение Node. js Express, которое использует API Microsoft Graph для получения сведений о календаре для пользователя.</span><span class="sxs-lookup"><span data-stu-id="a41ef-101">This tutorial teaches you how to build a Node.js Express web app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="a41ef-102">Если вы хотите просто скачать заполненный учебник, можно скачать его двумя способами.</span><span class="sxs-lookup"><span data-stu-id="a41ef-102">If you prefer to just download the completed tutorial, you can download it in two ways.</span></span>
>
> - <span data-ttu-id="a41ef-103">Скачайте [краткий старт Node. js](https://developer.microsoft.com/graph/quick-start?platform=option-node) , чтобы получить рабочий код в минутах.</span><span class="sxs-lookup"><span data-stu-id="a41ef-103">Download the [Node.js quick start](https://developer.microsoft.com/graph/quick-start?platform=option-node) to get working code in minutes.</span></span>
> - <span data-ttu-id="a41ef-104">Скачайте или скопируйте [репозиторий GitHub](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).</span><span class="sxs-lookup"><span data-stu-id="a41ef-104">Download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a41ef-105">Необходимые компоненты</span><span class="sxs-lookup"><span data-stu-id="a41ef-105">Prerequisites</span></span>

<span data-ttu-id="a41ef-106">Прежде чем приступать к этой демонстрации, на компьютере для разработки должен быть установлен [node. js](https://nodejs.org) .</span><span class="sxs-lookup"><span data-stu-id="a41ef-106">Before you start this demo, you should have [Node.js](https://nodejs.org) installed on your development machine.</span></span> <span data-ttu-id="a41ef-107">Если у вас нет Node. js, посетите предыдущую ссылку для получения вариантов загрузки.</span><span class="sxs-lookup"><span data-stu-id="a41ef-107">If you do not have Node.js, visit the previous link for download options.</span></span>

> [!NOTE]
> <span data-ttu-id="a41ef-108">Это руководство было написано с узлом версии 10.15.3.</span><span class="sxs-lookup"><span data-stu-id="a41ef-108">This tutorial was written with Node version 10.15.3.</span></span> <span data-ttu-id="a41ef-109">Действия, описанные в этом руководстве, могут работать с другими версиями, но не тестировались.</span><span class="sxs-lookup"><span data-stu-id="a41ef-109">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="a41ef-110">Отзывы</span><span class="sxs-lookup"><span data-stu-id="a41ef-110">Feedback</span></span>

<span data-ttu-id="a41ef-111">Сообщите о нем в [репозиторий GitHub](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).</span><span class="sxs-lookup"><span data-stu-id="a41ef-111">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).</span></span>