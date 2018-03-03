---
title: "Wyszukiwanie i rozszerzenia elementu Widget ekranu głównego"
description: "W tym artykule omówiono ulepszenia wprowadzone w systemie elementu Widget w systemie iOS 10 firmy Apple."
ms.topic: article
ms.prod: xamarin
ms.assetid: D66FD9E1-9E23-4BB6-825C-ED19B8F72A81
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: a6749ca9d8a793372ec088433780d622f2f05b41
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="search-and-home-screen-widget-enhancements"></a>Wyszukiwanie i rozszerzenia elementu Widget ekranu głównego

_W tym artykule omówiono ulepszenia wprowadzone w systemie elementu Widget w systemie iOS 10 firmy Apple._


Apple wprowadziła kilka ulepszeń systemu Widget, aby upewnić się, że elementy widget wygląda świetnie na tle, wszelkie istniejące na nowe iOS 10 ekranu blokady. Ponadto elementy widget zawierają teraz [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) właściwość, która umożliwia deweloperowi opisano, jak dużo zawartości jest dostępny i umożliwia użytkownikowi rozwijanie i zwijanie zawartości.

Elementy widget (znanej także jako dzisiaj Extensions) to specjalny typ systemu IOS rozszerzenia, w której wyświetlanie małej ilości przydatnych informacji na temat lub ujawniać funkcjonalność specyficzny dla aplikacji w odpowiednim czasie. Na przykład aplikacja wiadomości ma element widget, który zawiera najważniejsze nagłówki i aplikacji kalendarza zawiera dwa różne elementy widget: jeden do wyświetlania zdarzeń współczesnych i jednym pokazanie przyszłych zdarzeń.

Elementy widget w dużym stopniu dostosować i może zawierać elementy interfejsu użytkownika, takie jak tekst, obrazy, przyciski itp. Ponadto dewelopera można dostosować układ ich elementy widget.

[ ![](widgets-images/widgets01.png "Przykład widżetów")](widgets-images/widgets01.png)

Istnieją dwa główne miejsca, które użytkownik może wyświetlić i korzystać z aplikacji elementy widget:

- **Ekran wyszukiwania** — użytkownicy mogą dodawać elementy widget znajduje się najbardziej przydatne do ich ekranu wyszukiwania. Ekran wyszukiwania jest dostępny, szybko przesuwając w prawo na ekranach rodzimego i blokady.
- **Na ekranie Home** -ekranu z Narzędzia główne, użytkownik może użyć technologii 3D Touch aby otworzyć listę szybkie akcje, stosując wykorzystania ikonę aplikacji. Elementy widget aplikacji zostaną wyświetlone powyżej szybkie listy akcji. Zobacz nasze [wprowadzenie do technologii 3D Touch](~/ios/platform/3d-touch.md) dokumentacji, aby uzyskać więcej informacji.

## <a name="widgets-developer-suggestions"></a>Sugestie Developer widżetów

W idealnym przypadku Deweloper powinien zawsze spróbuj i projektowanie elementy widget, które użytkownik chce dodać do ekranów wyszukiwania. W tym celu Apple ma poniższe sugestie:

