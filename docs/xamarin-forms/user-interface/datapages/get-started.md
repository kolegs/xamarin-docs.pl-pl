---
title: Wprowadzenie do DataPages
description: W tym artykule wyjaśniono, jak zacząć tworzyć proste strony oparte na danych przy użyciu zestawu narzędzi Xamarin.Forms DataPages.
ms.prod: xamarin
ms.assetid: 6416E5FA-6384-4298-BAA1-A89381E47210
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 1fb8a06111271d453c578cd3d2db97ec8689c995
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38828214"
---
# <a name="getting-started-with-datapages"></a>Wprowadzenie do DataPages

![](~/media/shared/preview.png "Ten interfejs API jest obecnie w wersji zapoznawczej")

> [!IMPORTANT]
> Wymaga DataPages [motyw Xamarin.Forms](~/xamarin-forms/user-interface/themes/index.md) odwołania do renderowania.


Aby rozpocząć tworzenie prostego strony oparte na danych przy użyciu podglądu DataPages, wykonaj poniższe kroki. Używa tej wersji demonstracyjnej, kompilacje style zapisane na stałe ("zdarzenia") w wersji zapoznawczej, które działa tylko z określonym formacie JSON w kodzie.

[![](get-started-images/demo-sml.png "Aplikacja przykładowa DataPages")](get-started-images/demo.png#lightbox "DataPages przykładowej aplikacji")

## <a name="1-add-nuget-packages"></a>1. Dodawanie pakietów NuGet

Dodaj te pakiety Nuget do zestawu narzędzi Xamarin.Forms .NET Standard projektów biblioteki i aplikacji:

* Xamarin.Forms.Pages
* Xamarin.Forms.Theme.Base
* Z implementacją motyw Nuget (np.) Xamarin.Forms.Themes.Light)

## <a name="2-add-theme-reference"></a>2. Dodaj odwołanie do motywu

W **App.xaml** Dodaj niestandardowy `xmlns:mytheme` motywu i upewnij się, motyw jest scalany ze słownika zasobów aplikacji:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
  xmlns:mytheme="clr-namespace:Xamarin.Forms.Themes;assembly=Xamarin.Forms.Theme.Light"
  x:Class="DataPagesDemo.App">
    <Application.Resources>
        <ResourceDictionary MergedWith="mytheme:LightThemeResources" />
    </Application.Resources>
</Application>
```

**Ważne:** należy również wykonać kroki, aby [ładować zestawy motywu (poniżej)](#loadtheme) przez dodanie niektórych schematyczny kod służący do systemu iOS `AppDelegate` i Android `MainActivity`. To zostanie on ulepszony w wersji zapoznawczej w przyszłości.


## <a name="3-add-a-xaml-page"></a>3. Dodawanie strony XAML

Dodaj nową stronę XAML do aplikacji platformy Xamarin.Forms i *Zmień klasę bazową* z `ContentPage` do `Xamarin.Forms.Pages.ListDataPage`. Ma to odbywać się zarówno w języku C# i XAML:

**Plik języka C#**

```csharp
public partial class SessionDataPage : Xamarin.Forms.Pages.ListDataPage // was ContentPage
{
  public SessionDataPage ()
  {
    InitializeComponent ();
  }
}
```

**Plik XAML**

Validated elementu głównego dla `<p:ListDataPage>` niestandardowej przestrzeni nazw dla `xmlns:p` musi również zostać dodany:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<p:ListDataPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:p="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
             x:Class="DataPagesDemo.SessionDataPage">

    <ContentPage.Content></ContentPage.Content>

</p:ListDataPage>
```

**Podklasy aplikacji**

Zmiana `App` konstruktora klasy tak, aby `MainPage` ustawiono `NavigationPage` zawierające nowy `SessionDataPage`. Strona nawigacji *musi* można użyć.

```csharp
MainPage = new NavigationPage (new SessionDataPage ());
```

## <a name="3-add-the-datasource"></a>3. Dodaj źródło danych

Usuń `Content` elementu i zastąp go wartością `p:ListDataPage.DataSource` do wypełniania strony z danymi. W przykładzie poniżej Json zdalnego pliku danych jest ładowany z adresu URL.

**Uwaga:** korzystania z wersji zapoznawczej *wymaga* `StyleClass` atrybutu, aby zapewnić wskazówki renderowania dla źródła danych. `StyleClass="Events"` Odwołuje się do układu, który jest wstępnie zdefiniowane w wersji zapoznawczej i zawiera style *zapisane na stałe* do dopasowania używanego źródła danych JSON.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<p:ListDataPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:p="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
             x:Class="DataPagesDemo.SessionDataPage"
             Title="Sessions" StyleClass="Events">

    <p:ListDataPage.DataSource>
        <p:JsonDataSource Source="http://demo3143189.mockable.io/sessions" />
    </p:ListDataPage.DataSource>

