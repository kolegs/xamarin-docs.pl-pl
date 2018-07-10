---
title: Podsumowanie rozdziałów 13. Mapy bitowe
description: 'Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms: Podsumowanie rozdziałów 13. Mapy bitowe'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5D153857-B6B7-4A14-8FB9-067DE198C2C7
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 0f9b9e27afd5dbbf52f3653995470136e794f17b
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935202"
---
# <a name="summary-of-chapter-13-bitmaps"></a>Podsumowanie rozdziałów 13. Mapy bitowe

Xamarin.Forms [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) element Wyświetla mapę bitową. Wszystkie platformy Xamarin.Forms obsługuje formaty plików JPEG, PNG, GIF i BMP.

Mapy bitowe, w interfejsie Xamarin.Forms pochodzą z czterech miejsc:

- W sieci web określony przez adres URL
- Osadzony jako zasób w typowych Portable Class Library
- Osadzony jako zasób w projektach aplikacji platformy
- Z dowolnego miejsca, które mogą być przywoływane przez .NET `Stream` obiekt, w tym `MemoryStream`

Zasoby mapy bitowej w aplikacji PCL są niezależne od platformy, zasoby mapy bitowej w projektach platformy są specyficzne dla platformy.

Mapa bitowa jest określany przez ustawienie [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Source/) właściwość `Image` do obiektu typu [ `ImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/), trzy pochodne klasy abstrakcyjnej:

- [`UriImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/) do uzyskania dostępu do mapy bitowej w sieci web na podstawie `Uri` obiektu jest równa jego [ `Uri` ](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.Uri/) właściwości
- [`FileImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.FileImageSource/) do uzyskania dostępu do mapy bitowej przechowywanych w projekcie aplikacji platformy na podstawie ścieżki plików i folderów, równa jego [ `File` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FileImageSource.File/) właściwości
- [`StreamImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.StreamImageSource/) Ładowanie mapy bitowej przy użyciu platformy .NET `Stream` obiekt określony przez zwrócenie `Stream` z `Func` równa jego [ `Stream` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StreamImageSource.Stream/) właściwości

Możesz też (i częściej) można użyć następujących metod statycznych `ImageSource` klasy wszystkie które zwrotu `ImageSource` obiektów:

- [`ImageSource.FromUri`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) do uzyskania dostępu do mapy bitowej w sieci web na podstawie `Uri` obiektu
- [`ImageSource.FromResource`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) do uzyskania dostępu do mapy bitowej składowaną zasobu osadzonego w aplikacji PCL, lub [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/System.Type/) lub [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/System.Reflection.Assembly/) do dostępu do mapy bitowej w innym zestawie źródła
- [`ImageSource.FromFile`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromFile/p/System.String/) do uzyskania dostępu do mapy bitowej z projektu aplikacji platformy
- [`ImageSource.FromStream`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromStream/p/System.Func%7BSystem.IO.Stream%7D/) Ładowanie mapy bitowej na podstawie `Stream` obiektu

Nie ma odpowiednika klasy z `Image.FromResource` metody. `UriImageSource` Klasa jest przydatna, jeśli wymagane jest sterowanie buforowania. `FileImageSource` Klasy przydaje się w XAML. `StreamImageSource` jest przydatne w przypadku asynchroniczne ładowanie `Stream` obiektów, podczas gdy `ImageSource.FromStream` jest synchroniczne.

## <a name="platform-independent-bitmaps"></a>Map bitowych niezależnych od platformy

[ **WebBitmapCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapCode) projekt ładuje mapę bitową za pośrednictwem sieci web przy użyciu `ImageSource.FromUri`. `Image` Element jest ustawiony na `Content` właściwość `ContentPage`, więc jest ograniczony do rozmiaru strony. Bez względu na rozmiar mapy bitowej ograniczone `Image` element jest rozciągnięty do rozmiaru kontenera w jego i mapy bitowej jest wyświetlany w maksymalnego rozmiaru w ramach `Image` elementu przy zachowaniu współczynnika proporcji mapy bitowej. Obszary `Image` poza stosowano mapy bitowej [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/).

[ **WebBitmapXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapXaml) przykład jest podobny, ale po prostu ustawia `Source` właściwości do adresu URL. Konwersja odbywa się przez [ `ImageSourceConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSourceConverter/) klasy.

### <a name="fit-and-fill"></a>Dopasuj i wypełnienia

Można kontrolować, jak mapa bitowa jest rozciągany tak, ustawiając [ `Aspect` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Aspect/) właściwość `Image` do jednej z następujących członków [ `Aspect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Aspect/) wyliczenia:

- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit): szanuje współczynnik proporcji (ustawienie domyślne)
- [`Fill`](xref:Xamarin.Forms.Aspect.Fill): wypełnia obszar, nie przestrzega współczynnik proporcji
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill): wypełnia obszar, ale uwzględnia współczynnik proporcji, realizowane przez przycinanie część mapy bitowej

