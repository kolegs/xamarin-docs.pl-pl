---
title: Etykieta zestawu narzędzi Xamarin.Forms
description: W tym artykule wyjaśniono, jak korzystać z zestawu narzędzi Xamarin.Forms etykiety klasy do wyświetlania pojedynczych i wiele wierszy tekstu w aplikacji.
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/16/2018
ms.openlocfilehash: b5069381126db0859508480df5596ed5ec85686f
ms.sourcegitcommit: 4c0093ee5d4aeb16c0e6f0c740c4796736971651
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39203039"
---
# <a name="xamarinforms-label"></a>Etykieta zestawu narzędzi Xamarin.Forms

_Tekst wyświetlany w interfejsie Xamarin.Forms_

[ `Label` ](xref:Xamarin.Forms.Label) Widok służy do wyświetlania tekstu, zarówno w przypadku pojedynczych, jak i wiele wierszy. Etykiety można mieć, niestandardowych czcionek (rodziny, rozmiary i opcje) oraz kolorowego tekstu.

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>Obcięcie i pakowania

Etykiety można ustawić, aby obsłużyć tekst, który nie mieści się w jednym wierszu w jednym z kilku sposobów, udostępnianych przez `LineBreakMode` właściwości. [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode) to wyliczenie z następującymi wartościami:

- **HeadTruncation** &ndash; obcina Nagłówek tekstu, przedstawiający na końcu.
- **CharacterWrap** &ndash; zawija tekst na nowy wiersz na granicy znaków.
- **MiddleTruncation** &ndash; Wyświetla początku i końcu środkowej Zastąp wielokropkiem tekstem.
- **NoWrap** &ndash; nie zawija tekst, tylko wyświetlanie ilości tekstu może mieści się w jednym wierszu.
- **TailTruncation** &ndash; zawiera początek tekstu obcinanie na końcu.
- **WordWrap** &ndash; zawija tekst na granicy wyrazu.

## <a name="fonts"></a>Czcionki

Aby uzyskać więcej informacji o określaniu czcionki na `Label`, zobacz [czcionki](~/xamarin-forms/user-interface/text/fonts.md).

## <a name="colors"></a>Kolory

`Label`można ustawić s Użyj koloru niestandardowego tekstu przy użyciu możliwej do wiązania [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor) właściwości.

Szczególną jest niezbędne do zapewnienia, że kolory będzie można używać na każdej platformie. Ponieważ każdej z platform ma różne wartości domyślne kolory tekstu i tła, musisz Uważaj wybrać domyślny, który działa na każdym.

Aby ustawić kolor tekstu etykiety, użyj następujących XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TextSample.LabelPage"
             Title="Label Demo">
    <StackLayout Padding="5,10">
      <Label TextColor="#77d065" FontSize = "20" Text="This is a label." />
    </StackLayout>
