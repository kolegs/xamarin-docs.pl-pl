---
title: Ręcznego inicjowania obsługi dla platformy Xamarin.iOS
description: Po pomyślnym zainstalowaniu platformy Xamarin.iOS następny krok w rozwoju systemu iOS jest aprowizować urządzenia z systemem iOS. Tym przewodniku opisano sposób konfigurowania tworzenia certyfikatów i profilów z za pomocą ręcznego inicjowania obsługi administracyjnej.
ms.prod: xamarin
ms.assetid: E26ACC94-F4A5-4FF5-B7D4-BE596745A665
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 07/15/2017
ms.openlocfilehash: dd0afe03adbd021717a88cd4409e3e1351ba9b50
ms.sourcegitcommit: e98a9ce8b716796f15de7cec8c9465c4b6bb2997
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39111189"
---
# <a name="manual-provisioning-for-xamarinios"></a>Ręcznego inicjowania obsługi dla platformy Xamarin.iOS

_Po pomyślnym zainstalowaniu platformy Xamarin.iOS następny krok w rozwoju systemu iOS jest aprowizować urządzenia z systemem iOS. Tym przewodniku opisano sposób konfigurowania tworzenia certyfikatów i profilów z za pomocą ręcznego inicjowania obsługi administracyjnej._

> [!NOTE]
> Instrukcje na tej stronie są odpowiednie dla deweloperów, którzy zapłacili dostęp do programu Apple Developer. Jeśli masz bezpłatne konto, zapoznaj się z [aprowizacji BEZPŁATNEJ](~/ios/get-started/installation/device-provisioning/free-provisioning.md) przewodnika, aby uzyskać więcej informacji na temat testowania na urządzeniach.

## <a name="creating-a-signing-identity"></a>Tworzenie tożsamości podpisywania

Jest pierwszym krokiem w procesie konfigurowania urządzenia programowania do tworzenia tożsamości podpisywania. Tożsamość do podpisywania to składa się z następujących czynności:

- Certyfikatu deweloperskiego
- Klucz prywatny

