---
title: Szybkie techniki interakcji watchOS 3
description: "W tym artykule omówiono techniki interakcji szybkie Apple został dodany w watchOS 3 oraz sposób ich wdrażania w Xamarin.iOS dla Apple Watch."
ms.topic: article
ms.prod: xamarin
ms.assetid: 26697F68-AF7E-4A36-988F-85E2674A4DD1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: bf93744914a0caf4f6599fc333ae200468d66e48
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="quick-interaction-techniques-for-watchos-3"></a>Szybkie techniki interakcji watchOS 3

_W tym artykule omówiono techniki interakcji szybkie Apple został dodany w watchOS 3 oraz sposób ich wdrażania w Xamarin.iOS dla Apple Watch._

Udostępnienie interakcji użytkowników szybkiego są niezbędne do tworzenia atrakcyjnych aplikacji Apple Watch oraz komplikacje. Nowy do watchOS 3 Apple ma dodano obsługę aparatów rozpoznawania gestów, dostęp do cyfrowego wierzchołek i nowe techniki powiadomienie użytkownika i Nawigacja. To, oraz dodano obsługę zarówno SceneKit i SpriteKit, umożliwiają deweloperom łatwe tworzenie rozbudowanych, krótkie interfejsów, które są szybkie i elastyczny.

## <a name="what-are-quick-interactions"></a>Co to są szybkie interakcji

Dla deweloperów, który służy do tworzenia aplikacji dla systemu iOS lub macOS (gdzie ilość czasu, jaki użytkownik zużywa interakcji z aplikacją jest mierzony w minutach lub godzinach), projektowanie aplikacji powiodło się dla Apple Watch może być wyzwaniem i wymaga innej podejście.

W watchOS użytkownik zwykle chce podnieść ich nadgarstka, szybko interakcji z aplikacją (zazwyczaj za kilka krótki czas w sekundach), a następnie usunąć ich nadgarstka i kontynuować niezależnie od było ich wcześniej czynności.

Poniżej przedstawiono kilka przykładów typowych interakcji szybkie na Apple Watch:

- Uruchamia czasomierz.
- Sprawdzanie pogody.
- Oznaczanie elementów listy todo.

Na osiągnięcie tych celów, musi być aplikację na Apple Watch:

- **Krótkie** — co oznacza, że z szybki przegląd użytkownika powinno być możliwe uzyskanie informacji, których potrzebują. 
- **Można wykonać** — co oznacza, że użytkownicy powinien mieć możliwość szybkiego i świadomej decyzji.
- **Dynamiczne** — które oznacza, że użytkownik nigdy nie powinna czekać, aby odbierać informacje, które są im niezbędne lub wykona czynność chcą mieć.

### <a name="quick-interactions-length"></a>Długość szybkie interakcji

Z powodu krótkie charakteru aplikacji Apple Watch Apple sugeruje, że idealne szybkie interakcji musi być dwóch sekund lub mniej. W wyniku tego ograniczenia drugiego Deweloper należy poświęcić znaczną ilość czasu zarówno projektowanie i wdrażanie aplikacji Apple Watch. 

## <a name="new-watchos-3-features-and-apis"></a>Nowe watchOS 3 funkcje i interfejsy API

Apple dodał kilka nowych funkcji i interfejsów API do WatchKit ułatwiających dewelopera Dodawanie szybkie interakcje do swoich aplikacji Apple Watch:

- watchOS 3 zapewnia dostęp do nowych rodzajów takich jak dane wejściowe użytkownika:
    - Aparaty rozpoznawania gestów
    - Cyfrowe obrotu wierzchołek 
- watchOS 3 zawiera nowe sposoby wyświetlania i aktualizowania informacji, takich jak:
    - Ulepszone nawigacji tabeli
    - Nowa funkcja obsługi framework powiadomienie użytkownika
    - Integracja SpriteKit i SceneKit

Implementując te nowe funkcje, deweloper może zapewnienia ich aplikacji watchOS 3 Glanceable, Actionable i Responsive.

### <a name="gesture-recognizer-support"></a>Obsługa aparat rozpoznawania gestów

