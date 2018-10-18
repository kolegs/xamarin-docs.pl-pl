---
title: Specyficznych dla platformy Windows
description: Zezwalaj na specyficznych dla platformy, umożliwiają korzystanie z funkcji, które są dostępne tylko na danej platformie, bez stosowania niestandardowe programy renderujące lub efekty. W tym artykule pokazano, jak używać Windows specyficznych dla platformy, które są wbudowane w platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 22B403C0-FE6D-498A-AE53-095E6C4B527C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 2130bbbedbab66fac9427947ca42f21c346360ce
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2018
ms.locfileid: "38998419"
---
# <a name="windows-platform-specifics"></a>Specyficznych dla platformy Windows

_Zezwalaj na specyficznych dla platformy, umożliwiają korzystanie z funkcji, które są dostępne tylko na danej platformie, bez stosowania niestandardowe programy renderujące lub efekty. W tym artykule pokazano, jak używać Windows specyficznych dla platformy, które są wbudowane w platformy Xamarin.Forms._

## <a name="visualelements"></a>VisualElements

Na platformie Universal Windows następujące funkcje specyficzne dla platformy towarzyszy widoków, strony i układy platformy Xamarin.Forms:

- Ustawienie klucza dostępu dla [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Aby uzyskać więcej informacji, zobacz [klucze dostępu VisualElement ustawienie](#visualelement-accesskeys).
- Wyłączenie trybu kolorów starszej wersji w obsługiwanej [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Aby uzyskać więcej informacji, zobacz [wyłączenie starszej wersji trybu kolorów](#legacy-color-mode).

<a name="visualelement-accesskeys" />

### <a name="setting-visualelement-access-keys"></a>Ustawienie VisualElement klawiszy

Klawisze dostępu są skróty klawiaturowe, które poprawić użyteczność i dostępność aplikacji na platformie Universal Windows, zapewniając intuicyjny sposób użytkownicy mogą szybko przejść i wchodzić w interakcje z widoczne Interfejsie użytkownika aplikacji za pomocą klawiatury zamiast za pomocą dotyku lub myszy. Są one kombinacje klawisz Alt i co najmniej alfanumeryczne klawiszy, zwykle po kolei. Skróty klawiaturowe są obsługiwane automatycznie klucze dostępu, które użyj pojedynczego znaku alfanumerycznego.

Klawisze skrótu dostępu są liczb zmiennoprzecinkowych wskaźniki wyświetlane obok kontrolki, które zawierają klucze dostępu. Każda wskazówka klawiszy dostępu zawiera klawisze alfanumeryczne, które aktywują skojarzonego formantu. Gdy użytkownik naciśnie klawisz Alt, klawisze skrótu dostępu są wyświetlane.

Umożliwia określenie klucza dostępu dla określonych platform [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Jest używany w XAML, ustawiając [ `VisualElement.AccessKey` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyProperty) dołączona właściwość na wartość alfanumeryczne i opcjonalnie ustawiając [ `VisualElement.AccessKeyPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyPlacementProperty) dołączona właściwość na wartość [ `AccessKeyPlacement` ](xref:Xamarin.Forms.AccessKeyPlacement) wyliczenia, [ `VisualElement.AccessKeyHorizontalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyHorizontalOffsetProperty) dołączonych właściwości `double`i [ `VisualElement.AccessKeyVerticalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyVerticalOffsetProperty) dołączona właściwość do `double`:

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <ContentPage Title="Page 1"
                 windows:VisualElement.AccessKey="1">
        <StackLayout Margin="20">
            ...
            <Switch windows:VisualElement.AccessKey="A" />
            <Entry Placeholder="Enter text here"
                   windows:VisualElement.AccessKey="B" />
            ...
            <Button Text="Access key F, placement top with offsets"
                    Margin="20"
                    Clicked="OnButtonClicked"
                    windows:VisualElement.AccessKey="F"
                    windows:VisualElement.AccessKeyPlacement="Top"
                    windows:VisualElement.AccessKeyHorizontalOffset="20"
                    windows:VisualElement.AccessKeyVerticalOffset="20" />
            ...
        </StackLayout>
    </ContentPage>
    ...
</TabbedPage>
```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

var page = new ContentPage { Title = "Page 1" };
page.On<Windows>().SetAccessKey("1");

var switchView = new Switch();
switchView.On<Windows>().SetAccessKey("A");
var entry = new Entry { Placeholder = "Enter text here" };
entry.On<Windows>().SetAccessKey("B");
...

var button4 = new Button { Text = "Access key F, placement top with offsets", Margin = new Thickness(20) };
button4.Clicked += OnButtonClicked;
button4.On<Windows>()
    .SetAccessKey("F")
    .SetAccessKeyPlacement(AccessKeyPlacement.Top)
    .SetAccessKeyHorizontalOffset(20)
    .SetAccessKeyVerticalOffset(20);
...
```

`VisualElement.On<Windows>` Metoda określa, że określonych platform będzie uruchamiane tylko na platformie Universal Windows. [ `VisualElement.SetAccessKey` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKey(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.String)) Metody w [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) przestrzeni nazw jest używana do ustawiania wartości klucza dostępu dla `VisualElement`. [ `VisualElement.SetAccessKeyPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKeyPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},Xamarin.Forms.AccessKeyPlacement)) Metody, opcjonalnie określa położenie na potrzeby wyświetlanie klawiszy skrótu dostępu za pomocą [ `AccessKeyPlacement` ](xref:Xamarin.Forms.AccessKeyPlacement) wyliczenie, podając następujące możliwe wartości:

- [`Auto`](xref:Xamarin.Forms.AccessKeyPlacement.Auto) — Wskazuje, że położenie klawiszy skrótu dostępu jest zależna od systemu operacyjnego.
- [`Top`](xref:Xamarin.Forms.AccessKeyPlacement.Top) — Wskazuje, że klawiszy skrótu dostępu pojawi się powyżej górnej krawędzi `VisualElement`.
- [`Bottom`](xref:Xamarin.Forms.AccessKeyPlacement.Bottom) — Wskazuje, że klawiszy skrótu dostępu pojawi się poniżej dolnej krawędzi `VisualElement`.
- [`Right`](xref:Xamarin.Forms.AccessKeyPlacement.Right) — Wskazuje, że dostęp klawiszy skrótu pojawią się na prawo od prawej krawędzi `VisualElement`.
- [`Left`](xref:Xamarin.Forms.AccessKeyPlacement.Left) — Wskazuje, że dostęp klawiszy skrótu pojawią się na lewo od lewej krawędzi `VisualElement`.
- [`Center`](xref:Xamarin.Forms.AccessKeyPlacement.Center) — Wskazuje klawiszy skrótu dostępu, zostaną nałożone na środku `VisualElement`.

> [!NOTE]
> Zazwyczaj [ `Auto` ](xref:Xamarin.Forms.AccessKeyPlacement.Auto) umieszczania klawiszy skrótu jest wystarczająca, która obejmuje obsługę interfejsów użytkownika adaptacyjne.

[ `VisualElement.SetAccessKeyHorizontalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKeyHorizontalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Double)) i [ `VisualElement.SetAccessKeyVerticalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKeyVerticalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Double)) metody może służyć do bardziej szczegółową kontrolę dostępu do lokalizacji klawiszy skrótu. Argument `SetAccessKeyHorizontalOffset` metoda wskazuje, jak daleko przenieść dostępu klawisza skrótu po lewej lub prawej, argument `SetAccessKeyVerticalOffset` metoda wskazuje sposób znacznie przenoszenia klawiszy skrótu dostępu w górę lub w dół.

>[!NOTE]
> Nie można ustawić dostęp, przesunięcia klawiszy skrótu, gdy ustawiono umieszczenia klucza dostępu `Auto`.

Ponadto [ `GetAccessKey` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKey(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})), [ `GetAccessKeyPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKeyPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})), [ `GetAccessKeyHorizontalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKeyHorizontalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})), i [ `GetAccessKeyVerticalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKeyVerticalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})) można używać metod można pobrać dostępu do klucza wartość i jego lokalizacji.

Wynik jest, że klawiszy skrótu dostępu mogą być wyświetlane obok któregoś [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) wystąpień, które definiują dostęp do kluczy, naciskając klawisz Alt:

![Klucze dostępu VisualElement specyficzne dla platformy](windows-images/visualelement-accesskeys.png "klucze dostępu VisualElement specyficzne dla platformy")

Gdy użytkownik aktywuje klucz dostępu, naciskając klawisz Alt, a następnie dostęp do klucza, domyślna akcja dla `VisualElement` zostaną wykonane. Na przykład, gdy użytkownik aktywuje klucz dostępu na [ `Switch` ](xref:Xamarin.Forms.Switch), `Switch` jest włączony. Gdy użytkownik aktywuje klucz dostępu na [ `Entry` ](xref:Xamarin.Forms.Entry), `Entry` uzyska fokus. Gdy użytkownik aktywuje klucz dostępu na [ `Button` ](xref:Xamarin.Forms.Button), program obsługi zdarzeń dla [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) zdarzenia jest wykonywany.

Aby uzyskać więcej informacji na temat kluczy dostępu, zobacz [klucze dostępu](/windows/uwp/design/input/access-keys#key-tip-positioning).

<a name="legacy-color-mode" />

### <a name="disabling-legacy-color-mode"></a>Wyłączenie trybu kolorów starszej wersji

Niektóre widoki Xamarin.Forms są wyposażone w trybie starszej wersji kolorów. W tym trybie gdy [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) widoku zostaje ustalona `false`, widoku spowoduje zastąpienie kolory ustawionych przez użytkownika za pomocą natywnego kolory domyślne w stanie wyłączenia. Dla zapewnienia zgodności, to tryb kolorów starszej wersji pozostaje domyślne zachowanie dla obsługiwane widoki.

Określonych platform wyłącza ten tryb kolorów starszej wersji, tak, aby kolorów, ustaw dla widoku przez użytkownika pozostają, nawet wtedy, gdy widok jest wyłączony. Jest używany w XAML, ustawiając [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.IsLegacyColorModeEnabledProperty) dołączonych właściwości `false`:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Editor Text="Enter text here"
                TextColor="Blue"
                BackgroundColor="Bisque"
                windows:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

_legacyColorModeDisabledEditor.On<Windows>().SetIsLegacyColorModeEnabled(false);
```

`VisualElement.On<Windows>` Metoda określa, że określonych platform będzie uruchamiane tylko na Windows. [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Boolean)) Metody w [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) przestrzeni nazw jest używana do kontroli, czy tryb starszej wersji kolorów jest wyłączona. Ponadto [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})) metoda może służyć do zwrócenia, czy tryb starszej wersji kolorów jest wyłączona.

