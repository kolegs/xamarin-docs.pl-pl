---
title: "Kolor międzynarodowe"
description: "W tym artykule omówiono szeroki koloru i jak można w aplikacji platformy Xamarin.iOS lub Xamarin.Mac."
ms.topic: article
ms.prod: xamarin
ms.assetid: 576E978A-F182-489A-83E4-D8CDC6890B24
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: ab6124e2b11d26d4c10330e7b824e4761ebf4603
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="wide-color"></a>Kolor międzynarodowe

_W tym artykule omówiono szeroki koloru i jak można w aplikacji platformy Xamarin.iOS lub Xamarin.Mac._

iOS 10 i macOS Sierra podnosi poziom obsługi formatów pikseli rozszerzony zakres i całej gamy przestrzenie w całym systemie, w tym platform, takich jak Core grafiki, Core obrazu systemu operacyjnego i AVFoundation. Obsługa urządzeń z przedstawia szeroką kolor dalsze jest ułatwiony dostarczając to zachowanie przez cały stos całej grafiki.

## <a name="about-wide-color"></a>O szerokości kolorów

Jak już wspomniano, iOS 10 i macOS Sierra podnosi poziom obsługi formatów pikseli rozszerzony zakres i całej gamy przestrzenie w całym systemie, łącznie z platform, takich jak Core grafiki, Core obrazu systemu operacyjnego i AVFoundation. Obsługa urządzeń z przedstawia szeroką kolor dalsze jest ułatwiony dostarczając to zachowanie przez cały stos całej grafiki.

W lat 90-tych Apple utworzony ColorSync do obsługi kolor przetwarzania na komputerach Mac. One również pomogła znaleziono konsorcjum kolor międzynarodowych (ICC) do tworzenia i wspierania zestaw standardy obsługi kolor na sprzęcie komputerowym. Praca firmy Apple z ICC została włączona ColorSync i został skompilowany w podstawowe OS X (nazywane teraz macOS).

Apple również wynosiła co forefront technologii wyświetlania z sprzętu, takiego jak wyświetlić siatkówki, nowe P3 Wyświetl i wyświetlić P3 przestrzeń kolorów (wydane w iMac w 2015) i TrueTone wyświetla w dyskusjach iPad, iPhone 7 i iPhone 7 Plus.

Z szeroką kolor macOS Sierra i iOS 10 Apple zmienia sposób obsługi kolor, który w pełni korzystać z tych nowych technologii wyświetlana zarówno system macOS, jak i z systemem iOS.

## <a name="core-color-concepts"></a>Podstawowe koncepcje kolorów

Następujące podstawowe koncepcje kolor muszą zostać rozpatrzone przed zmianą głębiej spojrzeć na kolor macOS i iOS:

### <a name="color-space"></a>Przestrzeń kolorów

Przestrzeń kolorów to środowisko, w którym można reprezentowany i porównaniu kolorów. Można go przestrzeni trójwymiarowej 1 do 4 zdefiniowanego przez zwiększeniem jego składniki kolorów. 

[ ![](wide-color-images/color00.png "Przestrzeń kolorów")](wide-color-images/color00.png)

### <a name="color-channels"></a>Kanały kolorów

Składniki kolorów można także określane jako kanałów koloru. Niektóre znane oświadczenia będą RGB spacji, spacje szary, przestrzenie CMYK lub spacje niezależnie od urządzenia. 

[ ![](wide-color-images/color02.png "Składniki kolorów może być również określany jako kanałów koloru")](wide-color-images/color02.png)

### <a name="color-primaries"></a>Kolor kolory podstawowe

Kolor kolory podstawowe Podaj współrzędnych, który służy do porównywania i obliczeniowe kolorów. Kolor kolory podstawowe zazwyczaj dzielą się najwyżej intensywny wersję danego kolor, który można wygenerować w ramach kanału koloru.

[ ![](wide-color-images/color01.png "Kolor kolory podstawowe Podaj współrzędnych, który służy do porównywania i obliczeniowe kolorów")](wide-color-images/color01.png)

W przypadku reprezentowane powyżej przestrzeń kolorów RGB kolory podstawowe kolor jest where `1.0` jest zakotwiczona współrzędne (takich jak `[1.0, 0.0, 0.0]` czerwony).

