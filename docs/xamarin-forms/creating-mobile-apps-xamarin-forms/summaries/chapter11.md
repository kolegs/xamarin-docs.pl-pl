---
title: Podsumowanie rozdziałów 11. Infrastruktura z możliwością wiązania
description: 'Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms: Podsumowanie rozdziału 11. Infrastruktura z możliwością wiązania'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 34671C48-0ED4-4B76-A33D-D6505390DC5B
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 78a0a7d4ce3a3de52f281237429cebb38f3b8e52
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998784"
---
# <a name="summary-of-chapter-11-the-bindable-infrastructure"></a>Podsumowanie rozdziałów 11. Infrastruktura z możliwością wiązania

Każdy programista języka C# są zaznajomieni z C# *właściwości*. Właściwości zawierają *ustaw* metody dostępu i/lub *uzyskać* metody dostępu. Są często nazywane *właściwości aparatu CLR* dla środowiska uruchomieniowego języka wspólnego.

Xamarin.Forms definiuje właściwości rozszerzone mającą *właściwości możliwej do wiązania* zamknięte przez [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) klasy i obsługiwane przez [ `BindableObject` ](xref:Xamarin.Forms.BindableObject)klasy. Te klasy są powiązane, ale raczej odrębnym: `BindableProperty` służy do definiowania właściwości. `BindableObject` przypomina `object` , to klasa bazowa dla klas definiujących, które można powiązać właściwości.

## <a name="the-xamarinforms-class-hierarchy"></a>Hierarchia klas zestawu narzędzi Xamarin.Forms

[ **ClassHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/ClassHierarchy) próbka używa odbicia, aby wyświetlić hierarchię klas platformy Xamarin.Forms i zaprezentować kluczową rolę odgrywaną przez `BindableObject` w tej hierarchii. `BindableObject` pochodzi od klasy `Object` i klasa nadrzędna [ `Element` ](xref:Xamarin.Forms.Element) z którego [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) pochodzi. Jest to klasa nadrzędna [ `Page` ](xref:Xamarin.Forms.Page) i [ `View` ](xref:Xamarin.Forms.View), który jest klasa nadrzędna [ `Layout` ](xref:Xamarin.Forms.Layout):

