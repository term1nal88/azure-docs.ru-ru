---
title: Руководство. Интеграция Azure Active Directory с Dovetale | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Dovetale.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 81da50c3-df94-458a-8b6a-a30827ee6358
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2018
ms.author: jeedes
ms.openlocfilehash: dd2aa699c22518ebbe7f29ba8dfd296da7385476
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "40161840"
---
# <a name="tutorial-azure-active-directory-integration-with-dovetale"></a>Руководство. Интеграция Azure Active Directory с Dovetale

В этом руководстве описано, как интегрировать Dovetale с Azure Active Directory (Azure AD).

Интеграция Dovetale с Azure AD обеспечивает следующие преимущества.

- С помощью Azure AD можно контролировать доступ к Dovetale.
- Вы можете включить автоматический вход пользователей в Dovetale (единый вход) с учетной записью Azure AD.
- Вы можете управлять учетными записями централизованно — на портале Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Dovetale, потребуется:

- подписка Azure AD;
- подписка с поддержкой единого входа Dovetale.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете [получить пробную версию на один месяц](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление Dovetale из коллекции
2. настройка и проверка единого входа в Azure AD.

## <a name="adding-dovetale-from-the-gallery"></a>Добавление Dovetale из коллекции

Чтобы настроить интеграцию Dovetale с Azure AD, необходимо добавить Dovetale из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Dovetale из коллекции, сделайте следующее:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Кнопка "Azure Active Directory"][1]

2. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"][2]

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Новое приложение"][3]

