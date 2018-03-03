---
title: Lokalizacja systemu iOS
description: "W tym dokumencie opisano funkcje lokalizacji systemu IOS SDK oraz sposób uzyskiwać do nich dostęp za pomocą platformy Xamarin."
ms.topic: article
ms.prod: xamarin
ms.assetid: DFD9EB4A-E536-18E4-C8FD-679BA9C836D8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/28/2017
ms.openlocfilehash: ea91dbcf7148651cb5d10acae4ada8bb6758c39e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="ios-localization"></a>Lokalizacja systemu iOS

_W tym dokumencie opisano funkcje lokalizacji systemu IOS SDK oraz sposób uzyskiwać do nich dostęp za pomocą platformy Xamarin._

Zapoznaj się [kodowania internacjonalizacji](encodings.md) instrukcje w tym zestawów znaków/strony kodowe w aplikacjach, które musi przetworzyć danych z systemem innym niż Unicode.

## <a name="ios-platform-features"></a>Funkcje platformy systemu iOS

W tej sekcji opisano niektóre z funkcji lokalizacji w systemie iOS. Przejdź do [następnej sekcji](#basics) określonego kodu i przykłady.

### <a name="language"></a>Język

Użytkownicy wybierają język w **ustawienia** aplikacji. To ustawienie ma wpływ na ciągi języka i obrazów wyświetlanych przez system operacyjny, a także aplikacje, które wykrywa ustawienia języka.

Jest to, gdzie użytkownicy podejmie decyzję, czy chcą zobaczyć angielski, hiszpański, japoński, francuski lub innych języka wyświetlanego w swoich aplikacjach.

Rzeczywista lista języków obsługiwanych w systemie iOS 7 jest: angielski (USA), angielski (Zjednoczone Królestwo), chiński (uproszczony), chiński (tradycyjny), francuski, niemiecki, włoski, japoński, koreański, hiszpański, arabskiego, katalońskim, chorwacki, czeski, duński, holenderski, fiński, grecki, hebrajski, Węgierski, indonezyjski, malajski, norweski, Polski, portugalski, portugalski (Brazylia), rumuński, rosyjski, słowacki, szwedzki, tajski, turecki, ukraiński, wietnamski, język angielski (Australian), hiszpański (Meksyk).

Bieżący język może być badana uzyskując dostęp do pierwszego elementu obiektu `PreferredLanguages` tablicy:

```csharp
var lang = NSBundle.MainBundle.PreferredLocalizations[0];
```

Ta wartość będzie kod języka takich jak `en` dla języka angielskiego, `es` dla języka hiszpańskiego, `ja` w języku japońskim, itp. _Wartość zwracana jest ograniczone do jednego lokalizacje obsługiwane przez aplikację (przy użyciu reguł rezerwowy ustalić optymalne)._

Kod aplikacji nie zawsze jest konieczne do sprawdzenia, czy ta wartość - Xamarin i iOS zarówno zapewniają funkcje, które ułatwiają zapewnienie automatycznie poprawny ciąg lub zasobów dla języka użytkownika. Te funkcje są opisane w dalszej części tego dokumentu.

> [!NOTE]
> **Uwaga:** starszych niż iOS 9, zalecane kodu była `var lang = NSLocale.PreferredLanguages [0];`.
>
> Zobacz wyników zwróconych przez ten kod został zmieniony w systemie iOS 9 - [TN2418 Uwaga techniczna](https://developer.apple.com/library/content/technotes/tn2418/_index.html) Aby uzyskać więcej informacji.
>
> Można nadal używać `NSLocale.PreferredLanguages [0]` do określenia wartości rzeczywistej wybrane przez użytkownika (niezależnie od lokalizacje aplikacji obsługuje).

### <a name="locale"></a>Regionalne

Użytkownicy będą mogli wybrać ich ustawień regionalnych w **ustawienia** aplikacji. To ustawienie ma wpływ na sposób, w formacie daty, godziny, liczby i waluty.

Umożliwia to użytkownikom, wybierz czy zobaczy formaty czasu 12 lub 24-godzinnym, czy ich separatorem dziesiętnym jest przecinek lub punkt i kolejność dzień, miesiąc i rok w wyświetlanie daty.

Za pomocą platformy Xamarin, musisz mieć dostęp do obu Apple iOS klasy (`NSNumberFormatter`) oraz klas .NET w System.Globalization. Deweloperzy należy ocenić, która jest lepiej dostosowane do potrzeb, są różne funkcje dostępne w każdym. W szczególności pobieranie, wyświetlanie cen w zakupu aplikacji przy użyciu StoreKit ostatecznie powinny używać klas formatowania firmy Apple dla zwrócone informacje o cenach.

Bieżące ustawienia regionalne mogą być przeszukiwane przez:

- `NSLocale.CurrentLocale.LocaleIdentifier`
- `NSLocale.AutoUpdatingCurrentLocale.LocaleIdentifier`

Pierwsza wartość mogą być buforowane przez system operacyjny i dlatego nie zawsze uwzględnia obecnie wybranego ustawienia regionalne użytkownika. Druga wartość umożliwia uzyskanie aktualnie wybranego ustawienia regionalne.


### <a name="nscurrentlocaledidchangenotification"></a>NSCurrentLocaleDidChangeNotification

generuje iOS `NSCurrentLocaleDidChangeNotification` po aktualizacji ich ustawień regionalnych przez użytkownika. Aplikacji można nasłuchiwać dla tego powiadomienia, gdy są uruchomione i wprowadzić odpowiednie zmiany do interfejsu użytkownika.

<a name="basics" />

## <a name="localization-basics-in-ios"></a>Podstawowe informacje o lokalizacji w systemie iOS

Następujące funkcje systemu IOS można łatwo wykorzystać w program Xamarin, aby zapewnić zlokalizowanych zasobów do wyświetlenia dla użytkownika. Zapoznaj się [próbki TaskyL10n](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) na temat sposobu wykonania tych sposobów.

### <a name="infoplist"></a>Info.plist

Przed rozpoczęciem należy skonfigurować **Info.plist** pliku z następujących kluczy:

- `CFBundleDevelopmentRegion` -język domyślny dla aplikacji (zwykle język używany przez deweloperów i używane w scenorys i zasoby ciągów i obrazu). W poniższym przykładzie **en** (dla języka angielskiego) został określony.
- `CFBundleLocalizations` -tablicę inne lokalizacje obsługiwane przez aplikację, również użyć kodów langauge jak **es** (wersja hiszpańska) i **pt-PT** (portugalski używany w Portugalia).

```xml
<key>CFBundleDevelopmentRegion</key>
<string>en</string>
<key>CFBundleLocalizations</key>
<array>
  <string>de</string>
  <string>es</string>
  <string>ja</string>
  ...
</array>
```

### <a name="localizedstring-method"></a>LocalizedString — metoda

`NSBundle.MainBundle.LocalizedString` Metody wyszukuje zlokalizowany tekst przechowywanych w **.strings** pliki w projekcie. Te pliki są zorganizowane według języka, w specjalnej katalogi z **.lproj** sufiks.

#### <a name="strings-file-locations"></a>Lokalizacje plików .strings

- **Base.lproj** jest katalog, który zawiera zasoby dla języka domyślnego.
  Często znajduje się w katalogu głównym projektu (ale mogą także zostać umieszczone w **zasobów** folderu).
- **<language>.lproj** katalogów utworzonych dla każdej z obsługiwanych języków, zazwyczaj w **zasobów** folderu.

Może istnieć szereg różnych **.strings** plików w każdym katalogu języka:

- **Localizable.Strings** -główną listę zlokalizowanego tekstu.
- **InfoPlist.strings** — niektóre określone klucze są dozwolone w tym pliku można przetłumaczyć elementów, jak nazwa aplikacji.
- **< nazwa scenorysu > .strings** — opcjonalny plik, który zawiera tłumaczenia elementów interfejsu użytkownika w scenorysu.

**Akcja kompilacji** tych plików powinna być **zasobów pakietu**.

#### <a name="strings-file-format"></a>format pliku .strings

Składnia zlokalizowany ciąg wartości to:

```csharp
/* comment */
"key"="localized-value";
```

Należy wprowadzić następujące znaków w ciągach:

* `\"`  oferty
* `\\`  ukośnik odwrotny
* `\n`  Nowy wiersz

To jest przykład **es/Localizable.strings** (tj. Plik hiszpański) z próbki:

```csharp
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

```csharp
Default.png
Default@2x.png
Default-568h@2x.png
LaunchScreen.xib
```

### <a name="app-name"></a>Nazwa aplikacji

Wprowadzenie do **InfoPlist.strings** w pliku **.lproj** katalogu pozwala na zastępowanie niektórych wartości w aplikacji **Info.plist**, łącznie z nazwą aplikacji:

```csharp
"CFBundleDisplayName" = "LeónTodo";
```

Inne klucze, które służy do [localize — ciągi specyficzne dla aplikacji](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW21) są:

- CFBundleName
- CFBundleShortVersionString
- NSHumanReadableCopyright


<!--
## App icon

Does not seem to be possible (although it definitely used to be!).
-->

### <a name="localization-native-development-region"></a>Lokalizacja natywnych regionu

Domyślne parametry (znajdujące się w **Base.lproj** folder) będzie traktowane jako język "rezerwowej". Oznacza to, że jeśli tłumaczenie jest wymagane w kodzie i nie został znaleziony dla bieżącego języka **Base.lproj** folder ma zostać wyszukany domyślny ciąg do użycia (Jeśli nie znaleziono, sam ciąg identyfikatora tłumaczenia jest wyświetlane).

Deweloperzy mogą wybierz inny język na rezerwowy, ustawiając klucz plist `CFBundleDevelopmentRegionKey`. Wartość powinna być równa kod języka dla natywnych języka. Ten zrzut ekranu przedstawia zestawu plist w programie Xamarin Studio z obszaru macierzystego programowanie na język hiszpański (es):

![](images/cfbundledevelopmentregion.png "Właściwość InfoPList.strings")

Jeśli domyślny język używany w Twojej scenorys i w całym kodzie nie jest angielski, ustaw tę wartość, aby odzwierciedlić językiem macierzystym używane w całym kodzie aplikacji.

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

```csharp
Full,Long: Friday, August 7, 2015 at 10:29:32 AM PDT
Short,Short: 8/7/15, 10:29 AM
Medium,None: Aug 7, 2015
```

Wyniki dla języka hiszpańskiego w Hiszpanii:

```csharp
Full,Long: viernes, 7 de agosto de 2015, 10:26:58 GMT-7
Short,Short: 7/8/15 10:26
Medium,None: 7/8/2015
```

Odnoszą się do firmy Apple [elementy formatujące datę](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html) dokumentacji, aby uzyskać więcej informacji. Podczas testowania formatowania zależne od ustawień regionalnych daty i godziny, sprawdź zarówno **iPhone języka** i **Region** ustawienia.

<a name="rtl" />

### <a name="right-to-left-rtl-layout"></a>Układ od prawej do lewej (od prawej do lewej)

iOS udostępnia wiele funkcji, aby pomóc w tworzeniu aplikacji obsługujących od prawej do lewej:

* Użyj **układ automatyczny** `leading` i `trailing` atrybuty dla formantu Ustawienie osiowe (które odpowiada *po lewej stronie* i *prawo* dla języka angielskiego, na przykład, ale jest odwrócona dla języków RTL).
  [ `UIStackView` ](~/ios/user-interface/controls/uistackview.md) Kontroli jest szczególnie przydatne podczas rozmieszczania formantów pod uwagę od prawej do lewej.
* Użyj `TextAlignment = UITextAlignment.Natural` dla wyrównania tekstu (który będzie *po lewej stronie* większości języków, ale *prawo* dla od prawej do lewej).
* `UINavigationController` Przerzuca przycisku Wstecz i automatycznie zmieni kierunek Przejdź.

Pokaż poniższe zrzuty ekranu [zlokalizowanych **Tasky** próbki](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) arabski i hebrajski (chociaż angielski został wprowadzony w polach):

[ ![](images/rtl-ar-sml.png "Lokalizacja w arabski")](images/rtl-ar.png "Arabic") 

[ ![](images/rtl-he-sml.png "Lokalizacja (wersja hebrajska)")](images/rtl-he.png "Hebrew")

iOS automatycznie cofa `UINavigationController`, i inne formanty są umieszczone wewnątrz `UIStackView` lub wyrównane z automatycznym układem.
Tekst od prawej do lewej jest zlokalizowana przy użyciu **.strings** pliki w taki sam sposób jak tekstu od lewej do prawej.


<a name="code"/>

## <a name="localizing-the-ui-in-code"></a>Lokalizowanie interfejsu użytkownika w kodzie

[Tasky (zlokalizowany w kodzie)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) przykładowych pokazano, jak do zlokalizowania aplikacji gdy interfejs użytkownika korzysta z wbudowanej w code (zamiast XIBs lub scenorys).

### <a name="project-structure"></a>Struktury projektu



![](images/solution-code.png "Drzewo zasobów")

### <a name="localizablestrings-file"></a>Plik Localizable.Strings

Jak opisano powyżej, **Localizable.strings** format pliku składa się z pary klucz wartość, gdy klucz jest wybrane przez użytkownika ciąg, który wskazuje

Hiszpański (**es**) poniżej przedstawiono lokalizacje przykładowej:

```csharp
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

* w programie Visual Studio for Mac ma znajduje się w konsoli właściwości i wywołać **identyfikator lokalizacji**.
* w programie Xcode, jest nazywany **obiektu o identyfikatorze**.

Jest to wartość ciągu o często ma formę, takich jak **"NF3-h8 xmR"**:

![](images/xs-designer-localization-id.png "Widok Xcode scenorysu lokalizacji")

Ta wartość jest używana w **.strings** plik można automatycznie przypisać przetłumaczony tekst do każdego formantu.

### <a name="mainstoryboardstrings"></a>MainStoryboard.strings

Format pliku tłumaczenia scenorysu przypomina **Localizable.strings** pliku, z wyjątkiem tego, że klucz (wartość po lewej stronie) nie może być zdefiniowana przez użytkownika, ale zamiast tego musi mieć dokładnie format: `ObjectID.property`.

W przykładzie **Mainstoryboard.strings** poniżej widać `UITextField`s ma `placeholder` właściwości tekstu, który może być lokalizowany; `UILabel`ma s `text` właściwości; i `UIButton`s domyślny tekst jest ustawiany za pomocą `normalTitle`:

```csharp
"SXg-TT-IwM.placeholder" = "nombre de la tarea";
"Pqa-aa-ury.placeholder"= "otra información de tarea";
"zwR-D9-hM1.text" = "Detalles de la tarea";
"bAM-2j-Rzw.text" = "Notas";           /* Notes */
"NF3-h8-xmR.text" = "Completo";        /* Done */
"MWt-Ya-pMf.normalTitle" = "Guardar";  /* Save */
"IGr-pR-05L.normalTitle" = "Eliminar"; /* Delete */
```

> [!IMPORTANT]
> **Przy użyciu scenorysu z klasami rozmiar** może spowodować tłumaczeń nie są widoczne. Prawdopodobnie odnosi się do [ten problem](http://stackoverflow.com/questions/24989208/xcode-6-does-not-localize-interface-builder) gdzie mówi dokumentacji firmy Apple: Lokalizacja storyboard lub XIB zostanie nie localize poprawnie, jeśli spełnione są wszystkie poniższe warunki trzy: storyboard lub XIB używa klas rozmiar. Lokalizacja podstawowa i docelowa kompilacji są ustawiane na uniwersalny. Kompilacja jest przeznaczony dla systemu iOS 7.0.
Poprawka ma zduplikowane pliku ciągi scenorysu na dwa pliki o identycznych: **MainStoryboard~iphone.strings** i **MainStoryboard~ipad.strings**:

> ![](images/xs-dup-strings.png "Ciągi, pliki")


<!--
# Native Formatting

Xamarin.iOS applications can take advantage of the .NET framework's formatting options for localizing
numbers and dates.

### Date string formatting

https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html#//apple_ref/doc/uid/TP40002369-SW1


NSDateFormatter formatter = new NSDateFormatter ();
formatter.DateFormat = "MMMM/dd/yyyy";
NSString dateString = new NSString (formatter.ToString (d));


### Number formatting

https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfNumberFormatting10_4.html#//apple_ref/doc/uid/TP40002368-SW1

-->

<a name="appstore" />

## <a name="app-store-listing"></a>Lista sklepu z aplikacjami

Następuje firmy Apple — często zadawane pytania [lokalizacja magazynu App](https://itunespartner.apple.com/en/apps/faq/App%20Store_Localization) wprowadzenia tłumaczenia dla każdego kraju aplikacji znajduje się w sprzedaży. Należy pamiętać, ich ostrzeżenie, że tłumaczeń będą wyświetlane tylko, jeśli aplikacja zawiera również zlokalizowanych **.lproj** katalogu dla języka.

<!--

Once you’ve entered your application into iTunes Connect the default language
metadata and screenshots will appear as shown:

![]( "itunes connect 1")

Use the language list on the right to select other languages to provide
translated application name, description, search keywords, URLs and screenshots.
The complete list of languages is shown in this screenshot:

![]( "itunes connect 2")
-->

## <a name="summary"></a>Podsumowanie

W tym artykule opisano podstawy lokalizowania aplikacji systemu iOS przy użyciu zasobów wbudowane funkcje obsługi i scenorysu.

Aby dowiedzieć się więcej o i18n i L10n dla aplikacji systemu iOS, Android i wielu platform (w tym platformy Xamarin.Forms) w [tego podręcznika i platform](~/cross-platform/app-fundamentals/localization.md).


## <a name="related-links"></a>Linki pokrewne

- [Tasky (zlokalizowany w kodzie) (przykład)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [Tasky (zlokalizowanego scenorysu) (przykład)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard)
- [Przewodnik po lokalizacji firmy Apple](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)
- [Informacje o lokalizacji i Platform](~/cross-platform/app-fundamentals/localization.md)
- [Lokalizacja platformy Xamarin.Forms](~/xamarin-forms/app-fundamentals/localization.md)
- [Lokalizacja systemu android](~/android/app-fundamentals/localization.md)
