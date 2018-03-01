---
title: "Standard języka XAML (wersja zapoznawcza)"
description: "Jak rozpocząć eksplorowanie Podgląd standardowe XAML w platformy Xamarin.Forms"
ms.topic: article
ms.prod: xamarin
ms.assetid: 24382DF1-BE70-4608-B86F-B79FB23E4A78
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: 3214a76ee3f976f5dd2afb28edde07100db5a7a1
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="xaml-standard-preview"></a>Standard języka XAML (wersja zapoznawcza)

![Wersja zapoznawcza](~/media/shared/preview.png)

Wykonaj następujące kroki do eksperymentów z normą XAML platformy Xamarin.Forms:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Pobierz [tutaj pakietu NuGet w wersji preview](https://aka.ms/xf-xamlstandard-nuget).
2. Dodaj **Xamarin.Forms.Alias** pakiet NuGet do projektów platformy Xamarin.Forms PCL .NET Standard i platformy.
3. Inicjowanie pakietu z `Alias.Init()`
4. Dodaj `xmlns:a` odwołania `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. Użyj typów w języku XAML — zobacz [kontrolki odwołania](controls.md) Aby uzyskać więcej informacji.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Pobierz [tutaj pakietu NuGet w wersji preview](https://aka.ms/xf-xamlstandard-nuget).
2. Dodaj **Xamarin.Forms.Alias** pakiet NuGet do projektów platformy Xamarin.Forms PCL .NET Standard i platformy.
3. Inicjowanie pakietu z `Alias.Init()`
4. Dodaj `xmlns:a` odwołania `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. Użyj typów w języku XAML — zobacz [kontrolki odwołania](controls.md) Aby uzyskać więcej informacji.

-----

Następujące XAML przedstawiono niektóre formanty standardowe XAML w platformy Xamarin.Forms `ContentPage`:

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
> **Uwaga:** wymagające xmlns `a:` prefiks XAML standardowych formantów jest to ograniczenie bieżącej wersji zapoznawczej.


## <a name="related-links"></a>Linki pokrewne

- [NuGet w wersji zapoznawczej](https://aka.ms/xf-xamlstandard-nuget)
- [Odwołanie do formantów](controls.md)