Jeśli dewelopera zaimplementowano aparaty rozpoznawania gestów w systemie iOS, należy je znajomości działania aparaty rozpoznawania gestów w watchOS 3. Aby odświeżyć, aparaty rozpoznawania gestów są obiektami, które przeanalizować zdarzenia touch niskiego poziomu do gestów rozpoznawalnych, wstępnie zdefiniowane.

watchOS 3 obsługuje cztery następujące aparaty rozpoznawania gestów:

- Gestów osobne typy:
    - Gest Przejdź (`WKSwipeGestureRecognizer`).
    - Gest Tap (`WKTapGestureRecognizer`).
- Typy ciągłego gestu:
    - Gest przesuwanie (`WKPanGestureRecognizer`).
    - Gest naciśnij Long (`WKLongPressGestureRecognizer`).

Aby zaimplementować jedną z nowych aparatów rozpoznawania gestów, przeciągnij go na powierzchnię projektu w projektancie w programie Visual Studio dla systemu iOS dla komputerów Mac i skonfiguruj jego właściwości.

W kodzie Odpowiedz na akcję aparatu rozpoznawania do obsługi gestów inicjowane przez użytkownika. Ponownie odbywa się w taki sam sposób jak może być obsługiwane w systemie iOS.

#### <a name="discrete-gesture-states"></a>Stany gestu odrębny

Odrębny gestów akcji jest wywoływana po rozpoznaniu gestu i stan (`WKGestureRecognizerState`) jest przypisany jako:

