---
title: Wprowadzenie do ciągłej integracji za pomocą platformy Xamarin
description: W tym dokumencie opisano ciągłej integracji za pomocą platformy Xamarin. Omówiono w nim różnych środowisk ciągłej integracji i kontroli wersji.
ms.prod: xamarin
ms.assetid: C034200E-2947-4309-9DDD-80DAC505C43F
author: lobrien
ms.author: laobri
ms.date: 07/19/2017
ms.openlocfilehash: 5468495885e3af2afa2692ccad9191b669fa3328
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2018
ms.locfileid: "37066510"
---
# <a name="introduction-to-continuous-integration-with-xamarin"></a>Wprowadzenie do ciągłej integracji za pomocą platformy Xamarin

_Ciągła integracja to praktyka inżynierii oprogramowania, w którym zautomatyzowanej kompilacji kompiluje i opcjonalnie testuje aplikację, gdy kod jest dodane lub zmienione przez deweloperów w repozytorium kontroli wersji projektu. W tym artykule przedstawimy ogólnych pojęć ciągłej integracji i niektóre opcje dostępne dla ciągłej integracji z projektami Xamarin._

Są często nad projektami oprogramowania dla deweloperów na równoczesną pracę. W pewnym momencie konieczne zintegrowanie wszystkich z nich jest równoległe strumieni pracy do jednej bazy kodu, które tworzą produktu końcowego. W czasach, kiedy rozwoju oprogramowania tej integracji było wykonywane na końcu projektu, który został proces trudne i ryzykowne.

Ciągłej integracji (CI) uniknąć takich złożoności przez scalenie zmian Każdy deweloper wspólnej bazy kodu w sposób ciągły, zwykle w przypadku, gdy ewidencjonuje wszelkie deweloperów zmiany w projekcie udostępnionego repozytorium kodu. Każdego ewidencjonowania wyzwala automatyczne kompilowanie i uruchamia automatyczne testy, aby sprawdzić, czy nowo wprowadzonego kodu nie przerwać istniejący kod.  W ten sposób CI wydobywa informacje dotyczące problemów i błędów natychmiast i gwarantuje, że wszyscy członkowie zespołu na bieżąco z nich pracy. Skutkuje to spójny i stabilnych bazy kodu.

Systemy ciągłej integracji ma dwie główne części:

- **Kontrola wersji** — wersji kontroli (VC), nazywany również do kontroli źródła lub zarządzania kodem źródłowym, konsoliduje cały kod projektu do pojedynczego repozytorium na udostępnionych i przechowuje pełną historię każdej zmiany do każdego pliku. To repozytorium często określane jako *mainline* lub *wzorca* gałęzi, zawiera kod źródłowy, który ostatecznie będzie służyć do produkcji kompilacji lub wydania wersji aplikacji. Istnieje wiele typu open source i komercyjnych produktów dla tego zadania, które zwykle pozwalają użytkownikom indywidualnym rozwidlenie kopii kodu na pomocniczy gałęzie, gdzie można wprowadzić rozległych zmian lub przeprowadzanie eksperymentów, bez ryzyka do głównej gałęzi lub zespołów. Po zatwierdzeniu zmian w gałęzi dodatkowej można następnie je razem scalana z powrotem do gałęzi głównej.
- **Ciągła Integracja serwera** — serwer ciągłej integracji programu jest odpowiedzialny za zbieranie wszystkich artefaktów projektu (kod źródłowy, obrazów, filmów wideo, baz danych, zautomatyzowanych testów itp.), Kompilowanie aplikacji i Uruchamianie testów automatycznych. Ponownie istnieje wiele "open source", jak i komercyjnego narzędzia serwera ciągłej integracji.

Deweloperzy zazwyczaj mają funkcjonalną kopię jedną lub więcej gałęzi na swoich stacjach roboczych, w którym początkowo praca jest wykonywana. Po zakończeniu odpowiedniego zestawu roboczego zmiany "sprawdzone w" lub "przekazane" do gałęzi odpowiedniego propaguje je do kopie robocze z innymi deweloperami. Jest to, jak zespół zapewnia, że wszystko pracuje ten sam kod.

