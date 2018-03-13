---
title: "Ułatwienia dostępu w systemie iOS"
ms.topic: article
ms.prod: xamarin
ms.assetid: 88D59B36-05A3-4356-AE29-EC2B69CE7162
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/18/2016
ms.openlocfilehash: 39fa99ba534655331098abcb55789c8f4a8afb07
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="accessibility-on-ios"></a>Ułatwienia dostępu w systemie iOS

Ta strona informacje dotyczące używania interfejsów API ułatwień dostępu systemu iOS do skompilowania aplikacji, zgodnie z [Lista kontrolna ułatwień dostępu](~/cross-platform/app-fundamentals/accessibility.md).
Zapoznaj się [ułatwień dostępu systemu Android](~/android/app-fundamentals/accessibility.md) i [dostępności OS X](~/mac/app-fundamentals/accessibility.md) stron dla innych interfejsów API platformy.

## <a name="describing-ui-elements"></a>Opisujące elementy interfejsu użytkownika

udostępnia iOS `AccessibilityLabel` i `AccessibilityHint` właściwości dla deweloperów dodać opis, które mogą być używane przez VoiceOver czytnik, aby udostępnić formantów ekranu. Formanty również mogą być oznaczane co najmniej jeden cech, które zapewnić dodatkowy kontekst w trybach dostępny.

Niektóre formanty mogą nie muszą być dostępne (na przykład, etykiety na pola tekstowego lub obrazu, który jest całkowicie ozdobne) — `IsAccessibilityElement` jest dostarczany w celu wyłączenia ułatwień dostępu w takich przypadkach.

**Projektant interfejsu użytkownika**

**Konsoli właściwości** zawierającą sekcję ułatwień dostępu, która umożliwia te ustawienia można edytować, gdy jest wybrana kontrolka w Projektancie interfejsu użytkownika dla systemu iOS:

![](accessibility-images/ios-designer-sml.png "Ustawienia ułatwień dostępu")

**C#**

Te właściwości można też ustawić bezpośrednio w kodzie:

```csharp
usernameInput.AccessibilityLabel = "Search";
usernameInput.Hint = "Press Enter after typing to search employee list";
someLabel.IsAccessibilityElement = false;
displayOnlyText.AccessibilityTraits = UIAccessibilityTrait.Header | UIAccessibilityTrait.Selected;
```

### <a name="what-is-accessibilityidentifier"></a>Co to jest AccessibilityIdentifier?

`AccessibilityIdentifier` Można określić unikatowy klucz, który może służyć do odwoływania się do elementów interfejsu użytkownika za pomocą interfejsu API biblioteki UIAutomation.

Wartość `AccessibilityIdentifier` nigdy nie jest używany lub wyświetlana użytkownikowi.

<a name="postnotification" />

## <a name="postnotification"></a>PostNotification

`UIAccessibility.PostNotification` Metoda pozwala zdarzenia podniesione do użytkowników spoza bezpośredniej interakcji (na przykład, gdy ich interakcji z określonego formantu).

### <a name="announcement"></a>Anonsu

Powiadomienia mogą być wysyłane z kodu, aby poinformować użytkownika o niektórych stan został zmieniony (takie jak operacji w tle została ukończona). Może to być dołączone oznaczenie wizualne w interfejsie użytkownika:

```csharp
UIAccessibility.PostNotification (
  UIAccessibilityPostNotification.Announcement,
    new NSString(@"Item was saved"));
```

### <a name="layoutchanged"></a>LayoutChanged

`LayoutChanged` Anonsu jest używany podczas układu ekranu:

```csharp
UIAccessibility.PostNotification (
  UIAccessibilityPostNotification.LayoutChanged,
    someControl);  // someControl gets focus
```


## <a name="accessibility-and-localization"></a>Lokalizacja i ułatwień dostępu

Uzyskiwanie dostępu do właściwości jak etykiety i wskazówkę dotyczącą może być właśnie lokalizowany, takich jak inny tekst w interfejsie użytkownika.

**MainStoryboard.strings**

Jeśli interfejs użytkownika jest rozmieszczona w scenorysu, musisz podać tłumaczenia dla właściwości ułatwień dostępu w taki sam sposób jak inne właściwości. W poniższym przykładzie `UITextField` ma **identyfikator lokalizacji** z `Pqa-aa-ury` i dwie właściwości ułatwień dostępu ustawiany w języku hiszpańskim:

```csharp
/* Accessibility */
"Pqa-aa-ury.accessibilityLabel" = "Notas input";
"Pqa-aa-ury.accessibilityHint" = "escriba más información";
```

Ten plik zostanie umieszczony w **es.lproj** hiszpańskim zawartości katalogu.

**Localizable.strings**

Alternatywnie można dodać do tłumaczenia **Localizable.strings** plik w zlokalizowanej zawartości katalogu (np.) **ES.lproj** dla języka hiszpańskiego):

```csharp
/* Accessibility */
"Notes" = "Notas input";
"Provide more information" = "escriba más información";
```

Te tłumaczeń można używać w C# za pomocą `LocalizedString` metody:

```csharp
notesText.AccessibilityLabel = NSBundle.MainBundle.LocalizedString ("Notes", "");
notesText.AccessibilityHint = NSBundle.MainBundle.LocalizedString ("Provide more information", "");
```

Zapoznaj się [przewodnik lokalizacja iOS](~/ios/app-fundamentals/localization/index.md) uzyskać więcej informacji o lokalizacji zawartości.

<a name="testing" />

## <a name="testing-accessibility"></a>Testowanie ułatwień dostępu

VoiceOver jest włączone w **ustawienia** aplikacji, przechodząc do **ogólne > Dostępność > VoiceOver**:

![](accessibility-images/settings-sml.png "Ustawienie szybkości mowy")

**Ułatwień dostępu** ekranu są także ustawienia powiększenia, rozmiar tekstu, opcje Kolor & kontrast, ustawienia mowy i innych opcji konfiguracji.

Postępuj zgodnie z następującymi [instrukcje VoiceOver](https://developer.apple.com/library/ios/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html) do testowania dostępności na urządzeniach z systemem iOS.


## <a name="simulator-testing"></a>Symulator testowania

Podczas testowania w symulatorze **inspektora ułatwień dostępu** jest dostępny do weryfikowania właściwości ułatwień dostępu i zdarzenia są poprawnie skonfigurowane. Włącz Inspektor **ustawienia** aplikacji, przechodząc do **ogólne > ułatwień dostępu > Inspektor ułatwień dostępu**:

![](accessibility-images/settings-inspector-sml.png "Włączyć inspektora ułatwień dostępu")

Po włączeniu okna Inspektora znajduje się nad ekranu z systemem iOS przez cały czas.
Oto przykład danych wyjściowych w przypadku wybrania wierszy tabeli widoku — zauważyć **etykiety** znajduje się zdanie, zapewniająca zawartości wiersza oraz że "zakończeniu" (ie. znaczników jest widoczny):

![](accessibility-images/tableview-a11y-sml.png "Za pomocą Inspektora ułatwień dostępu")

Gdy kontroler jest widoczny, użyj ikony "X" w lewym górnym, aby tymczasowo wyświetlanie i ukrywanie nakładce i włączenia/wyłączenia ułatwień.



## <a name="related-links"></a>Linki pokrewne

- [Dostępność i platform](~/cross-platform/app-fundamentals/accessibility.md)
- [iOS ułatwień dostępu (Apple)](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/iPhoneAccessibility/Accessibility_on_iPhone/Accessibility_on_iPhone.html)
- [iOS VoiceOver](http://www.apple.com/accessibility/ios/voiceover/)