- **Utwórz doskonałym krótkie środowisko** -użytkownika mają widżety udostępniają informacje krótki, krótkie aktualizacje stanu lub zezwolić im szybko wykonywanie prostych zadań. Dzięki temu, zapewniając prawo ilość informacji i interakcję z ważnymi. Jeśli to możliwe, należy zezwolić użytkownikowi na wykonywanie zadań z danej jednym naciśnięciem. Ponadto ponieważ elementy widget nie obsługuje przesuwania lub przewijania, to będzie musiał brane pod uwagę w elemencie Widget projektu.
- **Szybko Pokaż zawartość** — elementy widget są zaprojektowane jako krótkie, więc użytkownik nigdy nie powinien mieć oczekiwania na zawartość załadować po wyświetleniu elementu Widget. Elementy widget powinien pamięci podręcznej ich zawartości lokalnie, podczas ładowania nowej zawartości w tle może pokazywały ostatnie zawartości.
- **Podaj odpowiednie uzupełnianie i marginesów** — elementy widget nigdy nie powinien wyglądać wiele, więc należy unikać rozszerzanie zawartości do krawędzi elementu Widget widoku. Należy zawsze kilka pikseli szeroki margines między krawędziami i zawartości. Apple również sugeruje, korzystając z ikony aplikacji, wyświetlany w górnej części elementu Widget jako przewodnik wyrównania. Jeśli widżetu prezentuje Układ tabelaryczny, upewnij się, że istnieje odpowiednie dopełnienie między elementami w siatce i spróbuj ograniczyć liczbę elementów do czterech max.
- **Użyj dostosowywalne układów** — szerokość widżetu A różni się zależnie od jest uruchomiona na urządzeniu i orientacji urządzenia. Wysokość elementu Widget może być różny na podstawie, jeśli jest ona wyświetlana w zwinięty (ustawienie domyślne) lub stan rozwinięty (nieobsługiwane przez wszystkie elementy widget). Element Widget zwinięte ma wysokość wierszy tabeli około dwóch i pół standardowe dla systemu iOS. Deweloper może zażądać rozmiar widżetu rozwinięty, ale najlepiej powinna być mniejsza niż wysokość ekranu. W stanie zwinięty widżetu powinien być wyświetlony autonomiczny, tylko istotne informacje. Gdy rozwinięty, widżetu powinien być wyświetlony informacje uzupełniające, które poprawia głównej zawartość wyświetlaną w stanie zwinięty. Elementy widget wyświetlane na liście szybkich akcji tylko będzie w stanie zwinięty.
- **Nie Dostosuj tła widżetu** — elementy widget są wyświetlane na tle światła, rozmyte obsługiwanych przez system. Jest to realizowane wspierać spójność elementy widget i poprawa czytelności ich zawartości. Unikaj używania obrazu jako tło elementu Widget, ponieważ go może mogą powodować konfliktów z tapety blokady i Home ekranu użytkownika.
- **Użyj czcionki systemowej czarny lub szary ciemny** — podczas wyświetlania tekstu w element Widget najlepsze działa czcionki systemowej. Czcionki musi należeć do czarne lub ciemny szarego koloru, aby wyróżnić światła, rozmyte w tle elementu Widget.
- **Podaj aplikacji dostępu podczas odpowiednie** -widżetu zawsze powinien działać niezależnie od ich aplikacji, jednak jeśli wymagana jest bardziej złożone funkcje, widżetu powinno być możliwe do uruchomienia aplikacji, aby wyświetlić lub edytować konkretne informacje. Nigdy nie dołączaj przycisk "Otwórz aplikację", po prostu umożliwia użytkownikowi wybierz samej zawartości i nigdy nie Otwórz 3rd strony aplikacji.
- **Wybierz Wyczyść zaznaczenie, krótkie nazwy elementu Widget** — ikona aplikacji i nazwa widżetu zawsze są wyświetlane w elemencie Widget zawartości. Apple sugeruje, przy użyciu nazwy aplikacji jego podstawowego elementu widget i zrozumiałe i zwięzłe nazwę zapewnia innych. Podczas tworzenia niestandardowego tytuł elementu Widget, musi być poprzedzona nazwą aplikacji (na przykład map pobliskich restauracjach mapy, itp.).
- **Poinformuj o tym, kiedy uwierzytelnianie dodaje wartość** — Jeśli dodatkowych funkcji lub informacji jest dostępna tylko wtedy, gdy użytkownik jest uwierzytelniony i zalogowanego, to widoczne dla użytkownika. Na przykład jazdy Moje powiedzieć "Zaloguj się na jazdy książki" aplikacji do udostępniania.
- **Zaznacz widżet listy szybkich akcji** — Jeśli aplikacja zawiera więcej niż jeden element Widget, dewelopera wybierz jednego widziały, gdy użytkownik wywołuje szybkie listę akcji, stosując wykorzystania ikonę aplikacji przy użyciu technologii 3D Touch.

Więcej informacji na temat pracy z elementy widget, zobacz nasze [wprowadzenie do rozszerzeń](~/ios/platform/extensions.md), [wprowadzenie do technologii 3D Touch](~/ios/platform/3d-touch.md) dokumentacji i firmy Apple [przewodnik programowania w języku rozszerzenie aplikacji](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html).

## <a name="working-with-vibrancy"></a>Praca z popularność

Popularność gwarantuje, że tekst elementu Widget pozostaje czytelny, gdy w elemencie Widget światło, rozmyte tła (dostarczane przez system). Przed iOS 10 użyje dewelopera [NotificationCenterVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1613917-notificationcentervibrancyeffect) dla widżetu popularność. Na przykład:

```csharp
// DEPRECATED: Get Widget Vibrancy Effect
var vibrancy = UIVibrancyEffect.CreateForNotificationCenter ();
```

To jest przestarzała w systemie iOS 10 i powinna zostać zastąpiona albo [WidgetPrimaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771278-widgetprimaryvibrancyeffect) lub [WidgetSecondaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771277-widgetsecondaryvibrancyeffect). Na przykład:

```csharp
// Get Primary Widget Vibrancy Effect
var vibrancy = UIVibrancyEffect.CreatePrimaryVibrancyEffectForNotificationCenter ();

// Get Secondary Widget Vibrancy Effect
var vibrancy2 = UIVibrancyEffect.CreateSecondaryVibrancyEffectForNotificationCenter ();
```

## <a name="working-with-collapsed-and-expanded-widgets"></a>Praca z zwiniętego i rozwiniętego widżetów

Nowy do iOS 10 zawierają teraz elementy widget [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) właściwość, która umożliwia deweloperowi opisano, jak dużo zawartości jest dostępny i umożliwia użytkownikowi rozwijanie i zwijanie zawartości.

Element Widget początkowo jest wyświetlany, jest w stanie zwinięty. Element Widget zwinięte ma wysokość wierszy tabeli około dwóch i pół standardowe dla systemu iOS. Deweloper może zażądać rozmiar widżetu rozwinięty, ale najlepiej powinna być mniejsza niż wysokość ekranu. 

