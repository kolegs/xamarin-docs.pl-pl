---
title: Zarządzanie kontem Apple
description: W tym dokumencie opisano sposób korzystania z funkcji zarządzania kontem firmy Apple w programie Visual Studio dla komputerów Mac i Visual Studio 2017.
ms.prod: xamarin
ms.assetid: 71388B83-699B-4E42-8CBF-8557A4A3CABF
author: asb3993
ms.author: amburns
ms.date: 05/06/2018
ms.openlocfilehash: 4557d3b055e5c49842b9fdcff1dac9ee996e8bab
ms.sourcegitcommit: be4da0cd7e1a915e3b8932a7e3d6bcd74c7055be
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38986021"
---
# <a name="apple-account-management"></a>Zarządzanie kontem Apple

Interfejs zarządzania konta Apple zapewnia sposób wyświetlenia wszystkich zespołów programistycznych skojarzony z identyfikatora Apple ID. Umożliwia także wyświetlić więcej szczegółów na temat każdego zespołu, wyświetlając listę _tożsamości podpisywania_ i _profilów aprowizacji_ są instalowane na komputerze.

Uwierzytelnianie identyfikator Apple ID odbywa się w wierszu polecenia za pomocą [programu fastlane](https://fastlane.tools/). Program fastlane musi być zainstalowany na komputerze służących do pomyślnego uwierzytelniania. Więcej informacji na temat program fastlane i sposobie jego instalowania została szczegółowo opisana w [programu fastlane](~/ios/deploy-test/provisioning/fastlane/index.md) przewodników.

Okno dialogowe konta Apple można wykonać następujące czynności:

* **Tworzenie certyfikatów i zarządzanie nimi**
* **Tworzenie i zarządzanie nimi profile aprowizacji**

W tym przewodniku opisano informacji na temat sposobu wykonania tego zadania.

> [!NOTE]
> Narzędzia Xamarin dla Zarządzanie kontem Apple zawierają tylko informacje o płatnego konta programu Apple developer. Aby dowiedzieć się, jak przetestować aplikację na urządzeniu bez płatnego konta dewelopera firmy Apple, zobacz [swobodnego udostępniania dla aplikacji platformy Xamarin.iOS](~/ios/get-started/installation/device-provisioning/free-provisioning.md) przewodnik.

Narzędzia do automatycznej aprowizacji systemu iOS umożliwia również automatycznie utworzyć i zarządzać tożsamości podpisywania, identyfikatory aplikacji i profilów aprowizacji. Aby uzyskać więcej informacji na temat korzystania z tych funkcji, zobacz [Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md) przewodnik.

## <a name="requirements"></a>Wymagania

Zarządzanie kontem Apple dostępnej w programie Visual Studio dla komputerów Mac i Visual Studio 2017 (w wersji 15.7 lub nowszej)

Musi mieć konto usługi dla deweloperów firmy Apple, aby użyć tej funkcji. Więcej informacji na temat konta programu Apple developer jest dostępna w [Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md) przewodnik.

- Upewnij się, że masz połączenie z Internetem. Jest to spowodowane programu fastlane komunikuje się bezpośrednio z portalu dla deweloperów firmy Apple.
- Upewnij się, że [zainstalowane narzędzia programu fastlane](~/ios/deploy-test/provisioning/fastlane/index.md#Installation).
- Upewnij się, że najnowsze narzędzia programu fastlane z [ https://download.fastlane.tools ](https://download.fastlane.tools).
- Przed rozpoczęciem upewnij się, że Zaakceptuj umowy licencyjne użytkownika w [portalu dla deweloperów](https://developer.apple.com/account/).

## <a name="adding-an-apple-developer-account"></a>Dodawanie konta dewelopera firmy Apple

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Aby otworzyć okno dialogowe zarządzania konta, przejdź do **programu Visual Studio > Preferencje > konto programu Apple Developer**:

    ![Opcje konta dewelopera firmy Apple](apple-account-management-images/image1.png)

2. Naciśnij klawisz **+** przycisk, aby wyświetlić w oknie dialogowym logowania, jak przedstawiono poniżej: 

    ![okno dialogowe programu fastlane.](apple-account-management-images/image2.png)

4. Wprowadź identyfikator Apple ID i hasło, a następnie kliknij przycisk **Sign In** przycisku. To spowoduje zapisanie poświadczeń w bezpiecznym pęku kluczy na tym komputerze. [Program fastlane](~/ios/deploy-test/provisioning/fastlane/index.md) służy do obsługi bezpiecznego swoje poświadczenia i przekazywać je do portalu dla deweloperów firmy Apple.
 
5. Wybierz **zawsze Zezwalaj na** w oknie dialogowym alertu, aby umożliwić programu Visual Studio do korzystania z poświadczeń:

    ![Zawsze zezwalaj na okna dialogowego alertu](apple-account-management-images/image4.png)

6. Po konta został dodany pomyślnie, zostanie wyświetlony identyfikator Apple ID i wszystkie zespoły, które identyfikator Apple ID jest częścią.

    ![Okno dialogowe konta dewelopera firmy Apple przy użyciu kont dodanych](apple-account-management-images/image5.png)

7. Wybierz zespół i naciśnij klawisz **Wyświetl szczegóły...** przycisk. Spowoduje to wyświetlenie listy wszystkich tożsamości podpisywania i profilów aprowizacji, które są zainstalowane na komputerze:

    ![Widok przedstawiający ekran szczegóły tożsamości podpisywania i inicjowania obsługi profilów na komputerze](apple-account-management-images/image6.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Przed dodaniem identyfikator Apple ID do programu Visual Studio 2017, upewnij się, że środowiska deweloperskiego jest [Sparowano z hostem kompilacji Mac.](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Aby otworzyć okno Zarządzanie konta, przejdź do **Narzędzia > Opcje > Xamarin > konta Apple**:

    ![Ekran opcji konta Apple](apple-account-management-images/prov1.png)

1. Wybierz **Dodaj** przycisk, a następnie wprowadź identyfikator Apple ID i hasło:

    ![okno dialogowe nazwy użytkownika i hasła](apple-account-management-images/prov1a.png)

1. Po konta został dodany pomyślnie, zostanie wyświetlony identyfikator Apple ID i wszystkie zespoły, które identyfikator Apple ID jest częścią.
 
1. Wybierz zespół i naciśnij klawisz **Wyświetl szczegóły...** przycisk. Spowoduje to wyświetlenie listy wszystkich tożsamości podpisywania i profilów aprowizacji, które są zainstalowane na komputerze:

    ![okno dialogowe nazwy użytkownika i hasła](apple-account-management-images/prov2.png)

-----


## <a name="managing-signing-identities-and-provisioning-profiles"></a>Profile aprowizacji i zarządzanie nimi tożsamości podpisywania

Okno dialogowe szczegółów zespołu Wyświetla listę tożsamości podpisywania, uporządkowane według typu. **Stan** kolumny informacją o tym, czy certyfikat jest: 

* **Nieprawidłowa** — tożsamości podpisywania (certyfikat i klucz prywatny) jest zainstalowany na komputerze i nie wygasł.

* **Nie w łańcuchu kluczy** — Brak prawidłowej tożsamości podpisywania na serwerze firmy Apple. Aby zainstalować to na komputerze, musi być eksportowany z innego komputera. Nie można pobrać tożsamości podpisywania z portalu dla deweloperów firmy Apple, ponieważ nie będzie zawierać klucz prywatny.

* **Brak klucza prywatnego** — certyfikat bez klucza prywatnego jest zainstalowany w pęku kluczy.

* **Wygasłe** — certyfikat nie wygasł. Należy usunąć ten, z łańcucha kluczy.

  ![informacje okna dialogowego Szczegóły zespołu](apple-account-management-images/image7.png)

## <a name="create-a-signing-identities"></a>Tworzenie tożsamości podpisywania

Aby utworzyć nową tożsamość podpisywania, wybierz **Tworzenie certyfikatu** przycisk listy rozwijanej i wybierz typ, która jest wymagana. Jeśli masz odpowiednie uprawnienia podpisywania nowej tożsamości pojawi się po kilku sekundach.

Jeśli opcja listy rozwijanej jest wyszarzona i niezaznaczoną, oznacza to, nie masz uprawnień właściwego zespołu, aby utworzyć ten typ certyfikatu.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Tworzenie certyfikatu opcji](apple-account-management-images/image8.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Tworzenie certyfikatu opcji](apple-account-management-images/prov3.png)

-----

## <a name="download-provisioning-profiles"></a>Pobrać profilów aprowizacji

Okno dialogowe szczegółów zespół również Wyświetla listę wszystkich profilów aprowizacji połączony z kontem dewelopera. Wszystkie profile aprowizacji można pobrać na komputer lokalny, naciskając klawisz **Pobierz wszystkie profile** przycisku

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Pobierz sekcji Profile inicjowania obsługi administracyjnej](apple-account-management-images/image9.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Pobierz sekcji Profile inicjowania obsługi administracyjnej](apple-account-management-images/prov4.png)

-----

## <a name="ios-bundle-signing"></a>Podpisywanie pakietu systemu iOS

Aby uzyskać informacje na temat wdrażania aplikacji na urządzeniu, zapoznaj się [Inicjowanie obsługi administracyjnej urządzeń](~/ios/get-started/installation/device-provisioning/index.md) przewodnik.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

### <a name="view-details-dialog-is-empty"></a>Wyświetl szczegóły w oknie dialogowym jest pusty

Obecnie jest to znany problem, odnoszące się do usterki [#53906](https://bugzilla.xamarin.com/show_bug.cgi?id=53906). Upewnij się, że używasz najnowszej stabilnej wersji programu Visual Studio dla komputerów Mac

### <a name="if-you-are-experiencing-issues-logging-in-your-account-please-try-the-following"></a>Jeśli występują problemy dotyczące logowania na koncie, spróbuj wykonać następujące czynności:

* Otwórz aplikację pęku kluczy i w obszarze Kategoria wybierz *haseł*. Wyszukaj `deliver.`i Usuń wszystkie wpisy.

### <a name="error-adding-account-please-sign-in-with-an-app-specific-password"></a>"Błąd podczas dodawania konta. Zaloguj się przy użyciu hasła dla aplikacji"

Jest to spowodowane 2 uwierzytelnianie dwuskładnikowe jest włączone na Twoim koncie. Upewnij się, że używasz najnowszej stabilnej wersji programu Visual Studio dla komputerów Mac

### <a name="failed-to-create-new-certificate"></a>Nie można utworzyć nowego certyfikatu
"Osiągnięto limit dla certyfikatów tego typu"

![okno dialogowe certyfikat limit](apple-account-management-images/image10.png)

Maksymalna liczba certyfikatów, które mogą mieć został wygenerowany. Aby rozwiązać ten problem, przejdź do [Centrum deweloperów firmy Apple](https://developer.apple.com/account/ios/certificate/distribution) i odwoływanie jeden z certyfikatów w środowisku produkcyjnym.

## <a name="known-issues"></a>Znane problemy

* Profile aprowizacji dystrybucji domyślnie będą dotyczyć sklepu App Store. Profile wewnętrzne i tymczasowe powinny być tworzone ręcznie.
