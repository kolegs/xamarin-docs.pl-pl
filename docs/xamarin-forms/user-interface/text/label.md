---
title: Etykieta
description: Tekst wyświetlany w platformy Xamarin.Forms
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: c1056626b336dd9b6ce265ab693ceed2a24eae0f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="label"></a>Etykieta

_Tekst wyświetlany w platformy Xamarin.Forms_

`Label` Widoku jest używana do wyświetlania tekstu, pojedyncze i wiele wierszy. Etykiety mogą mieć niestandardowych czcionek (rodziny, rozmiarów i opcje) i kolorowy tekst. W tym artykule omówiono następujące tematy:

- **[Obcięcie i zawijania](#Truncation_and_Wrapping)**  &ndash; obcięcie i opcje zawijania dla obsługi sytuacji, gdy tekst nie mieści się w jednym wierszu.
- **[Czcionki](#Font)**  &ndash; opcje czcionki.
- **[Kolor](#Color)**  &ndash; opcji Kolor tekstu etykiet.
- **[Tekst sformatowany](#Formatted_Text)**  &ndash; opcje wyświetlania tekstu w wielu formatach/style wbudowane.

## <a name="styling-label"></a>Etykieta stylów

W poniższych częściach omówiono ustawienie właściwości `Label` ręcznie dla poszczególnych wystąpień. Należy pamiętać, że ustawia właściwości można grupować w jednym stylu spójnie stosowany do jednego lub wielu widoków. Może to zwiększyć czytelność kodu i ułatwić wdrożenie zmiany w projekcie. Zobacz [style](~/xamarin-forms/user-interface/text/styles.md) Aby uzyskać więcej informacji.

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>Obcięcie i pakowania

Można ustawić etykiety do obsługi tekst, który nie mieści się w jednym wierszu kilka sposobów udostępnianych przez `LineBreakMode` właściwości. [`LineBreakMode`](https://developer.xamarin.com/api/type/Xamarin.Forms.LineBreakMode/) to wyliczenie z następujących opcji:

- **HeadTruncation** &ndash; obcina head tekstu, przedstawiający na końcu.
- **CharacterWrap** &ndash; zawija tekst na nowy wiersz w granicy znaków.
- **MiddleTruncation** &ndash; Wyświetla początek i koniec tekstu, w środkowej Zamień wielokropkiem.
- **NoWrap** &ndash; nie zawija tekst wyświetlanie tylko tyle tekstu, co może mieści się w jednym wierszu.
- **TailTruncation** &ndash; pokazuje początek tekstu, obcinanie na końcu.
- **WordWrap** &ndash; zawija tekst na granicy programu word.

## <a name="font"></a>Czcionki

Zobacz [Praca z czcionkami](~/xamarin-forms/user-interface/text/fonts.md) Aby uzyskać więcej informacji.

## <a name="color"></a>Kolor

`Label`Aby użyć koloru niestandardowego tekstu za pośrednictwem powiązania można ustawić s `TextColor` właściwości.

Szczególną jest niezbędne do zapewnienia, że kolorów będzie można używać na każdej z platform. Ponieważ różne ustawienia domyślne kolory tła i tekstu dotyczy wszystkich platform, należy należy zachować ostrożność wybrać domyślny, który działa na każdym.

Aby ustawić kolor tekstu etykiety, należy użyć poniższego kodu:

W kodzie:

```csharp
public partial class LabelPage : ContentPage
{
    public LabelPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        var label = new Label { Text="This is a label.", TextColor = Color.FromHex("#77d065"), FontSize = 20 };
        layout.Children.Add(label);
    }
}
```

W języku XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.LabelPage"
Title="Label Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
      <Label TextColor="#77d065" FontSize = "20" Text="This is a label." />
    </StackLayout>
  </ContentPage.Content>
</ContentPage>
```

![](label-images/textcolor.png "Przykład TextColor etykiety")

<a name="Formatted_Text" />

## <a name="formatted-text"></a>Tekst sformatowany

Udostępnianie etykiety `FormattedText` właściwość, której można prezentować tekstu z wielu czcionek i kolorów w jednym widoku.

`FormattedText` Właściwość jest typu [ `FormattedString` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FormattedString/). Ciągi sformatowane składają się z co najmniej jeden `Span`s, każde z następującymi właściwościami:

- **BackgroundColor** &ndash; pozwala ustawić kolor tła, na przykład w celu uzyskania efektu wyróżnienia.
- **FontAttributes** &ndash; może być ustawioną pogrubienie, kursywa lub nie.
- **FontFamily** &ndash; ustawia czcionki do użycia.
- **FontSize** &ndash; ustawia rozmiar tekstu.
- **ForegroundColor** &ndash; Ustawia kolor tekstu.
- **Tekst** &ndash; tekst, który ma zostać wyświetlone.

Poniższy kod C# przedstawia etykietę, gdzie pierwsze słowo jest pogrubiona, a wyraz ostatniej czerwony:

```csharp
public partial class LabelPage : ContentPage
{
    public LabelPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
    var label = new Label { FontSize = 20 };
    var s = new FormattedString ();
    s.Spans.Add (new Span{ Text = "Red Bold", FontAttributes = FontAttributes.Bold });
    s.Spans.Add (new Span{ Text = "Default" });
    s.Spans.Add (new Span{ Text = "italic small", FontSize =  Device.GetNamedSize(NamedSize.Small, typeof(Label)), FontAttributes = FontAttributes.Italic});
    label.FormattedText = s;
        layout.Children.Add(label);
    }
}
```

W języku XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.LabelPage"
Title="Label Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
      <Label FontSize=20>
        <Label.FormattedText>
          <FormattedString>
            <Span Text="Red Bold" ForegroundColor="Red" FontAttributes="Bold" />
            <Span Text="Default" />
            <Span Text="italic small" FontAttributes="Italic" FontSize="Small" />
          </FormattedString>
        </Label.FormattedText>
      </Label>
    </StackLayout>
  </ContentPage.Content>
</ContentPage>
```

![](label-images/formattedtext.png "Przykład FormattedText etykiety")


## <a name="related-links"></a>Linki pokrewne

- [Tworzenie aplikacji mobilnych za pomocą platformy Xamarin.Forms, rozdział 3](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [Tekst (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Etykieta interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)