### <a name="color-gamut"></a>Gama kolorów

Kolor przestrzeni odwołuje się do wszystkich kolorów, które mogą być definiowane jako kombinacja poszczególnych kanałów w zapewnia przestrzeń kolorów.

[ ![](wide-color-images/color03.png "Przykład przestrzeni kolorów")](wide-color-images/color03.png)

## <a name="what-is-wide-color"></a>Co to jest szeroki kolorów

Przed obejmujące temat kolor szerokości, należy uwzględnić dyskusji o bieżącym branżowy standard kolorów, standardowe przestrzeń kolorów RGB (sRGB). Jest najczęściej używana przestrzeń kolorów w obecnie obliczeniowych i jest domyślna przestrzeń kolorów dla systemów iOS i macOS.

Przestrzeń kolorów sRGB ma następujące właściwości:

- Jest on oparty na standardzie BT.709 ITU-R.
- Ma ona przybliżonej Gamma 2.2.
- Reprezentuje oświetlenia typowe warunki (D65).

Ponieważ sRGB jest to powszechnie używane w branży, deweloper wprowadzić pewne założenia który kolor określone zostanie wyrazami szacunku odwzorowane na dowolnym urządzeniu, jest wyświetlany na. Jednak to może nie zawsze być wielkość liter. Ponadto istnieje kilka kolorów, które nie mieszczą się w sRGB przestrzeń kolorów i, nie może być reprezentowany w nim.

Na przykład wiele tekstyliów zostały zaprojektowane z wątkami przy użyciu wielu farby i farby objętych poza sRGB. Również wiele produktów, które osoba napotka w ich codzienne są tworzone z jasny, żywe kolory, które są poza sRGB przestrzeń kolorów. Przykłady najbardziej atrakcyjnych kolorów, które nie mogą być reprezentowane w sRGB jest rzeczy charakter, takie jak sunsets, liście jesienią, egzotycznych kwiatów i tropikalnych wody.

Użytkownicy, którzy mają zostały przechwytywania obrazów cyfrowych w formacie NIEPRZETWORZONYM może mieć obrazów na ich urządzeniach, które zawierają wszystkie te dane koloru, mimo że nie można poprawnie przedstawić w sRGB przestrzeń kolorów, a do nich nie może być poprawnie wyświetlany na ekranie.

### <a name="the-display-p3-color-space"></a>Przestrzeń kolorów na wyświetlanie P3