Wynik jest, można wyłączyć tryb kolorów starszej wersji, co gwarantuje kolorów, ustaw dla widoku przez użytkownika nawet wtedy, gdy widok jest wyłączony:

![](windows-images/legacy-color-mode-disabled.png "Wyłączono tryb starszej wersji kolorów")

> [!NOTE]
> Podczas ustawiania [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) w widoku, tryb starszej wersji kolor całkowicie jest ignorowany. Aby uzyskać więcej informacji na temat stanów wizualnych zobacz [Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="views"></a>Widoki

Na Universal Windows Platform (platformy UWP), znajduje się następujące funkcje specyficzne dla platformy Xamarin.Forms widoków:

- Wykrywanie, kolejność odczytu z pliku tekstowego zawartości [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), i [ `Label` ](xref:Xamarin.Forms.Label) wystąpień. Aby uzyskać więcej informacji, zobacz [wykrywanie kolejność czytania od zawartości](#inputview-readingorder).
- Włączanie obsługi gestu wzorca tap w [ `ListView` ](xref:Xamarin.Forms.ListView). Aby uzyskać więcej informacji, zobacz [Włączanie naciśnij obsługi gestu w ListView](#listview-selectionmode).
- Włączanie [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) wchodzić w interakcje z aparatem sprawdzania pisowni. Aby uzyskać więcej informacji, zobacz [Włączanie sprawdzania pisowni SearchBar](#searchbar-spellcheck).
- Włączanie [ `WebView` ](xref:Xamarin.Forms.WebView) do wyświetlania alertów JavaScript w oknie dialogowym komunikatu platformy uniwersalnej systemu Windows. Aby uzyskać więcej informacji, zobacz [wyświetlanie alertów JavaScript](#webview-javascript-alert).

<a name="inputview-readingorder" />

### <a name="detecting-reading-order-from-content"></a>Wykrywanie, kolejność czytania od zawartości

Określonych platform umożliwia kolejność odczytu (od lewej do prawej lub od prawej do lewej) tekstu dwukierunkowego w [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), i [ `Label` ](xref:Xamarin.Forms.Label) wystąpień, aby zostało wykryte, dynamicznie. Jest używany w XAML, ustawiając [ `InputView.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.DetectReadingOrderFromContentProperty) (dla `Entry` i `Editor` wystąpień) lub [ `Label.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Label.DetectReadingOrderFromContentProperty) dołączonych właściwości `boolean` wartość:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Editor ... windows:InputView.DetectReadingOrderFromContent="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

editor.On<Windows>().SetDetectReadingOrderFromContent(true);
```

`Editor.On<Windows>` Metoda określa, że określonych platform będzie uruchamiane tylko na platformie Universal Windows. [ `InputView.SetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.SetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView},System.Boolean)) Metody w [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) przestrzeni nazw jest używana do kontroli, czy wykryto kolejność czytania od zawartości w [ `InputView` ](xref:Xamarin.Forms.InputView). Ponadto `InputView.SetDetectReadingOrderFromContent` metoda może służyć do wyświetlania i ukrywania wykryto kolejność czytania od zawartości przez wywołanie metody [ `InputView.GetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.GetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView})) metodę do zwracania bieżącej wartości:

```csharp
editor.On<Windows>().SetDetectReadingOrderFromContent(!editor.On<Windows>().GetDetectReadingOrderFromContent());
```

W wyniku [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), i [ `Label` ](xref:Xamarin.Forms.Label) wystąpień może mieć kolejność czytania ich zawartość dynamicznie wykryto:

[![InputView wykrywanie kolejność czytania od zawartości specyficznej dla platformy](windows-images/editor-readingorder.png "InputView wykrywanie kolejność czytania od zawartości specyficznej dla platformy")](windows-images/editor-readingorder-large.png#lightbox "InputView wykrywanie kolejność czytania od zawartość określonej platformy")

> [!NOTE]
> W przeciwieństwie do ustawienia [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwość, logiki dla widoków, które wykrywają kolejność czytania od ich zawartości tekstowej nie wpłynie na wyrównanie tekstu w widoku. Zamiast tego dostosowuje kolejność, w którym są ułożone bloki tekstu dwukierunkowego.

<a name="listview-selectionmode" />

### <a name="enabling-tap-gesture-support-in-a-listview"></a>Włączanie obsługi gestu wzorca Tap w ListView

Na platformie Universal Windows domyślnie Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) korzysta z natywnym `ItemClick` zdarzenie, aby odpowiedzieć na interakcję, a nie w natywnym `Tapped` zdarzeń. Dzięki temu funkcji ułatwień dostępu, aby Windows Narrator i klawiatury mogą wchodzić w interakcje z `ListView`. Jednak również renderuje wszystkie gesty wzorca tap wewnątrz `ListView` przestanie działać.

To określa specyficzne dla platformy czy elementy w [ `ListView` ](xref:Xamarin.Forms.ListView) mogą odpowiadać na naciśnij gestów i dlatego czy natywnych `ListView` generowane `ItemClick` lub `Tapped` zdarzeń. Jest używany w XAML, ustawiając [ `ListView.SelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SelectionModeProperty) dołączona właściwość na wartość [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) wyliczenia:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <ListView ... windows:ListView.SelectionMode="Inaccessible">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

listView.On<Windows>().SetSelectionMode(ListViewSelectionMode.Inaccessible);
```

`ListView.On<Windows>` Metoda określa, że określonych platform będzie uruchamiane tylko na platformie Universal Windows. [ `ListView.SetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView},Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode)) Metody w [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) przestrzeni nazw jest używana do kontroli tego, czy elementy w [ `ListView` ](xref:Xamarin.Forms.ListView) mogą odpowiadać na naciśnij gesty, przy użyciu [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) wyliczenie, zapewniając dwóch wartości:

- [`Accessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Accessible) — Wskazuje, że `ListView` nastąpi natywnych `ItemClick` zdarzenia w celu obsługi interakcji i dlatego udostępniają funkcje ułatwień dostępu. W związku z tym, Windows Narrator i klawiatury może interakcyjnie przeprowadzić `ListView`. Jednak elementy w `ListView` nie może odpowiadać na naciśnij gestów. Jest to domyślne zachowanie dla `ListView` wystąpień na platformie Universal Windows.
- [`Inaccessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Inaccessible) — Wskazuje, że `ListView` nastąpi natywnych `Tapped` zdarzenia w celu obsługi interakcji. W związku z tym, elementy w `ListView` mogą odpowiadać na naciśnij gestów. Jednak istnieje żadne funkcje ułatwień dostępu i dlatego Windows Narrator i klawiatury nie mogą współdziałać z `ListView`.

> [!NOTE]
> `Accessible` i `Inaccessible` tryby wyboru wykluczają się wzajemnie, a, musisz wybrać dostępny [ `ListView` ](xref:Xamarin.Forms.ListView) lub `ListView` , mogą odpowiadać na naciśnij gestów.

Ponadto [ `GetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.GetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView})) metoda może służyć do zwrócenia bieżącego [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode).

Wynik jest fakt, że określonym [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) jest stosowany do [ `ListView` ](xref:Xamarin.Forms.ListView), które kontrolki czy elementy w `ListView` mogą odpowiadać na gesty, naciśnij i dlatego czy natywnych `ListView` generowane `ItemClick` lub `Tapped` zdarzeń.

<a name="searchbar-spellcheck" />

### <a name="enabling-searchbar-spell-check"></a>Włączanie sprawdzania pisowni SearchBar

Umożliwia to specyficzne dla platformy [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) wchodzić w interakcje z aparatem sprawdzania pisowni. Jest używany w XAML, ustawiając [ `SearchBar.IsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.IsSpellCheckEnabledProperty) dołączonych właściwości `boolean` wartość:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SearchBar ... windows:SearchBar.IsSpellCheckEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

