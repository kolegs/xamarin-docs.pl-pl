---
title: Nowe style interfejsu użytkownika
description: W tym artykule omówiono jasnym i ciemnym kompozycje interfejsu użytkownika tego Apple został dodany do systemu tvOS, 10 oraz sposób ich wdrażania w aplikacji Xamarin.tvOS.
ms.prod: xamarin
ms.assetid: 8BC37683-AD9E-45CD-BE40-96965618AD1D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: da75a99e842b13d42251cdd1c5195ec66ff4a513
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="new-user-interface-styles"></a>Nowe style interfejsu użytkownika

_W tym artykule omówiono jasnym i ciemnym kompozycje interfejsu użytkownika tego Apple został dodany do systemu tvOS, 10 oraz sposób ich wdrażania w aplikacji Xamarin.tvOS._

systemu tvOS 10 teraz obsługuje zarówno ciemny, jak i interfejs użytkownika jasny motyw że będą wszystkie UIKit kompilacji w kontroluje automatycznie dostosowania, zgodnie z preferencjami użytkownika. Ponadto dewelopera ręcznie dostosować elementy interfejsu użytkownika na podstawie motywu, który został wybrany przez użytkownika, można zmienić danego motywu.


<a name="About-the-New-User-Interface-Styles" />

## <a name="about-the-new-user-interface-styles"></a>O nowych stylów interfejsu użytkownika

Jak już wspomniano systemu tvOS 10 teraz obsługuje zarówno ciemny, jak i interfejs użytkownika jasny motyw że będą wszystkie UIKit kompilacji w kontroluje automatycznie dostosowania, zgodnie z preferencjami użytkownika.

Użytkownika można przełączyć tego motywu, przechodząc do **ustawienia** > **ogólne** > **wygląd** i przełączanie między **jasny**  i **ciemny**:

