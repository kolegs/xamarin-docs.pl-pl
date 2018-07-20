---
title: Podsumowanie rozdział 3. Większe zagłębienie w tekst
description: 'Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms: Podsumowanie rozdział 3. Większe zagłębienie w tekst'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2E5581A6-4D3E-4BD5-9FDB-ACBA0F0FC734
author: charlespetzold
ms.author: chape
ms.date: 07/18/2018
ms.openlocfilehash: eabd001587034ac0bf1b86962fe63b016fe651e9
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156577"
---
# <a name="summary-of-chapter-3-deeper-into-text"></a>Podsumowanie rozdział 3. Większe zagłębienie w tekst

W tym rozdziale przedstawiono [ `Label` ](xref:Xamarin.Forms.Label) widoku bardziej szczegółowo, w tym kolor, czcionki i formatowanie.

## <a name="wrapping-paragraphs"></a>Zawijanie akapitów

Gdy [ `Text` ](xref:Xamarin.Forms.Label.Text) właściwość `Label` zawiera długie teksty `Label` automatycznie otacza go w wielu wierszach zgodnie [ **Baskervilles** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/Baskervilles) próbki. Możesz osadzić kodów Unicode, takie jak "\u2014"-Pauza lub C# znaków, takich jak '\r' na przerwanie i przejście do nowego wiersza.

Gdy [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) i [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) właściwości `Label` są ustawione na `LayoutOptions.Fill`, całkowity rozmiar `Label` podlega miejsce, jego kontener udostępnia. `Label` Jest nazywany *ograniczonego*. Rozmiar `Label` jest większy niż jego kontenera.

Gdy `HorizontalOptions` i `VerticalOptions` właściwości są ustawiane na wartości innych niż `LayoutOptions.Fill`, rozmiar `Label` podlega miejsca wymaganego do renderowania tekstu, rozmiarze, który udostępnia jego kontenera do `Label`. `Label` Jest nazywany *nieograniczonego* i określa swój własny rozmiar.

(Uwaga: warunki *ograniczonego* i *nieograniczonego* może być counter-intuitive, ponieważ widok nieograniczonego jest zwykle mniejsze niż ograniczonego widoku. Ponadto te warunki nie są używane spójnie w wczesnych rozdziały książki.)

Widok, takie jak `Label` może być ograniczona w jednym wymiarze i nieograniczonego w innym. A `Label` będzie tylko zawinąć tekst w wielu wierszach jest ograniczona w poziomie.

Jeśli `Label` jest ograniczona, jego może zajmować znacznie większej ilości miejsca, niż jest to wymagane dla tekstu. Tekst może być umieszczony w obszarze Ogólne `Label`. Ustaw [ `HorizontalTextAlignment` ](xref:Xamarin.Forms.Label.HorizontalTextAlignment) właściwości do elementu członkowskiego [ `TextAlignment` ](xref:Xamarin.Forms.TextAlignment) wyliczenia ([`Start`](xref:Xamarin.Forms.TextAlignment.Start), [ `Center` ](xref:Xamarin.Forms.TextAlignment.Center), lub [ `End` ](xref:Xamarin.Forms.TextAlignment.Center)) do sterowania wyrównaniem wszystkich wierszy akapitu. Wartość domyślna to `Start` i lewy wyrównuje tekst.

Ustaw [ `VerticalTextAlignment` ](xref:Xamarin.Forms.Label.VerticalTextAlignment) właściwości do elementu członkowskiego `TextAlignment` wyliczeniu, aby umieścić tekst u góry, center lub u dołu obszar zajmowany przez `Label`.

