---
title: Motyw ciemny zestawu narzędzi Xamarin.Forms
description: W tym artykule wyjaśniono, jak używać zestawu narzędzi Xamarin.Forms ciemny motyw, który w aplikacji.
ms.prod: xamarin
ms.assetid: 43A3798D-6F05-4734-AF5E-97235B46D9B9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 1fc329f506afde04b0dc59dc637d999865aafbe1
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38853248"
---
# <a name="xamarinforms-dark-theme"></a>Motyw ciemny zestawu narzędzi Xamarin.Forms

![](~/media/shared/preview.png "Ten interfejs API jest obecnie w wersji zapoznawczej")

> [!NOTE]
> Motywy wymaga wersji zapoznawczej 2.3 zestawu narzędzi Xamarin.Forms. Sprawdź [wskazówki dotyczące rozwiązywania problemów](~/xamarin-forms/user-interface/themes/index.md) Jeśli wystąpią błędy.

Aby użyć ciemnego motywu:

## <a name="1-add-nuget-packages"></a>1. Dodawanie pakietów Nuget

* Xamarin.Forms.Theme.Base
* Xamarin.Forms.Theme.Dark

## <a name="2-add-to-the-resource-dictionary"></a>2. Dodaj do słownika zasobów

W **App.xaml** pliku Dodaj nowe niestandardowe `xmlns` motywu i upewnij się, że zasoby motywu są scalane z słownik zasobów aplikacji.
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

Postępuj zgodnie z tym [rozwiązywania problemów krok](~/xamarin-forms/user-interface/themes/index.md) i Dodaj kod wymagany projektów aplikacji dla systemu Android i iOS.

## <a name="4-use-styleclass"></a>4. Użyj StyleClass

Oto przykład przycisków i etykiet motywu ciemny, wraz z kodu znaczników, która je tworzy.

[![](dark-images/dark-theme-sml.png "Przycisków i etykiet motywu ciemny")](dark-images/dark-theme.png#lightbox "przycisków i etykiet motywu ciemny")

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

[Pełną listę wbudowanych klas](~/xamarin-forms/user-interface/themes/index.md) pokazuje, jakie style są dostępne w przypadku niektórych typowych kontrolek.