[![Potrójna zrzut ekranu przedstawiający hierarchii klas, udostępnianie](images/ch11fg01-small.png "udostępnianie hierarchii klas")](images/ch11fg01-large.png#lightbox "udostępnianie hierarchii klas")

## <a name="a-peek-into-bindableobject-and-bindableproperty"></a>Wgląd w BindableObject i BindableProperty

W klasach, które wynikają z `BindableObject` wiele właściwości środowiska CLR są określane jako był "zabezpieczony za pomocą" właściwości możliwej do wiązania. Na przykład [ `Text` ](xref:Xamarin.Forms.Label.Text) właściwość `Label` klasy jest właściwość CLR, ale `Label` klasa definiuje również publicznych statyczne pole tylko do odczytu o nazwie [ `TextProperty` ](xref:Xamarin.Forms.Label.TextProperty) programu Typ `BindableProperty`.

Aplikacja może ustawić lub pobrać `Text` właściwość `Label` zwykle lub aplikacji można ustawić `Text` przez wywołanie metody [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) metody zdefiniowanej przez `BindableObject` z `Label.TextProperty` argument. Podobnie, aplikację można uzyskać wartość `Text` właściwości przez wywołanie metody [ `GetValue` ](xref:Xamarin.Forms.BindableObject.GetValue(Xamarin.Forms.BindableProperty)) metody ponownie, używając `Label.TextProperty` argumentu. Wskazuje na [ **PropertySettings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PropertySettings) próbki.

W rzeczywistości `Text` właściwość CLR całkowicie jest implementowany przy użyciu `SetValue` i `GetValue` metody zdefiniowane przez `BindableObject` w połączeniu z `Label.TextProperty` właściwość statyczna.

`BindableObject` i `BindableProperty` zapewnia pomoc techniczną dla:

- Podając wartości domyślne właściwości
- Przechowywanie ich bieżącymi wartościami
- Zapewnianie mechanizmy sprawdzania poprawności wartości właściwości
- Utrzymania spójności między powiązane właściwości w jednej klasy
- Reagowanie na zmiany właściwości
- Wyzwalanie powiadomienia, gdy właściwość ma zostać zmieniona lub został zmieniony
- Obsługa powiązań danych
- Obsługa stylów
- Dynamiczne zasoby pomocnicze

Zawsze, gdy właściwość, która jest wspierana przez zmianę właściwości możliwej do wiązania, `BindableObject` generowane [ `PropertyChanged` ](xref:Xamarin.Forms.BindableObject.PropertyChanged) zdarzeń identyfikująca właściwość, które uległy zmianie. To zdarzenie nie jest uruchamiany, gdy właściwość jest ustawiona na tę samą wartość.

Niektóre właściwości nie są wspierane przez właściwości możliwej do wiązania, a niektóre klasy zestawu narzędzi Xamarin.Forms &mdash; takich jak `Span` &mdash; pochodzi od `BindableObject`. Tylko klasę, która jest pochodną `BindableObject` może obsługiwać właściwości możliwej do wiązania, ponieważ `BindableObject` definiuje `SetValue` i `GetValue` metody.

Ponieważ `Span` nie pochodzi od `BindableObject`, żaden z jej właściwości &mdash; takich jak `Text` &mdash; są wspierane przez właściwości możliwej do wiązania. Dlatego `DynamicResource` ustawienie `Text` właściwość `Span` zgłasza wyjątek w [ **DynamicVsStatic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) próbki w poprzednim rozdziale. [ **DynamicVsStaticCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/DynamicVsStaticCode) przykład pokazuje, jak ustawić dynamicznych zasobów przy użyciu kodu [ `SetDynamicResource` ](xref:Xamarin.Forms.Element.SetDynamicResource(Xamarin.Forms.BindableProperty,System.String)) metody zdefiniowanej przez `Element`. Pierwszy argument jest obiektem typu `BindableProperty`.

Podobnie [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) metody zdefiniowanej przez `BindableObject` ma pierwszy argument typu `BindableProperty`.

## <a name="defining-bindable-properties"></a>Definiowanie właściwości możliwej do wiązania

Można zdefiniować własne właściwości możliwej do wiązania za pomocą statycznych [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) metodą tworzenia statycznego pola tylko do odczytu typu `BindableProperty`.

Jest to zaprezentowane w [ `AltLabel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AltLabel.cs) klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteki. Klasa pochodzi z `Label` i pozwala określić rozmiar czcionki w punktach. Wspomniane [ **PointSizedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PointSizedText) próbki.

Cztery argumenty `BindableProperty.Create` metody są wymagane:

- `propertyName`: tekstowa nazwa właściwości (taka sama jak nazwa właściwości środowiska CLR)
- `returnType`: typ właściwości CLR
- `declaringType`: typ klasy deklarowanie właściwości
- `defaultValue`: wartość domyślna właściwości

Ponieważ `defaultValue` typu `object`, kompilator musi być w stanie określić wartość domyślną typu. Na przykład jeśli `returnType` jest `double`, `defaultValue` należy ustawić na wartość podobną 0.0, a nie po prostu 0 lub niezgodność typów wyzwoli wyjątek w czasie wykonywania.

Jest również bardzo popularne, które można powiązać właściwości do uwzględnienia:

- `propertyChanged`: statyczne metoda wywoływana, gdy zmieni wartość. Pierwszy argument jest wystąpieniem klasy, których właściwość został zmieniony.

Inne argumenty `BindableProperty.Create` nie są jako wspólne:

- `defaultBindingMode`: używane w połączeniu z powiązanie danych (zgodnie z opisem w [ **rozdział 16. Powiązanie danych**](chapter16.md))
- `validateValue`: wywołanie zwrotne pod kątem prawidłowej wartości
- `propertyChanging`: wywołanie zwrotne w celu wskazania, gdy właściwość ma zostać zmieniona
- `coerceValue`: wywołanie zwrotne w celu coerce — ustaw wartość na inną wartość
- `defaultValueCreate`: wywołanie zwrotne do utworzenia wartość domyślną, która nie może być współużytkowane przez wystąpienia klasy (na przykład kolekcji)

### <a name="the-read-only-bindable-property"></a>Właściwość może być powiązana tylko do odczytu

Właściwości możliwej do wiązania może być tylko do odczytu. Tworzenie tylko do odczytu właściwości możliwej do wiązania wymagane jest wywołanie metody statycznej [ `BindableProperty.CreateReadOnly` ](xref:Xamarin.Forms.BindableProperty.CreateReadOnly(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) Aby zdefiniować prywatny statyczne pole tylko do odczytu typu [ `BindablePropertyKey` ](xref:Xamarin.Forms.BindablePropertyKey).

Następnie zdefiniuj właściwość CLR `set` accesor jako `private` do wywołania [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindablePropertyKey,System.Object)) przeciążenia z `BindablePropertyKey` obiektu. Zapobiega to ustawiania poza klasą właściwości.

Jest to zaprezentowane w [ `CountedLabel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CountedLabel.cs) klasa używana w [ **BaskervillesCount** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/BaskervillesCount) próbki.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst działu 11 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch11-Apr2016.pdf)
- [Przykłady działu 11](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11)
