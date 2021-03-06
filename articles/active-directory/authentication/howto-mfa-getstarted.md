---
title: Приступая к работе с Azure MFA в облаке
description: Начало работы со службой Многофакторной идентификации Microsoft Azure с условным доступом.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 09/01/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: bd2f796ab2feee4bb862d8de2c44efc742163f06
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/12/2018
ms.locfileid: "49167534"
---
# <a name="deploy-cloud-based-azure-multi-factor-authentication"></a>Развертывание облачной службы Многофакторной идентификации Azure

Приступить к работе со службой Многофакторной идентификации Azure (Azure MFA) просто.

Прежде чем приступить к работе, убедитесь, что у вас есть следующие необходимые компоненты.

* Учетная запись глобального администратора в клиенте Azure AD. Если вам нужна помощь в выполнении этого шага, прочитайте нашу статью [Приступая к работе с Azure AD](../get-started-azure-ad.md).
* Пользователям должны быть назначены правильные лицензии. Дополнительные сведения см. в разделе [Как получить Многофакторную идентификацию Azure](concept-mfa-licensing.md).

## <a name="choose-how-to-enable"></a>Выбор способа включения

**Включение политикой условного доступа**. Именно этот метод рассматривается в данной статье. Это наиболее гибкий способ включить двухфакторную проверку подлинности для пользователей. Включение с помощью политики условного доступа работает только для Azure MFA в облаке и является компонентом уровня "Премиум" в Azure AD.

**Включение с помощью Защиты идентификации Azure AD**. Этот метод использует политику рисков Защиты идентификации Azure AD, чтобы требовать двухфакторную проверку подлинности только на основе риска входа для всех облачных приложений. Этот метод требует лицензирования Azure Active Directory P2. Дополнительные сведения об этом методе можно найти в статье [Практическое руководство. Настройка политики риска пользователя](../identity-protection/howto-user-risk-policy.md).

**Включение путем изменения состояния пользователя**. Это традиционный метод применения двухфакторной проверки подлинности. Он подходит и для Azure MFA в облаке, и для сервера Azure MFA. Использование этого метода требует, чтобы пользователи выполняли двухфакторную проверку подлинности при **каждом** входе в учетную запись, и переопределяет политики условного доступа. Дополнительные сведения об этом методе можно найти в разделе [Включение двухфакторной проверки подлинности пользователя](howto-mfa-userstates.md).

