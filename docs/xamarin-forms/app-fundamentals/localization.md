---
title: Lokalizacja
description: Może być lokalizowany aplikacji platformy Xamarin.Forms przy użyciu plików zasobów .NET.
ms.prod: xamarin
ms.assetid: 852B4ED3-2D2D-48A5-A759-A6591F6A1509
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/06/2016
ms.openlocfilehash: f179fcfc26dd73bf1655c786078dce1f6a02b3a9
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2018
---
# <a name="localization"></a>Lokalizacja

_Może być lokalizowany aplikacji platformy Xamarin.Forms przy użyciu plików zasobów .NET._

## <a name="overview"></a>Omówienie

Wbudowany mechanizm lokalizowania zastosowań aplikacji .NET [pliki RESX](http://msdn.microsoft.com/library/ekyft91f(v=vs.90).aspx) i klasy w `System.Resources` i `System.Globalization` przestrzeni nazw. Pliki RESX zawierający przetłumaczone ciągi są osadzone w zestawie platformy Xamarin.Forms, wraz z klasy generowane przez kompilator, która udostępnia silnie typizowane tłumaczenia. Następnie można pobrać tłumaczenie tekstu w kodzie.

### <a name="sample-code"></a>Przykładowy kod

Istnieją dwa przykłady skojarzony z tym dokumentem:

* [UsingResxLocalization](https://github.com/xamarin/xamarin-forms-samples/tree/master/UsingResxLocalization) jest bardzo prosty pokaz wyjaśniono pojęcia. Fragmenty kodu pokazano poniżej pochodzą z tego przykładu.
* [TodoLocalized](https://github.com/xamarin/xamarin-forms-samples/tree/master/TodoLocalized) Podstawowa aplikacja pracy, która korzysta z tych metod lokalizacji.

#### <a name="shared-projects-are-not-recommended"></a>Nie zaleca się udostępnionych projektów

Zawiera przykładowe TodoLocalized [pokaz projektu udostępnionego](https://github.com/xamarin/xamarin-forms-samples/tree/master/TodoLocalized/SharedProject/) jednak ze względu na ograniczenia system kompilacji nie są pliki zasobów **. Designer.cs narzędzie** plik wygenerowany, która dzieli możliwość dostępu do przetłumaczone ciągi [jednoznacznie w kodzie](~/xamarin-forms/app-fundamentals/localization.md).

W pozostałej części tego dokumentu odnosi się do projektów przy użyciu szablonu PCL platformy Xamarin.Forms.

## <a name="globalizing-xamarinforms-code"></a>Globalizacja platformy Xamarin.Forms kodu

**Globalizacja** aplikacji to proces sprawia, że "gotowy world". Oznacza to, pisania kodu, który jest w stanie wyświetlanie różnych języków.

Jedną z kluczowych części globalizowanie aplikacji tworzy interfejs użytkownika taki sposób, aby nie *ustalony* tekstu. Zamiast tego wyświetlane użytkownikowi żadnych czynności powinny zostać pobrane z zestawem ciągów, które zostały przetłumaczyć ich wybranego języka.

W tym dokumencie zajmiemy się, jak używać pliki RESX do przechowywania tych parametrów i pobierania ich do wyświetlenia w zależności od preferencji użytkownika.

Przykłady wybierz języki angielski, francuski, hiszpański, niemiecki, chińskim, japońskim, rosyjski i portugalski — Brazylia. Aplikacje można przekształcić w kilku lub dowolną liczbę języków zgodnie z wymaganiami.

### <a name="adding-resources"></a>Dodawanie zasobów

Pierwszym etapem globalizowanie aplikacji platformy Xamarin.Forms PCL dodaje pliki zasobów RESX, które będą używane do przechowywania wszystkich tekst używany w aplikacji. Musimy dodać plik RESX, który zawiera domyślny tekst, a następnie dodaj dodatkowe pliki RESX dla każdego z języków, które mają zostać firma Microsoft do obsługi.

#### <a name="base-language-resource"></a>Podstawowy język zasobów

Plik zasobów podstawowej (RESX) będzie zawierać ciągi języka domyślnego (przykłady przyjęto założenie, że językiem domyślnym będzie angielski). Dodaj plik do projektu kodu z typowych platformy Xamarin.Forms prawym przyciskiem myszy projekt i wybierając pozycję **Dodaj > Nowy plik...** .

Wybierz nazwę opisową, takich jak **AppResources** i naciśnij klawisz **OK**.

[![Dodawanie pliku zasobów](localization-images/resx-new-file-sml.png "nowego okna dialogowego pliku")](localization-images/resx-new-file.png#lightbox "nowego okna dialogowego pliku")

Dwa pliki zostaną dodane do projektu:

* **AppResources.resx** pliku, gdzie można przetłumaczyć ciągi są przechowywane w formacie XML.
* **AppResources.designer.cs** pliku, który deklaruje częściowej klasy zawierają odwołania do wszystkich elementów, które są tworzone w pliku RESX XML.

W drzewie rozwiązania zostaną wyświetlone pliki jako powiązane. Plik RESX *powinien* można edytować w celu dodania nowych ciągów przetłumaczyć; **. Designer.cs narzędzie** pliku powinna *nie* można edytowane.

![](localization-images/appresources-tree.png "AppResources.resx File")

##### <a name="string-visibility"></a>Widoczność ciągu

Domyślnie podczas generowania jednoznacznie odwołania do ciągów, zostaną one `internal` do zestawu. Jest to spowodowane generuje domyślnego narzędzia kompilacji dla pliki RESX **. Designer.cs narzędzie** pliku z `internal` właściwości.

Wybierz **AppResources.resx** plików i Pokaż **właściwości** skonfigurować konsoli, gdzie jest to narzędzie kompilacji. Poniżej przedstawiono zrzut ekranu **narzędzie niestandardowe: ResXFileCodeGenerator**.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](localization-images/vs-resx-internal-sml.png "Okno właściwości dla AppResources.Resx")](localization-images/vs-resx-internal.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](localization-images/xs-resx-internal-sml.png "Właściwości konsoli dla AppResources.Resx")](localization-images/xs-resx-internal.png#lightbox)

-----

Aby właściwości ciąg jednoznacznie `public`, należy ręcznie zmienić konfigurację, aby **narzędzie niestandardowe: PublicResXFileCodeGenerator**, jak pokazano na poniższym zrzucie ekranu:


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](localization-images/vs-resx-public-sml.png "Okno właściwości dla AppResources.Resx")](localization-images/vs-resx-public.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](localization-images/xs-resx-internal-sml.png "Właściwości konsoli dla AppResources.Resx")](localization-images/xs-resx-internal.png#lightbox)


[![](localization-images/xs-resx-public-sml.png "Właściwości konsoli dla AppResources.Resx")](localization-images/xs-resx-public.png#lightbox)

-----

Ta zmiana jest opcjonalna i jest tylko wymagane, jeśli chcesz odwołać zlokalizowanych ciągów w różnych zestawów (na przykład, jeśli pliki RESX należy umieścić w innym zestawie kodu). Przykładowe związane z tym tematem pozostawia ciągi `internal` ponieważ są one zdefiniowane w tym samym zestawie PCL platformy Xamarin.Forms, gdzie są używane.

Musisz ustawić narzędzia niestandardowego w pliku RESX podstawowego, jak pokazano powyżej. nie należy ustawić *żadnych* narzędzia kompilacji na pliki RESX specyficzny dla języka omówione w poniższych sekcjach.

##### <a name="editing-the-resx-file"></a>Edytowanie pliku RESX

Niestety nie jest wbudowanego edytora RESX w programie Visual Studio dla komputerów Mac. Dodawanie nowych ciągów przetłumaczyć wymaga dodatkowo nowe XML `data` elementu dla każdego ciągu. Każdy `data` element może zawierać następujące:

* `name` atrybut (wymagane) to klucz dla tych parametrów można przetłumaczyć. Należy prawidłowa C# nazwa właściwości — co bez spacji i znaków specjalnych są dozwolone.
* `value` element (wymagane), który jest rzeczywisty ciąg, który jest wyświetlany w aplikacji.
* `comment` element (opcjonalnie) może zawierać instrukcji dotyczących translatora, który objaśnia, jak ten ciąg jest używany.
* `xml:space` atrybut (opcjonalnie) kontrolować sposób odstępy w parametrach są zachowywane.

Przykładowe `data` elementy są wyświetlane tutaj:

```xml
<data name="NotesLabel" xml:space="preserve">
    <value>Notes:</value>
    <comment>label for input field</comment>
</data>
<data name="NotesPlaceholder" xml:space="preserve">
    <value>eg. buy milk</value>
    <comment>example input for notes field</comment>
</data>
<data name="AddButton" xml:space="preserve">
    <value>Add new item</value>
</data>
```

Jak napisano aplikację, co element tekst wyświetlany użytkownikowi powinny zostać dodane do podstawowego pliku RESX zasobów w nowym `data` elementu. Zaleca się, że obejmuje `comment`s, ile to możliwe upewnić się, tłumaczenia wysokiej jakości.

> [!NOTE]
> Visual Studio (w tym bezpłatna wersja Community) zawiera podstawowe Edytor RESX. Jeśli masz dostęp do komputera z systemem Windows, który może być to wygodny sposób dodawać i edytować ciągów w pliki RESX.

#### <a name="language-specific-resources"></a>Zasoby językowe

Zazwyczaj rzeczywiste tłumaczenie ciągów tekstowych domyślne nie nastąpi aż zapisano duże zestawy aplikacji (w takim przypadku domyślnego pliku RESX będzie zawierać wiele ciągów). Nadal jest dobrym rozwiązaniem, aby dodać zasoby specyficzne dla języka wczesnym etapie cyklu projektowania, opcjonalnie zapełnianie przetłumaczony maszynowo tekstu, aby pomóc w testowania kodu lokalizacji.

Jeden dodatkowy plik RESX jest dodawany do każdego języka, który mamy zamierzają obsługuje.
Pliki zasobów specyficzne dla języka, należy wykonać określonych konwencji nazewnictwa: Użyj tej samej nazwy pliku, jak podstawowe zasobów (np plików. **AppResources**) następuje znak kropki (.), a następnie kod języka. Proste przykłady:

* **AppResources.fr.resx** -tłumaczenia na język francuski.
* **AppResources.es.resx** -tłumaczenia na język hiszpański.
* **AppResources.de.resx** -tłumaczenia na język niemiecki.
* **AppResources.ja.resx** -tłumaczeń języka japońskiego.
* **AppResources.zh Hans.resx** — chiński (uproszczony) język tłumaczenia.
* **AppResources.zh Hant.resx** — chiński (tradycyjny) język tłumaczenia.
* **AppResources.pt.resx** — portugalski język tłumaczenia.
* **AppResources.pt BR.resx** — Brazylia portugalski język tłumaczenia.

Ogólne wzorzec jest użycie kodów języków dwuliterowych, ale istnieje kilka przykładów (takich jak angielski, chiński), w którym jest używany inny format, a także inne przykłady (na przykład portugalski — Brazylia) którym wymagany jest identyfikator ustawień regionalnych czterech znaków.

Te pliki zasobów specyficzne dla języka *nie* wymagają **. Designer.cs narzędzie** klasy częściowe, może zostać dodana jako zwykłe pliki XML, z **Akcja kompilacji: EmbeddedResource**ustawiony. Ten zrzut ekranu przedstawia rozwiązanie zawierające pliki językowe zasobów:

![](localization-images/appresources-langs.png "Pliki zasobów specyficzne dla języka")

Zaprojektowano w aplikacji oraz pliku RESX podstawowego ma tekst dodany, należy wysyła go do tłumaczy, którzy przekształci każdego `data` element i przywrócenie pliku zasobu określonego języka (przy użyciu konwencji nazewnictwa przedstawiono) do uwzględnienia w aplikacji. Poniżej przedstawiono przykładowe "przetłumaczony maszynowo":

**AppResources.es.resx (wersja hiszpańska)**

```xml
<data name="AddButton" xml:space="preserve">
    <value>Escribir un artículo</value>
    <comment>this string appears on a button to add a new item to the list</comment>
</data>
```

**AppResources.ja.resx (wersja japońska)**

```xml
<data name="AddButton" xml:space="preserve">
    <value>新しい項目を追加</value>
    <comment>this string appears on a button to add a new item to the list</comment>
</data>
```

**AppResources.pt-BR.resx (Brazilian Portuguese)**

```xml
<data name="AddButton" xml:space="preserve">
    <value>adicionar novo item</value>
    <comment>this string appears on a button to add a new item to the list</comment>
</data>
```

Tylko `value` element musi zostać zaktualizowany przez translator - `comment` nie jest przeznaczona do tłumaczenia. Pamiętaj: podczas edytowania plików XML, aby wprowadzić zarezerwowanych znaków, takich jak `<`, `>`, `&` z `&lt;`, `&gt;`, i `&amp;` Jeśli pojawią się one w `value` lub `comment`.

<a name="incode" />

### <a name="using-resources-in-code"></a>Korzystanie z zasobów w kodzie

Ciągi w plikach zasobów RESX będą dostępne do użycia w użytkowników interface kodu za pomocą `AppResources` klasy. `name` Przypisany do każdego ciągu w RESX plik staje się właściwość tej klasy, która może być przywoływany w kodu platformy Xamarin.Forms, jak pokazano poniżej:

```csharp
var myLabel = new Label ();
var myEntry = new Entry ();
var myButton = new Button ();
// populate UI with translated text values from resources
myLabel.Text = AppResources.NotesLabel;
myEntry.Placeholder = AppResources.NotesPlaceholder;
myButton.Text = AppResources.AddButton;
```

Interfejs użytkownika w systemach iOS, Android i renderuje Windows platformy Uniwersalnej jako użytkownik z oczekiwaniami, z wyjątkiem teraz możliwe do tłumaczenia aplikacji na wiele języków, ponieważ tekst jest ładowany z zasobu, a nie jest ustalony. Poniżej przedstawiono zrzut ekranu przedstawiający interfejsu użytkownika na każdej platformie przed tłumaczenia:

![](localization-images/simple-example-english.png "Obsługujący wiele Platform UI przed tłumaczenia")

### <a name="troubleshooting"></a>Rozwiązywanie problemów

#### <a name="testing-a-specific-language"></a>Testowanie określonego języka

Może to być trudnych do przełączania symulator lub urządzenie do różnych języków, szczególnie podczas tworzenia, jeśli chcesz szybko przetestować innych kultur.

Możesz wymusić określonego języka być załadowana przez ustawienie `Culture` jak pokazano w poniższym przykładzie:

```csharp
// force a specific culture, useful for quick testing
AppResources.Culture =  new CultureInfo("fr-FR");
```

Takie podejście — ustawienie kultury bezpośrednio na `AppResources` klasy — mogą służyć do zaimplementowania selektor języka wewnątrz aplikacji (a nie przy użyciu ustawień regionalnych urządzenia).

#### <a name="loading-embedded-resources"></a>Ładowanie zasobów osadzonych

Poniższy fragment kodu przydaje się do debugowania problemów z zasobów osadzonych (takich jak pliki RESX). Dodaj ten kod do aplikacji (na wczesnym etapie cyklu życia aplikacji) i wyświetla listę wszystkich zasobów osadzonych w zestawie, przedstawiający identyfikator zasobu pełnej:

```csharp
using System.Reflection;
// ...
// NOTE: use for debugging, not in released app code!
var assembly = typeof(EmbeddedImages).GetTypeInfo().Assembly; // "EmbeddedImages" should be a class in your app
foreach (var res in assembly.GetManifestResourceNames())
{
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

W **AppResources.Designer.cs** pliku (wokół *wiersz 33*), pełny *nazwa Menedżera zasobów* określono (np.) `"UsingResxLocalization.Resx.AppResources"`) podobne do następujących:

```csharp
System.Resources.ResourceManager temp =
        new System.Resources.ResourceManager(
                "UsingResxLocalization.Resx.AppResources",
                typeof(AppResources).GetTypeInfo().Assembly);
```

Sprawdź **danych wyjściowych aplikacji** wyniki debugowania kodzie pokazanym powyżej, aby potwierdzić prawidłowe zasoby są wyświetlane (tj. `"UsingResxLocalization.Resx.AppResources"`).

Jeśli nie, `AppResources` klasa nie będzie można załadować jego zasobów.
Sprawdź następujące polecenie, aby rozwiązać problemy, których nie można odnaleźć zasobów:

* Domyślny obszar nazw dla projektu jest zgodna w głównej przestrzeni nazw **AppResources.Designer.cs** pliku.
* Jeśli **AppResources.resx** plik znajduje się w podkatalogu, nazwę podkatalogu powinna być częścią obszaru nazw *i* częścią identyfikator zasobu.
* **AppResources.resx** plik ma **Akcja kompilacji: EmbeddedResource**.
* **Opcje projektu > Kod źródłowy > zasady nazewnictwa platformy .NET > Użyj Visual Studio stylu zasobów nazwy** zaznaczono. Można to untick, jeśli wolisz, jednak należy obszarów nazw używanych podczas odwoływania się do Twoich zasobów RESX aktualizowane w całej aplikacji.

#### <a name="doesnt-work-in-debug-mode-android-only"></a>Nie działa w trybie debugowania (tylko Android)

Przetłumaczone ciągi pracy w kompilacjach wydania systemu Android, ale nie podczas debugowania, kliknij prawym przyciskiem myszy **projekt systemu Android** i wybierz **Opcje > kompilacji > Android kompilacji** i upewnij się, że **Szybkie wdrożenie zestawu** nie zaznaczono. Ta opcja powoduje problemy podczas ładowania zasobów i nie powinna być używana, jeśli testujesz zlokalizowanych aplikacji.

### <a name="displaying-the-correct-language"></a>Wyświetlanie języku

Do tej pory zostały zbadane jak napisać kod, dzięki czemu można podać tłumaczenia, ale nie jak faktycznie je. Kod platformy Xamarin.Forms można korzystać z. Zasoby w sieci można załadować tłumaczeń właściwy język, ale musimy na każdej z platform ustalić użytkownik wybrał języka w systemie operacyjnym.

Ponieważ niektóre specyficzne dla platformy kodu jest wymagane do uzyskania Preferencje językowe użytkownika, należy użyć [zależności usługi](~/xamarin-forms/app-fundamentals/dependency-service/index.md) ujawnić te informacje w aplikacji platformy Xamarin.Forms i wdrożyć go dla każdej platformy.

Najpierw należy zdefiniować interfejs do udostępnienia preferowanej kultury użytkownika, podobnie jak poniższy kod:

```csharp
public interface ILocalize
{
    CultureInfo GetCurrentCultureInfo ();
    void SetLocale (CultureInfo ci);
}
```

Po drugie, użyć [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) w platformy Xamarin.Forms `App` klasy do wywołania interfejsu i ustaw naszych RESX kultury zasobów do poprawnej wartości. Powiadomienie, że nie należy ręcznie ustawić tej wartości dla platformy uniwersalnej systemu Windows, ponieważ framework zasobów automatycznie rozpoznaje wybrany język na tych platformach.

```csharp
if (Device.RuntimePlatform == Device.iOS || Device.RuntimePlatform == Device.Android)
{
    var ci = DependencyService.Get<ILocalize>().GetCurrentCultureInfo();
    Resx.AppResources.Culture = ci; // set the RESX for resource localization
    DependencyService.Get<ILocalize>().SetLocale(ci); // set the Thread for locale-aware methods
}
```

Zasób `Culture` musi być ustawiona, gdy najpierw ładowania aplikacji używane są parametry odpowiedni język. Opcjonalnie możesz zaktualizować tę wartość w zależności od zdarzenia specyficzne dla platformy, które może zostać zgłoszone w systemie iOS lub Android, jeśli użytkownik aktualizuje swoje preferencje językowe, gdy aplikacja jest uruchomiona.

Implementacje dla `ILocalize` interfejsu są wyświetlane w [kod specyficzne dla platformy](#Platform-specific_Code) poniższej sekcji. Tych implementacji wykorzystać to `PlatformCulture` Klasa pomocy:

```csharp
public class PlatformCulture
{
    public PlatformCulture (string platformCultureString)
    {
        if (String.IsNullOrEmpty(platformCultureString))
        {
            throw new ArgumentException("Expected culture identifier", "platformCultureString"); // in C# 6 use nameof(platformCultureString)
        }
        PlatformString = platformCultureString.Replace("_", "-"); // .NET expects dash, not underscore
        var dashIndex = PlatformString.IndexOf("-", StringComparison.Ordinal);
        if (dashIndex > 0)
        {
            var parts = PlatformString.Split('-');
            LanguageCode = parts[0];
            LocaleCode = parts[1];
        }
        else
        {
            LanguageCode = PlatformString;
            LocaleCode = "";
        }
    }
    public string PlatformString { get; private set; }
    public string LanguageCode { get; private set; }
    public string LocaleCode { get; private set; }
    public override string ToString()
    {
        return PlatformString;
    }
}
```

<a name="Platform-specific_Code" />

### <a name="platform-specific-code"></a>Kod specyficzne dla platformy

Kod do wykrywania, które języka Aby wyświetlić musi być specyficzne dla platformy, ponieważ dla systemu iOS, Android i platformy uniwersalnej systemu Windows ujawnić te informacje w sposób nieco inne. Kod `ILocalize` zależności usługi są podane poniżej dla każdej platformy, wraz z dodatkowymi wymaganiami specyficzne dla platformy, aby upewnić się, zlokalizowany tekst jest renderowane poprawnie.

Kod specyficzne dla platformy musi obsługiwać także przypadki, w którym system operacyjny zezwala użytkownikowi na konfigurowanie identyfikator ustawień regionalnych, która nie jest obsługiwana przez. W sieci `CultureInfo` klasy. W takich przypadkach kod niestandardowy musi być przystosowana do wykrywania nieobsługiwane ustawienia regionalne i Zastąp najlepiej. Ustawienia regionalne zgodnych z sieci.

#### <a name="ios-application-project"></a>Projekt aplikacji systemu iOS

Użytkownicy systemu iOS wybierz preferowany język oddzielnie od kultury formatowania daty i godziny. Załadować poprawne zasobów do zlokalizowania aplikacji platformy Xamarin.Forms musimy zbadać `NSLocale.PreferredLanguages` tablicy dla pierwszego elementu.

Następujące wykonania `ILocalize` zależności usługi powinny być umieszczone w projekt aplikacji systemu iOS. Ponieważ iOS używa podkreślenia zamiast kreski (co jest reprezentacja standardowego .NET) kod zastępuje podkreślenie przed uruchamianiu `CultureInfo` klasy:

```csharp
[assembly:Dependency(typeof(UsingResxLocalization.iOS.Localize))]

namespace UsingResxLocalization.iOS
{
public class Localize : UsingResxLocalization.ILocalize
    {
        public void SetLocale (CultureInfo ci)
        {
            Thread.CurrentThread.CurrentCulture = ci;
            Thread.CurrentThread.CurrentUICulture = ci;
        }
        public CultureInfo GetCurrentCultureInfo ()
        {
            var netLanguage = "en";
            if (NSLocale.PreferredLanguages.Length > 0)
            {
                var pref = NSLocale.PreferredLanguages [0];
                netLanguage = iOSToDotnetLanguage(pref);
            }
            // this gets called a lot - try/catch can be expensive so consider caching or something
            System.Globalization.CultureInfo ci = null;
            try
            {
                ci = new System.Globalization.CultureInfo(netLanguage);
            }
            catch (CultureNotFoundException e1)
            {
                // iOS locale not valid .NET culture (eg. "en-ES" : English in Spain)
                // fallback to first characters, in this case "en"
                try
                {
                    var fallback = ToDotnetFallbackLanguage(new PlatformCulture(netLanguage));
                    ci = new System.Globalization.CultureInfo(fallback);
                }
                catch (CultureNotFoundException e2)
                {
                    // iOS language not valid .NET culture, falling back to English
                    ci = new System.Globalization.CultureInfo("en");
                }
            }
            return ci;
        }
        string iOSToDotnetLanguage(string iOSLanguage)
        {
            var netLanguage = iOSLanguage;
            //certain languages need to be converted to CultureInfo equivalent
            switch (iOSLanguage)
            {
                case "ms-MY":   // "Malaysian (Malaysia)" not supported .NET culture
                case "ms-SG":   // "Malaysian (Singapore)" not supported .NET culture
                    netLanguage = "ms"; // closest supported
                    break;
                case "gsw-CH":  // "Schwiizertüütsch (Swiss German)" not supported .NET culture
                    netLanguage = "de-CH"; // closest supported
                    break;
                // add more application-specific cases here (if required)
                // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
        string ToDotnetFallbackLanguage (PlatformCulture platCulture)
        {
            var netLanguage = platCulture.LanguageCode; // use the first part of the identifier (two chars, usually);
            switch (platCulture.LanguageCode)
            {
                case "pt":
                    netLanguage = "pt-PT"; // fallback to Portuguese (Portugal)
                    break;
                case "gsw":
                    netLanguage = "de-CH"; // equivalent to German (Switzerland) for this app
                    break;
                // add more application-specific cases here (if required)
                // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
    }
}
```

> [!NOTE]
> `try/catch` Bloki w `GetCurrentCultureInfo` metody imitują działanie rezerwowe najczęściej używane z ustawień regionalnych specyfikatory — Jeśli dokładnego dopasowania nie został znaleziony, poszukać dla zbliżona oparte tylko na langauge (pierwszy blok znaków w ustawieniach regionalnych).
>
> W przypadku platformy Xamarin.Forms, niektóre ustawienia regionalne są prawidłowe w systemie iOS, ale nie odpowiadają prawidłową `CultureInfo` .NET i kod nad próbuje rozwiązać.
>
> Na przykład iOS **Ustawienia > Ogólne języka &amp; Region** ekranu można ustawić telefonu **języka** do **angielski** , ale  **Region** do **Hiszpanii**, które powoduje ciąg ustawień regionalnych `"en-ES"`. Gdy `CultureInfo` tworzenia kończy się niepowodzeniem, kod przełączy się dwie pierwsze litery wybierz język wyświetlania.
>
> Należy zmodyfikować deweloperzy `iOSToDotnetLanguage` i `ToDotnetFallbackLanguage` metod obsługi określonych przypadków wymaganych do ich obsługiwanych języków.


Brak niektórych elementów interfejsu użytkownika zdefiniowane w systemie, które są automatycznie przetłumaczyła iOS, takie jak **gotowe** znajdującego się na `Picker` formantu. Aby wymusić iOS tłumaczenie tych elementów należy wskazać języki obsługujemy w **Info.plist** pliku. Można dodać te wartości za pośrednictwem **Info.plist > źródło** w sposób pokazany poniżej:

![Lokalizacja kluczy w pliku Info.plist](localization-images/info-plist.png "Lokalizacja kluczy w pliku Info.plist")

Można również otworzyć **Info.plist** plik w edytorze XML i bezpośredniego edytowania wartości:

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>de</string>
    <string>es</string>
    <string>fr</string>
    <string>ja</string>
    <string>pt</string> <!-- Brazil -->
    <string>pt-PT</string> <!-- Portugal -->
    <string>ru</string>
    <string>zh-Hans</string>
    <string>zh-Hant</string>
</array>
<key>CFBundleDevelopmentRegion</key>
<string>en</string>
```

Po zaimplementowana usługa zależności i zaktualizować **Info.plist**, aplikacji dla systemu iOS będą mogli wyświetlić zlokalizowanego tekstu.

> [!NOTE]
> Należy pamiętać, że Apple traktuje portugalski nieco inaczej, niż można oczekiwać.
> Z [ich docs](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW2): _"Użyj pt jako identyfikator języka dla portugalski ponieważ jest używany w Brazylia i pt-PT jako identyfikator języka dla portugalski, ponieważ jest on używany w Portugalii"_.
> Oznacza to, gdy język portugalski jest wybierany w niestandardowych ustawieniach regionalnych bazowy język będzie portugalski — Brazylia w systemach iOS, chyba że kod jest zapisywany zmienić to zachowanie (takie jak `ToDotnetFallbackLanguage` powyżej).

#### <a name="android-application-project"></a>Projekt aplikacji systemu android

Android udostępnia aktualnie wybranego ustawienia regionalne za pośrednictwem `Java.Util.Locale.Default`i używa również separatora podkreślenia zamiast myślnik (który jest zastępowany przez następujący kod). Dodaj tę implementację usługi zależności projekt aplikacji systemu Android:

```csharp
[assembly:Dependency(typeof(UsingResxLocalization.Android.Localize))]

namespace UsingResxLocalization.Android
{
    public class Localize : UsingResxLocalization.ILocalize
    {
        public void SetLocale(CultureInfo ci)
        {
            Thread.CurrentThread.CurrentCulture = ci;
            Thread.CurrentThread.CurrentUICulture = ci;
        }
        public CultureInfo GetCurrentCultureInfo()
        {
            var netLanguage = "en";
            var androidLocale = Java.Util.Locale.Default;
            netLanguage = AndroidToDotnetLanguage(androidLocale.ToString().Replace("_", "-"));
            // this gets called a lot - try/catch can be expensive so consider caching or something
            System.Globalization.CultureInfo ci = null;
            try
            {
                ci = new System.Globalization.CultureInfo(netLanguage);
            }
            catch (CultureNotFoundException e1)
            {
                // iOS locale not valid .NET culture (eg. "en-ES" : English in Spain)
                // fallback to first characters, in this case "en"
                try
                {
                    var fallback = ToDotnetFallbackLanguage(new PlatformCulture(netLanguage));
                    ci = new System.Globalization.CultureInfo(fallback);
                }
                catch (CultureNotFoundException e2)
                {
                    // iOS language not valid .NET culture, falling back to English
                    ci = new System.Globalization.CultureInfo("en");
                }
            }
            return ci;
        }
        string AndroidToDotnetLanguage(string androidLanguage)
        {
            var netLanguage = androidLanguage;
            //certain languages need to be converted to CultureInfo equivalent
            switch (androidLanguage)
            {
                case "ms-BN":   // "Malaysian (Brunei)" not supported .NET culture
                case "ms-MY":   // "Malaysian (Malaysia)" not supported .NET culture
                case "ms-SG":   // "Malaysian (Singapore)" not supported .NET culture
                    netLanguage = "ms"; // closest supported
                    break;
                case "in-ID":  // "Indonesian (Indonesia)" has different code in  .NET
                    netLanguage = "id-ID"; // correct code for .NET
                    break;
                case "gsw-CH":  // "Schwiizertüütsch (Swiss German)" not supported .NET culture
                    netLanguage = "de-CH"; // closest supported
                    break;
                    // add more application-specific cases here (if required)
                    // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
        string ToDotnetFallbackLanguage(PlatformCulture platCulture)
        {
            var netLanguage = platCulture.LanguageCode; // use the first part of the identifier (two chars, usually);
            switch (platCulture.LanguageCode)
            {
                case "gsw":
                    netLanguage = "de-CH"; // equivalent to German (Switzerland) for this app
                    break;
                    // add more application-specific cases here (if required)
                    // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
    }
}
```

> [!NOTE]
> `try/catch` Bloki w `GetCurrentCultureInfo` metody imitują działanie rezerwowe najczęściej używane z ustawień regionalnych specyfikatory — Jeśli dokładnego dopasowania nie został znaleziony, poszukać dla zbliżona oparte tylko na langauge (pierwszy blok znaków w ustawieniach regionalnych).
>
> W przypadku platformy Xamarin.Forms, niektóre ustawienia regionalne są prawidłowe w systemie Android, ale nie odpowiadają prawidłową `CultureInfo` .NET i kod nad próbuje rozwiązać.
>
> Należy zmodyfikować deweloperzy `iOSToDotnetLanguage` i `ToDotnetFallbackLanguage` metod obsługi określonych przypadków wymaganych do ich obsługiwanych języków.


Po dodaniu projekt aplikacji systemu Android ten kod będzie automatycznie wyświetlać przetłumaczone ciągi.

> [!NOTE]
>️ **Ostrzeżenie:** przetłumaczone ciągi pracy w kompilacjach wydania systemu Android, ale nie podczas debugowania, kliknij prawym przyciskiem myszy **projekt systemu Android** i wybierz **Opcje > kompilacji > dla systemu Android Tworzenie** i upewnij się, że **szybkie wdrożenie zestawu** nie zaznaczono. Ta opcja powoduje problemy podczas ładowania zasobów i nie powinna być używana, jeśli testujesz zlokalizowanych aplikacji.

#### <a name="universal-windows-platform"></a>Platforma uniwersalna systemu Windows

Uniwersalne projekty platformy systemu Windows (UWP) nie wymagają usługi zależności. Zamiast tej platformie automatycznie ustawia zasobu kultury poprawnie.

##### <a name="assemblyinfocs"></a>AssemblyInfo.cs

Rozwiń węzeł właściwości w projekcie przenośnej biblioteki klasy (PCL), a następnie kliknij dwukrotnie **AssemblyInfo.cs** pliku. Dodaj następujący wiersz do pliku, aby ustawić język asemblera neutralne zasoby język angielski:

```csharp
[assembly: NeutralResourcesLanguage("en")]
```

Informuje Menedżera zasobów kultury domyślnej aplikacji, dlatego zapewnienie, że ciągi zdefiniowane w pliku RESX niezależnym od języka (**AppResources.resx**) będą wyświetlane, gdy aplikacja jest uruchomiona w jednym języku angielskim ustawień regionalnych.

### <a name="example"></a>Przykład

Po aktualizacji projektów specyficzne dla platformy jak pokazano powyżej i ponownego kompilowania aplikacji z przetłumaczonego pliki RESX, tłumaczenia zaktualizowane będą dostępne w każdej aplikacji. Oto przykładowy kod przetłumaczony na język chiński uproszczony zrzut ekranu:

![](localization-images/simple-example-hans.png "Obsługujący wiele Platform UI translacji na chiński uproszczony")

## <a name="localizing-xaml"></a>Lokalizowanie elementów XAML

Podczas tworzenia znaczników interfejs użytkownika platformy Xamarin.Forms w języku XAML może wyglądać podobnie do poniższego, z ciągami osadzonych bezpośrednio w kodzie XML:

```xaml
<Label Text="Notes:" />
<Entry Placeholder="eg. buy milk" />
<Button Text="Add to list" />
```

W idealnym przypadku można przetłumaczyć formantów interfejsu użytkownika bezpośrednio w języku XAML, możemy tworząc *— rozszerzenie znaczników*. Poniżej przedstawiono kod rozszerzenia znaczników, który udostępnia zasoby RESX w języku XAML. Ta klasa powinny zostać dodane do typowy kod platformy Xamarin.Forms (wraz ze strony XAML):

```csharp
using System;
using System.Globalization;
using System.Reflection;
using System.Resources;
using Xamarin.Forms;
using Xamarin.Forms.Xaml;

namespace UsingResxLocalization
{
    // You exclude the 'Extension' suffix when using in XAML
    [ContentProperty("Text")]
    public class TranslateExtension : IMarkupExtension
    {
        readonly CultureInfo ci = null;
        const string ResourceId = "UsingResxLocalization.Resx.AppResources";

        static readonly Lazy<ResourceManager> ResMgr = new Lazy<ResourceManager>(
            () => new ResourceManager(ResourceId, IntrospectionExtensions.GetTypeInfo(typeof(TranslateExtension)).Assembly));

        public string Text { get; set; }

        public TranslateExtension()
        {
            if (Device.RuntimePlatform == Device.iOS || Device.RuntimePlatform == Device.Android)
            {
                ci = DependencyService.Get<ILocalize>().GetCurrentCultureInfo();
            }
        }

        public object ProvideValue(IServiceProvider serviceProvider)
        {
            if (Text == null)
                return string.Empty;

            var translation = ResMgr.Value.GetString(Text, ci);
            if (translation == null)
            {
#if DEBUG
                throw new ArgumentException(
                    string.Format("Key '{0}' was not found in resources '{1}' for culture '{2}'.", Text, ResourceId, ci.Name),
                    "Text");
#else
                translation = Text; // HACK: returns the key, which GETS DISPLAYED TO THE USER
#endif
            }
            return translation;
        }
    }
}
```

Następujące punktory opisano ważne elementy w powyższym kodzie:

* Klasy, nosi `TranslateExtension`, ale Konwencja firma Microsoft może się odwoływać do jest jako **Przetłumacz** w naszym znaczników.
* Implementuje klasy `IMarkupExtension`, wymagane przez platformy Xamarin.Forms go do pracy.
* `"UsingResxLocalization.Resx.AppResources"` jest identyfikatorem zasobu dla naszych zasobów RESX. Obejmuje naszych domyślnej przestrzeni nazw, folder, w którym znajdują się pliki zasobów i domyślną nazwę pliku RESX.
* `ResourceManager` Klasy jest tworzony przy użyciu `IntrospectionExtensions.GetTypeInfo(typeof(TranslateExtension)).Assembly)` można określić bieżącego zestawu do załadowania zasobów i pamięci podręcznej w statycznych `ResMgr` pola. Zostanie utworzona jako `Lazy` wpisz, aby jego utworzenia jest opóźnione aż najpierw jest używany w `ProvideValue` metody.
* `ci` korzysta z usługi zależności, aby uzyskać język wybranego użytkownika z natywnym systemem operacyjnym.
* `GetString` jest to metoda pobiera rzeczywisty ciąg przetłumaczonego z plików zasobów. Na platformie Windows Universal `ci` będzie równy null ponieważ `ILocalize` interfejsu nie jest zaimplementowana na tych platformach. Jest to odpowiednik wywołania `GetString` metody z pierwszym parametrem. Zamiast tego framework zasobów zostanie automatycznie rozpoznaje ustawień regionalnych i pobiera przetłumaczonego ciągu z odpowiedniego pliku RESX.
* Obsługa błędów zostało dołączone do debugowania brakuje zasobów przez Zgłaszanie wyjątku (w `DEBUG` tylko tryb).

Poniższy fragment kodu XAML pokazano, jak użyć rozszerzenia znaczników. Istnieją dwa kroki w celu zapewnienia pracy:

1. Deklarowanie niestandardowego `xmlns:i18n` przestrzeni nazw w węzła głównego. `namespace` i `assembly` musi być zgodna z ustawień projektu dokładnie — w tym przykładzie są identyczne, ale może być różna w projekcie.
2. Użyj `{Binding}` składni dla atrybutów, które zwykle zawiera tekst do wywołania `Translate` — rozszerzenie znaczników. Klucz zasobu jest tylko wymaganego parametru.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        x:Class="UsingResxLocalization.FirstPageXaml"
        xmlns:i18n="clr-namespace:UsingResxLocalization;assembly=UsingResxLocalization">
    <StackLayout Padding="0, 20, 0, 0">
        <Label Text="{i18n:Translate NotesLabel}" />
        <Entry Placeholder="{i18n:Translate NotesPlaceholder}" />
        <Button Text="{i18n:Translate AddButton}" />
    </StackLayout>
</ContentPage>
```

Następujące składni na pełniejsze również jest nieprawidłowy dla rozszerzenia znacznika:

```xaml
<Button Text="{i18n:TranslateExtension Text=AddButton}" />
```

## <a name="localizing-platform-specific-elements"></a>Lokalizowanie elementów specyficznych dla platformy

Mimo że firma Microsoft może obsługiwać tłumaczenie interfejsu użytkownika w kodzie platformy Xamarin.Forms, istnieją pewne elementy, które należy wykonać w każdym projekcie specyficzne dla platformy. W tej sekcji opisano sposób localize:

* Nazwa aplikacji
* Obrazy

Przykładowy projekt zawiera zlokalizowane obraz o nazwie **flag.png**, którego istnieje odwołanie w C# w następujący sposób:

```csharp
var flag = new Image();
switch (Device.RuntimePlatform)
{
    case Device.iOS:
    case Device.Android:
        flag.Source = ImageSource.FromFile("flag.png");
        break;
    case Device.UWP:
        flag.Source = ImageSource.FromFile("Assets/Images/flag.png");
        break;
}
```

Obraz flagi odwołuje się również w języku XAML w następujący sposób:

```xaml
<Image>
  <Image.Source>
    <OnPlatform x:TypeArguments="ImageSource">
        <On Platform="iOS, Android" Value="flag.png" />
        <On Platform="UWP" Value="Assets/Images/flag.png" />
    </OnPlatform>
  </Image.Source>
</Image>
```

Wszystkich platform automatycznie rozwiąże odwołań do obrazu do zlokalizowane wersje obrazów, takich jak wyjaśniono poniżej struktury projektu są implementowane.

### <a name="ios-application-project"></a>Projekt aplikacji systemu iOS

iOS używa standard nazewnictwa o nazwie Lokalizacja projektów lub **.lproj** katalogi do zawierają zasoby obrazu i ciąg. Te katalogi mogą zawierać zlokalizowane wersje obrazy używane w aplikacji, a także **InfoPlist.strings** pliku, który może służyć do zlokalizowania nazwy aplikacji.

#### <a name="images"></a>Obrazy

Ten zrzut ekranu przedstawia Przykładowa aplikacja dla systemu iOS z specyficzny dla języka **.lproj** katalogów. Hiszpański katalog o nazwie **es.lproj**, zawiera zlokalizowane wersje domyślnego obrazu, a także **flag.png**:

![](localization-images/ios-resources.png "Lokalizacja katalogu projektu systemu iOS")

Każdy katalog języka zawiera kopię **flag.png**zlokalizowaną dla tego języka. Jeśli obraz nie zostanie podany, system operacyjny będzie domyślnie obrazu w katalogu język domyślny. Aby uzyskać pełną pomoc siatkówki, należy określić **@2x** i **@3x** kopie każdego obrazu.

#### <a name="app-name"></a>Nazwa aplikacji

Zawartość **InfoPlist.strings** jest tylko pojedynczy klucz wartość do skonfigurowania Nazwa aplikacji:

```csharp
"CFBundleDisplayName" = "ResxEspañol";
```

Po uruchomieniu aplikacji, nazwy aplikacji i obrazu są oba zlokalizowane:

![](localization-images/ios-imageicon.png "iOS przykładowy tekst aplikację i Lokalizacja obrazu")

### <a name="android-application-project"></a>Projekt aplikacji systemu android

Android następuje inny schemat do przechowywania obrazów zlokalizowanych przy użyciu różnych **obiektów drawable** i **ciągów** katalogi z sufiksem kod języka. Jeśli kod ustawień regionalnych czterech list jest wymagany (takich jak zh-TW lub pt-BR), należy pamiętać, że system Android wymaga dodatkowego **r** następujących dash/poprzednie ustawienia regionalne kodu (np.) zh-rTW lub pt rBR).

#### <a name="images"></a>Obrazy

Ten zrzut ekranu przedstawia Android próbki z niektórych zlokalizowanych ciągów i drawables:

![](localization-images/android-resources.png "Android zlokalizowanych Drawables i katalogi ciągu")

Uwaga dla systemu Android nie używa zh-Hans i zh-Hant kodów uproszczony i języka chińskiego tradycyjnego; Zamiast tego obsługuje on tylko określonego kraju kody zh-CN i zh-TW.

Do obsługi obrazów innego rozwiązania dla ekrany o wysokiej gęstości, należy utworzyć foldery dodatkowych języków z `-*dpi` sufiksy, takich jak **drawables-es-mdpi**, **drawables-es-xdpi**, **drawables-es-xxdpi**itp. Zobacz [zapewnianie zasobów Android zamiast](http://developer.android.com/guide/topics/resources/providing-resources.html#AlternativeResources) Aby uzyskać więcej informacji.

#### <a name="app-name"></a>Nazwa aplikacji

Zawartość **strings.xml** jest tylko pojedynczy klucz wartość do skonfigurowania Nazwa aplikacji:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">ResxEspañol</string>
</resources>
```

Aktualizacja **MainActivity.cs** w projekcie aplikacji systemu Android, aby `Label` odwołuje się do ciągu XML.

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true,
        ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
```

Aplikacja jest teraz lokalizuje nazwy aplikacji i obrazów. Poniżej przedstawiono zrzut ekranu wyniku (w języku hiszpańskim):

![](localization-images/android-imageicon.png "Lokalizacja obrazu i tekstu aplikacji systemu android próbki")

### <a name="universal-windows-platform-application-projects"></a>Projekty aplikacji platformy uniwersalnej systemu Windows

Platforma uniwersalna systemu Windows ma infrastrukturę zasobu, która upraszcza Lokalizacja obrazów i nazwy aplikacji.

#### <a name="images"></a>Obrazy

Obrazy można lokalizować przez umieszczenie ich w folderze określonych zasobów, jak pokazano na poniższym zrzucie ekranu:

![](localization-images/uwp-image-folder-structure.png "Struktura folderów Lokalizacja obrazu platformy uniwersalnej systemu Windows")

W czasie wykonywania infrastruktury zasobów systemu Windows będzie wybierz odpowiedni obraz oparty na ustawienia regionalne użytkownika.

## <a name="summary"></a>Podsumowanie

Może być lokalizowany aplikacji platformy Xamarin.Forms przy użyciu pliki RESX i klasy globalizacji platformy .NET. Oprócz niewielki kod specyficzne dla platformy, aby wykryć użytkownika preferowany język większość wysiłków lokalizacja jest scentralizowane w typowy kod.

Ogólnie rzecz biorąc obsługi obrazów w sposób specyficzne dla platformy, aby móc korzystać z usługi rozpoznawania obsługę zarówno dla systemu iOS i Android. 

## <a name="related-links"></a>Linki pokrewne

- [Przykładowe lokalizacja RESX](https://developer.xamarin.com/samples/xamarin-forms/UsingResxLocalization/)
- [TodoLocalized przykładowej aplikacji](https://developer.xamarin.com/samples/xamarin-forms/TodoLocalized/)
- [Lokalizacja i Platform](~/cross-platform/app-fundamentals/localization.md)
- [Lokalizacja systemu iOS](~/ios/app-fundamentals/localization/index.md)
- [Lokalizacja systemu android](~/android/app-fundamentals/localization.md)
- [Używanie klasy CultureInfo (MSDN)](http://msdn.microsoft.com/library/87k6sx8t%28v=vs.90%29.aspx)
- [Lokalizuje i wykorzystania zasobów dla określonej kultury (MSDN)](http://msdn.microsoft.com/library/s9ckwb4b%28v=vs.90%29.aspx)
