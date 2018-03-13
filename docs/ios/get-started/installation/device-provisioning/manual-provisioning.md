---
title: "Ręcznego inicjowania obsługi administracyjnej"
description: "Po pomyślnym zainstalowaniu platformy Xamarin.iOS następny krok w rozwoju systemu iOS jest obsługi administracyjnej urządzeniu z systemem iOS. Ten przewodnik może zapoznać żądanie certyfikatów programowanie i profilach, Praca z usługi aplikacji i wdrażania aplikacji na urządzeniu."
ms.topic: article
ms.prod: xamarin
ms.assetid: E26ACC94-F4A5-4FF5-B7D4-BE596745A665
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/15/2017
ms.openlocfilehash: 2ad3bd55ae0abc44b0c9757bd79c2711eddf171d
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="manual-provisioning"></a>Ręcznego inicjowania obsługi administracyjnej

_Po pomyślnym zainstalowaniu platformy Xamarin.iOS następny krok w rozwoju systemu iOS jest obsługi administracyjnej urządzeniu z systemem iOS. Ten przewodnik może zapoznać żądanie certyfikatów programowanie i profilach, Praca z usługi aplikacji i wdrażania aplikacji na urządzeniu._

<a name="signingidentity" />

## <a name="creating-a-signing-identity"></a>Tworzenie tożsamości podpisywania

Pierwszym etapem konfigurowania urządzenia programowanie jest do tworzenia tożsamości podpisywania. Tożsamości podpisywania obejmuje dwie czynności:

- Certyfikatu deweloperskiego
- Klucz prywatny

