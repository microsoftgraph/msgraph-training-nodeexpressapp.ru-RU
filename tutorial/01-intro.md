<!-- markdownlint-disable MD002 MD041 -->

В этом руководстве рассказывается, как создать веб-приложение Node. js Express, которое использует API Microsoft Graph для получения сведений о календаре для пользователя.

> [!TIP]
> Если вы хотите просто скачать заполненный учебник, можно скачать его двумя способами.
>
> - Скачайте [краткий старт Node. js](https://developer.microsoft.com/graph/quick-start?platform=option-node) , чтобы получить рабочий код в минутах.
> - Скачайте или скопируйте [репозиторий GitHub](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).

## <a name="prerequisites"></a>Необходимые компоненты

Прежде чем приступать к этой демонстрации, на компьютере для разработки должен быть установлен [node. js](https://nodejs.org) . Если у вас нет Node. js, посетите предыдущую ссылку для получения вариантов загрузки.

Кроме того, у вас также должна быть личная учетная запись Майкрософт с почтовым ящиком на Outlook.com или рабочей или учебной учетной записью Майкрософт. Если у вас нет учетной записи Майкрософт, у вас есть несколько вариантов для получения бесплатной учетной записи:

- Вы можете [зарегистрироваться для создания новой личной учетной записи Майкрософт](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).
- Вы можете [зарегистрироваться в программе для разработчиков office 365](https://developer.microsoft.com/office/dev-program) , чтобы получить бесплатную подписку на Office 365.

> [!NOTE]
> Это руководство было написано с узлом версии 12.6.1. Действия, описанные в этом руководстве, могут работать с другими версиями, но не тестировались.

## <a name="watch-the-tutorial"></a>Просмотр руководства

Этот модуль записан и доступен в канале разработки Office на YouTube.

<!-- markdownlint-disable MD033 MD034 -->
<br/>

> [!VIDEO https://www.youtube-nocookie.com/embed/n6q8Cm-pTYY]
<!-- markdownlint-enable MD033 MD034 -->

## <a name="feedback"></a>Отзывы

Сообщите о нем в [репозиторий GitHub](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).
