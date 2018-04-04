---
title: Ograniczenia programowe układu
description: Ten przewodnik przedstawia informacje o pracy z systemem iOS ograniczenia automatycznie Rozmieść elementy w kodzie języka C# zamiast tworzenia ich w systemie iOS projektanta.
ms.prod: xamarin
ms.assetid: 119C8365-B470-4CD4-85F7-086F0A46DCBB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 093101d09f5ffff637b034b3b4794aa6e785a0df
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="programmatic-layout-constraints"></a>Ograniczenia programowe układu

_Ten przewodnik przedstawia informacje o pracy z systemem iOS ograniczenia automatycznie Rozmieść elementy w kodzie języka C# zamiast tworzenia ich w systemie iOS projektanta._

Automatycznie Rozmieść (zwane również "adaptacyjną układu") jest to podejście elastyczny projekt. W przeciwieństwie do systemu układu przejściowe, gdzie każdy element lokalizacji jest ustalony na punkcie ekranu, jest automatycznie układu o *relacje* -położenia elementów do innych elementów na powierzchni projektu. Istotą układu automatycznego jest ograniczenia lub reguły określające położenie element lub zbiór elementów w kontekście innych elementów na ekranie. Ponieważ elementy nie są związane z określonej pozycji na ekranie, warunki ograniczające zmniejszają tworzenie adaptacyjnych układu, który wygląda dobrze na różnych rozmiarów ekranu i orientacji urządzenia.

Zwykle podczas pracy z układem automatycznie w systemie iOS, użyjesz iOS projektanta graficznie umieścić ograniczenia układ elementów interfejsu użytkownika. Jednak może być potrzebne do tworzenia i stosowania ograniczeń w kodzie języka C#. Na przykład przy użyciu dynamicznie utworzenia elementów interfejsu użytkownika dodano do `UIView`.

W tym przewodniku opisano, jak utworzyć i pracować z ograniczeniami przy użyciu kodu C# zamiast tworzenia ich graficznie w systemie iOS projektanta.

<a name="Creating-Constraints-Programmatically" />

## <a name="creating-constraints-programmatically"></a>Programowe tworzenie ograniczenia

Jak już wspomniano, zwykle można będzie działać z ograniczeniami układu automatycznego w systemie iOS projektanta. W takich sytuacjach, w których masz utworzone programowo dostępne są trzy opcje do wyboru:

* [Kotwice układu](#Layout-Anchors) — ten interfejs API umożliwia dostęp do właściwości zakotwiczenia (takich jak `TopAnchor`, `BottomAnchor` lub `HeightAnchor`) elementów interfejsu użytkownika niewidocznej.
* [Ograniczenia układu](#Layout-Constraints) — można utworzyć ograniczenia bezpośrednio przy użyciu `NSLayoutConstraint` klasy.
* [Język formatowania Visual](#Visual-Format-Language) — zapewnia grafikę ASCII, takich jak metody do definiowania ograniczeń sieci.

Poniższe sekcje będą przekazywane każdej opcji szczegółowo.

<a name="Layout-Anchors" />

### <a name="layout-anchors"></a>Kotwice układu

Za pomocą `NSLayoutAnchor` klasa, ma interfejs fluent tworzenia ograniczenia na podstawie właściwości zakotwiczenia niewidocznej elementów interfejsu użytkownika. Na przykład, układu górny i dolny widok kontroler prowadzi ujawnia `TopAnchor`, `BottomAnchor` i `HeightAnchor` właściwości zakotwiczenia, podczas gdy widok udostępnia właściwości krawędzi, Centrum, rozmiar i linii bazowej.

> [!IMPORTANT]
> Oprócz standardowego zestawu właściwości zakotwiczenia również obejmować widoków dla systemu iOS `LayoutMarginsGuides` i `ReadableContentGuide` właściwości. Udostępnianie tych właściwości `UILayoutGuide` obiekty do pracy z marginesami widoku i możliwych do odczytu zawartości odpowiednio przewodników.

Kotwice układu zawierają kilka metod tworzenia ograniczenia w łatwy do odczytania, compact formacie:

- **ConstraintEqualTo** — definiuje relację gdzie `first attribute = second attribute + [constant]` z opcjonalnie podana `constant` wartość przesunięcia.
- **ConstraintGreaterThanOrEqualTo** — definiuje relację gdzie `first attribute >= second attribute + [constant]` z opcjonalnie podana `constant` wartość przesunięcia.
- **ConstraintLessThanOrEqualTo** — definiuje relację gdzie `first attribute <= second attribute + [constant]` z opcjonalnie podana `constant` wartość przesunięcia.

Na przykład:

```csharp
// Get the parent view's layout
var margins = View.LayoutMarginsGuide;

// Pin the leading edge of the view to the margin
OrangeView.LeadingAnchor.ConstraintEqualTo (margins.LeadingAnchor).Active = true;

// Pin the trailing edge of the view to the margin
OrangeView.TrailingAnchor.ConstraintEqualTo (margins.TrailingAnchor).Active = true;

// Give the view a 1:2 aspect ratio
OrangeView.HeightAnchor.ConstraintEqualTo (OrangeView.WidthAnchor, 2.0f);
```

Typowe ograniczenia układu może zostać wyrażona jako wyrażenie liniowej. Wykonaj poniższy przykład:

[![](programmatic-layout-constraints-images/graph01.png "Ograniczenie układu wyrażone jako wyrażenie liniowy")](programmatic-layout-constraints-images/graph01.png#lightbox)

Czy którego można przekonwertować na następujący wiersz kodu C# za pomocą kotwice układu:

```csharp
PurpleView.LeadingAnchor.ConstraintEqualTo (OrangeView.TrailingAnchor, 10).Active = true; 
```

Gdzie części kodu C# odpowiada danej części równanie w następujący sposób:

|Równości|Kod|
|---|---|
|Element 1|PurpleView|
|Atrybut 1|LeadingAnchor|
|Relacji|ConstraintEqualTo|
|Mnożnik|Wartość domyślna to 1.0, dlatego nie określono|
|Element 2|OrangeView|
|Atrybut 2|TrailingAnchor|
|Stała|10.0|

Oprócz parametrów, które są wymagane do rozwiązania równości ograniczenia układ danego każdej z metod zakotwiczenia układu wymusić zabezpieczenie typów parametrów przekazanych do nich. Ograniczenie tak poziome kotwice takich jak `LeadingAnchor` lub `TrailingAnchor` może być używany tylko z innych poziomych kotwicą typów i mnożników są udostępniane tylko ograniczenia rozmiaru.

<a name="Layout-Constraints" />

### <a name="layout-constraints"></a>Ograniczenia układu

Można dodać ograniczenia układu automatycznego ręcznie bezpośrednio tworząc `NSLayoutConstraint` w kodzie języka C#. W przeciwieństwie do usługi kotwice układu, musisz określić wartość dla każdego parametru, nawet jeśli go nie odniesie skutku ograniczenie definiowanego. W związku z tym spowoduje utworzenie znaczną ilość trudny do odczytania schematyczny kod służący do produkcji. Na przykład:

```csharp
//// Pin the leading edge of the view to the margin
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Leading, NSLayoutRelation.Equal, View, NSLayoutAttribute.LeadingMargin, 1.0f, 0.0f).Active = true;

//// Pin the trailing edge of the view to the margin
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Trailing, NSLayoutRelation.Equal, View, NSLayoutAttribute.TrailingMargin, 1.0f, 0.0f).Active = true;

//// Give the view a 1:2 aspect ratio
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Height, NSLayoutRelation.Equal, OrangeView, NSLayoutAttribute.Width, 2.0f, 0.0f).Active = true;
```

Gdzie `NSLayoutAttribute` wyliczenie definiuje wartość dla widoku marginesy i odpowiadają `LayoutMarginsGuide` właściwości, takie jak `Left`, `Right`, `Top` i `Bottom` i `NSLayoutRelation` wyliczenie definiuje relację który zostanie utworzona między podane atrybuty jako `Equal`, `LessThanOrEqual` lub `GreaterThanOrEqual`.

W odróżnieniu od przy użyciu interfejsu API zakotwiczenia układu `NSLayoutConstraint` metod tworzenia nie Zaznacz ważne kwestie związane z określonym ograniczeniem i nie ma żadnych kompilacji wykonać ograniczenie kontroli czasu. W związku z tym jest łatwy do skonstruowania nieprawidłowe ograniczenie, które spowoduje zgłoszenie wyjątku w czasie wykonywania.

<a name="Visual-Format-Language" />

### <a name="visual-format-language"></a>Język Visual formatu

Język Visual Format można zdefiniować ograniczenia przy użyciu ASCII graficznych, takich jak ciągów, które zapewniają wizualną reprezentację ograniczenia tworzona. To ma następujące zalety i wady:

- Język Visual Format wymusza tworzenie tylko prawidłowe ograniczenia.
 - Automatycznie Rozmieść generuje ograniczeń do konsoli przy użyciu Visual Format języka aby komunikaty debugowania będzie podobny kod używany do utworzenia ograniczenia.
 - Visual Format języka umożliwia tworzenie wielu ograniczeń w tym samym czasie, za pomocą bardzo compact wyrażenia.
 - Ponieważ nie ma żadnych kompilacji sprawdzania poprawności ciągów Visual Format języka, problemy można wykryć tylko w czasie wykonywania.
 - Ponieważ Visual Format języka jest kładzie nacisk wizualizacji za pośrednictwem kompletności, niektóre typy ograniczeń nie mogą być tworzone z nim (np. współczynnik).

Podczas korzystania z Visual Format języka Aby utworzyć ograniczenia należy wykonać następujące czynności:

1. Utwórz `NSDictionary` zawierający obiekty widoku i prowadnice i ciągu klucza, który będzie używany podczas definiowania formaty.
2. Opcjonalnie możesz utworzyć `NSDictionary` definiuje zestaw kluczy i wartości (`NSNumber`) używane jako wartości stałej dla ograniczenia.
3. Utwórz ciąg formatu, który ma być układu pojedyncza kolumna lub wiersz elementów.
4. Wywołanie `FromVisualFormat` metody `NSLayoutConstraint` klasy do wygenerowania z ograniczeniami.
5. Wywołanie `ActivateConstraints` metody `NSLayoutConstraint` klasy do aktywowania i zastosować ograniczenia.

Na przykład aby utworzyć zarówno zera, jak i ograniczenie końcowe w języku Visual Format, można użyć następujących czynności:

```csharp
// Get views being constrained
var views = new NSMutableDictionary (); 
views.Add (new NSString ("orangeView"), OrangeView);

// Define format and assemble constraints
var format = "|-[orangeView]-|";
var constraints = NSLayoutConstraint.FromVisualFormat (format, NSLayoutFormatOptions.AlignAllTop, null, views);

// Apply constraints
NSLayoutConstraint.ActivateConstraints (constraints);
```

Visual Format języka zawsze tworzy zero ograniczenia punktu dołączony do widoku nadrzędnego marginesy używając odstępy domyślne, ten kod tworzy identyczne wyniki w przykładach przedstawionych powyżej.

Bardziej złożonych wzorów interfejsu użytkownika, takich jak wiele widoków podrzędnych w jednym wierszu Visual Format języka określa zarówno odstępy w poziomie i w pionie. Tak jak w powyższym przykładzie gdzie Określa `AlignAllTop` `NSLayoutFormatOptions` Wyrównuje wszystkie widoki w wiersz lub kolumnę do ich stacjonarne.

Zobacz firmy Apple [Visual Format języka dodatku](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1) niektóre przykłady typowych ograniczeń i Visual gramatyki ciągu formatu.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym przewodniku są przedstawione tworzeniem i pracą z ograniczeniami automatycznie Rozmieść w języku C# zamiast tworzenia ich graficznie w systemie iOS projektanta. Najpierw przeglądał przy użyciu układu zakotwiczenia (`NSLayoutAnchor`) do obsługi układu automatycznego. Następnie go pokazano, jak pracować z ograniczeniami układu (`NSLayoutConstraint`). Na koniec przedstawione za pomocą Visual Format języka dla układu automatycznego.

## <a name="related-links"></a>Linki pokrewne

- [Wprowadzenie do scenorysów](~/ios/user-interface/storyboards/index.md)
- [iOS Designable wskazówki formantów](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Automatycznie Rozmieść przy użyciu projektanta Xamarin dla systemu iOS](~/ios/user-interface/designer/designer-auto-layout.md#modifying-in-code)
- [Apple — programowe tworzenie ograniczenia](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html#//apple_ref/doc/uid/TP40010853-CH16-SW1)
- [Apple — dodatek języka Visual formatu](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1)