Programowanie certyfikaty i skojarzone [klucze](#keypairs) są krytyczne dla deweloperów systemu iOS: ustalenia tożsamości z firmy Apple i można skojarzyć z danym urządzeniem i profilu dla rozwoju podobnie umieszczania podpisu cyfrowego w aplikacje. Apple sprawdza, czy certyfikaty do kontrolowania dostępu do urządzenia, są dozwolone w celu wdrożenia.

Zespoły deweloperów, certyfikaty i profile, które mogą być zarządzane przez uzyskiwanie dostępu do [certyfikaty, identyfikatory & profile](https://developer.apple.com/account/overview.action) sekcji Centrum członków firmy Apple. Firma Apple wymaga posiadania tożsamości podpisywania kodu dla urządzenie lub symulator tworzenie.  

> [!IMPORTANT]
> Należy pamiętać, że może zawierać tylko dwa certyfikaty programowanie dla systemu iOS w dowolnym momencie. Jeśli trzeba utworzyć więcej, konieczne będzie odwołać już istniejący. Maszyn przy użyciu certyfikat został odwołany nie będzie mógł zalogować się ich aplikacji.

Aby wygenerować tożsamości podpisywania, wykonaj następujące czynności:

1. Zaloguj się do [certyfikaty identyfikatory i profile części portalu dla deweloperów](https://developer.apple.com/account/overview.action) i wybierz **certyfikaty** sekcji z **aplikacji dla systemu iOS** kolumny. Następnie kliknij przycisk  **+**  można utworzyć nowego certyfikatu:

    [![](manual-provisioning-images/cert-plus.png "Kliknij pozycję +, aby utworzyć nowy certyfikat")](manual-provisioning-images/cert-plus.png#lightbox)

2. Wybierz **tworzenie aplikacji dla systemu iOS** opcji dla typów certyfikatów, a następnie kliknij przycisk **Kontynuuj**. Ten ekran mogą różnić się w zależności od uprawnień z konta:

    [![](manual-provisioning-images/cert-first.png "Wybierz opcję tworzenia aplikacji dla typów certyfikatów dla systemu iOS")](manual-provisioning-images/cert-first.png#lightbox)

3. Żądanie żądanie podpisania certyfikatu, który zostanie przekazany do wygenerowania certyfikatu ręcznie. Aby to zrobić, uruchom **dostęp łańcucha kluczy** na komputerach Mac. Przejdź do menu głównego i wybierz **Asystent certyfikatu** i **zażądać certyfikatu od urzędu certyfikacji...** , jak pokazano poniżej:

      [![](manual-provisioning-images/key-first.png "Żądanie żądania podpisania certyfikatu")](manual-provisioning-images/key-first.png#lightbox)

4. Wypełnij informacje, a następnie wybierz opcję **Zapisz, aby dysk**:

    [![](manual-provisioning-images/key-second.png "Wypełnij informacje")](manual-provisioning-images/key-second.png#lightbox)

5. Zapisz CSR w lokalizacji, w którym można je łatwo znaleźć:

    [![](manual-provisioning-images/cert-third.png "Zapisz obsługi klienta")](manual-provisioning-images/cert-third.png#lightbox)

6. Wróć do portalu inicjowania obsługi administracyjnej, Przekaż certyfikat do portalu i przesyłania:

    [![](manual-provisioning-images/cert-second.png "Przekaż certyfikat do portalu")](manual-provisioning-images/cert-second.png#lightbox)

    Jeśli nie masz uprawnień administratora, certyfikat musi być zatwierdzony przez administratora lub zespołu agenta.

7. Po zatwierdzeniu certyfikatu należy go pobrać z portalu inicjowania obsługi administracyjnej:

    [![](manual-provisioning-images/status-dev.png "Pobierz certyfikat z portalu inicjowania obsługi administracyjnej")](manual-provisioning-images/status-dev.png#lightbox)

8. Kliknij dwukrotnie pobrany certyfikat do uruchomienia dostęp łańcucha kluczy i otworzyć **moje certyfikaty** panelu wyświetlane nowe certyfikaty i skojarzony klucz prywatny:

    [![](manual-provisioning-images/keychain.png "Certyfikat w łańcuchu kluczy dostępu")](manual-provisioning-images/keychain.png#lightbox)

<a name="keypairs" />

### <a name="understanding-certificate-key-pairs"></a>Pary kluczy opis certyfikatu

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Profil Developer zawiera certyfikaty, ich skojarzone klucze i żaden profil inicjowania obsługi administracyjnej skojarzone z kontem. Są to dwie wersje profilu Developer — ten znajduje się na portalu dla deweloperów i innych znajduje się na lokalnych komputerach Mac. Różnica między nimi jest typem klucze zawierają: _profilu na portalu przechowuje wszystkie skojarzone z certyfikatami, podczas kopiowania na komputerze lokalnym Mac zawiera klucze prywatne klucze publiczne_. Dla certyfikatów identyfikator był prawidłowy pary kluczy muszą być zgodne. Na lokalny adres Mac, należy dysponować kopią zapasową profilu dewelopera, ponieważ w przypadku utraty kluczy prywatnych certyfikatów oraz profile inicjowania obsługi administracyjnej należy ponownie wygenerować.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Profil Developer zawiera certyfikaty, ich skojarzone klucze i żaden profil inicjowania obsługi administracyjnej skojarzone z kontem. Są to dwie wersje profilu Developer — ten znajduje się na portalu dla deweloperów i innych znajduje się na komputerach Mac. Różnica między nimi jest typem klucze zawierają: _profil w domach portalu wszystkich kluczy publicznych skojarzone z certyfikatami, podczas kopiowania na możesz jest Mac zawiera klucze prywatne_. Dla certyfikatów identyfikator był prawidłowy pary kluczy muszą być zgodne. Zachowaj kopii zapasowej profilu dewelopera z Mac Xamarin kompilacji hosta, ponieważ w przypadku utraty kluczy prywatnych certyfikatów oraz profile inicjowania obsługi administracyjnej należy ponownie wygenerować.

-----

> [!WARNING]
> **Uwaga:** utraty certyfikatu i skojarzonych kluczy mogą być bardzo dużo, trzeba będzie odwoływanie istniejących certyfikatów i ponowne inicjowanie obsługi administracyjnej wszystkie skojarzone urządzenia, włącznie z tymi zarejestrowany dla wdrożenia ad hoc. Po skonfigurowaniu pomyślnie programowanie certyfikaty, eksportowanie kopii zapasowej i zapisanie ich w bezpiecznym miejscu. Aby uzyskać więcej informacji, jak to zrobić, zapoznaj się z sekcją eksportowanie i importowanie certyfikatów oraz profile [obsługi certyfikatów](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/MaintainingCertificates/MaintainingCertificates.html) przewodnik w dokumentów firmy Apple.

<a name="provisioning" />

## <a name="provisioning-an-ios-device-for-development"></a>Inicjowanie obsługi administracyjnej systemu iOS urządzenia na potrzeby programowania

Po ustaleniu tożsamości z firmy Apple i mieć certyfikatu deweloperskiego należy skonfigurować profilu inicjowania obsługi administracyjnej i jednostkami, wymagane, można wdrożyć aplikację na urządzeniu z systemem Apple. Urządzenie musi być uruchomiona wersja systemu IOS, która jest obsługiwana przez Xcode — może być konieczne zaktualizować urządzenie, Xcode lub oba.

<a name="adddevice" />

## <a name="add-a-device"></a>Dodaj urządzenie

Podczas tworzenia profilu inicjowania obsługi administracyjnej dla rozwoju, możemy stanu urządzeń, które można uruchomić aplikacji. Aby je włączyć, można dodać maksymalnie 100 urządzeń na rok kalendarzowy do naszego portalu dla deweloperów, a w tym miejscu możemy wybierz urządzenia, które mają zostać dodane do określonego profilu inicjowania obsługi administracyjnej. Wykonaj poniższe kroki na komputerze Mac, aby dodać urządzenia do portalu dla deweloperów

1. Uruchom środowisko Xcode.
2. Podłącz urządzenie być przygotowana do komputera Mac za pomocą dostarczonego kabel USB.
2. Z **Windows** menu wybierz opcję **urządzeń**:

  [![](manual-provisioning-images/add01.png "Wybierz urządzenia, z menu systemu Windows")](manual-provisioning-images/add01.png#lightbox)

3. Wybierz urządzenie iOS żądaną z **urządzeń** listy po lewej stronie okna urządzenia.
4. Wyróżnij **identyfikator** ciągów i skopiuj go do Schowka:

  [![](manual-provisioning-images/add02.png "Wyróżnij ciąg identyfikatora")](manual-provisioning-images/add02.png#lightbox)

5. W programie Safari, przejdź do [Centrum deweloperów firmy Apple](https://developer.apple.com/membercenter/index.action) i zaloguj się.
6. Kliknij przycisk **certyfikaty, identyfikatory & profile** łącza:

  [![](manual-provisioning-images/add03.png "Kliknij pozycję certyfikaty, profile identyfikatory łącza")](manual-provisioning-images/add03.png#lightbox)

7. Polecenie **urządzeń** łącza:

  [![](manual-provisioning-images/add04.png "Kliknij łącze urządzeń")](manual-provisioning-images/add04.png#lightbox)

8. Kliknij przycisk  **+**  przycisk:

  [![](manual-provisioning-images/add05.png "Kliknij przycisk +")](manual-provisioning-images/add05.png#lightbox)

9. Podaj nazwę dla nowego urządzenia i Wklej urządzenia **identyfikator** możemy kopiowane powyżej w **UUID** pola:

  [![](manual-provisioning-images/add06.png "Podaj nazwę dla nowego urządzenia i identyfikator urządzenia")](manual-provisioning-images/add06.png#lightbox)

10. Kliknij przycisk **Kontynuuj** przycisku.
11. Ponadto, przejrzyj informacje i kliknij przycisk **zarejestrować** przycisk:

  [![](manual-provisioning-images/add07.png "Zapoznaj się z informacjami")](manual-provisioning-images/add07.png#lightbox)

Powtórz powyższe kroki dla każdego urządzenia z systemem iOS, które będzie służyć do testowania i debugowania aplikacji platformy Xamarin.iOS.

Po dodaniu urządzenia do portalu dla deweloperów, należy utworzyć profil inicjowania obsługi administracyjnej i Dodaj do niej urządzenia.


<a name="provisioningprofile" />

## <a name="creating-a-development-provisioning-profile"></a>Tworzenie rozwinięcie profil inicjowania obsługi administracyjnej

Zgodnie z certyfikatu deweloperskiego profile inicjowania obsługi można ręcznie utworzyć za pomocą [certyfikaty, identyfikatory & profile](https://developer.apple.com/account/overview.action) sekcji Centrum członków firmy Apple.

Przed utworzeniem profilu inicjowania obsługi administracyjnej, *identyfikator aplikacji* muszą być wprowadzane. Identyfikator aplikacji jest ciągiem styl wstecznego DNS, który unikatowo identyfikuje aplikację. Czynności spowoduje pokazują, jak utworzyć **identyfikator aplikacji symboli wieloznacznych**, które mogą służyć do tworzenia i zainstalowania większości aplikacji. **Jawne identyfikatorów aplikacji** tylko Pozwól na instalację jednej aplikacji (z pasujących identyfikator pakietu) i są zazwyczaj używane do niektórych funkcji systemu iOS, takie jak Apple Pay i HealthKit. Informacje dotyczące tworzenia jawne identyfikatorów aplikacji, zapoznaj się [Praca z funkcjami](~/ios/deploy-test/provisioning/capabilities/index.md) przewodnik.

### <a name="app-id"></a>Identyfikator aplikacji

1. W [portalu dla deweloperów](https://developer.apple.com/account/overview.action) przejdź do *certyfikatu, identyfikatory i profile* sekcji w Centrum deweloperów firmy Apple. Wybierz **identyfikatorów aplikacji** w obszarze **identyfikatory**.
2. Kliknij przycisk  **+**  przycisk i podaj **nazwa**:

    [![](manual-provisioning-images/appid05a.png "Podaj nazwę")](manual-provisioning-images/appid05a.png#lightbox)
3. Prefiks aplikacji należy wstępnie. Wybierz **identyfikator aplikacji symboli wieloznacznych** sufiksu aplikacji. Wprowadź identyfikator pakietu w formacie `com.[DomainName].*`:

  [![](manual-provisioning-images/appid05b.png "Wprowadź identyfikator pakietu")](manual-provisioning-images/appid05b.png#lightbox)

3. Kliknij przycisk **Kontynuuj** przycisk i zgodnie z instrukcjami wyświetlanymi na ekranie można utworzyć nowego identyfikatora aplikacji.

### <a name="provisioning-profile"></a>Profil inicjowania obsługi administracyjnej

Po utworzeniu Identyfikatora aplikacji można wyprodukować profilu inicjowania obsługi administracyjnej. Ten profil inicjowania obsługi administracyjnej zawiera informacje na *co* aplikacji (lub aplikacji, jeśli jest identyfikator aplikacji symboli wieloznacznych) dotyczy tego profilu, *kto* mogą używać profilu (w zależności od tego, jakie developer certyfikaty są dodawane), i *co* urządzeń mogą zainstalować aplikację.

Aby ręcznie utworzyć profil inicjowania obsługi administracyjnej do tworzenia aplikacji, wykonaj następujące czynności:

1. Użyj Safari, aby przejść do [Member Center przeznaczonej dla deweloperów firmy Apple](https://developer.apple.com/membercenter/index.action)i w sekcji *certyfikaty, identyfikatory & profile* Wybierz profile inicjowania obsługi.
2. Kliknij przycisk  **+**  przycisk, w prawym górnym rogu, aby utworzyć nowy profil.
3. Z **programowanie** wybierz przycisk radiowy obok **tworzenie aplikacji dla systemu iOS**i naciśnij klawisz **Kontynuuj**:

    [![](manual-provisioning-images/provisioning-profile01.png "Wybierz typ profilu")](manual-provisioning-images/provisioning-profile01.png#lightbox)
4. Z menu rozwijanego wybierz aplikację IDENTYFIKATORA, który umożliwia:

    [![](manual-provisioning-images/provisioning-profile02.png "Wybierz aplikację IDENTYFIKATORA, który umożliwia")](manual-provisioning-images/provisioning-profile02.png#lightbox)
5. Wybierz certyfikaty do uwzględnienia w profilu inicjowania obsługi administracyjnej, a następnie naciśnij klawisz **Kontynuuj**:

    [![](manual-provisioning-images/provisioning-profile03.png "Wybierz certyfikaty, które mają być uwzględnione w profilu inicjowania obsługi administracyjnej")](manual-provisioning-images/provisioning-profile03.png#lightbox)
6. Wybierz wszystkie urządzenia, których aplikacja zostanie zainstalowana na.

    [![](manual-provisioning-images/provisioning-profile04.png "Wybierz urządzenia, które aplikacja zostanie zainstalowana na")](manual-provisioning-images/provisioning-profile04.png#lightbox)
7. Udostępnianie profilu inicjowania obsługi administracyjnej zidentyfikować nazwę i naciśnij klawisz **Kontynuuj** Aby utworzyć profil:

    [![](manual-provisioning-images/provisioning-profile05.png "Podaj nazwę profilu inicjowania obsługi administracyjnej z osobowe")](manual-provisioning-images/provisioning-profile05.png#lightbox)
8. Naciśnij klawisz **Pobierz** można pobrać profilu inicjowania obsługi administracyjnej na komputerze Mac:

    [![](manual-provisioning-images/provisioning-profile06.png "Pobierz profil inicjowania obsługi administracyjnej")](manual-provisioning-images/provisioning-profile06.png#lightbox)
9. Kliknij dwukrotnie plik, aby zainstalować profil inicjowania obsługi administracyjnej w środowisku Xcode. Należy pamiętać, że się, że wszystkie visual clues został on zainstalowany profil z wyjątkiem otwierania Xcode nie mogą być wyświetlane. Można to zweryfikować przechodząc do **Xcode > Preferencje > kont**. Wybierz identyfikator Apple ID, a następnie kliknij przycisk **Wyświetl szczegóły...** . Powinien być wyświetlany nowy profil inicjowania obsługi administracyjnej, jak przedstawiono poniżej:

      [![](manual-provisioning-images/provisioning-profile07.png "Wyświetlanie profilu w środowisku Xcode")](manual-provisioning-images/provisioning-profile07.png#lightbox)

Po pomyślnym utworzeniu profilu inicjowania obsługi administracyjnej może być konieczne odświeżyć Xcode, dzięki czemu wszystkie certyfikaty programowanie są dostępne dla programu Visual Studio for Mac i Visual Studio.

<a name="download" />

## <a name="downloading-profiles-and-certificates-in-xcode"></a>Pobieranie profilów i certyfikatów w programie Xcode

Certyfikaty i inicjowania obsługi administracyjnej profilów, które zostały utworzone w portalu dla deweloperów firmy Apple, mogą nie pojawiać się automatycznie w programie Xcode. W związku z tym może być konieczne, aby je pobrać one, czy są one dostępne przez program Visual Studio for Mac i Visual Studio. Zaktualizowanie i pobranie wszelkich certyfikatów utworzone w portalu dla deweloperów firmy Apple, wykonaj następujące czynności:

1.   Zamknij program Visual Studio dla komputerów Mac lub Visual Studio.
2.   Uruchom środowisko Xcode.
3.   Wybierz **Xcode Menu > Preferencje...**
4.   Kliknij przycisk **kont** kartę.
5.   Wybierz zespół, a następnie kliknij przycisk **Pobierz Podręcznik profile** przycisku: [ ![ ] (manual-provisioning-images/selectteam1.png "profile ręcznego pobierania")](manual-provisioning-images/selectteam1.png#lightbox)

6.   Zamknij Xcode.
7.  Uruchom program Visual Studio dla komputerów Mac lub Visual Studio.

Nowe certyfikaty lub profile inicjowania obsługi administracyjnej jest dostępne w programie Visual Studio dla komputerów Mac lub Visual Studio i gotowe do użycia.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

> [!IMPORTANT]
> **Uwaga:** może być konieczne do zatrzymywania i ponownego uruchomienia programu Visual Studio for Mac zobaczą żadnych nowych lub zmodyfikowanych certyfikatów ani profile zaktualizowany przez Xcode.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

> [!IMPORTANT]
> **Uwaga:** może być konieczne zatrzymać i uruchomić ponownie program Visual Studio przed zobaczą żadnych certyfikatów nowe lub zmodyfikowane ani zaktualizowany w środowisku Xcode profilów.

-----

<a name="appservices" />

## <a name="provisioning-for-application-services"></a>Inicjowanie obsługi administracyjnej dla usług aplikacji

Apple zapewnia szereg specjalnych usług aplikacji, nazywane również możliwości, które mogą być uaktywniony na potrzeby aplikacji platformy Xamarin.iOS. Te usługi aplikacji muszą być skonfigurowane na iOS portalu inicjowania obsługi podczas **identyfikator aplikacji** jest tworzony w **Entitlements.plist** pliku część projekt aplikacji platformy Xamarin.iOS. Informacje na temat dodawania do aplikacji usługi aplikacji, zapoznaj się [wprowadzenie do funkcji](~/ios/deploy-test/provisioning/capabilities/index.md) przewodnik i [Praca z uprawnieniami](~/ios/deploy-test/provisioning/entitlements.md) przewodnik.

* Utwórz identyfikator aplikacji z usługami wymagana aplikacja.
* Utwórz nową [profil inicjowania obsługi administracyjnej](#provisioningprofile) zawierający ten identyfikator aplikacji.
* Ustawianie uprawnień w projekcie platformy Xamarin.iOS

<a name="deploy" />

## <a name="deploying-to-a-device"></a>Wdrażanie do urządzenia

W tym momencie inicjowania obsługi administracyjnej powinny zostać ukończone, a aplikacja jest gotowa do wdrożenia na urządzeniu. Aby to zrobić, wykonaj następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Upewnij się, że wartość None, w obszarze Opcje projektu selektora zespołu > iOS podpisywania pakietu.

1. Podłącz urządzenie do komputerów Mac.
2. W projekcie **Info.plist**, upewnij się, że identyfikator pakietu jest zgodny z Identyfikatorem aplikacji (chyba że identyfikator aplikacji jest symbol wieloznaczny):

  ![](manual-provisioning-images/deploydevice01xs.png "Wprowadzanie identyfikatora")

3. Kliknij prawym przyciskiem myszy na projekt, aby wyświetlić okno dialogowe Opcje projektu, a następnie przejdź do **kompilacji > iOS podpisywania pakietu**. Z listy rozwijanej obok zarówno **tożsamość podpisywania** i **profilu inicjowania obsługi administracyjnej**, sprawdź, czy można Zobacz poprawne Profile programu Visual Studio for Mac i wybierz określoną tożsamością & profilu:

  ![](manual-provisioning-images/deploydevice02xs.png "Wybierz określoną tożsamością & profilu")

Jeśli ta wartość jest równa **automatyczne**, Visual Studio dla komputerów Mac będzie Wybierz tożsamość i profil na podstawie Identyfikatora pakietu, który został ustawiony w kroku #2.

4. Upewnij się ustawić konfigurację kompilacji **iPhone** / **iPad**, a nie w symulatorze.
5. Kliknij przycisk **Uruchom** w programie Visual Studio dla komputerów Mac i wyświetlić aplikacji uruchomionej na urządzeniu.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Podłącz urządzenie do komputerów Mac.
2. W projekcie **Info.plist**, upewnij się, że identyfikator pakietu jest zgodny z Identyfikatorem aplikacji:

  ![](manual-provisioning-images/servicevs01.png "Wprowadzanie identyfikatora")

3. Kliknij prawym przyciskiem myszy na projekt, aby wyświetlić okno dialogowe Opcje projektu i przejdź do **kompilacji > iOS podpisywania pakietu**. Z listy rozwijanej obok zarówno **tożsamość podpisywania** i **profilu inicjowania obsługi administracyjnej** Sprawdź, czy program Visual Studio może zobacz poprawne profile i wybierz określoną tożsamością & profilu.

    Jeśli ta wartość jest równa **automatyczne**, Visual Studio wybierze tożsamości i profil na podstawie Identyfikatora pakietu, który został ustawiony w kroku #2.

4. Upewnij się ustawić konfigurację kompilacji **iPhone** lub **iPad**, a nie w symulatorze.
5. Kliknij przycisk **Uruchom** w programie Visual Studio i wyświetlić aplikacji uruchomionej na urządzeniu.


-----

## <a name="summary"></a>Podsumowanie

W tym przewodniku opisano kroki wymagane do konfiguracji środowiska programowania dla platformy Xamarin.iOS. Przedstawione go, jak aplikacja jest kod jest podpisywany przy informacje dotyczące dewelopera, zespołów, urządzenia z systemem przez aplikację na i identyfikator poszczególnych aplikacji.


## <a name="related-links"></a>Linki pokrewne

- [Bezpłatna aprowizacja](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [Dystrybucja aplikacji](~/ios/deploy-test/app-distribution/index.md)
- [Rozwiązywanie problemów](~/ios/deploy-test/troubleshooting.md)
- [Apple — podręczniku dystrybucji aplikacji](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
