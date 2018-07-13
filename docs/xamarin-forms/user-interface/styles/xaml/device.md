---
title: Style urządzenia w interfejsie Xamarin.Forms
description: Zestaw narzędzi Xamarin.Forms obejmuje sześć style dynamiczne, znane jako style urządzenia, w klasie Device.Styles. W tym artykule wyjaśniono, jak używać style urządzenia w aplikacji platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 7FF19ED1-0822-4238-9435-AD970317A2F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: bba42c966c6a606790655751db8b294d9ca7b6f9
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994380"
---
# <a name="device-styles-in-xamarinforms"></a>Style urządzenia w interfejsie Xamarin.Forms

_Zestaw narzędzi Xamarin.Forms obejmuje sześć style dynamiczne, znane jako style urządzenia, w klasie Device.Styles._

*Urządzenia* style są:

- [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle)
- [`CaptionStyle`](xref:Xamarin.Forms.Device.Styles.CaptionStyle)
- [`ListItemDetailTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyle)
- [`ListItemTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyle)
- [`SubtitleStyle`](xref:Xamarin.Forms.Device.Styles.SubtitleStyle)
- [`TitleStyle`](xref:Xamarin.Forms.Device.Styles.TitleStyle)

Wszystkie style sześć może być stosowany tylko do [ `Label` ](xref:Xamarin.Forms.Label) wystąpień. Na przykład `Label` Wyświetla treści akapitu może ustawić jej [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) właściwości [ `BodyStyle` ](xref:Xamarin.Forms.Device.Styles.BodyStyle).

Poniższy przykład kodu demonstruje sposób użycia *urządzenia* style na stronie XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DeviceStylesPage" Title="Device" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="myBodyStyle" TargetType="Label"
              BaseResourceKey="BodyStyle">
                <Setter Property="TextColor" Value="Accent" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="Title style"
              Style="{DynamicResource TitleStyle}" />
            <Label Text="Subtitle text style"
              Style="{DynamicResource SubtitleStyle}" />
            <Label Text="Body style"
              Style="{DynamicResource BodyStyle}" />
            <Label Text="Caption style"
              Style="{DynamicResource CaptionStyle}" />
            <Label Text="List item detail text style"
              Style="{DynamicResource ListItemDetailTextStyle}" />
            <Label Text="List item text style"
              Style="{DynamicResource ListItemTextStyle}" />
            <Label Text="No style" />
            <Label Text="My body style"
              Style="{StaticResource myBodyStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Style urządzenia są powiązane z przy użyciu `DynamicResource` — rozszerzenie znaczników. Dynamiczny charakter style są widoczne w systemie iOS, zmieniając **ułatwień dostępu** ustawienia rozmiar tekstu. Wygląd *urządzenia* style różni się na każdej platformie, jak pokazano na poniższych zrzutach ekranu:

![](device-images/device-styles.png "Style urządzenia na każdej platformie")

*Urządzenie* style mogą również pochodzić z ustawiając [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey) właściwość na nazwę klucza dla stylu urządzenia. W powyższym przykładzie kodu `myBodyStyle` dziedziczy [ `BodyStyle` ](xref:Xamarin.Forms.Device.Styles.BodyStyle) i ustawia kolor tekstu akcentowane. Aby uzyskać więcej informacji na temat dziedziczenie stylów dynamiczne Zobacz [dynamiczne dziedziczenie stylów](~/xamarin-forms/user-interface/styles/xaml/dynamic.md#dynamic-style-inheritance).

Poniższy przykład kodu demonstruje odpowiednich stron w języku C#:

```csharp
public class DeviceStylesPageCS : ContentPage
{
    public DeviceStylesPageCS ()
    {
        var myBodyStyle = new Style (typeof(Label)) {
            BaseResourceKey = Device.Styles.BodyStyleKey,
            Setters = {
                new Setter {
                    Property = Label.TextColorProperty,
                    Value = Color.Accent
                }
            }
        };

        Title = "Device";
        Icon = "csharp.png";
        Padding = new Thickness (0, 20, 0, 0);

        Content = new StackLayout {
            Children = {
                new Label { Text = "Title style", Style = Device.Styles.TitleStyle },
                new Label { Text = "Subtitle style", Style = Device.Styles.SubtitleStyle },
                new Label { Text = "Body style", Style = Device.Styles.BodyStyle },
                new Label { Text = "Caption style", Style = Device.Styles.CaptionStyle },
                new Label { Text = "List item detail text style",
                  Style = Device.Styles.ListItemDetailTextStyle },
                new Label { Text = "List item text style", Style = Device.Styles.ListItemTextStyle },
                new Label { Text = "No style" },
                new Label { Text = "My body style", Style = myBodyStyle }
            }
        };
    }
}
```

[ `Style` ](xref:Xamarin.Forms.VisualElement.Style) Właściwości każdego [ `Label` ](xref:Xamarin.Forms.Label) wystąpienia jest równa odpowiedniej właściwości ze [ `Devices.Styles` ](xref:Xamarin.Forms.Device.Styles) klasy.

## <a name="accessibility"></a>Ułatwienia dostępu

*Urządzenia* style przestrzegają preferencje ułatwień dostępu, więc rozmiary czcionek zmienią preferencje ułatwień dostępu zostaną zmienione na każdej platformie. W związku z tym, aby zapewnić obsługę tekstu ułatwiającego dostęp, upewnij się, że *urządzenia* style są używane jako podstawa dowolnego style tekstu w aplikacji.

Poniższe zrzuty ekranu pokazują style urządzenia na każdej platformie, z najmniejszy rozmiar czcionki dostępne:

[![](device-images/minimum-size.png "Style dostępne niewielkich urządzeń na każdej platformie")](device-images/minimum-size-large.png#lightbox "style dostępne niewielkich urządzeń na każdej platformie")

Poniższe zrzuty ekranu pokazują style urządzenia na każdej platformie, z największy rozmiar czcionki dostępne:

![](device-images/maximum-size.png "Style dostępnego urządzenia dużych na każdej platformie")

## <a name="summary"></a>Podsumowanie

Xamarin.Forms obejmuje sześć *dynamiczne* stylów, znane jako *urządzenia* style w [ `Devices.Styles` ](xref:Xamarin.Forms.Device.Styles) klasy. Wszystkie style sześć może być stosowany tylko do [ `Label` ](xref:Xamarin.Forms.Label) wystąpień.


## <a name="related-links"></a>Linki pokrewne

- [Style tekstu](~/xamarin-forms/user-interface/text/styles.md)
- [Rozszerzenia struktury znaczników XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Style dynamiczne (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/DynamicStyles/)
- [Praca ze stylami (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [Device.Styles](xref:Xamarin.Forms.Device.Styles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Styl](xref:Xamarin.Forms.Style)
- [Metody ustawiającej](xref:Xamarin.Forms.Setter)
