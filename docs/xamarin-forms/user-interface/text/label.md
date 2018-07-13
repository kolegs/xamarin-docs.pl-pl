---
title: Etykieta zestawu narzędzi Xamarin.Forms
description: W tym artykule wyjaśniono, jak korzystać z zestawu narzędzi Xamarin.Forms etykiety klasy do wyświetlania pojedynczych i wiele wierszy tekstu w aplikacji.
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: ce602a84ea1024dc22298a3ec1567a9a34ad4a82
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995969"
---
# <a name="xamarinforms-label"></a>Etykieta zestawu narzędzi Xamarin.Forms

_Tekst wyświetlany w interfejsie Xamarin.Forms_

`Label` Widok służy do wyświetlania tekstu, zarówno w przypadku pojedynczych, jak i wiele wierszy. Etykiety można mieć, niestandardowych czcionek (rodziny, rozmiary i opcje) oraz kolorowego tekstu. W tym artykule omówiono następujące tematy:

- **[Obcięcie i zawijania](#Truncation_and_Wrapping)**  &ndash; obcięcie i opcje zawijania dla obsługi sytuacje, gdy tekst nie mieści się w jednym wierszu.
- **[Czcionka](#Font)**  &ndash; opcje czcionki.
- **[Kolor](#Color)**  &ndash; etykiety tekstowe opcje kolorów.
- **[Sformatowany tekst](#Formatted_Text)**  &ndash; opcje wyświetlania tekstu z kilku wbudowanych formatów/style.

## <a name="styling-label"></a>Etykieta stylu

W poniższych częściach omówiono ustawienia właściwości `Label` ręcznie na podstawie poszczególnych wystąpień. Należy zauważyć, że zestawy właściwości, można podzielić na jeden styl, który spójnie jest stosowana do jednej lub wielu widoków. Może to zwiększyć czytelność kodu i ułatwić implementacji zmian w projekcie. Zobacz [style](~/xamarin-forms/user-interface/text/styles.md) Aby uzyskać więcej informacji.

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>Obcięcie i pakowania

Etykiety można ustawić, aby obsłużyć tekst, który nie mieści się w jednym wierszu w jednym z kilku sposobów, udostępnianych przez `LineBreakMode` właściwości. [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode) to wyliczenie z następujących opcji:

- **HeadTruncation** &ndash; obcina Nagłówek tekstu, przedstawiający na końcu.
- **CharacterWrap** &ndash; zawija tekst na nowy wiersz na granicy znaków.
- **MiddleTruncation** &ndash; Wyświetla początku i końcu środkowej Zastąp wielokropkiem tekstem.
- **NoWrap** &ndash; nie zawija tekst, tylko wyświetlanie ilości tekstu może mieści się w jednym wierszu.
- **TailTruncation** &ndash; zawiera początek tekstu obcinanie na końcu.
- **WordWrap** &ndash; zawija tekst na granicy wyrazu.

## <a name="font"></a>Czcionka

Zobacz [Praca z czcionkami](~/xamarin-forms/user-interface/text/fonts.md) Aby uzyskać więcej informacji.

## <a name="color"></a>Kolor

`Label`można ustawić s Użyj koloru niestandardowego tekstu przy użyciu możliwej do wiązania `TextColor` właściwości.

Szczególną jest niezbędne do zapewnienia, że kolory będzie można używać na każdej platformie. Ponieważ każdej z platform ma różne wartości domyślne kolory tekstu i tła, musisz Uważaj wybrać domyślny, który działa na każdym.

Użyj poniższego kodu, aby ustawić kolor tekstu etykiety:

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

W XAML:

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

Udostępnianie etykiety `FormattedText` właściwość, której można prezentować tekstu przy użyciu wiele czcionek i kolorów w jednym widoku.

`FormattedText` Właściwość jest typu [ `FormattedString` ](xref:Xamarin.Forms.FormattedString). Sformatowane ciągi składają się z co najmniej jeden `Span`s, każde z następującymi właściwościami:

- **BackgroundColor** &ndash; można ustawić kolor tła, na przykład osiągnąć efekt wyróżnienia.
- **FontAttributes** &ndash; może być równa pogrubienie, kursywa lub żadnego z tych celów.
- **FontFamily** &ndash; Ustawia czcionkę, która ma być używany.
- **FontSize** &ndash; ustawia rozmiar tekstu.
- **ForegroundColor** &ndash; Ustawia kolor tekstu.
- **Tekst** &ndash; tekst, który ma być wyświetlony.

Poniższy kod C# ilustruje etykietę, gdzie pierwszy wyraz jest pogrubiony a ostatni wyraz ma kolor czerwony:

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

W XAML:

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

- [Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms rozdział 3](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [Tekst (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Etykieta interfejsu API](xref:Xamarin.Forms.Label)