Ustaw [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode) właściwości do elementu członkowskiego [ `LineBreakMode` ](xref:Xamarin.Forms.LineBreakMode) wyliczenia ([`WordWrap`](xref:Xamarin.Forms.LineBreakMode.WordWrap), [ `CharacterWrap` ](xref:Xamarin.Forms.LineBreakMode.CharacterWrap), [ `NoWrap` ](xref:Xamarin.Forms.LineBreakMode.NoWrap), [ `HeadTruncation` ](xref:Xamarin.Forms.LineBreakMode.HeadTruncation), [ `MiddleTruncation` ](xref:Xamarin.Forms.LineBreakMode.MiddleTruncation), lub [ `TailTruncation` ](xref:Xamarin.Forms.LineBreakMode.TailTruncation)) do Kontrolka, jak wiele wierszy w podziału akapitu lub są obcinane.

## <a name="text-and-background-colors"></a>Kolory tekstu i tła

Ustaw [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor) i [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor) właściwości `Label` do [ `Color` ](xref:Xamarin.Forms.Color) wartości do kontrolowania kolor tekstu i tła.

`BackgroundColor` Ma zastosowanie do tła cały obszar zajmowany przez `Label`. W zależności od `HorizontalOptions` i `VerticalOptions` właściwości, że rozmiar może być znacznie większe niż obszar wymagany do wyświetlania tekstu. Kolorów można użyć do eksperymentowania z różnymi wartościami `HorizontalOptions`, `VerticalOptions`, `HorizontalExeAlignment`, i `VerticalTextAlignment` aby zobaczyć, jak wpływają na rozmiar i położenie `Label`czy rozmiar i położenie tekstu w `Label`.

## <a name="the-color-structure"></a>Struktura koloru

[ `Color` ](xref:Xamarin.Forms.Color) Struktury pozwala określać kolory, jako wartości czerwony, zielony, niebieski (RGB) lub wartości Hue-nasycenie-jasność (HSL) lub nazwę koloru. Kanał alfa jest również dostępne w celu wskazania przezroczystości.

Użyj `Color` konstruktora, aby określić:

- [odcień szarości](xref:Xamarin.Forms.Color.%23ctor(System.Double))
- [wartości RGB](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double))
- [wartości RGB z przezroczystością](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double,System.Double))

Argumenty są `double` wartości z zakresu od 0 do 1.

Umożliwia także kilka metod statycznych do utworzenia `Color` wartości:

