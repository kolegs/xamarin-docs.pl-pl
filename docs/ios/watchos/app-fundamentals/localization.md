---
title: Praca z lokalizacji
description: "Dostosowania watchOS aplikacji dla wielu języków"
ms.topic: article
ms.prod: xamarin
ms.assetid: 55834877-757B-4860-AF2F-933A948BE38D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 9ad3499a232e5f2b2ef362f772ed0197e71e6bee
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-localization"></a>Praca z lokalizacji

_Dostosowania watchOS aplikacji dla wielu języków_

![](localization-images/both-languages-sml.png "Apple Watch wyświetlanie zlokalizowanej zawartości")

aplikacje watchOS są zlokalizowane przy użyciu metod standardowych iOS:

- Przy użyciu **identyfikator lokalizacji** w przypadku elementów scenorysu
- **.Strings** pliki skojarzone z scenorysu, i
- **Localizable.Strings** pliki tekstowe używane w kodzie.

Scenorys domyślne i zasobów, które znajdują się w **Base** katalogu i tłumaczenia specyficzne dla języka i inne zasoby są przechowywane w **.lproj** katalogów.
iOS i OS czujki będą automatycznie używać wybór języka użytkownika załadować poprawne ciągów i zasobów.

Ponieważ aplikacja Apple Watch ma dwie części - czujki aplikacji i rozszerzenie Czujka — zlokalizowane ciągi zasobów są wymagane w dwóch miejscach, w zależności od sposobu ich używania.

Zlokalizowany tekst i zasoby będą *różnych* w aplikacji czujki i rozszerzenia czujki.

## <a name="watch-app"></a>Obejrzyj aplikacji

Ta aplikacja czujki zawiera scenorysu, który opisuje interfejs użytkownika aplikacji. Wszystkie formanty (takich jak `Label` i `Image`) czy obsługa lokalizacji mają **identyfikator lokalizacji**.

Właściwe dla każdego języka **.lproj** katalogu powinien zawierać **.strings** pliki z tłumaczenia dla każdego elementu (przy użyciu **identyfikator lokalizacji**), oraz obrazów odwołuje się scenorysu.

## <a name="watch-extension"></a>Obejrzyj rozszerzenia

Rozszerzenie Obejrzyj to, gdzie uruchamia kod aplikacji. Tekst, który jest wyświetlany na ekranie z kodu musi być lokalizowany w rozszerzeniu, a nie w aplikacji czujki.

Rozszerzenie powinien również zawierać specyficzne dla języka **.lproj** katalogów, ale **.strings** tylko pliki wymagają tłumaczeń tekstu, który jest używany w kodzie.

## <a name="globalizing-the-watch-solution"></a>Globalizacja rozwiązań czujki

Globalizacja to proces polegający na wprowadzaniu aplikacji lokalizowalny.
Dla aplikacji czujki, oznacza to, projektowanie scenorysu z różne długości tekstu, pamiętając zapewnienie każdego układu ekranu dostosowuje odpowiednio w zależności od tego, jaki tekst jest wyświetlany. Należy również upewnić się, wszelkie ciągi, do których odwołuje się kod rozszerzenia czujki można przetłumaczyć przy użyciu `LocalizedString` metody.

### <a name="watch-app"></a>Obejrzyj aplikacji

Domyślnie czujki aplikacji nie jest skonfigurowany do lokalizacji. Musisz przenieść plik scenorysu domyślne i utworzyć niektórych innych katalogów do tłumaczenia:

1. Utwórz **Base.lproj** katalogu i Przenieś **Interface.storyboard** do niego.

2. Utwórz  **<language>.lproj** katalogów dla każdego z obsługiwanych języków.

3. **.Lproj** katalogów powinna zawierać **Interface.strings** pliku tekstowego (nazwa pliku powinna odpowiadać nazwie storboard). Opcjonalnie można umieścić żadnych obrazów, które wymagają lokalizacji w tych katalogach.

Obejrzyj projektu aplikacji wygląda tak po dokonaniu zmian (tylko angielskim i hiszpańskim język pliki zostały dodane):

  ![](localization-images/watchapp-solution.png "Projekt aplikacji pliki językowe w języku angielskim i hiszpańskim, czujki")

#### <a name="storyboard-text"></a>Tekst scenorysu

