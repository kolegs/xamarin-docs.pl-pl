---
title: "Automatyczne inicjowanie obsługi administracyjnej"
description: "Po pomyślnym zainstalowaniu platformy Xamarin.iOS następny krok w rozwoju systemu iOS jest obsługi administracyjnej urządzeniu z systemem iOS. Ten przewodnik opisuje przy użyciu automatycznego podpisywania w programie Visual Studio dla komputerów Mac do żądania certyfikatów programowanie i profilów."
ms.topic: article
ms.prod: xamarin
ms.assetid: 81FCB2ED-687C-40BC-ABF1-FB4303034D01
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 11/17/2017
ms.openlocfilehash: a411c214e35f78ff9d3dd8d4e9122702d66a2156
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="automatic-provisioning"></a>Automatyczne inicjowanie obsługi administracyjnej

_Po pomyślnym zainstalowaniu platformy Xamarin.iOS następny krok w rozwoju systemu iOS jest obsługi administracyjnej urządzeniu z systemem iOS. Ten przewodnik opisuje przy użyciu automatycznego podpisywania w programie Visual Studio dla komputerów Mac do żądania certyfikatów programowanie i profilów._

## <a name="requirements"></a>Wymagania

- Visual Studio dla komputerów Mac 7.3 lub większa
- Xcode 9 lub nowsza

> [!IMPORTANT]
>  W tym przewodniku przedstawiono sposób konfigurowania urządzenia firmy Apple dla wdrożenia za pomocą programu Visual Studio dla komputerów Mac i wdrażania aplikacji. Ręczne kroki w tym celu lub w tym celu z programu Visual Studio w systemie Windows, zalecane jest, postępuj zgodnie z instrukcjami szczegółowe [ręcznego inicjowania obsługi administracyjnej](~/ios/get-started/installation/device-provisioning/manual-provisioning.md) przewodnik.

## <a name="enabling-automatic-signing"></a>Włączenie automatycznego podpisywania

Przed rozpoczęciem procesu podpisywania automatyczne powinny zapewnić zostały dodane w programie Visual Studio dla komputerów Mac, identyfikator Apple ID, zgodnie z opisem w [zarządzania kontami Apple](~/cross-platform/macios/apple-account-management.md) przewodnik. Po dodaniu identyfikator Apple ID, można użyć dowolnego skojarzonego _zespołu_. Dzięki temu certyfikaty, profile i innych identyfikatorów, które ma zostać wykonane przed zespołu. Identyfikator jest również używany do tworzenia zespołu prefiks dla Identyfikatora aplikacji, która zostanie uwzględniona w profilu inicjowania obsługi administracyjnej. Mając to umożliwia firmy Apple sprawdzić, za kogo się podaje się, że są.

Aby automatycznie zarejestrować aplikację do wdrożenia na urządzeniu z systemem iOS, wykonaj następujące czynności:

1. Otwórz projekt dla systemu iOS w programie Visual Studio dla komputerów Mac.

2. Otwórz **Info.plist** pliku.

3. W **podpisywanie** wybierz wybierz **automatyczne udostępnianie**:

    ![Lista rozwijana selektora zespołu](automatic-provisioning-images/image2.png)

4. Wybierz zespół z **zespołu** listy rozwijanej.

6. Po kilku sekundach zostanie utworzony profil certyfikatu podpisywania i inicjowania obsługi administracyjnej:

    ![Pomyślnie utworzono certyfikat i profil](automatic-provisioning-images/image5.png)

    Jeśli automatyczne podpisywania nie powiedzie się **automatyczne konsoli podpisywania** wyświetli przyczynę błędu.

## <a name="triggering-automatic-provisioning"></a>Wyzwalania automatycznego inicjowania obsługi administracyjnej

Jeśli włączono automatyczne podpisywania programu Visual Studio for Mac spowoduje zaktualizowanie tych artefaktów w razie potrzeby gdy wystąpi dowolne z następujących czynności:

* Urządzenia z systemem iOS jest podłączone do komputera mac
    - To automatycznie sprawdza, czy urządzenie jest zarejestrowany w portalu dla deweloperów firmy Apple. Jeśli nie, będzie go dodać i generowania nowego profilu inicjowania obsługi administracyjnej, który go zawiera.
* Identyfikator pakietu aplikacji zostanie zmieniona.
    - Spowoduje to zaktualizowanie identyfikator aplikacji. Utworzono nowy profil aprowizacji zawierający ten identyfikator aplikacji.
* Obsługiwane możliwości jest włączone w pliku Entitlements.plist.
    - Ta funkcja zostanie dodany do Identyfikatora aplikacji i profilu inicjowania obsługi administracyjnej z zaktualizowaną aplikację, której identyfikator jest generowany.
    - Nie wszystkie funkcje są obecnie obsługiwane. Aby uzyskać więcej informacji na te, które są obsługiwane, zapoznaj się z [Praca z funkcjami](~/ios/deploy-test/provisioning/capabilities/index.md) przewodnik.


## <a name="related-links"></a>Linki pokrewne

- [Bezpłatna aprowizacja](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [Dystrybucja aplikacji](~/ios/deploy-test/app-distribution/index.md)
- [Rozwiązywanie problemów](~/ios/deploy-test/troubleshooting.md)
- [Apple — podręczniku dystrybucji aplikacji](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
