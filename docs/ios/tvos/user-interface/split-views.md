---
title: "Praca z kontrolerami widoku podziału"
description: "Ten artykuł obejmuje projektowanie i Praca z kontrolerami widoku podziału wewnątrz aplikacji Xamarin.tvOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 21248CFB-5A94-4C19-B223-C72E0DC5F1D5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 8787913c04b11a84828cd98960407f0cc27aa391
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-split-view-controllers"></a>Praca z kontrolerami widoku podziału

_Ten artykuł obejmuje projektowanie i Praca z kontrolerami widoku podziału wewnątrz aplikacji Xamarin.tvOS._


Kontroler widoku podziału umożliwia wyświetlanie i zarządza Master i kontrolera widoku szczegółów side-by-side, na ekranie, w tym samym czasie. Podziału widoku kontrolery są używane do przedstawienia stałe, focusable zawartości w widoku głównego (mniejsze sekcji po lewej stronie) i powiązanych szczegółów w widoku szczegółów (większe sekcja po prawej stronie).

[ ![](split-views-images/intro01.png "Przykładowy widok podziału")](split-views-images/intro01.png)

<a name="About-Split-View-Controllers" />

## <a name="about-split-view-controllers"></a>O kontrolerach widoku podziału

Jak już wspomniano, kontrolera widoku podziału zarządza Master i kontrolera widoku szczegółów prezentowany side-by-side, wzorzec jest mniejszy widoku szczegółów większe po prawej stronie po lewej stronie. 

Ponadto może kontrolera widoku głównego zostały ukryte lub pokazane zgodnie z wymaganiami: 

[ ![](split-views-images/intro02.png "Kontroler widoku głównego ukryte")](split-views-images/intro02.png)

Podziel widoków kontrolerów są często używane do prezentowania listę można filtrować zawartość z kategorii w widoku głównego i filtrowane wyniki w widoku szczegółów. Zazwyczaj jest to przedstawione jako widok tabeli po lewej stronie, a [widok kolekcji](~/ios/tvos/user-interface/collection-views.md) po prawej stronie.

Projektowanie interfejsu użytkownika, który wymaga kontrolera widoku podziału, Apple sugeruje przy użyciu wzorca i kontrolerów widoku szczegółów, który nie należy zmieniać (tylko zmiany zawartości, nie struktury). Jeśli potrzebujesz wymiany kontrolerów widoku w poziomie, najlepiej użyć kontrolera do nawigacji jako podstawa kontrolera widoku, którą chcesz zmienić (Master lub szczegółów).

Apple ma poniższe sugestie dotyczące pracy z kontrolerami widoku podziału:

- **Użyj wartości podziału** -kontroler widoku podziału korzysta domyślnie jedna trzecia ekranu dla kontrolera widoku głównego i dwie trzecie kontrolera widoku szczegółów. Opcjonalnie można użyć 50/50 podziału. Wybierz prawidłowe procent aby Twoje zawartości występują zrównoważonym na ekranie.
- **Utrwalanie wybór Main** — podczas zawartości na może widok szczegółów zmiany jest odpowiedź na wybranej przez użytkownika w widoku głównego, należy ustalić zawartość widoku głównego. Ponadto należy wyraźnego wyświetlania aktualnie zaznaczonego elementu w widoku głównego.
- **Użyj pojedynczego Unified tytuł** — zwykle należy użyć jednej, wyśrodkowany tytuł w widoku szczegółów, zamiast tytuł w informacjach szczegółowych i widoku głównego.

<a name="Split-View-Controllers-and-Storyboards" />

## <a name="split-view-controllers-and-storyboards"></a>Podziel kontrolerów widoku i planów

Najprostszym sposobem pracy z kontrolerami widoku podziału w aplikacji Xamarin.tvOS jest dodanie ich do interfejsu użytkownika aplikacji przy użyciu projektanta dla systemu iOS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. W **konsoli rozwiązania**, kliknij dwukrotnie `Main.storyboard` pliku i otwórz go do edycji.
1. Przeciągnij **kontrolerów widoku podziału** z **przybornika** i upuść go w widoku: 

    [ ![](split-views-images/activity01.png "Kontroler widoku podziału")](split-views-images/activity01.png)
1. Domyślnie iOS projektanta zainstaluje kontrolera nawigacji i kontrolera widoku w widoku głównego. Jeśli to nie spełniają wymagania aplikacji, po prostu usuń je.
1. Jeśli usuniesz domyślny widok główny, przeciągnij nowego kontrolera widoku na powierzchnię projektu: 

    [ ![](split-views-images/activity02.png "Kontroler widoku")](split-views-images/activity02.png)
1. Sterowania kliknij i przeciągnij od kontrolera widoku podziału na nowy kontroler widoku głównego. 
1. Wybierz **wzorca** z **Menu podręcznego**: 

    [ ![](split-views-images/activity03.png "Wybierz główny z Menu podręcznego")](split-views-images/activity03.png)
1. Projektowanie zawartość wzorca i widoki szczegółów: 

    [ ![](split-views-images/activity04.png "Przykładowy układ strony")](split-views-images/activity04.png)
1. Przypisz **nazwy** w **kartę Widget** z **konsoli właściwości** do pracy z formantów interfejsu użytkownika w kodzie języka C#.
1. Zapisz zmiany i wróć do programu Visual Studio dla komputerów Mac.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie `Main.storyboard` pliku i otwórz go do edycji.
1. Przeciągnij **kontrolerów widoku podziału** z **przybornika** i upuść go w widoku: 

    [ ![](split-views-images/activity01-vs.png "Kontroler widoku podziału")](split-views-images/activity01-vs.png)