Podczas edytowania scenorysu, zaznacz każdy element i zwróć uwagę, **identyfikator lokalizacji** wyświetlonym w **właściwości** konsoli:

  [![](localization-images/storyboard-sml.png "Identyfikator lokalizacji, która jest wyświetlana w konsoli do właściwości")](localization-images/storyboard.png#lightbox)

W **Base.lproj** folderu, utworzyć pary klucz wartość, jak pokazano poniżej, gdy klucz jest tworzony przez **identyfikator lokalizacji** i nazwę właściwości formantu, połączonych pojedynczego znaku kropki (`.`).

```csharp
"AgC-eL-Hgc.title" = "WatchL10nEN"; // interface controller title
"0.text" = "Welcome to WatchL10n"; // Welcome
"1.text" = "Language settings are in Apple Watch App"; // How to change language
"2.title" = "Greetings"; // Greeting
"6.title" = "Detail";
"39.text" = "Second screen";
```

W tym przykładzie który **identyfikator lokalizacji** może być prosty numer ciąg (np.) "0", "1", itp.) lub ciąg bardziej złożonych (na przykład "AgC-eL-Hgc"). `Label` formanty mają `Text` właściwości i `Button`s ma `Title` właściwość, która jest widoczny w sposób ich zlokalizowanych wartości są ustawiane — należy użyć nazwy właściwości małe litery, jak pokazano w przykładzie powyżej.

Po wyrenderowaniu scenorysu na czujki poprawne wartości zostanie automatycznie wyodrębnić i wyświetlane zgodnie z językiem wybranym przez użytkownika.

#### <a name="storyboard-images"></a>Obrazy scenorysu

Zawiera również przykład rozwiązania  **gradient@2x.png**  obrazu w każdym folderze języka. Ten obraz mogą być różne dla każdego języka (np.) go osadzonych tekst, który ma tłumaczenia, lub użyj zlokalizowanych nadruków).

Wystarczy ustawić obrazu **obrazu** właściwości scenorysu i prawidłowy obraz będzie odtwarzany na czujki zgodnie z językiem wybranym przez użytkownika.

![](localization-images/storyboard-image.png "Ustaw właściwości obrazu w scenorysu obrazów")

Uwaga: ponieważ wszystkie obserwowanie Apple Wyświetla siatkówki tylko  **@2x**  wymagana jest wersja obrazu. Nie należy określić  **@2x**  w scenorysu.

### <a name="watch-extension"></a>Obejrzyj rozszerzenia

Rozszerzenie czujki wymaga podobną strukturę katalogu do obsługi lokalizacji, ale nie istnieje żaden scenorys. Zlokalizowanych ciągów w rozszerzeniu są tylko te, które odwołuje się kod w języku C#.

![](localization-images/watchextension-solution.png "Struktura katalogów czujki rozszerzenia obsługi lokalizacji")

#### <a name="strings-in-code"></a>Ciągi w kodzie

**Localizable.strings** plik ma strukturę nieco inne niż Jeśli został skojarzony z scenorysu. W takim przypadku można wybrać dowolny ciąg "klucz"; Firmy Apple zaleca się użycie klucza, które odzwierciedla tekstu, który będzie wyświetlany w języku domyślnym:

```csharp
"Breakfast time" = "Breakfast time!"; // morning
"Lunch time" = "Lunch time!"; // midday
"Dinner time" = "Dinner time!"; // evening
"Bed time" = "Bed time!"; // night
```

`NSBundle.MainBundle.LocalizedString` Metoda służy do rozpoznawania ciągi do ich odpowiedniki przetłumaczonego, jak pokazano w poniższym kodzie.

```csharp
var display = "Breakfast time";
var localizedDisplay =
  NSBundle.MainBundle.LocalizedString (display, comment:"greeting");
displayText.SetText (localizedDisplay);
```

#### <a name="images-in-code"></a>Obrazy w kodzie

Obrazy, które są wypełnione przez kod można ustawić na dwa sposoby.

1. Możesz zmienić `Image` formant ustawiając jego wartość nazwę ciągu obrazu, który już istnieje w aplikacji czujki np

  ```csharp
  displayImage.SetImage("gradient"); // image in Watch App (as shown above)
  ```

