---
title: Automatyczne inicjowanie obsługi administracyjnej dla platformy Xamarin.iOS
description: Po pomyślnym zainstalowaniu platformy Xamarin.iOS następny krok w rozwoju systemu iOS jest aprowizować urządzenia z systemem iOS. Ten przewodnik opisuje, za pomocą automatycznego podpisywania żądania tworzenia certyfikatów i profilów.
ms.prod: xamarin
ms.assetid: 81FCB2ED-687C-40BC-ABF1-FB4303034D01
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 05/22/2018
ms.openlocfilehash: a0c3179dc8e349c23d5521230e0957d1be9384ec
ms.sourcegitcommit: be4da0cd7e1a915e3b8932a7e3d6bcd74c7055be
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38986190"
---
# <a name="automatic-provisioning-for-xamarinios"></a>Automatyczne inicjowanie obsługi administracyjnej dla platformy Xamarin.iOS

_Po pomyślnym zainstalowaniu platformy Xamarin.iOS następny krok w rozwoju systemu iOS jest aprowizować urządzenia z systemem iOS. Ten przewodnik opisuje, za pomocą automatycznego podpisywania żądania tworzenia certyfikatów i profilów._

## <a name="requirements"></a>Wymagania

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

- Program Visual Studio dla komputerów Mac 7.3 lub większa
- Środowisko Xcode 9 lub nowszej

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

- Visual Studio 2017 w wersji 15.7 (lub nowszego)

Użytkownik, należy również sparowano z hostem kompilacji komputera Mac, który ma następujące:

- Środowisko Xcode 9 lub nowszej

-----

## <a name="enabling-automatic-signing"></a>Włączenie automatycznego podpisywania

Przed rozpoczęciem procesu automatycznego podpisywania, należy upewnić się, czy masz identyfikator Apple ID dodane w programie Visual Studio, zgodnie z opisem w [Zarządzanie kontem Apple](~/cross-platform/macios/apple-account-management.md) przewodnik. Po dodaniu identyfikatora Apple ID, możesz użyć dowolnego skojarzonego _zespołu_. Dzięki temu, certyfikaty, profile i innych identyfikatorów wykonane w stosunku do zespołu. Zespół identyfikator jest również używany do tworzenia prefiks dla Identyfikatora aplikacji, które zostaną uwzględnione w profilu inicjowania obsługi administracyjnej. Posiadanie to umożliwia firmy Apple, aby sprawdzić, za kogo się podaje się, że jesteś.

> [!IMPORTANT]
> Przed rozpoczęciem upewnij się, że Zaloguj się do albo [iTunes Connect](https://itunesconnect.apple.com/) lub [appleid.apple.com](https://appleid.apple.com) do sprawdzenia zaakceptowali najnowszą zasady kont firmy Apple. Jeśli zostanie wyświetlony monit, wykonaj kroki, aby akceptować żadnych nowych umów konto od firmy Apple. Jeśli nie zaakceptujesz umowę zachowania od maja 2018, otrzymasz następujący alert podczas próby aprowizacji urządzenia:
> ```
> Unexpected authentication failure. Reason: {
> "authType" : "sa"
>}
>```

Aby automatycznie zarejestrować aplikację do wdrożenia na urządzeniu z systemem iOS, wykonaj następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Otwórz projekt dla systemu iOS w programie Visual Studio dla komputerów Mac.

2. Otwórz **Info.plist** pliku.

3. W **podpisywanie** sekcji, wybierz pozycję **automatycznej aprowizacji**:

    ![Lista rozwijana selektora zespołu](automatic-provisioning-images/image2.png)

4. Wybierz zespół z **zespołu** listy rozwijanej.

6. Po kilku sekundach zostanie utworzony profil certyfikatu podpisywania i inicjowania obsługi:

    ![Pomyślnie utworzono certyfikat i profil](automatic-provisioning-images/image5.png)

    Jeśli automatyczne podpisywanie nie powiedzie się **automatycznego podpisywania konsola** wyświetli przyczyną tego błędu.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Sparuj programu Visual Studio 2017 do komputera Mac, zgodnie z opisem w [Sparuj z komputerem Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) przewodnik.

2. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nazwę projektu i wybierz **właściwości**. Następnie przejdź do **podpisywanie pakietu systemu iOS** kartę.

3. Wybierz **automatycznej aprowizacji** schemat:

    ![Wybór systemu automatycznej](automatic-provisioning-images/prov4.png)

4. Wybierz zespół z **zespołu** pola kombi, aby rozpocząć proces automatycznego podpisywania.

    ![Wybór zespołu](automatic-provisioning-images/prov3.png)

4. Spowoduje to uruchomienie procesu automatycznego podpisywania. Program Visual Studio następnie próbuje wygenerować identyfikator aplikacji profilu aprowizacji i tożsamości podpisywania do użycia tych artefaktów podczas podpisywania. Możesz zobaczyć proces generowania danych wyjściowych kompilacji:

    ![Generowanie wyświetlanie danych wyjściowych artefaktów kompilacji](automatic-provisioning-images/prov5.png)

-----

## <a name="triggering-automatic-provisioning"></a>Wyzwalanie automatycznej aprowizacji

Jeśli włączono automatyczne podpisywanie programu Visual Studio dla komputerów Mac zaktualizuje tych artefaktów w razie potrzeby gdy wystąpi dowolne z następujących czynności:

* Urządzenia z systemem iOS jest podłączone do komputera Mac
    - Automatycznie sprawdza, czy urządzenie jest zarejestrowany w portalu dla deweloperów firmy Apple. Jeśli nie, będzie go dodać i generowania nowego profilu aprowizacji, który go zawiera.
* Identyfikator pakietu aplikacji zostanie zmieniony.
    - Spowoduje to zaktualizowanie identyfikator aplikacji. Nowy profil aprowizacji zawierający ten identyfikator aplikacji jest tworzony.
* Obsługiwana możliwość jest włączona w pliku plik Entitlements.plist.
    - Ta funkcja zostanie dodany do Identyfikatora aplikacji oraz z zaktualizowaną aplikację, której identyfikator jest generowany nowy profil aprowizacji.
    - Nie wszystkie funkcje są obecnie obsługiwane. Aby uzyskać więcej informacji na temat tych, które są obsługiwane, zapoznaj się [Praca z funkcjami](~/ios/deploy-test/provisioning/capabilities/index.md) przewodnik.


## <a name="related-links"></a>Linki pokrewne

- [Bezpłatna aprowizacja](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [Dystrybucja aplikacji](~/ios/deploy-test/app-distribution/index.md)
- [Rozwiązywanie problemów](~/ios/deploy-test/troubleshooting.md)
- [Apple — podręczniku dystrybucji aplikacji](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
