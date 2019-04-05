<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="c13ee-101">В этом упражнении вы создадите регистрацию нового веб-приложения Azure AD с помощью центра администрирования Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c13ee-101">In this exercise, you will create a new Azure AD web application registration using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="c13ee-102">Откройте браузер и перейдите в [центр администрирования Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c13ee-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="c13ee-103">Вход с использованием **личной учетной записи** (с учетной записью Майкрософт) или **рабочей или учебНой учетной записи**.</span><span class="sxs-lookup"><span data-stu-id="c13ee-103">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="c13ee-104">Выберите **Azure Active Directory** в левой панели навигации, а затем выберите **Регистрация приложений (Предварительная версия)** в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="c13ee-104">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations (Preview)** under **Manage**.</span></span>

    ![<span data-ttu-id="c13ee-105">Снимок экрана с регистрациями приложений</span><span class="sxs-lookup"><span data-stu-id="c13ee-105">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="c13ee-106">Нажмите кнопку **создать регистрацию**.</span><span class="sxs-lookup"><span data-stu-id="c13ee-106">Select **New registration**.</span></span> <span data-ttu-id="c13ee-107">На странице **Регистрация приложения** задайте указанные ниже значения.</span><span class="sxs-lookup"><span data-stu-id="c13ee-107">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="c13ee-108">Задайте \*\*\*\* для `Node.js Graph Tutorial`параметра Name значение.</span><span class="sxs-lookup"><span data-stu-id="c13ee-108">Set **Name** to `Node.js Graph Tutorial`.</span></span>
    - <span data-ttu-id="c13ee-109">Установите **Поддерживаемые типы учетных** записей для **учетных записей в любом организационном каталоге и личных учетНых записях Майкрософт**.</span><span class="sxs-lookup"><span data-stu-id="c13ee-109">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="c13ee-110">В разделе **URI перенаправления**установите первый раскрывающийся список `Web` и присвойте ему значение `http://localhost:3000/auth/callback`.</span><span class="sxs-lookup"><span data-stu-id="c13ee-110">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `http://localhost:3000/auth/callback`.</span></span>

    ![Снимок страницы "регистрация приложения"](./images/aad-register-an-app.png)

1. <span data-ttu-id="c13ee-112">Выберите **регистр**.</span><span class="sxs-lookup"><span data-stu-id="c13ee-112">Choose **Register**.</span></span> <span data-ttu-id="c13ee-113">На странице **учебника Node. js Graph** СКОПИРУЙТЕ значение **идентификатора Application (Client)** и сохраните его, он понадобится на следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="c13ee-113">On the **Node.js Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Снимок экрана с ИДЕНТИФИКАТОРом приложения для новой регистрации приложения](./images/aad-application-id.png)

1. <span data-ttu-id="c13ee-115">Выберите пункт **Проверка** подлинности в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="c13ee-115">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="c13ee-116">НаХождение неЯвного раздела **предоставления разрешений** и включение **маркеров ID**.</span><span class="sxs-lookup"><span data-stu-id="c13ee-116">Locate the **Implicit grant** section and enable **ID tokens**.</span></span> <span data-ttu-id="c13ee-117">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="c13ee-117">Choose **Save**.</span></span>

    ![Снимок экрана с неЯвным разделом предоставления](./images/aad-implicit-grant.png)

1. <span data-ttu-id="c13ee-119">Выберите **Сертификаты _амп_ секреты** в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="c13ee-119">Select **Certificates & secrets** under **Manage**.</span></span> <span data-ttu-id="c13ee-120">Нажмите кнопку **создать секрет клиента** .</span><span class="sxs-lookup"><span data-stu-id="c13ee-120">Select the **New client secret** button.</span></span> <span data-ttu-id="c13ee-121">Введите значение в поле **Описание** и выберите один из вариантов исТечения **срока действия** и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="c13ee-121">Enter a value in **Description** and select one of the options for **Expires** and choose **Add**.</span></span>

    ![Снимок экрана: диалоговое окно добавления секрета клиента](./images/aad-new-client-secret.png)

1. <span data-ttu-id="c13ee-123">Скопируйте значение секрета клиента, прежде чем покинуть эту страницу.</span><span class="sxs-lookup"><span data-stu-id="c13ee-123">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="c13ee-124">Это потребуется на следующем этапе.</span><span class="sxs-lookup"><span data-stu-id="c13ee-124">You will need it in the next step.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="c13ee-125">Этот секрет клиента никогда не отображается еще раз, поэтому убедитесь, что вы хотите скопировать его сейчас.</span><span class="sxs-lookup"><span data-stu-id="c13ee-125">This client secret is never shown again, so make sure you copy it now.</span></span>

    ![Снимок экрана с недавно добавленным секретом клиента](./images/aad-copy-client-secret.png)