Tworzenia certyfikatów i skojarzony [klucze](#understanding-certificate-key-pairs) mają krytyczne znaczenie dla deweloperów systemu iOS: ustanawiają swoją tożsamość za pomocą firmy Apple i można skojarzyć z danego urządzenia i profilu dla rozwoju, podobnie umieszczenie podpisu cyfrowego w Twoje aplikacje. Apple sprawdza, czy certyfikaty do kontrolowania dostępu do urządzenia są dozwolone do wdrożenia.

Zespołów deweloperskich, certyfikatów i profilów, które mogą być zarządzane przez uzyskiwanie dostępu do [certyfikaty, identyfikatory i profile](https://developer.apple.com/account/overview.action) sekcji (wymagane zalogowanie) firmy Apple Member Center. Firma Apple wymaga posiadania tożsamości podpisywania do kompilacji kodu dla urządzenia lub symulatora.  

> [!IMPORTANT]
> Należy zauważyć, że możesz mieć tylko dwie certyfikatów programowania dla systemu iOS w dowolnym momencie. Jeśli musisz utworzyć więcej, należy odwołać istniejącą grupę. Wszystkie maszyny, przy użyciu odwołanego certyfikatu nie będzie się ich aplikacji.

Aby wygenerować tożsamości podpisywania, wykonaj następujące czynności:

1. Zaloguj się do [certyfikaty, identyfikatory i profile części portalu dla deweloperów](https://developer.apple.com/account/overview.action) i wybierz **certyfikaty** sekcja **aplikacji dla systemu iOS** kolumny. Kliknij przycisk **+** do utworzenia nowego certyfikatu:

    [![](manual-provisioning-images/cert-plus.png "Kliknij pozycję +, aby utworzyć nowy certyfikat")](manual-provisioning-images/cert-plus.png#lightbox)

2. Wybierz **programowanie aplikacji dla systemu iOS** opcji dla typu certyfikatu, a następnie kliknij przycisk **Kontynuuj**. Ten ekran może wyglądać inaczej w zależności od uprawnień Twojego konta:

    [![](manual-provisioning-images/cert-first.png "Wybierz opcję tworzenia aplikacji dla typu certyfikatu dla systemu iOS")](manual-provisioning-images/cert-first.png#lightbox)

3. Żądanie, żądanie podpisania certyfikatu, który zostanie przekazany do wygenerowania certyfikatu ręcznie. Aby to zrobić, uruchom **dostęp do pęku kluczy** na komputerach Mac. Przejdź do menu głównego, a następnie wybierz pozycję **Asystent certyfikatów** i **żądania certyfikatu od urzędu certyfikacji...** , jak pokazano poniżej:

      [![](manual-provisioning-images/key-first.png "Żądanie żądanie podpisania certyfikatu")](manual-provisioning-images/key-first.png#lightbox)

4. Wypełnij informacje, a następnie wybierz opcję, aby **Zapisz na dysku**:

    [![](manual-provisioning-images/key-second.png "Wypełnij informacje")](manual-provisioning-images/key-second.png#lightbox)

5. Zapisz plik CSR w lokalizacji, gdzie można je łatwo znaleźć:

    [![](manual-provisioning-images/cert-third.png "Zapisz plik CSR")](manual-provisioning-images/cert-third.png#lightbox)

6. Wróć do portalu inicjowania obsługi administracyjnej, przekazywanie certyfikatu do portalu i przesłać:

    [![](manual-provisioning-images/cert-second.png "Przekazywanie certyfikatu do portalu")](manual-provisioning-images/cert-second.png#lightbox)

    Jeśli nie masz uprawnień administratora, certyfikat musi być zatwierdzony przez administratora lub zespół agenta.

7. Po zatwierdzeniu certyfikatu można ją pobrać z portalu inicjowania obsługi administracyjnej:

    [![](manual-provisioning-images/status-dev.png "Pobierz certyfikat z poziomu portalu inicjowania obsługi administracyjnej")](manual-provisioning-images/status-dev.png#lightbox)

8. Kliknij dwukrotnie pobranego certyfikatu, aby uruchomić dostęp do pęku kluczy i Otwórz **moje certyfikaty** panelu przedstawiające nowe certyfikaty i skojarzony klucz prywatny:

    [![](manual-provisioning-images/keychain.png "Certyfikat w narzędziu Keychain Access")](manual-provisioning-images/keychain.png#lightbox)

### <a name="understanding-certificate-key-pairs"></a>Opis pary kluczy certyfikatów

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Profil dewelopera zawiera certyfikaty oraz ich skojarzonych kluczy i profilów aprowizacji skojarzonych z kontem. Istnieją dwie wersje profilu dla deweloperów — jeden jest w portalu dla deweloperów, a druga znajduje się na lokalnym komputerze Mac. Różnica między tymi dwoma to rodzaj klucze, które zawierają: _profilu w portalu przechowuje klucze publiczne skojarzone z certyfikatami, podczas kopiowania na lokalnym komputerze Mac zawiera kluczy prywatnych_. W przypadku certyfikatów ważność pary kluczy muszą być zgodne. Na lokalnym komputerze Mac, należy dysponować kopią zapasową profil dewelopera, ponieważ w przypadku utraty kluczy prywatnych certyfikatów i profilów aprowizacji należy ponownie wygenerowany.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Profil dewelopera zawiera certyfikaty oraz ich skojarzonych kluczy i profilów aprowizacji skojarzonych z kontem. Istnieją dwie wersje profilu dla deweloperów — jeden jest w portalu dla deweloperów, a druga znajduje się na komputerach Mac. Różnica między tymi dwoma to rodzaj klucze, które zawierają: _profil w domach Portal wszystkich kluczy publicznych skojarzone z certyfikatami, podczas kopiowania w przypadku komputerów Mac zawiera kluczy prywatnych_. W przypadku certyfikatów ważność pary kluczy muszą być zgodne. Dysponować kopią zapasową profil dewelopera z Xamarin kompilacji hosta komputera Mac, ponieważ w przypadku utraty kluczy prywatnych certyfikatów i profilów aprowizacji należy ponownie wygenerowany.

-----

> [!WARNING]
> Utraty certyfikat i skojarzone klucze mogą być bardzo uciążliwe, jak będzie wymagać, odwoływanie istniejących certyfikatów i ponowne inicjowanie obsługi administracyjnej żadnych skojarzonych urządzeń, łącznie z tymi zarejestrowane dla wdrożenia usługi ad-hoc. Po pomyślnym skonfigurowaniu tworzenia certyfikatów, należy wyeksportować kopii zapasowej i zapisanie ich w bezpiecznym miejscu. Więcej informacji na temat jak to zrobić, można znaleźć w sekcji eksportowania i importowania certyfikatów i profilów [obsługi certyfikatów](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/MaintainingCertificates/MaintainingCertificates.html) przewodnik w dokumentacji firmy Apple.

<a name="provisioning" />

## <a name="provisioning-an-ios-device-for-development"></a>Aprowizacja urządzenia na potrzeby programowania dla systemu iOS

Po ustanowieniu swoją tożsamość za pomocą firmy Apple i mieć certyfikatu deweloperskiego, należy skonfigurować profil aprowizowania i wymaganych jednostek więc istnieje możliwość wdrożyć aplikację na urządzeniu z systemem Apple. Urządzenie musi działać wersja systemu iOS, która jest obsługiwana przez program Xcode — może być konieczne zaktualizować urządzenie, Xcode lub obu.

<a name="adddevice" />

## <a name="add-a-device"></a>Dodaj urządzenie

Podczas tworzenia profilu aprowizacji dla rozwoju, firma Microsoft stanu urządzeń, które można uruchomić aplikację. Aby włączyć tę opcję, można dodać maksymalnie 100 urządzeń na rok kalendarzowy nasz portal dla deweloperów, a w tym miejscu można wybrać urządzenia, które mają zostać dodane do określonego profilu inicjowania obsługi administracyjnej. Wykonaj poniższe kroki na komputerze Mac, aby dodać urządzenia do portalu dla deweloperów

1. Uruchom program Xcode.
2. Podłącz urządzenie do aprowizowania z komputerem Mac przy użyciu podanej kabel USB.
2. Z **Windows** menu wybierz opcję **urządzeń**:

  [![](manual-provisioning-images/add01.png "Z Windows menu Wybierz urządzenia")](manual-provisioning-images/add01.png#lightbox)

3. Wybierz urządzenie odpowiednią dla systemu iOS z **urządzeń** liście po lewej stronie okna urządzenia.
4. Wyróżnij **identyfikator** ciągów i skopiuj go do Schowka:

  [![](manual-provisioning-images/add02.png "Wyróżnij ciągu identyfikatora")](manual-provisioning-images/add02.png#lightbox)

5. W przeglądarce Safari, przejdź do [Centrum deweloperów firmy Apple](https://developer.apple.com/membercenter/index.action) i zaloguj się.
6. Kliknij przycisk **certyfikaty, identyfikatory i profile** łącza:

  [![](manual-provisioning-images/add03.png "Kliknij pozycję certyfikaty, profile identyfikatory łącza")](manual-provisioning-images/add03.png#lightbox)

7. Kliknij pozycję **urządzeń** łącza:

  [![](manual-provisioning-images/add04.png "Kliknij łącze urządzeń")](manual-provisioning-images/add04.png#lightbox)

8. Kliknij przycisk **+** przycisku:

  [![](manual-provisioning-images/add05.png "Kliknij przycisk +")](manual-provisioning-images/add05.png#lightbox)

9. Podaj nazwę dla nowego urządzenia, a następnie wklej urządzenia **identyfikator** , firma Microsoft skopiowane powyżej do **UUID** pola:

  [![](manual-provisioning-images/add06.png "Podaj nazwę dla nowego urządzenia i identyfikator urządzenia")](manual-provisioning-images/add06.png#lightbox)

10. Kliknij przycisk **Kontynuuj** przycisku.
11. Na koniec, przejrzyj informacje i kliknij przycisk **zarejestrować** przycisku:

  [![](manual-provisioning-images/add07.png "Zapoznaj się z informacjami")](manual-provisioning-images/add07.png#lightbox)

Powtórz powyższe kroki dla wszystkich urządzeń z systemem iOS, która będzie służyć do testowania i debugowania aplikacji platformy Xamarin.iOS.

Po dodaniu urządzenia do portalu dla deweloperów, jest niezbędne do tworzenia profilu aprowizacji i dodać urządzenia do niego.

<a name="provisioningprofile" />

## <a name="creating-a-development-provisioning-profile"></a>Tworzenie, rozwój, profil inicjowania obsługi administracyjnej

Jak za pomocą certyfikatu deweloperskiego profilów aprowizacji mogą być tworzone ręcznie za pośrednictwem [certyfikaty, identyfikatory i profile](https://developer.apple.com/account/overview.action) sekcji Centrum elementy członkowskie firmy Apple.

Przed utworzeniem profilu aprowizacji, *Identyfikatora aplikacji* muszą być wykonane. Identyfikator aplikacji jest ciągiem styl wstecznego DNS, który unikatowo identyfikuje aplikację. Kroki przedstawione poniżej będą pokazują, jak utworzyć **identyfikator aplikacji z symbolami wieloznacznymi**, który może służyć do tworzenia i zainstalowania większości aplikacji. **Jawne identyfikatory aplikacji** tylko Pozwól na instalację jednej aplikacji (z dopasowania Identyfikatora pakietu) i są zwykle używane dla niektórych funkcji systemu iOS, takie jak Apple Pay i HealthKit. Aby uzyskać informacje dotyczące tworzenia jawnego identyfikatory aplikacji, zapoznaj się [Praca z funkcjami](~/ios/deploy-test/provisioning/capabilities/index.md) przewodnik.

### <a name="app-id"></a>Identyfikator aplikacji

1. W [portalu dla deweloperów](https://developer.apple.com/account/overview.action) wskaż *certyfikatu, identyfikatory i profile* sekcji w Centrum deweloperów firmy Apple. Wybierz **identyfikatory aplikacji** w obszarze **identyfikatory**.
2. Kliknij przycisk **+** przycisk, a następnie podaj **nazwa**:

    [![](manual-provisioning-images/appid05a.png "Podaj nazwę")](manual-provisioning-images/appid05a.png#lightbox)
3. Prefiks aplikacji powinny być wstępnie ustawione. Wybierz **identyfikator aplikacji z symbolami wieloznacznymi** sufiksem aplikacji. Wprowadź identyfikator pakietu w formacie `com.[DomainName].*`:

  [![](manual-provisioning-images/appid05b.png "Wprowadź identyfikator pakietu")](manual-provisioning-images/appid05b.png#lightbox)

3. Kliknij przycisk **Kontynuuj** przycisk i zgodnie z instrukcjami wyświetlanymi na ekranie, aby utworzyć nowy identyfikator aplikacji.

### <a name="provisioning-profile"></a>Profil aprowizacji

Po utworzeniu Identyfikatora aplikacji można tworzyć profilu aprowizacji. Ten profil aprowizacji zawiera informacje na *co* aplikacji (lub aplikacji, jeśli jest identyfikator aplikacji symboli wieloznacznych) dotyczy ten profil *kto* mogą używać profilu (zależności od tego, jakie developer certyfikaty są dodawane), i *co* urządzenia mogą instalować aplikacji.

Aby ręcznie utworzyć profil inicjowania obsługi administracyjnej dla rozwoju, wykonaj następujące czynności:

1. Użyj przeglądarki Safari, aby przejść do [Member Center deweloperów firmy Apple](https://developer.apple.com/membercenter/index.action)i w sekcji *certyfikaty, identyfikatory i profile* Wybierz profile inicjowania obsługi administracyjnej.
2. Kliknij przycisk **+** przycisk w prawym górnym rogu, aby utworzyć nowy profil.
3. Z **rozwoju** sekcji, zaznacz przycisk radiowy obok pozycji **programowanie aplikacji dla systemu iOS**i naciśnij klawisz **Kontynuuj**:

    [![](manual-provisioning-images/provisioning-profile01.png "Wybierz typ profilu, aby utworzyć")](manual-provisioning-images/provisioning-profile01.png#lightbox)
4. Z menu rozwijanego wybierz aplikację, ID, aby użyć:

    [![](manual-provisioning-images/provisioning-profile02.png "Wybierz aplikację identyfikator do użycia")](manual-provisioning-images/provisioning-profile02.png#lightbox)
5. Wybierz certyfikaty, aby uwzględnić w profilu inicjowania obsługi administracyjnej, a następnie naciśnij klawisz **Kontynuuj**:

    [![](manual-provisioning-images/provisioning-profile03.png "Wybierz certyfikaty, aby uwzględnić w profilu inicjowania obsługi administracyjnej")](manual-provisioning-images/provisioning-profile03.png#lightbox)
6. Wybierz urządzenia, które aplikacja zostanie zainstalowana na.

    [![](manual-provisioning-images/provisioning-profile04.png "Wybierz urządzenia, które aplikacja zostanie zainstalowana na")](manual-provisioning-images/provisioning-profile04.png#lightbox)
7. Udostępnij profil Aprowizowania do zidentyfikowania nazwę i naciśnij klawisz **Kontynuuj** do utworzenia profilu:

    [![](manual-provisioning-images/provisioning-profile05.png "Podaj nazwę profilu inicjowania obsługi administracyjnej za pomocą rozpoznawalny")](manual-provisioning-images/provisioning-profile05.png#lightbox)
8. Naciśnij klawisz **Pobierz** można pobrać profilu aprowizacji na komputerze Mac:

    [![](manual-provisioning-images/provisioning-profile06.png "Pobierz profil inicjowania obsługi administracyjnej")](manual-provisioning-images/provisioning-profile06.png#lightbox)
9. Kliknij dwukrotnie plik, aby zainstalować profil inicjowania obsługi administracyjnej w środowisku Xcode. Należy pamiętać, że środowisko Xcode może nie być widoczny, że każda wizualizacja clues, że ten składnik został zainstalowany profil z wyjątkiem otwierania. Można to sprawdzić, przechodząc do **Xcode > Preferencje > konta**. Wybierz identyfikator Apple ID, a następnie kliknij przycisk **Wyświetl szczegóły...** . Powinien zostać wyświetlony nowy profil inicjowania obsługi administracyjnej, jak przedstawiono poniżej:

      [![](manual-provisioning-images/provisioning-profile07.png "Wyświetlanie profilu w środowisku Xcode")](manual-provisioning-images/provisioning-profile07.png#lightbox)

Po pomyślnym utworzeniu profilu inicjowania obsługi administracyjnej może być potrzebny na odświeżenie środowisko Xcode, dzięki czemu wszystkie certyfikaty do rozwoju są dostępne w programie Visual Studio dla komputerów Mac i Visual Studio.

<a name="download" />

## <a name="downloading-profiles-and-certificates-in-xcode"></a>Pobieranie profilów i certyfikatów w środowisku Xcode

Certyfikaty i profile aprowizacji, które zostały utworzone w portalu dla deweloperów firmy Apple nie może automatycznie występować w środowisku Xcode. W związku z tym, może być konieczne pobranie więc one, że są one dostępne przez program Visual Studio dla komputerów Mac i Visual Studio. Zaktualizowanie i pobranie wszelkich certyfikatów, utworzone w portalu dla deweloperów firmy Apple, wykonaj następujące czynności:

1.   Zamknij program Visual Studio for Mac lub Visual Studio.
2.   Uruchom program Xcode.
3.   Wybierz **Xcode Menu > Preferencje...**
4.   Kliknij przycisk **kont** kartę.
5.   Wybierz zespół, a następnie kliknij przycisk **Pobierz profile ręczne** przycisku: [ ![ ] (manual-provisioning-images/selectteam1.png "ręczne pobranie profili")](manual-provisioning-images/selectteam1.png#lightbox)

6.   Zamknij program Xcode.
7.  Uruchom program Visual Studio for Mac lub Visual Studio.

Nowe certyfikaty lub profilów aprowizacji nie jest dostępne w programie Visual Studio for Mac lub Visual Studio i gotowe do użycia.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

> [!IMPORTANT]
> Może być konieczne zatrzymać i ponownie program Visual Studio dla komputerów Mac, zanim zostanie wyświetlony, wszystkie certyfikaty nowych lub zmodyfikowanych lub profile zaktualizowane przez Xcode.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

> [!IMPORTANT]
> Może być niezbędne do zatrzymania i ponownego uruchomienia programu Visual Studio, zanim zostanie wyświetlony, wszystkie certyfikaty nowych lub zmodyfikowanych lub profile zaktualizowane przez Xcode.

-----

<a name="appservices" />

## <a name="provisioning-for-application-services"></a>Inicjowanie obsługi administracyjnej dla usług aplikacji

Apple oferuje szereg specjalne usługi aplikacji, nazywany również możliwości, które mogą być aktywowana dla aplikacji platformy Xamarin.iOS. Te usługi aplikacji muszą być skonfigurowane na obu portalu aprowizacji systemu iOS po **Identyfikatora aplikacji** jest tworzony i **plik Entitlements.plist** pliku część projektu aplikacji platformy Xamarin.iOS. Aby uzyskać informacje na temat dodawania aplikacji usługi do aplikacji, zapoznaj się [wprowadzenie do funkcji](~/ios/deploy-test/provisioning/capabilities/index.md) przewodnik i [Praca z uprawnieniami](~/ios/deploy-test/provisioning/entitlements.md) przewodnik.

* Utwórz identyfikator aplikacji z wymaganymi usługami aplikacji.
* Utwórz nową [profil inicjowania obsługi administracyjnej](#provisioningprofile) zawierający ten identyfikator aplikacji.
* Ustawianie uprawnień do projektu Xamarin.iOS

## <a name="deploying-to-a-device"></a>Wdrażanie na urządzeniu

W tym momencie aprowizacji powinny zostać ukończone, a aplikacja jest gotowa do wdrożenia na urządzeniu. Aby to zrobić, wykonaj następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

> [!IMPORTANT]
> Przed rozpoczęciem upewnij się wybrać **ręcznego inicjowania obsługi administracyjnej** w **Info.plist**.

1. Podłącz urządzenie do komputerów Mac.
2. W projekcie **Info.plist**, upewnij się, że identyfikator pakietu jest zgodny z Identyfikatorem aplikacji (chyba że identyfikator aplikacji jest symbol wieloznaczny):

  ![](manual-provisioning-images/deploydevice01xs.png "Wprowadzając identyfikator")

3. Kliknij prawym przyciskiem myszy na projekt, aby wyświetlić okno dialogowe Opcje projektu, a następnie przejdź do **kompilacji > podpisywanie pakietu systemu iOS**. Z listy rozwijanej obok zarówno **tożsamość podpisywania** i **profilu aprowizacji**, sprawdź, czy program Visual Studio for Mac można Zobacz poprawne profile i wybierz określonej tożsamości i profilu:

  ![](manual-provisioning-images/deploydevice02xs.png "Wybieranie określonej tożsamości i profilu")

Jeśli jest ono ustawione na **automatyczne**, Visual Studio dla komputerów Mac wybierze tożsamość i profil na podstawie Identyfikatora pakietu, która została ustawiona w kroku #2.

4. Upewnij się ustawić konfigurację kompilacji **iPhone** / **iPad**, zamiast symulatora.
5. Kliknij przycisk **Uruchom** w programie Visual Studio dla komputerów Mac i wyświetlania aplikacji uruchomionej na urządzeniu.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

> [!IMPORTANT]
> Przed rozpoczęciem upewnij się wybrać **ręcznego inicjowania obsługi administracyjnej** w **projektu > właściwości aprowizacji...** .

1. Podłącz urządzenie z hostem kompilacji Mac.
2. W projekcie **Info.plist**, upewnij się, że identyfikator pakietu jest zgodny z Identyfikatorem aplikacji:

  ![](manual-provisioning-images/servicevs01.png "Wprowadzając identyfikator")

3. Kliknij prawym przyciskiem myszy na projekt, aby wyświetlić okno dialogowe Opcje projektu i przejdź do **kompilacji > podpisywanie pakietu systemu iOS**. Z listy rozwijanej obok zarówno **tożsamość podpisywania** i **profilu aprowizacji** Sprawdź, czy program Visual Studio można Zobacz poprawne profile i wybierz określonej tożsamości i profilu.

    Jeśli jest ono ustawione na **automatyczne**, tożsamość i profil na podstawie Identyfikatora pakietu, który został ustawiony w kroku #2 wybierze się programu Visual Studio.

4. Upewnij się ustawić konfigurację kompilacji **iPhone** lub **iPad**, zamiast symulatora.
5. Kliknij przycisk **Uruchom** w programie Visual Studio i wyświetlania aplikacji uruchomionej na urządzeniu.


-----

## <a name="summary"></a>Podsumowanie

W przewodniku opisano kroki wymagane do instalacji środowisko programistyczne dla platformy Xamarin.iOS. Przedstawione go, jak aplikacja jest kod podpisany przy użyciu informacji o dewelopera, ich zespołu, urządzenia, które aplikacja może działać na i identyfikator poszczególnych aplikacji.

## <a name="related-links"></a>Linki pokrewne

- [Bezpłatna aprowizacja](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [Dystrybucja aplikacji](~/ios/deploy-test/app-distribution/index.md)
- [Rozwiązywanie problemów](~/ios/deploy-test/troubleshooting.md)
- [Apple — podręczniku dystrybucji aplikacji](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