W przypadku 2015 Apple wydawane nowych produktów (iMac i iPad Pro 9.7") zawierających nowe przestrzeń kolorów P3 wyświetlania do obsługi problemów utworzone przez sRGB przestrzeń kolorów.

[ ![](wide-color-images/color04.png "Nowe przestrzeń kolorów na wyświetlanie P3")](wide-color-images/color04.png)

Kolor P3 ekran ma następujące właściwości:

- Obsługuje przestrzeń kolorów szerokości dla platform nowoczesnego sprzętu.
- Oparte na standardzie SMPTE DCI-P3. DCI P3 została zaprojektowana dla projektorów cyfrowego, ale została zmodyfikowana przez firmę Apple do obsługi monitorów.
- Ma ona przybliżonej Gamma 2.2.
- Reprezentuje oświetlenia typowe warunki (D65).

Zgodnie z firmy Apple użytkownicy są przeniesienie ich przepływów pracy do ich platform urządzeń przenośnych. Rozwiązywania problemów prezentacji kolorów, przedstawiony przez sRGB w professional urządzeń przenośnych (iPad specjaliści) więcej niż tylko tym wyświetlania szeroki kolorów. Jednym z rozwiązań było uaktualnić odwzorowania fabryki, więc poszczególnych urządzeń ma zostać kalibrować fabryki zapewnienie, że z urządzenia do urządzenia, wyświetlania kolorów są dokładne i spójne.

Innym rozwiązaniem, jest uwzględnienie zarządzania kolor pełne, całego systemu Apple ma wbudowaną funkcję iOS 10 i macOS Sierra. 

### <a name="the-extended-range-srgb-color-space"></a>Rozszerzony zakres sRGB przestrzeń kolorów

Nowe zarządzania kolor całego systemu iOS 10 musi konta dla wszystkich istniejących aplikacji dla systemu iOS, które są wbudowane i dopracowaniu dla sRGB. Została zaprojektowana w celu zapewnienia, że nie wpłynie albo kolor aplikacji lub reprezentacja wydajności tych istniejących aplikacji.

Aby obsłużyć taką sytuację, Apple była dostępna przestrzeń kolorów sRGB rozszerzony zakres w systemie iOS 10 (i również macOS Sierra).

Przestrzeń kolorów sRGB rozszerzony zakres ma następujące właściwości:

- Zawiera wszystkie tego samego sRGB kolory podstawowe.
- Ma ona przybliżonej Gamma 2.2.
- Reprezentuje oświetlenia typowe warunki (D65).
- Obsługuje ujemnych wartości, a wartości większej niż jeden (1).

Przy pozwalającą na wartości mniejsze od zera i większy niż jeden, sRGB rozszerzony zakres, który przestrzeń kolorów umożliwia nie tylko istniejących aplikacji do kolorów obecnych w sRGB bez trafienia wydajności lub zakłócenia, ale umożliwia przestrzeń kolorów do reprezentowania kolorów wewnątrz widoczne spektrum. Wszystko to odbywa się jednocześnie zachowując tego samego punktów kontrolnych jako sRGB przestrzeń kolorów.

### <a name="extended-range-srgb-in-action"></a>Rozszerzone sRGB zakresu w akcji

Aby zobaczyć, jak wartości spoza zero i co działa w sRGB rozszerzony zakres przestrzeń kolorów, należy wykonać poniższy przykład przedstawia najbardziej nasycenia czerwonego dostępne w przestrzeni wyświetlania P3 kolorów:

[ ![](wide-color-images/color05.png "Jak działa wartości spoza zero i jeden w sRGB rozszerzony zakres przestrzeń kolorów")](wide-color-images/color05.png)

W P3 wyświetlania tego koloru jest przedstawiany jako `[1.0, 0.0, 0.0]` w sRGB rozszerzony zakres byłoby `[1.358, -0.074, -0.012]`. Ponieważ sRGB wartości są pełne zawartych wewnątrz P3 wyświetlania i wartości wyświetlania P3 leżą "poza" sRGB zakresów.

Sprzętem fizycznym, który zezwala na przechodzenie od extreme dodatni do extreme wartości ujemnych wartości pikseli można wyświetlić wszystkie dostępne widma kolorów i te wartości są przedstawiane sRGB rozszerzony zakres przestrzeń kolorów.

### <a name="device-pixel-formats"></a>Formatów pikseli urządzenia 

SRGB przestrzeń kolorów został przede wszystkim normalizacji przy użyciu formatu piksela 8-bitowych, ponieważ 8-bitów na kanał kolor jest przeważnie wystarczającej do opisywania kolorów w sRGB. Nie jest to idealne, ale wystarczająco dobra i stanowi dobry zależności między użycia pamięci i procesora na wyświetlanie obrazów.

Ponieważ P3 wyświetlania może reprezentować współrzędne kolorów poza przestrzeń kolorów sRGB, wymaga 16-bitów na kanał kolorów, aby poprawnie sformatowane informacje i przestrzeń kolorów sRGB rozszerzony zakres kolorów.

## <a name="system-wide-wide-color-support"></a>Obsługa koloru szeroki systemowe

Aby w pełni obsługiwać szeroki koloru i szerokiej gamy wewnątrz iOS 10 i macOS Sierra, Apple ma rozszerzone następujące struktury zalet sRGB rozszerzony zakres przestrzeń kolorów i P3 wyświetlania:

- UIKit (dla tylko iOS)
- SceneKit
- Podstawowe grafiki
- ImageIO
- Obraz Core
- WebKit
- SpriteKit
- Animacja Core
- AppKit (dla macOS tylko)

Ponadto wyświetlana siatkówki została rozszerzona obsługa dla zakresu rozszerzone sRGB przestrzeń kolorów i wyświetlania P3 Wyświetla.

Kolor Wide jest obsługiwane i mogą być używane w następujących typów zawartości aplikacji:

- Obraz statyczny zasobów zawartych w pakiecie aplikacji.
- Dokument i sieci na podstawie zasobów obrazu.
- Zaawansowane nośniki, takie jak zdjęcia na żywo lub obrazów w formacie NIEPRZETWORZONYM.
- Obrazy tekstury programu do cieniowania 3D grafiki.

## <a name="solving-the-color-problem"></a>Rozwiązywanie problemu kolorów

Zawartość wyświetlana w aplikacji mogą pochodzić pochodzące z różnorodnych źródeł sformatowanego kolorów. Ponadto można wyświetlić tej zawartości na szeroki zakres urządzeń, każdy z własnych zakresem możliwości wyświetlania kolorów.

Aplikacja systemu iOS 10 stanowi różnica między obu problemów za pomocą nowego wbudowanych, systemowe kolor zarządzania systemu. Ten system gwarantuje, że obraz wygląda tak samo na dowolnym urządzeniu z systemem iOS, niezależnie od tego, w których przestrzeń kolorów obrazu został zakodowany w.

Zarządzanie kolorami rozpoczyna się od każdego obrazu skojarzona przestrzeń kolorów (lub profilu kolorów). Te informacje są używane w _kolor dopasowywania procesów_ gdzie kolorów obrazu źródłowego są dopasowywane do kolorów urządzenia wyjściowego. Ponieważ każdy piksel obrazu musi być dopasowane kolorów, go zająć dużo czasu i umieścić naprężenia po Procesora urządzenia.

Ze względu na specyfikę z _kolor dopasowywania procesów_, ta konwersja może być potencjalnie "stratnej" Jeśli urządzenia wyjściowego ma mniejszy przestrzeni niż obrazu źródłowego.

Na szczęście obliczenia, które wchodzą w _kolor dopasowywania procesów_ można łatwo można przyspieszane (albo w procesora CPU i procesora GPU) i Apple gwarantuje, że działa on automatycznie według budynków pomocy technicznej do podstawowej systemów, takich jak 2D kwarcowy ColorSync i Core animacji. Odpowiednio oznakowane zawartości kodowania nie jest wymagany mógł korzystać z tych funkcji.

Zarządzanie kolorami obsługiwanego na każdej z platform w następujący sposób:

- **System macOS** -macOS został zaznaczony zarządzane od momentu rozpoczęcia.
- **iOS** -iOS obsługiwał zarządzania kolorowi automatycznemu począwszy od zestawu iOS 9.3 (i nowszych).

### <a name="designing-for-wide-gamut"></a>Projektowanie dla szerokiej gamy

Przy użyciu szerokiej kolorów szerokiej gamy zawartości obrazu w aplikacjach systemu iOS i macOS i Apple ma następujące uwag dotyczących projektowania:

- Szerokiej gamy zawartości należy używać tylko w przypadku gdy w upewnij znaczeniu w aplikacji i ich nie automatycznie należy używać wszędzie.
- Należy używać tylko zawartość szerokiej gamy gdzie żywe kolory będzie ulepszanie środowiska użytkownika.
- W nie jest konieczna zmiana całą zawartość do P3 dla istniejących aplikacji.

Łańcuch narzędzi firmy Apple sprawia, że obsługi zawartości obrazu szerokiej gamy stopniowego zdecydować się na, dlatego obsługa szeroki kolorów w aplikacji nie jest all-or-nothing sytuacji.

### <a name="upgrading-existing-content-to-wide-color"></a>Uaktualnianie istniejącej zawartości do szerokości kolorów

Apple ma poniższe sugestie dotyczące uaktualniania istniejącego obrazu szeroki kolor:

- Nie tylko "przypisywanie" P3 profilu do zawartości w aplikacji do edycji obrazów. Dzięki temu po prostu ponownie zamapować kolor istniejącej zawartości do nowego miejsca kolor nieoczekiwane wyniki, takie jak rozciąganie kolory pasuje do nowego miejsca, w związku z tym zmienianie obrazu.
- Zawartości obrazu należy "skonwertowania" do profilu wyświetlana P3 za pomocą aplikacji do edycji obrazów.

### <a name="file-formats-and-color-profiles"></a>Formaty plików i profile kolorów

Apple ma następujące sugestie dotyczące formaty plików i profile kolorów używany w aplikacji szeroki kolorów obrazu zawartości:

- Za pomocą profilu kolor "Wyświetlania P3" RGB pracy spacji.
- Użyj 16-bitowych na tryb kanału kolorów.
- Użyj iMac najpóźniejsze 2015 (lub nowsza) aby prawidłowo wyświetlić podgląd obrazu.
- Eksportuj zasoby obrazu jako 16-bitowe pliki PNG z osadzonym profilem ICC "Wyświetlania P3".

> [!IMPORTANT]
> **Uwaga:** Using **zapisać dla sieci Web** lub **Eksportuj zasoby** funkcje znalezione w najbardziej popularnych oprogramowanie do edycji obrazów _nie_ nadają się do szerokości obrazy od te funkcje nie zostały zaktualizowane do obsługi jeszcze specyfikacje formatu wymaganego pliku.

### <a name="supporting-wide-color-with-asset-catalogs"></a>Obsługa szeroki koloru z katalogami zasobów

IOS 10 i macOS Sierra Apple rozwinął katalogi zasobów używany do uwzględnienia, a kategoryzowania statyczny obraz zawartości w pakiecie aplikacji obsługuje szeroką koloru.

Przy użyciu katalogi zasobów zapewniają następujące korzyści dla aplikacji:

- Udostępniają one najlepszych opcji wdrażania zasobów obraz statyczny.
- Obsługują one automatycznej korekcji kolorów.
- Obsługują one optymalizacji format piksela automatycznego.
- Obsługują one dzielenie aplikacji i spłaszczenia aplikacji, dzięki czemu tylko zawartość jest istotne Pobierz używany do urządzenia użytkownika końcowego.

Apple wprowadził następujące ulepszenia do zawartości katalogów obsługę szerokiej kolorów:

- Obsługuje zawartość źródłową 16-bitowych (na kanału koloru).
- Obsługują one przyspieszy zawartości przez wyświetlania przestrzeni. Zawartość można oznakowane sRGB lub one P3 wyświetlania zakresami.

Deweloper ma trzy opcje do obsługi zawartości szeroki kolorów w swoich aplikacjach:

1. **Nic nie rób** — ponieważ kolor szerokości zawartości należy używać tylko w sytuacji, gdy aplikacja potrzebuje do wyświetlenia żywe kolory (gdzie one zwiększy przez użytkownika), należy pozostawić zawartość poza to wymaganie jako — jest. Nadal będzie można poprawnie wyrenderować w wszystkich sytuacji sprzętu.
2. **Uaktualnij istniejącą zawartość do wyświetlania P3** -wymaga deweloperowi Zastąp istniejącej zawartości obrazu w katalogu ich zasobów uaktualnionego nowy plik w formacie P3 i oznaczyć ją jako takie w edytorze zasobów. W czasie kompilacji zostanie wygenerowany obraz pochodnych sRGB z tych zasobów.
3. **Podaj zoptymalizowanych pod kątem zawartości zasobów** — w takim przypadku dewelopera zapewni zarówno sRGB 8-bitowych, jak i 16-bitowych wizji P3 wyświetlania każdego obrazu w katalogu zasobów (oznakowany prawidłowo w edytorze zasobów).

### <a name="asset-catalog-deployment"></a>Wdrożenie katalogu zasobów

Następujące ma miejsce, gdy dewelopera przesyła aplikacji do sklepu z aplikacjami z katalogami zasobów, obejmujących szeroki kolorów obrazu:

- Gdy aplikacja jest wdrażana dla użytkownika końcowego, tworzenia wycinków aplikacji zapewnia dostarczenie odpowiedniej zawartości variant do urządzenia użytkownika.
- Na urządzeniu, które nie obsługują międzynarodowe koloru nie ma żadnych kosztów ładunek dla szerokości kolor zawartości, w tym, jako nigdy nie jest dostarczony na urządzeniu.
- `NSImage` na macOS Sierra (i nowszych) automatycznie wybiera najlepsze reprezentacja zawartości do wyświetlenia sprzętu.
- Wyświetlona zawartość będzie odświeżana automatycznie, gdy lub sprzętu urządzeń wyświetlić zmiany właściwości.

### <a name="asset-catalog-storage"></a>Magazyn katalogu zasobów

Magazyn w katalogu zasobów ma poniższe właściwości i ich wpływ na aplikację:

- W czasie kompilacji Apple próbuje zoptymalizować magazyn zawartości obrazu za pomocą konwersji obrazu wysokiej jakości.
- 16-bitowej są używane na kolor kanał kolor szerokości zawartości.
- Kompresja zawartości obrazu zależnych służy do obniżyć dostarczanego rozmiarów zawartości. Nowe kompresji "stratnej" zostały dodane do jeszcze bardziej zoptymalizować rozmiarów zawartości.

## <a name="colors-in-user-interfaces"></a>Kolory interfejsy użytkownika

Podczas pracy z kolorów w interfejsie użytkownika, większość pikseli na ekranie są w jednolitym kolorem. Ponadto większość tych pikseli nie pochodzą od zasoby obrazów, ale są rysowane bezpośrednio przez aplikację (lub przez system operacyjny w imieniu aplikacji).

Kolor szerokiej gamy stanowi wyzwanie, kilka podczas pracy na poziomie interfejsu użytkownika:

- **Komunikacja kolory** — w przypadku kolorów projektanci i deweloperzy zazwyczaj _zakłada, że_ sRGB zaangażowany przestrzeń kolorów. Dlatego kolor mogą być przekazywane jako `rgb(128, 45, 56)` lub `#FF0456`. W projekcie szerokiej gamy te oświadczenia nie zawiera informacji wystarczających do przedstawiać podany kolor, również muszą być uwzględnione pracy przestrzeń kolorów. Sugeruje Apple przy użyciu `P3(128, 45, 56)` i `P3#FF0456` zamiast tego. 
- **Pobieranie kolorów** — większość popularnych obraz edycji i projektowania oprogramowania boryka się z te same ograniczenia co przestrzeń kolorów sRGB używając ich próbników kolorów wbudowanych. Projektant powinien zapewnienia, że są w obszarze "Wyświetlania P3" kolor próbnika kolorów podczas pracy z projektami szeroki kolorów.
- **Kodowanie kolorów** — zarówno `NSColor` (macOS) i `UIColor` (z systemem iOS & systemu tvOS) mają nowych metod wygody generowania kolorów P3 bezpośrednio i oba zostały rozszerzone do obsługi kolorów w rozszerzonym zakresem przestrzeń kolorów sRGB również.
- **Przechowywanie kolory** -ostrożność podczas przechowywania szerokiej gamy kolory w dokumencie aplikacji. W przypadku, gdy 8 bitów na kanał kolor działały prawidłowo dla przestrzeń kolorów sRGB 16-bitowych na kolor kanał powinien być używany dla szerokiego kolorów. Deweloper musi również uwagę na wystąpienia zakładanego przestrzeń kolorów (ponieważ wszystko tradycyjnie była tylko sRGB).

## <a name="colors-on-the-web"></a>Kolory w sieci Web

Należy zachować ostrożność podczas pracy z szeroką kolor na stronach sieci web i na urządzeniach, które obsługują wyświetlanie szerokiego kolor. Jeśli całej zawartości obrazu, która została uwzględniona w witrynie zostały odpowiednio oznakowane, iOS i macOS automatycznie kolor dopasowania i ich poprawnie wyświetlić.

Zapytaniami multimediów są także dostępne w celu rozwiązania zasobów między P3 i sRGB urządzeń obsługujących:

```xml
<picture>
    <source srcset="monkey-p3.jpg" media="(color-gamut: p3)">
    <source srcset="monkey-rpg.jpg">
</picture>
```

Apple ma również propozycji WebKit, która umożliwi CSS, należy określić w innych przestrzenie oprócz miejsca zakładanego sRGB.

## <a name="rendering-off-screen-images-in-app"></a>Renderowanie obrazów ukrytej w aplikacji

Na podstawie typu aplikacji, tworzony, może mieć od użytkowników zawartości obrazu zostały pobrane z Internetu lub utworzyć obraz zawartości bezpośrednio wewnątrz aplikacji (takich jak wektorowa, na przykład rysowania aplikacji).

W obu przypadkach aplikacji umożliwiający renderowanie obrazów wymaganych jako ukrytej w kolorze szerokości przy użyciu ulepszone funkcje dodane do systemu iOS oraz macOS.

### <a name="drawing-wide-color-in-ios"></a>Rysowanie szeroki kolorów w systemie iOS

Przed omówieniem jak poprawnie narysować obrazu szeroki kolorów w systemie iOS 10, Przyjrzyjmy się następujące typowe kod rysowania iOS:

```csharp
public UIImage DrawWideColorImage ()
{
    var size = new CGSize (250, 250);
    UIGraphics.BeginImageContext (size);

    ...
    
    UIGraphics.EndImageContext ();
    return image = UIGraphics.GetImageFromCurrentImageContext ();
}
```

Występują problemy ze standardowego kodu, które należy rozważyć _przed_ może służyć do rysowania obrazu szeroki kolorów. `UIGraphics.BeginImageContext (size)` Metody używane do uruchamiania rysowania obrazu systemu iOS ma następujące ograniczenia:

- Konteksty obrazu nie może utworzyć z więcej niż 8 bitów na kanał koloru.
- Nie może on reprezentować kolorów w sRGB rozszerzony zakres przestrzeń kolorów.
- Nie ma możliwości zapewnia interfejs do tworzenia kontekstów w miejscu Color sRGB z systemem innym niż z powodu niskiego poziomu procedury C wywoływana w tle.

Aby obsługiwać te ograniczenia i rysowania obrazu szeroki kolorów w systemie iOS 10, należy użyć następującego kodu:

```csharp
public UIImage DrawWideColorImage ()
{
    var size = new CGSize (250, 250);
    var render = new UIGraphicsImageRenderer (size);

    var image = render.CreateImage ((UIGraphicsImageRendererContext context) => {
        var bounds = context.Format.Bounds;
        CGRect slice;
        CGRect remainder;
        bounds.Divide (bounds.Width / 2, CGRectEdge.MinXEdge, out slice, out remainder);

        var redP3 = UIColor.FromDisplayP3 (1.0F, 0.0F, 0.0F, 1.0F);
        redP3.SetColor ();
        context.FillRect (slice);

        var redRGB = UIColor.Red;
        redRGB.SetColor ();
        context.FillRect (remainder);
    });

    // Return results
    return image;
}
```

Nowe `UIGraphicsImageRenderer` klasy tworzy nowy kontekst obrazu, który może obsługiwać sRGB rozszerzony zakres przestrzeń kolorów i ma następujące funkcje:

- Pełni jest zarządzane przez domyślny kolor.
- Obsługuje ona domyślnie przestrzeń kolorów sRGB rozszerzony zakres.
- Decyduje inteligentnie, jeśli mają renderować sRGB lub sRGB rozszerzony zakres przestrzeń kolorów na możliwości urządzenia z systemem iOS, w którym aplikacja jest uruchomiona na podstawie.
- W pełni i automatycznie zarządza kontekstu obrazu (`CGContext`) okresu istnienia, więc deweloper nie trzeba się martwić o wywołania rozpoczęcia i zakończenia polecenia kontekstu.
- Jest on zgodny z `UIGraphics.GetCurrentContext()` metody.

`CreateImage` Metody `UIGraphicsImageRenderer` klasy jest wywoływana w celu utworzenia obrazu szeroki kolorów i przekazany z kontekstem obrazu do rysowania do obsługi uzupełniania. Wszystkie rysunku odbywa się wewnątrz tego programu obsługi zakończenia w następujący sposób:

- `UIColor.FromDisplayP3` Metoda tworzy nowy kolor czerwony pełni nasycenia w szerokiej gamy przestrzeń kolorów P3 wyświetlania i jest używany do rysowania pierwszej połowy prostokąta. 
- Druga połowa prostokąta jest rysowana w normalnym sRGB pełni przepełniony kolor czerwony dla porównania.

### <a name="drawing-wide-color-in-macos"></a>Rysowanie szeroki kolorów w macOS

`NSImage` Klasa została rozszerzona w macOS Sierra do obsługi rysowania kolor szerokości obrazów. Na przykład:

```csharp
var size = CGSize(250,250);
var wideColorImage = new NSImage(size, false, (drawRect) =>{
    var rects = drawRect.Divide(drawRect.Size.Width/2,CGRect.MinXEdge);
    
    var color = new NSColor(1.0, 0.0, 0.0, 1.0).Set();
    var path = new NSBezierPath(rects.Slice).Fill();
    
    color = new NSColor(1.0, 0.0, 0.0, 1.0).Set();
    path = new NSBezierPath(rects.Remainder).Fill();
    
    // Return modified
    return true;
});
```

## <a name="rendering-on-screen-images-in-app"></a>Na ekranie renderowanie obrazów w aplikacji

Do renderowania na ekranie szeroki obrazy, proces działa podobnie do rysowania obrazu ukrytej kolor szerokości dla macOS i iOS przedstawionych powyżej.

### <a name="rendering-on-screen-in-ios"></a>Na ekranie renderowania w systemie iOS

Gdy aplikacja potrzebuje by renderować obraz w szerokiej kolor na ekranie w systemie iOS, należy zastąpić `Draw` metody `UIView` dane w zwykły sposób. Na przykład:

```csharp
using System;
using UIKit;
using CoreGraphics;

namespace MonkeyTalk
{
    public class MonkeyView : UIView
    {
        public MonkeyView ()
        {
        }

        public override void Draw (CGRect rect)
        {
            base.Draw (rect);

            // Draw the image to screen
            ...
        }
    }
}
``` 

Tak jak w przypadku systemu iOS 10 z `UIGraphicsImageRenderer` pokazanym powyżej inteligentnie decyduje, jeśli mają renderować sRGB lub sRGB rozszerzony zakres kolorów obszar, w oparciu o możliwości urządzenia z systemem iOS, które aplikacja jest uruchomiona, gdy klasa `Draw` metoda jest wywoływana. Ponadto `UIImageView` został zaznaczony zarządzane od również iOS 9.3.

Jeśli aplikacja musi wiedzieć, jak renderowania jest wykonywana na `UIView` lub `UIViewController`, można sprawdzić nowy `DisplayGamut` właściwość `UITraitCollection` klasy. Ta wartość będzie `UIDisplayGamut` wyliczenia z następujących czynności:

- P3
- SRGB
- Nieokreślony

Jeśli aplikacja chce kontrolować, które przestrzeń kolorów jest używany do rysowania obrazu, można użyć nowego `ContentsFormat` właściwość `CALayer` określić żądaną przestrzeń kolorów. Ta wartość może być `CAContentsFormat` wyliczenia z następujących czynności:

- Gray8Uint
- Rgba16Float
- Rgba8Uint

### <a name="rendering-on-screen-in-macos"></a>Na ekranie renderowania w macOS

Gdy aplikacja potrzebuje by renderować obraz w szerokiej kolor na ekranie w macOS, Zastąp `DrawRect` metody `NSView` dane w zwykły sposób. Na przykład:

```csharp
using System;
using AppKit;
using CoreGraphics;
using CoreAnimation;

namespace MonkeyTalkMac
{
    public class MonkeyView : NSView
    {
        public MonkeyView ()
        {
        }

        public override void DrawRect (CGRect dirtyRect)
        {
            base.DrawRect (dirtyRect);

            // Draw graphics into the view
            ...
        }
    }
}
```

Ponownie go inteligentnie decyduje o tym, jeśli mają renderować sRGB lub sRGB rozszerzony zakres kolorów obszar, w oparciu o możliwości sprzętu Mac, która aplikacja jest uruchomiona, gdy `DrawRect` metoda jest wywoływana.

Jeśli aplikacja chce kontrolować, które przestrzeń kolorów jest używany do rysowania obrazu, można użyć nowego `DepthLimit` właściwość `NSWindow` klasy, aby określić wymaganą ilość miejsca kolorów. Ta wartość może być `NSWindowDepth` wyliczenia z następujących czynności:

- OneHundredTwentyEightBitRgb
- SixtyfourBitRgb
- TwentyfourBitRgb

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego szeroki koloru i sposób, że mogą być wdrożone i używany wewnątrz aplikacji platformy Xamarin.iOS lub Xamarin.Mac.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
