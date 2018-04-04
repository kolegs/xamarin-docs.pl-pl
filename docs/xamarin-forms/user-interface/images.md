---
title: Obrazy
description: Obrazy mogą być udostępniane między platformy z platformy Xamarin.Forms, może być załadowany specjalnie dla każdej platformy, lub mogą być pobierane do wyświetlenia.
ms.prod: xamarin
ms.assetid: C025AB53-05CC-49BA-9815-75D6DF9E40B7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/15/2017
ms.openlocfilehash: ddbcb74d34f09c7bb60891148bd50b36bc5094c3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="images"></a>Obrazy

_Obrazy mogą być udostępniane między platformy z platformy Xamarin.Forms, może być załadowany specjalnie dla każdej platformy, lub mogą być pobierane do wyświetlenia._

Obrazy są kluczową kwestią nawigacji aplikacji, użytecznością i znakowania. Aplikacje platformy Xamarin.Forms muszą mieć możliwość udostępniania obrazów na wszystkich platformach, ale również wyświetlanie obrazów różne na różnych platformach.

Obrazy specyficzne dla platformy są również wymagane ikon i ekranów powitalnego; te należy skonfigurować na podstawie poszczególnych platform.

W tym dokumencie omówiono następujące tematy:

- [ **Obrazy lokalnego** ](#Local_Images) — wyświetlanie obrazów dostarczanych z aplikacji, w tym rozwiązaniu rozdzielczości, takich jak iOS siatkówki, Android lub platformy uniwersalnej systemu Windows wersji wysokiej rozdzielczości obrazu.
- [ **Obrazy osadzone** ](#Embedded_Images) — wyświetlanie obrazów osadzony jako zasób zestawu.
- [ **Pobrane obrazy** ](#Downloading_Images) — pobieranie i wyświetlanie obrazów.
- [ **Ikon i ekranów powitalnych** ](#Icons_and_splashscreens) -uruchamiania obrazów i ikon specyficzne dla platformy.

## <a name="displaying-images"></a>Wyświetlanie obrazów

Używa platformy Xamarin.Forms [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) widoku umożliwia wyświetlanie obrazów na stronie. Ma dwie ważne właściwości:

- [`Source`](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Source/) - [ `ImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/) Wystąpienia, pliku, identyfikatora Uri lub zasób, który ustawia obraz do wyświetlenia.
- [`Aspect`](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Aspect/) -Porady rozmiaru obrazu w granicach, który jest ona wyświetlana w ramach (czy stretch, przycięcia lub letterbox).

[`ImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/) wystąpienia można uzyskać za pomocą metod statycznych dla każdego typu źródło obrazu:

- [`FromFile`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromFile/p/System.String/) -Wymaga nazwy pliku lub ścieżka pliku, który można rozwiązać na każdej z platform.
- [`FromUri`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) -Np wymaga obiekt Uri.  `new Uri("http://server.com/image.jpg")` .
- [`FromResource`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) -Wymaga identyfikatorem zasobu z plikiem obrazu osadzonego w aplikacji lub PCL, z **akcji kompilacji: EmbeddedResource**.
- [`FromStream`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromStream/p/System.Func%7BSystem.IO.Stream%7D/) -Wymaga strumienia, który dostarcza dane obrazu.

[ `Aspect` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Aspect/) Właściwość określa, jak obraz będą skalowane w celu dopasowania do obszaru wyświetlania:

- [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.Fill/) -Obraz jest rozciągany tak aby całkowicie i dokładnie wypełnienia obszaru wyświetlania. Może to spowodować, że obraz jest zakłócona.
- [`AspectFill`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.AspectFill/) — Przycina obraz tak, aby wypełnił obszar wyświetlania przy zachowaniu aspektu (tzn. nie zakłócania).
- [`AspectFit`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.AspectFit/) -Letterboxes obrazu (w razie potrzeby), aby cały obraz był mieści się w obszarze, puste miejsca dodane do górne i dolne lub stronach w zależności od tego, czy obraz jest szerokość lub wysokość.

Obrazy mogą być ładowane z [pliku lokalnego](#Local_Images_in_Xaml), [osadzonego zasobu](#embedded_images), lub [pobrane](#Downloading_Images).

<a name="Local_Images" />

## <a name="local-images"></a>Lokalne obrazów

Pliki obrazów mogą być dodawane do każdego projektu aplikacji lub odwoływane za pośrednictwem kodu platformy Xamarin.Forms udostępnionych. Aby użyć jednego obrazu we wszystkich aplikacjach, *tej samej nazwy pliku musi być używany na każdej platformie*, i powinna być prawidłową Android nazwę zasobu (ie. dozwolone są tylko małe litery, cyfry, podkreślenia i okres).

- **iOS** — preferowany sposób zarządzania i obsługi obrazów, ponieważ jest użycie systemu iOS 9 **Ustawia obraz katalogu zasobów**, który powinien zawierać wszystkie wersje obrazu, które są niezbędne do obsługi różnych urządzeń i skalować czynniki aplikacja. Aby uzyskać więcej informacji, zobacz [Dodawanie obrazów do zasobu katalogu obrazu ustawić](~/ios/app-fundamentals/images-icons/displaying-an-image.md).
- **Android** -umieścić obrazy w **obiektów drawable/zasoby** katalogu z **Akcja kompilacji: AndroidResource**. Wysoki i niski DPI wersji obrazu można również podać (w nazwanego **zasobów** podkatalogów, takich jak **obiektów drawable ldpi**, **obiektów drawable hdpi**i **obiektów drawable xhdpi**).
- **Windows Phone** -umieścić obrazy w katalogu głównym aplikacji z **Akcja kompilacji: zawartości**.
- **Windows platformy Uniwersalnej** -umieścić obrazy w katalogu głównym aplikacji z **Akcja kompilacji: zawartości**.

> [!IMPORTANT]
> Przed iOS 9, obrazy, zwykle zostały umieszczone w **zasobów** folder o **Akcja kompilacji: BundleResource**. Jednak ta metoda pracy z obrazów w aplikacji systemu iOS została zastąpiona przez firmę Apple. Aby uzyskać więcej informacji, zobacz [rozmiary obrazów i plików](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Przestrzegać tych reguł do nazywania plików i umieszczania umożliwia skonfigurowanie następujących XAML do ładowania i wyświetlić obraz na wszystkich platformach:

```xaml
<Image Source="waterfront.jpg" />
```

Odpowiednik kodu C# jest następujący:

```csharp
var image = new Image { Source = "waterfront.jpg" };
```

Poniższe zrzuty ekranu pokazują wynik wyświetlanie lokalnego obrazu na każdej platformie:

[![Lokalne ImageSource](images-images/local-sml.png "Przykładowa aplikacja wyświetlania obrazu lokalnego")](images-images/local.png#lightbox "Przykładowa aplikacja wyświetlania obrazu lokalnego")

Aby uzyskać większą elastyczność `Device.RuntimePlatform` właściwości można wybrać inny plik obrazu lub ścieżka dla niektórych lub wszystkich platform, jak pokazano w tym przykładzie kodu:

```csharp
image.Source = Device.RuntimePlatform == Device.Android ? ImageSource.FromFile("waterfront.jpg") : ImageSource.FromFile("Images/waterfront.jpg");
```

> [!IMPORTANT]
> Aby użyć tej samej nazwy pliku obrazu na wszystkich platformach nazwa musi być prawidłową na wszystkich platformach. Android drawables ma ograniczenia nazewnictwa — są dozwolone tylko małe litery, cyfry, podkreślenia i okres — i zgodności między platformami to musi występować na innych platformach zbyt. Nazwa pliku przykład **waterfront.png** następujące reguły, ale przykłady nieprawidłowych nazw plików obejmują "wody front.png", "WaterFront.png", "wody front.png" i "wåterfront.png".

<a name="Native_Resolutions" />

### <a name="native-resolutions-retina-and-high-dpi"></a>Rozdzielczości (siatkówki i wysokiej rozdzielczości DPI)

iOS, Android, Windows Phone i platformy uniwersalnej systemu Windows obsługują rozwiązania innego obrazu, w którym system operacyjny wybierze odpowiednie obrazu w czasie wykonywania w oparciu o możliwości urządzenia. Ładowanie lokalnego obrazów, aby automatycznie program obsługuje rozwiązania alternatywnego, jeśli pliki są poprawnie o nazwie i znajduje się w projekcie platformy Xamarin.Forms używa wybranych platformach natywnych interfejsów API.

Preferowany sposób zarządzać obrazami, ponieważ system iOS 9 polega na przeciągnięciu obrazami dla każdego rozwiązania wymagane do odpowiednich zasobów katalogu obrazu zestawu. Aby uzyskać więcej informacji, zobacz [Dodawanie obrazów do zasobu katalogu obrazu ustawić](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Przed iOS 9, siatkówki wersje obrazu może być umieszczone w **zasobów** folderu - 2 i 3 razy rozpoznawania o **@2x** lub **@3x**sufiksy na nazwę pliku przed rozszerzeniem (np.) **myimage@2x.png**). Jednak ta metoda pracy z obrazów w aplikacji systemu iOS została zastąpiona przez firmę Apple. Aby uzyskać więcej informacji, zobacz [rozmiary obrazów i plików](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Obrazy systemu android rozwiązania alternatywne powinna zostać umieszczona w [specjalnie o nazwie katalogów](http://developer.android.com/guide/practices/screens_support.html) w projekcie systemu Android, jak pokazano na poniższym zrzucie ekranu:

[![Lokalizacja systemu android wielu rozdzielczość obrazu](images-images/xs-highdpisolution-sml.png "lokalizacji systemu Android wielu rozdzielczość obrazu")](images-images/xs-highdpisolution.png#lightbox "lokalizacji systemu Android wielu rozdzielczość obrazu")

Nazwy pliku obrazu platformy uniwersalnej systemu Windows i Windows Phone [kończyły się słowem `.scale-xxx` przed rozszerzeniem](https://docs.microsoft.com/windows/uwp/app-resources/images-tailored-for-scale-theme-contrast), gdzie `xxx` procent skalowania zastosować do zasobu, np. **myimage.scale-200.png**. Obrazy mogą być następnie przywoływane w kodzie lub XAML bez modyfikatora skali, np. po prostu **myimage.png**. Platforma wybierze oparte na bieżącej DPI wyświetlacza najbliższej skali odpowiednich zasobów.

### <a name="additional-controls-that-display-images"></a>Dodatkowe funkcje kontroli, które wyświetlanie obrazów

Niektóre formanty mają właściwości, które wyświetlania obrazu, takich jak:

- [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) -Wszelkie strony typu pochodzącego od `Page` ma [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Icon/) i [ `BackgroundImage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.BackgroundImage/) właściwości, które można przypisać odwołanie pliku lokalnego. W pewnych okolicznościach, takie jak kiedy [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) są wyświetlane [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/), jeśli jest to obsługiwane przez platformę będzie wyświetlana ikona.

  > [!IMPORTANT]
  > W systemach iOS [ `Page.Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Icon/) właściwości nie można wypełnić z obrazu w zestawie obrazu katalogu zasobów. Zamiast tego należy załadować obrazy ikon dla `Page.Icon` właściwość z **zasobów** folderu projektu systemu iOS.

- [`ToolbarItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/) -Ma [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Icon/) właściwość, która może być ustawiony na odwołanie do pliku lokalnego.
- [`ImageCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/) -Ma [ `ImageSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ImageCell.ImageSource/) pobrać właściwość, która może być ustawiony na obraz z pliku lokalnego, osadzonego zasobu lub identyfikator URI.

<a name="embedded_images" />

## <a name="embedded-images"></a>Obrazy osadzone

Obrazy osadzone również są dostarczane z aplikacją (np. obrazów lokalnego), ale zamiast kopiowania obrazu w każdej aplikacji struktury plików obrazu pliku jest osadzony w zestawie jako zasób. Ta metoda dystrybucji obrazów jest szczególnie przydatna do tworzenia składników, jako obraz jest umieszczany w pakietach z kodem.

Aby osadzić obraz w projekcie, kliknij prawym przyciskiem do dodawania nowych elementów, a następnie wybierz obraz/s, który chcesz dodać. Domyślnie obraz będzie mieć **Akcja kompilacji: Brak**; musi on być **Akcja kompilacji: EmbeddedResource**.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](images-images/vs-buildaction.png "Ustawić akcji kompilacji: EmbeddedResource")

**Akcja kompilacji** można wyświetlać i zmieniać w **właściwości** okna dla pliku.

W tym przykładzie jest identyfikator zasobu **WorkingWithImages.beach.jpg**.
IDE wygenerował to ustawienie domyślne, łącząc **Namespace domyślne** dla tego projektu z pliku za pomocą kropki (.) między każdą wartość.
<!-- https://msdn.microsoft.com/library/ms950960.aspx -->

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](images-images/xs-buildaction.png "Ustawić akcji kompilacji: EmbeddedResource")

**Akcja kompilacji** można również wyświetlać i zmieniać w **właściwości** konsoli dla pliku.
Ta konsola przedstawia **identyfikator zasobu** umożliwiające odwoływać się do zasobu w kodzie. Zrzut ekranu poniżej **identyfikator zasobu** jest **WorkingWithImages.beach.jpg**.
IDE wygenerował to ustawienie domyślne, łącząc **Namespace domyślne** dla tego projektu z pliku za pomocą kropki (.) między każdą wartość.
Ten identyfikator może być edytowany w **właściwości** konsoli, ale te przykłady wartość **WorkingWithImages.beach.jpg** będą używane.

![](images-images/xs-embeddedproperties.png "EmbeddedResource właściwości konsoli")

-----

Jeśli obrazy osadzone w folderach w ramach projektu, nazwy folderu są oddzielone kropką (.) z identyfikatorem zasobu. Przenoszenie **beach.jpg** obraz do folderu o nazwie **MyImages** spowodowałoby identyfikator zasobu **WorkingWithImages.MyImages.beach.jpg**

Kod, aby załadować osadzonego obrazu po prostu przekazuje **identyfikator zasobu** do [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) — metoda, jak pokazano poniżej:

```csharp
var embeddedImage = new Image { Source = ImageSource.FromResource("WorkingWithImages.beach.jpg") };
```

Obecnie nie istnieje niejawna konwersja identyfikatorów zasobów. Zamiast tego należy użyć [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) lub `new ResourceImageSource()` do ładowania obrazów osadzonych.

Poniższe zrzuty ekranu pokazują wynik wyświetlanie osadzony obraz na każdej platformie:

[![ResourceImageSource](images-images/resource-sml.png "przykładowej aplikacji wyświetlanie osadzony obraz")](images-images/resource.png#lightbox "przykładowej aplikacji Wyświetlanie osadzonego obrazu")

<a name="Embedded_Images_in_Xaml" />

### <a name="using-xaml"></a>Przy użyciu kodu XAML

Ponieważ nie istnieje żaden konwerter typów wbudowanych z `string` do `ResourceImageSource`, te typy obrazów nie można załadować natywnie przez XAML. Zamiast tego prostego niestandardowego rozszerzenia znaczników XAML mogą być zapisywane do ładowania obrazów za pomocą **identyfikator zasobu** określony w języku XAML:

```csharp
[ContentProperty (nameof(Source))]
public class ImageResourceExtension : IMarkupExtension
{
 public string Source { get; set; }

 public object ProvideValue (IServiceProvider serviceProvider)
 {
   if (Source == null)
   {
     return null;
   }
   
   // Do your translation lookup here, using whatever method you require
   var imageSource = ImageSource.FromResource(Source);

   return imageSource;
 }
}
```

Aby użyć tego rozszerzenia, Dodaj niestandardowy `xmlns` na języku XAML, za pomocą poprawnych wartości przestrzeni nazw i zestawu dla projektu. Źródło obrazu można następnie ustawić przy użyciu następującej składni: `{local:ImageResource WorkingWithImages.beach.jpg}`. Poniżej przedstawiono pełny przykład XAML:

```xaml
<?xml version="1.0" encoding="UTF-8" ?>
<ContentPage
   xmlns="http://xamarin.com/schemas/2014/forms"
   xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
   xmlns:local="clr-namespace:WorkingWithImages;assembly=WorkingWithImages"
   x:Class="WorkingWithImages.EmbeddedImagesXaml">
 <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
   <!-- use a custom Markup Extension -->
   <Image Source="{local:ImageResource WorkingWithImages.beach.jpg}" />
 </StackLayout>
</ContentPage>
```

### <a name="troubleshooting-embedded-images"></a>Rozwiązywanie problemów z obrazów osadzonych

<a name="Debugging_Embedded_Images" />

#### <a name="debugging-code"></a>Debugowanie kodu

Ponieważ czasami trudno jest zrozumienie, dlaczego nie został załadowany zasób określonego obrazu, poniższy kod debugowania można tymczasowo dodane do aplikacji, aby potwierdzić, że zasoby są poprawnie skonfigurowane. On dane wyjściowe obejmują wszystkie znanych zasobów osadzonych w danym zestawie do <span class="UIItem">konsoli</span> do debugowania problemów ładowania zasobów.

```csharp
using System.Reflection;
// ...
// NOTE: use for debugging, not in released app code!
var assembly = typeof(EmbeddedImages).GetTypeInfo().Assembly;
foreach (var res in assembly.GetManifestResourceNames())
{
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

#### <a name="images-embedded-in-other-projects-dont-appear"></a>Obrazy osadzone w innych projektów nie są wyświetlane.

`Image.FromResource` Wyszukuje tylko obrazy w tym samym zestawie co wywołanie kodu `FromResource`. Przy użyciu kodu debugowania powyżej, które można określić zestawów, które zawierają konkretnego zasobu, zmieniając `typeof()` instrukcji `Type` wiadomo w każdym zestawie.

<a name="Downloading_Images" />

## <a name="downloading-images"></a>Pobieranie obrazów

Można automatycznie pobierać obrazy do wyświetlenia, jak pokazano w poniższych XAML:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
       x:Class="WorkingWithImages.DownloadImagesXaml">
  <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
    <Label Text="Image UriSource Xaml" />
    <Image Source="https://xamarin.com/content/images/pages/forms/example-app.png" />
    <Label Text="example-app.png gets downloaded from xamarin.com" />
  </StackLayout>
</ContentPage>
```

Odpowiednik kodu C# jest następujący:

```csharp
var webImage = new Image { Source = ImageSource.FromUri(new Uri("https://xamarin.com/content/images/pages/forms/example-app.png")) };
```

[ `ImageSource.FromUri` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) Metoda wymaga `Uri` obiektu i zwraca nowy [ `UriImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/) które odczytuje z `Uri`.

Istnieje również niejawna konwersja ciągów identyfikatora URI, więc poniższy przykład również będzie działać:

```csharp
webImage.Source = "https://xamarin.com/content/images/pages/forms/example-app.png";
```

Poniższe zrzuty ekranu pokazują wynik wyświetlanie zdalnego obrazu na każdej platformie:

[![Pobrane ImageSource](images-images/download-sml.png "przykładowej aplikacji wyświetlanie pobranych obrazu")](images-images/download.png#lightbox "przykładowej aplikacji wyświetlanie pobranych obrazu")

<a name="Image_Caching" />

### <a name="downloaded-image-caching"></a>Buforowanie pobrany obraz

A [ `UriImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/) obsługuje również buforowanie pobrany obrazów, skonfigurować za pomocą następujących właściwości:

- [`CachingEnabled`](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.CachingEnabled/) — Czy włączone jest buforowanie (`true` domyślnie).
- [`CacheValidity`](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.CacheValidity/) -A `TimeSpan` określający, jak długo obrazu będzie przechowywany lokalnie.

Buforowanie jest włączone domyślnie i zapisze obraz lokalnie przez 24 godziny. Aby wyłączyć buforowanie dla określonego obrazu, wystąpienia źródło obrazu w następujący sposób:

```csharp
image.Source = new UriImageSource { CachingEnabled = false, Uri="http://server.com/image" };
```

Aby ustawić okres określonych pamięci podręcznej (na przykład 5 dni) wystąpienia źródło obrazu w następujący sposób:

```csharp
webImage.Source = new UriImageSource
{
    Uri = new Uri("https://xamarin.com/content/images/pages/forms/example-app.png"),
    CachingEnabled = true,
    CacheValidity = new TimeSpan(5,0,0,0)
};
```

Wbudowanemu buforowaniu ułatwia bardzo obsługi scenariuszy, takich jak przewijanie listy obrazów, w którym można ustawić (lub powiązać) obraz w każdej komórce i pozwól wbudowanych pamięci podręcznej zajmie się ponownie podczas ładowania obrazu, gdy komórka jest przewijane powrót do widoku.

<a name="Icons_and_splashscreens" />

## <a name="icons-and-splashscreens"></a>Ikon i ekranów powitalnych

Gdy nie dotyczą [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) widoku, aplikacji ikon i ekranów powitalnych są również zastosowanie obrazów w projektach platformy Xamarin.Forms.

Ustawienie ikon i ekranów powitalnych dla aplikacji platformy Xamarin.Forms odbywa się w poszczególnych projektach aplikacji. Oznacza to, że Generowanie poprawnie o rozmiarze obrazy dla systemu iOS, Android i platformy uniwersalnej systemu Windows. Tych obrazów, należy o nazwie i znajduje się zgodnie z wymaganiami każdej platformy.

## <a name="icons"></a>Ikony

Zobacz [iOS Praca z obrazami](~/ios/app-fundamentals/images-icons/index.md), [nadruków Google](http://developer.android.com/design/style/iconography.html), i [wytyczne dotyczące zasobów kafelków i ikona](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/) Aby uzyskać więcej informacji na temat tworzenia tych zasobów aplikacji.

## <a name="splashscreens"></a>Ekranów powitalnych

Tylko aplikacje systemów iOS i platformy uniwersalnej systemu Windows wymagają ekranu powitalnego (nazywanych również obrazu ekranu lub wartość domyślną uruchamiania).

Zapoznaj się z dokumentacją dotyczącą [iOS Praca z obrazami](~/ios/app-fundamentals/images-icons/index.md) i [powitalny ekrany](/windows/uwp/launch-resume/splash-screens/) w Centrum deweloperów systemu Windows.

## <a name="summary"></a>Podsumowanie

Platformy Xamarin.Forms oferuje kilka różnych sposobów obejmują obrazy w aplikacji i platform, umożliwiając dla tego samego obrazu do użycia na platformach lub obrazów specyficzne dla platformy, należy określić. Obrazy pobrany automatycznie są buforowane, automatyzacji typowy scenariusz kodowania.

Obrazów ikony, jak i ekranu powitalnego aplikacji są konfiguracji i skonfigurowane tak jak w przypadku aplikacji z systemem innym niż platformy Xamarin.Forms — wykonaj te same wskazówki używana dla specyficznego dla platformy aplikacji.


## <a name="related-links"></a>Linki pokrewne

- [WorkingWithImages (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithImages/)
- [iOS Praca z obrazami](~/ios/app-fundamentals/images-icons/index.md)
- [Nadruków systemu android](http://developer.android.com/design/style/iconography.html)
- [Wytyczne dotyczące zasobów kafelków i ikony](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/)
