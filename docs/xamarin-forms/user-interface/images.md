---
title: Obrazy w interfejsie Xamarin.Forms
description: Obrazy mogą być współużytkowane w różnych platformach za pomocą zestawu narzędzi Xamarin.Forms, może być załadowany specjalnie dla każdej platformy lub mogą być pobierane do wyświetlenia.
ms.prod: xamarin
ms.assetid: C025AB53-05CC-49BA-9815-75D6DF9E40B7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/15/2017
ms.openlocfilehash: f55a7878be898cbae5681d628d07cbe8598c9509
ms.sourcegitcommit: be4da0cd7e1a915e3b8932a7e3d6bcd74c7055be
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38986125"
---
# <a name="images-in-xamarinforms"></a>Obrazy w interfejsie Xamarin.Forms

_Obrazy mogą być współużytkowane w różnych platformach za pomocą zestawu narzędzi Xamarin.Forms, może być załadowany specjalnie dla każdej platformy lub mogą być pobierane do wyświetlenia._

Obrazy są kluczowym elementem nawigacji w aplikacji, użytecznością i znakowanie. Aplikacje Xamarin.Forms muszą mieć możliwość udostępniania obrazów na wszystkich platformach, ale również potencjalnie wyświetlania różnych obrazów na każdej platformie.

Specyficzne dla platformy obrazy są również wymagane ikon i ekrany powitalne; te muszą być skonfigurowane na poszczególnych platform.

W tym dokumencie omówiono następujące tematy:

- [ **Obrazów lokalnych** ](#Local_Images) — wyświetlanie obrazów są dostarczane z aplikacji, w tym rozwiązywanie rozdzielczości, takiej jak iOS (wyświetlacz retina), Android i platformy uniwersalnej systemu Windows o dużej rozdzielczości wersji obrazu.
- [ **Obrazy osadzone** ](#Embedded_Images) — wyświetlanie obrazów osadzony jako zasób zestawu.
- [ **Pobrano obrazów** ](#Downloading_Images) — pobieranie i wyświetlanie obrazów.
- [ **Ikony i splashscreens** ](#Icons_and_splashscreens) -ikony specyficznej dla platformy i obrazów uruchamiania.

## <a name="displaying-images"></a>Wyświetlanie obrazów

Korzysta z zestawu narzędzi Xamarin.Forms [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) widok, aby wyświetlić obrazy na stronie. Posiada dwie ważne właściwości:

- [`Source`](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Source/) - [ `ImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/) Wystąpienia, pliku, identyfikator Uri lub zasób, który ustawia obraz do wyświetlania.
- [`Aspect`](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Aspect/) -Porady rozmiar obrazu w granicach, jest wyświetlana w ramach (czy stretch, przycinanie lub letterbox).

[`ImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/) wystąpienia można uzyskać za pomocą metod statycznych dla każdego typu źródła obrazu:

- [`FromFile`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromFile/p/System.String/) — Wymagają nazwy pliku lub ścieżki pliku, która może zostać rozpoznana na każdej platformie.
- [`FromUri`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) — Na przykład wymagane w obiekcie Uri.  `new Uri("http://server.com/image.jpg")` .
- [`FromResource`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) — Wymagają identyfikatora zasobu, do pliku obrazu osadzonego w aplikacji lub projekt biblioteki .NET Standard z **akcji kompilacji: EmbeddedResource**.
- [`FromStream`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromStream/p/System.Func%7BSystem.IO.Stream%7D/) -Wymaga strumienia, który dostarcza dane obrazu.

[ `Aspect` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Aspect/) Właściwość określa, jak obraz, który będzie odpowiednio dopasowane obszaru wyświetlania:

- [`Fill`](xref:Xamarin.Forms.Aspect.Fill) -Obraz jest rozciągany tak aby całkowicie i dokładnie wypełnił obszar wyświetlania. Może to spowodować, że obraz jest zniekształcony.
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill) — Przycina obraz, aby wypełnił obszar wyświetlania przy jednoczesnym zachowaniu aspektu (tzn. nie zakłócania).
- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit) -Letterboxes obrazu (w razie potrzeby), aby cały obraz dopasowuje się do obszaru wyświetlania, za pomocą pustego miejsca dodane w górę/w dół lub stronach w zależności od tego, czy obraz jest szerokość lub wysokość.

Obrazy mogą być ładowane z [lokalnego pliku](#Local_Images_in_Xaml), [osadzony zasób](#embedded_images), lub [pobrane](#Downloading_Images).

<a name="Local_Images" />

## <a name="local-images"></a>Obrazów lokalnych

Pliki obrazów mogą być dodawane do każdego projektu aplikacji lub odwoływać się z zestawu narzędzi Xamarin.Forms udostępnionego kodu. Używać jednego obrazu dla wszystkich aplikacji *tej samej nazwy pliku musi być używana w firmach*, i powinna być prawidłową Android nazwę zasobu (tj. dozwolone są tylko małe litery, cyfry, podkreślenia i okres).

- **iOS** — preferowany sposób zarządzania i obsługi obrazów, ponieważ system iOS 9 jest użycie **zestawów obrazu wykazu zasobów**, który powinien zawierać wszystkie wersje obrazu, które są niezbędne do obsługi różnych urządzeń i skalowanie czynników aplikacja. Aby uzyskać więcej informacji, zobacz [Dodawanie obrazów do zasobu katalogu obrazu zestawie](~/ios/app-fundamentals/images-icons/displaying-an-image.md).
- **Android** — umieść obrazów w **zasobów/drawable** katalogu przy użyciu **Build Action: AndroidResource**. Wysoka i niska DPI wersji obrazu może również dostarczyć (w odpowiednio nazwanych **zasobów** podkatalogów, takie jak **drawable ldpi**, **drawable hdpi**i **drawable xhdpi**).
- **Windows platformy Uniwersalnej** — umieść obrazy w katalogu głównym aplikacji za pomocą **Build Action: zawartości**.

> [!IMPORTANT]
> Przed systemem iOS 9, obrazy, zwykle zostały umieszczone w **zasobów** folder z **Build Action: BundleResource**. Jednak ta metoda pracy z obrazów w aplikacji systemu iOS została zastąpiona przez firmę Apple. Aby uzyskać więcej informacji, zobacz [rozmiary obrazów i plików](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Dostosowanie się do tych reguł do nazywania plików i umieszczania umożliwia następujące XAML ładowania oraz wyświetlania obrazu na wszystkich platformach:

```xaml
<Image Source="waterfront.jpg" />
```

Równoważny kod C# jest następująca:

```csharp
var image = new Image { Source = "waterfront.jpg" };
```

Poniższych zrzutach ekranu przedstawiono wynik wyświetlanie lokalny obraz na każdej z platform:

[![Lokalne ImageSource](images-images/local-sml.png "przykładowej aplikacji wyświetlanie lokalny obraz")](images-images/local.png#lightbox "przykładowej aplikacji wyświetlanie lokalny obraz")

Aby uzyskać większą elastyczność `Device.RuntimePlatform` właściwości można wybrać inny plik obrazu lub ścieżkę dla niektórych lub wszystkich platform, jak pokazano w poniższym przykładzie kodu:

```csharp
image.Source = Device.RuntimePlatform == Device.Android ? ImageSource.FromFile("waterfront.jpg") : ImageSource.FromFile("Images/waterfront.jpg");
```

> [!IMPORTANT]
> Aby użyć tej samej nazwy pliku obrazu na wszystkich platformach nazwa musi być prawidłową na wszystkich platformach. Android drawables ma ograniczenia nazewnictwa — dozwolone są tylko małe litery, cyfry, podkreślenia i kropki — i dla zgodności międzyplatformowej to musi występować na innych platformach zbyt. Nazwa pliku przykład **waterfront.png** kieruje się regułami, ale przykłady nieprawidłowych nazw plików obejmują "water front.png", "WaterFront.png", "water front.png" i "wåterfront.png".

<a name="Native_Resolutions" />

### <a name="native-resolutions-retina-and-high-dpi"></a>Rozdzielczości ((wyświetlacz retina) i wysokiej rozdzielczości DPI)

dla systemu iOS, Android i platformy uniwersalnej systemu Windows obejmują wsparcie dla rozwiązania innego obrazu, w którym system operacyjny wybierze odpowiedni obraz w oparciu o możliwościach urządzenia w czasie wykonywania. Zestaw narzędzi Xamarin.Forms używa interfejsów API natywnych platform do ładowania obrazów lokalnych, aby automatycznie obsługuje ona rozwiązania alternatywnego, jeśli pliki są poprawnie o nazwie i znajduje się w projekcie.

Preferowanym sposobem zarządzania obrazami, ponieważ system iOS 9 jest aby przeciągnąć obrazy dla każdego rozwiązania, wymagane do zestawu obrazów katalogów odpowiednich zasobów. Aby uzyskać więcej informacji, zobacz [Dodawanie obrazów do zasobu katalogu obrazu zestawie](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Przed systemu iOS 9 (wyświetlacz retina) wersje obrazu można można umieścić w **zasobów** folder - 2 i 3 razy rozpoznawania o **@2x** lub **@3x**sufiksy na nazwę pliku, przed rozszerzeniem pliku (np.) **myimage@2x.png**). Jednak ta metoda pracy z obrazów w aplikacji systemu iOS została zastąpiona przez firmę Apple. Aby uzyskać więcej informacji, zobacz [rozmiary obrazów i plików](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Obrazy rozwiązania alternatywne dla systemu android będzie umieszczona w [specjalnie o silnych nazwach katalogów](http://developer.android.com/guide/practices/screens_support.html) w projekcie dla systemu Android, jak pokazano na poniższym zrzucie ekranu:

[![Lokalizacja obrazu systemu android rozpoznawania wielu](images-images/xs-highdpisolution-sml.png "lokalizacji obrazu systemu Android rozpoznawania wielu")](images-images/xs-highdpisolution.png#lightbox "lokalizacji obrazu rozpoznawania wielu systemu Android")

Nazwy plików obrazów platformy uniwersalnej systemu Windows [kończyły się słowem `.scale-xxx` przed rozszerzeniem pliku](https://docs.microsoft.com/windows/uwp/app-resources/images-tailored-for-scale-theme-contrast), gdzie `xxx` procent skalowanie zastosowany do zasobu, np. **myimage.scale 200.png**. Obrazy następnie można się odwoływać w kodzie lub XAML bez modyfikatora skali, np. po prostu **myimage.png**. Platforma wybierze najbliższej skalowania odpowiednich zasobów, oparte na bieżącej DPI wyświetlacza.

### <a name="additional-controls-that-display-images"></a>Dodatkowe formanty, które wyświetlanie obrazów

Niektóre kontrolki mają właściwości, które wyświetla obraz, takich jak:

- [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) -Wszelkie stronie Typ, który pochodzi od klasy `Page` ma [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Icon/) i [ `BackgroundImage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.BackgroundImage/) właściwości, które można przypisać odwołanie do pliku lokalnego. W pewnych okolicznościach, takie jak czas [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) Wyświetla [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/), jeśli jest obsługiwany przez platformę będzie wyświetlana ikona.

  > [!IMPORTANT]
  > W systemach iOS [ `Page.Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Icon/) właściwości nie można wypełnić z obrazu w zestawie zasobów katalogu obrazu. Zamiast tego należy załadować obrazy ikon dla `Page.Icon` właściwość **zasobów** folderu w projekcie dla systemu iOS.

- [`ToolbarItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/) — Ma [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Icon/) właściwość, która może być ustawiona na odwołanie do pliku lokalnego.
- [`ImageCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/) — Ma [ `ImageSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ImageCell.ImageSource/) właściwość, która może być ustawiona na obrazie pobierane z pliku lokalnego, zasobu osadzonego lub identyfikator URI.

<a name="embedded_images" />

## <a name="embedded-images"></a>Obrazy osadzone

Obrazy osadzone również są dostarczane z aplikacją (np. obrazów lokalnych), ale zamiast kopię obrazu w każdej aplikacji struktury plików obrazu pliku jest osadzony w zestawie jako zasób. Ta metoda dystrybucja obrazów jest szczególnie przydatna do tworzenia składników, jak obraz, który jest powiązany z kodem.

Osadzanie obrazu w projekcie, kliknij prawym przyciskiem myszy w celu dodawania nowych elementów, a następnie wybierz obraz/s, który chcesz dodać. Domyślnie ma obraz **Build Action: Brak**; to musi być równa **Build Action: EmbeddedResource**.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](images-images/vs-buildaction.png "Ustaw akcję kompilacji: EmbeddedResource")

**Build Action** można wyświetlać i zmieniać w **właściwości** okna dla pliku.

W tym przykładzie jest identyfikator zasobu **WorkingWithImages.beach.jpg**.
IDE wygenerował to ustawienie domyślne przez złączenie **Namespace domyślne** dla tego projektu przy użyciu nazwy pliku przy użyciu kropki (.) między każdą wartość.
<!-- https://msdn.microsoft.com/library/ms950960.aspx -->

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](images-images/xs-buildaction.png "Ustaw akcję kompilacji: EmbeddedResource")

**Akcja kompilacji** można również wyświetlać i zmieniać w **właściwości** konsoli do pliku.
Ten blok zawiera **identyfikator zasobu** służący do odwoływać się do zasobu w kodzie. Na poniższym zrzucie ekranu **identyfikator zasobu** jest **WorkingWithImages.beach.jpg**.
IDE wygenerował to ustawienie domyślne przez złączenie **Namespace domyślne** dla tego projektu przy użyciu nazwy pliku przy użyciu kropki (.) między każdą wartość.
Ten identyfikator może być edytowane w **właściwości** konsoli, ale w poniższych przykładach wartość **WorkingWithImages.beach.jpg** będą używane.

![](images-images/xs-embeddedproperties.png "Blok właściwości EmbeddedResource")

-----

Jeśli umieścisz obrazów osadzonych w folderach w ramach projektu, nazwy folderów są oddzielone kropką (.) identyfikator zasobu. Przenoszenie **beach.jpg** obrazów w folderze o nazwie **Mojeobrazy** mogłoby spowodować identyfikator zasobu **WorkingWithImages.MyImages.beach.jpg**

Kod, aby załadować osadzony obraz po prostu przekazuje **identyfikator zasobu** do [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) metody, jak pokazano poniżej:

```csharp
var embeddedImage = new Image { Source = ImageSource.FromResource("WorkingWithImages.beach.jpg", typeof(EmbeddedImages).GetTypeInfo().Assembly) };
```

> [!NOTE]
> Do obsługi wyświetlania obrazów osadzonych w trybie przygotowania do wydania na platformie Universal Windows, należy go Użyj przeciążenia `ImageSource.FromResource` , który określa zestaw źródłowy, w których należy szukać obrazu.

Obecnie nie istnieje niejawna konwersja identyfikatorów zasobów. Zamiast tego należy użyć [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) lub `new ResourceImageSource()` można załadować obrazów osadzonych.

Poniższych zrzutach ekranu przedstawiono wynik wyświetlanie osadzony obraz na każdej z platform:

[![ResourceImageSource](images-images/resource-sml.png "przykładowej aplikacji wyświetlanie osadzony obraz")](images-images/resource.png#lightbox "przykładowej aplikacji wyświetlanie osadzony obraz")

<a name="Embedded_Images_in_Xaml" />

### <a name="using-xaml"></a>Przy użyciu XAML

Ponieważ nie istnieje konwerter nie wbudowany typ z `string` do `ResourceImageSource`, tego rodzaju obrazów nie można załadować natywnie przez XAML. Zamiast tego prostego niestandardowego rozszerzenia znaczników XAML mogą być zapisywane do ładowania obrazów przy użyciu **identyfikator zasobu** określone w XAML:

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
   var imageSource = ImageSource.FromResource(Source, typeof(ImageResourceExtension).GetTypeInfo().Assembly);

   return imageSource;
 }
}
```

> [!NOTE]
> Do obsługi wyświetlania obrazów osadzonych w trybie przygotowania do wydania na platformie Universal Windows, należy go Użyj przeciążenia `ImageSource.FromResource` , który określa zestaw źródłowy, w których należy szukać obrazu.

Aby użyć tego rozszerzenia Dodaj niestandardowy `xmlns` do XAML, przy użyciu poprawnych wartości przestrzeni nazw i zestawu dla projektu. Źródło obrazu można następnie ustawić przy użyciu następującej składni: `{local:ImageResource WorkingWithImages.beach.jpg}`. Poniżej przedstawiono pełny przykład dla XAML:

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

Ponieważ czasami trudno jest zrozumieć, dlaczego nie został załadowany zasobu określonego obrazu, poniższy kod debugowania, można tymczasowo można dodać do aplikacji, aby pomóc, upewnij się, że zasoby są prawidłowo skonfigurowane. Generuje wszystkie znane zasoby osadzone w danym zestawie, który ma <span class="UIItem">konsoli</span> debugować problemy ładowania zasobów.

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

#### <a name="images-embedded-in-other-projects"></a>Obrazy osadzone w innych projektach

Domyślnie `ImageSource.FromResource` metoda wyszukuje tylko obrazy z tego samego zestawu co wywołanie metody kod `ImageSource.FromResource` metody. Przy użyciu debugowania kodu powyżej można określić zestawy, które zawierają określony zasób, zmieniając `typeof()` instrukcję, aby `Type` wiadomo, że są w każdym zestawie.

Jednak zestaw źródłowy wyszukane osadzonego obrazu można określić jako argument do `ImageSource.FromResource` metody:

```csharp
var imageSource = ImageSource.FromResource("filename.png", typeof(MyClass).GetTypeInfo().Assembly);
```

<a name="Downloading_Images" />

## <a name="downloading-images"></a>Pobieranie obrazów

Można automatycznie pobierać obrazy do wyświetlania, jak pokazano w poniższym XAML:

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

Równoważny kod C# jest następująca:

```csharp
var webImage = new Image { Source = ImageSource.FromUri(new Uri("https://xamarin.com/content/images/pages/forms/example-app.png")) };
```

[ `ImageSource.FromUri` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) Metoda wymaga `Uri` obiektu i zwraca nowy [ `UriImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/) która odczytuje z `Uri`.

Dostępna jest również niejawna konwersja ciągi identyfikatora URI, więc również sprawdzi się następująco:

```csharp
webImage.Source = "https://xamarin.com/content/images/pages/forms/example-app.png";
```

Poniższych zrzutach ekranu przedstawiono wynik wyświetlanie zdalnego obrazu na każdej z platform:

[![Pobrano ImageSource](images-images/download-sml.png "przykładowej aplikacji wyświetlanie pobrany obraz")](images-images/download.png#lightbox "przykładowej aplikacji wyświetlanie pobrany obraz")

<a name="Image_Caching" />

### <a name="downloaded-image-caching"></a>Buforowanie pobrany obraz

A [ `UriImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/) obsługuje również buforowania pobrane zdjęcia, skonfigurować za pomocą następujących właściwości:

- [`CachingEnabled`](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.CachingEnabled/) -Czy włączone jest buforowanie (`true` domyślnie).
- [`CacheValidity`](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.CacheValidity/) -A `TimeSpan` definiujący, jak długo obraz będą przechowywane lokalnie.

Buforowanie jest domyślnie włączona i będzie przechowywać obraz lokalnie przez 24 godziny. Aby wyłączyć buforowanie dla określonego obrazu, wystąpienia źródło obrazu w następujący sposób:

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

Buforowanie wbudowanych ułatwia bardzo do obsługi scenariuszy, takich jak przewijając listę obrazów, w którym można ustawić (lub powiązać) obrazu w każdej komórce i umożliwić wbudowaną pamięć podręczną, zajmie się ponownie ładuje obraz, gdy komórka jest przewijane powrót do widoku.

<a name="Icons_and_splashscreens" />

## <a name="icons-and-splashscreens"></a>Ikony i splashscreens

Gdy nie dotyczą [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) widoku, ikony aplikacji i splashscreens są również istotne użycie obrazów w projektach zestawu narzędzi Xamarin.Forms.

Ustawienie ikon i splashscreens dla aplikacji platformy Xamarin.Forms odbywa się we wszystkich projektach aplikacji. Oznacza to, że poprawnie generowania rozmiar obrazów dla systemów iOS, Android i platformy uniwersalnej systemu Windows. Te obrazy należy o nazwie i znajduje się zgodnie z wymaganiami dotyczącymi poszczególnych platform.

## <a name="icons"></a>Ikony

Zobacz [iOS Praca z obrazami](~/ios/app-fundamentals/images-icons/index.md), [nadruków Google](http://developer.android.com/design/style/iconography.html), i [wytyczne dotyczące zasobów kafelka i ikona](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/) Aby uzyskać więcej informacji dotyczących tworzenia tych zasobów aplikacji.

## <a name="splashscreens"></a>Splashscreens

Tylko aplikacje systemów iOS i platformy uniwersalnej systemu Windows wymagają ekranu powitalnego (nazywane również obraz ekranu lub domyślnego uruchamiania).

Zapoznaj się z dokumentacją dla [iOS Praca z obrazami](~/ios/app-fundamentals/images-icons/index.md) i [powitalny ekrany](/windows/uwp/launch-resume/splash-screens/) w Centrum deweloperów Windows.

## <a name="summary"></a>Podsumowanie

Zestaw narzędzi Xamarin.Forms oferuje wiele różnych sposobów, aby uwzględnić obrazów w aplikacji dla wielu platform, dzięki czemu ten sam obraz, który ma być używany na platformach lub obrazy specyficzne dla platformy, należy określić. Pobrane zdjęcia również automatycznie są buforowane, automatyzacja typowy scenariusz kodowania.

Obrazy ikon i ekranu powitalnego aplikacji są konfiguracji i skonfigurowane, jak w przypadku aplikacji innych niż zestawu narzędzi Xamarin.Forms — wykonaj te same wskazówki, które są używane dla określonych platform aplikacji.

## <a name="related-links"></a>Linki pokrewne

- [WorkingWithImages (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithImages/)
- [iOS Praca z obrazami](~/ios/app-fundamentals/images-icons/index.md)
- [Nadruków dla systemu android](http://developer.android.com/design/style/iconography.html)
- [Wytyczne dotyczące zasobów kafelka i ikony](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/)
