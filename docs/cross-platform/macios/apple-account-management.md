---
title: Zarządzanie kontem firmy Apple
ms.prod: xamarin
ms.assetid: 71388B83-699B-4E42-8CBF-8557A4A3CABF
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 05/06/2018
ms.openlocfilehash: a4ca803085f31ff0db5dd4f194b705d765447c9d
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2018
---
# <a name="apple-account-management"></a>Zarządzanie kontem firmy Apple

Interfejs zarządzania konto Apple zapewnia sposób, aby wyświetlić wszystkie zespoły programistów skojarzone z identyfikatora firmy Apple. Umożliwia również wyświetlić szczegółowe informacje o każdym zespołu za pomocą wyświetlania listy _tożsamości podpisywania_ i _profile inicjowania obsługi_ są instalowane na tym komputerze.

Uwierzytelnianie przy użyciu Identyfikatora Apple odbywa się w wierszu polecenia z [fastlane](https://fastlane.tools/). fastlane musi być zainstalowany na tym komputerze można zostać pomyślnie uwierzytelnione. Więcej informacji na temat instalacji i fastlane została szczegółowo opisana w [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md) przewodników.

Okno dialogowe konto Apple umożliwia wykonaj następujące czynności:

* **Tworzenie i zarządzanie certyfikatami** 
* **Tworzenie i zarządzanie nimi profile aprowizacji** 

W tym przewodniku opisano informacji na temat sposobu wykonania tego zadania.

Umożliwia także automatyczne udostępnianie narzędzi dla systemu iOS do automatycznego tworzenia i zarządzania Twojej tożsamości podpisywania, identyfikatorów aplikacji i profile inicjowania obsługi.

Aby uzyskać więcej informacji na temat używania tych funkcji, zapoznaj się [Inicjowanie obsługi administracyjnej urządzeń](~/ios/get-started/installation/device-provisioning/index.md) przewodnik.
️
## <a name="requirements"></a>Wymagania

Zarządzanie kontami Apple jest dostępne w programie Visual Studio for Mac i Visual Studio 2017 (wersja 15.7 i nowsze)

Musi mieć konto Apple Developer, aby użyć tej funkcji. Więcej informacji na temat kont deweloperów firmy Apple jest dostępna w [Inicjowanie obsługi administracyjnej urządzeń](~/ios/get-started/installation/device-provisioning/index.md) przewodnik.

- Upewnij się, że masz połączenie z Internetem. Jest to spowodowane fastlane komunikuje się bezpośrednio z portalu dla deweloperów firmy Apple.
- Upewnij się, masz [zainstalowano narzędzia fastlane](~/ios/deploy-test/provisioning/fastlane/index.md#Installation).
- Upewnij się, masz najnowsze narzędzia fastlane z [ https://download.fastlane.tools ](https://download.fastlane.tools).
- Przed rozpoczęciem upewnij się zaakceptować wszystkie umowy licencyjne użytkownika w [portalu dla deweloperów](https://developer.apple.com/account/).

## <a name="adding-an-apple-developer-account"></a>Dodawanie konta deweloperów firmy Apple

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Aby otworzyć okno dialogowe Zarządzanie konta, przejdź do **programu Visual Studio > Preferencje > konta dewelopera Apple**:

    ![Opcje konta dewelopera firmy Apple](apple-account-management-images/image1.png)

2. Naciśnij klawisz **+** przycisk, aby wyświetlić w oknie dialogowym logowania, jak pokazano poniżej: 

    ![okno dialogowe fastlane.](apple-account-management-images/image2.png)

4. Wprowadź identyfikator Apple ID i hasło, a następnie kliknij przycisk **logowania** przycisku. Spowoduje to zapisanie w łańcuchu kluczy bezpiecznej poświadczeń na tym komputerze. [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md) służy do obsługi bezpiecznie poświadczeń i przekazują je do portalu dla deweloperów firmy Apple.
 
5. Wybierz **zawsze Zezwalaj** w oknie dialogowym alertu, aby umożliwić Visual Studio do korzystania z poświadczeń:

    ![Zawsze zezwalaj na okna dialogowego alertu](apple-account-management-images/image4.png)

6. Po konta został dodany pomyślnie, zostanie wyświetlony identyfikator Apple ID i wszystkie zespoły, które identyfikator Apple ID jest częścią.

    ![Okno dialogowe konta deweloperów firmy Apple z kontami dodane](apple-account-management-images/image5.png)

7. Wybierz zespołu i naciśnij klawisz **Wyświetl szczegóły...** przycisk. Spowoduje to wyświetlenie listy wszystkich tożsamości podpisywania i profile inicjowania obsługi, które są zainstalowane na komputerze:

    ![Wyświetl szczegóły ekranu przedstawiający tożsamości podpisywania i Inicjowanie obsługi profilów na tym komputerze](apple-account-management-images/image6.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Przed dodaniem swój identyfikator firmy Apple do programu Visual Studio 2017 r, upewnij się, że środowiska deweloperskiego jest [łączyć się z hostem kompilacji Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Aby otworzyć okno Zarządzanie konta, przejdź do **Narzędzia > Opcje > Xamarin > Apple kont**:

    ![Ekran opcje konta firmy Apple](apple-account-management-images/prov1.png)

1. Wybierz **Dodaj** przycisk, a następnie wprowadź identyfikator Apple ID i hasło:

    ![okno dialogowe nazwy użytkownika i hasła](apple-account-management-images/prov1a.png)

1. Po konta został dodany pomyślnie, zostanie wyświetlony identyfikator Apple ID i wszystkie zespoły, które identyfikator Apple ID jest częścią.
 
1. Wybierz zespołu i naciśnij klawisz **Wyświetl szczegóły...** przycisk. Spowoduje to wyświetlenie listy wszystkich tożsamości podpisywania i profile inicjowania obsługi, które są zainstalowane na komputerze:

    ![okno dialogowe nazwy użytkownika i hasła](apple-account-management-images/prov2.png)

-----


## <a name="managing-signing-identities-and-provisioning-profiles"></a>Zarządzanie tożsamościami podpisywania i profile aprowizacji

Okno dialogowe szczegółów zespołu zostanie wyświetlona lista tożsamości podpisywania, uporządkowane według typu. **Stan** kolumny informacją o tym, czy certyfikat jest: 

* **Nieprawidłowa** — tożsamości podpisywania (certyfikat i klucz prywatny) jest zainstalowany na tym komputerze i nie wygasł.

* **Nie znajduje się w łańcuchu kluczy** — Brak prawidłowej tożsamości podpisywania na serwerze firmy Apple. Do zainstalowania tego na komputerze, musi być eksportowany z innego komputera. Nie można pobrać tożsamości podpisywania z portalu dla deweloperów firmy Apple, ponieważ będzie ona zawierać klucz prywatny.

* **Brak klucza prywatnego** — certyfikat bez klucza prywatnego jest zainstalowany w łańcuchu kluczy.

* **Ważność** — certyfikat wygasł. Należy usunąć ten, z Twojego łańcucha kluczy.

  ![Okno Szczegóły zespołu](apple-account-management-images/image7.png)

## <a name="create-a-signing-identities"></a>Tworzenie tożsamości usługi podpisywania

Aby utworzyć nową tożsamość podpisywania, wybierz **Tworzenie certyfikatu** przycisku rozwijanego i wybierz typ, która jest wymagana. Jeśli masz odpowiednie uprawnienia podpisywania nowej tożsamości pojawi się po kilku sekundach.

Jeśli opcja w listy rozwijanej jest nieaktywny i niezaznaczoną, oznacza to, że nie mają zespołu poprawne uprawnienia do utworzenia tego typu certyfikatu.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Tworzenie certyfikatu opcji](apple-account-management-images/image8.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Tworzenie certyfikatu opcji](apple-account-management-images/prov3.png)

-----

## <a name="download-provisioning-profiles"></a>Pobierz profile inicjowania obsługi administracyjnej

Okno dialogowe szczegółów zespołu również zostanie wyświetlona lista wszystkich profilów aprowizacji podłączone do Twojego konta dewelopera. Możesz pobrać wszystkie profile inicjowania obsługi administracyjnej na komputerze lokalnym, naciskając **Pobierz wszystkie profile** przycisku

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Pobierz sekcji Profile inicjowania obsługi administracyjnej](apple-account-management-images/image9.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Pobierz sekcji Profile inicjowania obsługi administracyjnej](apple-account-management-images/prov4.png)

-----

## <a name="ios-bundle-signing"></a>Podpisywanie pakietu systemu iOS

Aby uzyskać informacje na temat wdrażania aplikacji na urządzeniu, zapoznaj się [Inicjowanie obsługi administracyjnej urządzeń](~/ios/get-started/installation/device-provisioning/index.md) przewodnik.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

### <a name="view-details-dialog-is-empty"></a>Okno dialogowe szczegółów w widoku jest pusty

Obecnie jest to znany problem związany z usterki [#53906](https://bugzilla.xamarin.com/show_bug.cgi?id=53906). Upewnij się, że używasz najnowsza stabilna wersja programu Visual Studio dla komputerów Mac

### <a name="if-you-are-experiencing-issues-logging-in-your-account-please-try-the-following"></a>Jeśli występują problemy dotyczące logowania na koncie, należy spróbować wykonać następujące czynności:

* Otwórz aplikację łańcucha kluczy i kategorii wybierz *hasła*. Wyszukaj `deliver.`i Usuń wszystkie wpisy.

### <a name="error-adding-account-please-sign-in-with-an-app-specific-password"></a>"Błąd podczas dodawania konta. Zaloguj się przy użyciu hasła specyficzny dla aplikacji"

Jest to spowodowane 2 uwierzytelnianie dwuskładnikowe jest włączone na Twoim koncie. Upewnij się, że używasz najnowsza stabilna wersja programu Visual Studio dla komputerów Mac

### <a name="failed-to-create-new-certificate"></a>Nie można utworzyć nowego certyfikatu
"Osiągnięto limit dla certyfikatów tego typu"

![okno dialogowe limit certyfikatu](apple-account-management-images/image10.png)

Maksymalna liczba certyfikatów, które mogą zostać wygenerowane. Aby rozwiązać ten problem, przejdź do [Centrum deweloperów firmy Apple](https://developer.apple.com/account/ios/certificate/distribution) i Odwołaj certyfikat produkcji.

## <a name="known-issues"></a>Znane problemy

* Profile aprowizacji dystrybucji domyślnie będą dotyczyć sklepu App Store. Profile wewnętrzne i tymczasowe powinny być tworzone ręcznie.