</p:ListDataPage>
```

**Dane JSON**

Przykładowe dane JSON z [źródła pokaz](http://demo3143189.mockable.io/sessions) znajdują się poniżej:

```json
[{
  "end": "2016-04-27T18:00:00Z",
  "start": "2016-04-27T17:15:00Z",
  "abstract": "The new Apple TV has been released, and YOU can be one of the first developers to write apps for it. To make things even better, you can build these apps in C#! This session will introduce the basics of how to create a tvOS app with Xamarin, including: differences between tvOS and iOS APIs, TV user interface best practices, responding to user input, as well as the capabilities and limitations of building apps for a television. Grab some popcorn—this is going to be good!",
  "title": "As Seen On TV … Bringing C# to the Living Room",
  "presenter": "Matthew Soucoup",
  "biography": "Matthew is a Xamarin MVP and Certified Xamarin Developer from Madison, WI. He founded his company Code Mill Technologies and started the Madison Mobile .Net Developers Group.  Matt regularly speaks on .Net and Xamarin development at user groups, code camps and conferences throughout the Midwest. Matt gardens hot peppers, rides bikes, and loves Wisconsin micro-brews and cheese.",
  "image": "http://i.imgur.com/ASj60DP.jpg",
  "avatar": "http://i.imgur.com/ASj60DP.jpg",
  "room": "Crick"
}]
```

## <a name="4-run"></a>4. Uruchamianie!

Powyższe kroki powinno spowodować strony danych roboczych:

[![](get-started-images/demo-sml.png "Aplikacja przykładowa DataPages")](get-started-images/demo.png#lightbox "DataPages przykładowej aplikacji")

To działa, ponieważ wstępnie skompilowanych styl **"miara Events"** istnieje w pakiecie Nuget motyw jasny i style zdefiniowane, które pasuje do źródła danych (np.) "title", "image", "prezentera").

"Events" `StyleClass` został opracowany pod kątem wyświetlania `ListDataPage` kontrola przy użyciu niestandardowego `CardView` oznacza to kontrolować Xamarin.Forms.Pages zdefiniowaną w. `CardView` Kontrolka ma trzy właściwości: `ImageSource`, `Text`, i `Detail`. Motyw jest zapisane na stałe, można powiązać źródła trzy pola (z pliku JSON) do tych właściwości do wyświetlenia.

## <a name="5-customize"></a>5. Dostosuj

Styl dziedziczonych można przesłonić, określając szablonu i za pomocą powiązania źródła danych. Poniżej XAML deklaruje szablonu niestandardowego dla każdego wiersza za pomocą nowego `ListItemControl` i `{p:DataSourceBinding}` składni, która jest ujęta w **Xamarin.Forms.Pages** Nuget:

```xaml
<p:ListDataPage.DefaultItemTemplate>
    <DataTemplate>
        <ViewCell>
            <p:ListItemControl
                Title="{p:DataSourceBinding title}"
                Detail="{p:DataSourceBinding room}"
                ImageSource="{p:DataSourceBinding image}"
                DataSource="{Binding Value}"
                HeightRequest="90"
            >
            </p:ListItemControl>
        </ViewCell>
    </DataTemplate>
</p:ListDataPage.DefaultItemTemplate>
```

Zapewniając `DataTemplate` ten kod zastępuje `StyleClass` i zamiast tego używa domyślnego układu `ListItemControl`.

[![](get-started-images/custom-sml.png "Aplikacja przykładowa DataPages")](get-started-images/custom.png#lightbox "DataPages przykładowej aplikacji")

Deweloperów, którzy wolą języka C# do XAML można tworzyć dane źródło powiązania zbyt (Pamiętaj, aby uwzględnić `using Xamarin.Forms.Pages;` instrukcji):

```csharp
SetBinding (TitleProperty, new DataSourceBinding ("title"));
```


Jest to nieco więcej pracy, aby utworzyć motywy od podstaw (zobacz [przewodnik motywy](~/xamarin-forms/user-interface/themes/index.md)), lecz przyszłe wersje zapoznawcze ułatwi to zrobić.


## <a name="troubleshooting"></a>Rozwiązywanie problemów

<a name="loadtheme" />

## <a name="could-not-load-file-or-assembly-xamarinformsthemelight-or-one-of-its-dependencies"></a>Nie można załadować pliku lub zestawu "Xamarin.Forms.Theme.Light" lub jednej z jego zależności

W wersji zapoznawczej motywów nie można załadować w czasie wykonywania. Dodaj kod pokazany poniżej do odpowiednie projekty, aby naprawić ten błąd.

**iOS**

W **AppDelegate.cs** Dodaj następujące wiersze po `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.iOS.UnderlineEffect);
```

**Android**

W **MainActivity.cs** Dodaj następujące wiersze po `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.Android.UnderlineEffect);
```



## <a name="related-links"></a>Linki pokrewne

- [Przykładowe DataPagesDemo](https://github.com/xamarin/xamarin-forms-samples/tree/master/Pages/DataPagesDemo)