W stanie zwinięty widżetu powinien być wyświetlony autonomiczny, tylko istotne informacje. Gdy rozwinięty, widżetu powinien być wyświetlony informacje uzupełniające, które poprawia głównej zawartość wyświetlaną w stanie zwinięty. Na przykład aplikacja pogody pokazuje bieżący pogodą podczas zwinięte i dodaje godzinowo prognozy po rozwinięciu.

Elementy widget wyświetlane na liście szybkich akcji tylko będzie w stanie zwinięty. Jeśli aplikacja zawiera więcej niż jeden element Widget, dewelopera wybierz jednego widziały, gdy użytkownik wywołuje szybkie listę akcji, stosując wykorzystania ikonę aplikacji przy użyciu technologii 3D Touch.

Poniższy przykład jest proste dzisiaj rozszerzenia (element Widget) obsługujący zwinięty i rozwinięty stanów:

```csharp
using System;
using NotificationCenter;
using Foundation;
using UIKit;
using CoreGraphics;

namespace MonkeyAbout
{
    public partial class TodayViewController : UIViewController, INCWidgetProviding
    {
        protected TodayViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Tell widget it can be expanded
            ExtensionContext.SetWidgetLargestAvailableDisplayMode (NCWidgetDisplayMode.Expanded);

            // Get the maximum size
            var maxSize = ExtensionContext.GetWidgetMaximumSize (NCWidgetDisplayMode.Expanded);
        }

        [Export ("widgetPerformUpdateWithCompletionHandler:")]
        public void WidgetPerformUpdate (Action<NCUpdateResult> completionHandler)
        {
            // Take action based on the display mode
            switch (ExtensionContext.GetWidgetActiveDisplayMode()) {
            case NCWidgetDisplayMode.Compact:
                Content.Text = "Let's Monkey About!";
                break;
            case NCWidgetDisplayMode.Expanded:
                Content.Text = "Gorilla!!!!";
                break;
            }

            // Report results
            // If an error is encoutered, use NCUpdateResultFailed
            // If there's no update required, use NCUpdateResultNoData
            // If there's an update, use NCUpdateResultNewData
            completionHandler (NCUpdateResult.NewData);
        }

        [Export ("widgetActiveDisplayModeDidChange:withMaximumSize:")]
        public void WidgetActiveDisplayModeDidChange (NCWidgetDisplayMode activeDisplayMode, CGSize maxSize)
        {
            // Take action based on the display mode
            switch (activeDisplayMode) {
            case NCWidgetDisplayMode.Compact:
                PreferredContentSize = maxSize;
                Content.Text = "Let's Monkey About!";
                break;
            case NCWidgetDisplayMode.Expanded:
                PreferredContentSize = new CGSize (0, 200);
                Content.Text = "Gorilla!!!!";
                break;
            }
        }

    }
}
```

Spójrz na określonym kod trybu wyświetlania elementu Widget szczegółowo. Aby informuje system, że tego elementu Widget obsługuje stanu rozwinięty, użyto:

```csharp
// Tell widget it can be expanded
ExtensionContext.SetWidgetLargestAvailableDisplayMode (NCWidgetDisplayMode.Expanded);
```

Aby uzyskać bieżący tryb wyświetlania w elemencie Widget, używa:

```csharp
ExtensionContext.GetWidgetActiveDisplayMode()
```

Aby uzyskać maksymalny rozmiar stanu zwinięty albo rozwinięty, używa:

```csharp
// Get the maximum size
var maxSize = ExtensionContext.GetWidgetMaximumSize (NCWidgetDisplayMode.Expanded);
```

I na potrzeby obsługi stanu (tryb wyświetlania), po którym zmiana, używa:

```csharp
[Export ("widgetActiveDisplayModeDidChange:withMaximumSize:")]
public void WidgetActiveDisplayModeDidChange (NCWidgetDisplayMode activeDisplayMode, CGSize maxSize)
{
    // Take action based on the display mode
    switch (activeDisplayMode) {
    case NCWidgetDisplayMode.Compact:
        PreferredContentSize = maxSize;
        Content.Text = "Let's Monkey About!";
        break;
    case NCWidgetDisplayMode.Expanded:
        PreferredContentSize = new CGSize (0, 200);
        Content.Text = "Gorilla!!!!";
        break;
    }
}
```

Poza ustawieniem żądany rozmiar dla każdego stanu (zwinięty lub rozwinięty), aktualizuje również wyświetlane do nowego rozmiaru zawartości.

## <a name="summary"></a>Podsumowanie

Ten artykuł ma pasuje do rozszerzenia, wprowadzone do systemu iOS 10 do elementu Widget firmy Apple i pokazano sposób ich wdrażania w Xamarin.iOS.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
- [Wprowadzenie do rozszerzeń](~/ios/platform/extensions.md)
- [Wprowadzenie do technologii 3D Touch](~/ios/platform/3d-touch.md)
- [Przewodnik programowania w języku rozszerzenia aplikacji](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html)
