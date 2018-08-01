---
title: Lokalizacja w rozszerzeniu Xamarin.iOS
description: W tym dokumencie opisano funkcje lokalizacji systemu iOS i sposobie używania tych funkcji w aplikacji platformy Xamarin.iOS. Omówiono w nim języka, ustawienia regionalne, pliki ciągów i obrazów uruchamiania.
ms.prod: xamarin
ms.assetid: DFD9EB4A-E536-18E4-C8FD-679BA9C836D8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/28/2017
ms.openlocfilehash: 2a6096efc18f40d18ea37573e77d93796e812cc2
ms.sourcegitcommit: 4cc17681ee4164bdf2f5da52ac1f2ae99c391d1d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39387443"
---
# <a name="localization-in-xamarinios"></a>Lokalizacja w rozszerzeniu Xamarin.iOS

_W tym dokumencie opisano funkcje lokalizacji zestawu iOS SDK oraz sposób uzyskiwać do nich dostęp za pomocą platformy Xamarin._

Zapoznaj się [kodowanie internacjonalizacji](encodings.md) instrukcje w tym zestawy znaków/strony kodowe w aplikacjach, które musi przetworzyć danych innego niż Unicode.

## <a name="ios-platform-features"></a>Funkcje platformy systemu iOS

W tej sekcji opisano niektóre funkcje lokalizacji w systemie iOS. Przejdź do [następnej sekcji](#basics) określonego kodu i przykłady.

### <a name="language"></a>Język

Użytkownicy wybierają język w **ustawienia** aplikacji. To ustawienie wpływa na ciągi w języku i obrazów wyświetlanych przez system operacyjny i w aplikacjach. 

Aby określić język używany w aplikacji, należy pobrać pierwszy element `NSBundle.MainBundle.PreferredLocalizations`:

```csharp
var lang = NSBundle.MainBundle.PreferredLocalizations[0];
```

Ta wartość będzie kod języka takich jak `en` dla języka angielskiego `es` dla języka hiszpańskiego, `ja` w języku japońskim, itp. Wartość zwracana jest ograniczona do jednego z lokalizacje obsługiwane przez aplikację (przy użyciu reguł rezerwowego określić najlepsze dopasowanie).

Kod aplikacji nie zawsze jest konieczne Sprawdź, czy ta wartość — Xamarin i systemu iOS obie zapewniają funkcje, które ułatwiają zapewnienie automatycznie poprawne parametry lub zasobów dla języka użytkownika. Te funkcje są opisane w dalszej części tego dokumentu.

> [!NOTE]
> Użyj `NSLocale.PreferredLanguages` ustalenie, preferencje językowe użytkownika, niezależnie od tego, lokalizacje obsługiwane przez aplikację. Wartości zwracane przez tę metodę zmienione w systemie iOS 9. zobacz [techniczne TN2418 Uwaga](https://developer.apple.com/library/content/technotes/tn2418/_index.html) Aby uzyskać szczegółowe informacje.

### <a name="locale"></a>Regionalne

Użytkownicy wybierają swoje ustawienia regionalne w **ustawienia** aplikacji. To ustawienie ma wpływ na sposób, w formacie daty, godziny, numery i waluty.

To umożliwia użytkownikom wybranie, czy zobaczą formatów godziny 12-godzinny lub 24-godzinnym, czy ich separatora dziesiętnego jest przecinek lub punkt i kolejność dzień, miesiąc i rok w wyświetlania daty.

Za pomocą platformy Xamarin mają dostęp do obu firmy Apple dla systemu iOS (`NSNumberFormatter`) oraz klas .NET w System.Globalization. Programiści powinni rozważyć nadaje lepiej do swoich potrzeb, jak istnieją różne funkcje dostępne w każdym. W szczególności jeśli są pobieranie i wyświetlanie ceny zakupu w aplikacji przy użyciu StoreKit należy użyć klasy formatowania firmy Apple dla zwrócone informacje o cenach.

Bieżących ustawień regionalnych, mogą być przeszukiwane przy użyciu jednej z dwóch sposobów:

- `NSLocale.CurrentLocale.LocaleIdentifier`
- `NSLocale.AutoUpdatingCurrentLocale.LocaleIdentifier`

Pierwsza wartość mogą być buforowane przez system operacyjny, a więc może nie zawsze odzwierciedla aktualnie wybranego ustawienia regionalne użytkownika. Druga wartość umożliwia uzyskanie aktualnie wybranego ustawienia regionalne.

> [!NOTE]
> Narzędzie mono (środowiska uruchomieniowego .NET podstawie Xamarin.iOS) i interfejsów API systemu iOS firmy Apple nie obsługują identyczne zestawy warunków o kombinacje język/region.
> W związku z tym jest możliwe wybranie kombinacji języka i regionu w narzędziu iOS **ustawienia** aplikację, która nie jest mapowany na prawidłową wartość w rozwiązaniu Mono. Na przykład ustawienie jego region i język iPhone do języka angielskiego (Hiszpania) spowoduje, że następujące interfejsy API umożliwiające uzyskanie różne wartości:
> 
> - `CurrentThead.CurrentCulture`: en US (interfejs API platformy Mono)
> - `CurrentThread.CurrentUICulture`: en US (interfejs API platformy Mono)
> - `NSLocale.CurrentLocale.LocaleIdentifier`: en_ES (interfejs API firmy Apple)
>
> Ponieważ używane narzędzie Mono `CurrentThread.CurrentUICulture` aby wybrać zasoby i `CurrentThread.CurrentCulture` do formatowania dat i walut, platformy Mono na podstawie lokalizacji (na przykład z plikami .resx) może nie przynieść oczekiwanych wyników, aby uzyskać te kombinacje język/region. W takich sytuacjach zależą od interfejsów API firmy Apple, aby zlokalizować, gdy jest to konieczne.

### <a name="nscurrentlocaledidchangenotification"></a>NSCurrentLocaleDidChangeNotification

generuje dla systemu iOS `NSCurrentLocaleDidChangeNotification` po użytkownik nie zaktualizuje jego ustawienia regionalne. Aplikacje może nasłuchiwać dla tego powiadomienia, gdy są uruchomione i ułatwia odpowiednie zmiany w interfejsie użytkownika.

<a name="basics" />

## <a name="localization-basics-in-ios"></a>Podstawowe informacje o lokalizacji, w systemie iOS

Następujące funkcje systemu iOS można łatwo wykorzystać w środowisku Xamarin, aby zapewnić zlokalizowanych zasobów do wyświetlania użytkownikowi. Zapoznaj się [przykładowe TaskyL10n](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) aby zobaczyć, jak zaimplementować te pomysły.

### <a name="specifying-default-and-supported-languages-in-infoplist"></a>Określanie domyślnego i obsługiwanych języków w pliku Info.plist

W [pytania techniczne i QA1828: jak iOS określa język dla twojej aplikacji](https://developer.apple.com/library/content/qa/qa1828/_index.html), Apple opisano, jak dla systemu iOS wybiera język do użycia w aplikacji. Następujące czynniki wpływ, jaki język jest wyświetlany:

- Użytkownik preferowanego języków (znalezione w **ustawienia** aplikacji)
- Lokalizacje powiązane z aplikacją (.lproj foldery)
- `CFBundleDevelopmentRegion` (**Info.plist** wartość określającą język domyślny dla aplikacji)
- `CFBundleLocalizations` (**Info.plist** tablicy, określając wszystkie obsługiwane lokalizacje)

Jak wskazano w technicznej pytań i odpowiedzi, `CFBundleDevelopmentRegion` reprezentuje domyślny region i język aplikacji. Aplikacja jawnie nie obsługuje żadnego z preferowanych języków przez użytkownika, zostanie użyty język określony według tego pola. 

> [!IMPORTANT]
> System iOS 11 dotyczy ten mechanizm zaznaczania języka bardziej restrykcyjnie, nie było w poprzednich wersjach systemu operacyjnego. W związku z tym dowolnej aplikacji systemu iOS 11, które jawnie deklaruj obsługiwane lokalizacje — tym foldery .lproj lub ustawiania wartości dla `CFBundleLocalizations` — może być wyświetlany inny język w systemie iOS 11, niż w systemie iOS 10.

Jeśli `CFBundleDevelopmentRegion` nie została określona w **Info.plist** pliku, narzędzia do kompilacji rozszerzenia Xamarin.iOS obecnie używana wartość domyślna `en_US`. Chociaż może to ulec zmianie w przyszłych wydaniach, oznacza to, że domyślnym językiem jest angielski.

Aby upewnić się, że aplikacja wybiera oczekiwanego języka, wykonaj następujące czynności:

- Określ język domyślny. Otwórz **Info.plist** i użyj **źródła** widok, aby ustawić wartość dla `CFBundleDevelopmentRegion` klucz; w pliku XML, powinien wyglądać podobnie do poniższego:

```xml
<key>CFBundleDevelopmentRegion</key>
<string>es</string>
```

W tym przykładzie użyto "es", aby określić, gdy żaden użytkownik preferowane, języki są obsługiwane, domyślnie hiszpański.

- Należy zadeklarować wszystkie obsługiwane lokalizacje. W **Info.plist**, użyj **źródła** widok, aby ustawić tablicę `CFBundleLocalizations` klucz; w pliku XML, powinien wyglądać podobnie do poniższego:

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>es</string>
    ...
</array>
```

Aplikacji platformy Xamarin.iOS, które zostały zlokalizowane za pomocą mechanizmów .NET, takich jak pliki resx, musisz podać te **Info.plist** również wartości.

Aby uzyskać więcej informacji na temat tych **Info.plist** klucze, zapoznaj się z firmy Apple [informacji właściwości listy kluczach](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html).

### <a name="getlocalizedstring-method"></a>Metoda GetLocalizedString

`NSBundle.MainBundle.GetLocalizedString` Metoda wyszukuje zlokalizowanego tekstu, które zostały zapisane w **.strings** pliki w projekcie. Te pliki są uporządkowane według języka, w specjalnej katalogów przy użyciu **.lproj** sufiks.

#### <a name="strings-file-locations"></a>Lokalizacje plików .strings

- **Base.lproj** jest katalogiem, który zawiera zasoby domyślnego języka.
  Często znajduje się w katalogu głównym projektu (, ale mogą także zostać umieszczone w **zasobów** folder).
- **<language>.lproj** katalogi są tworzone dla każdego z obsługiwanych języków z zwykle w **zasobów** folderu.

Może istnieć szereg różnych **.strings** plików w każdym katalogu języka:

- **Localizable.Strings** — główny listy zlokalizowanego tekstu.
- **InfoPlist.strings** — niektórych określonych klucze są dozwolone w tym pliku do translacji elementy, takie jak nazwa aplikacji.
- **< nazwa scenorysu > .strings** — opcjonalny plik, który zawiera tłumaczenia elementów interfejsu użytkownika w scenorysu.

**Build Action** dla tych plików powinny być **zasobów pakietu**.

#### <a name="strings-file-format"></a>format pliku .strings

Składnia dla wartości ciągu zlokalizowanego jest następująca:

```console
/* comment */
"key"="localized-value";
```

Należy wprowadzić następujące znaków w ciągach:

* `\"`  oferty
* `\\`  Ukośnik odwrotny
* `\n`  nowy wiersz

Jest to przykład **es/Localizable.strings** (tj. Hiszpański) plik z przykładu:

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

1. Na przykład odwoływać się do obrazu w kodzie:

  ```csharp
  UIImage.FromBundle("flag");
  ```

2. Umieść plik obrazu domyślnego **flag.png** w **Base.lproj** (katalog natywny programowania języka).

3. Opcjonalnie umieść zlokalizowane wersje obrazu w **.lproj** folderów dla każdego języka (np.) **es.lproj**, **ja.lproj**). Użyj tej samej nazwy pliku **flag.png** w każdym katalogu języka.

Jeśli obraz nie jest obecny dla określonego języka, dla systemu iOS będzie wrócić do domyślnego folderu macierzystym języku i załadowania obrazu stamtąd.


#### <a name="launch-images"></a>Obrazy uruchamiania

Użyj standardowych konwencji nazewnictwa dla obrazów uruchamiania (i pliku XIB lub Scenorysie dla modeli telefonów iPhone 6) w przypadku umieszczenia ich w **.lproj** katalogów dla każdego języka.

```console
Default.png
Default@2x.png
Default-568h@2x.png
LaunchScreen.xib
```

### <a name="app-name"></a>Nazwa aplikacji

Wprowadzenie do **InfoPlist.strings** w pliku **.lproj** katalogu pozwala zastąpić niektóre wartości z poziomu aplikacji **Info.plist**, łącznie z nazwą aplikacji:

```console
"CFBundleDisplayName" = "LeónTodo";
```

Inne klucze używane do [localize — ciągi specyficzne dla aplikacji](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW21) są:

- CFBundleName
- CFBundleShortVersionString
- NSHumanReadableCopyright

### <a name="dates-and-times"></a>Daty i godziny

Chociaż istnieje możliwość użycia wbudowanych funkcji daty i godziny .NET (wraz z bieżącego `CultureInfo`) do formatowania daty i godziny dla ustawień regionalnych, to będzie ignorować specyficzne dla ustawień regionalnych — ustawienia użytkownika (które można skonfigurować niezależnie od języka).

Użyj ustawień systemu iOS `NSDateFormatter` do generowania danych wyjściowych, preferencjom ustawienia regionalne użytkownika. Następujący przykładowy kod przedstawia podstawowe formatowania daty i godziny opcji:

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

Wyniki w języku angielskim na terenie Stanów Zjednoczonych:

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

Odnoszą się do firmy Apple [formatujących datę](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html) dokumentacji, aby uzyskać więcej informacji. Podczas testowania, zależne od ustawień regionalnych daty i godziny, formatowanie, zaznacz zarówno **iPhone języka** i **Region** ustawienia.

<a name="rtl" />

### <a name="right-to-left-rtl-layout"></a>Układ od prawej do lewej (RTL)

dla systemu iOS zapewnia szereg funkcji, która pomaga w tworzeniu aplikacji z obsługą od prawej do lewej:

* Użyj automatycznego układu `leading` i `trailing` atrybuty ustawienie sterowania osiowe, (które odnosi się do lewej i prawej strony w języku angielskim, ale została odwrócona dla języków od prawej do lewej).
  [ `UIStackView` ](~/ios/user-interface/controls/uistackview.md) Formant jest szczególnie przydatne w przypadku układu kontrolek trzeba pamiętać od prawej do lewej.
* Użyj `TextAlignment = UITextAlignment.Natural` dla wyrównania tekstu, (które zostaną pozostawione większość języków, ale po prawej od prawej do lewej).
* `UINavigationController` Przerzuca przycisku Wstecz i automatycznie zmieni kierunek przesunięcia.

Poniższe zrzuty ekranu Pokaż [zlokalizowane przykładowe Tasky](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) arabski i hebrajski (Chociaż język angielski został wprowadzony w pola):

[![](images/rtl-ar-sml.png "Lokalizacja w języku arabskim")](images/rtl-ar.png#lightbox "Arabic") 

[![](images/rtl-he-sml.png "Lokalizacja hebrajska")](images/rtl-he.png#lightbox "Hebrew")

iOS automatycznie odtwarzana od końca `UINavigationController`, i inne formanty są umieszczone wewnątrz `UIStackView` lub dostosowane do automatycznego układu.
Tekst od prawej do lewej są lokalizowane za pomocą **.strings** pliki w taki sam sposób jak tekst od lewej do prawej.

<a name="code"/>

## <a name="localizing-the-ui-in-code"></a>Lokalizowanie interfejsu użytkownika w kodzie

[Tasky (zlokalizowany w kodzie)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) przykład pokazuje sposób lokalizowanie aplikacji, których interfejs użytkownika jest wbudowana w code (zamiast XIb lub scenorysów).

### <a name="project-structure"></a>Struktura projektu

![](images/solution-code.png "Drzewo zasobów")

### <a name="localizablestrings-file"></a>Plik Localizable.Strings

Jak opisano powyżej, **Localizable.strings** formatu pliku składa się z pary klucz wartość. Klucz opisuje przeznaczenie parametru ciągu, a wartość jest przetłumaczony tekst do użycia w aplikacji.

Hiszpański (**es**) poniżej przedstawiono lokalizacje dla przykładu:

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

W kodzie aplikacji wszędzie tam, gdzie tekst wyświetlany interfejs użytkownika jest ustawiony (nieważne, czy tekst etykiety lub symbol zastępczy wartości wejściowej, itp.) w kodzie użyto systemu iOS `GetLocalizedString` funkcję, aby pobrać poprawne tłumaczenie do wyświetlenia:

```csharp
var localizedString = NSBundle.MainBundle.GetLocalizedString ("key", "optional");
someControl.Text = localizedString;
```

<a name="storyboard"/>

## <a name="localizing-storyboard-uis"></a>Lokalizowanie UI scenorysu

Przykład [Tasky (zlokalizowany scenorysu)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard) pokazuje, jak zlokalizować tekstu dla kontrolek w scenorysu.

### <a name="project-structure"></a>Struktura projektu

**Base.lproj** katalogu zawiera scenorysu, a także powinna zawierać żadnych obrazów używanych w aplikacji.

Zawierają inne katalogi języka **Localizable.strings** pliku dla wszystkich zasobów ciągu, do którego odwołuje się kod, a także **MainStoryboard.strings** pliku, który zawiera tłumaczenia tekstu w scenorysu.

![](images/solution-storyboard.png "Drzewo zasobów")

Katalogi języka powinna zawierać kopię żadnych obrazów, które zostały zlokalizowane, aby zastąpić ten, który istnieje w **Base.lproj**.

### <a name="object-id--localization-id"></a>Obiekt o identyfikatorze / identyfikator lokalizacji

Podczas tworzenia i edytowania kontrolek w scenorysu, zaznacz każdy formant i Sprawdź identyfikator na potrzeby lokalizacji:

* W programie Visual Studio dla komputerów Mac znajduje się w **konsoli właściwości** i nosi nazwę **identyfikator lokalizacji**.
* W środowisku Xcode jest wywoływana **obiektu o identyfikatorze**.

Ta wartość ciągu często ma postać takich jak "NF3-h8-xmR", jak pokazano na poniższym zrzucie ekranu:

![](images/xs-designer-localization-id.png "Widok scenorysu lokalizacji środowiska Xcode")

Ta wartość jest używana w **.strings** plik, aby automatycznie przypisywać przetłumaczonego tekstu do każdej kontrolki.

### <a name="mainstoryboardstrings"></a>MainStoryboard.strings

Format pliku translacji scenorys jest podobne do **Localizable.strings** pliku, z tą różnicą, że klucz (wartość po lewej stronie) nie może być zdefiniowana przez użytkownika, ale zamiast tego muszą mieć format bardzo szczegółowych: `ObjectID.property`.

W przykładzie **Mainstoryboard.strings** poniżej widać `UITextField`ma `placeholder` właściwości tekstu, która może być lokalizowana; `UILabel`ma `text` właściwości i `UIButton`s domyślny tekst można ustawić przy użyciu `normalTitle`:

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
> Za pomocą scenorysu z klas rozmiaru może spowodować tłumaczenia, które nie są wyświetlane w aplikacji. [Informacje o wersji programu Xcode firmy Apple](https://developer.apple.com/library/content/releasenotes/DeveloperTools/RN-Xcode/Chapters/Introduction.html) wskazują, że storyboard i XIB będzie nie jest lokalizowany poprawnie, jeśli spełnione są trzy rzeczy: używa klas rozmiaru, podstawowa lokalizacja i element docelowy kompilacji, które są ustawione na uniwersalny i kompilacja jest przeznaczony dla systemu iOS — 7.0. Obejście polega na zduplikowane plik scenorysu ciągi na dwa identyczne pliki: **MainStoryboard~iphone.strings** i **MainStoryboard~ipad.strings**, jak pokazano na poniższym zrzucie ekranu:
> 
> ![](images/xs-dup-strings.png "Pliki parametrów")

<a name="appstore" />

## <a name="app-store-listing"></a>Lista App Store

Następuje firmy Apple — często zadawane pytania [App Store lokalizacji](https://itunespartner.apple.com/en/apps/faq/App%20Store_Localization) wprowadzenia tłumaczenia dla każdego kraju, Twoja aplikacja jest na sprzedaż. Należy pamiętać, ich ostrzeżenie, że tłumaczenia będą wyświetlane tylko wtedy, jeśli aplikacja zawiera również zlokalizowany **.lproj** katalogu dla języka.

## <a name="summary"></a>Podsumowanie

W tym artykule opisano podstawy lokalizowanie aplikacji systemu iOS przy użyciu funkcji scenorysu i obsługi zasobów wbudowanych.

Możesz dowiedzieć się więcej na temat i18n i L10n dla aplikacji systemów iOS, Android i dla wielu platform (w tym zestawu narzędzi Xamarin.Forms) w [ten przewodnik dla wielu platform](~/cross-platform/app-fundamentals/localization.md).

## <a name="related-links"></a>Linki pokrewne

- [Tasky (zlokalizowany w kodzie) (przykład)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [Tasky (zlokalizowany scenorysu) (przykład)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard)
- [Przewodnik po lokalizacji firmy Apple](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)
- [Przegląd lokalizacja dla wielu Platform](~/cross-platform/app-fundamentals/localization.md)
- [Lokalizacja zestawu narzędzi Xamarin.Forms](~/xamarin-forms/app-fundamentals/localization/index.md)
- [Lokalizacja systemu android](~/android/app-fundamentals/localization.md)
