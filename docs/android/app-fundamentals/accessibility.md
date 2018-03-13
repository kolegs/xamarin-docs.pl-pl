---
title: "Ułatwienia dostępu w systemie Android"
ms.topic: article
ms.prod: xamarin
ms.assetid: 157F0899-4E3E-4538-90AF-B59B8A871204
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/28/2018
ms.openlocfilehash: 2dea77b4c52db0c032aba9bde471e76eb36ba3ad
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="accessibility-on-android"></a>Ułatwienia dostępu w systemie Android

Ta strona informacje dotyczące używania interfejsów API systemu Android ułatwień dostępu do skompilowania aplikacji, zgodnie z [Lista kontrolna ułatwień dostępu](~/cross-platform/app-fundamentals/accessibility.md).
Zapoznaj się [ułatwień dostępu systemu iOS](~/ios/app-fundamentals/accessibility.md) i [dostępności OS X](~/mac/app-fundamentals/accessibility.md) stron dla innych interfejsów API platformy.


## <a name="describing-ui-elements"></a>Opisujące elementy interfejsu użytkownika

Udostępnia android `ContentDescription` właściwość, która jest używana przez interfejsy API do odczytywania zawartości ekranu do zapewnienia dostępny opis przeznaczenia formantu.

W obu C# lub w pliku układu AXML można ustawić opis zawartości.

**C#**

Opis można ustawić w kodzie do dowolnego ciągu (lub zasobu ciągu):

```csharp
saveButton.ContentDescription = "Save data";
```

**Układ AXML**

W kodzie XML użyj układów `android:contentDescription` atrybutu:

```xml
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="Save data" />
```

### <a name="use-hint-for-textview"></a>Użyć wskazówki dotyczącej TextView

Dla `EditText` i `TextView` formanty dla danych wejściowych, użyj `Hint` właściwość, aby podać opis oczekuje się, jakie dane wejściowe (zamiast `ContentDescription`).
Po wprowadzeniu tekst sam tekst można "odczyta" zamiast wskazówce.

**C#**

Ustaw `Hint` właściwości w kodzie:

```csharp
someText.Hint = "Enter some text"; // displays (and is "read") when control is empty
```

**Układ AXML**

W kodzie XML Użyj plików układu `android:hint` atrybutu:

```xml
<EditText
    android:id="@+id/someText"
    android:hint="Enter some text" />
```


### <a name="labelfor-links-input-fields-with-labels"></a>Łącza LabelFor dane wejściowe pola z etykietami

Aby skojarzyć etykietę z kontrolki wprowadzania danych, użyj `LabelFor` właściwości

**C#**

W języku C#, ustaw `LabelFor` właściwość identyfikatora zasobu tym ta zawartość formantu opisuje (zwykle ta właściwość jest ustawiona na etykiecie i odwołuje się do innych kontrolki wprowadzania):

```csharp
EditText edit = FindViewById<EditText> (Resource.Id.editFirstName);
TextView tv = FindViewById<TextView> (Resource.Id.labelFirstName);
tv.LabelFor = Resource.Id.editFirstName;
```

**Układ AXML**

W układzie Użyj XML `android:labelFor` właściwości przywoływać identyfikatora inny formant:

```xml
<TextView
    android:id="@+id/labelFirstName"
    android:hint="Enter some text"
    android:labelFor="@+id/editFirstName" />
<EditText
    android:id="@+id/editFirstName"
    android:hint="Enter some text" />
```

### <a name="announce-for-accessibility"></a>Ogłaszamy ułatwień dostępu

Użyj `AnnounceForAccessibility` metody na dowolnym wyświetlanie formantu do komunikowania się stan lub zdarzenia zmiany użytkownikom, gdy dostępność jest włączona. Ta metoda nie jest wymagane dla większości operacji gdzie wbudowanych Narratora zapewnia wystarczającą opinii, ale powinien być używany, gdy dodatkowe informacje mogą być przydatne dla użytkownika.

Poniższy kod przedstawia wywoływanie prosty przykład `AnnounceForAccessibility`:

```csharp
button.Click += delegate {
  button.Text = string.Format ("{0} clicks!", count++);
  button.AnnounceForAccessibility (button.Text);
};
```

## <a name="changing-focus-settings"></a>Zmiana ustawień fokus

Dostępny nawigacji zależy od formantów, których fokusu w celu ułatwienia użytkownikowi zrozumieć, jakie operacje są dostępne. Udostępnia android `Focusable` właściwości, której można oznaczać formanty jako specjalnie można ustawić fokusu podczas nawigacji.

**C#**

Aby zapobiec formantu uzyskiwanie fokusu w języku C#, ustaw `Focusable` właściwości `false`:

```csharp
label.Focusable = false;
```

**Układ AXML**

W układzie pliki XML zestawu `android:focusable` atrybutu:

```xml
<android:focusable="false" />
```

Można też kontrolować kolejność fokus z `nextFocusDown`, `nextFocusLeft`, `nextFocusRight`, `nextFocusUp` atrybuty, zwykle ustawić w układzie AXML. Użyj tych atrybutów, aby upewnij się, że użytkownik może łatwo poruszać kontroli na ekranie.


## <a name="accessibility-and-localization"></a>Lokalizacja i ułatwień dostępu

W powyższych przykładach są wskazówki i zawartości opis ustawić bezpośrednio do wartości wyświetlania. Zaleca się użycie wartości w **Strings.xml** plików, takich jak ta:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="enter_info">Enter some text</string>
    <string name="save_info">Save data</string>
</resources>
```

W języku C# i AXML układu plików, przy użyciu tekstu z pliku ciągów jest pokazany poniżej:

**C#**

Zamiast używania literałów ciągów w kodzie, wyszukiwanie przetłumaczonego wartości z plików ciągów `Resources.GetText`:

```csharp
someText.Hint = Resources.GetText (Resource.String.enter_info);
saveButton.ContentDescription = Resources.GetText (Resource.String.save_info);
```

**AXML**

W układzie XML, takich jak atrybuty dostępności `hint` i `contentDescription` może być ustawiony na identyfikator ciągu:

```xml
<TextView
    android:id="@+id/someText"
    android:hint="@string/enter_info" />
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="@string/save_info" />
```

Zaletą przechowywanie tekstu w osobnym pliku jest wiele tłumaczenia na język pliku można podać w aplikacji. Zobacz [Android lokalizacja przewodnik](~/android/app-fundamentals/localization.md) Aby dowiedzieć się, jak dodać pliki zlokalizowane ciągi do projektu aplikacji.


## <a name="testing-accessibility"></a>Testowanie ułatwień dostępu

Postępuj zgodnie z [te kroki](http://developer.android.com/training/accessibility/testing.html#how-to) umożliwienie TalkBack i Eksploruj przez Touch przetestować ułatwień dostępu na urządzeniach z systemem Android.

Musisz zainstalować [TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) z witryny Google Play, jeśli nie ma w **Ustawienia > ułatwień dostępu**.


## <a name="related-links"></a>Linki pokrewne

- [Dostępność i platform](~/cross-platform/app-fundamentals/accessibility.md)
- [Interfejsy API systemu android ułatwień dostępu](http://developer.android.com/guide/topics/ui/accessibility/index.html)
