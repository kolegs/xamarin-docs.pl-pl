---
title: "Podsumowanie rozdziału 11. Powiązania infrastruktury"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 3e3cfb55f7b96751979d14b489e892bc07817780
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-11-the-bindable-infrastructure"></a>Podsumowanie rozdziału 11. Powiązania infrastruktury

Każdy programisty C# jest znajomość języka C# *właściwości*. Właściwości zawierają *ustawić* metody dostępu i/lub *uzyskać* metody dostępu. Są one często nazywane *CLR właściwości* dla środowiska CLR.

Platformy Xamarin.Forms definiuje definicji właściwości rozszerzonych o nazwie *właściwości możliwej do wiązania* zamknięte przez [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) klasy i obsługiwane przez [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)klasy. Te klasy są powiązane, ale dość distinct: `BindableProperty` służy do definiowania właściwości. `BindableObject` przypomina `object` w tym jest klasę podstawową dla klas definiujących właściwości.

## <a name="the-xamarinforms-class-hierarchy"></a>Hierarchia klas platformy Xamarin.Forms

[ **ClassHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/ClassHierarchy) w przykładzie użyto odbicia do wyświetlania hierarchii klasy platformy Xamarin.Forms i prezentacja sprawą kluczową rolę odgrywaną przez `BindableObject` w tej hierarchii. `BindableObject` pochodną `Object` i jest to klasa nadrzędna do [ `Element` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/) z którego [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) pochodzi. Jest to klasa nadrzędna [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) i [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/), która jest klasa nadrzędna [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/):

[![Potrójna zrzut ekranu przedstawiający hierarchii klas udostępnianie](images/ch11fg01-small.png "udostępniania hierarchii klasy")](images/ch11fg01-large.png "udostępniania hierarchii — klasa")

## <a name="a-peek-into-bindableobject-and-bindableproperty"></a>Peek w BindableObject i BindableProperty

W klasach pochodzących od `BindableObject` wiele właściwości CLR są określane jako "kopii przez" właściwości. Na przykład [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) właściwość `Label` klasy jest właściwość CLR, ale `Label` klasy definiuje również publiczne statyczne tylko do odczytu pola o nazwie [ `TextProperty` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextProperty/) z Typ `BindableProperty`.

Aplikację można ustawiać ani pobierać `Text` właściwość `Label` zwykle lub aplikacji można ustawić `Text` przez wywołanie metody [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) metody zdefiniowanej przez `BindableObject` z `Label.TextProperty` argument. Podobnie, aplikację można uzyskać wartość `Text` właściwości przez wywołanie metody [ `GetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.GetValue/p/Xamarin.Forms.BindableProperty/) metody ponownie, podając `Label.TextProperty` argumentu. Wskazuje na [ **PropertySettings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PropertySettings) próbki.

W rzeczywistości `Text` właściwość CLR całkowicie jest implementowane za pomocą `SetValue` i `GetValue` metody zdefiniowane przez `BindableObject` w połączeniu z `Label.TextProperty` właściwości statycznej.

`BindableObject` i `BindableProperty` zapewniają obsługę:

- Podając wartości domyślnych właściwości
- Przechowywanie ich bieżącymi wartościami
- Zapewnianie mechanizmów sprawdzania poprawności wartości właściwości
- Obsługa spójność powiązane właściwości w jednej klasy
- Odpowiada na żądania zmiany właściwości
- Wyzwolenie powiadomienia, gdy właściwość ma zostać zmieniona lub został zmieniony
- Obsługa powiązanie danych
- Obsługa stylów
- Obsługa dynamicznego zasobów

Zawsze, gdy właściwość, która nie jest obsługiwana przez zmiany właściwości możliwej do wiązania, `BindableObject` uruchamiany [ `PropertyChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.BindableObject.PropertyChanged/) zdarzeń identyfikujące właściwość, która została zmieniona. To zdarzenie nie jest uruchamiany, gdy właściwość jest ustawiona na tę samą wartość.

Niektóre właściwości są ona obsługiwana przez właściwości oraz niektóre klasy platformy Xamarin.Forms & #x 2014; takie jak `Span` & #x 2014; nie pochodzi od `BindableObject`. Tylko klasy, która pochodzi z `BindableObject` może obsługiwać właściwości, ponieważ `BindableObject` definiuje `SetValue` i `GetValue` metody.

