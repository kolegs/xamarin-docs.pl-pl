---
title: Praca z przycisków
description: Ten artykuł obejmuje projektowanie i Praca z przycisków wewnątrz aplikacji Xamarin.tvOS.
ms.prod: xamarin
ms.assetid: DA6EF400-A4E3-4245-A0D4-F2398CAE2C9B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/07/2017
ms.openlocfilehash: e915e96690fe67f0e704ec558313427f01753438
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-buttons"></a>Praca z przycisków

_Ten artykuł obejmuje projektowanie i Praca z przycisków wewnątrz aplikacji Xamarin.tvOS._


Użyj wystąpienia `UIButton` klasy w celu utworzenia focusable, wybranie przycisku w oknie systemu tvOS. Gdy użytkownik wybierze przycisk, wysyła komunikat akcji do obiektu docelowego Zezwalaj wejściowych użytkownika Xamarin.tvOS aplikacji odpowiadanie na żądania użytkownika.

[![](buttons-images/buttons01.png "Przykład przycisków")](buttons-images/buttons01.png#lightbox)

Aby uzyskać więcej informacji dotyczących pracy z fokusem ani nawigować za pomocą zdalnego Siri, zobacz nasze [Praca z nawigacji i skoncentrować się](~/ios/tvos/app-fundamentals/navigation-focus.md) i [Siri zdalnego oraz kontrolerów Bluetooth](~/ios/tvos/platform/remote-bluetooth.md) dokumentacji.

<a name="About-Buttons" />

## <a name="about-buttons"></a>Przyciski — informacje

W systemu tvOS przyciski są używane przez akcje specyficzny dla aplikacji i może zawierać tytuł, ikony lub obu. Jako użytkownik nawiguje przy użyciu interfejsu użytkownika aplikacji [zdalnego Siri](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote), otrzymuje fokus przycisku danego, dzięki czemu zmienić kolory tła i tekstu. Cień jest również stosowany do przycisk dodawania efekt 3W ją do wzrostu powyżej pozostała część interfejsu użytkownika.

[![](buttons-images/buttons01.png "Przykład przycisków")](buttons-images/buttons01.png#lightbox)

Apple ma poniższe sugestie dotyczące pracy z przycisków:

- **Użyj a tytuł lub ikonę** — podczas obu ikonę i tytuł można dołączyć do przycisku, ilość miejsca jest ograniczona dlatego nie należy łączyć zarówno.
- **Wyraźnie przycisków destrukcyjnego znacznik** — Jeśli destrukcyjnego wykonuje przycisku akcji (np. usunięcie pliku), wyraźnie Oznacz ją jako takie przy użyciu tekstu i/lub ikonę. Akcje destrukcyjnego zawsze powinni przedstawić [alertu](~/ios/tvos/user-interface/alerts.md) użytkownikowi ograniczenie akcji.
- **Użyj przyciski Wstecz nie** — przycisk Menu na komputerze zdalnym Siri służy do zwracania do poprzedniego ekranu. Jedynym wyjątkiem od tej reguły jest zakupy w aplikacji lub destrukcyjne działania gdzie **anulować** powinien być wyświetlany przycisk.

Aby uzyskać więcej informacji na temat pracy z fokusem i nawigacja, zobacz nasze [Praca z nawigacji i skoncentrować się](~/ios/tvos/app-fundamentals/navigation-focus.md) dokumentacji.

<a name="Button-Icons" />

### <a name="button-icons"></a>Przycisk ikon

Apple sugeruje, użyj prostego, upraszcza obrazy dla ikony przycisku. Ikony zbyt skomplikowane są trudne do rozpoznaje na ekranie TV w pomieszczeniu na kanapie, dlatego spróbuj użyć najprostszym reprezentacja można uzyskać pomysł. Jeśli to możliwe, użyj standard, także znać obrazów dla ikon (na przykład Lupa wyszukiwania).

<a name="Button-Titles" />

### <a name="button-titles"></a>Przycisk tytułów

Podczas tworzenia tytuły z przycisków, Apple ma poniższe sugestie:

- **Pokaż opisowy tekst ikony poniżej przyciski** — w przypadku, gdy to możliwe, miejscu wyczyść opisowy tekst poniżej ikona tylko przyciski dalsze Get celu przycisku między.
- **Używaj zleceń ani fraz zlecenie tytułu** -wyraźnie stanu miejsce akcję, która spowoduje przejście, gdy użytkownik kliknie przycisk.
- **Stosowanie liter styl tytułu** — z wyjątkiem artykuły, spójników lub przyimki (cztery litery lub mniej), każdego wyrazu tytuł przycisku należy wpisać wielkimi literami.
- **Użyj krótki, tytuł do punktu** — opis przycisku akcji za pomocą najmniejszej możliwe wyrażenia.

<a name="Buttons-and-Storyboards" />

## <a name="buttons-and-storyboards"></a>Przyciski i planów

Najprostszym sposobem, aby pracować z przycisków w aplikacji Xamarin.tvOS jest dodanie ich do interfejsu użytkownika aplikacji przy użyciu narzędzia Projektant Xamarin dla systemu iOS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


1. W **Eksploratora rozwiązań**, kliknij dwukrotnie `Main.storyboard` pliku i otwórz go do edycji.
1. Przeciągnij **przycisk** z **biblioteki** i upuść go w widoku: 

    [![](buttons-images/storyboard01.png "A button")](buttons-images/storyboard01.png#lightbox)
1. W **Explorer właściwości**, można dostosować kilka właściwości przycisku, takie jak jego **tytuł** i **kolor tekstu**: 

    [![](buttons-images/storyboard02.png "Właściwości przycisku")](buttons-images/storyboard02.png#lightbox)
1. Następnie należy przełączyć się do **kartę zdarzenia** oraz przewodowy **zdarzeń** z **przycisk** i nadaj mu `ButtonPressed`: 

    [![](buttons-images/storyboard03.png "Na karcie zdarzenia")](buttons-images/storyboard03.png#lightbox)
1. Użytkownik zostanie automatycznie przełączona na `ViewController.cs` umieszczane nową akcję w kodzie za pomocą widoku **się** i **dół** klawisze strzałek: 

    [![](buttons-images/storyboard04.png "Umieszczenie nową akcję w kodzie")](buttons-images/storyboard04.png#lightbox)
1. Naciśnij klawisz **Enter** aby wybrać lokalizację: 

    [![](buttons-images/storyboard05.png "Edytor kodu")](buttons-images/storyboard05.png#lightbox)
1. Zapisz zmiany do wszystkich plików.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie `Main.storyboard` pliku i otwórz go do edycji.
1. Przeciągnij **przycisk** z **biblioteki** i upuść go w widoku: 

    [![](buttons-images/storyboard01vs.png "A button")](buttons-images/storyboard01vs.png#lightbox)
1. W **Explorer właściwości**, można dostosować kilka właściwości przycisku, takie jak jego **tytuł** i **kolor tekstu**: 

    [![](buttons-images/storyboard02vs.png "W Eksploratorze właściwości")](buttons-images/storyboard02vs.png#lightbox)
1. Następnie należy przełączyć się do **kartę zdarzenia** oraz przewodowy **zdarzeń** z **przycisk** i nadaj mu `ButtonPressed`: 

    [![](buttons-images/storyboard03vs.png "Na karcie zdarzenia")](buttons-images/storyboard03vs.png#lightbox)
1. Zapisz zmiany do wszystkich plików.



Edytuj kontroler widoku (przykład `ViewController.cs`) i Dodaj następujący kod, aby obsłużyć przycisk jest zaznaczony:


```

using System;
using UIKit;

namespace tvRemote
{
    public partial class ViewController : UIViewController
    {
        ...

        partial void ButtonPressed (UIButton sender) {
            // Handle click here
            ...
        }
    }
}

```

-----

Tak długo, jak przycisk `Enabled` właściwość jest `true` i nie jest objęty przez inny formant lub widok, można go było przy użyciu zdalnego Siri elementu fokusu. Jeśli użytkownik wybierze przycisk i kliknie przycisk powierzchni Touch `ButtonPressed` będzie można wykonać akcji określonej powyżej.

> [!IMPORTANT]
> W trakcie można przypisać akcje, takie jak `TouchUpInside` do `UIButton` w systemie iOS projektanta podczas tworzenia **obsługi zdarzeń**, nigdy nie będzie ona wywoływana ponieważ Apple TV, nie ma dotykowego ekranu lub obsługuje zdarzenia touch. Należy zawsze używać domyślnej **typ akcji** podczas tworzenia **akcje** dla elementów interfejsu użytkownika systemu tvOS.




Aby uzyskać więcej informacji na temat pracy z Scenorys, zobacz nasze [Hello, przewodnik Szybki Start systemu tvOS](~/ios/tvos/get-started/hello-tvos.md).

<a name="Buttons-and-Code" />

## <a name="buttons-and-code"></a>Przyciski i kod

Opcjonalnie `UIButton` można tworzyć w kodzie języka C# i dodać do aplikacji systemu tvOS widoku. Na przykład:

```csharp
var button = new UIButton(UIButtonType.System);
button.Frame = new CGRect (25, 25, 300, 150);
button.SetTitle ("Hello", UIControlState.Normal);
button.AllEvents += (sender, e) => {
    // Do something when the button is clicked
    ...
};
View.AddSubview (button);
```

Podczas tworzenia nowego `UIButton` w kodzie, należy określić jego `UIButtonType` jako jedną z następujących czynności:

- **System** — jest to standardowy typ przycisku przedstawiony przez systemu tvOS, typu, która będzie używana najczęściej.
- **DetailDisclosure** — przedstawia "Zmniejsz" typ przycisku używane do ukrywania lub pokazywania szczegółowych informacji.
- **InfoDark** -ciemnego szczegółowe informacje "i" wyświetlany przycisk w okręgu.
- **InfoLight** -światło szczegółowe informacje "i" wyświetlany przycisk w okręgu.
- **AddContact** — przycisk wyświetlany jako przycisk Dodaj kontakt.
- **Niestandardowe** — umożliwia dostosowanie kilka cech przycisku.

Następnie można zdefiniować na ekranie rozmiar i położenie przycisku. Przykład:

```csharp
button.Frame = new CGRect (25, 25, 300, 150);
```

Ustaw tytuł dla przycisku. `UIButtons` są inne niż większość `UIKit` kontrolki, w tym mają stan, więc nie można wykonać tylko po prostu zmienić tytuł, należy je zmienić dla danego `UIControlState`. Na przykład:

```csharp
button.SetTitle ("Hello", UIControlState.Normal);
```

Następnie użyj `AllEvents` zdarzeń, aby zobaczyć, gdy użytkownik kliknął przycisk. Przykład:

```csharp
button.AllEvents += (sender, e) => {
    // Do something when the button is clicked
    ...
};
```

Na koniec Dodaj przycisku do widoku, aby go wyświetlić:

```csharp
View.AddSubview (button);
```

> [!IMPORTANT]
> Gdy jest można przypisać akcje, takie jak `TouchUpInside` do `UIButton`, nigdy nie będzie ona wywoływana ponieważ Apple TV, nie ma dotykowego ekranu lub obsługuje zdarzenia touch. Zawsze należy używać zdarzeń takich jak **AllEvents** lub **PrimaryActionTriggered**.




<a name="Styling-a-Button" />

## <a name="styling-a-button"></a>Style przycisku

systemu tvOS udostępnia kilka właściwości `UIButton` który może służyć do zapewnienia jego tytuł i stylem go z kolor tła i obrazów.

<a name="Button-Titles" />

### <a name="button-titles"></a>Przycisk tytułów

Jak widzieliśmy powyżej, `UIButtons` są inne niż większość `UIKit` kontrolki, w tym mają stan, więc nie można wykonać tylko po prostu zmienić tytuł, należy je zmienić dla danego `UIControlState`. Na przykład:

```csharp
button.SetTitle ("Hello", UIControlState.Normal);
```

Można ustawić za pomocą przycisku kolor tytułu `SetTitleColor` metody. Na przykład:

```csharp
button.SetTitleColor (UIColor.White, UIControlState.Normal);
```

I dostosować tytuł w tle przy użyciu `SetTitleShadowColor`. Na przykład:

```csharp
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

Można ustawić cienia tytułu zmiana z *Wgłębiony* do *uwypuklenie* , gdy przycisk zostanie wyróżniona, używając następującego kodu:

```csharp
button.ReverseTitleShadowWhenHighlighted = true;
```

Ponadto można użyć oparte na atrybutach tekstu jako tytuł przycisku. Na przykład:

```csharp
var normalAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle (normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle (highlightedAttributedTitle, UIControlState.Highlighted);
```

### <a name="button-images"></a>Obrazy dla przycisków

A `UIButton` może mieć do niego dołączony obrazu i służy obrazu tła.

Aby ustawić obraz tła przycisku dla danego `UIControlState`, użyj następującego kodu:

```csharp
button.SetBackgroundImage(UIImage.FromFile("my image.png"), UIControlState.Normal);
```

Ustaw `AdjustsImageWhenHiglighted` właściwości `true` do rysowania jaśniejszy obraz, gdy przycisk zostanie wyróżniona (jest to wartość domyślna). Ustaw `AdjustsImageWhenDisabled` właściwości `true` do rysowania ciemniejszego obrazu, gdy przycisk jest wyłączone (ponownie, jest to wartość domyślna).

Aby ustawić obraz wyświetlany na przycisku, użyj następującego kodu:

```csharp
button.SetImage(UIImage.FromFile("my image.png"), UIControlState.Normal);
```

Użyj `TintColor` właściwości, aby ustawić kolor odcienia, stosowany do tytułu i obrazu przycisku. W przypadku przycisków z `Custom` typu, ta właściwość nie ma wpływu, musisz zaimplementować `TintColor` zachowanie samodzielnie.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego projektowanie i Praca z przycisków wewnątrz aplikacji Xamarin.tvOS. Go pokazano, jak pracować z przycisków w systemie iOS projektanta i tworzenie przycisków w kodzie języka C#. Na koniec go pokazano, jak zmodyfikować przycisk tytuł i Zmień styl i wyglądu.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [systemu tvOS człowieka przewodniki — interfejs](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Przewodnik programowania w języku aplikacji dla systemu tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
