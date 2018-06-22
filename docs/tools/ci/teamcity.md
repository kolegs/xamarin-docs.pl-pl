---
title: Przy użyciu Miasto zespołu za pomocą platformy Xamarin
description: W tym przewodniku będzie omawiać kroki związane z używaniem TeamCity do skompilowania aplikacji dla urządzeń przenośnych i przesłać je do chmury testowej Xamarin.
ms.prod: xamarin
ms.assetid: AC2626CB-28A7-4808-B2A9-789D67899546
author: topgenorth
ms.author: toopge
ms.date: 03/23/2017
ms.openlocfilehash: 32338bc89df2ef7ee4426482b1967861f0c0e058
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
ms.locfileid: "33921953"
---
# <a name="using-team-city-with-xamarin"></a>Przy użyciu Miasto zespołu za pomocą platformy Xamarin

_W tym przewodniku będzie omawiać kroki związane z używaniem TeamCity do skompilowania aplikacji dla urządzeń przenośnych i przesłać je do chmury testowej Xamarin._

Zgodnie z opisem w [wprowadzenie do ciągłej integracji](~/tools/ci/intro-to-ci.md) przewodnik, ciągłej integracji (CI) jest rozwiązaniem przydatne podczas opracowywania aplikacji dla urządzeń przenośnych jakości. Istnieje wiele opcji działało dla ciągłej integracji serwera oprogramowania; Ten przewodnik koncentruje się na [TeamCity](http://www.jetbrains.com/teamcity/) z JetBrains.

Istnieje kilka różnych kombinacji TeamCity instalacji. Poniżej przedstawiono listę niektórych z tych:

- **Usługa systemu Windows** — w tym scenariuszu TeamCity rozpoczyna się po rozruchu systemu Windows jako usługi systemu Windows. Muszą być skojarzone z hostem kompilacji Mac do skompilowania aplikacji dla systemu iOS.

- **Uruchamianie demon na OS X** — koncepcyjnie, jest bardzo podobny do uruchamiania jako usługa systemu Windows, opisane w poprzednim kroku. Domyślnie kompilacji zostanie uruchomiony w ramach konta głównego.

- **Konto użytkownika na OS X** — można uruchomić TeamCity przy użyciu konta użytkownika, która rozpoczyna się podczas logowania się użytkownika.

Poprzednie scenariuszy działania TeamCity przy użyciu konta użytkownika na OS X jest najprostsza i najłatwiejszy w Instalatorze.

Istnieje kilka czynności niezbędnych do konfigurowania TeamCity:

- **Instalowanie TeamCity** — instalacja TeamCity nie jest uwzględnione w tym przewodniku. W tym przewodniku założono, że TeamCity jest zainstalowana i uruchomiona na koncie użytkownika. Instrukcje dotyczące [instalowanie TeamCity](http://confluence.jetbrains.com/display/TCD8/Installation) znajdują się w [dokumentacji TeamCity 8](http://confluence.jetbrains.com/display/TCD8/TeamCity+Documentation) przez JetBrains.

- **Przygotowywanie serwera kompilacji** — ten krok obejmuje, zainstalowanie oprogramowanie niezbędne narzędzia i certyfikaty wymagane do tworzenia aplikacji dla urządzeń przenośnych i przygotować je do dystrybucji.

- **Tworzenie skryptu kompilacji A** — ten krok nie jest to niezbędne, ale skrypt kompilacji jest przydatne w pomocy do tworzenia aplikacji instalacji nienadzorowanej. Za pomocą skryptu kompilacji pomoże rozwiązywania problemów kompilacji, które mogą wystąpić podczas i zapewnia spójne i powtarzalnej sposób tworzenia plików binarnych do dystrybucji, nawet wtedy, gdy ciągła integracja nie jest stosowane są.

- **Tworzenie projektu TeamCity A** — po wykonaniu poprzednie trzy kroki utworzyć projekt TeamCity, który będzie zawierać wszystkie dane meta pobrać kodu źródłowego, Kompiluj projekty i przesłać testów do chmury testowej Xamarin.

## <a name="requirements"></a>Wymagania

Obsługa przy użyciu z [chmury testowej Xamarin](https://developer.xamarin.com/guides/testcloud) jest wymagana.

Wymagana jest znajomość TeamCity 8.1. Instalacja TeamCity wykracza poza zakres tego dokumentu. Zakłada się, że TeamCity jest zainstalowany na OS X Mavericks i działa w ramach konta zwykłych użytkowników, a nie konta głównego.

Serwer kompilacji powinien być komputerem autonomicznym systemem OS X, mający na celu ciągłej integracji. W idealnym przypadku serwer kompilacji nie będzie odpowiedzialny za innych ról, takich jak serwer bazy danych, serwer sieci web lub stacji roboczej developer.

> [!IMPORTANT]
> Ten przewodnik nie obejmuje "bezobsługowe" Instalacja programu Xamarin.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="preparing-the-build-server"></a>Przygotowywanie serwera kompilacji

Jest to kluczowy etap w konfigurowaniu serwera kompilacji można zainstalować wszystkie niezbędne narzędzia, oprogramowania i certyfikatów do tworzenia aplikacji dla urządzeń przenośnych. Należy pamiętać, że serwer kompilacji można skompilować rozwiązania mobilne i uruchom wszystkie testy. Aby zminimalizować problemy z konfiguracją, oprogramowania i narzędzia należy zainstalować na tym samym koncie użytkownika, który jest hostem TeamCity. Poniżej przedstawiono listę co jest wymagane:

1. **Visual Studio for Mac** — dotyczy to również Xamarin.iOS i platformy Xamarin.Android.
2. **Logowanie w magazynie składników Xamarin** — jest to krok opcjonalny, tylko wymagane, jeśli aplikacja używa składników w magazynie składników Xamarin. Aktywne rejestrowanie w magazynie składnika w tym momencie uniemożliwi problemy podczas kompilacji TeamCity próbuje Kompilowanie aplikacji.
3. **Xcode** — Xcode jest wymagany do kompilacji i logowania aplikacji systemu iOS.
4. **Narzędzia wiersza polecenia Xcode** — to jest opisany w kroku 1 sekcji instalacji [aktualizowanie Ruby z rbenv](https://developer.xamarin.com/guides/testcloud/calabash/updating-ruby-using-rbenv/) przewodnik.
5. **Podpisywanie tożsamości i profile inicjowania obsługi** — zaimportować certyfikaty i inicjowania obsługi profilu za pośrednictwem XCode. Zobacz Przewodnik firmy Apple [eksportowanie tożsamości podpisywania i profile inicjowania obsługi](https://developer.apple.com/library/ios/recipes/xcode_help-accounts_preferences/articles/export_signing_assets.html) więcej szczegółów.
6. **Android Keystores** — kopiowanie wymagane keystores systemu Android do czy TeamCity użytkownik ma dostęp, tzn. katalogu `~/Documents/keystores/MyAndroidApp1`.
7. **Calabash** — jest to krok opcjonalny, jeśli aplikacja wygenerowała testów napisane przy użyciu Calabash. Zobacz [instalowanie Calabash na OS X Mavericks](https://developer.xamarin.com/guides/testcloud/calabash/osx-installation/) przewodnik i [aktualizowanie Ruby z rbenv](https://developer.xamarin.com/guides/testcloud/calabash/updating-ruby-using-rbenv/) przewodnika, aby uzyskać więcej informacji.

Na poniższym diagramie przedstawiono wszystkie te składniki:

![](teamcity-images/image1.png "Ten diagram przedstawia wszystkich tych składników")

Po zainstalowaniu wszystkich programów, zaloguj się do konta użytkownika i upewnij się, że całe oprogramowanie został poprawnie zainstalowany i działa. Powinno to obejmować kompilowania rozwiązania i przesyłanie aplikacji do chmury testowej. To może znacznie uprościć przez uruchomienie skryptu kompilacji zgodnie z opisem w następnej sekcji.

## <a name="create-a-build-script"></a>Utwórz skrypt w kompilacji

Nawet TeamCity do obsługi wszystkich aspektów kompilowania i przesyłanie aplikacji dla urządzeń przenośnych do chmury testowej samodzielnie, ale zdecydowanie zaleca się Tworzenie skryptu kompilacji. Skrypt kompilacji zapewnia następujące korzyści:

1. **Dokumentacja** — skryptu kompilacji służy jako formę dokumentacji w jaki sposób oprogramowanie jest tworzone. Spowoduje to usunięcie części "Magia" jest skojarzony z wdrożeniem aplikacji, który umożliwia deweloperom skoncentrować się na funkcjonalność.
1. **Powtarzalność** — skryptu kompilacji gwarantuje, że aplikacja jest skompilowana i wdrożona, zawsze zdarza się to w taki sam sposób, niezależnie od tego, kto lub co działa. Ta powtarzalne spójności usuwa wszelkie problemy lub błędy, które mogą pełzanie w ze względu na nieprawidłowo wykonane kompilacji lub błędów ludzkich.
1. **Przechowywanie wersji** — skryptu kompilacji można dołączyć do systemu kontroli źródła. Oznacza to, że zmiany w skrypcie kompilacji śledzone, monitorowania i poprawione w przypadku znalezienia błędów lub nieprawidłowości.
1. **Przygotowanie środowiska** — skryptu kompilacji może zawierać logiki, aby zainstalować wymagane zależności firm 3. Daje to pewność, że aplikacje są tworzone odpowiednie składniki.

Skrypt kompilacji można tak proste, jak plik programu Powershell (w systemie Windows) lub bash skryptu (na OS X). Podczas tworzenia skryptu kompilacji, istnieje kilka opcji dla języków skryptów:

- [**Nachylenia** ](https://github.com/jimweirich/rake) — jest to określonego języka domeny (DSL) do tworzenia projektów, opartych na języku Ruby. Nachylenia ma tę zaletę popularność i rozbudowany ekosystem bibliotek.

- [**psake** ](https://github.com/psake/psake) — jest to biblioteki programu Windows Powershell do tworzenia oprogramowania

- [**SFAŁSZOWAĆ** ](http://fsharp.github.io/FAKE/) — jest to DSL, na podstawie języka F # co umożliwia wykorzystanie istniejącej bibliotek .NET, w razie potrzeby.

Jaki język skryptów zależy preferencji i wymagania. [TaskyPro Calabash](https://github.com/xamarin/test-cloud-samples/tree/master/TaskyPro/TaskyPro-Calabash) przykład zawiera przykład użycia nachylenia jako [kompilacji skryptu](https://github.com/xamarin/test-cloud-samples/blob/master/TaskyPro/TaskyPro-Calabash/Rakefile).

> [!NOTE]
> Istnieje możliwość użycia systemu kompilacji oparte na języku XML, na przykład programu MSBuild lub NAnt, ale brakuje wyrazistość i łatwości konserwacji DSL, służącego do tworzenia oprogramowania.

### <a name="parameterizing-the-build-script"></a>Ustawianie skryptu kompilacji

Proces tworzenia i testowania oprogramowania wymaga informacji, które powinny być przechowywane poufne. W szczególności tworzenie APK może wymagać hasła dla magazynu kluczy i/lub aliasu klucza w magazynie kluczy. Podobnie chmury testowej wymaga klucz interfejsu API, który jest unikatowy dla deweloperów. Typy wartości nie powinna być twarde kodowanych w skrypcie kompilacji. Zamiast tego powinny zostać przekazane jako zmienne do skryptu kompilacji.

Mniej wrażliwych to wartości, takie jak działa z systemem iOS identyfikator urządzenia lub identyfikator urządzenia z systemem Android, które zidentyfikować urządzenia, które chmury testowania należy używać do testowania. Nie są one wartości, które muszą być chronione, ale mogą one ulec zmianie od kompilacji do kompilacji.

Przechowywanie tych typów zmiennych poza skryptu kompilacji również ułatwia udostępnianie skryptu kompilacji w organizacji, na przykład z deweloperami. Deweloperzy mogą użyć dokładnie tego samego skryptu jako serwera kompilacji, ale można użyć własnych keystores i klucze interfejsu API.

Dostępne są dwie opcje możliwe do przechowywania tych wartości poufnych:

- **Plik konfiguracji** — do ochrony klucza interfejsu API chmury testu, ta wartość nie powinna być sprawdzana do kontroli wersji. Plik można tworzyć dla każdego komputera. Jak wartości są odczytywane z tego pliku zależy od język skryptów.

- **Zmienne środowiskowe** — te opcje można łatwo ustawić na poszczególnych komputerach i ared niezależnie od podstawowy język skryptów.

Brak zalety i wady każdej z tych opcji. TeamCity działa dobrze zmiennych środowiskowych, w tym przewodniku zaleci ta technika, podczas tworzenia skryptów kompilacji.

### <a name="build-steps"></a>Kroki procesu kompilacji

Skrypt kompilacji musi mieć możliwość wykonaj następujące czynności:

- **Kompilowanie aplikacji** — dotyczy to również podpisywania aplikacji z poprawnego profilu inicjowania obsługi administracyjnej.

- **Złożyć wniosek do chmury testowej Xamarin** — dotyczy to również podpisywania i zip wyrównywanie APK z odpowiedniego magazynu kluczy.

Następujące dwa kroki opisano szczegółowo poniżej.

#### <a name="compiling-a-xamarinios-application"></a>Kompilowanie aplikacji platformy Xamarin.iOS

[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

#### <a name="compiling-a-xamarinandroid-application"></a>Kompilowanie aplikacji platformy Xamarin.Android

Aby skompilować aplikację systemu Android, należy użyć **xbuild** (lub **msbuild** w systemie Windows):

```bash
/Library/Frameworks/Mono.framework/Commands/xbuild /t:SignAndroidPackage /p:Configuration=Release /path/to/android.csproj
```

Należy zauważyć, że do skompilowania aplikacji dla systemu Xamarin Android **xbuild** używa projektu, a do tworzenia aplikacji dla systemu iOS **xbuild** wymaga rozwiązania.

#### <a name="submitting-xamarinuitests-to-test-cloud"></a>Przesyłanie Xamarin.UITests do chmury testu

UITests są przesyłane przy użyciu `test-cloud.exe` aplikacji, jak pokazano w poniższy fragment kodu:

```bash
test-cloud.exe <path-to-apk-or-ipa-file> <test-cloud-team-api-key> --devices <device-selection-id> --assembly-dir <path-to-tests-containing-test-assemblies> --nunit-xml report.xml --user <email>
```

Po uruchomieniu testu, zostaną zwrócone wyniki testu w formie pliku XML NUnit styl o nazwie **report.xml**. TeamCity wyświetli informacje w dzienniku kompilacji.

Aby uzyskać więcej informacji o sposobie zgłaszania UITests do chmury testu, sprawdź w przewodniku dla platformy Xamarin w [przesyłanie testów do chmury testowej](https://developer.xamarin.com/guides/testcloud/uitest/working-with/submitting-tests-to-xamarin-test-cloud/).

#### <a name="submitting-calabash-tests-to-test-cloud"></a>Przesyłanie testów Calabash chmury

Testy Calabash są przesyłane przy użyciu `test-cloud` gem, jak pokazano w poniższy fragment kodu:

```bash
test-cloud submit /path/to/APK-or-IPA <test-cloud-team-api-key> --devices <device-id> --user <email>
```
Aby przesłać aplikacji systemu Android do testowania chmury, konieczne jest najpierw Odbuduj serwer testu APK przy użyciu calabash android:

```bash
$ calabash-android build </path/to/signed/APK>
$ test-cloud submit /path/to/APK <test-cloud-team-api-key> --devices <ANDROID_DEVICE_ID> --profile=android --config=config/cucumber.yml --pretty
```
Aby uzyskać więcej informacji na przesyłanie Calabash testy, należy zapoznać się na platformie Xamarin guide na [przesyłanie Calabash testy do chmury testowej](https://developer.xamarin.com/guides/testcloud/calabash/working-with/submitting-tests-to-xamarin-test-cloud/).

## <a name="creating-a-teamcity-project"></a>Tworzenie projektu TeamCity

Po zainstalowaniu TeamCity i programu Visual Studio for Mac można skompilować projekt, nadszedł czas na utworzenie projektu w TeamCity, aby skompilować projekt i przesłać je do chmury testowej.

1. Rozpoczęto, logując się do TeamCity za pośrednictwem przeglądarki sieci web. Przejdź do katalogu głównego projektu:

    ![Przejdź do katalogu głównego projektu](teamcity-images/image2.png "przejdź do katalogu głównego projektu") poniżej główny projekt, Utwórz nowy projekt podrzędne:

    ![Przejdź do katalogu głównego projektu Underneath głównego projektu, Utwórz nowy projekt podrzędne](teamcity-images/image3.png "przejdź do katalogu głównego projektu Underneath głównego projektu Utwórz nowy projekt podrzędne")
2. Po utworzeniu projektu podrzędnego, Dodaj nową konfigurację kompilacji:

    ![Po utworzeniu projektu podrzędnego, dodać nową konfigurację kompilacji](teamcity-images/image5.png "po utworzeniu projektu podrzędnego, dodać nową konfigurację kompilacji")
3. Dołączanie projektu VC dla konfiguracji kompilacji. Można to zrobić za pośrednictwem ekranu ustawienia kontroli wersji:

    ![Odbywa się za pośrednictwem ekranu ustawienia kontroli wersji](teamcity-images/image6.png "odbywa się za pośrednictwem ekranu ustawienia kontroli wersji")

    Jeśli nie nie utworzonego projektu VC, masz opcję, aby utworzyć nowy element główny VC strony pokazano poniżej:

    ![Jeśli nie nie utworzonego projektu VC, masz opcję, aby utworzyć nowy element główny VC strony](teamcity-images/image7.png "Jeśli nie nie utworzonego projektu VC, masz opcję, aby utworzyć nowy element główny VC strony")

    Po dołączeniu głównego VC wyewidencjonowania spowoduje TeamCity projektu i spróbuj automatycznie wykryć kroki procesu kompilacji. Jeśli znasz TeamCity, następnie należy wybrać jeden z kroków kompilacji wykryte. Jest bezpiecznie zignorować kroków kompilacji wykryto teraz.

4. Skonfiguruj wyzwalacz kompilacji. Spowoduje to kolejki kompilację, gdy zostaną spełnione określone warunki, na przykład gdy użytkownik zatwierdza kod do repozytorium. Poniższy zrzut ekranu przedstawia sposób dodawania wyzwalacz kompilacji:

    ![Ten zrzut ekranu pokazuje, jak dodać wyzwalacza kompilacji](teamcity-images/image8.png "tego zrzutu ekranu pokazuje, jak dodać wyzwalacza kompilacji") przykład konfigurowania wyzwalacz kompilacji są widoczne na poniższym zrzucie ekranu:

    ![Przykład konfigurowania wyzwalacz kompilacji są widoczne w tym zrzucie ekranu pokazano](teamcity-images/image9.png "przykład konfigurowania wyzwalacz kompilacji są widoczne w tym zrzut ekranu")

5. Poprzedniej sekcji, ustawianie skryptu kompilacji sugerowane przechowywania niektórych wartości jako zmienne środowiskowe. Te zmienne można dodawać do konfiguracji kompilacji za pomocą parametrów ekranu. Dodaj zmienne dla testu klucz interfejsu API w chmurze, identyfikator urządzenia z systemem iOS i Android identyfikator urządzenia, jak pokazano na poniższym zrzucie ekranu:

    ![Dodawanie zmienne dla testu klucz interfejsu API w chmurze, identyfikator urządzenia z systemem iOS i Android identyfikator urządzenia](teamcity-images/image11.png "dodać zmienne dla testu klucz interfejsu API w chmurze, identyfikator urządzenia z systemem iOS i Android identyfikator urządzenia")

6. Ostatnim krokiem jest dodać kroku kompilacji, która wywoła skryptu kompilacji do skompilowania aplikacji i umieścić w kolejce aplikacji w chmurze testu. Poniższy zrzut ekranu jest przykładem krok kompilacji, który używa Rakefile do utworzenia aplikacji:

    ![Ten zrzut ekranu jest przykładem kroku kompilacji, używaną do tworzenia aplikacji Rakefile](teamcity-images/image12.png "tego zrzutu ekranu znajduje się przykład krok kompilacji, który używa Rakefile do tworzenia aplikacji")

7. W tym momencie konfiguracja kompilacji zostanie zakończona. Należy dobrze, aby wyzwolić kompilację, aby upewnić się, że projekt jest skonfigurowany prawidłowo. Dobrym sposobem, w tym celu jest można przekazać małe, nieznaczne zmiany do repozytorium. TeamCity powinna wykryć zatwierdzenia i uruchomić kompilację.

8. Po ukończeniu kompilacji, sprawdź w dzienniku kompilacji i czy istnieją jakiekolwiek problemy lub ostrzeżenia o kompilacji, które wymagają uwagi.

## <a name="summary"></a>Podsumowanie

W tym przewodniku opisano sposób użycia TeamCity do tworzenia aplikacji mobilnych Xamarin i przesłać je do chmury testowej. Omówiono tworzenie skryptu kompilacji do automatyzacji procesu kompilacji. Skrypt kompilacji zajmuje się skompilowania aplikacji, przesyłanie do chmury testu i Oczekiwanie na wyniki

Następnie możemy opisano sposób tworzenia projektu w TeamCity kolejki kompilację zawsze dewelopera zatwierdza kodu, który wywoła skryptu kompilacji.

## <a name="related-links"></a>Linki pokrewne

- [Przesyłanie testów do chmury testowej Xamarin (UITest)](https://developer.xamarin.com~/testcloud/uitest/working-with/submitting-tests-to-xamarin-test-cloud/)
- [Przesyłanie testów do chmury testowej Xamarin (Calabash)](https://developer.xamarin.com~/testcloud/calabash/working-with/submitting-tests-to-xamarin-test-cloud/)
- [Instalowanie i konfigurowanie TeamCity](http://confluence.jetbrains.com/display/TCD8/Installing+and+Configuring+the+TeamCity+Server)