Ponieważ `Span` nie pochodzi od `BindableObject`, żaden z jego właściwości & #x 2014; takich jak `Text` & #x 2014; bazują na właściwości możliwej do wiązania. Jest to dlaczego `DynamicResource` ustawienie `Text` właściwości `Span` zgłasza wyjątek w [ **DynamicVsStatic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) przykładowa w poprzednim rozdziale. [ **DynamicVsStaticCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/DynamicVsStaticCode) przykładzie pokazano sposób ustawiania dynamiczna zasobów za pomocą kodu [ `SetDynamicResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Element.SetDynamicResource/p/Xamarin.Forms.BindableProperty/System.String/) metody zdefiniowanej przez `Element`. Pierwszy argument jest typu obiektu `BindableProperty`.

Podobnie [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) metody zdefiniowanej przez `BindableObject` ma pierwszy argument typu `BindableProperty`.

## <a name="defining-bindable-properties"></a>Definiowanie właściwości

Można zdefiniować własny właściwości możliwej do wiązania za pomocą statycznych [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) metody lub krótszy nieco [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/) przeciążenia, aby utworzyć statycznego pola tylko do odczytu typu `BindableProperty`.

To jest przedstawiona w [ `AltLabel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AltLabel.cs) klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteki. Klasa pochodzi z `Label` i pozwala określić rozmiar czcionki w punktach. Zostało to przedstawione w [ **PointSizedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PointSizedText) próbki.

Cztery argumenty `BindableProperty.Create` metody są wymagane:

- `propertyName`: Nazwa tekst właściwości (taka sama jak nazwa właściwości CLR)
- `returnType`: typ CLR właściwości
- `declaringType`: typ klasy deklarowanie właściwości
- `defaultValue`: wartość domyślna właściwości

Ponieważ `defaultValue` jest typu `object`, kompilator musi być możliwe ustalenie typ wartości domyślnej. Na przykład jeśli `returnType` jest `double`, `defaultValue` powinien być ustawiony na coś 0.0, a nie tylko 0 lub niezgodność typów wyzwoli Wystąpił wyjątek w czasie wykonywania.

Jest również bardzo często można powiązać właściwości do uwzględnienia:

- `propertyChanged`: metody statycznej o nazwie, gdy właściwość zostanie zmieniona wartość. Pierwszy argument jest wystąpieniem klasy, których właściwość został zmieniony.

Argumenty do `BindableProperty.Create` nie są jako wspólne:

- `defaultBindingMode`: używany podczas wiązania z danymi (zgodnie z opisem w [ **rozdziale 16. Powiązanie danych**](chapter16.md))
- `validateValue`: wywołanie zwrotne do sprawdzenia prawidłową wartość
- `propertyChanging`: wywołanie zwrotne, aby wskazać, gdy właściwość ma zostać zmieniona
- `coerceValue`: wywołanie zwrotne do wymuszone ustaw wartość na inną wartość
- `defaultValueCreate`: wywołanie zwrotne do utworzenia wartość domyślną, która nie może być współużytkowane przez wystąpień klasy (na przykład kolekcja)

### <a name="the-read-only-bindable-property"></a>Można powiązać właściwości tylko do odczytu

Można powiązać właściwości mogą być tylko do odczytu. Tworzenie powiązania właściwości tylko do odczytu wymaga wywołania metody statycznej [ `BindableProperty.CreateReadOnly` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateReadOnly/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) lub krótszy [ `BindableProperty.CreateReadOnly` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateReadOnly/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/) do definiowania prywatnego statycznego pola tylko do odczytu typu [ `BindablePropertyKey` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindablePropertyKey/).

Następnie należy określić właściwość CLR `set` accesor jako `private` do wywołania [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindablePropertyKey/System.Object/) przeciążenia z `BindablePropertyKey` obiektu. Zapobiega to ustawiany poza klasą właściwości.

To jest przedstawiona w [ `CountedLabel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CountedLabel.cs) klasa używana w [ **BaskervillesCount** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/BaskervillesCount) próbki.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdziału 11 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch11-Apr2016.pdf)
- [Przykłady rozdziale 11](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11)
