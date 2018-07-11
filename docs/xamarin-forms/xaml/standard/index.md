---
title: XAML Standard (wersja zapoznawcza)
description: W tym artykule opisano sposób rozpocząć eksplorowanie XAML Standard (wersja zapoznawcza) w interfejsie Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 24382DF1-BE70-4608-B86F-B79FB23E4A78
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: 61e0fa2587ce9a8794dbd32ff9de1f13da857342
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38838013"
---
# <a name="xaml-standard-preview"></a>XAML Standard (wersja zapoznawcza)

![Wersja zapoznawcza](~/media/shared/preview.png)

Wykonaj następujące kroki, aby eksperymentować z XAML Standard w interfejsie Xamarin.Forms:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Pobierz [tutaj pakietu NuGet w wersji zapoznawczej](https://aka.ms/xf-xamlstandard-nuget).
2. Dodaj **Xamarin.Forms.Alias** pakiet NuGet do swoich projektów zestawu narzędzi Xamarin.Forms .NET Standard i platformy.
3. Inicjowanie pakietu `Alias.Init()`
4. Dodaj `xmlns:a` odwołania `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. Używanie typów w XAML — zobacz [dokumentacja formantów](controls.md) Aby uzyskać więcej informacji.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Pobierz [tutaj pakietu NuGet w wersji zapoznawczej](https://aka.ms/xf-xamlstandard-nuget).
2. Dodaj **Xamarin.Forms.Alias** pakiet NuGet do swoich projektów zestawu narzędzi Xamarin.Forms .NET Standard i platformy.
3. Inicjowanie pakietu `Alias.Init()`
4. Dodaj `xmlns:a` odwołania `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. Używanie typów w XAML — zobacz [dokumentacja formantów](controls.md) Aby uzyskać więcej informacji.

-----

Następujące XAML pokazuje niektóre kontrolki XAML Standard używany w interfejsie Xamarin.Forms `ContentPage`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ContentPage 
    xmlns="http://xamarin.com/schemas/2014/forms" 
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" 
    xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"
    x:Class="XAMLStandardSample.ItemsPage" 
    Title="{Binding Title}" x:Name="BrowseItemsPage">
    <ContentPage.ToolbarItems>
        <ToolbarItem Text="Add" Clicked="AddItem_Clicked" />
    </ContentPage.ToolbarItems>
    <ContentPage.Content>
        <a:StackPanel>
            <ListView x:Name="ItemsListView" ItemsSource="{Binding Items}" VerticalOptions="FillAndExpand" HasUnevenRows="true" RefreshCommand="{Binding LoadItemsCommand}" IsPullToRefreshEnabled="true" IsRefreshing="{Binding IsBusy, Mode=OneWay}" CachingStrategy="RecycleElement" ItemSelected="OnItemSelected">
                <ListView.ItemTemplate>
                    <DataTemplate>
                        <ViewCell>
                            <StackLayout Padding="10">
                                <a:TextBlock Text="{Binding Text}" LineBreakMode="NoWrap" Style="{DynamicResource ListItemTextStyle}" FontSize="16" />
                                <a:TextBlock Text="{Binding Description}" LineBreakMode="NoWrap" Style="{DynamicResource ListItemDetailTextStyle}" FontSize="13" />
                            </StackLayout>
                        </ViewCell>
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
        </a:StackPanel>
    </ContentPage.Content>
</ContentPage>
```

> [!NOTE]
> Wymaganie xmlns `a:` prefiks dla kontrolek XAML Standard jest ograniczenie bieżącej wersji zapoznawczej.


## <a name="related-links"></a>Linki pokrewne

- [NuGet w wersji zapoznawczej](https://aka.ms/xf-xamlstandard-nuget)
- [Dokumentacja kontrolek](controls.md)