searchBar.On<Windows>().SetIsSpellCheckEnabled(true);
```

`SearchBar.On<Windows>` Metoda określa, że określonych platform będzie uruchamiane tylko na platformie Universal Windows. [ `SearchBar.SetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.SetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar},System.Boolean)) Metody w [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) przestrzeni nazw, włącza moduł sprawdzania pisowni i wyłącza. Ponadto `SearchBar.SetIsSpellCheckEnabled` metoda może służyć do przełączenia sprawdzania pisowni przez wywołanie metody [ `SearchBar.GetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.GetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar})) metodę, aby zwrócić na to, czy moduł sprawdzania pisowni jest włączone:

```csharp
searchBar.On<Windows>().SetIsSpellCheckEnabled(!searchBar.On<Windows>().GetIsSpellCheckEnabled());
```

Wynik jest ten tekst wprowadzony w [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) może być sprawdzana pisownia, za pomocą niepoprawne błędy pisowni, które są określone dla użytkownika:

![SearchBar pisowni wyboru specyficzne dla platformy](windows-images/searchbar-spellcheck.png "SearchBar pisowni wyboru specyficzne dla platformy")

> [!NOTE]
> `SearchBar` Klasy w [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) przestrzeni nazw ma również [ `EnableSpellCheck` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.EnableSpellCheck*) i [ `DisableSpellCheck` ](xre:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.DisableSpellCheck*) metod, które mogą służyć do włączania i wyłączania Moduł sprawdzania pisowni na `SearchBar`, odpowiednio.

<a name="webview-javascript-alert" />

### <a name="displaying-javascript-alerts"></a>Wyświetlanie alertów JavaScript

Umożliwia to specyficzne dla platformy [ `WebView` ](xref:Xamarin.Forms.WebView) do wyświetlania alertów JavaScript w oknie dialogowym komunikatu platformy uniwersalnej systemu Windows. Jest używany w XAML, ustawiając [ `WebView.IsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabledProperty) dołączonych właściwości `boolean` wartość:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <WebView ... windows:WebView.IsJavaScriptAlertEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

