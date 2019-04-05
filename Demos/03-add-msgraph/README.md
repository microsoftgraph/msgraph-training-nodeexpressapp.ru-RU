# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="f493b-101">Выполнение завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="f493b-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f493b-102">Необходимые компоненты</span><span class="sxs-lookup"><span data-stu-id="f493b-102">Prerequisites</span></span>

<span data-ttu-id="f493b-103">Чтобы запустить завершенный проект в этой папке, вам потребуются следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="f493b-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="f493b-104">[Узел node. js](https://nodejs.org) , установленный на компьютере для разработки.</span><span class="sxs-lookup"><span data-stu-id="f493b-104">[Node.js](https://nodejs.org) installed on your development machine.</span></span> <span data-ttu-id="f493b-105">Если у вас нет Node. js, посетите предыдущую ссылку для получения вариантов загрузки.</span><span class="sxs-lookup"><span data-stu-id="f493b-105">If you do not have Node.js, visit the previous link for download options.</span></span> <span data-ttu-id="f493b-106">(**Примечание:** это руководство было написано с узлом версии 10.7.0.</span><span class="sxs-lookup"><span data-stu-id="f493b-106">(**Note:** This tutorial was written with Node version 10.7.0.</span></span> <span data-ttu-id="f493b-107">Действия, описанные в этом руководстве, могут работать с другими версиями, но не тестировались.</span><span class="sxs-lookup"><span data-stu-id="f493b-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="f493b-108">Личная учетная запись Майкрософт с почтовым ящиком на Outlook.com или рабочей или учебной учетной записью Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="f493b-108">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="f493b-109">Если у вас нет учетной записи Майкрософт, у вас есть несколько вариантов для получения бесплатной учетной записи:</span><span class="sxs-lookup"><span data-stu-id="f493b-109">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="f493b-110">Вы можете [зарегистрироваться для создания новой личной учетной записи Майкрософт](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="f493b-110">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="f493b-111">Вы можете [зарегистрироваться в программе для разработчиков office 365](https://developer.microsoft.com/office/dev-program) , чтобы получить бесплатную подписку на Office 365.</span><span class="sxs-lookup"><span data-stu-id="f493b-111">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="f493b-112">Регистрация веб-приложения с помощью центра администрирования Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f493b-112">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="f493b-113">Откройте браузер и перейдите в [центр администрирования Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f493b-113">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="f493b-114">Вход с использованием **личной учетной записи** (с учетной записью Майкрософт) или **рабочей или учебНой учетной записи**.</span><span class="sxs-lookup"><span data-stu-id="f493b-114">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="f493b-115">Выберите **Azure Active Directory** в левой панели навигации, а затем выберите **Регистрация приложений (Предварительная версия)** в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="f493b-115">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations (Preview)** under **Manage**.</span></span>

    ![<span data-ttu-id="f493b-116">Снимок экрана с регистрациями приложений</span><span class="sxs-lookup"><span data-stu-id="f493b-116">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="f493b-117">Нажмите кнопку **создать регистрацию**.</span><span class="sxs-lookup"><span data-stu-id="f493b-117">Select **New registration**.</span></span> <span data-ttu-id="f493b-118">На странице **Регистрация приложения** задайте указанные ниже значения.</span><span class="sxs-lookup"><span data-stu-id="f493b-118">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="f493b-119">Задайте \*\*\*\* для `Node.js Graph Tutorial`параметра Name значение.</span><span class="sxs-lookup"><span data-stu-id="f493b-119">Set **Name** to `Node.js Graph Tutorial`.</span></span>
    - <span data-ttu-id="f493b-120">Установите **Поддерживаемые типы учетных** записей для **учетных записей в любом организационном каталоге и личных учетНых записях Майкрософт**.</span><span class="sxs-lookup"><span data-stu-id="f493b-120">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="f493b-121">В разделе **URI перенаправления**установите первый раскрывающийся список `Web` и присвойте ему значение `http://localhost:3000/auth/callback`.</span><span class="sxs-lookup"><span data-stu-id="f493b-121">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `http://localhost:3000/auth/callback`.</span></span>

    ![Снимок страницы "регистрация приложения"](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="f493b-123">Выберите **регистр**.</span><span class="sxs-lookup"><span data-stu-id="f493b-123">Choose **Register**.</span></span> <span data-ttu-id="f493b-124">На странице **учебника Node. js Graph** СКОПИРУЙТЕ значение **идентификатора Application (Client)** и сохраните его, он понадобится на следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="f493b-124">On the **Node.js Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Снимок экрана с ИДЕНТИФИКАТОРом приложения для новой регистрации приложения](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="f493b-126">Выберите пункт **Проверка** подлинности в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="f493b-126">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="f493b-127">НаХождение неЯвного раздела **предоставления разрешений** и включение **маркеров ID**.</span><span class="sxs-lookup"><span data-stu-id="f493b-127">Locate the **Implicit grant** section and enable **ID tokens**.</span></span> <span data-ttu-id="f493b-128">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="f493b-128">Choose **Save**.</span></span>

    ![Снимок экрана с неЯвным разделом предоставления](/tutorial/images/aad-implicit-grant.png)

1. <span data-ttu-id="f493b-130">Выберите **Сертификаты _амп_ секреты** в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="f493b-130">Select **Certificates & secrets** under **Manage**.</span></span> <span data-ttu-id="f493b-131">Нажмите кнопку **создать секрет клиента** .</span><span class="sxs-lookup"><span data-stu-id="f493b-131">Select the **New client secret** button.</span></span> <span data-ttu-id="f493b-132">Введите значение в поле **Описание** и выберите один из вариантов исТечения **срока действия** и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="f493b-132">Enter a value in **Description** and select one of the options for **Expires** and choose **Add**.</span></span>

    ![Снимок экрана: диалоговое окно добавления секрета клиента](/tutorial/images/aad-new-client-secret.png)

1. <span data-ttu-id="f493b-134">Скопируйте значение секрета клиента, прежде чем покинуть эту страницу.</span><span class="sxs-lookup"><span data-stu-id="f493b-134">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="f493b-135">Это потребуется на следующем этапе.</span><span class="sxs-lookup"><span data-stu-id="f493b-135">You will need it in the next step.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="f493b-136">Этот секрет клиента никогда не отображается еще раз, поэтому убедитесь, что вы хотите скопировать его сейчас.</span><span class="sxs-lookup"><span data-stu-id="f493b-136">This client secret is never shown again, so make sure you copy it now.</span></span>

    ![Снимок экрана с недавно добавленным секретом клиента](/tutorial/images/aad-copy-client-secret.png)

## <a name="configure-the-sample"></a><span data-ttu-id="f493b-138">Настройка примера</span><span class="sxs-lookup"><span data-stu-id="f493b-138">Configure the sample</span></span>

1. <span data-ttu-id="f493b-139">Переименуйте `.env.example` файл в `.env`.</span><span class="sxs-lookup"><span data-stu-id="f493b-139">Rename the `.env.example` file to `.env`.</span></span>
1. <span data-ttu-id="f493b-140">Измените `.env` файл и внесите следующие изменения.</span><span class="sxs-lookup"><span data-stu-id="f493b-140">Edit the `.env` file and make the following changes.</span></span>
    1. <span data-ttu-id="f493b-141">Замените `YOUR_APP_ID_HERE` идентификатором **приложения** , полученНым на портале регистрации приложений.</span><span class="sxs-lookup"><span data-stu-id="f493b-141">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
    1. <span data-ttu-id="f493b-142">Замените `YOUR_APP_PASSWORD_HERE` паролем, полученНым на портале регистрации приложений.</span><span class="sxs-lookup"><span data-stu-id="f493b-142">Replace `YOUR_APP_PASSWORD_HERE` with the password you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="f493b-143">В интерфейсе командной строки (CLI) перейдите к этому каталогу и выполните следующую команду, чтобы установить требования.</span><span class="sxs-lookup"><span data-stu-id="f493b-143">In your command-line interface (CLI), navigate to this directory and run the following command to install requirements.</span></span>

    ```Shell
    npm install
    ```

## <a name="run-the-sample"></a><span data-ttu-id="f493b-144">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="f493b-144">Run the sample</span></span>

1. <span data-ttu-id="f493b-145">Выполните следующую команду в командной панели CLI, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="f493b-145">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    npm start
    ```

1. <span data-ttu-id="f493b-146">Откройте браузер и перейдите по адресу `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="f493b-146">Open a browser and browse to `http://localhost:3000`.</span></span>