### <a name="embedded-resources"></a>Zasoby osadzone

Plik mapy bitowej można dodać do aplikacji PCL lub do folderu w aplikacji PCL. Nadaj **Build Action** z **EmbeddedResource**. [ **ResourceBitmapCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ResourceBitmapCode) przykład pokazuje, jak używać `ImageSource.FromResource` można załadować pliku. Nazwa zasobu przekazywany do metody składa się z nazwy zestawu, a następnie kropka, a następnie według nazwy folderu opcjonalne i kropka, a następnie nazwę pliku.

Ustawia program `VerticalOptions` i `HorizontalOptions` właściwości `Image` do `LayoutOptions.Center`, co sprawia, że `Image` element nieograniczone. `Image` i mapy bitowej mają taki sam rozmiar:

- W systemach iOS i Android `Image` jest rozmiar mapy bitowej w pikselach. Istnieje mapowanie jeden do jednego między pikselami mapy bitowej, a w pikselach.
- Na platformach Windows Runtime `Image` jest rozmiar w pikselach mapy bitowej w jednostkach niezależnych od urządzenia. Na większości urządzeń każdego piksela mapy bitowej zajmuje wiele w pikselach.

[ **StackedBitmap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/StackedBitmap) przykładowy umieszcza `Image` w pionie `StackLayout` w XAML. Rozszerzenia znaczników o nazwie [ `ImageResourceExtension` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter13/StackedBitmap/StackedBitmap/StackedBitmap/ImageResourceExtension.cs) pozwala odwoływać się do osadzonego zasobu w XAML. Ta klasa ładowane są tylko zasoby z zestawu, w którym się znajduje, więc nie można umieścić w bibliotece.

### <a name="more-on-sizing"></a>Więcej informacji na temat zmiany rozmiaru

Często jest to pożądane, aby rozmiar mapy bitowe, stale wśród wszystkich platform.
Eksperymentowanie z **StackedBitmap**, można ustawić `WidthRequest` na `Image` element pionowym `StackLayout` się rozmiar spójne we wszystkich platform, ale tylko wtedy można zmniejszyć rozmiar przy użyciu tej metody.

Można również ustawić `HeightRequest` obraz o rozmiarach spójne na platformach, ale ograniczone szerokość mapy bitowej ograniczy uniwersalność tej techniki. W przypadku obrazu w pionie `StackLayout`, `HeightRequest` należy unikać.

Najlepszym rozwiązaniem jest zaczynają się od mapę bitową większa niż szerokość telefonu w jednostkach niezależnych od urządzenia, a następnie ustaw `WidthRequest` na żądaną szerokość w jednostkach niezależnych od urządzenia. Jest to zaprezentowane w [ **DeviceIndBitmapSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DeviceIndBitmapSize) próbki.

[ **MadTeaParty** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/MadTeaParty) Wyświetla rozdział 7 w roku *przygód Alicji w Wonderland* przy użyciu oryginalnego ilustracje przez Tenniel Jan:

[![Potrójna zrzut ekranu przedstawiający mad — strona herbaty](images/ch13fg16-small.png "Mad tekst książki strona herbaty Hatters")](images/ch13fg16-large.png#lightbox "Mad Hatters strona herbaty książki tekstu")

### <a name="browsing-and-waiting"></a>Przeglądanie i oczekiwania

[ **ImageBrowser** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageBrowser) przykładowe umożliwia użytkownikowi przeglądanie za pomocą standardowych obrazów przechowywanych w witrynie sieci web platformy Xamarin. Używa ona .NET `WebRequest` klasy, aby pobrać plik JSON z listą mapy bitowej.

Program używa [ `ActivityIndicator` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/) do wskazania, że coś, co się dzieje. Ponieważ ładowania każdą mapę bitową tylko do odczytu [ `IsLoading` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.IsLoading/) właściwość `Image` jest `true`. `IsLoading` Właściwość jest wspierana przez właściwości możliwej do wiązania, więc `PropertyChanged` zdarzenie jest wywoływane po zmianie właściwości. Dołącza obsługę do tego zdarzenia, program używa bieżącego ustawienia `IsLoaded` można ustawić [ `IsRunning` ](https://api/property/Xamarin.Forms.ActivityIndicator.IsRunning/) właściwość `ActivityIndicator`.

## <a name="streaming-bitmaps"></a>Przesyłanie strumieniowe map bitowych

`ImageSource.FromStream` Metoda tworzy `ImageSource` oparte na platformie .NET `Stream`. Metoda muszą być przekazywane `Func` obiekt, który zwraca `Stream` obiektu.

### <a name="accessing-the-streams"></a>Uzyskiwanie dostępu do strumieni

[ **BitmapStreams** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/BitmapStreams) przykład pokazuje, jak używać `ImaageSource.FromStream` metody, można załadować mapy bitowej przechowywane w postaci zasobów osadzonych i załadować mapy bitowej w sieci Web.

### <a name="generating-bitmaps-at-run-time"></a>Generowanie mapy bitowej w czasie wykonywania

Wszystkie platformy Xamarin.Forms obsługuje nieskompresowanych formatu pliku BMP, łatwych do utworzenia w kodzie, a następnie zapisać w `MemoryStream`. Ta technika pozwala algorithmically Tworzenie mapy bitowej w czasie wykonywania, zaimplementowanego w [ `BmpMaker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs) klasy w **Xamrin.FormsBook.Toolkit** biblioteki.

"Należy ją samodzielnie" [ **DiyGradientBitmap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DiyGradientBitmap) przykład demonstruje użycie `BmpMaker` do utworzenia mapy bitowej przy użyciu gradientów obrazu.

## <a name="platform-specific-bitmaps"></a>Mapy bitowe specyficzne dla platformy

Wszystkie platformy Xamarin.Forms umożliwia przechowywanie mapy bitowe w zestawach aplikacji platformy. Po pobraniu przez aplikację platformy Xamarin.Forms, mapy bitowe tych platform są typu [ `FileImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FileImageSource/). Można je wykorzystać do:

- [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Icon/) właściwość [`MenuItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/)
- [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Icon/) właściwość [`ToolbarItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/)
- [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) właściwość `Button`

Zestawy platformy zawierają już map bitowych, ikon i ekrany powitalne:

- W projekcie dla systemu iOS w **zasobów** folderu
- W projekcie dla systemu Android w podfolderach **zasobów** folderu
- W projektach Windows w **zasoby** folder (mimo że do tego folderu na platformach Windows nie ograniczają mapy bitowe)

[ **PlatformBitmaps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/PlatformBitmaps) przykładowych użyto kodu, aby wyświetlić ikonę w projektach aplikacji platformy.

### <a name="bitmap-resolutions"></a>Rozwiązania mapy bitowej

Wszystkie platformy umożliwiają przechowywanie wielu wersji obrazy mapy bitowej dla rozdzielczości innego urządzenia. W czasie wykonywania odpowiednia wersja jest ładowany w zależności od rozdzielczości ekranu.

W systemach iOS te mapy bitowe są zróżnicowane według sufiks nazwy pliku:

- Brak przyrostka 160 DPI urządzeń (1 piksel w jednostce niezależnej od urządzenia)
- "@2x" sufiks 320 DPI urządzeń (w pikselach 2 do DIU)
- "@3x" sufiks 480 DPI urządzeń (3 pikseli DIU)

Mapy bitowej powinny być wyświetlane jako jeden cala kwadrat istniałby w trzech wersjach:

- Mójobraz.jpg w kształcie kwadratu 160 pikseli
- MyImage@2x.jpg w kształcie kwadratu 320 pikseli
- MyImage@3x.jpg na kształcie kwadratu 480 pikseli

Program będzie odwoływać się do tej mapy bitowej jako Mójobraz.jpg, ale odpowiednią wersję są pobierane w czasie wykonywania na podstawie rozdzielczości ekranu. Gdy nieograniczone, mapy bitowej zawsze będzie renderowany w 160 jednostek niezależnych od urządzenia.

Dla systemów Android, mapy bitowe są przechowywane w różnych podfolderach **zasobów** folderu:

- drawable-ldpi (niski DPI) dla urządzeń 120 DPI (0,75 pikseli DIU)
- drawable-mdpi (średni) dla urządzeń 160 DPI (1 piksel DIU)
- drawable-hdpi (wysoka) dla urządzeń 240 DPI (1,5 raza więcej pikseli DIU)
- drawable-xhdpi (bardzo wysoka) dla urządzeń 320 DPI (2 pikseli DIU)
- drawable-xxhdpi (bardzo bardzo wysoka) dla urządzeń 480 DPI (3 pikseli DIU)
- drawable-xxxhdpi (trzy dodatkowe takich) dla urządzeń 640 DPI (4 pikseli DIU)

Dla mapy bitowej ma być renderowany w jednym cala kwadratowego różne wersje mapy bitowej będzie mieć taką samą nazwę, ale innego rozmiaru i znajdować się w tych folderach:

- drawable-ldpi/Mójobraz.jpg w kształcie kwadratu 120 pikseli
- drawable-mdpi/Mójobraz.jpg w kształcie kwadratu 160 pikseli
- drawable-hdpi/Mójobraz.jpg w kształcie kwadratu 240 pikseli
- drawable-xhdpi/Mójobraz.jpg w kształcie kwadratu 320 pikseli
- drawable-xxhdpi/Mójobraz.jpg na kwadratowym 480 pikseli
- drawable-xxxhdpi/Mójobraz.jpg w kształcie kwadratu 640 pikseli

Mapa bitowa zawsze będzie renderowany w 160 jednostek niezależnych od urządzenia. (Standardowy szablon rozwiązania Xamarin.Forms zawiera tylko hdpi xhdpi i foldery xxhdpi.)

Projekty Windows Runtime obsługują mapy bitowej schemat, który składa się z współczynnik skalowania w pikselach na jednostka miary niezależna od urządzenia jako procent, na przykład nazwy:

- MyImage.scale-200.jpg w kształcie kwadratu 320 pikseli

Prawidłowe są tylko niektóre wartości procentowe. Przykładowe programy książki obejmują tylko obrazy o **200 skalowania** sufiksów, ale bieżące szablony rozwiązania Xamarin.Forms obejmują **skali 100**, **125 skalowania**, **150 skalowania**, i **400 skalowania**.

Podczas dodawania mapy bitowe w projektach platformy **Build Action** powinno być:

- dla systemu iOS: **BundleResource**
- Android: **AndroidResource**
- Środowisko wykonawcze Windows: **zawartości**

[ **ImageTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageTap) Przykładowa aplikacja tworzy dwa obiekty przycisku przypominającej składający się z `Image` elementów za pomocą `TapGestureRecognizer` zainstalowane. Jest ona przeznaczona, obiekty znajdować się jeden cala kwadrat. `Source` Właściwość `Image` można ustawić przy użyciu `OnPlatform` i `On` obiektów, aby odwoływać się do innej nazwy plików na platformach. Obrazy mapy bitowej obejmują numery wskazują ich rozmiar w pikselach, aby było widać, których rozmiar mapy bitowej są pobierane i renderowania.

### <a name="toolbars-and-their-icons"></a>Paski narzędzi i ich ikony

Jednym z podstawowych zastosowań mapy bitowe specyficzne dla platformy jest narzędzi Xamarin.Forms, który jest tworzony przez dodanie [ `ToolbarItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/) obiekty do [ `ToolbarItems` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.ToolbarItems/) kolekcji określone przez `Page`. `ToobarItem` pochodzi od klasy [ `MenuItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/) z którego dziedziczy niektórych właściwości.

Najważniejsze `ToolbarItem` właściwości są:

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Text/) tekst, który może pojawić się w zależności od platformy i `Order`
- [`Icon`](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Icon/) typu `FileImageSource` obrazu, który może pojawić się w zależności od platformy i `Order`
- [`Order`](xref:Xamarin.Forms.ToolbarItem.Order) typu [ `ToolbarItemOrder` ](xref:Xamarin.Forms.ToolbarItemOrder), moduł wyliczenie z trzema elementami członkowskimi, [ `Default` ](xref:Xamarin.Forms.ToolbarItemOrder.Default), [ `Primary` ](xref:Xamarin.Forms.ToolbarItemOrder.Primary), i [ `Secondary` ](xref:Xamarin.Forms.ToolbarItemOrder.Secondary).

Liczba `Primary` elementy powinny być ograniczone do trzech lub czterech. Należy uwzględnić `Text` ustawienie dla wszystkich elementów. W przypadku większości platform tylko `Primary` elementy wymagają `Icon` , ale wymaga Windows 8.1 `Icon` dla wszystkich elementów. Ikony powinny być kwadratowe 32 jednostek niezależnych od urządzenia. `FileImageSource` Typu wskazuje, że są one specyficzne dla platformy.

`ToolbarItem` Generowane [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.MenuItem.Clicked/) zdarzenie, kiedy nacisnął, podobnie jak `Button`. `ToolbarItem` obsługuje również [ `Command` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Command/) i [ `CommandParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.CommandParameter/) właściwości często używane w połączeniu z modelem MVVM. (Zobacz [rozdział 18, MVVM](chapter18.md)).

Systemów iOS i Android wymagają strona, wyświetlająca pasek narzędzi [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) lub strony przejście przez `NavigationPage`. [ **ToolbarDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ToolbarDemo) program zestawy `MainPage` właściwość jego `App` klasy [ `NavigationPage` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.NavigationPage.NavigationPage/p/Xamarin.Forms.Page/) z `ContentPage` argument i pokazuje konstrukcja i zdarzenie obsługi paska narzędzi.

### <a name="button-images"></a>Obrazy przycisków

Umożliwia także map bitowych określonych platform można ustawić [ `Image` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Image/) właściwość `Button` do mapy bitowej kwadratu 32 jednostek niezależnych od urządzenia, jak pokazano w [ **ButtonImage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ButtonImage) próbki.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdziale 13 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch13-Apr2016.pdf)
- [Przykłady rozdziale 13](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)
- [Praca z obrazami](~/xamarin-forms/user-interface/images.md)