var webView = new Xamarin.Forms.WebView
{
  Source = new HtmlWebViewSource
  {
    Html = @"<html><body><button onclick=""window.alert('Hello World from JavaScript');"">Click Me</button></body></html>"
  }
};
webView.On<Windows>().SetIsJavaScriptAlertEnabled(true);
```

`WebView.On<Windows>` Metoda określa, że określonych platform będzie uruchamiane tylko na platformie Universal Windows. [ `WebView.SetIsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.SetIsJavaScriptAlertEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.WebView},System.Boolean)) Metody w [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) przestrzeni nazw jest używana do kontroli, czy alerty JavaScript są włączone. Ponadto `WebView.SetIsJavaScriptAlertEnabled` metoda może służyć do Przełącz alerty JavaScript przez wywołanie metody [ `IsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabled*) metodę, aby zwrócić na to, czy są włączone:

```csharp
_webView.On<Windows>().SetIsJavaScriptAlertEnabled(!_webView.On<Windows>().IsJavaScriptAlertEnabled());
```

Wynik jest, że JavaScript alerty mogą być wyświetlane w oknie dialogowym komunikatu platformy uniwersalnej systemu Windows:

![Ostrzeżenie WebView JavaScript specyficzne dla platformy](windows-images/webview-javascript-alert.png "alert WebView JavaScript specyficzne dla platformy")

## <a name="pages"></a>Strony

Na platformie Universal Windows towarzyszy następujące funkcje specyficzne dla platformy Xamarin.Forms strony:

- Zwijanie [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) paska nawigacyjnego. Aby uzyskać więcej informacji, zobacz [zwijanie pasek nawigacyjny MasterDetailPage](#collapsable_navigation_bar).
- Ustawianie opcji umieszczania paska narzędzi. Aby uzyskać więcej informacji, zobacz [zmiana położenia narzędzi strony](#toolbar_placement).
- Włączanie strony ikony, który będzie wyświetlany w [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) paska narzędzi. Aby uzyskać więcej informacji, zobacz [Włączanie ikony na TabbedPage](#tabbedpage-icons).

<a name="collapsable_navigation_bar" />

### <a name="collapsing-a-masterdetailpage-navigation-bar"></a>Zwijanie MasterDetailPage paska nawigacyjnego

Umożliwia Zwiń pasek nawigacyjny na określonych platform [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage)i jest używane w XAML, ustawiając [ `MasterDetailPage.CollapseStyle` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapseStyleProperty) i [ `MasterDetailPage.CollapsedPaneWidth` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidthProperty)dołączonych właściwości:

```xaml
<MasterDetailPage ...
                  xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
                  windows:MasterDetailPage.CollapseStyle="Partial"
                  windows:MasterDetailPage.CollapsedPaneWidth="48">
  ...
</MasterDetailPage>

```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetCollapseStyle(CollapseStyle.Partial).CollapsedPaneWidth(148);
```

`MasterDetailPage.On<Windows>` Metoda określa, że określonych platform będzie uruchamiane tylko na Windows. [ `Page.SetCollapseStyle` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.SetCollapseStyle(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage},Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle)) Metody w [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) przestrzeni nazw, jest używany do określania stylu Zwiń z [ `CollapseStyle` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle) wyliczenie, zapewniając dwóch wartości: [ `Full` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Full) i [ `Partial` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Partial). [ `MasterDetailPage.CollapsedPaneWidth` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidth(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage},System.Double)) Metoda służy do określania szerokości częściowo zwiniętym paskiem nawigacji.

