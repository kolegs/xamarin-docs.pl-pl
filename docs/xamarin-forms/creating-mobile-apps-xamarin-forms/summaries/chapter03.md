---
title: "Podsumowanie rozdział 3. Zapoznanie się z tekstu"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 9d283a4136a7cdfe39ea0b2da65273332fd47b00
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-3-deeper-into-text"></a>Podsumowanie rozdział 3. Zapoznanie się z tekstu

W tym rozdziale Eksploruje [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) widoku bardziej szczegółowo w tym kolorów i czcionek i formatowania.

## <a name="wrapping-paragraphs"></a>Zawijanie akapitów

Gdy [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) właściwość `Label` zawiera długi tekst `Label` jest automatycznie zawijany go do wielu linii zgodnie [ **Baskervilles** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/Baskervilles) próbki. Można osadzić kodów Unicode, takie jak "\u2014"-pauzy lub C# znaków, takich jak "\r" przerwanie i przejście do nowego wiersza.

Gdy [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) i [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) właściwości `Label` są ustawione na `LayoutOptions.Fill`, całkowity rozmiar `Label` podlega miejsce który jego kontenera udostępnia. `Label` Jest określany jako *ograniczonego*. Rozmiar `Label` rozmiar jego kontenera.

Gdy `HorizontalOptions` i `VerticalOptions` właściwości są ustawione na wartości innych niż `LayoutOptions.Fill`, rozmiar `Label` podlega miejsce wymagane do renderowania tekstu, zgodnie z rozmiarem, który udostępnia jego kontenera do `Label`. `Label` Jest określany jako *nieograniczonego* i określa własną rozmiar.

(Uwaga: warunki *ograniczonego* i *nieograniczonego* może być counter-intuitive, ponieważ widok nieograniczonego jest zwykle mniejsze niż ograniczonego widoku. Ponadto te warunki nie są używane spójnie w rozdziałach wczesne książki.)

Widok, takich jak `Label` może być ograniczony w jednym wymiarze i nieograniczonego w innym. A `Label` tylko będzie Zawijaj tekst w wielu wierszach wtedy, gdy jest ograniczona w poziomie.

