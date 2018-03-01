---
title: "Krok 1. Rejestrowanie aplikacji przy użyciu usługi Azure Active Directory"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0B17991A-4573-4F6C-9E86-D4B9D1A47E4D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 52d06dc6125f91f98e8f3ee8b4f91ad7b52347a3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="step-1-register-an-app-to-use-azure-active-directory"></a>Krok 1. Rejestrowanie aplikacji przy użyciu usługi Azure Active Directory

1. Przejdź do [windowsazure.com](https://manage.windowsazure.com) i zaloguj się przy użyciu Account Microsoft lub konta organizacji w portalu Azure. Jeśli nie masz subskrypcji platformy Azure, możesz pobrać z wersji próbnej [witrynie azure.com](http://www.azure.com)

2. Po zalogowaniu, przejdź do **usługi Active Directory** sekcji (1) i wybierz katalog, w którym chcesz zarejestrować aplikację (2)

  [ ![](register-images/01.-active-directory-in-azure-portal-sml.jpg "sekcja i wybierz katalog, w którym chcesz zarejestrować aplikację")](register-images/01.-active-directory-in-azure-portal.jpg)

3. Kliknij przycisk **Dodaj** Aby utworzyć nową aplikację, zaznacz opcję **Dodaj aplikację moją organizację**

  [ ![](register-images/02.-add-new-application-sml.jpg "Dodaj aplikację opracowywaną przez moją organizację")](register-images/02.-add-new-application.jpg)

4. Na następnym ekranie nazwę aplikacji (np.) XAM-DEMO).
  Upewnij się, że wybrano **natywną aplikację kliencką** jako typu aplikacji.

  ![](register-images/03.-app-name.jpg "Upewnij się, że wybierz natywną aplikację kliencką jako typ aplikacji")

5. Na ekranie końcowym, podaj **identyfikator URI przekierowania* jest unikatowym dla twojej aplikacji spowoduje zwrócenie do tego identyfikatora URI, po zakończeniu uwierzytelniania.

  ![](register-images/04.-app-redirect.jpg "Na ekranie końcowym Podaj identyfikator URI przekierowania będący unikatowym dla twojej aplikacji spowoduje zwrócenie do tego identyfikatora URI, po zakończeniu uwierzytelniania")

6. Po utworzeniu aplikacji przejdź do **Konfiguruj** kartę. Zapisz **identyfikator klienta** , które będą używane w naszej aplikacji później. Ponadto na tym ekranie można udzielić dostępu aplikacji mobilnej do usługi Active Directory lub dodać inna aplikacja interfejsu API sieci Web lub usługi Office 365, które mogą być używane przez aplikacji mobilnej, po zakończeniu uwierzytelniania.

    ![](register-images/05.-configure.jpg "Ponadto na tym ekranie można udzielić dostępu aplikacji mobilnej do usługi Active Directory lub Dodaj inną aplikację interfejsu API sieci Web lub usługi Office 365")



## <a name="related-links"></a>Linki pokrewne

- [Przykładowe NativeClient firmy Microsoft](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
