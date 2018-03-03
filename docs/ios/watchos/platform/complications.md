---
title: Komplikacji
description: "watchOS umożliwia deweloperom Pisanie niestandardowych komplikacji dla powierzchni czujki"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7ACD9A2B-CF69-46EA-B0C8-10E7D81216E8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/03/2017
ms.openlocfilehash: a13de7fbb4b6e1f9fa2853ce599f3a038a5e4040
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="complications"></a>Komplikacji

_watchOS umożliwia deweloperom Pisanie niestandardowych komplikacji dla powierzchni czujki_

Ta strona opisano różne typy dostępnych komplikacji oraz sposobu dodawania complication do aplikacji watchOS 3.

Należy pamiętać, że każda aplikacja watchOS może mieć tylko jeden complication.

Rozpocznij od przeczytania [dokumentów firmy Apple](https://developer.apple.com/library/watchos/documentation/General/Conceptual/WatchKitProgrammingGuide/ManagingComplications.html) do ustalenia, czy aplikacja jest odpowiedni dla complication. Istnieje 5 `CLKComplicationFamily` typami wyświetlania do wyboru:

[ ![](complications-images/all-complications-sml.png "5 dostępnych typów CLKComplicationFamily: cykliczne małej, moduły małych, moduły dużych, użytkowych małych, użytkowych duże")](complications-images/all-complications.png)

Aplikacje można zaimplementować w stylu tylko jednego lub wszystkich pięciu, w zależności od danych będzie wyświetlany.
Można również obsługiwać czasu podróży, dostarczając wartości dla ostatnich i/lub przyszłych godzin zgodnie z wyświetlająca cyfrowe wierzchołek.

<a name="adding" />

## <a name="adding-a-complication"></a>Dodawanie Complication

### <a name="configuration"></a>Konfiguracja

Komplikacji mogą być dodawane do aplikacji czujki podczas tworzenia lub ręcznie dodawane do istniejącego rozwiązania.

### <a name="add-new-project"></a>Dodaj nowy projekt...

**Dodać nowy projekt...**  Kreator zawiera pole wyboru, które automatycznie utworzy klasę kontrolera complication i skonfigurować **Info.plist** pliku:

![](complications-images/file-new-project-sml.png "Pole wyboru obejmują Complication")

### <a name="existing-projects"></a>Istniejące projekty

Aby dodać complication do istniejącego projektu:

1. Utwórz nową **ComplicationController.cs** pliku klasy i wdrożenie `CLKComplicationDataSource`.
2. Skonfiguruj aplikację **Info.plist** do udostępnienia complication i tożsamości grupy complication, które są obsługiwane.

Te kroki są opisane bardziej szczegółowo poniżej.

<a name="clkcomplicationcontroller" />

### <a name="clkcomplicationdatasource-class"></a>Klasa CLKComplicationDataSource

Następujący szablon języka C# zawiera minimalne wymagane metody służące do implementacji `CLKComplicationDataSource`.

```csharp
[Register ("ComplicationController")]
public class ComplicationController : CLKComplicationDataSource
{
  public ComplicationController ()
  {
    }
  public override void GetPlaceholderTemplate (CLKComplication complication, Action<CLKComplicationTemplate> handler)
    {
    }
  public override void GetCurrentTimelineEntry (CLKComplication complication, Action<CLKComplicationTimelineEntry> handler)
    {
    }
  public override void GetSupportedTimeTravelDirections (CLKComplication complication, Action<CLKComplicationTimeTravelDirections> handler)
    {
    }
}
```

Postępuj zgodnie z [zapisywania complication](#writing) instrukcjami, aby dodać kod do tej klasy.

### <a name="infoplist"></a>Info.plist

Rozszerzenie czujki **Info.plist** pliku należy określić nazwę `CLKComplicationDataSource` i grupy, które complication chcesz wspierać:

[ ![](complications-images/complications-config-sml.png "Typy rodziny complication")](complications-images/complications-config.png)

**Klasy źródła danych** listy wpisów wyświetli nazwy klas tego podklasy `CLKComplicationDataSource` podklasy, który zawiera logikę complication.

## <a name="clkcomplicationdatasource"></a>CLKComplicationDataSource

Wszystkie funkcje complication jest zaimplementowana w jednej klasy, zastępowanie metody `CLKComplicationDataSource` klasy abstrakcyjnej (które implementuje `ICLKComplicationDataSource` interfejs).

### <a name="required-methods"></a>Metody wymagane

Musi implementować następujące metody complication do uruchomienia:

- `GetPlaceholderTemplate` -Zwróć wyświetlanie statyczne, używane podczas konfigurowania lub aplikacja nie może dostarczyć wartości.
- `GetCurrentTimelineEntry` -Obliczyć prawidłowe wyświetlanie uruchomionej complication.
- `GetSupportedTimeTravelDirections` -Zwraca opcje z `CLKComplicationTimeTravelDirections` takich jak `None`, `Forward`, `Backward`, lub `Forward | Backward`.

### <a name="privacy"></a>Ochrona prywatności

Komplikacji, które zawierają dane osobowe

* `GetPrivacyBehavior` - `CLKComplicationPrivacyBehavior.ShowOnLockScreen` lub `HideOnLockScreen`

Jeśli ta metoda zwraca `HideOnLockScreen` complication wyświetli albo ikonę lub nazwę aplikacji (i nie wszystkie dane), a następnie gdy wyrażenie kontrolne jest zablokowane.

### <a name="updates"></a>Aktualizacje

- `GetNextRequestedUpdateDate` -Return czasu, gdy system operacyjny powinien obok zapytania aplikacji dla zaktualizować complication wyświetlania danych.

Można również wymusić aktualizację z aplikacji systemu iOS.

### <a name="supporting-time-travel"></a>Obsługa podróży czasu

Czas obsługi podróży jest opcjonalny i kontrolowane przez `GetSupportedTimeTravelDirections` metody. Jeśli zmienna zwraca `Forward`, `Backward`, lub `Forward | Backward` , a następnie musi implementować następujące metody

- `GetTimelineStartDate`
- `GetTimelineEndDate`
- `GetTimelineEntriesBeforeDate`
- `GetTimelineEntriesAfterDate`

<a name="writing" />

## <a name="writing-a-complication"></a>Zapisywanie Complication

Zakres komplikacji z proste danych wyświetlany obraz skomplikowane i renderowania danych z obsługą podróży czasu. Poniższy kod przedstawia sposób kompilacji complication proste i jednego szablonu.

<!--
The [sample]() for this article supports more template styles.
-->

## <a name="sample-code"></a>Przykładowy kod

W tym przykładzie obsługuje tylko `UtilitarianLarge` szablonu, dlatego można wybrać tylko na stronach obserwowanie określonych, które obsługują tego rodzaju complication. Podczas *wybranie* komplikacji na oglądanie, wyświetla **Moje COMPLICATION** i kiedy *systemem* on wyświetla tekst **MINUTĘ _godzinę_**   (z części czasu).

```csharp
[Register ("ComplicationController")]
public class ComplicationController : CLKComplicationDataSource
{
    public ComplicationController ()
    {
    }
    public ComplicationController (IntPtr p) : base (p)
    {
    }
    public override void GetCurrentTimelineEntry (CLKComplication complication, Action<CLKComplicationTimelineEntry> handler)
    {
        CLKComplicationTimelineEntry entry = null;
    var complicationDisplay = "MINUTE " + DateTime.Now.Minute.ToString(); // text to display on watch face
        if (complication.Family == CLKComplicationFamily.UtilitarianLarge)
        {
            var textTemplate = new CLKComplicationTemplateUtilitarianLargeFlat();
            textTemplate.TextProvider = CLKSimpleTextProvider.FromText(complicationDisplay); // dynamic display
            entry = CLKComplicationTimelineEntry.Create(NSDate.Now, textTemplate);
        } else {
            Console.WriteLine("Complication family timeline not supported (" + complication.Family + ")");
        }
        handler (entry);
    }
    public override void GetPlaceholderTemplate (CLKComplication complication, Action<CLKComplicationTemplate> handler)
    {
        CLKComplicationTemplate template = null;
        if (complication.Family == CLKComplicationFamily.UtilitarianLarge) {
            var textTemplate = new CLKComplicationTemplateUtilitarianLargeFlat ();
            textTemplate.TextProvider = CLKSimpleTextProvider.FromText ("MY COMPLICATION"); // static display
            template = textTemplate;
        } else {
            Console.WriteLine ("Complication family placeholder not not supported (" + complication.Family + ")");
        }
        handler (template);
    }
    public override void GetSupportedTimeTravelDirections (CLKComplication complication, Action<CLKComplicationTimeTravelDirections> handler)
    {
        handler (CLKComplicationTimeTravelDirections.None);
    }
}
```


<a name="templates" />

## <a name="complication-templates"></a>Szablony complication

Istnieje wiele różnych szablonów są dostępne dla każdego stylu complication.
**Pierścień** Szablony umożliwiają wyświetlanie pierścień postępu stylu wokół complication, którego można użyć do wyświetlenia graficznie postęp lub inną wartość.

[Dokumentacja CLKComplicationTemplate firmy Apple](https://developer.apple.com/reference/clockkit/clkcomplicationtemplate)

### <a name="circular-small"></a>Mała liczba godzin cykliczne

Te nazwy klasy szablonu są poprzedzane prefiksem `CLKComplicationTemplateCircularSmall`:

- **RingImage** -wyświetlić pojedynczy obraz, pierścień postępu wokół niego.
- **RingText** -wyświetlić pojedynczy wiersz tekstu, w toku pierścień wokół niego.
- **SimpleImage** — po prostu wyświetl pojedynczy mały obraz.
- **SimpleText** — tylko wyświetlanie małych fragment tekstu.
- **StackImage** -wyświetlania obrazu i tekstu, jeden nad drugim wierszu
- **StackText** -dwóch wierszy tekstu do wyświetlenia.

### <a name="modular-small"></a>Mała liczba godzin moduły

Te nazwy klasy szablonu są poprzedzane prefiksem `CLKComplicationTemplateModularSmall`:

- **ColumnsText** — wyświetlanie małych siatki wartości tekstowych (2 wierszy i kolumn 2).
- **RingImage** -wyświetlić pojedynczy obraz, pierścień postępu wokół niego.
- **RingText** -wyświetlić pojedynczy wiersz tekstu, w toku pierścień wokół niego.
- **SimpleImage** — po prostu wyświetl pojedynczy mały obraz.
- **SimpleText** — tylko wyświetlanie małych fragment tekstu.
- **StackImage** -wyświetlania obrazu i tekstu, jeden nad drugim wierszu
- **StackText** -dwóch wierszy tekstu do wyświetlenia.

### <a name="modular-large"></a>Moduły duże

Te nazwy klasy szablonu są poprzedzane prefiksem `CLKComplicationTemplateModularLarge`:

- **Kolumny** -wyświetlić siatkę 3 wiersze z kolumny 2, opcjonalnie tym obrazu z lewej strony każdego wiersza.
- **StandardBody** — wyświetlanie ciągu bold nagłówka, z dwa wiersze w postaci zwykłego tekstu. Nagłówek Opcjonalnie można wyświetlić obraz po lewej stronie.
- **Tabela** — wyświetlanie ciągu bold nagłówka, z siatką 2 x 2 tekstu znajdujące się poniżej. Nagłówek Opcjonalnie można wyświetlić obraz po lewej stronie.
- **TallBody** — wyświetlanie ciągu bold nagłówka, z większą czcionki pojedynczy wiersz tekstu poniżej.

### <a name="utilitarian-small"></a>Mała liczba godzin użytkowych

Te nazwy klasy szablonu są poprzedzane prefiksem `CLKComplicationTemplateUtilitarianSmall`:

- **Płaski** -Wyświetla obraz i tekst w jednym wierszu (tekst powinien być krótki).
- **RingImage** -wyświetlić pojedynczy obraz, pierścień postępu wokół niego.
- **RingText** -wyświetlić pojedynczy wiersz tekstu, w toku pierścień wokół niego.
- **Kwadratowe** -wyświetlania obrazu kwadratowych (40px lub 44px kwadratowe dla 38 mm lub mm 42 Apple Watch odpowiednio).

### <a name="utilitarian-large"></a>Użytkowych duże

Istnieje tylko jeden szablon tego stylu complication: `CLKComplicationTemplateUtilitarianLargeFlat`.
Wyświetla jeden obraz i tekst, wszystko w jednym wierszu.



## <a name="related-links"></a>Linki pokrewne

- [Dokumentacja firmy Apple](https://developer.apple.com/library/watchos/documentation/General/Conceptual/WatchKitProgrammingGuide/ComplicationEssentials.html)
- [WWDC video](https://developer.apple.com/videos/play/wwdc2015-209/)
