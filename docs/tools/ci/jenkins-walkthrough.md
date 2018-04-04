---
title: Przy użyciu Wpięć za pomocą platformy Xamarin
description: W tym przewodniku przedstawiono sposób ustawiania Wpięć jako serwer ciągłej integracji i automatyzacji Kompilowanie aplikacji utworzony za pomocą platformy Xamarin dla urządzeń przenośnych. Przedstawiono sposób instalowania Wpięć na OS X, jest skonfigurowana i skonfigurować zadania do skompilowania aplikacji platformy Xamarin.iOS i Xamarin.Android podczas zmiany zostały zastosowane systemu zarządzania kodu źródłowego.
ms.prod: xamarin
ms.assetid: 1E6825DF-1254-4FCB-B94D-ADD33D1B5309
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/23/2017
ms.openlocfilehash: f183eb487b49d60c896bef9c90c711cd3da846b7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="using-jenkins-with-xamarin"></a>Przy użyciu Wpięć za pomocą platformy Xamarin

_W tym przewodniku przedstawiono sposób ustawiania Wpięć jako serwer ciągłej integracji i automatyzacji Kompilowanie aplikacji utworzony za pomocą platformy Xamarin dla urządzeń przenośnych. Przedstawiono sposób instalowania Wpięć na OS X, jest skonfigurowana i skonfigurować zadania do skompilowania aplikacji platformy Xamarin.iOS i Xamarin.Android podczas zmiany zostały zastosowane systemu zarządzania kodu źródłowego._

[Wprowadzenie do ciągłej integracji z platformą Xamarin](~/tools/ci/intro-to-ci.md) wprowadza ciągłej integracji rozwiązaniem rozwoju oprogramowania przydatne, która zapewnia wczesne ostrzeganie kodu uszkodzona lub niezgodna. CI umożliwia deweloperom problemów i rozwiązania problemów w miarę ich powstawania i śledzi w odpowiedniej stanie dla wdrożenia oprogramowania. W tym przewodniku opisano sposób korzystania z zawartości z oba dokumenty razem.

W tym przewodniku pokazano, jak zainstalować Wpięć na dedykowanym komputerze z systemem OS X i jest skonfigurowana do automatycznego uruchamiania podczas uruchamiania komputera. Po zainstalowaniu Wpięć zostanie zainstalowany dodatkowe wtyczki do obsługi MS Build. Wpięć obsługuje Git bez. Jeśli TFS jest używana do kontroli kodu źródłowego, należy także zainstalować dodatkowe narzędzia wtyczki i wiersza polecenia.

Po skonfigurowaniu Wpięć i zainstalowane niezbędne się jakieś wtyczki, zostanie utworzony co najmniej jedno zadanie do kompilacji projektów platformy Xamarin.Android i Xamarin.iOS. Zadanie jest kolekcją kroki i metadane wymagane do wykonania dodatkowych czynności. Zadania zazwyczaj składa się z następujących czynności:

-  **Źródła kodu zarządzania (SCM)** — jest to wpis metadanych w plikach konfiguracji Wpięć, który zawiera informacje na temat nawiązywania połączenia z kontrolą kodu źródłowego i co pliki do pobrania.
-  **Wyzwalacze** — wyzwalacze są używane do uruchamiania zadania opartego na pewne akcje, takie jak kiedy dewelopera zatwierdza zmiany do repozytorium kodu źródłowego.
-  **Instrukcje tworzenia** — to jest wtyczką lub skrypt, który będzie kompilowanie kodu źródłowego i tworzy binarny, który może zostać zainstalowany na urządzeniach przenośnych.
-  **Opcjonalne akcje kompilacji** — mogą one obejmować przeprowadzanie testów jednostkowych, wykonywania analizy statycznej na podpisywania kodu, kodu lub uruchomienie innego zadania do wykonywania innych kompilacji powiązane pracy.
-  **Powiadomienia** — zadanie może wysłać powiadomienia o stanie kompilacji określonego rodzaju.
-  **Zabezpieczenia** — mimo że jest to opcjonalne, zdecydowanie zaleca się, włączenia funkcji zabezpieczeń Wpięć.


Ten przewodnik przeprowadzi sposób konfigurowania serwera Wpięć obejmujące każdego z tych punktów. Do końca go firma Microsoft ma dobrą znajomością działania do instalacji i konfiguracji Wpięć można utworzyć plik IPA i jego APK dla naszych projektów mobilnymi Xamarin.

## <a name="requirements"></a>Wymagania

Serwer idealne kompilacji jest komputerem autonomicznym dedykowany wyłącznie w celu tworzenia i prawdopodobnie testowania aplikacji. Dedykowanym komputerze gwarantuje, że artefakty, które mogą być wymagane dla innych ról (np. z serwera sieci web) nie skażenia kompilacji. Na przykład jeśli serwer kompilacji jest również działającym jako serwer sieci web, serwer sieci web może wymagać powodujące konflikt wersji niektórych typowych biblioteki. Ze względu na konflikt serwera sieci web mogą nie działać poprawnie lub Wpięć może utworzyć kompilacje, które nie działają w przypadku wdrożone dla użytkowników.

