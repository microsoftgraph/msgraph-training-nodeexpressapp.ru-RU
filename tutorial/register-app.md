<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="e7e6d-101">В этом упражнении вы создадите регистрацию нового веб-приложения Azure AD с помощью центра администрирования Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-101">In this exercise, you will create a new Azure AD web application registration using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="e7e6d-102">Откройте браузер и перейдите к [Центру администрирования Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e7e6d-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="e7e6d-103">Войдите с помощью **личной учетной записи** (т.е. учетной записи Microsoft) или **рабочей (учебной) учетной записи**.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-103">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="e7e6d-104">Выберите **Azure Active Directory** в левой панели навигации, а затем выберите **Регистрация приложений** в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-104">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="e7e6d-105">Снимок экрана с регистрациями приложений</span><span class="sxs-lookup"><span data-stu-id="e7e6d-105">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="e7e6d-106">Выберите **Новая регистрация**.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-106">Select **New registration**.</span></span> <span data-ttu-id="e7e6d-107">На странице**Зарегистрировать приложение** задайте необходимые значения следующим образом.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-107">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="e7e6d-108">Введите **имя** `Node.js Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-108">Set **Name** to `Node.js Graph Tutorial`.</span></span>
    - <span data-ttu-id="e7e6d-109">Введите **поддерживаемые типы учетных записей** для **учетных записей в любом каталоге организаций и личных учетных записей Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-109">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="e7e6d-110">В разделе **URI адрес перенаправления** введите значение в первом раскрывающемся списке `Web` и задайте значение `http://localhost:3000/auth/callback`.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-110">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `http://localhost:3000/auth/callback`.</span></span>

    ![Снимок страницы "регистрация приложения"](./images/aad-register-an-app.png)

1. <span data-ttu-id="e7e6d-112">Выберите **Зарегистрировать**.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-112">Choose **Register**.</span></span> <span data-ttu-id="e7e6d-113">На странице **учебника Node. js Graph** СКОПИРУЙТЕ значение **идентификатора Application (Client)** и сохраните его, он понадобится на следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-113">On the **Node.js Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Снимок экрана с ИДЕНТИФИКАТОРом приложения для новой регистрации приложения](./images/aad-application-id.png)

1. <span data-ttu-id="e7e6d-115">Выберите **Проверка подлинности** в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-115">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="e7e6d-116">Найдите раздел **Неявное представление** и включите **Маркеры идентификации**.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-116">Locate the **Implicit grant** section and enable **ID tokens**.</span></span> <span data-ttu-id="e7e6d-117">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-117">Choose **Save**.</span></span>

    ![Снимок экрана с неявным разделом предоставления](./images/aad-implicit-grant.png)

1. <span data-ttu-id="e7e6d-119">Выберите **Сертификаты и секреты** в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-119">Select **Certificates & secrets** under **Manage**.</span></span> <span data-ttu-id="e7e6d-120">Нажмите кнопку **Новый секрет клиента**.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-120">Select the **New client secret** button.</span></span> <span data-ttu-id="e7e6d-121">Введите значение в **Описание** и выберите один из вариантов для **Срок действия** и нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-121">Enter a value in **Description** and select one of the options for **Expires** and choose **Add**.</span></span>

    ![Снимок экрана: диалоговое окно добавления секрета клиента](./images/aad-new-client-secret.png)

1. <span data-ttu-id="e7e6d-123">Скопируйте значение секрета клиента, а затем покиньте эту страницу.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-123">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="e7e6d-124">Оно вам понадобится на следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-124">You will need it in the next step.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="e7e6d-125">Это секрет клиента, он никогда не отображается еще раз, поэтому убедитесь, что вы скопировали его.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-125">This client secret is never shown again, so make sure you copy it now.</span></span>

    ![Снимок экрана с недавно добавленным секретом клиента](./images/aad-copy-client-secret.png)