Ponownie z ciągłą integracją czynność zatwierdzania zmian powoduje, że serwer ciągłej integracji skompilować projekt i uruchomić testy automatyczne, aby sprawdzić poprawność kodu źródłowego. Jeśli występują błędy kompilacji lub niepowodzenia testów, serwer ciągłej integracji powiadamia dewelopera odpowiada (za pośrednictwem poczty e-mail, wiadomości Błyskawicznych, Twitter, Growl, itp.), dzięki czemu użytkownik może rozwiązać ten problem. (Ciągła Integracja serwerów można nawet odmówić zatwierdzenia w przypadku awarii, co jest nazywane "gated check-in".)

Na poniższym diagramie przedstawiono ten proces:

[![](intro-to-ci-images/intro01-small.png "Na tym diagramie przedstawiono ten proces")](intro-to-ci-images/intro01.png#lightbox)

Aplikacje mobilne wprowadzają unikatowe wyzwania w celu zapewnienia ciągłej integracji. Aplikacje mogą wymagać czujników, takich jak GPS lub kamery, które są dostępne tylko na urządzeniach fizycznych. Ponadto symulatory lub emulatorów są tylko przybliżeniem sprzętu i mogą ukrywają lub zasłaniać problemów. Na koniec należy przetestować aplikację mobilną na sprzęt rzeczywisty, aby mieć pewność, jest naprawdę gotowości.

[App Center Test](https://docs.microsoft.com/appcenter/test-cloud) rozwiązuje ten problem określonego przez testowanie aplikacji bezpośrednio na setkach urządzeń fizycznych. Programiści pisać akceptacje zautomatyzowanych testów, które umożliwiają testowanie interfejsu użytkownika zaawansowane. Gdy te testy są przekazywane do usługi App Center, serwer ciągłej integracji mogły być uruchamiane automatycznie jako część procesu ciągłej integracji jak pokazano na poniższym diagramie:

[![](intro-to-ci-images/intro02-small.png "Gdy te testy są przekazywane do usługi App Center, serwer ciągłej integracji mogły być uruchamiane automatycznie jako część procesu ciągłej integracji jak pokazano na poniższym diagramie")](intro-to-ci-images/intro02.png#lightbox)

# <a name="components-of-continuous-integration"></a>Składniki integracji ciągłej

Rozbudowany ekosystem narzędzi komercyjnych i open-source przeznaczony do obsługi ciągłej integracji nie istnieje. W tej sekcji opisano niektóre z najbardziej typowymi.

## <a name="version-control"></a>Kontrola wersji

### <a name="azure-devops-and-team-foundation-server"></a>Usługa Azure DevOps i Team Foundation Server

[Usługa Azure DevOps](https://azure.microsoft.com/services/devops/) i [Team Foundation Server](https://visualstudio.microsoft.com/tfs/) (TFS) są narzędzia współpracy firmy Microsoft w celu zapewnienia ciągłej integracji kompilacji usług, do śledzenia zadań, zwinnego planowania i raportowania, narzędzi i kontroli wersji. Z kontrolą wersji DevOps platformy Azure i TFS może pracować z własnym systemem (Team Foundation Version Control lub TFVC) lub z projektami w serwisie GitHub.

- Visual Studio Team Services udostępnia usługi za pośrednictwem chmury. Jej główna zaleta modelu jest nie wymaga dedykowanego sprzętu ani infrastruktury i są dostępne z dowolnego miejsca za pośrednictwem przeglądarki sieci web oraz popularnymi narzędziami deweloperskimi takich jak Visual Studio, dzięki czemu atrakcyjne dla zespołów, które są geograficznie rozproszone. Jest bezpłatna dla zespołów pięciu deweloperów lub mniej, po które dodatkowe licencje można nabyć w celu uwzględnienia zespół rozrasta się.
- TFS jest przeznaczony do serwerów Windows w środowisku lokalnym i dostępne za pośrednictwem sieci lokalnej lub połączenie sieci VPN do tej sieci. Jej główna zaleta modelu jest w pełni kontrolować konfiguracji serwerów kompilacji i można zainstalować niezależnie od dodatkowego oprogramowania lub usług są wymagane. Serwer TFS ma wolne, klasy podstawowej wersji Express dla małych zespołów.

TFS i DevOps platformy Azure są ściśle zintegrowane z programem Visual Studio, a deweloperzy mogą wykonywać wiele kontroli wersji i zadań ciągłej integracji przy użyciu w ramach przy użyciu jednego środowiska IDE. Dostępna jest również wtyczki Team Explorer Everywhere dla środowiska Eclipse (patrz poniżej). Program Visual Studio for Mac [Podgląd TFVC dostępne](/visualstudio/mac/tf-version-control/).

[Usługa Azure potoków metodyki DevOps](https://docs.microsoft.com/azure/devops/pipelines/languages/xamarin/) ma bezpośrednią obsługę projekty Xamarin, w których utworzyć definicję kompilacji, dla każdej platformy, które chcesz docelowy (systemy Android, iOS i Windows). Odpowiednią licencję platformy Xamarin jest wymagana dla każdej definicji kompilacji. Istnieje również możliwość nawiązywania połączeń z lokalnym, obsługą platformy Xamarin w programie TFS kompilacji serwera DevOps platformy Azure, w tym celu. Za pomocą tego Instalatora kompilacje, które są umieszczane w kolejce do metodyki DevOps platformy Azure będzie można przekazać do serwera lokalnego. Aby uzyskać szczegółowe informacje, zapoznaj się [Build and release agents i](https://docs.microsoft.com/azure/devops/pipelines/agents/agents). Alternatywnie można użyć innego narzędzia kompilacji, takich jak Jenkins lub Miasto zespołu.

Pełne podsumowanie wszystkich funkcji zarządzania cyklem życia aplikacji (ALM) programu Visual Studio, DevOps platformy Azure i Team Foundation Server, zobacz [DevOps przy użyciu aplikacji Xamarin](https://docs.microsoft.com/visualstudio/cross-platform/application-lifecycle-management-alm-with-xamarin-apps).

### <a name="team-explorer-everywhere"></a>Team Explorer Everywhere

[Team Explorer Everywhere](https://docs.microsoft.com/azure/devops/java/download-eclipse-plug-in/) oferuje funkcje programu Team Foundation Server i Visual Studio Team Services zespołom tworzącym aplikacje poza programem Visual Studio. Dzięki niej deweloperzy mogą połączyć się z projektami zespołowymi lokalnie lub w chmurze z programu Eclipse lub z klienta wiersza polecenia dla wielu platform for OS X i Linux. Team Explorer Everywhere, który zapewnia pełny dostęp do elementów roboczych do kontroli wersji (w tym Git) i możliwości kompilacji na platformy inne niż Windows.

### <a name="git"></a>Git

[Git](http://git-scm.com) jest rozwiązaniem kontroli wersji typu open source, która pierwotnie został opracowany, aby zarządzać kodem źródłowym jądra systemu Linux. Jest to bardzo szybki, elastyczny system, który popularnych z projektami oprogramowania wszystkich rozmiarów. Łatwo można skalować z jednego deweloperom niską dostęp do Internetu dla dużych zespołów, które rozciągają się świecie. Usługi Git sprawia, że bardzo łatwa w użyciu, rozgałęzianie, który z kolei może zachęcać strumieni równoległego programowania przy użyciu minimalnego ryzyka.

Git może działać wyłącznie za pośrednictwem przeglądarki sieci web lub za pomocą [klientach GUI](http://git-scm.com/downloads/guis) uruchamianą w systemie Linux, Mac OSX i Windows. Bezpłatnie w przypadku publicznych repozytoriów; prywatne repozytoria wymagają [płatny plan](https://github.com/pricing).

Visual Studio 2015 i Visual Studio for Mac oferuje natywnej obsługi usługi Git; w przypadku starszych wersji, firma Microsoft udostępnia [dostępne do pobrania rozszerzenie Git](http://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c). Jak wspomniano powyżej, Visual Studio Team Services i serwera TFS można wykorzystać Git do kontroli wersji, zamiast w TFVC.

### <a name="subversion"></a>Subversion

[Subversion](http://subversion.apache.org) (SVN) to system kontroli wersji popularny typu open source, które było używane od 2000. SVN działa we wszystkich wersjach nowoczesnego systemu OS X, Windows, FreeBSD, Linux i Unix. Program Visual Studio for Mac zapewnia natywną obsługę SVN. Brak rozszerzenia innych firm, które łączą SVN pomocy technicznej programu Visual Studio.

## <a name="continuous-integration-environments"></a>Ciągła Integracja środowiska

Konfigurowanie środowiska ciągła integracja oznacza, że łączenie system kontroli wersji przy użyciu usługi kompilacji.  W przypadku drugiego nagłówka, dwie najbardziej typowymi są:

- [Potoki usługi Azure](https://docs.microsoft.com/azure/devops/pipelines/) to system kompilacji DevOps platformy Azure i programu TFS. Jest ściśle zintegrowana z programem Visual Studio, wygodne deweloperom na wyzwalanie zbudował, co sprawia, że automatyczne testy i wyświetlić wyniki.
- Jenkins to serwer ciągłej integracji typu open source, wraz z bogatego ekosystemu wtyczek umożliwiających wszelkiego rodzaju rozwoju oprogramowania. Działa na Windows i Mac OS X. Jenkins nie jest zintegrowany z dowolnym określonym środowisku IDE. Zamiast tego jest konfigurowane i zarządzane za pośrednictwem interfejsu sieci web. Jenkins to również łatwo zainstalować i skonfigurować, co czyni go atrakcyjne dla małych zespołów.

Można użyć DevOps programu TFS/platformy Azure przez siebie lub Jenkins można używać w połączeniu z DevOps programu TFS/platformy Azure lub program Git, zgodnie z opisem w poniższych sekcjach.

### <a name="visual-studio-team-services-and-team-foundation-server"></a>Visual Studio Team Services i serwera Team Foundation Server

Zgodnie z opisem, Visual Studio Team Services i serwera Team Foundation Server zawiera zarówno wersji kontroli i usługi kompilacji. Usługi kompilacji zawsze wymagają licencji Xamarin Business lub Enterprise dla każdej platformy docelowej.

Za pomocą programu Visual Studio Team Services utwórz definicję kompilacji osobne dla każdej platformy docelowej i wprowadź ma odpowiednią licencję. Gdy zostanie skonfigurowany, usługa Azure DevOps run kompiluje i testuje w chmurze. Zobacz [potoki Azure](https://docs.microsoft.com/azure/devops/pipelines/) Aby uzyskać więcej informacji.

Za pomocą Team Foundation Server należy skonfigurować maszynę kompilacji w następujący sposób dla określonych platform:

- **Dla systemu android i Windows:** Zainstaluj program Visual Studio i Xamarin narzędzi (dla systemów Android i Windows zarówno) i skonfigurować swoje licencje środowiska Xamarin. Należy również przeniesienia zestawu SDK systemu Android w udostępnionej lokalizacji na serwerze, gdzie agenta kompilacji programu TFS, mogą ją odnaleźć. Aby uzyskać więcej informacji, zobacz [Konfigurowanie TFVC](https://docs.microsoft.com/azure/devops/repos/tfvc/overview).
- **iOS i platformy Xamarin:** Zainstaluj program Visual Studio i narzędzia środowiska Xamarin na serwerze Windows z odpowiednią licencją. Następnie należy zainstalować program Visual Studio dla komputerów Mac, na komputerze Mac OS X dostępne w sieci, który będzie pełnić rolę hosta kompilacji i tworzenie pakietu ostatecznej aplikacji (IPA dla systemów iOS, aplikacji dla systemu OS X).

Na poniższym diagramie przedstawiono ten topografii:

[![](intro-to-ci-images/intro03-small.png "Na tym diagramie przedstawiono ten topografii")](intro-to-ci-images/intro03.png#lightbox)

Istnieje również możliwość łączenie lokalnego serwera TFS do programu Visual Studio Team Services projektu tak, aby kompilacje DevOps platformy Azure są delegowane do serwera lokalnego. Aby uzyskać więcej informacji, zobacz [Build and release agents i](https://docs.microsoft.com/azure/devops/pipelines/agents/agents/).

### <a name="visual-studio-team-services-and-jenkins"></a>Visual Studio Team Services i Jenkins

Jeśli używasz narzędzia Jenkins do tworzenia aplikacji, można zapisać kod w Visual Studio Team Services lub serwera Team Foundation Server i kontynuować używanie narzędzia Jenkins dla usługi kompilacji ciągłej integracji. Podczas wypychania kodu do repozytorium Git projektu zespołowego lub gdy zaewidencjonujesz kod w celu TFVC, możesz wyzwolić kompilację narzędzia Jenkins. Aby uzyskać więcej informacji, zobacz [Jenkins przy użyciu usługi Azure DevOps](https://docs.microsoft.com/azure/devops/service-hooks/services/jenkins).

[![](intro-to-ci-images/intro04-small.png "Jeśli używasz narzędzia Jenkins do tworzenia aplikacji, można zapisać kod w Visual Studio Team Services lub serwera Team Foundation Server i kontynuować używanie narzędzia Jenkins dla usługi kompilacji ciągłej integracji")](intro-to-ci-images/intro04.png#lightbox)

### <a name="git-and-jenkins"></a>Git i Jenkins

Inne typowe środowisko ciągłej integracji może być całkowicie OS X na podstawie. Ten scenariusz obejmuje przy użyciu narzędzia Git do kontroli kodu źródłowego i serwera Jenkins na serwerze kompilacji. Oba te są uruchomione na jednym komputerze Mac OS X z programem Visual Studio dla komputerów Mac jest zainstalowany. Jest to bardzo podobne do Visual Studio Team Services i środowiska narzędzia Jenkins, opisanych w poprzedniej sekcji:

[![](intro-to-ci-images/intro05-small.png "Jest to bardzo podobne do Visual Studio Team Services i środowiska narzędzia Jenkins, opisanych w poprzedniej sekcji")](intro-to-ci-images/intro05.png#lightbox)

> [!IMPORTANT]
> **Jenkins to [nie są obsługiwane przez firmę Microsoft](~/cross-platform/troubleshooting/questions/xamarin-jenkins.md).**

# <a name="summary"></a>Podsumowanie

Ten dokument wprowadza pojęcia ciągłej integracji i korzyści, jakie oferuje zespołom programistycznym. Znaczenie kontroli wersji został omówiony oraz role i obowiązki serwera kompilacji. W celu omówienia ich niektóre z narzędzi, które można używać w kontroli kodu źródłowego i serwery kompilacji dokumentu wystąpił. Wprowadzono też App Center Test, który pomaga deweloperom Opublikuj wspaniałych aplikacji, uruchamiania testów automatycznych, które dowiedzie jakości i możliwości swoich aplikacji. Więcej szczegółowa dokumentacja dotycząca przesyłania aplikacji i testów do Centrum aplikacji można znaleźć [tutaj](https://docs.microsoft.com/appcenter/test-cloud). Na koniec aby lepiej zrozumieć, jak te narzędzia i składniki współdziałają ze sobą, firma Microsoft opisano kilka różnych środowiskach ciągłej integracji, które organizacje mogą nawiązują do ciągłej integracji. Aby uzyskać więcej informacji przy użyciu programu Visual Studio Team Services i serwera Team Foundation Server z projekty Xamarin, zobacz [Konfigurowanie TFVC](https://docs.microsoft.com/azure/devops/repos/tfvc/overview/) to [wprowadzenie ciągłej integracji](https://docs.microsoft.com/azure/devops/pipelines/get-started-designer/). Podobnie, jeśli używasz narzędzia Jenkins, zobacz [przy użyciu narzędzia Jenkins z platformą Xamarin](~/tools/ci/jenkins-walkthrough.md) Aby uzyskać szczegółowe informacje na temat konfigurowania ciągłej integracji.
