---
title: Wprowadzenie do cyklu programistycznym oprogramowanie dla urządzeń przenośnych
description: W tym artykule omówiono cyklu programistycznym oprogramowania w odniesieniu do aplikacji dla urządzeń przenośnych i omówiono niektóre kwestie wymagane podczas kompilowania projekty przenośnych. Deweloperzy chcący wystarczy przejść bezpośrednio w i rozpocząć tworzenie w tym przewodniku można pominięty i przeczytać później, aby uzyskać bardziej szczegółowy opis rozwoju przenośnych.
ms.prod: xamarin
ms.assetid: 420c5fdf-4610-4e71-9db5-fe894c961924
author: asb3993
ms.author: amburns
ms.date: 11/22/2016
ms.openlocfilehash: c93a063c9c933e1b9f397d172115471473cf8f35
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="introduction-to-the-mobile-software-development-lifecycle"></a>Wprowadzenie do cyklu programistycznym oprogramowanie dla urządzeń przenośnych

_W tym artykule omówiono cyklu programistycznym oprogramowania w odniesieniu do aplikacji dla urządzeń przenośnych i omówiono niektóre kwestie wymagane podczas kompilowania projekty przenośnych. Deweloperzy chcący wystarczy przejść bezpośrednio w i rozpocząć tworzenie w tym przewodniku można pominięty i przeczytać później, aby uzyskać bardziej szczegółowy opis rozwoju przenośnych._

Tworzenie aplikacji dla urządzeń przenośnych można w prosty jako otwarcia IDE, wyrzucanie coś ze sobą, podczas szybkiego bit testowania i przesyłanie do sklepu z aplikacjami — wszystkie wykonane w popołudnie. Lub może być bardzo zaangażowany procesu, który obejmuje rygorystyczne projektowania początkowych użyteczność, testowania i testują rozwiązania na tysiące urządzeń, beta pełny cykl życia, a następnie wdrożenia na kilka różnych sposobów pytań i odpowiedzi.

W tym dokumencie chcemy się zająć zbadania wprowadzające tworzenia aplikacji dla urządzeń przenośnych, w tym:

1.   **Proces** — proces tworzenia oprogramowania jest nazywany cyklu tworzenia oprogramowania (SDLC). Zajmiemy się wszystkich faz SDLC względem rozwoju aplikacji mobilnej, w tym: Inspirującymi, projekt programowanie, stabilizacji, wdrażania i konserwacji.
1.   **Zagadnienia dotyczące** — istnieją pewne kwestie podczas tworzenia aplikacji dla urządzeń przenośnych, szczególnie w przeciwieństwie do tradycyjnych sieci web lub aplikacji klasycznych. Zajmiemy się powyższe zagadnienia i ich wpływ na projektowanie przenośnych.

Ten dokument jest przeznaczony do odpowiedzi na podstawowe pytania dotyczące tworzenia aplikacji mobilnych, dla deweloperów aplikacji nowych i doświadczonym podobne. Trwa dość wyczerpującą wprowadzenie większość pojęcia, które będą uruchamiane w podczas cały cykl życia rozwoju oprogramowania (SDLC). Ten dokument nie może być dla wszystkich użytkowników, jeśli jest itching można po prostu zacznij tworzenie aplikacji firma Microsoft zaleca jednak przeskok w przód do [wprowadzenie do programowania aplikacji mobilnych](~/cross-platform/get-started/introduction-to-mobile-development.md) guide, a następnie powracające do tego dokumentu później.

## <a name="mobile-development-sdlc"></a>Mobile Development SDLC

Cykl życia przenośnych programowanie przede wszystkim nie różni się od SDLC dla aplikacji sieci web lub pulpitu. Podobnie jak w przypadku, są zazwyczaj 5 głównych części procesu:

1.   **Incepcja** — wszystkie aplikacje rozpoczynać pogląd. Czy rozwiązaniem jest zwykle wprowadzono ulepszenia w solidną podstawę dla aplikacji.
1.   **Projekt** — na etapie projektowania składa się z definiujący środowisko użytkownika aplikacji (UX) np. jakie Układ ogólny, jak to działa, itp., a także włączenie tego środowiska użytkownika do prawidłowego projektu interfejsu użytkownika (UI), zwykle za pomocą projektanta grafiki.
1.   **Programowanie** — zazwyczaj najwięcej zasobów znacznym fazy, jest to rzeczywiste tworzenie aplikacji.
1.   **Stabilizacji** — Jeśli programowanie jest wystarczający wraz, pytań i odpowiedzi rozpoczyna się zwykle do testowania aplikacji i usunięto usterek. Często razy aplikacji zostaną umieszczone w fazie ograniczone w wersji beta, w którym szersze odbiorców użytkownika znajduje się możliwość użycia go i wyrazić swoją opinię i poinformować o zmiany.
1.  **Wdrażanie**

Często wiele z tych elementów jest nakładających się, na przykład jest typowe dla rozwoju przejściem podczas zatwierdzony interfejsu użytkownika, a nawet informuje projektu interfejsu użytkownika. Ponadto aplikacji mogą być kierowane do fazę stabilizacji na to, że nowe funkcje są dodawane do nowej wersji.

Ponadto fazy te można w dowolnej liczbie metodologii SDLC, takie jak Agile, Spirala, wykresu kaskadowego itp.

Każdy z tych fazach będzie można wyjaśnić bardziej szczegółowo w następujących sekcjach.

### <a name="inception"></a>Incepcja

Powszechności i poziom interakcji czytelników o urządzeniach przenośnych oznacza, że niemal każdy ma pomysł dla aplikacji mobilnej. Urządzenia przenośne otwarcie zupełnie nowy sposób na interakcję z obliczeniowych, sieci web, a nawet firmowej infrastruktury.

Na etapie incepcja jest zdefiniowanie i uściślenie rozwiązaniem dla aplikacji.
Tworzenie aplikacji powiodło się, jest pytany o niektórych podstawowych. Poniżej przedstawiono niektóre czynności, które należy uwzględnić przed opublikowaniem aplikacji w jednym z publicznego sklepów z aplikacjami:

-   **Przewagi konkurencyjnej** — są podobne aplikacje tam już? Jeśli tak, jak tej aplikacji odróżnienia jej od innych użytkowników?

Dla aplikacji rozproszonych w przedsiębiorstwie:

-   **Integracja infrastruktury** — istniejącą infrastrukturę będzie go zintegrować z lub rozszerzyć?

Ponadto aplikacje powinny być oceniane w kontekście wygląd formularza przenośnych:

-   **Wartość** — jakie korzyści tej aplikacji można osiągać użytkowników? Jak będą one go używać?
-   **Formularz/mobilności** — jak ta aplikacja działa w obudowie przenośnych? Jak dodać wartość przy użyciu technologii mobilnych Rozpoznawanie lokalizacji, aparatu fotograficznego, np.?