> [!Note]
> Дополнительные сведения о лицензиях и ценах можно найти на страницах цен для [Azure AD](https://azure.microsoft.com/pricing/details/active-directory/
) и [Многофакторной идентификации](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

## <a name="choose-authentication-methods"></a>Выбор методов проверки подлинности

Включите по крайней мере один способ аутентификации пользователей в соответствии с требованиями своей организации. По нашим наблюдениям Microsoft Authenticator обеспечивает максимальное удобство для пользователей. Если вам нужно понять, какие методы доступны и как их настроить, ознакомьтесь со статьей [Какие методы проверки подлинности доступны?](concept-authentication-methods.md).

## <a name="get-users-to-enroll"></a>Побуждение пользователей к регистрации

После включения политики условного доступа пользователи будут вынуждены зарегистрироваться при следующем использовании приложения, защищенного с помощью этой политики. Если включить политику, которая требует MFA, для всех пользователей во всех облачных приложениях, это может создать проблемы для пользователей и вашей службы технической поддержки. Рекомендуется попросить пользователей зарегистрировать методы проверки подлинности заранее, с помощью портала регистрации по адресу [https://aka.ms/mfasetup](https://aka.ms/mfasetup). Многие организации обнаружили, печать афиш, использование подставок с брошюрами и рассылка электронных сообщений помогает внедрению.

## <a name="enable-multi-factor-authentication-with-conditional-access"></a>Включение службы Многофакторной идентификации Azure с условным доступом

Войдите на [портал Azure](https://portal.azure.com), используя учетную запись глобального администратора.

### <a name="choose-verification-options"></a>Выбор вариантов проверки

Перед включением службы Многофакторной идентификации Azure организации необходимо определить, какие варианты проверки являются допустимыми. В целях этого упражнения вы разрешите телефонный вызов и отправку текстового сообщения на телефон, так как это универсальные методы, доступные большинству пользователей. Дополнительные сведения о методах проверки подлинности и их использовании можно найти в статье [Какие методы проверки подлинности доступны?](concept-authentication-methods.md)

1. Перейдите в раздел **Azure Active Directory**, **Пользователи**, **Многофакторная идентификация**.

   ![Доступ к порталу Многофакторной идентификации из колонки "Пользователи" Azure AD на портале Azure](media/howto-mfa-getstarted/users-mfa.png)

1. В новой вкладке, которая откроется, перейдите к разделу **Параметры службы**.
1. В разделе **Параметры проверки** установите флажки для методов, которые доступны для пользователей.

   ![Настройка методов проверки на вкладке параметров службы "Многофакторная идентификация"](media/howto-mfa-getstarted/mfa-servicesettings-verificationoptions.png)

4. Щелкните **Save**(Сохранить).
5. Закройте вкладку **Параметры службы**.

### <a name="create-conditional-access-policy"></a>Создание политики условного доступа

1. Войдите на [портал Azure](https://portal.azure.com), используя учетную запись глобального администратора.
1. Выберите **Azure Active Directory**, **Условный доступ**.
1. Выберите **Новая политика**.
1. Введите понятное имя для политики.
1. В разделе **Пользователи и группы** сделайте следующее.
   * На вкладке **Включить** щелкните переключатель **Все пользователи**.
   * Рекомендация. На вкладке **Исключить** установите флажок для параметра **Пользователи и группы** и выберите группу для исключений, то есть пользователей, у которых нет доступа к их методам проверки подлинности.
   * Нажмите кнопку **Done**(Готово).
1. В разделе **Облачные приложения** щелкните переключатель **Все облачные приложения**.
   * Необязательно. На вкладке **Исключить** выберите облачные приложения в своей организации, для которых не требуется MFA.
   * Нажмите кнопку **Done**(Готово).
1. В разделе **Условия** сделайте следующее.
   * Необязательно. Если вы включили службу "Защита идентификации Azure", то вы можете включить оценку риска входа в политику.
   * Необязательно. Если у вас настроены надежные расположения или именованные расположения, их можно включить в политику или исключить из нее.
1. В разделе **Предоставление** убедитесь, что выбран переключатель **Предоставить доступ**.
    * Установите флажок **Требовать многофакторную проверку подлинности**.
    * Нажмите кнопку **Выбрать**.
1. Пропустите раздел **Сеанс**.
1. Для переключателя **Включить политику** установите значение **Вкл.**
1. Нажмите кнопку **Создать**.

![Создание политики условного доступа для включения MFA для пользователей портала Azure в пилотной группе](media/howto-mfa-getstarted/conditionalaccess-newpolicy.png)

### <a name="test-azure-multi-factor-authentication"></a>Тестирование службы Многофакторной идентификации Azure

Чтобы убедиться, что политика условного доступа работает, проверьте вход на ресурс, который не требует MFA, а затем вход на портал Azure, который требует MFA.

1. Откройте новое окно браузера в анонимном режиме или в режиме InPrivate и перейдите по ссылке: [https://account.activedirectory.windowsazure.com](https://account.activedirectory.windowsazure.com).
   * Войдите с помощью тестового пользователя, созданного в рамках раздела предварительных требований этой статьи. Вас не должны просить выполнить MFA.
   * Закройте окно браузера.
2. Откройте новое окно браузера в анонимном режиме или в режиме InPrivate и перейдите по ссылке: [https://portal.azure.com](https://portal.azure.com).
   * Войдите с помощью тестового пользователя, созданного в рамках раздела предварительных требований этой статьи. Обратите внимание, что теперь необходимо зарегистрироваться и использовать службу "Многофакторная идентификация Azure".
   * Закройте окно браузера.

## <a name="next-steps"></a>Дополнительная информация

Поздравляем! Вы настроили службу Многофакторной идентификации Azure в облаке.

Почему пользователю было предложено или не предложено выполнить MFA? Ознакомьтесь с разделом [Отчет по действиям входа Azure AD](howto-mfa-reporting.md#azure-ad-sign-ins-report).

Чтобы настроить дополнительные параметры, например надежные IP-адреса, пользовательские голосовые сообщения и предупреждения о мошенничестве, см. статью [Настройка параметров Многофакторной идентификации Azure](howto-mfa-mfasettings.md).

Сведения об управлении пользовательскими параметрами Многофакторной идентификации Azure можно найти в статье [Управление параметрами пользователей с помощью Многофакторной идентификации Azure в облаке](howto-mfa-userdevicesettings.md).

[Конвергентная регистрация методов самостоятельного сброса пароля и многофакторной идентификации Azure (общедоступная предварительная версия)](concept-registration-mfa-sspr-converged.md)