Serwer kompilacji dla aplikacji mobilnych Xamarin skonfigurowano się bardzo podobne jak w przypadku stacji roboczej dewelopera. Konto użytkownika ma w których Wpięć programu Visual Studio for Mac, i Xamarin.iOS i Xamarin.Android zostaną zainstalowane. Wszystkie certyfikaty, inicjowanie obsługi administracyjnej profile i magazyny klucza podpisywania kodu musi być zainstalowany także. *Zazwyczaj konta użytkownika serwera kompilacji jest oddzielony od konta dewelopera — należy zainstalować i skonfigurować oprogramowanie, klucze i certyfikaty po zalogowaniu się przy użyciu konta użytkownika serwera kompilacji.*

Na poniższym diagramie przedstawiono wszystkie te elementy na serwerze kompilacji typowe Wpięć:

 [![](jenkins-walkthrough-images/image1.png "Ten diagram przedstawia wszystkich tych elementów na serwerze kompilacji typowe Wpięć")](jenkins-walkthrough-images/image1.png#lightbox)

aplikacje systemu iOS można tylko wbudowane i podpisane na komputerze z systemem Mac OS X. Mac Mini jest uzasadnione opcja tanie, ale każdy komputer umożliwiający uruchomienie systemu OS X 10.10 (Yosemite) lub nowszy jest wystarczająca.

Jeśli TFS jest używana do kontroli kodu źródłowego, można zainstalować [Team Explorer Everywhere](http://www.microsoft.com/en-ca/download/details.aspx?id=40785), udostępnianych przez firmę Microsoft. Team Explorer Everywhere zapewnia dostęp i platform do TFS w terminalu w OS X.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="installing-jenkins"></a>Instalowanie Wpięć

Pierwszym zadaniem do korzystania z Wpięć jest jej zainstalowanie. Istnieją trzy sposoby uruchamiania Wpięć na OS X:

-  Jako demon uruchomione w tle.
-  Wewnątrz kontenera serwlet jak Tomcat i Jetty lub JBoss.
-  Jako normalny proces uruchomiony na koncie użytkownika.


Uruchomione w tle, albo jako demon najbardziej tradycyjne aplikacje ciągłej integracji (na OS X lub \*nix) lub jako usługa (w systemie Windows). Jest to odpowiednie dla scenariuszy, gdzie wymagane interakcji graficznego interfejsu użytkownika nie istnieje, a ustawienia środowiska kompilacji można łatwo wykonać. Aplikacje mobilne również wymagać keystores i podpisywania certyfikatów, które mogą być problemy na dostęp, jeśli Wpięć działa jako demon. Z powodu tych problemów ten dokument koncentruje się na trzeci scenariusz — uruchomiona Wpięć przy użyciu konta użytkownika na serwerze kompilacji.

Jenkins.App jest wygodnym sposobem instalowania Wpięć. Jest to otoki AppleScript, które upraszcza początkową i zatrzymywanie serwera Wpięć. Zamiast działać w powłoki bash, Wpięć działa jako aplikacja z ikony w doku, jak pokazano na poniższym zrzucie ekranu:

 [![](jenkins-walkthrough-images/image2.png "Zamiast działać w powłoki bash, Wpięć działa jako aplikacji z ikoną w doku, jak pokazano w tym zrzut ekranu")](jenkins-walkthrough-images/image2.png#lightbox)

Uruchamianie lub zatrzymywanie Wpięć jest tak proste, jak uruchamianie lub zatrzymywanie Jenkins.App.

Aby zainstalować Jenkins.App, Pobierz najnowszą wersję ze strony pobierania projektu, na poniższym zrzucie ekranu:

 [![](jenkins-walkthrough-images/image3.png "Aplikacja, pobierania strony, na tym zrzucie ekranu pokazano pobierania najnowszej wersji w projektach")](jenkins-walkthrough-images/image3.png#lightbox)

Wyodrębnij plik zip do `/Applications` folderu na serwerze kompilacji i rozpoczęcia, podobnie jak inne aplikacji OS X.
Podczas pierwszego uruchamiania Jenkins.App, wyświetli się okno dialogowe informujące, że będą pobierać Wpięć:

 [![](jenkins-walkthrough-images/image4.png "Aplikacja, przedstawi okno dialogowe informujące, że będą pobierać Wpięć")](jenkins-walkthrough-images/image4.png#lightbox)

Po zakończeniu jego pobierania Jenkins.App wyświetli okno dialogowe pytaniem, jeśli chcesz dostosować uruchamiania Wpięć, jak pokazano na poniższym zrzucie ekranu:

 [![](jenkins-walkthrough-images/image5.png "Aplikacja zakończyło się jego pobierania, wyświetli okno dialogowe pytaniem, jeśli chcesz dostosować uruchamiania Wpięć w tym zrzut ekranu")](jenkins-walkthrough-images/image5.png#lightbox)

Dostosowywanie Wpięć jest opcjonalna i nie trzeba wykonywać zawsze, gdy aplikacja jest uruchomiona — ustawienia domyślne dla Wpięć będzie działać w większości sytuacji.

Jeśli jest to niezbędne do dostosowania Wpięć polecenie **zmienić ustawienia domyślne** przycisku. To spowoduje wyświetlenie dwóch kolejnych okien dialogowych: jeden z pytaniem, czy parametry wiersza polecenia języka Java, a druga pyta o Wpięć parametry wiersza polecenia. Dwa poniższe zrzuty ekranu pokazują tych dwóch okien dialogowych:

 [![](jenkins-walkthrough-images/image6.png "Ten zrzut ekranu przedstawia okna dialogowe")](jenkins-walkthrough-images/image6.png#lightbox)

 [![](jenkins-walkthrough-images/image7.png "Ten zrzut ekranu przedstawia okna dialogowe")](jenkins-walkthrough-images/image7.png#lightbox)

Po uruchomieniu Wpięć, można skonfigurować jako element logowania tak zostanie uruchomiony podczas logowania użytkownika w na komputerze. Aby to zrobić, klikając prawym przyciskiem myszy ikonę Wpięć w doku i wybierając polecenie **opcje... Otwórz przy logowaniu**, jak pokazano na poniższym zrzucie ekranu:

 [![](jenkins-walkthrough-images/image8.png "Aby to zrobić, prawym przyciskiem myszy ikonę Wpięć w doku i wybierając polecenie OptionsOpen przy logowaniu, jak pokazano w tym zrzut ekranu")](jenkins-walkthrough-images/image8.png#lightbox)

Spowoduje to Jenkins.App automatycznie uruchomić zawsze loguje się użytkownik, ale nie gdy komputer jest uruchamiany. Istnieje możliwość określenia konta użytkownika używanego do automatycznego logowania przy użyciu OS X w czasie rozruchu. Otwórz **preferencjach systemowych**i wybierz **& grupy użytkowników** ikony, jak pokazano w tym zrzut ekranu:

 [![](jenkins-walkthrough-images/image9.png "Otwórz preferencjach systemowych i wybierz ikonę grupy użytkowników, jak pokazano w tym zrzut ekranu")](jenkins-walkthrough-images/image9.png#lightbox)

Polecenie **opcje logowania** przycisk, a następnie wybierz konta używanego do logowania OS X w czasie rozruchu.

W tym momencie Wpięć została zainstalowana. Jednak jeśli chcemy do tworzenia aplikacji platformy Xamarin dla urządzeń przenośnych, możemy będą musieli zainstalować niektórych wtyczek.


### <a name="installing-plugins"></a>Instalowanie wtyczki

Po zakończeniu Instalator Jenkins.App go uruchomi Wpięć i uruchomić przeglądarki sieci web z adresem URL http://localhost:8080, jak pokazano na poniższym zrzucie ekranu:

 [![](jenkins-walkthrough-images/image10.png "8080, jak pokazano w tym zrzut ekranu")](jenkins-walkthrough-images/image10.png#lightbox)

Na tej stronie wybierz **Wpięć > Zarządzaj Wpięć > Zarządzaj wtyczkami** z menu w lewy górny róg, jak pokazano na poniższym zrzucie ekranu:

 [![](jenkins-walkthrough-images/image11.png "Na tej stronie Wybierz wtyczki, zarządzanie Wpięć Zarządzanie Wpięć z menu w lewy górny narożnik")](jenkins-walkthrough-images/image11.png#lightbox)

Spowoduje to wyświetlenie **Wpięć wtyczki Menedżera** strony. Po kliknięciu na karcie dostępne, zostanie wyświetlona lista ponad 600 wtyczek, które może zostać pobrana i zainstalowana. To jest przedstawiony na poniższym zrzucie ekranu:

 [![](jenkins-walkthrough-images/image12.png "Po kliknięciu na karcie dostępne, zostanie wyświetlona lista ponad 600 wtyczek, które może zostać pobrana i zainstalowana")](jenkins-walkthrough-images/image12.png#lightbox)

Przewijanie wszystkie wtyczki, aby znaleźć kilka może być niewygodny 600 i błąd podatnych na błędy. Wpięć zawiera pole wyszukiwania filtru w prawym górnym rogu interfejsu. Za pomocą tego pola filtru wyszukiwania uprości lokalizowania i zainstalowane jedno lub wszystkie następujące wtyczki:

-  **Wtyczka programu MSBuild Wpięć** — tej wtyczki umożliwia kompilacji programu Visual Studio i Visual Studio dla komputerów Mac rozwiązania (sln) i projekty (.csproj).
-  **Dodatek iniektor środowiska** — jest to opcjonalne, ale przydatne wtyczkę, która pozwala na Ustawianie zmiennych środowiskowych w zadaniu i kompilacji poziomu. Oferuje również dodatkowej ochrony dla zmiennych, takich jak hasła używane do kodu podpisania aplikacji. Czasami jest skracana jako *wtyczki EnvInject* .
-  **Team Foundation Server wtyczki** — jest to opcjonalne wtyczki, które są tylko wymagane, jeśli używasz programu Team Foundation Server lub Team Foundation Services do kontroli kodu źródłowego.

Bez żadnych dodatkowych wtyczek Wpięć obsługuje Git.

Po zainstalowaniu wszystkich wtyczek, należy ponownie uruchomić Wpięć i skonfigurować globalne ustawienia dla każdej wtyczki. Ustawienia globalne dla wtyczki można znaleźć po wybraniu **Wpięć > Zarządzaj Wpięć > skonfiguruj System** z lewy górny róg, jak pokazano na poniższym zrzucie ekranu:

 [![](jenkins-walkthrough-images/image13.png "Ustawienia globalne dla wtyczki można znaleźć po wybraniu Wpięć / Zarządzanie Wpięć / rogu ręcznie skonfigurować System z lewej górnej")](jenkins-walkthrough-images/image13.png#lightbox)

Po wybraniu tej opcji menu, nastąpi przekierowanie do **skonfigurować System [Wpięć]** strony. Ta strona zawiera sekcje do konfigurowania Wpięć się i ustawić niektórych wartości globalnej wtyczki.  Poniższy zrzut ekranu przedstawia przykład strony:

 [![](jenkins-walkthrough-images/image14.png "Ten zrzut ekranu przedstawia przykład strony")](jenkins-walkthrough-images/image14.png#lightbox)


#### <a name="configuring-the-msbuild-plugin"></a>Konfigurowanie wtyczki programu MSBuild

Wtyczka programu MSBuild musi być skonfigurowana do używania **/Library/Frameworks/Mono.framework/Commands/xbuild** skompilować dla komputerów Mac pliki rozwiązań i projektów programu Visual Studio. Przewiń w dół **skonfigurować System [Wpięć]** strony do **dodać MSBuild** przycisk pojawia się, jak pokazano na poniższym zrzucie ekranu:

 [![](jenkins-walkthrough-images/image15.png "Przewiń w dół strony konfigurowania systemu Wpięć aż pojawi się przycisk Dodaj MSBuild")](jenkins-walkthrough-images/image15.png#lightbox)

Kliknij ten przycisk, a następnie wypełnij **nazwa** i **ścieżki** do **MSBuild** pól w wyświetlonym formularzu. Nazwa użytkownika **MSBuild** instalacja powinna być, wpisując tekst opisowy, podczas **ścieżki dla programu MSBuild** powinna być ścieżką do `xbuild`, co jest typowe **/Library/struktury / Mono.framework/Commands/xbuild**. Po możemy zapisać zmiany, klikając przycisk Zastosuj w dolnej części strony lub Zapisz Wpięć będą mogli używać `xbuild` skompilować rozwiązań.

#### <a name="configuring-the-tfs-plugin"></a>Konfigurowania serwera TFS

Ta sekcja jest wymagane, jeśli zamierzasz używać programu TFS dla programu kontroli kodu źródłowego.

Aby stacji roboczej OS X do interakcji z serwerem TFS Team Explorer Everywhere musi być zainstalowany na stacji roboczej. Team Explorer Everywhere jest zestaw narzędzi firmy Microsoft, który zawiera klient wiersza polecenia i platform do uzyskiwania dostępu do programu TFS. Team Explorer Everywhere można pobrać z witryny Microsoft i zainstalowane w trzy kroki:

1. Rozpakuj plik archiwum do katalogu, który jest dostępny dla konta użytkownika. Na przykład może Rozpakuj plik **~/tee**.
2. Konfigurowanie ścieżki powłoki lub system uwzględnienie folder, w którym znajdują się pliki, które rozpakowane w kroku powyżej jednego. Na przykład

        echo export PATH~/tee/:$PATH' >> ~/.bash_profile

3. Aby upewnić się, że jest zainstalowana Team Explorer Everywhere, otwórz sesję terminala i wykonać `tf` polecenia. Jeśli tf jest prawidłowo skonfigurowane, zobaczysz następujące dane wyjściowe w sesji terminala:

        $ tf
        Team Explorer Everywhere Command Line Client (version 11.0.0.201306181526)

        Available commands and their options:

Po zainstalowaniu klienta wiersza polecenia dla serwerów TFS Wpięć musi być skonfigurowany z pełną ścieżką do `tf` klient wiersza polecenia. Przewiń w dół **skonfigurować System [Wpięć]** strony do momentu znalezienia sekcji Team Foundation Server, jak pokazano na poniższym zrzucie ekranu:

 [![](jenkins-walkthrough-images/image17.png "Przewiń w dół strony konfigurowania Wpięć systemu do Znajdź sekcję Team Foundation Server")](jenkins-walkthrough-images/image17.png#lightbox)

Wprowadź pełną ścieżkę do `tf` polecenia, a następnie kliknij przycisk **zapisać** przycisku.

### <a name="configure-jenkins-security"></a>Konfigurowanie zabezpieczeń Wpięć

Podczas pierwszej instalacji Wpięć ma zabezpieczeń wyłączone, co dla każdego użytkownika skonfigurować i uruchomić anonimowo dowolnego rodzaju zadania. W tej sekcji opisano sposób konfigurowania zabezpieczeń przy użyciu bazy danych użytkowników Wpięć Konfigurowanie uwierzytelniania i autoryzacji.

Ustawienia zabezpieczeń można znaleźć po wybraniu **Wpięć > Zarządzaj Wpięć > Konfigurowanie zabezpieczeń globalnych**, jak pokazano w tym zrzut ekranu:

 [![](jenkins-walkthrough-images/image18.png "Ustawienia zabezpieczeń można znaleźć po wybraniu Wpięć / Zarządzanie Wpięć / Konfigurowanie zabezpieczeń globalnych")](jenkins-walkthrough-images/image18.png#lightbox)

Na **Konfigurowanie zabezpieczeń globalnych** strony, sprawdź **Włącz zabezpieczenia** wyboru i **kontroli dostępu** formularza powinny wyglądać, podobnie jak dalej zrzut ekranu:

 [![](jenkins-walkthrough-images/image19.png "Na stronie Konfigurowanie zabezpieczeń globalnych Sprawdź Włącz zabezpieczenia wyboru i kontroli dostępu formularza powinna zostać wyświetlona, podobny do tego zrzutu ekranu")](jenkins-walkthrough-images/image19.png#lightbox)

Przełącz przycisk radiowy **bazy danych użytkowników własnych Wpięć** w **sekcji obszaru zabezpieczeń**i upewnij się, że **umożliwiają użytkownikom logowanie** jest również sprawdzane, zgodnie z opisami w Poniższy zrzut ekranu:

 [![](jenkins-walkthrough-images/image20.png "Przełącz przycisk radiowy Wpięć własnych użytkownika z bazy danych w sekcji obszaru zabezpieczeń i upewnij się również zaznaczono Zezwalaj użytkownikom logowanie")](jenkins-walkthrough-images/image20.png#lightbox)

Na koniec uruchom ponownie Wpięć i Utwórz nowe konto. Pierwsze konto, który jest tworzony jest konta głównego i to konto zostanie automatycznie podwyższony do poziomu administratora. Przejdź z powrotem do **Konfigurowanie zabezpieczeń globalnych** strony, a następnie odznaczyć **zabezpieczenia oparte na macierzy** przycisk radiowy. Konta głównego może być przyznany dostęp, a konto anonimowe ma uzyskać dostęp tylko do odczytu, jak pokazano na poniższym zrzucie ekranu:

 [![](jenkins-walkthrough-images/image21.png "Konta głównego może być przyznany dostęp, a konto anonimowe ma uzyskać dostęp tylko do odczytu")](jenkins-walkthrough-images/image21.png#lightbox)

Gdy te ustawienia są zapisywane i ponownym uruchomieniu Wpięć zabezpieczeń zostanie włączona.


#### <a name="disabling-security"></a>Wyłączenie zabezpieczeń

W przypadku zapomniane hasło lub całej Wpięć blokady możliwe jest wyłączenie zabezpieczeń, wykonaj następujące czynności:

1. Zatrzymaj Wpięć. Jeśli używasz Jenkins.app, aby to zrobić, klikając prawym przyciskiem myszy ikonę Jenkins.App w doku i wybierając Zamknij z menu, które pojawia się:

    ![](jenkins-walkthrough-images/image19.png "Ikona aplikacji w doku i wybierając z menu, które pojawia się Zamknij")
2. Otwórz plik **~/.jenkins/config.xml** w edytorze tekstów.
3. Zmień wartość `<usesecurity></usesecurity>` element z `true` do `false`.
4. Usuń `<authorizationstrategy></authorizationstrategy>` i `<securityrealm></securityrealm>` elementy z pliku.
5. Uruchom ponownie Wpięć.

## <a name="setting-up-a-job"></a>Konfigurowanie zadania

Na najwyższym poziomie Wpięć organizuje wszystkie poszczególne zadania wymagane do tworzenia oprogramowania do *zadania*. Zadanie ma również metadane skojarzone z nią dostarczanie informacji o kompilacji, takich jak uzyskiwanie kod źródłowy, jak często ma być uruchamiany kompilacji, zmienne specjalne, które są niezbędne do tworzenia i jak powiadomić deweloperów, jeśli kompilacja zakończy się niepowodzeniem.

Zadania są tworzone przez wybranie **Wpięć > nowe zadanie** z menu w prawym górnym rogu, jak pokazano na poniższym zrzucie ekranu:


![](jenkins-walkthrough-images/image22.png "Zadania są tworzone, wybierając z menu w prawym górnym rogu Wpięć nowe zadanie")

Spowoduje to wyświetlenie **nowe zadanie [Wpięć]** strony. Wprowadź nazwę dla zadania, a następnie wybierz **kompilowania projektu oprogramowania wolne stylu** przycisk radiowy. Poniższy zrzut ekranu przedstawia przykład:

![](jenkins-walkthrough-images/image23.png "Wprowadź nazwę dla zadania, a następnie wybierz kompilacji oprogramowania wolne styl przycisku radiowego projektu")

Kliknięcie przycisku **OK** przycisk przedstawia ze stroną konfiguracji tego zadania. Powinno to wyglądać Poniższy zrzut ekranu:

![](jenkins-walkthrough-images/image24.png "To powinien przypominać ten zrzut ekranu")

Wpięć organizuje zadań w katalogu na dysku twardym znajduje się w następującej ścieżce: **~/.jenkins/jobs/[JOB nazwy]**

Ten folder zawiera wszystkie pliki i artefaktów przeznaczonych tylko dla zadania, takie jak dzienniki, pliki konfiguracji i kod źródłowy, który ma być kompilowana.

Po utworzeniu zadania początkowej, musi być skonfigurowany z co najmniej jeden z następujących czynności:

 - Należy określić system zarządzania kodu źródłowego.
 - Co najmniej jeden *akcji* musi zostać dodany do projektu. Oto kroki lub zadania wymagane do tworzenia aplikacji.
 - To zadanie należy przypisać jedną *kompilacji wyzwalacza* — zestaw instrukcji dowie się, jak często Wpięć można pobrać kodu i kompilacji ostatecznego projektu.

### <a name="configuring-source-code-control"></a>Konfigurowanie kontroli kodu źródłowego

Pierwszym zadaniem Wpięć nie jest pobrać kodu źródłowego z systemu zarządzania kodu źródłowego. Wpięć obsługuje wiele obecnie dostępne w systemach zarządzania kodem źródłowym popularnych. W tej sekcji omówiono dwóch popularnych systemów Git i Team Foundation Server. Każdy z tych systemach zarządzania kodem źródłowym omówiono bardziej szczegółowo w poniższych sekcjach.

#### <a name="using-git-for-source-code-control"></a>Przy użyciu usługi Git do kontroli kodu źródłowego

Jeśli używasz programu TFS do kontroli kodu źródłowego, [pominąć](#Using_TFS_for_Source_Code_Management) tej sekcji, a następnie przejdź do następnej sekcji za pomocą TFS.

Wpięć obsługuje Git fabrycznej — nie dodatkowe wtyczki są niezbędne. Aby użyć narzędzia Git, kliknij na **Git** przycisk radiowy i wprowadź adres URL repozytorium Git, jak pokazano na poniższym zrzucie ekranu:

![](jenkins-walkthrough-images/image25.png "Aby użyć narzędzia Git, kliknij przycisk radiowy Git i wprowadź adres URL repozytorium Git")

Po zapisaniu zmian Git konfiguracja zostanie zakończona.

#### <a name="using-tfs-for-source-code-management"></a>Za pomocą TFS do zarządzania kodem źródłowym

Ta sekcja dotyczy tylko użytkowników TFS.

Polecenie **Team Foundation Server** przycisku radiowego i TFS sekcji konfiguracji powinna zostać wyświetlona, podobnie jak w poniższym zrzucie ekranu:


![](jenkins-walkthrough-images/image26.png "Kliknij przycisk radiowy Team Foundation Server i powinna zostać wyświetlona w sekcji konfiguracji serwera TFS")

Podaj informacje niezbędne do TFS. Poniższy zrzut ekranu przedstawia przykład wypełnionego formularza:

![](jenkins-walkthrough-images/image27.png "Ten zrzut ekranu przedstawia przykład wypełnionego formularza")

#### <a name="testing-the-source-code-control-configuration"></a>Testowanie konfiguracji kontroli kodu źródłowego

Po skonfigurowaniu kontroli kodu źródłowego odpowiednie kliknij **zapisać** Aby zapisać zmiany. Spowoduje to powrót do strony głównej dla zadania, które będą wyglądać Poniższy zrzut ekranu:

![](jenkins-walkthrough-images/image28.png "Spowoduje to powrót do strony głównej dla zadania, które będzie podobny do tego zrzutu ekranu")

Najprostszym sposobem, aby zweryfikować, że poprawnie skonfigurowano kontroli kodu źródłowego jest wyzwolić kompilację, ręcznie, nawet jeśli nie ma żadnych akcji kompilacji określony. Aby ręcznie uruchomić kompilacji, strony głównej zadanie ma **kompilacji teraz** Połącz w menu po lewej stronie, jak pokazano na poniższym zrzucie ekranu:

![](jenkins-walkthrough-images/image29.png "Aby ręcznie uruchomić kompilacji, strony głównej zadania zawiera łącze kompilacji teraz w menu po lewej stronie")

Podczas kompilacji został uruchomiony, okno dialogowe Tworzenie historii Wyświetla miga niebieskie koło, pasek postępu, numer kompilacji i godzina rozpoczęcia kompilacji, podobnie jak poniższy zrzut ekranu:

![](jenkins-walkthrough-images/image30.png "Rozpoczęcie kompilacji okno dialogowe Tworzenie historii Wyświetla koło miga niebieski, pasek postępu, numer kompilacji i czasu, który uruchomił kompilacji")

Jeśli zadanie zakończy się pomyślnie, zostanie wyświetlony niebieskie koło. Jeśli zadanie nie powiedzie się, pojawi się czerwone kółko.

Aby pomóc w rozwiązywaniu problemów, które mogą wystąpić podczas kompilacji, Wpięć będzie przechwytywać wszystkie dane wyjściowe konsoli dla zadania. Aby wyświetlić dane wyjściowe konsoli, kliknij pozycję zadania w **kompilacji historii**, a następnie na **dane wyjściowe konsoli** łącza w menu po lewej stronie. Poniższy zrzut ekranu przedstawia **dane wyjściowe konsoli** łącza, a także niektóre z danych wyjściowych z zadania powiodło się:

![](jenkins-walkthrough-images/image31.png "Ten zrzut ekranu przedstawia łącze dane wyjściowe konsoli, a także niektóre z danych wyjściowych z zadania powiodło się")

#### <a name="location-of-build-artifacts"></a>Lokalizacja artefaktów kompilacji

Wpięć pobierze kod źródłowy całego do specjalnego folderu o nazwie *obszaru roboczego*. Ten katalog znajduje się w folderze w następującej lokalizacji:

    ~/.jenkins/jobs/[JOB NAME]/workspace

Ścieżka do obszaru roboczego będzie przechowywana w zmiennej środowiskowej o nazwie `$WORKSPACE`.

Umożliwia przeglądanie folderu roboczego w Wpięć nawigowania do strony docelowej dla zadania, a następnie klikając polecenie na **obszaru roboczego** łącza w menu po lewej stronie. Poniższy zrzut ekranu przedstawia przykład obszaru roboczego dla zadania o nazwie **HelloWorld**:

![](jenkins-walkthrough-images/image32.png "Ten zrzut ekranu przedstawia przykład obszaru roboczego dla zadania o nazwie HelloWorld")

### <a name="build-triggers"></a>Tworzenie wyzwalaczy

Istnieje kilka strategii inicjowania kompilacji w Wpięć — są one nazywane *kompilacji wyzwalaczy*. Wyzwalacz kompilacji pomaga Wpięć zadecydować o czasie rozpocząć zadanie i skompilować projekt. Dwa najczęściej wyzwalaczy kompilacji są:

- **Tworzenie okresowo** — Wpięć do rozpoczęcia zadania w określonych odstępach czasu, takich jak co dwie godziny lub o północy w dni robocze powoduje tego wyzwalacza. Kompilacja zostanie uruchomiony niezależnie od tego, czy zaszły zmiany w repozytorium kodu źródłowego.
- **Menedżer sterowania usługami sondowania** — tego wyzwalacza wykona sondowanie kontroli kodu źródłowego na bieżąco. Jeśli zmiany zostały przekazane do repozytorium kodu źródłowego, Wpięć spowoduje uruchomienie nowej kompilacji.

Sondowanie SCM jest popularnych wyzwalacza, ponieważ zapewnia szybkie opinii dewelopera zatwierdza zmiany powodujące kompilacji do dzielenia. Jest to przydatne alertów zespoły ostatnio zatwierdzone kodu powoduje problemy i umożliwia deweloperom rozwiązać problem podczas zmiany są nadal świeże pamiętać.

Okresowe kompilacje są często używane do tworzenia wersję aplikacji, które mogą być dystrybuowane testerów. Na przykład kompilacja okresowych może zostać zaplanowane wieczorem piątek, aby członkowie zespołu pytań i odpowiedzi można testować pracy poprzedniego tygodnia.


### <a name="compiling-a-xamarinios-applications"></a>Kompilowanie aplikacji platformy Xamarin.iOS
Za pomocą wiersza polecenia można kompilować projektów platformy Xamarin.iOS `xbuild` lub `msbuild`. Polecenia powłoki będzie wykonywany w kontekście konta użytkownika, który działa Wpięć. Należy pamiętać, że konto użytkownika ma dostęp do profilu inicjowania obsługi administracyjnej, dzięki czemu można poprawnie spakować aplikację do dystrybucji. Istnieje możliwość dodania tego polecenia powłoki do strony Konfiguracja zadania.

Przewiń w dół do **kompilacji** sekcji. Kliknij przycisk **kroku kompilacji Dodaj** i wybrać **wykonywania powłoki**, jak pokazano na poniższym zrzucie ekranu:

![](jenkins-walkthrough-images/image33.png "Kliknij przycisk Dodaj krok kompilacji i wybierz Execute powłoki")


[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

### <a name="building-a-xamarinandroid-project"></a>Tworzenie projektu platformy Xamarin.Android

Tworzenie projektu platformy Xamarin.Android jest bardzo podobna do tworzenia projektu platformy Xamarin.iOS. Aby utworzyć APK z projektu platformy Xamarin.Android, Wpięć musi być skonfigurowana do wykonania następujących dwóch kroków:

 - Kompilowanie projektu przy użyciu wtyczki programu MSBuild
 - Znak, a następnie zip wyrównuje APK z keystore prawidłowy wersji.

Następujące dwa kroki zostanie omówiona bardziej szczegółowo w dwóch następnych sekcjach.

### <a name="creating-the-apk"></a>Tworzenie plik APK

Polecenie **kroku kompilacji Dodaj** i wybrać **kompilacji projektu programu Visual Studio lub za pomocą MSBuild rozwiązania**, jak pokazano na poniższym zrzucie ekranu:

![](jenkins-walkthrough-images/image36.png "Tworzenie kliknij APK na przycisku kroku kompilacji Dodaj i wybierz kompilację projektu Visual Studio lub rozwiązanie przy użyciu programu MSBuild")

Po dodaniu kroku kompilacji do projektu, wypełnij pola formularza, które są wyświetlane. Poniższy zrzut ekranu jest jednym z przykładów wypełnionego formularza:

![](jenkins-walkthrough-images/image37.png "Po dodaniu kroku kompilacji do projektu, wypełnij pola formularza, które są wyświetlane")


Ten krok kompilacji będzie wykonywał `xbuild` w **$WORKSPACE** folderu. Ustawiono pliku kompilacji MSBuild **Xamarin.Android.csproj** pliku. **Argumenty wiersza polecenia** Określ kompilację wersji docelowej **PackageForAndroid**. Produkt ten krok będzie APK który w następującej lokalizacji:

    $WORKSPACE/[PROJECT NAME]/bin/Release

Poniższy zrzut ekranu przedstawia przykład tego APK:

![](jenkins-walkthrough-images/image38.png "Ten zrzut ekranu przedstawia przykład tego APK")

To APK nie jest gotowa do wdrożenia, ponieważ nie został podpisany z prywatnej magazynu kluczy i muszą być zip wyrównane.

#### <a name="signing-and-zipaligning-the-apk-for-release"></a>Podpisywanie i Zipaligning APK w wersji

Podpisywanie i zipaligning plik APK są technicznie dwóch oddzielnych zadań wykonywanych przez dwa oddzielne wiersza polecenia narzędzi z zestawu SDK systemu Android. Jednak jest wygodne wykonywanie je w jednej kompilacji akcji. Aby uzyskać więcej informacji o logowaniu i zipaligning APK dokumentacji firmy Xamarin na przygotowanie aplikacji systemu Android w wersji.

Oba te polecenia wymaga parametrów wiersza polecenia, które mogą się różnić od projektu do projektu. Ponadto niektóre z tych parametrów wiersza polecenia są hasła, które nie mogą występować w danych wyjściowych konsoli, gdy kompilacja jest uruchomiona. Niektóre z tych parametrów wiersza polecenia będą przechowywane w zmiennych środowiskowych. Zmienne środowiskowe wymagane do podpisywania i/lub wyrównywanie zip zostały opisane w poniższej tabeli:

|Zmienna środowiskowa|Opis|
|--- |--- |
|KEYSTORE_FILE|To jest ścieżka do magazynu kluczy do podpisywania plik APK|
|KEYSTORE_ALIAS|Klucz w magazynie kluczy, który będzie służyć do logowania się plik APK.|
|INPUT_APK|APK, który jest tworzony przez `xbuild`.|
|SIGNED_APK|Podpisem APK utworzonego przez `jarsigner`.|
|FINAL_APK|Jest to zip wyrównane APK, który jest generowany przez `zipalign`.|
|STORE_PASS|To hasło, które jest używane do dostępu do zawartości dla singing pliku magazynu kluczy.|

Zgodnie z opisem w sekcji wymagań, podczas kompilacji za pomocą wtyczki EnvInject można ustawić zmienne środowiskowe. Zadanie powinno mieć nowej kompilacji krok dodany oparte na wsuwania zmiennych środowiskowych, jak pokazano w następnym zrzut ekranu:

![](jenkins-walkthrough-images/image39.png "Zadanie powinno mieć nowej kompilacji krok dodany oparte na wsuwania zmienne środowiskowe")

W **właściwości zawartości** tworzą pola, które będą wyświetlane, środowisko zmienne są dodawane, po jednym w każdym wierszu, w następującym formacie:

    ENVIRONMENT_VARIABLE_NAME = value

Poniższy zrzut ekranu przedstawia zmiennych środowiskowych, które są wymagane do podpisywania plik APK:

![](jenkins-walkthrough-images/image40.png "Ten zrzut ekranu przedstawia zmiennych środowiskowych, które są wymagane do podpisywania plik APK")

Zwróć uwagę, niektóre zmienne środowiskowe dla plików APK są tworzone `WORKSPACE` zmiennej środowiskowej.

Zmienna środowiskowa końcowego jest hasło, aby uzyskać dostęp do zawartości magazynu kluczy: `STORE_PASS`. Hasła są wartości poufnych, które powinny być zasłonięty lub pominięty w plikach dziennika. Dodatek EnvInject można skonfigurować do ochrony tych wartości, tak aby nie są widoczne w dziennikach.

Bezpośrednio przed **kompilacji** sekcji konfiguracji zadania jest **Build Environment** sekcji. Gdy **wprowadzenia hasła** pole wyboru jest włączone, jakiegoś pola pojawią się. Te pola są używane do przechwytywania nazwę i wartość zmiennej środowiskowej. Poniższy zrzut ekranu jest przykładem Dodawanie `STORE_PASS` zmiennej środowiskowej:

![](jenkins-walkthrough-images/image41.png "Ten zrzut ekranu jest przykładem Dodawanie zmiennej środowiskowej STOREPASS")

Zmienne środowiskowe zostały zainicjowane, następnym krokiem po dodać kroku kompilacji do podpisywania i zip wyrównywanie plik APK. Natychmiast po kroku kompilacji, aby wstawić zmiennych środowiskowych będzie inny **wykonywania powłoki** poleceń kompilacji, która będzie wykonywana `jarsigner` i `zipalign`. Każde polecenie będzie zajmować jednej linii, jak pokazano w poniższy fragment kodu:


    jarsigner -verbose -sigalg MD5withRSA -digestalg SHA1 -keystore $KEYSTORE_FILE -storepass $STORE_PASS -signedjar $SIGNED_APK $INPUT_APK $KEYSTORE_ALIAS
    zipalign -f -v 4 $SIGNED_APK $FINAL_APK

Poniższy zrzut ekranu przedstawia przykład sposobu wprowadzania `jarsigner` i `zipalign` polecenia w kroku:

![](jenkins-walkthrough-images/image42.png "Ten zrzut ekranu przedstawia przykład sposobu wprowadzania polecenia jarsigner i zipalign w kroku")

Po zatwierdzeniu wszystkich akcji kompilacji w miejscu, jest dobrym rozwiązaniem, aby wyzwolić ręczne kompilacji, aby sprawdzić, czy wszystko działa. Jeśli kompilacja zakończy się niepowodzeniem, **dane wyjściowe konsoli** należy sprawdzić, aby uzyskać informacje dotyczące przyczyn niepowodzenia kompilacji.

### <a name="submitting-tests-to-test-cloud"></a>Przesyłanie testów do chmury testowej

Testy automatyczne może zostać przesłane do chmury testowej przy użyciu powłoki poleceń. Aby uzyskać więcej informacji na temat konfigurowania uruchomienia testu w chmury testowej Xamarin mamy przewodniki dotyczące korzystania z [Xamarin.UITest](https://developer.xamarin.com/guides/testcloud/uitest/working-with/submitting-tests-to-xamarin-test-cloud/) lub [Calabash](https://developer.xamarin.com/guides/testcloud/calabash/working-with/submitting-tests-to-xamarin-test-cloud/).


## <a name="summary"></a>Podsumowanie

W tym przewodniku możemy wprowadzono Wpięć jako serwer kompilacji w systemie Mac OS X i skonfigurować go skompilować i przygotować aplikacje platformy Xamarin dla wersji. Zainstalowano Wpięć na komputerze Mac OS X oraz kilka wtyczek do obsługi procesu kompilacji. Firma Microsoft utworzone i skonfigurowane zadania pobierania kodu z Git lub TFS, a następnie skompilować kod w wersji aplikacji gotowe. Możemy również przedstawione dwa różne sposoby Zaplanuj, kiedy zadania są uruchamiane.

## <a name="related-links"></a>Linki pokrewne

- [Ciągła integracja](https://developer.xamarin.com~/testcloud/ci/)
- [Przesyłanie testów do chmury testowej Xamarin](https://developer.xamarin.com~/testcloud/submitting/)
- [Dlaczego Wpięć nie jest obsługiwana przez program Xamarin?](~/cross-platform/troubleshooting/questions/xamarin-jenkins.md)
