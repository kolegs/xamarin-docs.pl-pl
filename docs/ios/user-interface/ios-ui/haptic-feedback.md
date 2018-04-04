---
title: Przekazywanie opinii dotykowych
description: W tym artykule omówiono nowe typy dotykowych opinii, dostępnych w iOS 10 oraz sposób ich wdrażania w platformy Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 888106D1-58F4-453F-BACC-91D51FA39C80
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: f2d1bd73ea764cd5bf56775abd7c7357b039bc79
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="providing-haptic-feedback"></a>Przekazywanie opinii dotykowych

_W tym artykule omówiono nowe typy dotykowych opinii, dostępnych w iOS 10 oraz sposób ich wdrażania w platformy Xamarin.iOS._

<a name="Overview" />

## <a name="overview"></a>Omówienie

Na telefonie iPhone 7 i iPhone 7 Plus, Apple dołączył nowe dotykowych odpowiedzi, które zapewniają dodatkowe sposoby fizycznie udziału użytkownika. Opinie dotykowych (nazywanej często po prostu Haptics) używa znaczeniu touch (za pośrednictwem życie drgań lub ruchu) w projekcie interfejsu użytkownika. Opcje te nowe dotykiem opinii uzyskać uwagi użytkownika i wzmocnienia ich działania.

Poniższe tematy zostanie omówiona szczegółowo:

- [O dotykowych opinii](#About-Haptic-Feedback)
- [UIImpactFeedbackGenerator](#UIImpactFeedbackGenerator)
- [UINotificationFeedbackGenerator](#UINotificationFeedbackGenerator)
- [UISelectionFeedbackGenerator](#UISelectionFeedbackGenerator)

<a name="About-Haptic-Feedback" />

## <a name="about-haptic-feedback"></a>O dotykowych opinii

Kilka wbudowanych elementów interfejsu użytkownika już reagowanie dotykowych takich jak selektorami, przełączniki i suwaki. iOS 10 teraz dodaje możliwość programowo wyzwolenia haptics przy użyciu konkretnych podklasą klasy `UIFeedbackGenerator` klasy.

Deweloper może użyć jednej z następujących `UIFeedbackGenerator` podklas programowo wyzwalacza dotykowych opinii:

- `UIImpactFeedbackGenerator` — Użyj tego generatora opinii uzupełnienie akcji lub zadań, takich jak prezentacji "thud", gdy widok slajdów na miejscu lub jeśli dwa obiekty na ekranie dojść do kolizji.
- `UINotificationFeedbackGenerator` — Użyj tego generatora opinii dla powiadomień, takie jak typ akcji Kończenie, wystąpił błąd lub inne ostrzeżenia.
- `UISelectionFeedbackGenerator` — Użyj tego generatora opinii dla aktywnie zmiany, takie jak pobieranie elementu z listy wyboru.

<a name="UIImpactFeedbackGenerator" />

### <a name="uiimpactfeedbackgenerator"></a>UIImpactFeedbackGenerator

Użyj tego generatora opinii uzupełnienie akcji lub zadań, takich jak prezentacji "thud", gdy widok slajdów na miejscu lub jeśli dwa obiekty na ekranie dojść do kolizji.

Użyć poniższego kodu do wyzwalania wpływ opinii:

```csharp
using UIKit;
...

// Initialize feedback
var impact = new UIImpactFeedbackGenerator (UIImpactFeedbackStyle.Heavy);
impact.Prepare ();

// Trigger feedback
impact.ImpactOccurred ();
```

Gdy dewelopera tworzy nowe wystąpienie klasy `UIImpactFeedbackGenerator` klasy zapewniają `UIImpactFeedbackStyle` określenie siłę opinii jako:

- `Heavy`
- `Medium`
- `Light`

`Prepare` Metoda `UIImpactFeedbackGenerator` jest wywoływana, aby poinformować system dotykowych opinie o zbliżającym się wystąpić, dzięki czemu można zminimalizować opóźnienie.

`ImpactOccurred` Metoda następnie wyzwala dotykowych opinii.

<a name="UINotificationFeedbackGenerator" />

### <a name="uinotificationfeedbackgenerator"></a>UINotificationFeedbackGenerator

Użyj tego generatora opinii dla powiadomień, takie jak typ akcji Kończenie, wystąpił błąd lub inne ostrzeżenia.

Użyć poniższego kodu do wyzwalania opinii powiadomień:

```csharp
using UIKit;
...

// Initialize feedback
var notification = new UINotificationFeedbackGenerator ();
notification.Prepare ();

// Trigger feedback
notification.NotificationOccurred (UINotificationFeedbackType.Error);
```

Nowe wystąpienie klasy `UINotificationFeedbackGenerator` klasa jest tworzona i jego `Prepare` wywoływana jest metoda informuje system dotykowych opinie o zbliżającym się wystąpić, dzięki czemu można zminimalizować opóźnienie.

`NotificationOccurred` Jest wywoływana w celu wyzwolenia dotykowych opinii z danym `UINotificationFeedbackType` z:

- `Success`
- `Warning`
- `Error`

<a name="UISelectionFeedbackGenerator" />

### <a name="uiselectionfeedbackgenerator"></a>UISelectionFeedbackGenerator

Użyj tego generatora opinii dla aktywnie zmiany, takie jak pobieranie elementu z listy wyboru.

Użyć poniższego kodu do wyzwalania wybór opinii:

```csharp
using UIKit;
...

// Initialize feedback
var selection = new UISelectionFeedbackGenerator ();
selection.Prepare ();

// Trigger feedback
selection.SelectionChanged ();
```

Nowe wystąpienie klasy `UISelectionFeedbackGenerator` klasa jest tworzona i jego `Prepare` wywoływana jest metoda informuje system dotykowych opinie o zbliżającym się wystąpić, dzięki czemu można zminimalizować opóźnienie.

`SelectionChanged` Metoda następnie wyzwala dotykowych opinii.

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego nowych typów dotykowych opinii, dostępnych w iOS 10 oraz sposób ich wdrażania w platformy Xamarin.iOS.

## <a name="related-links"></a>Linki pokrewne

- [Przykłady iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