Wynik jest fakt, że określonym [ `CollapseStyle` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle) jest stosowany do [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) wystąpienia o szerokości także określić:

[![](windows-images/collapsed-navigation-bar.png "Zwinięty pasek nawigacji specyficznymi dla platformy")](windows-images/collapsed-navigation-bar-large.png#lightbox "zwinięty pasek nawigacji specyficznymi dla platformy")

<a name="toolbar_placement" />

### <a name="changing-the-page-toolbar-placement"></a>Zmiana położenia narzędzi strony

Umożliwia zmienić położenie paska narzędzi na określonych platform [ `Page` ](xref:Xamarin.Forms.Page)i jest używane w XAML, ustawiając [ `Page.ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.ToolbarPlacementProperty) dołączona właściwość na wartość [ `ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement) wyliczenia:

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
            windows:Page.ToolbarPlacement="Bottom">
  ...
</TabbedPage>
```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetToolbarPlacement(ToolbarPlacement.Bottom);
```

`Page.On<Windows>` Metoda określa, że określonych platform będzie uruchamiane tylko na Windows. [ `Page.SetToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.SetToolbarPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.Page},Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement)) Metody w [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) przestrzeni nazw jest używana do ustawiania położenie paska narzędzi z [ `ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement) dostarczanie wyliczenia trzy wartości: [ `Default` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Default), [ `Top` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Top), i [ `Bottom` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Bottom).

Wynik jest zastosowana do umieszczania określonego narzędzi [ `Page` ](xref:Xamarin.Forms.Page) wystąpienie:

[![](windows-images/toolbar-placement.png "Pasek narzędzi umieszczania specyficzne dla platformy")](windows-images/toolbar-placement-large.png#lightbox "narzędzi umieszczania specyficzne dla platformy")

<a name="tabbedpage-icons" />

### <a name="enabling-icons-on-a-tabbedpage"></a>Włączanie ikony na TabbedPage

Określonych platform umożliwia strony ikony, który będzie wyświetlany w [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) narzędzi i oferuje możliwość Opcjonalnie można określić rozmiar ikon. Jest używany w XAML, ustawiając [ `TabbedPage.HeaderIconsEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.HeaderIconsEnabledProperty) dołączonych właściwości `true`i opcjonalnie ustawiając [ `TabbedPage.HeaderIconsSize` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.HeaderIconsSizeProperty) dołączonych właściwości [ `Size` ](xref:Xamarin.Forms.Size) wartość:

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
            windows:TabbedPage.HeaderIconsEnabled="true">
    <windows:TabbedPage.HeaderIconsSize>
        <Size>
            <x:Arguments>
                <x:Double>24</x:Double>
                <x:Double>24</x:Double>
            </x:Arguments>
        </Size>
    </windows:TabbedPage.HeaderIconsSize>
    <ContentPage Title="Todo" Icon="todo.png">
        ...
    </ContentPage>
    <ContentPage Title="Reminders" Icon="reminders.png">
        ...
    </ContentPage>
    <ContentPage Title="Contacts" Icon="contacts.png">
        ...
    </ContentPage>
</TabbedPage>
```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

public class WindowsTabbedPageIconsCS : Xamarin.Forms.TabbedPage
{
  public WindowsTabbedPageIconsCS()
    {
    On<Windows>().SetHeaderIconsEnabled(true);
    On<Windows>().SetHeaderIconsSize(new Size(24, 24));

    Children.Add(new ContentPage { Title = "Todo", Icon = "todo.png" });
    Children.Add(new ContentPage { Title = "Reminders", Icon = "reminders.png" });
    Children.Add(new ContentPage { Title = "Contacts", Icon = "contacts.png" });
  }
}
```

`TabbedPage.On<Windows>` Metoda określa, że określonych platform będzie uruchamiane tylko na platformie Universal Windows. [ `TabbedPage.SetHeaderIconsEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.SetHeaderIconsEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.TabbedPage},System.Boolean)) Metody w [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) przestrzeni nazw, umożliwia włączanie i wyłączanie nagłówka ikon. [ `TabbedPage.SetHeaderIconsSize` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.SetHeaderIconsSize(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.TabbedPage},Xamarin.Forms.Size)) Metoda opcjonalnie określa rozmiar ikony nagłówka z [ `Size` ](xref:Xamarin.Forms.Size) wartości.

