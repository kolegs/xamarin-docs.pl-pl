---
title: Lokalizacja systemu android
description: "Ten dokument zawiera funkcje lokalizacji zestawu SDK systemu Android i jak uzyskać do nich dostęp za pomocą platformy Xamarin."
ms.topic: article
ms.prod: xamarin
ms.assetid: D1277939-A1E8-468E-B136-820D816AF853
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: adfc0da404c6b9df79c3b2be51f8cafa302a6bc3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="android-localization"></a>Lokalizacja systemu android

_Ten dokument zawiera funkcje lokalizacji zestawu SDK systemu Android i jak uzyskać do nich dostęp za pomocą platformy Xamarin._

## <a name="android-platform-features"></a>Funkcje platformy systemu android

W tej sekcji opisano funkcje lokalizacji głównej systemu android. Przejdź do [następnej sekcji](#basics) określonego kodu i przykłady.

### <a name="locale"></a>Regionalne

Użytkownicy wybierają język w **Ustawienia > Langauge & wejściowych**. To pole wyboru określa język wyświetlany i ustawienia regionalne używane (np.) Data i formatowanie liczb).

Bieżące ustawienia regionalne mogą być przeszukiwane przy użyciu bieżącego kontekstu `Resources`:

```csharp
var lang = Resources.Configuration.Locale; // eg. "es_ES"
```