4. В поле поиска введите **Dovetale**, выберите **Dovetale** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Dovetale в списке результатов](./media/dovetale-tutorial/tutorial_dovetale_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в Dovetale с использованием тестового пользователя Britta Simon.

Для работы единого входа в Azure AD необходимо знать, какой пользователь в Dovetale соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Dovetale.

Чтобы настроить и проверить единый вход Azure AD в Dovetale, требуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
3. **[Создание тестового пользователя приложения Dovetale](#create-a-dovetale-test-user)** требуется для того, чтобы в Dovetale существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD.
5. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении Dovetale.

**Чтобы настроить единый вход Azure AD в Dovetale, выполните следующие действия.**

1. На портале Azure на странице интеграции с приложением **Dovetale** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"][4]

2. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.

    ![Диалоговое окно "Единый вход"](./media/dovetale-tutorial/tutorial_dovetale_samlbase.png)

3. Если нужно настроить приложение в режиме, инициируемом **IDP**, то в разделе **Домены и URL-адреса приложения Dovetale**, пользователю не нужно ничего делать, так как это приложение уже предварительно интегрировано в Azure.

    ![Сведения о домене и URL-адресах единого входа приложения Dovetale](./media/dovetale-tutorial/tutorial_dovetale_url1.png)

4. Установите флажок **Показать дополнительные параметры URL-адресов**, и выполните следующее действие, если хотите настроить приложение для работы в режиме, инициируемом **поставщиком услуг**:

    ![Сведения о домене и URL-адресах единого входа приложения Dovetale](./media/dovetale-tutorial/tutorial_dovetale_url2.png)

    В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `<COMPANYNAME>.dovetale.com`

    > [!NOTE]
    > Это значение приведено для справки. Вместо него необходимо указать фактический URL-адрес входа. Для получения данного значения обратитесь в [службу поддержки клиентов Dovetale](mailto:support@dovetale.com).

5. Приложение Dovetale ожидает утверждения SAML в определенном формате. Настройте следующие утверждения для этого приложения. Управлять значениями этих атрибутов можно в разделе **Атрибуты пользователя** на странице интеграции приложения. На следующем снимке экрана приведен пример.
    
    ![Настройка единого входа](./media/dovetale-tutorial/tutorial_attribute.png)

6. В разделе **Атрибуты пользователя** установите флажок **Просмотреть и изменить все другие атрибуты пользователей**, чтобы развернуть атрибуты. Выполните следующие действия для каждого отображаемого атрибута.

    | Имя атрибута | Значение атрибута |
    | ---------------| --------------- |
    | email | user.mail |
    | first_name | user.givenname |
    | name | user.userprincipalname |
    | last_name | user.surname |

    a. Щелкните **Добавить атрибут**, чтобы открыть диалоговое окно **Добавление атрибута**.

    ![Настройка единого входа](./media/dovetale-tutorial/tutorial_attribute_04.png)

    ![Настройка единого входа](./media/dovetale-tutorial/tutorial_attribute_05.png)

    b. В текстовом поле **Имя** введите **имя атрибута**, отображаемое для этой записи.

    c. В списке **Значение** выберите значение атрибута, отображаемое для этой строки.

    d. Оставьте значение **Пространства имен** пустым.

    д. Нажмите кнопку **ОК**.

7. В разделе **Сертификат подписи SAML** нажмите кнопку "Копировать", чтобы скопировать **URL-адрес метаданных федерации приложений**. Затем вставьте его в Блокнот.

    ![Ссылка для скачивания сертификата](./media/dovetale-tutorial/tutorial_dovetale_certificate.png) 

8. Нажмите кнопку **Сохранить** .

    ![Кнопка "Сохранить" в окне настройки единого входа](./media/dovetale-tutorial/tutorial_general_400.png)

9. В разделе **Конфигурация Dovetale** щелкните **Настроить Dovetale**, чтобы открыть окно **Настройка единого входа**. Скопируйте **идентификатор сущности SAML и URL-адрес службы единого входа SAML** из раздела **Краткий справочник**.

    ![Конфигурация Dovetale](./media/dovetale-tutorial/tutorial_dovetale_configure.png)

10. Чтобы настроить единый вход на стороне **Dovetale**, нужно отправить скачанный **URL-адрес метаданных федерации приложений, идентификатор сущности SAML и URL-адрес службы единого входа SAML** в [службу поддержки Dovetale](mailto:support@dovetale.com). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

   ![Создание тестового пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На портале Azure в области слева нажмите кнопку **Azure Active Directory**.

    ![Кнопка "Azure Active Directory"](./media/dovetale-tutorial/create_aaduser_01.png)

2. Чтобы открыть список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](./media/dovetale-tutorial/create_aaduser_02.png)

3. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна **Все пользователи** щелкните **Добавить**.

    ![Кнопка "Добавить"](./media/dovetale-tutorial/create_aaduser_03.png)

4. В диалоговом окне **Пользователь** сделайте следующее.

    ![Диалоговое окно "Пользователь"](./media/dovetale-tutorial/create_aaduser_04.png)

    a. В поле **Имя** введите **BrittaSimon**.

    Б. В поле **Имя пользователя** введите адрес электронной почты для пользователя Britta Simon.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.

    d. Нажмите кнопку **Создать**.

### <a name="create-a-dovetale-test-user"></a>Создание тестового пользователя Dovetale

Цель этого раздела — создать пользователя с именем Britta Simon в Dovetale. Приложение Dovetale поддерживает JIT-подготовку. Эта функция включена по умолчанию. В этом разделе никакие действия с вашей стороны не требуются. Пользователь будет создан при попытке получить доступ к приложению Dovetale (если он еще не создан).

> [!Note]
> Чтобы создать пользователя вручную, обратитесь к [группе поддержки Dovetale](mailto:support@dovetale.com).

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив доступ к Dovetale.

![Назначение роли пользователя][200]

**Чтобы назначить пользователя Britta Simon в Dovetale, сделайте следующее.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201]

2. В списке приложений выберите **Dovetale**.

    ![Ссылка на Dovetale в списке "Приложения"](./media/dovetale-tutorial/tutorial_dovetale_app.png)  

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"][202]

4. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"][203]

5. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

6. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент Dovetale на панели доступа, вы автоматически войдете в приложение Dovetale.
Дополнительные сведения о панели доступа см. в статье [Общие сведения о панели доступа](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/dovetale-tutorial/tutorial_general_01.png
[2]: ./media/dovetale-tutorial/tutorial_general_02.png
[3]: ./media/dovetale-tutorial/tutorial_general_03.png
[4]: ./media/dovetale-tutorial/tutorial_general_04.png

[100]: ./media/dovetale-tutorial/tutorial_general_100.png

[200]: ./media/dovetale-tutorial/tutorial_general_200.png
[201]: ./media/dovetale-tutorial/tutorial_general_201.png
[202]: ./media/dovetale-tutorial/tutorial_general_202.png
[203]: ./media/dovetale-tutorial/tutorial_general_203.png