- [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Double,System.Double,System.Double)) Aby uzyskać `double` wartości RGB z zakresu od 0 do 1
- [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Int32,System.Int32,System.Int32)) Aby uzyskać wartości RGB liczbą całkowitą z zakresu od 0 do 255
- [`Color.FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Double,System.Double,System.Double,System.Double)) Aby uzyskać `double` wartości RGB z przezroczystością
- [`Color.FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Int32,System.Int32,System.Int32,System.Int32)) Aby uzyskać wartości RGB liczby całkowitej z przezroczystością
- [`Color.FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double)) Aby uzyskać `double` wartości HSL z przezroczystością
- [`Color.FromUint`](xref:Xamarin.Forms.Color.FromUint(System.UInt32)) Aby uzyskać `uint` wartość obliczona jako (B + 256 * (G + 256 * (R + 256 * A)))
- [`Color.FromHex`](xref:Xamarin.Forms.Color.FromHex(System.String)) Aby uzyskać `string` format liczb szesnastkowych w formie "#AARRGGBB" lub "#RRGGBB" lub "#ARGB" lub "#RGB", gdzie każdej litery odpowiada cyfrą szesnastkową alfa, czerwony, zielony i niebieski kanałów. Ta metoda jest kluczem podstawowym używana podczas konwersji kolor XAML, zgodnie z opisem w [rozdział 7, XAML a kod](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md).

Po utworzeniu `Color` wartość jest niezmienny. Właściwości koloru można uzyskać z następującymi właściwościami:

- [`R`](xref:Xamarin.Forms.Color.R)
- [`G`](xref:Xamarin.Forms.Color.G)
- [`B`](xref:Xamarin.Forms.Color.B)
- [`A`](xref:Xamarin.Forms.Color.A)
- [`Hue`](xref:Xamarin.Forms.Color.Hue)
- [`Saturation`](xref:Xamarin.Forms.Color.Saturation)
- [`Luminosity`](xref:Xamarin.Forms.Color.Luminosity)

Są to wszystkie `double` wartości z zakresu od 0 do 1.

`Color` definiuje również 240 publicznych statycznego pola tylko do odczytu dla pospolitych kolorów. W czasie, który został napisany książki tylko 17 pospolitych kolorów były dostępne.

Inny publiczne statyczne pole tylko do odczytu definiuje kolor ze wszystkich kanałów koloru, należy ustawić na zero:

- [`Color.Transparent`](xref:Xamarin.Forms.Color.Transparent)

Kilka metod wystąpienia Zezwalaj na modyfikowanie istniejącego koloru, aby utworzyć nowy kolor:

- [`AddLuminosity`](xref:Xamarin.Forms.Color.AddLuminosity(System.Double))
- [`MultiplyAlpha`](xref:Xamarin.Forms.Color.MultiplyAlpha(System.Double))
- [`WithHue`](xref:Xamarin.Forms.Color.WithHue(System.Double))
- [`WithLuminosity`](xref:Xamarin.Forms.Color.WithLuminosity(System.Double))
- [`WithSaturation`](xref:Xamarin.Forms.Color.WithSaturation(System.Double))

Na koniec dwie statycznych właściwości tylko do odczytu zdefiniować wartość koloru specjalne:

- [`Color.Default`](xref:Xamarin.Forms.Color.Default), ustaw wszystkie kanały &ndash;1
- [`Color.Accent`](xref:Xamarin.Forms.Color.Accent)

`Color.Default` Służy do wymuszania schemat kolorów platformy i w związku z tym ma inne znaczenie w różnych kontekstach na różnych platformach. Domyślnie platforma schematy kolorów są:

- dla systemu iOS: ciemny tekst na tle światła
- Android: Jasny na ciemnym tle (w książce) ani ciemny tekstu na jasnym (dla Material Design za pośrednictwem AppCompat w **wzorca** gałęzi przykładowego repozytorium kodu)
- Platformy uniwersalnej systemu Windows: Ciemny tekstu jasnym

`Color.Accent` Wartość wyniki w określonych platform (i czasami wybierane przez użytkownika) kolor, który jest widoczny na tle ciemny lub światła.

## <a name="changing-the-application-color-scheme"></a>Zmiana schematu kolorów aplikacji

Różne platformy mają domyślny schemat kolorów, jak pokazano na powyższej liście.

Gdy przeznaczonych dla systemu Android, istnieje możliwość, określając motyw jasny, w pliku Android.Manifest.xml lub przełączyć się do schematu dark-light [Dodawanie AppCompat i Material Design](~/xamarin-forms/platform/android/appcompat.md).

Dla platform Windows, motyw kolorów jest zwykle wybrane przez użytkownika, ale można dodać `RequestedTheme` atrybut ustawiony na `Light` lub `Dark` w pliku App.xaml platformy. Domyślnie plik App.xaml w projekcie platformy uniwersalnej systemu Windows zawiera `RequestedTheme` ustawioną wartość atrybutu `Light`.

## <a name="font-sizes-and-attributes"></a>Rozmiary czcionek i atrybuty

Ustaw [ `FontFamily` ](xref:Xamarin.Forms.Label.FontFamily) właściwość `Label` na ciąg, taki jak "Razy Roman" do Wybierz rodzinę czcionek. Jednak należy określić rodziny czcionek, która jest obsługiwana na konkretnej platformie i w związku z tym platformy są niespójne.

Ustaw [ `FontSize` ](xref:Xamarin.Forms.Label.FontSize) właściwość `Label` do `double` do określenia wysokości około czcionki. Zobacz [rozdział 5, Obsługa rozmiarów](chapter05.md), aby uzyskać więcej informacji na temat wybierania inteligentnie rozmiary czcionek.

Alternatywnie można uzyskać jeden z kilku rozmiarów wstępnie czcionki zależny od platformy. Statyczne [ `Device.GetNamedSize` ](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,System.Type)) metody i [przeciążenia](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,Xamarin.Forms.Element)) oba zwracają `double` rozmiar czcionki wartość odpowiednią do korzystania z platformy oparte na elementach członkowskich [ `NamedSize` ](xref:Xamarin.Forms.NamedSize)wyliczenia ([`Default`](xref:Xamarin.Forms.NamedSize.Default), [ `Micro` ](xref:Xamarin.Forms.NamedSize.Micro), [ `Small` ](xref:Xamarin.Forms.NamedSize.Small), [ `Medium` ](xref:Xamarin.Forms.NamedSize.Medium),  i [ `Large` ](xref:Xamarin.Forms.NamedSize.Large)). Wartość zwracana z `Medium` elementu członkowskiego niekoniecznie jest taka sama jak `Default`. [ **NamedFontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) przykład wyświetla tekst z nimi o nazwie rozmiarów.

Ustaw [ `FontAttributes` ](xref:Xamarin.Forms.Label.FontAttributes) właściwość `Label` do elementu członkowskiego [ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes) wyliczenia, [ `Bold` ](xref:Xamarin.Forms.FontAttributes.Bold), [ `Italic` ](xref:Xamarin.Forms.FontAttributes.Italic), lub [ `None` ](xref:Xamarin.Forms.FontAttributes.None). Można połączyć `Bold` i `Italic` elementów członkowskich przy użyciu języka C# bitowy operator OR.

## <a name="formatted-text"></a>Tekst sformatowany

We wszystkich przykładach w do tej pory cały tekst wyświetlany przez `Label` został sformatowany w jednolity sposób. Będzie się różnić, formatowanie w ciągu tekstowym, nie należy ustawiać `Text` właściwość `Label`. Zamiast tego należy ustawić [ `FormattedText` ](xref:Xamarin.Forms.Label.FormattedText) właściwość do obiektu typu [ `FormattedString` ](xref:Xamarin.Forms.FormattedString).

`FormattedString` ma [ `Spans` ](xref:Xamarin.Forms.FormattedString.Spans) właściwość, która jest kolekcją [ `Span` ](xref:Xamarin.Forms.Span) obiektów. Każdy `Span` obiekt ma swój własny [ `Text` ](xref:Xamarin.Forms.Span.Text), [ `FontFamily` ](xref:Xamarin.Forms.Span.FontFamily), [ `FontSize` ](xref:Xamarin.Forms.Span.FontSize), [ `FontAttributes` ](xref:Xamarin.Forms.Span.FontAttributes), [ `ForegroundColor` ](xref:Xamarin.Forms.Span.ForegroundColor), i [ `BackgroundColor` ](xref:Xamarin.Forms.Span.BackgroundColor) właściwości.

[ **VariableFormattedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormText) przykładzie pokazano użycie `FormattedText` właściwość pojedynczy wiersz tekstu, a [ **VariableFormattedParagraph** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormPara) techniki dla całego akapitu, pokazuje, jak pokazano poniżej:

[![Potrójna zrzut ekranu przedstawiający zmiennej sformatowane akapitu](images/ch03fg06-small.png "tekstu etykiety w formacie zmiennej")](images/ch03fg06-large.png#lightbox "tekstu etykiety w formacie zmiennej")

[ **NamedFontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) program używa pojedynczego `Label` i `FormattedString` obiektu, aby wyświetlić wszystkie rozmiary czcionek nazwane dla każdej platformy.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 3 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch03-Apr2016.pdf)
- [Przykłady rozdział 3](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)
- [Przykłady rozdział 3 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/FS)
- [Etykieta](~/xamarin-forms/user-interface/text/label.md)
- [Praca z kolorami](~/xamarin-forms/user-interface/colors.md)
