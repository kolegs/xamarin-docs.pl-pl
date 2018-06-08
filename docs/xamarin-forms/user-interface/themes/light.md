---
title: Motywu jasny
ms.prod: xamarin
ms.assetid: D5D16AE3-F51F-4359-B37A-E1087ECE512B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 87c2a1a1003868aba10c7c1ec50856f307cc5bff
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848008"
---
# <a name="light-theme"></a>Motywu jasny

![](~/media/shared/preview.png "Ten interfejs API jest obecnie w wersji zapoznawczej")

> [!NOTE]
> Motywy wymagają wersji zapoznawczej 2.3 platformy Xamarin.Forms. Sprawdź [porady dotyczące rozwiązywania problemów](~/xamarin-forms/user-interface/themes/index.md) po wystąpieniu błędów.

Aby użyć motywu jasny:

## <a name="1-add-nuget-packages"></a>1. Dodawanie pakietów Nuget

* Xamarin.Forms.Theme.Base
* Xamarin.Forms.Theme.Light

## <a name="2-add-to-the-resource-dictionary"></a>2. Dodaj do słownika zasobów

W **App.xaml** pliku Dodawanie nowej niestandardowej `xmlns` motywu i upewnij się, motywu zasoby są łączone ze słownika zasobów aplikacji.
Poniżej przedstawiono przykładowy plik XAML:

```xaml
<?xml version="1.0" encoding="utf-8"?>
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="EvolveApp.App"
             xmlns:light="clr-namespace:Xamarin.Forms.Themes;assembly=Xamarin.Forms.Theme.Light">
    <Application.Resources>
        <ResourceDictionary MergedWith="light:LightThemeResources" />
    </Application.Resources>
</Application>
```

## <a name="3-load-theme-classes"></a>3. Załaduj motyw klas

Wykonaj to [Rozwiązywanie problemów z kroku](~/xamarin-forms/user-interface/themes/index.md) i Dodaj wymagane kod w projektów aplikacji systemu Android i iOS.

## <a name="4-use-styleclass"></a>4. Użyj StyleClass

Oto przykład przycisków i etykiet w motywu jasny, wraz z kod znaczników, który tworzy je.

[![](light-images/light-theme-sml.png "Przycisków i etykiet w motywu jasny")](light-images/light-theme.png#lightbox "przycisków i etykiet w motywu jasny")

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

