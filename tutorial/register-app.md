<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="8e80d-101">В этом упражнении вы создадите регистрацию нового веб-приложения Azure AD с помощью портала реестра приложений (ARP).</span><span class="sxs-lookup"><span data-stu-id="8e80d-101">In this exercise, you will create a new Azure AD web application registration using the Application Registry Portal (ARP).</span></span>

1. <span data-ttu-id="8e80d-102">Откройте браузер и перейдите на [портал регистрации приложений](https://apps.dev.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="8e80d-102">Open a browser and navigate to the [Application Registration Portal](https://apps.dev.microsoft.com).</span></span> <span data-ttu-id="8e80d-103">Вход с использованием **личной учетной записи** (с учетной записью Майкрософт) или **рабочей или учебНой учетной записи**.</span><span class="sxs-lookup"><span data-stu-id="8e80d-103">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="8e80d-104">В верхней части страницы выберите **Добавить приложение** .</span><span class="sxs-lookup"><span data-stu-id="8e80d-104">Select **Add an app** at the top of the page.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8e80d-105">Если на странице отображается несколько кнопок **Добавить приложение** , выберите ту, которая соответствует списку **приложений** для конвергенции.</span><span class="sxs-lookup"><span data-stu-id="8e80d-105">If you see more than one **Add an app** button on the page, select the one that corresponds to the **Converged apps** list.</span></span>

1. <span data-ttu-id="8e80d-106">На странице **Регистрация приложения** задайте в руководстве **имя приложения** **Graph. js Graph** и нажмите кнопку **создать**.</span><span class="sxs-lookup"><span data-stu-id="8e80d-106">On the **Register your application** page, set the **Application Name** to **Node.js Graph Tutorial** and select **Create**.</span></span>

    ![Снимок экрана: создание нового приложения на веб-сайте портала регистрации приложений](./images/arp-create-app-01.png)

1. <span data-ttu-id="8e80d-108">На странице **регистрации учебника Node. js Graph** в разделе **свойства** скопируйте **идентификатор приложения** так, как он понадобится позже.</span><span class="sxs-lookup"><span data-stu-id="8e80d-108">On the **Node.js Graph Tutorial Registration** page, under the **Properties** section, copy the **Application Id** as you will need it later.</span></span>

    ![Снимок экрана с ИДЕНТИФИКАТОРом только что созданного приложения](./images/arp-create-app-02.png)

1. <span data-ttu-id="8e80d-110">ПроКрутите список вниз до раздела **секреты приложения** .</span><span class="sxs-lookup"><span data-stu-id="8e80d-110">Scroll down to the **Application Secrets** section.</span></span>

    1. <span data-ttu-id="8e80d-111">Выберите **создать новый пароль**.</span><span class="sxs-lookup"><span data-stu-id="8e80d-111">Select **Generate New Password**.</span></span>
    1. <span data-ttu-id="8e80d-112">В диалоговом окне **новый пароль** в созданном пароле скопируйте содержимое поля так, как оно потребуется позже.</span><span class="sxs-lookup"><span data-stu-id="8e80d-112">In the **New password generated** dialog, copy the contents of the box as you will need it later.</span></span>

        > <span data-ttu-id="8e80d-113">**Важно!** Этот пароль никогда не отображается еще раз, поэтому убедитесь, что вы хотите скопировать его сейчас.</span><span class="sxs-lookup"><span data-stu-id="8e80d-113">**Important:** This password is never shown again, so make sure you copy it now.</span></span>

    ![Снимок экрана с новым паролем приложения](./images/arp-create-app-03.png)

1. <span data-ttu-id="8e80d-115">ПроКрутите окно вниз до раздела **платформы** .</span><span class="sxs-lookup"><span data-stu-id="8e80d-115">Scroll down to the **Platforms** section.</span></span>

    1. <span data-ttu-id="8e80d-116">Нажмите кнопку **Добавить платформу**.</span><span class="sxs-lookup"><span data-stu-id="8e80d-116">Select **Add Platform**.</span></span>
    1. <span data-ttu-id="8e80d-117">В диалоговом окне **Добавление платформы** выберите **веб**.</span><span class="sxs-lookup"><span data-stu-id="8e80d-117">In the **Add Platform** dialog, select **Web**.</span></span>

        ![Снимок экрана: создание платформы для приложения](./images/arp-create-app-04.png)

    1. <span data-ttu-id="8e80d-119">В поле **веб-** платформа введите URL-адрес `http://localhost:3000/auth/callback` для **URL-адресов перенаправления**.</span><span class="sxs-lookup"><span data-stu-id="8e80d-119">In the **Web** platform box, enter the URL `http://localhost:3000/auth/callback` for the **Redirect URLs**.</span></span>

        ![Снимок экрана: недавно добавленная веб-платформа для приложения](./images/arp-create-app-05.png)

1. <span data-ttu-id="8e80d-121">ПроКрутите страницу вниз и выберите команду **сохранить**.</span><span class="sxs-lookup"><span data-stu-id="8e80d-121">Scroll to the bottom of the page and select **Save**.</span></span>