---
title: Podsumowanie rozdziale 13. Mapy bitowe
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5D153857-B6B7-4A14-8FB9-067DE198C2C7
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 76551057abc1abdd150591c0a1be39e9f68c4278
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-13-bitmaps"></a>Podsumowanie rozdziale 13. Mapy bitowe

Platformy Xamarin.Forms [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) element Wyświetla mapy bitowej. Wszystkie platformy platformy Xamarin.Forms obsługują formaty plików JPEG, GIF, PNG i BMP.

Mapy bitowe w platformy Xamarin.Forms pochodzą z czterech miejscach:

- Za pośrednictwem sieci web, określony przez adres URL
- Osadzony jako zasób w typowych przenośnej biblioteki klas
- Osadzony jako zasób w projektach aplikacji platformy
- Z dowolnego miejsca, które mogą odwoływać się .NET `Stream` obiektu, w tym `MemoryStream`

Zasoby mapy bitowej w PCL są niezależne od platformy, podczas gdy zasoby mapy bitowej w projektach platformy są specyficzne dla platformy.

Mapy bitowej jest określany przez ustawienie [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Source/) właściwość `Image` do obiektu typu [ `ImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/), trzy pochodne klasy abstrakcyjnej:

- [`UriImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/) do uzyskiwania dostępu do mapy bitowej za pośrednictwem sieci web, na podstawie `Uri` ustawioną obiektu jego [ `Uri` ](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.Uri/) właściwości
- [`FileImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.FileImageSource/) do uzyskiwania dostępu do mapy bitowej przechowywanych w projekcie aplikacji platformy na podstawie ścieżki folderu i pliku ustawioną jego [ `File` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FileImageSource.File/) właściwości
- [`StreamImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.StreamImageSource/) ładowania mapy bitowej przy użyciu platformy .NET `Stream` obiekt określony przez zwrócenie `Stream` z `Func` ustawioną jego [ `Stream` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StreamImageSource.Stream/) właściwości

Możesz też (i częściej) można użyć następujących metod statycznych `ImageSource` klasy wszystkich które zwrotu `ImageSource` obiektów:

- [`ImageSource.FromUri`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) Aby uzyskać dostęp do mapy bitowej za pośrednictwem sieci web, na podstawie `Uri` obiektu
- [`ImageSource.FromResource`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) Aby uzyskać dostęp do mapy bitowej przechowywane jako osadzony zasób w aplikacji PCL, lub [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/System.Type/) lub [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/System.Reflection.Assembly/) mapy bitowej w innym zestawie źródła dostęp do
- [`ImageSource.FromFile`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromFile/p/System.String/) Aby uzyskać dostęp do mapy bitowej z projektu aplikacji platformy
- [`ImageSource.FromStream`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromStream/p/System.Func%7BSystem.IO.Stream%7D/) ładowania mapy bitowej na podstawie `Stream` obiektu

Nie ma odpowiednika klasy z `Image.FromResource` metody. `UriImageSource` Klasa jest przydatna, jeśli należy kontrolować buforowania. `FileImageSource` Klasy jest przydatne w języku XAML. `StreamImageSource` jest przydatne w przypadku asynchroniczne ładowanie `Stream` obiektów, podczas gdy `ImageSource.FromStream` jest synchroniczne.

## <a name="platform-independent-bitmaps"></a>Mapy bitowe niezależne od platformy

[ **WebBitmapCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapCode) projektu ładuje mapę bitową za pośrednictwem sieci web, przy użyciu `ImageSource.FromUri`. `Image` Element jest ustawiony na wartość `Content` właściwość `ContentPage`, więc jest ograniczona do rozmiaru strony. Niezależnie od rozmiaru mapy bitowej ograniczonego `Image` element jest rozciągany tak, aby rozmiar jego kontenera i mapy bitowej jest wyświetlany w maksymalnego rozmiaru w `Image` elementu przy zachowaniu współczynnika proporcji mapy bitowej. Obszary `Image` poza stosowano mapy bitowej [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/).

[ **WebBitmapXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapXaml) próbki jest podobny, ale po prostu ustawia `Source` właściwości do adresu URL. Konwersja jest obsługiwany przez [ `ImageSourceConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSourceConverter/) klasy.

### <a name="fit-and-fill"></a>Dopasuj i wypełnienia

Można kontrolować sposób mapy bitowej jest rozciągany tak, ustawiając [ `Aspect` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Aspect/) właściwość `Image` do jednego z następujących członków [ `Aspect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Aspect/) wyliczenie:

- [`AspectFit`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.AspectFit/): szanuje współczynnik proporcji (ustawienie domyślne)
- [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.Fill/): wypełnienia obszaru, nie przestrzega współczynnik proporcji
- [`AspectFill`](https://developer.xamarin.com/api/type/Xamarin.Forms.Aspect.AspectFill/): wypełnienia obszaru, ale szanuje proporcje osiągnąć przycinanie część mapy bitowej

### <a name="embedded-resources"></a>Zasobów osadzonych

PCL, lub do folderu w PCL, można dodać plik mapy bitowej. Nadaj **Akcja kompilacji** z **EmbeddedResource**. [ **ResourceBitmapCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ResourceBitmapCode) przykładzie pokazano sposób użycia `ImageSource.FromResource` można załadować pliku. Nazwa zasobu przekazywany do metody składa się z nazwy zestawu następuje kropką, następuje nazwa folderu opcjonalne i kropkę, a następnie nazwę pliku.

Ustawia program `VerticalOptions` i `HorizontalOptions` właściwości `Image` do `LayoutOptions.Center`, co czyni `Image` nieograniczonego elementu. `Image` i mapy bitowej mają taki sam rozmiar:

- W systemach iOS i Android `Image` pikseli rozmiar mapy bitowej. Brak mapowanie jeden do jednego między mapy bitowej pikseli i w pikselach.
- Na platformach Windows Runtime `Image` pikseli rozmiar mapy bitowej w jednostkach niezależnych od urządzenia. Na większości urządzeń każdego piksela mapy bitowej zajmuje wiele w pikselach.

[ **StackedBitmap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/StackedBitmap) przykładowe naraża `Image` w pionowej `StackLayout` w języku XAML. Rozszerzenie znacznika o nazwie [ `ImageResourceExtension` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter13/StackedBitmap/StackedBitmap/StackedBitmap/ImageResourceExtension.cs) pozwala odwoływać się do osadzonego zasobu w języku XAML. Ta klasa ładuje tylko zasoby z zestawu, w którym się znajduje, więc nie można umieścić w bibliotece.

### <a name="more-on-sizing"></a>Więcej informacji na temat zmiany rozmiaru

Często jest to pożądane map bitowych rozmiar spójnie wśród wszystkich platform.
Eksperymentowanie z **StackedBitmap**, można ustawić `WidthRequest` na `Image` element w pionie `StackLayout` być takie same dla wszystkich platform, ale rozmiar tylko zmniejszyć rozmiar przy użyciu tej metody.

Można również ustawić `HeightRequest` obraz rozmiary spójne na platformach, ale ograniczonego szerokość mapy bitowej ograniczy wszechstronność tej metody. W przypadku obrazu w pionowej `StackLayout`, `HeightRequest` należy unikać.

Najlepszym rozwiązaniem jest rozpocząć od mapy bitowej większa niż szerokość telefonu w jednostkach niezależnych od urządzenia i ustaw `WidthRequest` pożądaną szerokość w jednostkach niezależnych od urządzenia. To jest przedstawiona w [ **DeviceIndBitmapSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DeviceIndBitmapSize) próbki.

[ **MadTeaParty** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/MadTeaParty) Wyświetla rozdział 7 w roku *przygód Alicji w Wonderland* z oryginalnego ilustracje przez Tenniel Jan:

[![Potrójna zrzut ekranu strony zepołowy mad —](images/ch13fg16-small.png "Mad tekst książki strona Zepołowy Hatters")](images/ch13fg16-large.png#lightbox "Mad Hatters strona Zepołowy książki tekstu")

### <a name="browsing-and-waiting"></a>Przeglądanie i oczekiwania

[ **ImageBrowser** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageBrowser) próbki umożliwia użytkownikowi przeglądanie standardowych obrazy przechowywane w witrynie sieci web platformy Xamarin. Używa programu .NET `WebRequest` klasy, aby pobrać plik JSON zawierający listę map bitowych.

Program używa [ `ActivityIndicator` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/) aby wskazać, że coś. Jak ładowania każdą mapę bitową tylko do odczytu [ `IsLoading` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.IsLoading/) właściwość `Image` jest `true`. `IsLoading` Właściwość nie jest obsługiwana przez właściwości możliwej do wiązania, więc `PropertyChanged` zdarzenie jest wywoływane po zmianie tej właściwości. Program obsługi jest dołączana do tego zdarzenia i używa bieżące ustawienie `IsLoaded` można ustawić [ `IsRunning` ](https://api/property/Xamarin.Forms.ActivityIndicator.IsRunning/) właściwość `ActivityIndicator`.

## <a name="streaming-bitmaps"></a>Mapy bitowe przesyłania strumieniowego

`ImageSource.FromStream` Metoda tworzy `ImageSource` oparte na .NET `Stream`. Metoda musi być przekazywany `Func` obiekt, który zwraca `Stream` obiektu.

### <a name="accessing-the-streams"></a>Uzyskiwanie dostępu do strumieni

[ **BitmapStreams** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/BitmapStreams) przykładzie pokazano sposób użycia `ImaageSource.FromStream` metody załadować mapy bitowej przechowywane jako osadzonego zasobu i załadować mapy bitowej w sieci Web.

### <a name="generating-bitmaps-at-run-time"></a>Generowanie map bitowych w czasie wykonywania

Wszystkie platformy platformy Xamarin.Forms obsługują nieskompresowanym formatem pliku BMP, łatwych do utworzenia w kodzie, a następnie umieść w `MemoryStream`. Ta technika pozwala algorithmically tworzenie map bitowych w czasie wykonywania, zgodnie z implementacją w [ `BmpMaker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs) klasy w **Xamrin.FormsBook.Toolkit** biblioteki.

"Należy ją samodzielnie" [ **DiyGradientBitmap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DiyGradientBitmap) przykładzie przedstawiono użycie `BmpMaker` można utworzyć mapy bitowej z obrazem gradientu.

## <a name="platform-specific-bitmaps"></a>Mapy bitowe specyficzne dla platformy

Wszystkie platformy platformy Xamarin.Forms Zezwalaj na zapisywanie map bitowych w zestawach aplikacji platformy. Po pobraniu przez aplikację platformy Xamarin.Forms tych map bitowych platform są typu [ `FileImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FileImageSource/). Można użyć go do:

- [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Icon/) właściwości [`MenuItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/)
- [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Icon/) właściwości [`ToolbarItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/)
- [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) właściwości `Button`

Zestawy platformy już zawierają mapy bitowe ikon i ekranów powitalnego:

- W projekcie systemu iOS w **zasobów** folderu
- W projekcie systemu Android w podfolderach **zasobów** folderu
- W projektach systemu Windows w **zasoby** folder (chociaż platformy systemu Windows nie ograniczają bitmap do tego folderu)

[ **PlatformBitmaps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/PlatformBitmaps) przykładowy kod jest używany do wyświetlana ikona w projektach aplikacji platformy.

### <a name="bitmap-resolutions"></a>Rozwiązania mapy bitowej

Wszystkie platformy umożliwiają przechowywanie wielu wersji obrazów mapy bitowej dla rozdzielczości innego urządzenia. W czasie wykonywania odpowiedniej wersji jest ładowany w zależności od rozdzielczości ekranu.

W systemach iOS te map bitowych są zróżnicowane przez sufiks nazwy pliku:

- Nie sufiks 160 DPI urządzenia (1 piksela jednostki niezależnych od urządzenia)
- "@2x" sufiks 320 DPI urządzeń (2 pikseli DIU)
- "@3x" sufiks 480 DPI urządzeń (3 pikseli DIU)

Bitmapy powinny być wyświetlane jako jeden cala kwadratowe będą istniały w trzech wersjach:

- Mójobraz.jpg na 160 pikseli kwadratowych
- MyImage@2x.jpg na 320 pikseli kwadratowych
- MyImage@3x.jpg na 480 pikseli kwadratowych

Program może odwołać się do tej mapy bitowej jako Mójobraz.jpg, ale poprawnej wersji są pobierane w czasie wykonywania na podstawie rozdzielczości ekranu. Gdy nieograniczonego, mapy bitowej zawsze będzie renderowany w 160 jednostki niezależnych od urządzenia.

Dla systemu Android, mapy bitowe są przechowywane w różnych podfolderach **zasobów** folderu:

- obiektów drawable-ldpi (DPI niski) dla urządzeń 120 DPI (0,75 pikseli DIU)
- obiektów drawable-mdpi (średnia liczba godzin) dla urządzeń 160 DPI (1 piksela DIU)
- obiektów drawable-hdpi (wysoka) dla urządzeń 240 DPI (1,5 pikseli DIU)
- obiektów drawable-xhdpi (bardzo dużo) dla urządzeń 320 DPI (2 pikseli DIU)
- obiektów drawable-xxhdpi (bardzo bardzo wysoka) dla urządzeń 480 DPI (3 pikseli DIU)
- obiektów drawable-xxxhdpi (trzy dodatkowe dobrych) dla urządzeń 640 DPI (4 pikseli DIU)

Dla mapy bitowej mają być renderowane w jednym cala kwadratowego różne wersje mapy bitowej będzie mieć taką samą nazwę, ale inny rozmiar, a w tych folderach:

- obiektów drawable-ldpi/Mójobraz.jpg na 120 pikseli kwadratowych
- obiektów drawable-mdpi/Mójobraz.jpg na 160 pikseli kwadratowych
- obiektów drawable-hdpi/Mójobraz.jpg na 240 pikseli kwadratowych
- obiektów drawable-xhdpi/Mójobraz.jpg na 320 pikseli kwadratowych
- drawable-xxhdpi/MyImage.jpg at 480 pixels square
- drawable-xxxhdpi/MyImage.jpg at 640 pixels square

Mapa bitowa zawsze będzie renderowany w 160 jednostki niezależnych od urządzenia. (Standardowy szablon rozwiązania platformy Xamarin.Forms zawiera tylko hdpi xhdpi i foldery xxhdpi.)

Projekty środowiska wykonawczego systemu Windows obsługują mapy bitowej nazewnictwa schematu, która składa się z czynnik skalowania w pikselach na jednostkę niezależnych od urządzenia jako procent, na przykład:

- MyImage.scale-200.jpg at 320 pixels square

Tylko niektóre wartości procentowe są prawidłowe. Przykładowe programy książki obejmują tylko obrazy z **200 skali** sufiksy, ale bieżący szablony rozwiązań platformy Xamarin.Forms obejmują **skali 100**, **125 skali**, **skali 150**, i **skali 400**. 

Gdy Dodawanie bitmap do projektów platformy **Akcja kompilacji** powinna być:

- iOS: **BundleResource**
- System android: **AndroidResource**
- Środowisko wykonawcze systemu Windows: **zawartości**

[ **ImageTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageTap) przykładowe tworzy dwa obiekty przycisk przypominającej składające się z `Image` elementy z `TapGestureRecognizer` zainstalowane. Ma ona, czy obiekty można kwadratowe jednego cala. `Source` Właściwość `Image` jest ustawiany za pomocą `OnPlatform` i `On` obiektów, aby odwołać potencjalnie różne nazwy plików na platformach. Obrazy mapy bitowej obejmują liczby wskazujące ich rozmiar pikseli pozwala zobaczyć, których rozmiar mapy bitowej jest pobierany i renderowane.

### <a name="toolbars-and-their-icons"></a>Paski narzędzi i ich ikony

Jednym z podstawowych zastosowań map bitowych specyficzne dla platformy jest narzędzi platformy Xamarin.Forms, który jest tworzony przez dodanie [ `ToolbarItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/) obiekty do [ `ToolbarItems` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.ToolbarItems/) zdefiniowanej przez kolekcji`Page`. `ToobarItem` pochodną [ `MenuItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/) po którym dziedziczy niektórych właściwości.

Najważniejsze `ToolbarItem` właściwości są:

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Text/) dla tekstu, który może pojawić się w zależności od platformy i `Order`
- [`Icon`](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Icon/) typu `FileImageSource` obrazu, który może pojawić się w zależności od platformy i `Order`
- [`Order`](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Order/) typu [ `ToolbarItemOrder` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItemOrder/), jako wyliczenie z trzech elementów członkowskich, [ `Default` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ToolbarItemOrder.Default/), [ `Primary` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ToolbarItemOrder.Primary/), i [ `Secondary` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ToolbarItemOrder.Secondary/).

Liczba `Primary` elementów powinien być ograniczony do trzech lub czterech. Należy uwzględnić `Text` ustawienie dla wszystkich elementów. W przypadku większości platform tylko `Primary` elementy wymagają `Icon` , ale wymaga Windows 8.1 `Icon` dla wszystkich elementów. Ikony powinna być 32 kwadratowy jednostki niezależnych od urządzenia. `FileImageSource` Typ wskazuje, że są specyficzne dla platformy.

`ToolbarItem` Uruchamiany [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.MenuItem.Clicked/) zdarzeń po dotknięciu, podobne jak w przypadku `Button`. `ToolbarItem` obsługuje również [ `Command` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Command/) i [ `CommandParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.CommandParameter/) często używane w połączeniu z modelem MVVM właściwości. (Zobacz [rozdział 18, MVVM](chapter18.md)).

Zarówno dla systemu iOS i Android wymagają strona, wyświetlająca paska narzędzi [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) lub strona, do którego prowadzi `NavigationPage`. [ **ToolbarDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ToolbarDemo) program zestawy `MainPage` właściwość jego `App` klasy do [ `NavigationPage` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.NavigationPage.NavigationPage/p/Xamarin.Forms.Page/) z `ContentPage` argument i pokazuje konstruowania i zdarzeń obsługi paska narzędzi.

### <a name="button-images"></a>Obrazy dla przycisków

Umożliwia także bitmapy specyficzne dla platformy można ustawić [ `Image` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Image/) właściwość `Button` do mapy bitowej kwadrat 32 jednostki niezależnych od urządzenia, jak pokazano w [ **ButtonImage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ButtonImage) próbki.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdziale 13 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch13-Apr2016.pdf)
- [Przykłady rozdziale 13](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)
- [Praca z obrazami](~/xamarin-forms/user-interface/images.md)