</ContentPage>
```

Jest równoważny kod C#:

```csharp
public partial class LabelPage : ContentPage
{
    public LabelPage ()
    {
        InitializeComponent ();

        var layout = new StackLayout { Padding = new Thickness(5,10) };
        var label = new Label { Text="This is a label.", TextColor = Color.FromHex("#77d065"), FontSize = 20 };
        layout.Children.Add(label);
        this.Content = layout;
    }
}
```

Poniższych zrzutach ekranu przedstawiono wynik ustawienie `TextColor` właściwości:

![](label-images/textcolor.png "Przykład TextColor etykiety")

Aby uzyskać więcej informacji na temat kolorów, zobacz [kolory](~/xamarin-forms/user-interface/colors.md).

<a name="Formatted_Text" />

## <a name="formatted-text"></a>Tekst sformatowany

Udostępnianie etykiety [ `FormattedText` ](xref:Xamarin.Forms.Label.FormattedText) właściwość, która umożliwia przedstawienia tekstu z wiele czcionek i kolorów w jednym widoku.

`FormattedText` Właściwość jest typu [ `FormattedString` ](xref:Xamarin.Forms.FormattedString), która składa się z co najmniej jeden [ `Span` ](xref:Xamarin.Forms.Span) przypadkach ustawione za pośrednictwem [ `Spans` ](xref:Xamarin.Forms.FormattedString.Spans) właściwości . Każdy `Span` ma następujące właściwości:

- [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor) — kolor tła obszaru.
- [`Font`](xref:Xamarin.Forms.Span.Font) — czcionki dla tekstu w zakresie.
- [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes) — atrybutów czcionki dla tekstu w zakresie.
- [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily) — rodzinę czcionek, do której należy czcionki dla tekstu w zakresie.
- [`FontSize`](xref:Xamarin.Forms.Span.FontSize) — rozmiar czcionki dla tekstu w zakresie.
- [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor) — kolor tekstu w zakresie. Ta właściwość jest przestarzały i został zastąpiony przez `TextColor` właściwości.
- [`Style`](xref:Xamarin.Forms.Span.Style) — Styl zakresu.
- [`Text`](xref:Xamarin.Forms.Span.Text) — tekstu zakresu.
- [`TextColor`](xref:Xamarin.Forms.Span.TextColor) — kolor tekstu w zakresie.

Pokazano w poniższym przykładzie XAML `FormattedText` właściwość, która składa się z trzech `Span` wystąpień:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TextSample.LabelPage"
             Title="Label Demo - XAML">
    <StackLayout Padding="5,10">
        ...
        <Label>
            <Label.FormattedText>
                <FormattedString>
                    <Span Text="Red Bold, " TextColor="Red" FontAttributes="Bold" />
                    <Span Text="default, " Style="{DynamicResource BodyStyle}" />
                    <Span Text="italic small." FontAttributes="Italic" FontSize="Small" />
                </FormattedString>
            </Label.FormattedText>
        </Label>
    </StackLayout>
</ContentPage>
```

Jest równoważny kod C#:

```csharp
public class LabelPageCode : ContentPage
{
    public LabelPageCode ()
    {
        var layout = new StackLayout{ Padding = new Thickness (5, 10) };
        ...
        var formattedString = new FormattedString ();
        formattedString.Spans.Add (new Span{ Text = "Red bold, ", ForegroundColor = Color.Red, FontAttributes = FontAttributes.Bold });
        formattedString.Spans.Add (new Span { Text = "default, ", Style = Device.Styles.BodyStyle });
        formattedString.Spans.Add (new Span { Text = "italic small.", FontAttributes = FontAttributes.Italic, FontSize =  Device.GetNamedSize(NamedSize.Small, typeof(Label)) });

        layout.Children.Add (new Label { FormattedText = formattedString });
        this.Content = layout;
    }
}
```

> [!NOTE]
> [ `Text` ](xref:Xamarin.Forms.Span.Text) Właściwość `Span` można ustawić za pomocą powiązania danych. Aby uzyskać więcej informacji, zobacz [powiązanie danych](~/xamarin-forms/app-fundamentals/data-binding/index.md).

Poniższych zrzutach ekranu przedstawiono wynik ustawienie `FormattedString` właściwości do trzech `Span` wystąpień:

![](label-images/formattedtext.png "Przykład FormattedText etykiety")

## <a name="styling-a-label"></a>Ustawianie stylów etykietę

Przedstawione w poprzednich sekcjach omówione ustawienie [ `Label` ](xref:Xamarin.Forms.Label) właściwości na podstawie poszczególnych wystąpień. Jednak zestawy właściwości, można podzielić na jeden styl, który spójnie jest stosowana do jednej lub wielu widoków. Może to zwiększyć czytelność kodu i ułatwić implementacji zmian w projekcie. Aby uzyskać więcej informacji, zobacz [style](~/xamarin-forms/user-interface/text/styles.md).

## <a name="related-links"></a>Linki pokrewne

- [Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms rozdział 3](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [Tekst (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Etykieta interfejsu API](xref:Xamarin.Forms.Label)