Aby ułatwić projektowanie funkcji aplikacji, jest przydatny do definiowania złośliwych użytkowników i [przypadki użycia](http://en.wikipedia.org/wiki/Use_case). Podmioty są role w aplikacji i często użytkowników. Przypadki użycia zazwyczaj są to akcje lub lokalizacji docelowych.

Na przykład zadanie śledzące aplikacji może mieć dwóch osób: *użytkownika* i *Friend*. Użytkownik może *utworzyć zadanie*, i *udostępnić zadania* znajomym. W takim przypadku zadania tworzenia i udostępniania zadania są dwa różne przypadki użycia, w połączeniu z uczestników, poinformuje, co możesz ekrany trzeba kompilacji, a także jednostek biznesowych i logiki będzie potrzebny opracowywane.

Po zarejestrowaniu odpowiednią liczbę przypadków użycia i uczestników, jest znacznie ułatwia rozpoczęcie projektowania aplikacji. Programowanie może następnie skoncentrować się na sposób tworzenia aplikacji, zamiast co lub aplikacji należy wykonać.

### <a name="designing-mobile-applications"></a>Projektowanie aplikacji dla urządzeń przenośnych

Po określeniu funkcji i funkcjonalności aplikacji, następnym krokiem jest start próby rozwiązania środowiska użytkownika lub UX.

#### <a name="ux-design"></a>Projektowanie interfejsu użytkownika

UX zwykle odbywa się za pośrednictwem wireframes lub makiety przy użyciu jednej z wielu [projektowania narzędzi](https://docs.microsoft.com/windows/uwp/design/downloads/). Zezwalaj na makiety UX UX projektowane, nie martwiąc się o rzeczywisty projekt interfejsu użytkownika:

 [![](introduction-to-mobile-sdlc-images/balsamiq.png "UX zwykle odbywa się za pośrednictwem wireframes lub makiety za pomocą narzędzi, takich jak Balsamiq")](introduction-to-mobile-sdlc-images/balsamiq.png#lightbox)

Podczas tworzenia makiety środowiska użytkownika, należy wziąć pod uwagę wytyczne interfejs dla różnych platform, których aplikacja będzie obowiązywać. Aplikacja powinna "możesz w domu" na każdej z platform. Oficjalna wskazówek dla każdej platformy są:

1.   **Apple** -  [Human Interface Guidelines](https://developer.apple.com/ios/human-interface-guidelines/overview/themes/)
1.   **Android** — [wskazówki dotyczące projektowania](http://developer.android.com/design/index.html)
1.   **Platformy uniwersalnej systemu Windows** — [podstawowe informacje o projekcie platformy UWP](https://docs.microsoft.com/windows/uwp/design/basics/)

Na przykład każda aplikacja ma metaphor do przełączania między częściami w aplikacji. iOS używa pasek karty u dołu ekranu, Android używa pasek karty w górnej części ekranu i platformy uniwersalnej systemu Windows używa [Pivot lub kartę](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/tabs-pivot) widoku.

Ponadto samego sprzętu decyduje również decyzji dotyczących środowiska użytkownika. Na przykład urządzenia z systemem iOS mieć nie fizycznych *ponownie* przycisk i wprowadzić metaphor kontrolera nawigacji:

 ![](introduction-to-mobile-sdlc-images/01-navigation-controller.png "urządzenia z systemem iOS mieć nie fizycznych przycisk Wstecz i wprowadzić metaphor kontrolera nawigacji")

Ponadto obudowa wpływa także decyzje środowiska użytkownika. Tabletem ma dużo więcej nieruchomości, a więc można wyświetlić więcej informacji. Często co wymaga wielu ekrany na telefonie jest skompresowany do jednego na komputerze typu tablet:

 [![](introduction-to-mobile-sdlc-images/iphone-vs-ipad.png "Często co wymaga wielu ekrany na telefonie jest skompresowany do jednego dla tabletu")](introduction-to-mobile-sdlc-images/iphone-vs-ipad.png#lightbox)

I z powodu różnych rozmiarach tam istnieją często średnich rozmiarach (między Telefon i tablet), które mogą również chcesz przeanalizować.

#### <a name="user-interface-ui-design"></a>Projektowanie interfejsu użytkownika

Po określeniu środowiska użytkownika następnym krokiem jest utworzenie projektu interfejsu użytkownika. Podczas środowiska użytkownika jest zazwyczaj właśnie i czarnych makiety, na etapie projektowania interfejsu użytkownika jest gdzie kolory, grafiki, itp., zostaną wprowadzone i sfinalizowany. Ważne jest poświęcany czas na dobrej projektu interfejsu użytkownika i ogólnie rzecz biorąc, najbardziej popularne aplikacje mają profesjonalnego projektu.

Zgodnie z UX, ważne jest, aby zrozumieć, że każdej z platform został jest języka własny projekt, tak dobrze zaprojektowanej aplikacji mogą nadal różnić się na każdej platformie:

 [![](introduction-to-mobile-sdlc-images/multiplatform-1.png "Dobrze zaprojektowanej aplikacji nadal mogą wyglądać inaczej w każdej z platform")](introduction-to-mobile-sdlc-images/multiplatform-1.png#lightbox)

### <a name="development"></a>Tworzenie

Na etapie projektowania zazwyczaj rozpoczyna się bardzo wcześniej. W rzeczywistości pomysł ma niektórych dojrzewania w fazie koncepcyjnej/inspirującymi, często prototypu pracy jest opracowany sprawdzania funkcjonalności, założenia i pozwala podać opis zakresu prac.

W pozostałej części samouczków firma Microsoft będzie skupić się przede wszystkim na etapie projektowania.

### <a name="stabilization"></a>Stabilizacji

Stabilizacji to proces wypracowania usterki w aplikacji. Nie tylko z punktu widzenia funkcjonalne, np.: "Uległa awarii po kliknięciu tego przycisku" ale również użyteczność i wydajność. Zaleca się uruchomić stabilizacji bardzo wcześniej w ramach procesu tworzenia korekt kursu może wystąpić, zanim staną się kosztowne. Zazwyczaj aplikacji przejdź do *prototypu*, *alfa*, *Beta*, i *Release Candidate* etapach. Zdefiniuj te inaczej różne osoby, ale ogólnie zgodne następującego wzorca.

1.   **Prototyp** — aplikacja jest nadal w fazie Weryfikacja koncepcji i tylko podstawowych funkcji lub pracy określonych części aplikacji. Występują błędy głównych.
1.   **Alpha** — podstawowe funkcje jest zwykle obejmuje kodu (skompilowane, ale nie w pełni przetestowane). Usterki główne są nadal dostępne, odległe mniejsze funkcje mogą nadal być obecny.
1.   **W wersji beta** — większość funkcji zostało ukończone i miała co najmniej światła testowania i naprawienie błędów. Główne znanych problemów może nadal być obecny.
1.   **Wersja Release Candidate** — wszystkie funkcje są kompletne i przetestowane. Jeśli nie wystąpi nowych usterek aplikacji jest kandydatem do wersji symbole.

Nigdy nie może być zbyt wcześnie ma rozpocząć testowanie aplikacji. Na przykład jeśli problem głównych znajduje się w fazie prototyp, UX aplikacji nadal można zmodyfikować, aby je obsługiwać. Jeśli problem z wydajnością znajduje się w fazie alfa, to wczesne, aby zmodyfikować architektury przed dużej ilości kodu został utworzony na podstawie założenia false.

Zwykle, gdy aplikacja przechodzi dalej w tym samym w cyklu życia zostało otwarte na większej liczbie użytkowników, aby wypróbować jej możliwości, przetestować go, podaj opinii itp. Na przykład aplikacje prototypu może tylko być widoczny, czy udostępnione najważniejszych uczestników, podczas gdy release candidate aplikacje mogą być dystrybuowane do klientów, którzy Załóż wczesny dostęp.

Do wczesnego testowania i wdrażania na urządzeniach względnie mało zwykle wdrażanie bezpośrednio na komputerze programowanie jest wystarczająca. Jednak ponieważ odbiorców rozszerzenie, to może szybko stać się skomplikowane. Tak istnieje wiele testów opcje wdrażania tam powodujących, że ten proces ułatwi, umożliwiając Zaproś inne osoby do testowania puli, wersja kompilacje w sieci web i zapewniają narzędzia, która pozwala na opinie użytkowników.

W przypadku testowania i wdrażania, można użyć [Centrum aplikacji](https://appcenter.ms/) do stale tworzenia, testowania, wersji i monitorować aplikacje.

### <a name="distribution"></a>Dystrybucji

Gdy aplikacja uzyska, nadszedł czas na wylogowanie pobrać go do środowiska naturalnego. Istnieje wiele opcji dystrybucji różne, w zależności od platformy.

#### <a name="ios"></a>iOS

Xamarin.iOS i aplikacji w języku Objective C są dystrybuowane w taki sam sposób:

1.   **Apple App Store** — firmy Apple App Store to repozytorium dostępnego globalnie aplikacji online wbudowanego w systemie Mac OS X za pomocą programu iTunes. Jest on zdecydowanie najpopularniejszych metoda dystrybucji dla aplikacji i umożliwia deweloperom rynku i dystrybucja aplikacji online z bardzo małego wysiłku.
1.   **Wdrażanie wewnętrznych** — wewnętrzna wdrożenia jest przeznaczona dla wewnętrznego dystrybucja aplikacji firmowych, które nie są dostępne publicznie za pośrednictwem sklepu z aplikacjami.
1.   **Wdrażanie AD Hoc** — wdrażanie Ad hoc jest przeznaczony głównie do projektowania i testowania i umożliwia wdrożenie do ograniczonej liczby urządzeń prawidłowo zainicjowana. Podczas wdrażania na urządzeniu za pomocą środowiska Xcode lub Visual Studio for Mac nazywa wdrożenia ad hoc.

#### <a name="android"></a>Android

Wszystkie aplikacje systemu Android muszą być podpisane przed dystrybucji. Deweloperzy swoje aplikacje są podpisane za pomocą ich własnych certyfikatów chroniony przez klucz prywatny. Ten certyfikat może zapewnić łańcuch autentyczności łączący Deweloper aplikacji dla aplikacji, które dewelopera ma wbudowane i wydane.
Należy zauważyć, że podczas tworzenia certyfikatu dla systemu Android mogą być podpisane przez zaufanego urzędu certyfikacji, większość deweloperów nie zdecydować się na korzystanie z tych usług i samodzielnie zarejestrować swoje certyfikaty. Głównym celem certyfikatów jest do rozróżniania różnych deweloperów i aplikacji.
Android wykorzystuje te informacje pomagające wymuszania delegowanie uprawnień między aplikacjami i składniki działające w ramach systemu operacyjnego Android.

W przeciwieństwie do innych popularnych platform urządzeń przenośnych Android trwa bardzo Otwórz podejście dystrybucji aplikacji. Urządzenia nie są zablokowane do magazynu jednego, zatwierdzonych aplikacji.
Zamiast tego każdy użytkownik będzie mógł utworzyć sklepu z aplikacjami i najbardziej telefony Zezwalaj na użycie aplikacji do zainstalowania z tymi sklepami innych firm.

Dzięki temu deweloperzy mogą potencjalnie większego jeszcze bardziej złożonych kanału dystrybucji dla swoich aplikacji. [Google Play](https://play.google.com/store?hl=en) jest oficjalnego sklepu firmy Google, ale w przypadku wielu innych. Kilka najpowszechniejsze są:

1.  [AppBrain](http://www.appbrain.com/)
1.  [Amazon sklepu z aplikacjami dla systemu Android](http://www.amazon.com/mobile-apps/b?ie=UTF8&amp;node=2350149011)
1.  [Handango](http://www.handango.com/)
1.  [GetJar](http://www.getjar.com/)

#### <a name="uwp"></a>Platforma UWP 

Aplikacji platformy uniwersalnej systemu Windows są dystrybuowane do użytkowników za pośrednictwem Microsoft Store. Deweloperzy przesłać swoje aplikacje do zatwierdzenia, po którym są wyświetlane w magazynie. Aby uzyskać więcej informacji na temat publikowania aplikacji systemu Windows, zobacz dla platformy UWP [publikowania](https://docs.microsoft.com/windows/uwp/publish/) dokumentacji.

## <a name="mobile-development-considerations"></a>Zagadnienia dotyczące Mobile Development

Podczas opracowywania aplikacji dla urządzeń przenośnych nie różni się rozwoju tradycyjnej sieci web/pulpitu pod względem procesu lub architektura, istnieją pewne kwestie, którymi należy się zapoznać.

### <a name="common-considerations"></a>Typowe kwestie wymagające rozważenia

#### <a name="multitasking"></a>Wielozadaniowości

Istnieją dwa istotne wyzwanie wielozadaniowości (o wielu aplikacji uruchamianych na raz) na urządzeniu przenośnym. Po pierwsze podana nieruchomości ograniczone ekranu, trudno jest wyświetlić jednocześnie wiele aplikacji. W związku z tym na urządzeniach przenośnych tylko jednej aplikacji może być na pierwszym planie, w tym samym czasie. Po drugie o wielu aplikacji, które otwarte i wykonywanie zadań szybko korzystać z baterii.

Każdej platformy obsługuje wielozadaniowości inny sposób, który firma Microsoft będzie Eksplorowanie w bit.

#### <a name="form-factor"></a>Wygląd formularza

Urządzenia przenośne zwykle można podzielić na dwie kategorie, telefony i tablety, z kilka urządzeń skrzyżowanego między nimi. Tworzenie aplikacji dla tych rozmiarach jest zazwyczaj bardzo podobne, ale projektowania aplikacji pod kątem ich może być bardzo różnią się.
Telefony być bardzo ograniczoną przestrzeń na ekranie i tabletów, podczas gdy większe, są urządzenia przenośne nadal z mniej miejsca na ekranie niż nawet większość komputerów przenośnych. W związku z tym kontrolek interfejsu użytkownika platform przenośnych zostały zaprojektowane specjalnie, aby były skuteczne w mniejszych rozmiarach.

#### <a name="device-and-os-fragmentation"></a>Urządzenia i systemu operacyjnego fragmentacji

Należy wziąć pod uwagę różnych urządzeń w całym cyklu programistycznym całego oprogramowania:

1.   **Conceptualization i planowanie** — należy pamiętać, że sprzęt i funkcje będą się różnić z urządzenia do urządzenia, aplikacji, która opiera się na niektóre funkcje mogą nie działać poprawnie na niektórych urządzeniach. Na przykład, nie wszystkie urządzenia mają kamery, więc jeśli tworzysz wideo, aplikacja do obsługi wiadomości, niektóre urządzenia może istnieć możliwość odtwarzania wideo, ale nie mieć ich.
1.   **Projekt** — podczas projektowania środowisko użytkownika aplikacji (UX), zwrócić uwagę rozmiary różnych urządzeń i różnych współczynników proporcji ekranu. Ponadto podczas projektowania aplikacji interfejsu użytkownika (UI), należy rozważyć różnych rozdzielczości ekranu.
1.   **Programowanie** — po przy użyciu funkcji z kodu, obecności tej funkcji należy zawsze przetestować najpierw. Na przykład przed użyciem funkcji urządzenia, na przykład aparatu, zawsze zapytanie system operacyjny obecności tej funkcji najpierw. Następnie podczas inicjowania funkcji lub urządzeniu, upewnij się, że żądanie obecnie są obsługiwane z systemu operacyjnego, informacje o tym urządzeniu, a następnie użyj tych ustawień konfiguracji.
1.   **Testowanie** — jest niezwykle ważne do testowania aplikacji wczesne i często na rzeczywistego urządzenia. Nawet urządzeniami przy użyciu tego samego specyfikacje sprzętowe różnią można w ich zachowanie.

#### <a name="limited-resources"></a>Ograniczone zasoby

Urządzenia przenośne uzyskiwanie bardziej zaawansowanych czasu, ale są nadal przenośnych urządzeń, które mają ograniczone możliwości w porównaniu z pulpitu lub notesu komputerów. Na przykład pulpitu Deweloperzy zazwyczaj nie martw się o pojemności pamięci; są używane do mających zarówno pamięci fizycznej i wirtualnej w ilości dużą, podczas gdy na urządzeniach przenośnych można szybko wykorzystać całą dostępną pamięć przez ładowanie kilka obrazów wysokiej jakości.

Ponadto aplikacji intensywnie z procesora, takich jak gry lub rozpoznawania tekstu może naprawdę podatku przenośnych procesora CPU i negatywnie wpłynąć na wydajność urządzenia.

Ze względów takich ważne jest, aby inteligentnie kodu i wdrożyć wczesne i często do rzeczywistego urządzenia do sprawdzania poprawności czasu odpowiedzi.

### <a name="ios-considerations"></a>zagadnienia dotyczące systemu iOS

#### <a name="multitasking"></a>Wielozadaniowości

Wielozadaniowości jest ściśle kontrolowane w systemie iOS i istnieje wiele reguł i zachowania, które aplikacji musi być zgodna z gdy inna aplikacja pochodzi pierwszego planu, w przeciwnym razie aplikacja zostanie przerwane przez system iOS.

#### <a name="device-specific-resources"></a>Zasoby specyficzne dla urządzenia

W ramach określonego obudowie sprzętu może się znacznie różnić między różne modele. Na przykład niektóre urządzenia mają aparatem skierowanym, niektóre również mają aparatem skierowanym i niektóre mają Brak.

Niektóre starsze urządzenia (iPhone sieci 3G i starszych) nawet nie zezwalaj na wielozadaniowości.

Z powodu różnic między modelami urządzenia należy sprawdzić obecność funkcji przed próbą użycia go.

#### <a name="os-specific-constraints"></a>Ograniczenia dotyczące systemu operacyjnego

Aby upewnić się, że aplikacje są dynamiczne i bezpieczne, iOS wymusza liczbę reguł, które muszą przestrzegać aplikacji. Oprócz zasad dotyczących wielozadaniowości istnieje wiele metod zdarzeń, z których aplikacji muszą zwracać w określonym czasie, w przeciwnym razie zostanie uzyskać przerwane przez system iOS.

Również warto zauważyć, aplikacje uruchomione w określane jako piaskownicy, wymusza ograniczenia zabezpieczeń, które ograniczają, co aplikacji można uzyskać dostępu do środowiska. Na przykład aplikacja może odczytywać i zapisywać do własnego katalogu, ale jeśli podejmie próbę zapisu do innego katalogu aplikacji, będzie można przerwać.

### <a name="android-considerations"></a>Zagadnienia dotyczące systemu android

#### <a name="multitasking"></a>Wielozadaniowości

Wielozadaniowości w systemie Android ma dwa składniki; Pierwsza to cyklem życia działania. Każdy ekranu w aplikacji systemu Android jest reprezentowany przez działanie, i ma określonych zdarzeń występujących podczas aplikacji znajduje się w tle lub do pierwszego planu. Aplikacje muszą być zgodne z tego cyklu można utworzyć reakcji, dobrze behaved aplikacji. Aby uzyskać więcej informacji, zobacz [cyklu życia działania](~/android/app-fundamentals/activity-lifecycle/index.md) przewodnik.

Drugim składnikiem do wielozadaniowości w systemie Android jest korzystanie z usług.
Usługi są niezależne od aplikacji procesy długotrwałe, które istnieją i są używane do wykonywania procesów, podczas gdy aplikacja jest w tle. Aby uzyskać więcej informacji, zobacz [tworzenie usług](~/android/app-fundamentals/services/index.md) przewodnik.

#### <a name="many-devices-and-many-form-factors"></a>Wiele urządzeń i wiele obudów

Google nie nakłada żadnych limitów, urządzeń, które można uruchomić system operacyjny Android. Ten model Otwórz wyniki w środowisku produkcyjnym wprowadzana przez różnych różnych urządzeń z bardzo sprzętowe, rozdzielczości ekranu i wskaźniki funkcji urządzenia i możliwości.

Z powodu fragmentacji extreme urządzeń z systemem Android większość użytkowników wybierz najpopularniejszych urządzeń 5 lub 6 do projektowania i testowania i priorytety tych.

#### <a name="security-considerations"></a>Zagadnienia dotyczące zabezpieczeń

Aplikacje na system operacyjny Android wszystkie działają tożsamością distinct, izolowanego z ograniczonymi uprawnieniami. Domyślnie aplikacje można zrobić bardzo mało. Na przykład bez specjalnych uprawnień aplikacji nie można wysłać wiadomości SMS, określić stanu telefonu lub nawet uzyskać dostęp do Internetu! Aby uzyskać dostęp do tych funkcji, aplikacji należy określić w pliku manifestu aplikacji uprawnienia, które miałyby, jak i są instalowane są; system operacyjny odczytuje te uprawnienia, powiadamia użytkownika, że aplikacja żąda uprawnienia, a następnie umożliwia użytkownika kontynuować, lub Anuluj instalację.
Jest to kluczowy etap w modelu Android dystrybucji ze względu na otwartej aplikacji modelu magazynu, ponieważ aplikacje są nie wyselekcjonowanych sposób są one dla systemu iOS, na przykład. Aby uzyskać listę uprawnień aplikacji, zobacz [manifestu uprawnienia](http://developer.android.com/reference/android/Manifest.permission.html) artykule w dokumentacji systemu Android.

### <a name="windows-considerations"></a>Zagadnienia dotyczące systemu Windows

#### <a name="multitasking"></a>Wielozadaniowości

Wielozadaniowości w platformy uniwersalnej systemu Windows ma dwie części: cykl dla stron i aplikacji i procesów w tle. Każdy ekranu w aplikacji jest wystąpieniem klasy strony, która ma zdarzenia związane z odbywa się aktywne lub nieaktywne (z reguły specjalne obsługi stan nieaktywny lub trwa "replikacji"). 

Druga część dostarcza agentów przetwarzania w tle dla przetwarzania zadań, nawet wtedy, gdy aplikacja nie jest uruchomiona na pierwszym planie. 

#### <a name="device-capabilities"></a>Możliwości urządzenia

Chociaż sprzętu platformy uniwersalnej systemu Windows jest dość jednorodne, istnieją składniki, które są opcjonalne i dlatego wymaga specjalnych rozważane podczas kodowania. Możliwości sprzętu obejmują aparatu kompas i Żyroskop. Jest również specjalną klasę ilości pamięci (256MB), który wymaga szczególną uwagę lub deweloperzy mogą zrezygnować obsługi ilości pamięci.

#### <a name="security-considerations"></a>Zagadnienia dotyczące zabezpieczeń

Aby uzyskać informacje na ważne zagadnienia dotyczące zabezpieczeń w platformy uniwersalnej systemu Windows, zapoznaj się [zabezpieczeń](https://docs.microsoft.com/windows/uwp/security/) dokumentacji.

## <a name="summary"></a>Podsumowanie

W tym przewodniku udzielił wprowadzenie do SDLC, ponieważ odnosi się do rozwoju przenośnych. Wprowadzono Ogólne zagadnienia dotyczące tworzenia aplikacji dla urządzeń przenośnych, a numer kwestie specyficzne dla platformy, w tym projektowanie, testowanie i wdrażanie.

## <a name="related-links"></a>Linki pokrewne

- [Wprowadzenie do tworzenia aplikacji dla urządzeń przenośnych](~/cross-platform/get-started/introduction-to-mobile-development.md)
- [Witaj, iOS](~/ios/get-started/hello-ios/index.md)
- [Hello, Android](http://developer.xamarin.com/get-started-droid/)
- [Podstawy aplikacji](~/cross-platform/app-fundamentals/index.md)
