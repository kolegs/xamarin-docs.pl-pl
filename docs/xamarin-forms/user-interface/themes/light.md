---
title: Motyw jasny zestawu narzędzi Xamarin.Forms
description: W tym artykule wyjaśniono, jak używać zestawu narzędzi Xamarin.Forms motyw jasny w aplikacji.
ms.prod: xamarin
ms.assetid: D5D16AE3-F51F-4359-B37A-E1087ECE512B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 7f40e375d653acec60f8848627234ab46fcce8de
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38842755"
---
# <a name="xamarinforms-light-theme"></a>Motyw jasny zestawu narzędzi Xamarin.Forms

![](~/media/shared/preview.png "Ten interfejs API jest obecnie w wersji zapoznawczej")

> [!NOTE]
> Motywy wymaga wersji zapoznawczej 2.3 zestawu narzędzi Xamarin.Forms. Sprawdź [wskazówki dotyczące rozwiązywania problemów](~/xamarin-forms/user-interface/themes/index.md) Jeśli wystąpią błędy.

Aby użyć motyw jasny:

## <a name="1-add-nuget-packages"></a>1. Dodawanie pakietów Nuget

* Xamarin.Forms.Theme.Base
* Xamarin.Forms.Theme.Light

## <a name="2-add-to-the-resource-dictionary"></a>2. Dodaj do słownika zasobów

W **App.xaml** pliku Dodaj nowe niestandardowe `xmlns` motywu i upewnij się, że zasoby motywu są scalane z słownik zasobów aplikacji.
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

Postępuj zgodnie z tym [rozwiązywania problemów krok](~/xamarin-forms/user-interface/themes/index.md) i Dodaj kod wymagany projektów aplikacji dla systemu Android i iOS.

## <a name="4-use-styleclass"></a>4. Użyj StyleClass

Oto przykład przycisków i etykiet w motyw jasny, wraz z kodu znaczników, która je tworzy.

[![](light-images/light-theme-sml.png "Przycisków i etykiet w motyw jasny")](light-images/light-theme.png#lightbox "przycisków i etykiet w motyw jasny")

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
