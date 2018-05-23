---
title: Automatyczne inicjowanie obsługi administracyjnej
description: Po pomyślnym zainstalowaniu platformy Xamarin.iOS następny krok w rozwoju systemu iOS jest obsługi administracyjnej urządzeniu z systemem iOS. Ten przewodnik opisuje za pomocą podpisywania automatycznego żądania certyfikatów programowanie i profilów.
ms.prod: xamarin
ms.assetid: 81FCB2ED-687C-40BC-ABF1-FB4303034D01
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 05/22/2018
ms.openlocfilehash: d324e469ba392b14c635990d607bf04c949ad5db
ms.sourcegitcommit: 9f8e7393019791bbd6af4fefaa24a1602adabb4e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/23/2018
---
# <a name="automatic-provisioning"></a>Automatyczne inicjowanie obsługi administracyjnej

_Po pomyślnym zainstalowaniu platformy Xamarin.iOS następny krok w rozwoju systemu iOS jest obsługi administracyjnej urządzeniu z systemem iOS. Ten przewodnik opisuje za pomocą podpisywania automatycznego żądania certyfikatów programowanie i profilów._

## <a name="requirements"></a>Wymagania

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

- Visual Studio dla komputerów Mac 7.3 lub większa
- Xcode 9 lub nowsza

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

- Visual Studio 2017 wersji 15.7 (lub nowszego)

Możesz muszą również łączyć się z hostem kompilacji Mac, który ma następujące:

- Xcode 9 lub nowsza

-----

## <a name="enabling-automatic-signing"></a>Włączenie automatycznego podpisywania

Przed rozpoczęciem procesu podpisywania automatyczne powinny zapewnić zostały dodane w programie Visual Studio identyfikator Apple ID, zgodnie z opisem w [zarządzania kontami Apple](~/cross-platform/macios/apple-account-management.md) przewodnik. Po dodaniu identyfikator Apple ID, można użyć dowolnego skojarzonego _zespołu_. Dzięki temu certyfikaty, profile i innych identyfikatorów, które ma zostać wykonane przed zespołu. Identyfikator jest również używany do tworzenia zespołu prefiks dla Identyfikatora aplikacji, która zostanie uwzględniona w profilu inicjowania obsługi administracyjnej. Mając to umożliwia firmy Apple sprawdzić, za kogo się podaje się, że są.

> [!IMPORTANT]
> Przed rozpoczęciem upewnij się, że Zaloguj się do albo [iTunes Connect](https://itunesconnect.apple.com/) lub [appleid.apple.com](https://appleid.apple.com) do sprawdzenia zaakceptowali najnowszą zasady konta firmy Apple. Jeśli zostanie wyświetlony monit, wykonaj kroki, aby zaakceptować żadnych nowych umów konto od firmy Apple. Jeśli nie zaakceptujesz umowę prywatności z maja 2018 podczas próby zainicjowania obsługi administracyjnej urządzeniu zostanie wyświetlony następujący alert:
> ```
> Unexpected authentication failure. Reason: {
> "authType" : "sa"
>}
>```

Aby automatycznie zarejestrować aplikację do wdrożenia na urządzeniu z systemem iOS, wykonaj następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Otwórz projekt dla systemu iOS w programie Visual Studio dla komputerów Mac.

2. Otwórz **Info.plist** pliku.

3. W **podpisywanie** wybierz wybierz **automatyczne udostępnianie**:

    ![Lista rozwijana selektora zespołu](automatic-provisioning-images/image2.png)

4. Wybierz zespół z **zespołu** listy rozwijanej.

6. Po kilku sekundach zostanie utworzony profil certyfikatu podpisywania i inicjowania obsługi administracyjnej:

    ![Pomyślnie utworzono certyfikat i profil](automatic-provisioning-images/image5.png)

    Jeśli automatyczne podpisywania nie powiedzie się **automatyczne konsoli podpisywania** wyświetli przyczynę błędu.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Sparuj programu Visual Studio 2017 na komputerze Mac, zgodnie z opisem w [pary Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) przewodnik.

2. Otwórz opcje obsługi administracyjnej, wybierając **projektu > inicjowania obsługi administracyjnej właściwości...**

3. Wybierz **automatyczne Inicjowanie obsługi administracyjnej** schematu:

    ![Wybór systemu automatycznego](automatic-provisioning-images/prov4.png)

4. Wybierz zespół z **zespołu** pole kombi, aby rozpocząć proces podpisywania automatycznego.

    ![Wybór zespołu](automatic-provisioning-images/prov3.png)

4. Spowoduje to uruchomienie automatyczne proces podpisywania. Visual Studio następnie próbuje wygenerować identyfikator aplikacji, profilu inicjowania obsługi administracyjnej i tożsamości podpisywania do użycia podczas podpisywania tych artefaktów. Można wyświetlić proces generowania danych wyjściowych kompilacji:

    ![Tworzenie generowania danych wyjściowych przedstawiający artefaktów](automatic-provisioning-images/prov5.png)

-----

## <a name="triggering-automatic-provisioning"></a>Wyzwalania automatycznego inicjowania obsługi administracyjnej

Jeśli włączono automatyczne podpisywania programu Visual Studio for Mac spowoduje zaktualizowanie tych artefaktów w razie potrzeby gdy wystąpi dowolne z następujących czynności:

* Urządzenia z systemem iOS jest podłączone do komputera Mac
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
