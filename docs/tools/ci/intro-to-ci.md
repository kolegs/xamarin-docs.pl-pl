---
title: Wprowadzenie do ciągłej integracji z platformą Xamarin
description: Ciągła Integracja jest rozwiązaniem engineering oprogramowania, w którym automatycznych kompilacji kompiluje i opcjonalnie testuje aplikację, jeśli kod jest dodane lub zmienione przez deweloperów w repozytorium kontroli wersji projektu. W tym artykule przedstawimy ogólne koncepcje ciągłej integracji i niektóre z dostępnych opcji dla ciągłej integracji z projektami Xamarin.
ms.prod: xamarin
ms.assetid: C034200E-2947-4309-9DDD-80DAC505C43F
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 07/19/2017
ms.openlocfilehash: 017691ece68f979eea1627c0442f49018d5742fb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-continuous-integration-with-xamarin"></a>Wprowadzenie do ciągłej integracji z platformą Xamarin

_Ciągła Integracja jest rozwiązaniem engineering oprogramowania, w którym automatycznych kompilacji kompiluje i opcjonalnie testuje aplikację, jeśli kod jest dodane lub zmienione przez deweloperów w repozytorium kontroli wersji projektu. W tym artykule przedstawimy ogólne koncepcje ciągłej integracji i niektóre z dostępnych opcji dla ciągłej integracji z projektami Xamarin._

Są często w przypadku projektów oprogramowania deweloperom działa równolegle. W pewnym momencie jest niezbędne do integracji wszystkich tych równoległych strumieni pracy do jednej ścieżki bazowej kodu, które tworzą produktu końcowego. Na początku istnienia rozwoju oprogramowania integracja została wykonana na końcu projektu, które było trudne i ryzykowne procesu.

Ciągłej integracji (CI) należy unikać takich skomplikowane przez scalenie zmian Każdy deweloper wspólnej ścieżki bazowej kodu w sposób ciągły, zazwyczaj, gdy wszystkie deweloperzy sprawdza się zmiany w projekcie udostępnionym repozytorium kodu. Każdy zaewidencjonowania wyzwala automatyczne kompilacji i uruchamia zautomatyzowanych testów, aby sprawdzić, czy nowo wprowadzonych kodu nie Podziel istniejący kod.  W ten sposób CI natychmiast powierzchni błędów i problemów i zapewnia, że wszyscy członkowie zespołu pozostają aktualne z nich pracy. Powoduje to codebase spójności i stabilna.

Systemy ciągłej integracji ma dwie główne części:

-  **Kontrola wersji** — wersji kontroli (VC), nazywany również do kontroli źródła lub zarządzania kodem źródłowym, konsoliduje cały kod projektu w jednym repozytorium udostępnionego i zawiera pełną historię każdej zmiany do każdego pliku. To repozytorium często określany jako *mainline* lub *wzorca* gałęzi, zawiera kod źródłowy, który ostatecznie będzie służyć do kompilacji produkcyjne lub wersji aplikacji. Istnieje wiele typu open source i komercyjnych produktów dla tego zadania, które zwykle pozwalają zespoły lub osób do rozwidlania kopii kodu do dodatkowej oddziałów, w którym można zmieniać szeroką gamę lub prowadzenia eksperymentów bez ryzyka do głównej gałęzi. Po zatwierdzeniu zmian w gałęzi dodatkowej można następnie je wszystkie razem scalony z powrotem do gałęzi głównej.
-  **Ciągła Integracja serwera** — serwer integracji ciągłej jest odpowiedzialny za zbieranie wszystkie artefakty projektu (kod źródłowy, obrazów, klipów wideo, baz danych, zautomatyzowanych testów itp.), Kompilowanie aplikacji i Uruchamianie testów automatycznych. Ponownie istnieje wiele typu open source i narzędzia serwera CI komercyjnego.