Jeśli `Label` jest ograniczony, jego może zajmować znacznie więcej miejsca niż wymagany dla tekstu. Tekst może być umieszczony w zakresie ogólnej `Label`. Ustaw [ `HorizontalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.HorizontalTextAlignment/) właściwości do elementu członkowskiego [ `TextAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextAlignment/) — wyliczenie ([`Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Start/), [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Center/), lub [ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Center/)) w celu sterowania wyrównaniem wszystkie wiersze akapitu. Wartość domyślna to `Start` i lewej Wyrównanie tekstu.

Ustaw [ `VerticalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.VerticalTextAlignment/) właściwości do elementu członkowskiego `TextAlignment` wyliczeniu, aby umieścić tekst u góry, Centrum lub u dołu obszaru zajmowanego przez `Label`.

Ustaw [ `LineBreakMode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.LineBreakMode/) właściwości do elementu członkowskiego [ `LineBreakMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LineBreakMode/) — wyliczenie ([`WordWrap`](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.WordWrap/), [ `CharacterWrap` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.CharacterWrap/), [ `NoWrap` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.NoWrap/), [ `HeadTruncation` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.HeadTruncation/), [ `MiddleTruncation` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.MiddleTruncation/), lub [ `TailTruncation` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.TailTruncation/)) do Formant jak wiele wierszy podziału akapitu lub są obcinane.

## <a name="text-and-background-colors"></a>Kolory tła i tekstu

Ustaw [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) i [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/) właściwości `Label` do [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) wartości sterujące kolor tła i tekstu.

`BackgroundColor` Ma zastosowanie do tła cały obszar zajmowany przez `Label`. W zależności od `HorizontalOptions` i `VerticalOptions` właściwości, że rozmiar może być znacznie większe niż obszar wymagany do wyświetlania tekstu. Kolor można użyć do eksperymentów z różnymi wartościami `HorizontalOptions`, `VerticalOptions`, `HorizontalExeAlignment`, i `VerticalTextAlignment` wyświetlić ich wpływ na rozmiar i położenie `Label`czy rozmiar i położenie tekstu w `Label`.

## <a name="the-color-structure"></a>Struktura koloru

[ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) Struktury pozwala określić kolory, jako wartości czerwony-zielony-niebieski (RGB) lub wartości Hue-nasycenie-jasność (HSL) lub nazwą koloru. Kanał alfa jest również dostępne w celu wskazania przezroczystość.

Użyj `Color` konstruktora, aby określić:

- [odcień szarości](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/)
- [wartości RGB](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/)
- [wartości RGB o przezroczystości](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/System.Double/)

Argumenty są `double` wartości z zakresu od 0 do 1.

Umożliwia także kilka metod statycznych do utworzenia `Color` wartości:

- [`Color.FromRgb`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgb/p/System.Double/System.Double/System.Double/) Aby uzyskać `double` wartości RGB z zakresu od 0 do 1
- [`Color.FromRgb`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgb/p/System.Int32/System.Int32/System.Int32/) dla wartości RGB liczbą całkowitą z zakresu od 0 do 255
- [`Color.FromRgba`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgba/p/System.Double/System.Double/System.Double/System.Double/) Aby uzyskać `double` wartości RGB z przezroczystości
- [`Color.FromRgba`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgba/p/System.Int32/System.Int32/System.Int32/System.Int32/) dla wartości RGB całkowitych z zachowaniem przezroczystości
- [`Color.FromHsla`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHsla/p/System.Double/System.Double/System.Double/System.Double/) Aby uzyskać `double` wartości HSL przezroczystości
- [`Color.FromUint`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromUint/p/System.UInt32/) Aby uzyskać `uint` wartość obliczany jako (B + 256 * (G + 256 * (R + 256 * A)))
- [`Color.FromHex`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHex/p/System.String/) Aby uzyskać `string` format cyfr szesnastkowych w formie "#AARRGGBB" lub "#RRGGBB" lub "#ARGB" lub "#RGB", gdzie każda litera odpowiada cyfrą szesnastkową dla alfa, czerwony, zielonemu i niebieskiemu kanałów. Ta metoda jest podstawowy używany do konwersji kolorów XAML, zgodnie z opisem w [rozdział 7, XAML, a kod](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md).

Raz utworzony, `Color` wartości nie można modyfikować. Właściwości kolor można uzyskać z następującymi właściwościami:

- [`R`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.R/)
- [`G`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.G/)
- [`B`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.B/)
- [`A`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.A/)
- [`Hue`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Hue/)
- [`Saturation`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Saturation/)
- [`Luminosity`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Luminosity/)

Są to wszystkie `double` wartości z zakresu od 0 do 1.

`Color` definiuje również 240 publicznego statycznego pola tylko do odczytu dla wspólnych kolorów. W momencie książce zostało zapisane tylko 17 wspólnych kolorów były dostępne.

Inny publiczny statyczne pole tylko do odczytu definiuje kolor ze wszystkich kanałów koloru ustawić na zero:

- [`Color.Transparent`](https://developer.xamarin.com/api/field/Xamarin.Forms.Color.Transparent/)

Kilka metod wystąpienia Zezwalaj na modyfikowanie istniejącego koloru, aby utworzyć nowy kolor:

- [`AddLuminosity`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.AddLuminosity/p/System.Double/)
- [`MultiplyAlpha`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.MultiplyAlpha/p/System.Double/)
- [`WithHue`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.WithHue/p/System.Double/)
- [`WithLuminosity`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.WithLuminosity/p/System.Double/)
- [`WithSaturation`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.WithSaturation/p/System.Double/)

Na koniec dwa statycznego właściwości tylko do odczytu zdefiniować wartości koloru specjalne:

- [`Color.Default`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/), wszystkie kanały ustawioną & #x 2013; 1
- [`Color.Accent`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Accent/)

`Color.Default` Służy do wymuszania schemat kolorów platformy, a w związku z tym ma inne znaczenie w różnych kontekstach na różnych platformach. Domyślnie platforma schematy kolorów są:

- iOS: ciemny tekst na jasnym
- System android: Jasny na ciemnym tle (w książce) i ciemny tekstu na jasnym (dotyczące projektowania materiałów za pośrednictwem AppCompat w **wzorca** gałęzi w repozytorium kodu przykładowej)
- Platformy uniwersalnej systemu Windows: Ciemny tekst jasnym
- Windows 8.1: Jasny tekst na tle ciemny
- Windows Phone 8.1: Jasny tekst na tle ciemny

`Color.Accent` Wartość powoduje specyficzne dla platformy (i czasami użytkownik może wybrać) kolor, który jest widoczny na tle ciemny lub jasny.

## <a name="changing-the-application-color-scheme"></a>Zmiana schematu kolorów aplikacji

Różnych platform ma domyślny schemat kolorów opisane w powyższej listy.

Gdy przeznaczonych dla systemu Android, istnieje możliwość przełączyć się do schematu ciemny na światło, określając motywu jasny, w pliku Android.Manifest.xml lub [Dodawanie AppCompat i projektowania materiałów](~/xamarin-forms/platform/android/appcompat.md).

Dla platform Windows motywu kolorów jest zwykle wybrane przez użytkownika, ale można dodać `RequestedTheme` atrybutu ustawiona jako `Light` lub `Dark` w pliku App.xaml platformy. Domyślnie znajduje się w pliku App.xaml w projekcie platformy UWP `RequestedTheme` ustawić atrybutu `Light`.

## <a name="font-sizes-and-attributes"></a>Rozmiary czcionek i atrybuty

Ustaw [ `FontFamily` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FontFamily/) właściwość `Label` na ciąg, takie jak "Razy łacińskich" Wybierz rodzinę czcionek. Należy określić rodzinę czcionek, która jest obsługiwana na konkretnej platformy i platformy są niespójne w tym zakresie.

Ustaw [ `FontSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FontSize/) właściwość `Label` do `double` służący do określania przybliżonej wysokość czcionki. Zobacz [rozdział 5, zajmujących się rozmiary](chapter05.md), aby uzyskać więcej informacji o wyborze inteligentnie rozmiary czcionek.

Alternatywnie można uzyskać jeden z kilku rozmiarów predefiniowanych czcionki zależny od platformy. Statycznych [ `Device.GetNamedSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.GetNamedSize/p/Xamarin.Forms.NamedSize/System.Type/) — metoda i [przeciążenia](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.GetNamedSize/p/Xamarin.Forms.NamedSize/Xamarin.Forms.Element/) zwrócą `double` wartość rozmiaru czcionki odpowiednie platformy oparte na elementach członkowskich [ `NamedSize` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NamedSize/)— wyliczenie ([`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Default/), [ `Micro` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Micro/), [ `Small` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Small/), [ `Medium` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Medium/),  i [ `Large` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Large/)). Wartość zwracana z `Medium` element członkowski nie jest zawsze taki sam jak `Default`. [ **NamedFontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) przykładowy tekst jest wyświetlany z tych o nazwie rozmiary.

Ustaw [ `FontAttributes` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FontAttributes/) właściwość `Label` do elementu członkowskiego [ `FontAttributes` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FontAttributes/) wyliczenia, [ `Bold` ](https://developer.xamarin.com/api/field/Xamarin.Forms.FontAttributes.Bold/), [ `Italic` ](https://developer.xamarin.com/api/field/Xamarin.Forms.FontAttributes.Italic/), lub [ `None` ](https://developer.xamarin.com/api/field/Xamarin.Forms.FontAttributes.None/). Możesz połączyć ze sobą `Bold` i `Italic` członków z C# bitowego operatora OR.

## <a name="formatted-text"></a>Tekst sformatowany

We wszystkich przykładów wykonanej do tej pory cały tekst wyświetlany przez `Label` jednolicie został sformatowany. Aby zróżnicować formatowania w ciągu tekstowym, nie należy ustawiać `Text` właściwość `Label`. Zamiast tego należy ustawić [ `FormattedText` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FormattedText/) dla właściwości typu obiektu [ `FormattedString` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FormattedString/).

`FormattedString` ma [ `Spans` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FormattedString.Spans/) właściwość, która jest kolekcją [ `Span` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Span/) obiektów. Każdy `Span` obiekt ma własną [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.Text/), [ `FontFamily` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.FontFamily/), [ `FontSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.FontSize/), [ `FontAttributes` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.FontAttributes/), [ `ForegroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.ForegroundColor/), i [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.BackgroundColor/) właściwości.

[ **VariableFormattedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormText) przykładzie pokazano, za pomocą `FormattedText` właściwość pojedynczy wiersz tekstu, a [ **VariableFormattedParagraph** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormPara) technika całego akapitu, pokazuje, jak pokazano poniżej:

[![Potrójna zrzut ekranu przedstawiający zmiennej sformatowany akapit](images/ch03fg06-small.png "tekstu etykiety w formacie zmiennej")](images/ch03fg06-large.png "zmiennej sformatowany tekst etykiety")

[ **NamedFontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) program używa pojedynczego `Label` i `FormattedString` obiektu, aby wyświetlić wszystkie rozmiary czcionki o nazwie dla każdej platformy.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 3 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch03-Apr2016.pdf)
- [Przykłady rozdział 3](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)
- [Przykłady rozdział 3 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/FS)
- [Etykieta](~/xamarin-forms/user-interface/text/label.md)
- [Praca z kolorem](~/xamarin-forms/user-interface/colors.md)