[![](user-interface-styles-images/theme01.png "Ustawienia aplikacji")](user-interface-styles-images/theme01.png#lightbox)

Gdy **ciemny** motywu jest zaznaczona, wszystkie elementy interfejsu użytkownika nastąpi przełączenie do prostych tekst na tle ciemny:

[![](user-interface-styles-images/theme02.png "Ciemny motyw")](user-interface-styles-images/theme02.png#lightbox)

Opcja przełączania motyw w dowolnym momencie i użytkownik zrobić tak na podstawie bieżącego działania, w którym znajduje się Apple TV lub godzinę.

Jasny motyw interfejsu użytkownika jest motyw domyślny, a wszelkie istniejące aplikacje systemu tvOS nadal używać motywu jasny, niezależnie od preferencji użytkownika, chyba że zostaną one zmienione dla systemu tvOS 10, aby móc korzystać z ciemnego motywu. Aplikacja systemu tvOS 10 ma również możliwość zastąpienia bieżącego motywu i zawsze używaj jasny i ciemny motyw dla niektórych lub wszystkich jego interfejsie użytkownika.

<a name="Adopting-the-Light-and-Dark-Themes" />

## <a name="adopting-the-light-and-dark-themes"></a>Przyjmowanie jasnym i ciemnym motywów

Do obsługi tej funkcji, Apple został dodany nowy interfejs API do `UITraitCollection` klasy i aplikacji systemu tvOS musi wyrazić zgodę na obsługuje ciemny wygląd (przez ustawienie w jego `Info.plist` pliku).

Aby wyrazić zgodę na jasny i ciemny motyw pomocy technicznej, wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie `Info.plist` plik, aby otworzyć do edycji.
2. Wybierz **źródła** widoku (od dołu edytora).
3. Dodaj nowy klucz i nadaj mu `UIUserInterfaceStyle`: 

    [![](user-interface-styles-images/theme03.png "Klucz UIUserInterfaceStyle")](user-interface-styles-images/theme03.png#lightbox)
4. Pozostaw ustawioną typ `String` , a następnie wprowadź wartość `Automatic`: 

    [![](user-interface-styles-images/theme04.png "Wprowadź automatyczne")](user-interface-styles-images/theme04.png#lightbox)
5. Zapisz zmiany w pliku.

Istnieją trzy możliwe wartości `UIUserInterfaceStyle` klucza:

- **Jasny** -wymusza interfejsu użytkownika aplikacji systemu tvOS, aby zawsze była używana motywu jasny.
- **Ciemny** — wymusza interfejsu użytkownika aplikacji systemu tvOS, aby zawsze używać ciemnego motywu.
- **Automatyczne** -przełączników między jasnym i ciemnym motywem zgodnie z preferencjami użytkownika w ustawieniach. Jest to preferowane ustawienie.

<a name="UIKit-Theme-Support" />

### <a name="uikit-theme-support"></a>Obsługa UIKit kompozycji

Jeśli aplikacja systemu tvOS używa standardowego, wbudowanego `UIView` na podstawie kontrolek będzie automatycznie odpowiadać motywu interfejsu użytkownika, bez żadnej interwencji developer.

Ponadto `UILabel` i `UITextView` automatycznie zmieni kolor na podstawie wybierz motywu interfejsu użytkownika:

- Tekst będzie czarne motywu jasny.
- Tekst będzie biały ciemnego motywu.

Deweloper kiedykolwiek zmienia kolor tekstu ręcznie (albo w Storyboard lub kodu), będzie odpowiedzialny za obsługę zmiany kolor na podstawie motywu interfejsu użytkownika.

<a name="New-Blur-Effects" />

### <a name="new-blur-effects"></a>Nowe efekty rozmycia

Do obsługi motywów jasny i ciemny w aplikacji systemu tvOS 10, Apple został dodany dwa nowe efekty rozmycia. Te nowe efekty spowoduje automatyczne dopasowanie rozmycia na podstawie motywu interfejsu użytkownika, który użytkownik wybrał w następujący sposób:

- `UIBlurEffectStyleRegular` -Używa światło rozmycia motywu jasny i ciemny rozmycia w ciemnym motywem.
- `UIBlurEffectStyleProminent` -Używa bardzo światła rozmycia w motywu jasny i ciemny bardzo rozmycia w ciemnym motywem.

<a name="Working-with-Trait-Collections" />

## <a name="working-with-trait-collections"></a>Praca z kolekcjami cechy

Nowy `UserInterfaceStyle` właściwość `UITraitCollection` klasy można pobrać aktualnie wybranego motywu interfejsu użytkownika i będą `UIUserInterfaceStyle` wyliczenia jednego z następujących wartości:

- **Jasny** -motywu jasny interfejsu użytkownika jest zaznaczone.
- **Ciemny** — motyw ciemny interfejsu użytkownika jest zaznaczone.
- **Nieokreślony** — nie został wyświetlony widok na ekranie jeszcze, więc bieżącego motywu interfejsu użytkownika jest nieznany.

Ponadto cechy kolekcje mają następujące funkcje w systemu tvOS 10:
 
- Można dostosować wygląd serwera proxy na podstawie `UserInterfaceStyle` z danym `UITraitCollection` zmiana rzeczy, takich jak obrazy lub element na podstawie motywu kolorów.
- Aplikacja systemu tvOS może obsługiwać zmiany kolekcji cechy przez zastąpienie `TraitCollectionDidChange` metody `UIView` lub `UIViewController` klasy.

> [!IMPORTANT]
> Podgląd wczesne Xamarin.tvOS systemu tvOS 10 nie obsługują w pełni `UIUserInterfaceStyle` dla `UITraitCollection` jeszcze. Pełna obsługa zostanie dodana w przyszłej wersji.




<a name="Customizing-Appearance-Based-on-Theme" />

### <a name="customizing-appearance-based-on-theme"></a>Dostosowywanie wyglądu na podstawie motywu

Dla elementów interfejsu użytkownika, które obsługują proxy wygląd ich wyglądu można dostosować oparte na temat interfejsu użytkownika kolekcji ich cechy. Tak dla danego elementu interfejsu użytkownika projektanta można określić jeden kolor motywu jasny i innym kolorze dla motywu ciemny.

```csharp
button.SetTitleColor (UIColor.Red, UIControlState.Normal);

// TODO - Pseudocode because this isn't currently supported in the preview bindings.
var light = new UITraitCollection(UIUserInterfaceStyle.Light);
var dark = new UITraitCollection(UIUserInterfaceStyle.Dark);

button.ForTraitCollection(light).SetTitleColor (UIColor.Red, UIControlState.Normal);
button.ForTraitCollection(dark).SetTitleColor (UIColor.White, UIControlState.Normal);
```

> [!IMPORTANT]
> Niestety, nie w pełni obsługują systemu tvOS 10 w wersji zapoznawczej Xamarin.tvOS `UIUserInterfaceStyle` dla `UITraitCollection`, więc dostosowania tego typu nie jest jeszcze dostępna. Pełna obsługa zostanie dodana w przyszłej wersji.

<a name="Responding-to-Theme-Changes-Directly" />

### <a name="responding-to-theme-changes-directly"></a>Odpowiada na żądania zmiany motywu bezpośrednio

W dewelopera wymaga lepszą kontrolę nad wyglądem elementu interfejsu użytkownika oparta na wybrany motyw interfejsu użytkownika, można zastępują one `TraitCollectionDidChange` metody `UIView` lub `UIViewController` klasy.

Na przykład:

```csharp
public override void TraitCollectionDidChange (UITraitCollection previousTraitCollection)
{
    base.TraitCollectionDidChange (previousTraitCollection);
    
    // Take action based on the Light or Dark theme
    ...
}
```

<a name="Responding-to-Theme-Changes-Directly" />

### <a name="overriding-a-trait-collection"></a>Zastępowanie kolekcji cechy

Oparte na projekt aplikacji systemu tvOS, mogą wystąpić podczas deweloper musi zastąpić kolekcji cechy danego elementu interfejsu użytkownika i jego określonych motywu interfejsu użytkownika należy zawsze używać razy.

Można to zrobić za pomocą `SetOverrideTraitCollection` metoda `UIViewController` klasy. Na przykład:

```csharp
// Create new trait and configure it
var trait = new UITraitCollection ();
...

// Apply new trait collection
SetOverrideTraitCollection (trait, this);
```

Aby uzyskać więcej informacji, zobacz [cech](~/ios/user-interface/storyboards/unified-storyboards.md) i [zastępowanie cech](~/ios/user-interface/storyboards/unified-storyboards.md) sekcje naszych [wprowadzenie do ujednoliconego Scenorys](~/ios/user-interface/storyboards/unified-storyboards.md) dokumentacji.

<a name="Trait-Collections-and-Storyboards" />

### <a name="trait-collections-and-storyboards"></a>Kolekcje cechy i planów

10 systemu tvOS scenorysu aplikacji można ustawić reagować cechy kolekcji, a wiele elementów interfejsu użytkownika może zostać powiadomieni jasnym i ciemnym motywem. Bieżący Podgląd wczesne Xamarin.tvOS systemu tvOS 10 nie obsługuje tej funkcji w Projektancie interfejsu jeszcze, więc scenorysu, należy w konstruktora interfejsu w środowisku Xcode jako rozwiązanie alternatywne można edytować.

Aby włączyć obsługę cechy kolekcji, wykonaj następujące czynności:

1. Kliknij prawym przyciskiem myszy plik scenorysu w **Eksploratora rozwiązań** i wybierz **Otwórz za pomocą** > **Xcode interfejsu konstruktora**: 

    [![](user-interface-styles-images/theme05.png "Otwórz za pomocą konstruktora Xcode — interfejs")](user-interface-styles-images/theme05.png#lightbox) 
2. Aby włączyć obsługę cechy kolekcji, przełącz się do **Inspector plików** i sprawdź **Użyj cechy odmiany** właściwości w **dokument konstruktora interfejsu** sekcji: 

    [![](user-interface-styles-images/theme06.png "Włącz obsługę cechy kolekcji")](user-interface-styles-images/theme06.png#lightbox)
3. Potwierdź zmianę w celu używania odmiany cechy: 

    [![](user-interface-styles-images/theme07.png "Użyj odmiany cechy alertu")](user-interface-styles-images/theme07.png#lightbox)
4. Zapisz zmiany w pliku scenorysu.

Podczas edycji systemu tvOS Scenorys konstruktora interfejsu, firmy Apple został dodany następujące możliwości:

* Deweloper można określić różnych zmian elementów interfejsu użytkownika na podstawie motywu interfejsu użytkownika w **inspektora atrybutu**:
    
    * Kilka właściwości już **+** obok nich, które można kliknąć, aby dodać określoną wersję motywu interfejsu użytkownika: 

        [![](user-interface-styles-images/theme08.png "Dodaj określoną wersję motywu interfejsu użytkownika")](user-interface-styles-images/theme08.png#lightbox) 
    
    * Deweloper może określić nową właściwość lub kliknij przycisk **x** przycisk, aby usunąć go: 

        [![](user-interface-styles-images/theme09.png "Określ nową właściwość lub kliknij przycisk x w celu usunięcia go")](user-interface-styles-images/theme09.png#lightbox)
* Deweloper może Podgląd interfejsu użytkownika w jasny i ciemny motyw z wewnątrz konstruktora interfejsu:
    
    * Dolnej części powierzchni projektu umożliwia deweloperowi przełącznika bieżącego motywu interfejsu użytkownika: 

        [![](user-interface-styles-images/theme10.png "Dolnej części powierzchni projektowej")](user-interface-styles-images/theme10.png#lightbox)
        
    * Nowy motyw będą wyświetlane w konstruktora interfejsu i zostanie wyświetlony dopasowania określonej kolekcji cechy: 

        [![](user-interface-styles-images/theme11.png "Motyw wyświetlane w interfejsie konstruktora")](user-interface-styles-images/theme11.png#lightbox)

Ponadto systemu tvOS symulatora ma teraz skrótu klawiaturowego, który umożliwia deweloperom szybkie przełączanie się między jasnym i ciemnym kompozycji podczas debugowania aplikacji systemu tvOS. Użyj **polecenia-Shift-D** klawiatury sekwencji, aby przełączyć między jasnym i ciemnym.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego jasnym i ciemnym kompozycje interfejsu użytkownika tego Apple został dodany do systemu tvOS, 10 oraz sposób ich wdrażania w aplikacji Xamarin.tvOS.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [What's new in systemu tvOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
