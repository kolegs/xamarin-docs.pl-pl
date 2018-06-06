---
title: Lokalizacja w Xamarin.iOS
description: Tego dokumentu opisano funkcje lokalizacji systemu iOS i sposób używania tych funkcji w aplikacji platformy Xamarin.iOS. Zawarto informacje języka, ustawienia regionalne, ciągi pliki i obrazy uruchamiania.
ms.prod: xamarin
ms.assetid: DFD9EB4A-E536-18E4-C8FD-679BA9C836D8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/28/2017
ms.openlocfilehash: 06758fd8fac62a63c309b173738a8ee889716143
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785269"
---
# <a name="localization-in-xamarinios"></a>Lokalizacja w Xamarin.iOS

_W tym dokumencie opisano funkcje lokalizacji systemu IOS SDK oraz sposób uzyskiwać do nich dostęp za pomocą platformy Xamarin._

Zapoznaj się [kodowania internacjonalizacji](encodings.md) instrukcje w tym zestawów znaków/strony kodowe w aplikacjach, które musi przetworzyć danych z systemem innym niż Unicode.

## <a name="ios-platform-features"></a>Funkcje platformy systemu iOS

W tej sekcji opisano niektóre z funkcji lokalizacji w systemie iOS. Przejdź do [następnej sekcji](#basics) określonego kodu i przykłady.

### <a name="language"></a>Język

Użytkownicy wybierają język w **ustawienia** aplikacji. To ustawienie ma wpływ na ciągi języka i obrazów wyświetlanych przez system operacyjny i aplikacje. 

Można ustalić języka używany w aplikacji, należy uzyskać pierwszy element `NSBundle.MainBundle.PreferredLocalizations`:

```csharp
var lang = NSBundle.MainBundle.PreferredLocalizations[0];
```

Ta wartość będzie kod języka takich jak `en` dla języka angielskiego, `es` dla języka hiszpańskiego, `ja` w języku japońskim, itp. Wartość zwracana jest ograniczone do jednego lokalizacje obsługiwane przez aplikację (przy użyciu reguł rezerwowy ustalić optymalne).

Kod aplikacji nie zawsze jest konieczne do sprawdzenia, czy ta wartość — Xamarin i iOS zarówno zapewniają funkcje, które ułatwiają zapewnienie automatycznie poprawny ciąg lub zasobów dla języka użytkownika. Te funkcje są opisane w dalszej części tego dokumentu.

> [!NOTE]
> Użyj `NSLocale.PreferredLanguages` ustalenie Preferencje językowe użytkownika, niezależnie od lokalizacje obsługiwanych przez aplikację. Wartości zwracane przez tę metodę zmienione w systemie iOS 9. zobacz [TN2418 Uwaga techniczna](https://developer.apple.com/library/content/technotes/tn2418/_index.html) szczegółowe informacje.

### <a name="locale"></a>Regionalne

Użytkownicy będą mogli wybrać ich ustawień regionalnych w **ustawienia** aplikacji. To ustawienie ma wpływ na sposób, w formacie daty, godziny, liczby i waluty.

Umożliwia to użytkownikom, wybierz, czy zobaczy formaty czasu 12-godzinnym lub 24-godzinnym, czy ich separatorem dziesiętnym jest przecinek lub punkt i kolejność dzień, miesiąc i rok w wyświetlanie daty.

Za pomocą platformy Xamarin, musisz mieć dostęp do obu Apple iOS klasy (`NSNumberFormatter`) oraz klas .NET w System.Globalization. Deweloperzy należy ocenić, która jest lepiej dostosowane do potrzeb, są różne funkcje dostępne w każdym. W szczególności pobieranie, wyświetlanie ceny zakupu w aplikacji przy użyciu StoreKit należy używać klas formatowania firmy Apple dla zwrócone informacje o cenach.

Bieżące ustawienia regionalne mogą być przeszukiwane przy użyciu jednej z dwóch sposobów:

- `NSLocale.CurrentLocale.LocaleIdentifier`
- `NSLocale.AutoUpdatingCurrentLocale.LocaleIdentifier`

Pierwsza wartość mogą być buforowane przez system operacyjny i dlatego nie zawsze uwzględnia obecnie wybranego ustawienia regionalne użytkownika. Druga wartość umożliwia uzyskanie aktualnie wybranego ustawienia regionalne.

### <a name="nscurrentlocaledidchangenotification"></a>NSCurrentLocaleDidChangeNotification

generuje iOS `NSCurrentLocaleDidChangeNotification` po aktualizacji ich ustawień regionalnych przez użytkownika. Aplikacji można nasłuchiwać dla tego powiadomienia, gdy są uruchomione i można wprowadzić odpowiednie zmiany do interfejsu użytkownika.

<a name="basics" />

## <a name="localization-basics-in-ios"></a>Podstawowe informacje o lokalizacji w systemie iOS

Następujące funkcje systemu IOS można łatwo wykorzystać w program Xamarin, aby zapewnić zlokalizowanych zasobów do wyświetlenia dla użytkownika. Zapoznaj się [próbki TaskyL10n](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) na temat sposobu wykonania tych sposobów.

### <a name="specifying-default-and-supported-languages-in-infoplist"></a>Określanie domyślnego i obsługiwanych języków w pliku Info.plist

W [techniczne Q & A QA1828: jak iOS określa język dla aplikacji platformy](https://developer.apple.com/library/content/qa/qa1828/_index.html), Apple opisano, jak iOS wybiera język do użycia w aplikacji. Język, który jest wyświetlany wpływ na następujące czynniki:

- Języki preferowana przez użytkownika (w **ustawienia** aplikacji)
- Lokalizacje powiązane z aplikacją (.lproj foldery)
- `CFBundleDevelopmentRegion` (**Info.plist** wartość określającą język domyślny dla aplikacji)
- `CFBundleLocalizations` (**Info.plist** tablicy określającym wszystkie obsługiwane lokalizacje)

Jak wskazano w techniczne Q & A, `CFBundleDevelopmentRegion` reprezentuje domyślny region i język aplikacji. Jeśli aplikacja jawnie nie obsługuje języki preferowanym przez użytkownika, zostanie użyty język określony według tego pola. 

> [!IMPORTANT]
> iOS 11 stosuje ten mechanizm wyboru języka bardziej ściśle nie było w poprzednich wersjach systemu operacyjnego. W związku z tym dowolną aplikację systemu iOS 11, który nie deklaruje jawnie obsługiwane lokalizacje — łącznie z folderami .lproj lub ustawienie wartości dla `CFBundleLocalizations` — mogą być wyświetlane innego języka w systemie iOS 11 niż w systemie iOS 10.

Jeśli `CFBundleDevelopmentRegion` nie została określona w **Info.plist** pliku narzędzia kompilacji Xamarin.iOS obecnie używana wartość domyślna `en_US`. Gdy to mogą ulec zmianie w przyszłych wydaniach, oznacza to, że język domyślny jest angielskiej wersji językowej.

Aby upewnić się, że aplikacja wybiera oczekiwany język, należy wykonać następujące czynności:

- Określ domyślny język. Otwórz **Info.plist** i użyj **źródła** widok, aby ustawić wartość `CFBundleDevelopmentRegion` klucza; w pliku XML, powinien być podobny do następującego:

```xml
<key>CFBundleDevelopmentRegion</key>
<string>es</string>
```

W tym przykładzie użyto "es", aby określić, że gdy brak użytkownika preferowane języki są obsługiwane, domyślny język hiszpański.

- Należy zadeklarować wszystkie obsługiwane lokalizacje. W **Info.plist**, użyj **źródła** widok, aby ustawić tablicę `CFBundleLocalizations` klucza; w pliku XML, powinien być podobny do następującego:

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>es</string>
    ...
</array>
```

Aplikacji platformy Xamarin.iOS, które zostały zlokalizowane za pomocą mechanizmów .NET, takie jak pliki .resx podać te **Info.plist** oraz wartości.

Aby uzyskać więcej informacji o tych **Info.plist** kluczy, Przyjrzyjmy się firmy Apple [informacji właściwości listy z informacjami o kluczach](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html).

### <a name="localizedstring-method"></a>LocalizedString — metoda

`NSBundle.MainBundle.LocalizedString` Metody wyszukuje zlokalizowany tekst przechowywanych w **.strings** pliki w projekcie. Te pliki są zorganizowane według języka, w specjalnej katalogi z **.lproj** sufiks.

#### <a name="strings-file-locations"></a>Lokalizacje plików .strings

- **Base.lproj** jest katalog, który zawiera zasoby dla języka domyślnego.
  Często znajduje się w katalogu głównym projektu (ale mogą także zostać umieszczone w **zasobów** folderu).
- **<language>.lproj** katalogów utworzonych dla każdej z obsługiwanych języków, zazwyczaj w **zasobów** folderu.

Może istnieć szereg różnych **.strings** plików w każdym katalogu języka:

- **Localizable.Strings** — główny listę zlokalizowany tekst.
- **InfoPlist.strings** — niektóre określone klucze są dozwolone w tym pliku można przetłumaczyć elementów, takich jak nazwa aplikacji.
- **< nazwa scenorysu > .strings** — opcjonalny plik, który zawiera tłumaczenia elementów interfejsu użytkownika w scenorysu.

**Akcja kompilacji** tych plików powinna być **zasobów pakietu**.

#### <a name="strings-file-format"></a>format pliku .strings

Składnia zlokalizowany ciąg wartości to:

```console
/* comment */
"key"="localized-value";
```

Należy wprowadzić następujące znaków w ciągach:

* `\"`  oferty
* `\\`  ukośnik odwrotny
* `\n`  Nowy wiersz

To jest przykład **es/Localizable.strings** (tj. Plik hiszpański) z próbki:

```console
"<new task>" = "<new task>";
"Task Details" = "Detalles de la tarea";
"Name" = "Nombre";
"task name" = "nombre de la tarea";
"Notes" = "Notas";
"other task info"= "otra información de tarea";
"Done" = "Completo";
"Save" = "Guardar";
"Delete" = "Eliminar";
```

### <a name="images"></a>Obrazy

Aby zlokalizować obrazu w systemie iOS:

1. Na przykład można skorzystać z obrazu w kodzie:

  ```csharp
  UIImage.FromBundle("flag");
  ```

2. Umieść plik obrazu domyślnego **flag.png** w **Base.lproj** (katalog natywnych języka).

3. Opcjonalnie umieścić zlokalizowane wersje obrazu w **.lproj** folderów dla każdego języka (np.) **es.lproj**, **ja.lproj**). Użyj tej samej nazwy pliku **flag.png** w każdym katalogu języka.

Jeśli obraz nie jest obecny w określonym języku, iOS rezerwowe w domyślnym folderze językiem macierzystym i załadować obrazu z tej lokalizacji.


#### <a name="launch-images"></a>Uruchamianie obrazów

Użyj standardowej konwencji nazewnictwa dla obrazów uruchamiania (i XIB lub scenorysu dla modeli telefonów iPhone 6) podczas umieszczania ich w **.lproj** katalogów dla każdego języka.

```console
Default.png
Default@2x.png
Default-568h@2x.png
LaunchScreen.xib
```

### <a name="app-name"></a>Nazwa aplikacji

Wprowadzenie do **InfoPlist.strings** w pliku **.lproj** katalogu pozwala na zastępowanie niektórych wartości w aplikacji **Info.plist**, łącznie z nazwą aplikacji:

```console
"CFBundleDisplayName" = "LeónTodo";
```

Inne klucze, które służy do [localize — ciągi specyficzne dla aplikacji](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW21) są:

- CFBundleName
- CFBundleShortVersionString
- NSHumanReadableCopyright

### <a name="dates-and-times"></a>Daty i godziny

Mimo że można użyć wbudowanych funkcji daty i godziny .NET (wraz z bieżącej `CultureInfo`) do formatowania daty i godziny dla ustawień regionalnych, to czy ignorowanie ustawień regionalnych użytkownika ustawień (które można skonfigurować niezależnie od języka).

Użyj systemu iOS `NSDateFormatter` do generowania danych wyjściowych preferencjom ustawienia regionalne użytkownika. Następujący przykładowy kod przedstawia podstawowe datę i godzinę opcje formatowania:

```csharp
var date = NSDate.Now;
var df = new NSDateFormatter ();
df.DateStyle = NSDateFormatterStyle.Full;
df.TimeStyle = NSDateFormatterStyle.Long;
Debug.WriteLine ("Full,Long: " + df.StringFor(date));
df.DateStyle = NSDateFormatterStyle.Short;
df.TimeStyle = NSDateFormatterStyle.Short;
Debug.WriteLine ("Short,Short: " + df.StringFor(date));
df.DateStyle = NSDateFormatterStyle.Medium;
df.TimeStyle = NSDateFormatterStyle.None;
Debug.WriteLine ("Medium,None: " + df.StringFor(date));
```

Wyniki dla języka angielskiego na terenie Stanów Zjednoczonych:

```console
Full,Long: Friday, August 7, 2015 at 10:29:32 AM PDT
Short,Short: 8/7/15, 10:29 AM
Medium,None: Aug 7, 2015
```

Wyniki dla języka hiszpańskiego w Hiszpanii:

```console
Full,Long: viernes, 7 de agosto de 2015, 10:26:58 GMT-7
Short,Short: 7/8/15 10:26
Medium,None: 7/8/2015
```

Odnoszą się do firmy Apple [elementy formatujące datę](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html) dokumentacji, aby uzyskać więcej informacji. Podczas testowania formatowania zależne od ustawień regionalnych daty i godziny, sprawdź zarówno **iPhone języka** i **Region** ustawienia.

<a name="rtl" />

### <a name="right-to-left-rtl-layout"></a>Układ od prawej do lewej (od prawej do lewej)

iOS udostępnia wiele funkcji, aby pomóc w tworzeniu aplikacji obsługujących od prawej do lewej:

* Użyj automatycznego układu `leading` i `trailing` atrybuty dla formantu Ustawienie osiowe, (który odpowiada lewy i prawy dla języka angielskiego, ale została odwrócona dla języków RTL).
  [ `UIStackView` ](~/ios/user-interface/controls/uistackview.md) Kontroli jest szczególnie przydatne podczas rozmieszczania formantów pod uwagę od prawej do lewej.
* Użyj `TextAlignment = UITextAlignment.Natural` dla wyrównania tekstu, (która zostanie pozostawiony w przypadku większości języków, ale prawo od prawej do lewej).
* `UINavigationController` Przerzuca przycisku Wstecz i automatycznie zmieni kierunek Przejdź.

Pokaż poniższe zrzuty ekranu [zlokalizowanych próbki Tasky](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) arabski i hebrajski (chociaż angielski został wprowadzony w polach):

[![](images/rtl-ar-sml.png "Lokalizacja w arabski")](images/rtl-ar.png#lightbox "Arabic") 

[![](images/rtl-he-sml.png "Lokalizacja (wersja hebrajska)")](images/rtl-he.png#lightbox "Hebrew")

iOS automatycznie cofa `UINavigationController`, i inne formanty są umieszczone wewnątrz `UIStackView` lub wyrównane z automatycznym układem.
Tekst od prawej do lewej jest zlokalizowana przy użyciu **.strings** pliki w taki sam sposób jak tekstu od lewej do prawej.

<a name="code"/>

## <a name="localizing-the-ui-in-code"></a>Lokalizowanie interfejsu użytkownika w kodzie

[Tasky (zlokalizowany w kodzie)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) przykładowych pokazano, jak do zlokalizowania aplikacji gdy interfejs użytkownika korzysta z wbudowanej w code (zamiast XIBs lub scenorys).

### <a name="project-structure"></a>Struktury projektu

![](images/solution-code.png "Drzewo zasobów")

### <a name="localizablestrings-file"></a>Plik Localizable.Strings

Jak opisano powyżej, **Localizable.strings** format pliku składa się z pary klucz wartość. Klucz opisuje zamierzone ciągu i wartość jest przetłumaczony tekst do użycia w aplikacji.

Hiszpański (**es**) poniżej przedstawiono lokalizacje przykładowej:

```console
"<new task>" = "<new task>";
"Task Details" = "Detalles de la tarea";
"Name" = "Nombre";
"task name" = "nombre de la tarea";
"Notes" = "Notas";
"other task info"= "otra información de tarea";
"Done" = "Completo";
"Save" = "Guardar";
"Delete" = "Eliminar";
```

### <a name="performing-the-localization"></a>Wykonywanie lokalizacji

W kodzie aplikacji wszędzie tam, gdzie tekst wyświetlany interfejs użytkownika jest ustawiona (czy jest tekst etykiety, lub symbol zastępczy danych wejściowych, itp.) w kodzie za pomocą systemu iOS `LocalizedString` funkcji, aby pobrać poprawne tłumaczenie do wyświetlenia:

```csharp
var localizedString = NSBundle.MainBundle.LocalizedString ("key", "optional");
someControl.Text = localizedString;
```

<a name="storyboard"/>

## <a name="localizing-storyboard-uis"></a>Lokalizowanie scenorysu UI

Przykład [Tasky (zlokalizowanego scenorysu)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard) pokazano, jak do zlokalizowania tekstu dla kontrolek w scenorysu.

### <a name="project-structure"></a>Struktury projektu

**Base.lproj** katalogu zawiera scenorysu, a powinien również zawierać wszystkie obrazy używane w aplikacji.

Zawierać innych katalogów języka **Localizable.strings** pliku dla wszystkich zasobów ciągu, do którego odwołuje się kod, a także **MainStoryboard.strings** plik zawierający tłumaczeń tekstu w scenorysu.

![](images/solution-storyboard.png "Drzewo zasobów")

Katalogi języka powinna zawierać żadnych obrazów, które zostały zlokalizowane, do zastąpienia w jednej kopii **Base.lproj**.

### <a name="object-id--localization-id"></a>Obiekt o identyfikatorze / identyfikator lokalizacji

Podczas tworzenia i edycji kontrolek w scenorysu, wybierz każdego formantu i nazwę do użycia na potrzeby lokalizacji:

* W programie Visual Studio dla komputerów Mac znajduje się w **konsoli właściwości** i nosi nazwę **identyfikator lokalizacji**.
* w programie Xcode, jest nazywany **obiektu o identyfikatorze**.

Ta wartość ciągu jest to spowodowane formularza, takie jak "NF3-h8-xmR", jak pokazano na poniższym zrzucie ekranu:

![](images/xs-designer-localization-id.png "Widok Xcode scenorysu lokalizacji")

Ta wartość jest używana w **.strings** plik można automatycznie przypisać przetłumaczony tekst do każdego formantu.

### <a name="mainstoryboardstrings"></a>MainStoryboard.strings

Format pliku tłumaczenia scenorysu przypomina **Localizable.strings** pliku, z wyjątkiem tego, że klucz (wartość po lewej stronie) nie może być zdefiniowana przez użytkownika, ale zamiast tego musi mieć dokładnie format: `ObjectID.property`.

W przykładzie **Mainstoryboard.strings** poniżej widać `UITextField`s ma `placeholder` właściwości tekstu, który może być lokalizowany; `UILabel`ma s `text` właściwości; i `UIButton`s domyślny tekst jest ustawiany za pomocą `normalTitle`:

```console
"SXg-TT-IwM.placeholder" = "nombre de la tarea";
"Pqa-aa-ury.placeholder"= "otra información de tarea";
"zwR-D9-hM1.text" = "Detalles de la tarea";
"bAM-2j-Rzw.text" = "Notas";           /* Notes */
"NF3-h8-xmR.text" = "Completo";        /* Done */
"MWt-Ya-pMf.normalTitle" = "Guardar";  /* Save */
"IGr-pR-05L.normalTitle" = "Eliminar"; /* Delete */
```

> [!IMPORTANT]
> Korzystanie z klasami rozmiar scenorysu może skutkować tłumaczenia, które nie są wyświetlane w aplikacji. [Informacje o wersji Xcode firmy Apple](https://developer.apple.com/library/content/releasenotes/DeveloperTools/RN-Xcode/Chapters/Introduction.html) wskazują, że storyboard lub XIB będzie lokalizować poprawnie, jeśli spełnione są trzy elementy: używa klas wielkości, Lokalizacja podstawowa i docelowa kompilacji są ustawione na uniwersalny i kompilacji jest przeznaczony dla systemu iOS 7.0. Poprawka ma zduplikowane pliku ciągi scenorysu na dwa pliki o identycznych: **MainStoryboard~iphone.strings** i **MainStoryboard~ipad.strings**, jak pokazano na poniższym zrzucie ekranu:
> 
> ![](images/xs-dup-strings.png "Ciągi, pliki")

<a name="appstore" />

## <a name="app-store-listing"></a>Lista sklepu z aplikacjami

Następuje firmy Apple — często zadawane pytania [lokalizacja magazynu App](https://itunespartner.apple.com/en/apps/faq/App%20Store_Localization) wprowadzenia tłumaczenia dla każdego kraju aplikacji znajduje się w sprzedaży. Należy pamiętać, ich ostrzeżenie, że tłumaczeń będą wyświetlane tylko, jeśli aplikacja zawiera również zlokalizowanych **.lproj** katalogu dla języka.

## <a name="summary"></a>Podsumowanie

W tym artykule opisano podstawy lokalizowania aplikacji systemu iOS przy użyciu zasobów wbudowane funkcje obsługi i scenorysu.

Aby dowiedzieć się więcej o i18n i L10n dla aplikacji systemu iOS, Android i wielu platform (w tym platformy Xamarin.Forms) w [tego podręcznika i platform](~/cross-platform/app-fundamentals/localization.md).

## <a name="related-links"></a>Linki pokrewne

- [Tasky (zlokalizowany w kodzie) (przykład)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [Tasky (zlokalizowanego scenorysu) (przykład)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard)
- [Przewodnik po lokalizacji firmy Apple](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)
- [Informacje o lokalizacji i Platform](~/cross-platform/app-fundamentals/localization.md)
- [Lokalizacja platformy Xamarin.Forms](~/xamarin-forms/app-fundamentals/localization/index.md)
- [Lokalizacja systemu android](~/android/app-fundamentals/localization.md)