Ta wartość będzie identyfikator ustawień regionalnych, który zawiera zarówno kod języka i kod ustawień regionalnych, oddzielonymi znakiem podkreślenia. Odwołanie, w tym miejscu jest [listę ustawień regionalnych Java](http://www.oracle.com/technetwork/java/javase/locales-137662.html) i [Android obsługiwanych ustawień regionalnych za pośrednictwem StackOverflow](http://stackoverflow.com/questions/7973023/what-is-the-list-of-supported-languages-locales-on-android).

Typowe przykłady obejmują:

* `en_US` angielski (Stany Zjednoczone Statees)
* `es_ES` dla hiszpański (Hiszpania)
* `ja_JP` dla języka japońskiego (Japonia)
* `zh_CN` dla języka chińskiego (Chiny)
* `zh_TW` dla języka chińskiego (Tajwan)
* `pt_PT` dla portugalski (Portugalia)
* `pt_BR` dla portugalski (Brazylia)

### <a name="localechanged"></a>LOCALE_CHANGED

Generuje android `android.intent.action.LOCALE_CHANGED` podczas zmiany ich wybór języka użytkownika.

Działania można wybrać opcję rozwiązać przez ustawienie `android:configChanges` atrybut w działaniu, jak to:

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true, Icon="@drawable/launcher",
        ConfigurationChanges = ConfigChanges.Locale | ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
```

<a name="basics" />

## <a name="internationalization-basics-in-android"></a>Podstawy internacjonalizacji w systemie Android

Strategia lokalizacji w systemie android zawiera następujące części klucza:

- Foldery zasobów zawierają zlokalizowanych ciągów, obrazy i innych zasobów.

- `GetText` Metoda, która służy do pobierania zlokalizowanych ciągów w kodzie

- `@string/id` w plikach AXML do automatycznego umieszczania zlokalizowanych ciągów w układów.

### <a name="resource-folders"></a>Foldery zasobów

Aplikacje systemu android Zarządzanie większość zawartości w folderach zasobów, takich jak:

* **Układ** — zawiera pliki układu AXML.
* **obiektów drawable** — zawiera obrazów i innych obiektów drawable zasobów.
* **wartości** — zawiera ciągi.
* **nieprzetworzona** -zawiera dane plików.

Większość deweloperów zna już przy użyciu **dpi** sufiksy na **obiektów drawable** directory, aby zapewnić wiele wersji obrazu, dzięki czemu systemu Android wybierz poprawną wersję dla każdego urządzenia. Ten sam mechanizm służy zapewnienie wielu tłumaczenia na język suffixing katalogi zasobów o identyfikatorach języka i kultury.

![Zrzut ekranu przedstawiający foldery obiektów drawable/zasobów i zasobów/wartości dla wielu identyfikatorów kultury](localization-images/resources.png)

> [!NOTE]
> **Uwaga:** podczas określania językiem najwyższego poziomu, takich jak `es` tylko dwa znaki są wymagane; jednak podczas określania pełnej ustawień regionalnych, format nazwy katalogu wymaga kreska i małe **r** do dwóch oddzielnych na przykład części **pt rBR** lub **zh-rCN**. To porównać do wartości zwracanej w kodzie, który zawiera znak podkreślenia (np.) `pt_BR`). Oba te są różne wartości .NET `CultureInfo` klasy zastosowań, mającego łącznikiem tylko (np.) `pt-BR`). Zachowaj te różnice na uwadze podczas pracy różnych platform Xamarin.

#### <a name="stringsxml-file-format"></a>Format pliku Strings.XML

Zlokalizowane **wartości** katalogu (np.) **wartości es** lub **wartości pt-rBR**) powinna zawierać plik o nazwie **Strings.xml** który będzie zawierał tłumaczenia dla ustawień regionalnych.

Każdy ciąg możliwa jest elementem XML z zasobem identyfikator określony jako `name` atrybut i przetłumaczonego ciąg jako wartość:

```xml
<string name="app_name">TaskyL10n</string>
```

Musisz wprowadzić zgodnie z normalnym reguł XML, a `name` musi być Identyfikatorem prawidłowy zasób systemu Android (bez spacji i łączniki). Oto przykład domyślnego pliku ciągi (angielski), na przykład:

**values/Strings.xml**

```xml
<resources>
    <string name="app_name">TaskyL10n</string>
    <string name="taskadd">Add Task</string>
    <string name="taskname">Name</string>
    <string name="tasknotes">Notes</string>
    <string name="taskdone">Done</string>
    <string name="taskcancel">Cancel</string>
</resources>
```

Hiszpański katalogu **es wartości** zawiera plik o takiej samej nazwie (**Strings.xml**) zawiera tłumaczenia:

**values-es/Strings.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">TaskyLeon</string>
    <string name="taskadd">agregar tarea</string>
    <string name="taskname">Nombre</string>
    <string name="tasknotes">Notas</string>
    <string name="taskdone">Completo</string>
    <string name="taskcancel">Cancelar</string>
</resources>
```

![Zrzut ekranu przedstawiający wielu wartości folderów, każda z nich zawiera plik Strings.xml](localization-images/values.png)

Przy instalowaniu plików ciągów przetłumaczonego wartości może być przywoływany w obu układów i kod.

### <a name="axml-layout-files"></a>AXML układu plików

Aby odwołać zlokalizowanych ciągów w plikach układu, należy użyć `@string/id` składni. Ten fragment kodu XML z przedstawiono przykładowe `text` właściwości ustawiany z zlokalizowanych zasobów identyfikatorów (niektóre inne atrybuty zostały pominięte):

```xml
<TextView
    android:id="@+id/NameLabel"
    android:text="@string/taskname"
    ... />
<CheckBox
    android:id="@+id/chkDone"
    android:text="@string/taskdone"
    ... />
```

### <a name="gettext-method"></a>GetText — metoda

Aby pobrać przetłumaczone ciągi w kodzie, należy użyć `GetText` — metoda i przekazać identyfikator zasobu:

```csharp
var cancelText = Resources.GetText (Resource.String.taskcancel);
```

#### <a name="quantity-strings"></a>Ilość ciągów

Zasoby ciągów dla systemu android umożliwiają również tworzenie *ciągów ilość* umożliwiające tłumaczy zapewnienie tłumaczenia różne dla różnych ilości, takich jak:

* "Jest 1 zadań po lewej".
* "Brak 2 zadań nadal zrobić."

(zamiast ogólnego "istnieją n zadania po lewej").

W **Strings.xml**

```xml
<plurals name="numberOfTasks">
         <!--
                    As a developer, you should always supply "one" and "other"
                    strings. Your translators will know which strings are actually
                    needed for their language.
             -->
         <item quantity="one">There is %d task left.</item>
         <item quantity="other">There are %d tasks still to do.</item>
 </plurals>
```

Do renderowania Użyj pełny ciąg `GetQuantityString` jest metoda identyfikator zasobu i wartości, które mają być wyświetlane (która jest przekazywana dwukrotnie). Drugi parametr służy do określania przez system Android *którego* `quantity` ciąg do użycia, trzeci parametr jest wartość rzeczywista zastąpione w ciągu (oba są wymagane).

```csharp
var translated = Resources.GetQuantityString (
                    Resource.Plurals.numberOfTasks, taskcount, taskcount);`
```

Nieprawidłowa `quantity` przełączniki są:

* zero
* jeden
* dwa
* kilka
* wiele
* Inne

Są one opisane bardziej szczegółowo w [Android docs](http://developer.android.com/guide/topics/resources/string-resource.html#Plurals). Jeśli danego języka nie wymaga "special" Obsługa tych `quantity` parametry zostaną zignorowane (na przykład używane tylko w języku angielskim `one` i `other`; określanie `zero` ciągu nie odniesie żadnego skutku, nie będą używane).

### <a name="images"></a>Obrazy

Zlokalizowane obrazy są zgodne z regułami jako ciągi pliki: wszystkie obrazy, do którego odwołuje się aplikacja powinna zostać umieszczona w **obiektów drawable** katalogów, więc są używane.

Obrazy specyficzne dla ustawień regionalnych następnie należy umieścić w kwalifikowanej foldery obiektów drawable takich jak **obiektów drawable es** lub **obiektów drawable Japonia** (można również dodać specyfikatory dpi).

W tym zrzucie ekranu pokazano cztery obrazy są zapisywane w **obiektów drawable** katalogu, ale tylko jeden **flag.png**, został zlokalizowany kopie w innych katalogach.

![Zrzut ekranu przedstawiający wielu foldery obiektów drawable, każda z nich zawiera jeden lub więcej plików zlokalizowanych .png](localization-images/drawable.png)


#### <a name="other-resource-types"></a>Inne typy zasobów

Można też podać inne rodzaje ewentualnie zasoby specyficzne dla języka, w tym układów, animacji i pliki raw. Oznacza to, mogły udostępnić układu ekranu określonym dla jednego lub więcej języków docelowego, na przykład można utworzyć układ w szczególności na język niemiecki, umożliwiający bardzo długi tekst etykiety.

System android 4.2 wprowadzono obsługę [od prawej do lewej języków (od prawej do lewej)](http://android-developers.blogspot.fr/2013/03/native-rtl-support-in-android-42.html) Jeśli wartość ustawienia aplikacji `android:supportsRtl="true"`. Kwalifikator zasobów `"ldrtl"` mogą zostać włączone w nazwie direcory zawiera układy niestandardowe, które są przeznaczone dla wyświetlane od prawej do lewej.

Więcej informacji dotyczących nazewnictwa katalogu zasobów i rezerwowych, można znaleźć w Android dokumentów dotyczących [zapewnianie zasobów alternatywnych](http://developer.android.com/guide/topics/resources/providing-resources.html#AlternativeResources).


### <a name="app-name"></a>Nazwa aplikacji

Nazwa aplikacji jest łatwy do zlokalizowania przy użyciu `@string/id` w przypadku `MainLauncher` działania:

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true, Icon="@drawable/launcher",
    ConfigurationChanges =  ConfigChanges.Orientation | ConfigChanges.Locale)]
```

### <a name="right-to-left-rtl-languages"></a>Języki od prawej do lewej (od prawej do lewej)

System android 4.2 lub nowszej zapewnia pełną obsługę układów od prawej do lewej, opisano szczegółowo w [Native obsługuje RTL blogu](http://android-developers.blogspot.dk/2013/03/native-rtl-support-in-android-42.html).

Korzystając z systemem Android 4.2 (poziomu 17 interfejsu API) i nowsze, ustawienie osiowe można wprowadzić wartości z `start` i `end` zamiast `left` i `right` (na przykład `android:paddingStart`). Dostępne są także nowych interfejsów API, takich jak `LayoutDirection`, `TextDirection`, i `TextAlignment` pomocny w tworzeniu osłony, które pozwalają dostosować czytelnicy od prawej do lewej.

Poniższy zrzut ekranu przedstawia [zlokalizowanych **Tasky** próbki](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) w arabski:

[![Zrzut ekranu przedstawiający Tasky aplikacji w arabski](localization-images/rtl-ar-sml.png)](localization-images/rtl-ar.png) 

Następny zrzut ekranu przedstawia [zlokalizowanych **Tasky** próbki](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) (wersja hebrajska):

[![Zrzut ekranu przedstawiający Tasky aplikacji (wersja hebrajska)](localization-images/rtl-he-sml.png)](localization-images/rtl-he.png)

Tekst od prawej do lewej jest zlokalizowana przy użyciu **Strings.xml** pliki w taki sam sposób jak tekstu od lewej do prawej.

<a name="testing" />

## <a name="testing"></a>Testowanie

Upewnij się, że dokładnie przetestować domyślnych ustawień regionalnych. Aplikacja ulegnie awarii, jeśli domyślnych zasobów nie może zostać załadowany z jakiegoś powodu (tj. są one Brak).

### <a name="emulator-testing"></a>Emulator testowania

Zapoznaj się firmy Google [testowanie w emulatorze systemu Android](https://developer.android.com/guide/topics/resources/localization.html#testing) sekcji, aby uzyskać instrukcje dotyczące sposobu ustawiania emulatora do określonych ustawień regionalnych przy użyciu powłoki ADB.

```shell
adb shell setprop persist.sys.locale fr-CA;stop;sleep 5;start
```

### <a name="device-testing"></a>Testowanie urządzenia

Aby przetestować na urządzeniu, Zmień język w **ustawienia** aplikacji.
**Porada:** ustawić Uwaga ikon i lokalizację elementów menu, dzięki czemu można przywrócić pierwotne ustawienia języka.


## <a name="summary"></a>Podsumowanie

W tym artykule opisano podstawy lokalizowania aplikacji systemu Android przy użyciu Obsługa wbudowanych zasobów. Możesz dowiedzieć się więcej o i18n i L10n dla systemu iOS, Android i aplikacje i platform (w tym platformy Xamarin.Forms) w [tego podręcznika i platform](~/cross-platform/app-fundamentals/localization.md).



## <a name="related-links"></a>Linki pokrewne

- [Tasky (zlokalizowany w kodzie) (przykład)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [Lokalizowanie zasobów systemu android](http://developer.android.com/guide/topics/resources/localization.html)
- [Informacje o lokalizacji i Platform](~/cross-platform/app-fundamentals/localization.md)
- [Lokalizacja platformy Xamarin.Forms](~/xamarin-forms/app-fundamentals/localization.md)
- [Lokalizacja systemu iOS](~/ios/app-fundamentals/localization/index.md)
