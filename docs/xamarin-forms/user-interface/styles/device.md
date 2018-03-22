---
title: "Style urządzenia"
description: "Platformy Xamarin.Forms obejmuje sześć style dynamicznych, znany jako style urządzenia, w klasie Device.Styles."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7FF19ED1-0822-4238-9435-AD970317A2F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 7e9d8768969affdbaf1172c59eaa5fc1e64460e8
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="device-styles"></a>Style urządzenia

_Platformy Xamarin.Forms obejmuje sześć style dynamicznych, znany jako style urządzenia, w klasie Device.Styles._

*Urządzenia* style są:

- [`BodyStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyle/)
- [`CaptionStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.CaptionStyle/)
- [`ListItemDetailTextStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemDetailTextStyle/)
- [`ListItemTextStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemTextStyle/)
- [`SubtitleStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.SubtitleStyle/)
- [`TitleStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.TitleStyle/)

Wszystkie sześć style może być stosowany tylko do [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) wystąpień. Na przykład `Label` Wyświetla treści akapitu może ustawić jej [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) właściwości [ `BodyStyle` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyle/).

Poniższy przykład kodu pokazuje, za pomocą *urządzenia* style w strony XAML:

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

Style urządzenia są powiązane z przy użyciu `DynamicResource` — rozszerzenie znaczników. Dynamiczne rodzaj style są widoczne w systemie iOS, zmieniając **ułatwień dostępu** ustawienia rozmiar tekstu. Wygląd *urządzenia* style różni się na każdej platformie, jak pokazano na poniższych zrzutach ekranu:

![](device-images/device-styles.png "Style urządzenia dla każdej platformy")

*Urządzenie* style mogą również pochodzić z przez ustawienie [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) właściwość na nazwę klucza styl urządzenia. W powyższym przykładzie kodu `myBodyStyle` dziedziczy [ `BodyStyle` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyle/) i ustawia kolor tekstu akcentowane. Aby uzyskać więcej informacji o stylu dynamiczne dziedziczenia, zobacz [dynamiczne dziedziczenia styl](~/xamarin-forms/user-interface/styles/dynamic.md#dynamic-style-inheritance).

Poniższy przykład kodu pokazuje odpowiedniej strony w języku C#:

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

[ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) Właściwości każdego [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) wystąpienie ma ustawioną wartość odpowiednią właściwość z [ `Devices.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) klasy.

## <a name="accessibility"></a>Ułatwienia dostępu

*Urządzenia* style przestrzegać preferencje dostępności, więc rozmiary czcionek zmienia preferencje dostępności zostały zmienione na każdej z platform. W związku z tym, aby umożliwić obsługę dostępnego tekstu, upewnij się, że *urządzenia* style są używane jako podstawa dla dowolnego style tekstu w aplikacji.

Poniższe zrzuty ekranu pokazują style urządzenia na każdej platformie, z najmniejszy rozmiar czcionki dostępne:

[![](device-images/minimum-size.png "Style dostępnego urządzenia małych na każdej platformie")](device-images/minimum-size-large.png#lightbox "style dostępnego urządzenia małych na każdej platformie")

Poniższe zrzuty ekranu pokazują style urządzenia na każdej platformie, największy rozmiar czcionki dostępne:

![](device-images/maximum-size.png "Style dostępnego urządzenia dużych na każdej platformie")

## <a name="summary"></a>Podsumowanie

Platformy Xamarin.Forms obejmuje sześć *dynamiczne* style, znany jako *urządzenia* style w [ `Devices.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) klasy. Wszystkie sześć style może być stosowany tylko do [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) wystąpień.


## <a name="related-links"></a>Linki pokrewne

- [Style tekstu](~/xamarin-forms/user-interface/text/styles.md)
- [Rozszerzenia struktury znaczników XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Style dynamiczna (na przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/DynamicStyles/)
- [Praca z style (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [Device.Styles](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [Styl](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Metody ustawiającej](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