2. Obraz można przenosić z rozszerzenia, przy użyciu czujki `FromBundle` i aplikacja będzie automatycznie wybierać obrazu poprawny wybór języka użytkownika. W rozwiązaniu przykład jest obrazem  **language@2x.png**  w każdym z tych języków folderu który jest wyświetlany na `DetailController` przy użyciu następującego kodu:

  ```csharp
  using (var image = UIImage.FromBundle ("language")) {
    displayImage.SetImage (image);
  }
  ```

  Należy pamiętać, że nie należy określić  **@2x**  podczas odwoływania się do nazwy pliku obrazu.

Druga metoda dotyczy również pobranie obrazu na serwerze zdalnym do renderowania czujki; Jednak w takim przypadku należy upewnić się, obraz, który można pobrać poprawnie zlokalizowaną zgodnie z preferencjami użytkownika.

## <a name="localization"></a>Lokalizacja

Po skonfigurowaniu rozwiązania tłumaczy, należy przetworzyć Twoje **.strings** pliki i obrazy dla każdego z obsługiwanych języków.

Można utworzyć dowolną liczbę **.lproj** katalogi w ramach muszą (po jednej dla każdego z obsługiwanych języków). Są to przy użyciu kodów języków, takich jak **en**, **es**, **de**, **ja**, **pt-BR**, itp. (dla języka polskiego Hiszpański, japoński, niemiecki i portugalski (Brazylia) odpowiednio).

Przykładowe dołączone nawiązywał pokazują, jak do zlokalizowania aplikacji watchOS tłumaczenia (generowane przez komputer).

### <a name="watch-app"></a>Obejrzyj aplikacji

Te wartości są używane do tłumaczenia interfejsu użytkownika zdefiniowanych w aplikacji czujki scenorysu. *Klucza* wartość jest kombinacją każdego formantu scenorysu **identyfikator lokalizacji** i właściwości tłumaczonym.

Zalecane jest dodawanie komentarzy zawierające oryginalny tekst do pliku, dzięki czemu tłumaczy wie, co powinno być tłumaczenia.

#### <a name="eslprojinterfacestrings"></a>es.lproj/Interface.strings

Poniżej przedstawiono ciągów języka hiszpańskiego (przetłumaczony maszynowo) dla scenorysu. Warto dodawać komentarze do każdego wiersza, ponieważ jest trudne wiedzieć, co **identyfikator lokalizacji** odwołuje się do inaczej:

```csharp
"AgC-eL-Hgc.title" = "Spanish"; // app screen heading
"0.text" = "Bienvenido a WatchL10n"; // Welcome to WatchL10n
"1.text" = "Ajustes de idioma están en Apple Watch App"; // Change the language in the Apple Watch App
"2.title" = "Saludos"; // Greetings
"6.title" = "2nd"; // second screen heading
"39.text" = "Segunda pantalla"; // second screen
```

### <a name="watch-extension"></a>Obejrzyj rozszerzenia

Te wartości są używane w kodzie do tłumaczy informacji przed wyświetleniem dla użytkownika. *Klucza* wybrane przez dewelopera, gdy są one pisanie kodu i zwykle zawiera rzeczywisty ciąg ma zostać poddany translacji.

#### <a name="eslprojlocalizablestrings-file"></a>Plik es.lproj/Localizable.strings

Ciągi języka Spansish (przetłumaczony maszynowo):

```csharp
"Breakfast time" = "la hora del desayuno"; // morning
"Lunch time" = "hora de comer"; // midday
"Dinner time" = "hora de la cena"; // evening
"Bed time" = "la hora de dormir"; // night
```

## <a name="testing"></a>Testowanie

Metodę, aby zmienić preferencje językowe różni się między symulator i fizyczne urządzenia.

### <a name="simulator"></a>Symulator

W symulatorze, wybierz język, aby przetestować go przy użyciu systemu iOS **ustawienia** aplikacji (ikona szare narzędzi w symulatorze ekranu głównego).

  ![](localization-images/sim-settings-sml.png "Lokalizacja aplikacji ustawień systemu iOS")

### <a name="watch-device"></a>Obejrzyj urządzenia

Podczas testowania za pomocą czujki, Zmień język czujki w **Apple Watch** aplikacji na sparowanej iPhone.

  ![](localization-images/phone-settings-sml.png "Zmień język czujki w aplikacji Apple Watch w parach iPhone")



## <a name="related-links"></a>Linki pokrewne

- [WatchLocalization (przykład)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchLocalization/)
