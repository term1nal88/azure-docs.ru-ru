---
title: Учебник. Интеграция Azure Active Directory с Cornerstone OnDemand | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Cornerstone OnDemand.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f57c5fef-49b0-4591-91ef-fc0de6d654ab
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: jeedes
ms.openlocfilehash: 4927421afeddc337856c027b3ed32539f4f8c1fc
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39441703"
---
# <a name="tutorial-azure-active-directory-integration-with-cornerstone-ondemand"></a>Руководство. Интеграция Azure Active Directory с Cornerstone OnDemand

В этом руководстве описано, как интегрировать Cornerstone OnDemand с Azure Active Directory (Azure AD).

Интеграция Cornerstone OnDemand с Azure AD обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать доступ к Cornerstone OnDemand
- Вы можете включить автоматический вход пользователей в Cornerstone OnDemand (единый вход) с учетной записью Azure AD
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Cornerstone OnDemand, вам потребуется:

- подписка Azure AD;
- подписка Cornerstone OnDemand с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление Cornerstone OnDemand из коллекции
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-cornerstone-ondemand-from-the-gallery"></a>Добавление Cornerstone OnDemand из коллекции
Чтобы настроить интеграцию Cornerstone OnDemand с Azure AD, необходимо добавить это приложение из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Cornerstone OnDemand из коллекции, выполните следующие действия:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Active Directory][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![ПРИЛОЖЕНИЯ][2]

1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![ПРИЛОЖЕНИЯ][3]

1. В поле поиска введите **Cornerstone OnDemand**.

    ![Создание тестового пользователя Azure AD](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_search.png)

1. В области результатов выберите **Cornerstone OnDemand** и нажмите кнопку **Добавить**, чтобы добавить приложение.

    ![Создание тестового пользователя Azure AD](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>настройка и проверка единого входа в Azure AD.
В этом разделе описана настройка и проверка единого входа Azure AD в Cornerstone OnDemand для тестового пользователя Britta Simon.

Для работы единого входа Azure AD необходимо знать, какой пользователь в Cornerstone OnDemand соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Cornerstone OnDemand.

Чтобы установить эту связь, укажите **имя пользователя** в Azure AD в качестве значения **имени пользователя** в Cornerstone OnDemand.

Чтобы настроить и проверить единый вход Azure AD в Cornerstone OnDemand, вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа в Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя Cornerstone OnDemand](#creating-a-cornerstone-ondemand-test-user)** требуется для создания в Cornerstone OnDemand пользователя Britta Simon, связанного с представлением этого пользователя в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD;
1. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении Cornerstone OnDemand.

**Чтобы настроить единый вход Azure AD в Cornerstone OnDemand, выполните следующие действия:**

1. На портале Azure на странице интеграции с приложением **Cornerstone OnDemand** щелкните **Единый вход**.

    ![Настройка единого входа][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.

    ![Настройка единого входа](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_samlbase.png)

1. В разделе **Домены и URL-адреса Cornerstone OnDemand** сделайте следующее:

    ![Настройка единого входа](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_url.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<company>.csod.com`

    b. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://<company>.csod.com`

    > [!NOTE] 
    > Эти значения приведены в качестве примера. Замените эти значения фактическим URL-адресом для входа и идентификатором. Чтобы получить эти значения, обратитесь в [службу поддержки клиентов Cornerstone OnDemand](mailTo:moreinfo@csod.com).

1. В разделе **Сертификат подписи SAML** щелкните **Сертификат (Base64)**, а затем сохраните файл сертификата на компьютере.

    ![Настройка единого входа](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Настройка единого входа](./media/cornerstone-ondemand-tutorial/tutorial_general_400.png)

1. В разделе **Настройка Cornerstone OnDemand** щелкните **Настроить Cornerstone OnDemand**, чтобы открыть окно **Настройка единого входа**. Скопируйте **URL-адрес выхода и URL-адрес службы единого входа SAML** из раздела **Краткий справочник**.

    ![Настройка единого входа](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_configure.png) 

1. Чтобы настроить единый вход на стороне **Cornerstone OnDemand**, нужно отправить скачанный **Сертификат**, **URL-адрес выхода** и **URL-адрес службы единого входа SAML** в [службу поддержки Cornerstone OnDemand](mailTo:moreinfo@csod.com). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/cornerstone-ondemand-tutorial/create_aaduser_01.png)

1. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.

    ![Создание тестового пользователя Azure AD](./media/cornerstone-ondemand-tutorial/create_aaduser_02.png) 

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.

    ![Создание тестового пользователя Azure AD](./media/cornerstone-ondemand-tutorial/create_aaduser_03.png)

1. На странице диалогового окна **Пользователь** выполните следующие действия.

    ![Создание тестового пользователя Azure AD](./media/cornerstone-ondemand-tutorial/create_aaduser_04.png) 

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    b. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Нажмите кнопку **Создать**.

### <a name="creating-a-cornerstone-ondemand-test-user"></a>Создание тестового пользователя Cornerstone OnDemand

Цель этого раздела — создать пользователя с именем Britta Simon в Cornerstone OnDemand. Cornerstone OnDemand поддерживает автоматическую подготовку пользователей, которая по умолчанию включена. Дополнительные сведения о настройке автоматической подготовки пользователей можно найти [здесь](cornerstone-ondemand-provisioning-tutorial.md).

**Если необходимо создать пользователя вручную, выполните следующие действия:**

Чтобы настроить подготовку пользователей, отправьте сведения о пользователе Azure AD, которого необходимо подготовить (например, имя, адрес электронной почты), в [службу поддержки Cornerstone OnDemand](mailTo:moreinfo@csod.com).

>[!NOTE]
>Вы можете использовать любые другие средства создания учетной записи пользователя Cornerstone OnDemand или API, предоставляемые Cornerstone OnDemand, для подготовки учетных записей пользователей AAD.

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как включить единый вход Azure для пользователя Britta Simon, предоставив этому пользователю доступ к Cornerstone OnDemand.

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon для приложения Cornerstone OnDemand, сделайте следующее:**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. В списке приложений выберите **Cornerstone OnDemand**.

    ![Настройка единого входа](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_app.png) 

1. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="testing-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент Cornerstone OnDemand на панели доступа, вы автоматически войдете в приложение Cornerstone OnDemand.
Дополнительные сведения о панели доступа см. в статье [Общие сведения о панели доступа](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Руководство по настройке Google Apps для автоматической подготовки пользователей](cornerstone-ondemand-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/cornerstone-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/cornerstone-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/cornerstone-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/cornerstone-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/cornerstone-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/cornerstone-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/cornerstone-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/cornerstone-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/cornerstone-ondemand-tutorial/tutorial_general_203.png