Ponadto `TabbedPage` klasy w `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` przestrzeni nazw ma również [ `EnableHeaderIcons` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.EnableHeaderIcons*) metodę, która umożliwia ikony nagłówka [ `DisableHeaderIcons` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.DisableHeaderIcons*) metodę, która wyłącza ikony nagłówka i [ `IsHeaderIconsEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.IsHeaderIconsEnabled*) metodę, która zwraca `boolean` wartość, która wskazuje, czy nagłówek ikony są włączone.

Wynik jest tej strony, które mogą być wyświetlane ikony na [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) paska narzędzi o rozmiarze ikonę opcjonalnie Ustaw żądany rozmiar:

![TabbedPage włączone ikony specyficznej dla platformy](windows-images/tabbedpage-icons.png "TabbedPage włączone ikony specyficznej dla platformy")

## <a name="summary"></a>Podsumowanie

W tym artykule pokazano, jak używać Windows specyficznych dla platformy, które są wbudowane w platformy Xamarin.Forms. Zezwalaj na specyficznych dla platformy, umożliwiają korzystanie z funkcji, które są dostępne tylko na danej platformie, bez stosowania niestandardowe programy renderujące lub efekty.

## <a name="related-links"></a>Linki pokrewne

- [Tworzenie funkcji specyficznych dla platformy](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (przykład)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [WindowsSpecific](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
