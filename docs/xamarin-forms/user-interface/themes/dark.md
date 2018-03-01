---
title: Ciemny motyw
ms.topic: article
ms.prod: xamarin
ms.assetid: 43A3798D-6F05-4734-AF5E-97235B46D9B9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 0b3a714e8295ed60f201c202fbe30e45dbdcc205
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="dark-theme"></a>Ciemny motyw

![](~/media/shared/preview.png "Ten interfejs API jest obecnie w wersji zapoznawczej")

> [!NOTE]
> Motywy wymagają wersji zapoznawczej 2.3 platformy Xamarin.Forms. Sprawdź [porady dotyczące rozwiązywania problemów](~/xamarin-forms/user-interface/themes/index.md) po wystąpieniu błędów.

Aby użyć ciemnego motywu:

## <a name="1-add-nuget-packages"></a>1. Dodawanie pakietów Nuget

* Xamarin.Forms.Theme.Base
* Xamarin.Forms.Theme.Dark

## <a name="2-add-to-the-resource-dictionary"></a>2. Dodaj do słownika zasobów

W **App.xaml** pliku Dodawanie nowej niestandardowej `xmlns` motywu i upewnij się, motywu zasoby są łączone ze słownika zasobów aplikacji.
Poniżej przedstawiono przykładowy plik XAML:

```xaml
<?xml version="1.0" encoding="utf-8"?>
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="EvolveApp.App"
             xmlns:dark="clr-namespace:Xamarin.Forms.Themes;assembly=Xamarin.Forms.Theme.Dark">
    <Application.Resources>
        <ResourceDictionary MergedWith="dark:DarkThemeResources" />
    </Application.Resources>
</Application>
```

## <a name="3-load-theme-classes"></a>3. Załaduj motyw klas

Wykonaj to [Rozwiązywanie problemów z kroku](~/xamarin-forms/user-interface/themes/index.md) i Dodaj wymagane kod w projektów aplikacji systemu Android i iOS.

## <a name="4-use-styleclass"></a>4. Użyj StyleClass

Oto przykład przycisków i etykiet w motywu ciemny, wraz z kod znaczników, który tworzy je.

[ ![](dark-images/dark-theme-sml.png "Przycisków i etykiet w ciemnym motywem")](dark-images/dark-theme.png "przycisków i etykiet w ciemny motyw")

```xaml
<StackLayout Padding="20">
    <Button Text="Button Default" />
    <Button Text="Button Class Default" StyleClass="Default" />
    <Button Text="Button Class Primary" StyleClass="Primary" />
    <Button Text="Button Class Success" StyleClass="Success" />
    <Button Text="Button Class Info" StyleClass="Info" />
    <Button Text="Button Class Warning" StyleClass="Warning" />
    <Button Text="Button Class Danger" StyleClass="Danger" />
    <Button Text="Button Class Link" StyleClass="Link" />

    <Button Text="Button Class Default Small" StyleClass="Small" />
    <Button Text="Button Class Default Large" StyleClass="Large" />
</StackLayout>
```

[Pełną listę wbudowanych klas](~/xamarin-forms/user-interface/themes/index.md) pokazuje, jakie style są dostępne dla niektórych typowych formantów.