1. Domyślnie iOS projektanta doda kontrolera nawigacji i kontrolera widoku w widoku głównego. Jeśli to nie spełniają wymagania aplikacji, po prostu usuń je.
1. Jeśli usuniesz domyślny widok główny, przeciągnij nowego kontrolera widoku na powierzchnię projektu: 

    [ ![](split-views-images/activity02-vs.png "Kontroler widoku")](split-views-images/activity02-vs.png)
1. Sterowania kliknij i przeciągnij od kontrolera widoku podziału na nowy kontroler widoku głównego. 
1. Wybierz **wzorca** z **Menu podręcznego**: 

    [ ![](split-views-images/activity03-vs.png "Wybierz główny z Menu podręcznego")](split-views-images/activity03-vs.png)
1. Projektowanie zawartość wzorca i widoki szczegółów: 

    [ ![](split-views-images/activity04.png "Układ zawartości")](split-views-images/activity04.png)
1. Przypisz **nazwy** w **kartę Widget** z **Explorer właściwości** do pracy z formantów interfejsu użytkownika w kodzie języka C#.
1. Zapisz zmiany.
    
-----

Aby uzyskać więcej informacji na temat pracy z Scenorys, zobacz nasze [Hello, przewodnik Szybki Start systemu tvOS](~/ios/tvos/get-started/hello-tvos.md).

<a name="Working-with-Split-View-Controllers" />

## <a name="working-with-split-view-controllers"></a>Praca z kontrolerami widoku podziału

Jak już wspomniano, kontrolera widoku podziału jest często używane w sytuacjach, w którym są wyświetlane filtrowane zawartości dla użytkownika. Główne kategorie są wyświetlane po lewej stronie w widoku głównego i filtrowane wyniki po prawej stronie w widoku szczegółów oparte na wybranej przez użytkownika.

<a name="Accessing-Master-and-Detail" />

### <a name="accessing-master-and-detail"></a>Uzyskiwanie dostępu do głównego i szczegóły

Jeśli potrzebujesz dostępu do głównego i kontrolerów widoku szczegółów programowo, użyj `ViewControllers ` właściwości kontrolera widoku podziału. Na przykład:

```csharp
// Gain access to master and detail view controllers
var masterController = ViewControllers [0] as MasterViewController;
var detailController = ViewControllers [1] as DetailViewController;
```

Jest on przedstawiony jako tablica, których szczegóły jest pierwszym elementem (0) w kontroler widoku głównego i drugiego elementu (1).

<a name="Accessing-Detail-from-Master" />

### <a name="accessing-detail-from-master"></a>Dostęp do szczegółów z wzorca

Ponieważ szczegółowe informacje są zwykle wyświetlane w widoku szczegółów oparte na wybranej przez użytkownika w głównym, należy sposób uzyskać dostęp do szczegółów z wzorcem.

Najprostszym sposobem, w tym celu jest na przykład ujawnić właściwości w klasie kontrolera widoku głównego:

```csharp
public DetailViewController DetailController { get; set;}
```

Na kontrolerze widoku podziału zastąpienie `ViewDidLoad` — metoda i grupie równych wartości dwóch widoków jednocześnie. Na przykład:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Gain access to master and detail view controllers
    var masterController = ViewControllers [0] as MasterViewController;
    var detailController = ViewControllers [1] as DetailViewController;

    // Wire-up views
    masterController.SplitViewController = this;
    masterController.DetailController = detailController;
    detailController.SplitViewController = this;
}
```

Właściwości i metody mogą uwidaczniać służącego do przedstawienia nowych danych zgodnie z wymaganiami wzorca kontrolera widoku szczegółów.

<a name="Showing-and-Hiding-Master" />

### <a name="showing-and-hiding-master"></a>Pokazywanie i ukrywanie wzorca

Opcjonalnie można wyświetlać i Ukryj przy użyciu kontrolera widoku głównego `PreferredDisplayMode` właściwości kontrolera widoku podziału. Na przykład:

```csharp
// Show hide split view
if (SplitViewController.DisplayMode == UISplitViewControllerDisplayMode.PrimaryHidden) {
    SplitViewController.PreferredDisplayMode = UISplitViewControllerDisplayMode.AllVisible;
} else {
    SplitViewController.PreferredDisplayMode = UISplitViewControllerDisplayMode.PrimaryHidden;
}
```

`UISplitViewControllerDisplayMode` Wyliczenie definiuje sposób kontroler widoku głównego zostanie wyświetlone jako jedną z następujących czynności:

- **Automatyczne** -systemu tvOS kontroli prezentacji wzorca i widoki szczegółów.
- **PrimaryHidden** — spowoduje to ukrycie kontroler widoku głównego.
- **AllVisible** -wyświetla zarówno głównym, jak i kontrolery widoku szczegółów side-by-side. Jest to normalne, prezentacji domyślne.
- **PrimaryOverlay** -kontrolera widoku szczegółów rozszerza w obszarze i jest objęta wzorca.

Aby uzyskać bieżący stan prezentacji, należy użyć `DisplayMode` właściwości kontrolera widoku podziału.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego projektowanie i Praca z kontrolerami widoku podziału wewnątrz aplikacji Xamarin.tvOS.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [systemu tvOS człowieka przewodniki — interfejs](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Przewodnik programowania w języku aplikacji dla systemu tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