Zwykle programiści muszą kopia robocza jedną lub kilka gałęzi na swoich stacjach roboczych, w którym początkowo praca jest wykonywana. Po zakończeniu pracy odpowiedni zestaw zmian "wyewidencjonowany do" lub "zadeklarowane" oddziału odpowiednie propaguje je do kopie robocze z innymi deweloperami. Jest to, jak zespół upewnia się, że wszystkie działają na ten sam kod.

Ponownie ciągłej integracji czynność zatwierdzania zmian powoduje, że serwer elementu konfiguracji skompilować projekt i uruchomić zautomatyzowanych testów, aby sprawdzić poprawność kodu źródłowego. Jeśli występują błędy kompilacji lub testu awarii, serwer CI powiadamia odpowiedzialny developer (w wiadomościach e-mail, wiadomości Błyskawicznych, Twitter, Growl, itp.), użytkownik może rozwiązać problem. (Serwerów CI może nawet odmowy zatwierdzenia Jeśli występują błędy, które jest nazywane "warunkowe zaewidencjonowanie".)

Na poniższym diagramie przedstawiono ten proces:

[![](intro-to-ci-images/intro01-small.png "Ten diagram przedstawia proces")](intro-to-ci-images/intro01.png#lightbox)

Aplikacje mobilne wprowadzenie wyjątkowe wyzwanie dla ciągłej integracji. Aplikacje mogą wymagać czujników takich jak GPS lub aparatu fotograficznego, które są dostępne tylko na fizycznych urządzeniach. Ponadto symulatorów lub emulatory są zbliżenia sprzętu i może użyć w celu zamaskowania lub przesłaniać problemów. Dzięki temu jest niezbędne do testowania aplikacji mobilnej na rzeczywiste sprzęt, aby mieć pewność, że to naprawdę klientowi.

[Test Centrum aplikacji](https://docs.microsoft.com/en-us/appcenter/test-cloud) rozwiązano ten problem określonego przez testowanie aplikacji bezpośrednio na setki urządzeń fizycznych. Deweloperzy zapisu akceptacji zautomatyzowanych testów, umożliwiających wydajne testowania interfejsu użytkownika. Po te testy są przekazywane do Centrum aplikacji, serwer CI można uruchomić je automatycznie w ramach procesu CI jak pokazano na poniższym diagramie:

[![](intro-to-ci-images/intro02-small.png "Po te testy są przekazywane do Centrum aplikacji, serwer CI można uruchomić je automatycznie w ramach procesu CI jak pokazano na tym diagramie")](intro-to-ci-images/intro02.png#lightbox)

# <a name="components-of-continuous-integration"></a>Składniki ciągłej integracji

Brak ekosystem szeroką gamę narzędzi handlowych i open source do obsługi elementu konfiguracji. W tej sekcji opisano kilka najbardziej typowe.

## <a name="version-control"></a>Kontrola wersji

### <a name="visual-studio-team-services-and-team-foundation-server"></a>Team Foundation Server i Visual Studio Team Services

[Visual Studio Team Services](https://www.visualstudio.com/products/visual-studio-team-services-vs) (VSTS) i [Team Foundation Server](http://msdn.microsoft.com/en-us/vstudio/ff637362.aspx) (TFS) są narzędzia współpracy firmy Microsoft dla ciągłej integracji tworzenie usług, śledzenia zadań, planowania elastycznego i raportowania narzędzia i wersji formant. Z kontroli wersji programu VSTS TFS może pracować z własnym systemem (Team Foundation Version Control lub TFVC) lub z projektami w usłudze GitHub.

 - Visual Studio Team Services udostępnia usługi za pośrednictwem chmury. Jego zalet podstawowego jest nie musi podawać dedykowanego sprzętu i infrastruktury i można uzyskać z dowolnego miejsca za pośrednictwem przeglądarki sieci web oraz narzędzia do programowania popularnych takiego jak Visual Studio, dzięki czemu atrakcyjne dla zespołów, które są od siebie lokalizacjach geograficznych rozproszone. Jest bezpłatna dla zespołów deweloperów pięciu lub mniej, po których dodatkowe licencje można zakupić do uwzględnienia rosnącym zespołu.
 - TFS jest przeznaczona dla serwerów systemu Windows w lokalnych i dostępne za pośrednictwem sieci lokalnej lub połączenie sieci VPN do sieci. Jego głównej korzyści jest w pełni kontrolować konfiguracji serwerów kompilacji i można zainstalować niezależnie od dodatkowego oprogramowania lub usług są wymagane. TFS ma bezpłatna wersja Express klasy podstawowej dla małych zespołów.

Zarówno TFS i programu VSTS są ściśle powiązane z programem Visual Studio i umożliwiają deweloperom wykonywać wiele kontroli wersji i zadań elementu konfiguracji z poziomu komfortu pojedynczego IDE. Dostępna jest również wtyczki programu Team Explorer Everywhere dla programu Eclipse (patrz poniżej). Visual Studio for Mac nie oferuje obsługę TFS lub VSTS.

System kompilacji programu Visual Studio Team usługi ma bezpośrednią obsługę projektów Xamarin, w których tworzenie definicji kompilacji dla każdej platformy, które chcesz docelowego (Android, iOS i Windows). Odpowiednią licencję Xamarin jest wymagany dla każdej definicji kompilacji. Istnieje również możliwość nawiązania połączenia lokalnego, serwer do Visual Studio Team Services, w tym celu kompilacji TFS obsługą Xamarin. W przypadku tej konfiguracji kompilacje, które są umieszczane w kolejce programu VSTS zostanie delegowane do serwera lokalnego. Aby uzyskać więcej informacji, zapoznaj się [Wdróż i skonfiguruj serwer kompilacji](https://msdn.microsoft.com/en-us/library/ms181712.aspx). Alternatywnie można użyć innego narzędzia kompilacji, takie jak Wpięć lub Miasto zespołu.

Pełne podsumowanie wszystkich funkcji zarządzania cyklem życia aplikacji (ALM) programu Visual Studio, Visual Studio Team Services i Team Foundation Server, zobacz [zarządzania cyklem życia aplikacji przy użyciu aplikacji Xamarin](https://msdn.microsoft.com/en-us/library/mt162217(v=vs.140).aspx) w witrynie MSDN.


### <a name="team-explorer-everywhere"></a>Team Explorer Everywhere

[Team Explorer Everywhere](http://msdn.microsoft.com/en-us/library/gg413285.aspx) oferuje zespoły rozwoju poza Visual Studio Team Foundation Server i Visual Studio Team Services. Umożliwia deweloperom łączyć się z projektami zespołowymi lokalnie lub w chmurze z Eclipse lub wiersza polecenia obsługujący wiele platform klient OS X i Linux. Programu Team Explorer Everywhere zapewnia pełny dostęp do systemu kontroli wersji (w tym Git) elementów roboczych i kompilacji możliwości dla platform z systemem innym niż Windows.


### <a name="git"></a>Git

[Git](http://git-scm.com) jest rozwiązaniem kontroli wersji popularnych typu open source, które pierwotnie został opracowany, aby zarządzać kod źródłowy dla jądra systemu Linux. Jest to bardzo szybkie, elastyczne system będący popularnych z oprogramowania projektów o różnej wielkości. Jej łatwe może obsłużyć deweloperów jednego z dostępem do Internetu niską duże zespoły rozciągających się na całym świecie. Git powoduje rozgałęzianie bardzo łatwe, który z kolei może zwiększyć liczbę równoległych strumieni programowanie z minimalnym ryzyka.

Git może działać wyłącznie za pośrednictwem przeglądarki sieci web lub za pomocą [klientach graficznego interfejsu użytkownika](http://git-scm.com/downloads/guis) uruchomionymi w systemie Linux, Mac OS x i Windows. Jest bezpłatna dla repozytoriów publicznych; Wymagaj prywatne repozytoria [plan płatnej](https://github.com/pricing).

Visual Studio 2015 i Visual Studio for Mac zapewniają natywną obsługę dla Git; dla starszych wersji, firma Microsoft udostępnia [dostępne do pobrania rozszerzenie Git](http://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c). Jak wspomniano powyżej, Visual Studio Team Services i TFS można użyć narzędzia Git kontroli wersji, zamiast TFVC.


### <a name="subversion"></a>Podwersją

[Podwersją](http://subversion.apache.org) (SVN) to system kontroli wersji popularnych typu open source, które było używane od 2000. SVN działa we wszystkich wersjach nowoczesnych systemu OS X, Windows FreeBSD, Linux i Unix. Visual Studio for Mac ma macierzystą obsługę SVN. Brak rozszerzenia innych firm, których obsługa SVN dla programu Visual Studio.


## <a name="continuous-integration-environments"></a>Środowisk ciągłej integracji

Konfigurowanie środowiska ciągłej integracji oznacza, że połączenie z usługą kompilacji systemu kontroli wersji.  W przypadku drugiego nagłówka dwa najbardziej typowe są:

- [Team Foundation Build](https://msdn.microsoft.com/Library/vs/alm/Build/overview) jest system kompilacji programu Visual Studio Team Services i serwera TFS. Jest ściśle zintegrowany z programem Visual Studio, które umożliwia wygodne dla deweloperów w celu wyzwolenia tworzenia, automatycznie uruchamiać testy i wyświetlić wyniki.
- Wpięć jest serwerem CI open source że w programie rozbudowany ekosystem wtyczek do obsługi wszystkich rodzajów rozwoju oprogramowania. Działa w systemie Windows i Mac OS X. Wpięć nie jest zintegrowany z żadnych szczególnych IDE. Zamiast tego jest on skonfigurowany i zarządzane za pośrednictwem interfejsu sieci web. CI Wpięć jest również łatwo zainstalować i skonfigurować, dzięki czemu mają niewielkich zespołów.

Można użyć programu TFS/VSTS samodzielnie lub Wpięć można używać w połączeniu z programem TFS/VSTS lub Git zgodnie z opisem w poniższych sekcjach.

### <a name="visual-studio-team-services-and-team-foundation-server"></a>Team Foundation Server i Visual Studio Team Services

Zgodnie z opisem, Team Foundation Server i Visual Studio Team Services zapewnia zarówno wersji kontroli oraz tworzenie usług. Usługi kompilacji zawsze wymagają licencji firmowej Xamarin dla każdej platformy docelowej.

Z programu Visual Studio Team Services utwórz definicję kompilacji osobne dla każdej platformy docelowej i wprowadź odpowiednią licencję istnieje. Po skonfigurowaniu programu VSTS uruchomi kompilacji i testy w chmurze. Zobacz [Team Foundation Build](https://msdn.microsoft.com/Library/vs/alm/Build/overview) więcej szczegółów.

Z Team Foundation Server w przypadku skonfigurowania maszyny kompilacji w następujący sposób dla określonych platform:

- **Android i Windows:** zainstalować program Visual Studio i Xamarin narzędzia (dla systemów Android i Windows zarówno) i skonfigurować z licencjami Xamarin. Jest także niezbędne do przeniesienia zestawu SDK systemu Android w udostępnionej lokalizacji na serwerze, gdzie agenta kompilacji TFS można go znaleźć. Aby uzyskać więcej informacji, zobacz [Konfigurowanie TFVC](https://docs.microsoft.com/vsts/tfvc/overview).
- **iOS i Xamarin:** program Visual Studio i Xamarin narzędzi w systemie Windows server z odpowiednią licencją. Następnie zainstalować program Visual Studio dla komputerów Mac na komputerze Mac OS X dostępne w sieci, która będzie służyć jako host kompilacji i tworzenia pakietu ostatecznej aplikacji (IPA dla systemu iOS, aplikacji dla systemu OS X).

Na poniższym diagramie przedstawiono ten topografii:

[![](intro-to-ci-images/intro03-small.png "Ten diagram przedstawia tego topografii")](intro-to-ci-images/intro03.png#lightbox)

Użytkownik może również połączyć z lokalnego serwera TFS z projektu programu Visual Studio Team Services, tak aby kompilacji programu VSTS mają delegowane do serwera lokalnego. Aby uzyskać więcej informacji, zobacz [Wdróż i skonfiguruj serwer kompilacji](http://msdn.microsoft.com/en-us/library/ms181712.aspx) w witrynie MSDN.

### <a name="visual-studio-team-services-and-jenkins"></a>Visual Studio Team Services i Wpięć

Jeśli używasz Wpięć do tworzenia aplikacji, można przechowywać swój kod w Visual Studio Team Services lub program Team Foundation Server i nadal używać Wpięć kompilacji elementu konfiguracji. Można wyzwolić kompilację Wpięć, gdy wypchnąć kod do repozytorium Git lub po zaewidencjonowaniu kodu do TFVC Twój projekt zespołowy. Aby uzyskać więcej informacji, zobacz [Wpięć z Visual Studio Team Services](https://www.visualstudio.com/en-us/docs/marketplace/integrate/service-hooks/services/jenkins).

[![](intro-to-ci-images/intro04-small.png "Jeśli używasz Wpięć do tworzenia aplikacji, można przechowywać swój kod w Visual Studio Team Services lub program Team Foundation Server i nadal używać Wpięć kompilacji elementu konfiguracji")](intro-to-ci-images/intro04.png#lightbox)

### <a name="git-and-jenkins"></a>Usługi Git i Wpięć

Inne typowe środowisko CI może być całkowicie OS X na podstawie. Ten scenariusz polega na użyciu narzędzia Git dla kontroli kodu źródłowego i Wpięć dla serwera kompilacji. Oba te działają na pojedynczym komputerze Mac OS X przy użyciu programu Visual Studio dla komputerów Mac zainstalowane. To jest bardzo podobny do programu Visual Studio Team Services + środowiska Wpięć opisanych w poprzedniej sekcji:

[![](intro-to-ci-images/intro05-small.png "To jest bardzo podobny do programu Visual Studio Team Services + środowiska Wpięć opisanych w poprzedniej sekcji")](intro-to-ci-images/intro05.png#lightbox)

> [!IMPORTANT]
> **Uwaga: Jest Wpięć [nie są obsługiwane przez program Xamarin](~/cross-platform/troubleshooting/questions/xamarin-jenkins.md).**


# <a name="summary"></a>Podsumowanie

Ten dokument wprowadzono pojęcie ciągłej integracji i korzyści, które oferuje zespoły rozwoju oprogramowania. Znaczenie kontroli wersji został omówiony oraz role i obowiązki serwera kompilacji. Omówimy niektóre narzędzia, które można użyć do kontroli kodu źródłowego i serwery kompilacji dokumentu wystąpił. Wprowadzono też Test Centrum aplikacji, który ułatwia deweloperom publikowanie atrakcyjnych aplikacji przez uruchomienie zautomatyzowanych testów, które będzie potwierdzenia jakości i możliwości ich aplikacji. Bardziej szczegółowe dokumentacji na przesyłanie aplikacji i testy do Centrum aplikacji można znaleźć [tutaj](https://docs.microsoft.com/en-us/appcenter/test-cloud). Na koniec aby lepiej poznać sposób tych narzędzi i wszystkie składniki dopasowania, możemy opisane kilka różnych środowiskach elementu konfiguracji, których organizacje mogą ustalić dla ciągłej integracji. Aby uzyskać więcej informacji, za pomocą Team Foundation Server i Visual Studio Team Services z projektami Xamarin, zobacz [Konfigurowanie TFVC](https://docs.microsoft.com/vsts/tfvc/overview) i to [wprowadzenie ciągłej integracji](https://docs.microsoft.com/en-us/vsts/build-release/actions/ci-cd-part-1). Podobnie w przypadku korzystania z Wpięć zobacz [przy użyciu Wpięć za pomocą platformy Xamarin](~/tools/ci/jenkins-walkthrough.md) Aby uzyskać szczegółowe informacje na temat konfigurowania ciągłej integracji.