[![](quick-interaction-techniques-images/quick01.png "Stany gestu odrębny")](quick-interaction-techniques-images/quick01.png#lightbox)

Wszystkie gesty odrębny uruchamiane `Possible` stanu i przejście do albo `Failed` lub `Recognized` stanu. Gdy za pomocą gestów odrębny, dewelopera zazwyczaj nie uwzględniać bezpośrednio ze stanem. Zamiast tego opierają się na akcję wywoływany po rozpoznaniu gestu tylko.

#### <a name="continuous-gesture-states"></a>Stany gestu ciągłej

Ciągłe gestów są nieco inne niż odrębny gestów, w którym akcja jest wywołana wiele razy, jak rozpoznano gestu:

[![](quick-interaction-techniques-images/quick02.png "Stany gestu ciągłej")](quick-interaction-techniques-images/quick02.png#lightbox)

Ponownie, ciągłe gestów rozpoczyna się `Possible` stanu, ale postępu za pośrednictwem wielu aktualizacji. W tym miejscu dewelopera należy wziąć pod uwagę stanu przez aparat rozpoznawania i aktualizacji w Interfejsie użytkownika aplikacji podczas `Changed` fazy, dopóki nie zostanie ostatecznie gestu `Recognized` lub `Canceled`.

#### <a name="gesture-recognizer-usage-tips"></a>Porady dotyczące użytkowania aparat rozpoznawania gestów

Apple zasugerować następujące podczas pracy z aparatów rozpoznawania gestów w watchOS 3

- Dodaj aparaty rozpoznawania gestów na elementy grupy zamiast poszczególnych formantów. Ponieważ Apple Watch ma mniejszy rozmiar ekranu fizycznego, elementy grupy mogą być większe i łatwiejsze docelowe użytkownikowi trafień. Ponadto aparaty rozpoznawania gestów mogą powodować konflikt z wbudowanych gestów już natywnych kontrolek interfejsu użytkownika.
- Ustaw relacji zależności w aplikacji czujki scenorysu.
- Niektóre gestu pierwszeństwo innych typów gestu, takich jak:
    - Przewijanie
    - Wymuś Touch
 
### <a name="digital-crown-rotation"></a>Obracanie wierzchołek cyfrowych

Implementując w swoich aplikacjach watchOS 3 cyfrowe wierzchołek pomocy technicznej, deweloper może zapewnić nawigacji zwiększona szybkość i dokładność interakcji dla użytkowników.

Ponieważ watchOS 2, można użyć aplikacji Apple Watch `WKInterfacePicker` obiektu dostęp do cyfrowego wierzchołek przez podanie listy `WKPickerItems` i styl selektora (listy, skumulowany lub sekwencja obrazów). watchOS może następnie użytkownika na potrzeby cyfrowe wierzchołek wybierz element z listy.

Korzystając z `WKInterfacePicker`, WatchKit obsługuje większość pracy przez:

- Rysowanie listy i elementy indywidualnego interfejsu.
- Przetwarzanie zdarzeń wierzchołek cyfrowego.
- Po wybraniu elementu podczas wywoływania akcji.

Nowe watchOS 3 dewelopera teraz ma bezpośredni dostęp do zdarzeń obrotu wierzchołek cyfrowych, co pozwala na tworzenie własnych elementów interfejsu użytkownika, które odpowiadają wartościom obrotu.

Cyfrowego dostępu wierzchołek jest świadczona przez następujące elementy:

- `WKCrownSequencer` — Zapewnia dostęp do obrotów na sekundę.
- `WKCrownDelegate` — Zapewnia dostęp do zdarzeń obrotowej delta.

#### <a name="rotations-per-second"></a>Obrotów na sekundę

Dostęp do obrotów na sekundę z cyfrowego wierzchołek jest przydatne, gdy praca z fizycznych na podstawie animacji. Aby uzyskać dostęp do obrotów na sekundę, należy użyć `CrownSequencer` właściwość `WKInterfaceController` czujki rozszerzenia. Na przykład:

```csharp
var rotationsPerSecond = CrownSequencer.RotationsPerSecond;
```

#### <a name="rotational-deltas"></a>Obrotowej delty

Umożliwia liczbę obrotów obrotowej delty z wierzchołek cyfrowego. Użyj `CrownDidRotate` przesłonić metodę `WKCrownDelegate` dostępu może obrotowej do. Na przykład:

```csharp
using System;
using WatchKit;
using Foundation;

namespace MonkeyWatch.MonkeySeeExtension
{
    public class CrownDelegate : WKCrownDelegate
    {
        #region Computed Properties
        public double AccumulatedRotations { get; set;}
        #endregion

        #region Constructors
        public CrownDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void CrownDidRotate (WKCrownSequencer crownSequencer, double rotationalDelta)
        {
            base.CrownDidRotate (crownSequencer, rotationalDelta);

            // Accumulate rotations
            AccumulatedRotations += rotationalDelta;
        }
        #endregion
    }
}
```

W tym miejscu aplikacja przechowuje akumulatora (`AccumulatedRotations`) w celu określenia liczby obrotów. Jeden pełny obrót cyfrowe wierzchołek jest taki sam, jak skumulowany różnicowych `1.0` i będzie połowa obrotu `0.5`.

Apple opuścił go do projektanta, aby określić, jak liczby obrotu odpowiadają czułość zmiany dotyczące elementu interfejsu użytkownika aktualizacji.

Znak (`+/-`) obrotowej różnicowych wskazuje kierunek, czy użytkownik jest włączenie wierzchołek cyfrowych:

[![](quick-interaction-techniques-images/quick03.png "Znak obrotowej różnicowych wskazuje kierunek, czy użytkownik jest włączenie wierzchołek cyfrowych")](quick-interaction-techniques-images/quick03.png#lightbox)


Jeśli użytkownik jest przewijanie w górę WatchKit zwróci delty dodatnią i przewijanie w dół, następnie ujemna delty zostaną zwrócone, niezależnie od tego, jakie orientacji w użytkownik sobie czujki w.

#### <a name="digital-crown-focus"></a>Cyfrowe wierzchołek fokus

Podobnie jak inne elementy interfejsu cyfrowy wierzchołek ma pojęcie fokus. Fokus można przesunięte przeciwną wierzchołek cyfrowe do innych elementów interfejsu oparte na jak użytkownika prowadzi interakcję z czujki. 

Na przykład wykraść dowolnej z poniższych formantów fokus wierzchołek cyfrowych:

- Selektor
- Suwak
- Przewijanie kontrolera

Jest developer do określania, kiedy ich element niestandardowy interfejs musi mieć fokus wierzchołek cyfrowych. Apple sugeruje, przy użyciu nowego aparaty rozpoznawania gestów uzyskanie fokus w elemencie niestandardowego interfejsu użytkownika.

### <a name="vertical-paging"></a>Stronicowanie w pionie

Standardowy sposób, że użytkownik przechodzi widoku tabeli w aplikacji watchOS jest przewiń do żądanego elementu danych, wybierz określonego wiersza, aby wyświetlić widok szczegółowy, naciśnij przycisk Wstecz, po zakończeniu wyświetlanie szczegółów i powtórz ten proces dla innych informacji który y są zainteresowani z wewnątrz tabeli:

[![](quick-interaction-techniques-images/quick04.png "Przenoszenie między tabelą i widok szczegółów")](quick-interaction-techniques-images/quick04.png#lightbox)

Nowy do watchOS 3 dewelopera można włączyć stronicowanie w pionie na ich kontrolki widoku tabeli. Ta funkcja jest włączona użytkownik może przewiń do Znajdź wiersz tabeli widoku i wybierz wiersz, aby wyświetlić jego szczegóły jako przed. Jednak ich można teraz szybko przesuń zapasowej wybierz następnego wiersza w tabeli lub w dół, aby wybrać poprzedniego wiersza (lub użyć wierzchołek cyfrowego), wszystko to bez konieczności powrócić do widoku tabeli najpierw:

[![](quick-interaction-techniques-images/quick05.png "Przenoszenie między tabelą i widok szczegółów i szybko przesuwając w górę i w dół do przechodzenia między innych wierszy")](quick-interaction-techniques-images/quick05.png#lightbox)

Aby włączyć ten tryb, Otwórz aplikację watchOS scenorysu w środowisku Xcode do edycji, wybierz widok tabeli i sprawdź **pionowy stronicowania szczegółów** wyboru:

[![](quick-interaction-techniques-images/quick06.png "Zaznacz pole wyboru pionowy stronicowania szczegółów")](quick-interaction-techniques-images/quick06.png#lightbox)

Upewnij się, że tabela używa Segues Aby wyświetlić widok szczegółowy i zapisać zmiany do scenorysu i powrócić do programu Visual Studio dla komputerów Mac do synchronizacji.

Deweloper może programowo Uwzględnij stronicowanie w pionie na określony wiersz, używając następującego kodu w widoku tabeli:

```csharp
// Segue into Vertical Paging and select the first row
MenuTable.PerformSegue (0);
```

Podczas korzystania z pionowego stronicowania, deweloper musi należy pamiętać, że WatchKit będzie automatycznie obsługiwał wstępnego ładowania kontrolerów i w związku z tym niektóre metody cyklu życia kontrolera może zostać wywołana przed udostępnieniem faktycznie interfejsu użytkownika.

### <a name="notification-enhancements"></a>Ulepszenia powiadomień

Powiadomienia są formularz podstawowy szybkie interakcji użytkownika zwykle napotyka na watchOS i były dostępne tylko od czasu pierwszej Apple Watch i watchOS 1.

Typowy interakcji szybkie powiadomień przebiega następująco:

1. Użytkownik zacznie dotykowych powiadomienia po odebraniu nowe powiadomienie.
2. One podnieść ich nadgarstka, aby wyświetlić interfejs krótkich Szukaj powiadomienia.
3. Jeśli nadal zachować ich nadgarstka wywoływane watchOS automatycznie zostanie przekazana w interfejsie długi Szukaj powiadomień.

Istnieje kilka metod, które użytkownik może odpowiadać na powiadomienia:

- Poprawnie zdefiniowany i wyświetlone powiadomienia użytkownik będzie nic nie rób i po prostu odrzucenie powiadomienia.
- Mogą one także wybrać powiadomienia, aby uruchomić aplikację w watchOS.
- Powiadomienia, który obsługuje akcje niestandardowe użytkownik może wybrać jedną z akcji niestandardowych. Mogą to być:
    - **Akcje pierwszego planu** -je uruchomić aplikację w celu wykonania akcji.
    - **Akcje w tle** — zawsze były kierowane do iPhone w watchOS 2, ale mogą być kierowane do watchApp w watchOS 3.

Nowe dla watchOS 3:

* Powiadomienie za pośrednictwem podobne interfejsu API na wszystkich platformach (z systemem iOS, watchOS i systemu tvOS macOS).
* Lokalnego powiadomienia mogą być planowane na Apple Watch.
* Tło powiadomienia będą kierowane do rozszerzenia aplikacji, jeśli zostały one zaplanowane na Apple Watch.

#### <a name="notification-scheduling-and-delivery"></a>Planowanie powiadomień i dostarczania

Powiadomienie z iPhone użytkownika będzie dalej Apple Watch w następujących przypadkach:

* IPhone ekranu jest wyłączona.
* Apple Watch jest trwa zużyte i został odblokowany.

W watchOS 3 lokalnego powiadomienia mogą być planowane na Apple Watch i tylko są dostarczane na czujki. Jest developer można zaplanować iPhone odpowiednie powiadomienie, jeśli jest to wymagane przez aplikację.

W tym tego samego identyfikatora powiadomienia na Apple Watch i wersje iPhone powiadomień, utrudnia zduplikowane powiadomienia będą wyświetlane na czujki. Wersja Apple Watch powiadomienia będą miały pierwszeństwo przed wersji iPhone.

Ponieważ watchOS 3 wykorzystuje takie same `UINotification` strukturę interfejsu API jako iOS 10, zobacz nasze iOS 10 [Framework powiadomienia użytkownika](~/ios/platform/user-notifications/index.md) dokumentację, aby uzyskać więcej szczegółowych informacji.

### <a name="using-spritekit-and-scenekit"></a>Przy użyciu SpriteKit i SceneKit

Nowy do watchOS 3 dewelopera można teraz używać zarówno SpritKit SceneKit obiekty i w projekcie interfejsu użytkownika ich aplikacji do prezentowania grafiki zarówno 2W i 3W.

Dwa nowe klasy interfejsu zostały dodane do obsługi tej funkcji:

- `WKInterfaceSKScene` — Do pracy z SpriteKit 2D grafiki.
- `WKInterfaceSCNScene` — W przypadku pracy z SceneKit grafiki 3D.

Aby użyć tych obiektów, po prostu przeciągnij je na powierzchnię projektu wewnątrz aplikacji czujki scenorysu w Konstruktorze interfejsu w środowisku Xcode i użyj **inspektora atrybuty** ich konfiguracji.

Z tego punktu Praca z SpriteKit albo SceneKit sceny działa tak samo jak wewnątrz aplikacji systemu iOS. Przedstawia aplikacji czujki `WKInterfaceSKScene` przez wywoływanie jednej z `Present` metody. SceneKit, wystarczy ustawić `Scene` właściwość `WKInterfaceSCNScene` obiektu.

## <a name="actionable-complications"></a>Można wykonać komplikacji

W watchOS 2 Firma Apple wprowadziła komplikacji 3 strony aplikacji. W watchOS 3 Apple rozszerzono o możliwości zawierające przez dewelopera w WatchKit Complication. 

Ponadto, jeden z wbudowanych w czujki kroje teraz mogą zawierać komplikacji i istniejących czujki skierowany, że obsługiwanych komplikacji można teraz już komplikacji jeszcze więcej.

Również nowe jest możliwość przez użytkownika szybko Przejdź lewego lub prawego na przejście przez wszystkie kroje czujki, zainstalowania na ich Apple Watch. Za pomocą nowego galerii na Apple Watch pomocnika iPhone aplikacji, użytkownika można dodawać i dostosowywać powierzchnie czujki i wszelkie komplikacji, które mogą zawierać.

Z powodu te nowe funkcje Apple sugeruje, że każda aplikacja na Apple Watch powinien również zawierać co najmniej jeden Complication i jako takie aplikacji natywnej Apple Watch teraz mieć komplikacji.

Komplikacji zapewniają następujące funkcje do aplikacji:

- Są one bardzo krótkie, ponieważ są one zawsze na powierzchni czujki.
- Komplikacji często są aktualizowane za watchOS. Dowolną aplikację zawierającą Complication po stronie użytkownika, obejrzyj aktualnie wyświetlane są aktualizowane co najmniej dwa razy na godzinę.
- Dowolną aplikację z Complication na powierzchni aktualnie wyświetlanego czujki użytkownika jest przechowywany w pamięci, który ułatwia szybkie uruchamianie aplikacji i przyspiesza odpowiedzi z aplikacji.
- Komplikacji ułatwiają użytkownikowi na uruchamianie określonych funkcji w aplikacji watchOS.

## <a name="glanceable-notification"></a>Krótkie powiadomienie

Powiadomienie w Apple Watch umożliwiają dużą, które można dostosowywać szybko powiadamia użytkownika zdarzeń lub nowe informacje, takie jak wiadomości przychodzących lub osiągnięciu celu w aplikacji ćwiczeń.

Przy użyciu powiadomienia, cenne informacje szybko widoczne dla użytkownika. W wielu sytuacjach dobrze zaprojektowanego powiadomień można usunąć konieczność użytkownikowi faktycznie Uruchom aplikację.

Jesteś nowym użytkownikiem watchOS 3, wszystkie obsługują teraz powiadomień:

- SpriteKit
- SceneKit
- Wbudowany wideo

## <a name="enhanced-ui-with-spritekit-and-scenekit"></a>Rozszerzony interfejs użytkownika z SpriteKit i SceneKit

Zwykle gdy SpriteKit i SceneKit są wymienione dewelopera wydaje się gier interfejsu użytkownika. Jednak zarówno SpriteKit i SceneKit mogą być przydatne do tworzenia gier z systemem innym niż UI obejmujących układów niestandardowych zawartości i animacji, które nie są możliwe w WatchKit samodzielnie.

Na przykład powiadomienia użytkownika aplikacji do udostępniania fotografii można użyć SpriteKit zapewnienie użyciu zaawansowanego środowiska użytkownika w tym użytkownika, który zaksięgowany obraz wraz z rzeczywistego obrazu i inne informacje niestandardowe wzbogaca środowisko użytkownika.

Ponadto zarówno SpriteKit, jak i SceneKit można można łączyć z standardowe elementy interfejsu użytkownika WatchKit w projekcie interfejsu użytkownika aplikacji.

## <a name="simple-navigation"></a>Proste nawigacji

watchOS 3 przedstawia kilka sposobów, że deweloper może uprościć nawigację w ramach swoje aplikacje watchOS, takie jak nowe [pionowy stronicowania](#Vertical-Paging), [Obsługa aparat rozpoznawania gestów](#Gesture-Recognizer-Support) i [wierzchołek cyfrowych Obracanie](#Digital-Crown-Rotation) funkcji przedstawionych powyżej.

Cyfrowe wierzchołek jest unikatowa dla Apple Watch i można na wiele sposobów, aby uprościć nawigacji. Na przykład aplikacji czasomierza umożliwia cyfrowe wierzchołek: wyczyść za pomocą czasomierza dostępne długości.

Gestów niestandardowych można przedstawić nowych i unikatowych możliwości użytkowników do interakcji z aplikacją czujki i może również służyć do uproszczenia nawigacji aplikacji.

Apple sugeruje szukasz sposobów, aby połączyć wszystkie nowe funkcje interakcji szybki, dodane do watchOS 3 do prezentowania sformatowanego, łatwo i szybko używać interfejsów aplikacji watchOS.

## <a name="finishing-the-quick-interaction"></a>Kończenie szybkie interakcji

Środowisko przemyślany interakcji szybkie zapewni użytkownika zaufania, aby usunąć ich nadgarstka (i odłączyć z aplikacją) po zakończeniu ich bieżący interakcji.

W przypadku, gdy w szczególności staje się on problemu jest po wykonanie dowolnego typu połączenia sieciowego lub udostępnianie informacji z aplikacji iPhone jego pomocnika aplikacji czujki. Często może to prowadzić do wskaźnika oczekiwania, podczas transakcji, która nie jest pożądane podczas szybkiego interakcji. Wykonaj poniższy przykład:

[![](quick-interaction-techniques-images/quick07.png "Diagram aplikacji czujki podczas połączenia sieciowego oraz udostępniania tych informacji z jego pomocnika iPhone aplikacji")](quick-interaction-techniques-images/quick07.png#lightbox)

1. Użytkownik wybiera element, aby kupić na czujki.
2. One naciśnij przycisk Kup.
3. Uruchamia transakcji sieci i wyświetlany jest wskaźnik ładowania aplikacji.
4. Później, pewien czas zakończenia operacji i aplikacja wyświetla budowy zakupu.
5. Użytkownik porzuca ich nadgarstka i odłącza z aplikacją.

Od czasu naciśnięciu przycisku Kup do czasu ukończenia transakcji mają one ich nadgarstka wywoływane spojrzenie na wskaźnik ładowania. Aby rozwiązać ten problem, Apple sugeruje dewelopera powinni przedstawić opinii błyskawiczne do użytkownika nie wyświetla wskaźnik ładowania. 

Przy użyciu modelu sugerowane firmy Apple, Przyjrzyjmy się ponownie tej samej interakcji szybkie:

[![](quick-interaction-techniques-images/quick08.png "Diagram modelu sugerowane jabłek")](quick-interaction-techniques-images/quick08.png#lightbox)

1. Użytkownik wybiera element, aby kupić na czujki.
2. One naciśnij przycisk Kup.
3. Aplikacja rozpoczyna się transakcji sieci i zostanie wyświetlony komunikat informujący o tym, że zakupu została pomyślnie uruchomiona.
4. Użytkownik porzuca ich nadgarstka i odłącza z aplikacją.
5. Po zakończeniu transakcji pomyślnie później trochę czasu, aplikacja wyświetla lokalnego powiadomienia informują użytkownika o pomyślnym zakupu.

Teraz, jak najszybciej po naciśnięciu przycisk Kup, które są wyświetlane komunikat informujący, że zakupu została uruchomiona, dzięki czemu może bezpiecznie porzucić ich nadgarstka i kończyć się w tym momencie szybkie interakcji. Później zostali poinformowani o powodzeniu lub niepowodzeniu transakcji w powiadomienie użytkownika. W ten sposób użytkownika jest tylko interakcji z aplikacją podczas "active" fazy procesu.

Dla aplikacji, które robią sieci, można użyć tło `NSURLSession` do obsługi komunikacji sieciowej z zadania pobierania. Pozwoli to aplikacji wznawiane w tle, aby przetworzyć pobrane informacje. Dla aplikacji, które wymagają przetwarzania w tle należy użyć potwierdzenia zadań tła do obsługi przetwarzania wymagane.

## <a name="quick-interaction-design-tips"></a>Wskazówki dotyczące projektowania szybkie interakcji

Ponieważ wymagana długość szybkie interakcji dwóch sekund lub mniej, deweloper skoncentrować się projekt interakcji aplikacji od samego początku procesu projektowania. Znajdź obszary, w którym tych interakcji można uprościć (przy użyciu technik przedstawionych powyżej) i korzystać z nowych funkcji watchOS 3 można zapewnić szybkie i reakcji aplikacji.

Apple sugeruje następujące czynności:

- Skupić się na szybki interakcje przełączając najczęściej używane funkcje aplikacji do przodu.
- Użyj komplikacji i powiadomienia użytkowników na powierzchni typowe cechy i funkcje.
- Tworzenie rozbudowanych, krótkie interfejsu użytkownika z SceneKit i SpriteKit.
- Jeśli to możliwe, należy uprościć nawigacji w aplikacji.
- Nie należy wprowadzać użytkownika oczekiwania, zezwolić im na porzucić ich nadgarstka i jak najszybciej rozłączają się z aplikacją.

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego techniki interakcji szybkie Apple został dodany w watchOS 3 oraz sposób ich wdrażania w Xamarin.iOS dla Apple Watch.

## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu watchOS](https://developer.xamarin.com/samples/watchos/all/)
