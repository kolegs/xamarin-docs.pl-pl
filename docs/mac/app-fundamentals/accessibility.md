---
title: Ułatwienia dostępu w macOS
description: Ten dokument zawiera opis sposobu pracy z macOS funkcje ułatwień dostępu w aplikacji Xamarin.Mac. Zawarto informacje opisujące elementy interfejsu użytkownika w scenorys i kod, kontrolki niestandardowe i testowania dostępności.
ms.prod: xamarin
ms.assetid: D7F4892B-501A-4271-A7E0-BDD1586B63AD
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: b2406654a46428e8c22284f5c7d114b07a463251
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791166"
---
# <a name="accessibility-on-macos"></a>Ułatwienia dostępu w macOS

Ta strona opisano sposób użycia macOS interfejsów API ułatwień dostępu do skompilowania aplikacji, zgodnie z [Lista kontrolna ułatwień dostępu](~/cross-platform/app-fundamentals/accessibility.md).
Zapoznaj się [ułatwień dostępu systemu Android](~/android/app-fundamentals/accessibility.md) i [ułatwień dostępu systemu iOS](~/ios/app-fundamentals/accessibility.md) stron dla innych interfejsów API platformy.

Aby zrozumieć, jak działają dostępność interfejsów API w w macOS (nazywanych OS X), Pierwsza kontrola [OS X ułatwień dostępu modelu](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXmodel.html).

## <a name="describing-ui-elements"></a>Opisujące elementy interfejsu użytkownika

Używa AppKit `NSAccessibility` protokołu uwidacznia interfejsów API udostępnić interfejsu użytkownika. Obejmuje to zachowanie domyślne, które podejmuje próbę ustawienia łatwy do rozpoznania wartości dla właściwości ułatwień dostępu, takie jak Ustawianie przycisku `AccessibilityLabel`. Etykieta jest zazwyczaj pojedynczego wyrazu lub krótka fraza opisujące kontroli lub widoku.

### <a name="storyboard-files"></a>Pliki scenorysu

Xamarin.Mac używa konstruktora interfejsu Xcode do edycji plików scenorysu.
Informacje o ułatwieniach dostępu można edytowane w **inspektora tożsamości** po wybraniu formantu na powierzchni projektu (jak pokazano na poniższym zrzucie ekranu):

[![Dodawanie ułatwień dostępu w programie Xcode interfejsu konstruktora](accessibility-images/xcode.png "Dodawanie ułatwień dostępu w programie Xcode konstruktora interfejsu")](accessibility-images/xcode-large.png#lightbox)

### <a name="code"></a>Kod

Xamarin.Mac nie ujawniać jako `AccessibilityLabel` metody ustawiającej.  Dodaj następującą metodę pomocnika można ustawić etykiety ułatwień dostępu:

```csharp
public static class AccessibilityHelper
{
    [System.Runtime.InteropServices.DllImport (ObjCRuntime.Constants.ObjectiveCLibrary)]
    extern static void objc_msgSend (IntPtr handle, IntPtr selector, IntPtr label);

    static public void SetAccessibilityLabel (this NSView view, string value)
    {
        objc_msgSend (view.Handle, new ObjCRuntime.Selector ("setAccessibilityLabel:").Handle, new NSString (value).Handle);
    }
}
```

Tej metody następnie można użyć w kodzie, jak pokazano:

```csharp
AccessibilityHelper.SetAccessibilityLabel (someButton, "New Accessible Description");
```

`AccessibilityHelp` Właściwość jest wyjaśnienie formant lub widok czego i powinny być dodane tylko, gdy etykiety może nie zawierają wystarczającej ilości informacji. Tekst pomocy powinien nadal znajdować się możliwie krótki, na przykład "Usuwa dokumentu".

Niektóre elementy interfejsu użytkownika nie są istotne dla dostępny dostępu (na przykład etykieta obok wejściem ma swoją własną etykiety ułatwień dostępu i Pomoc).
W takich sytuacjach należy ustawić `AccessibilityElement = false` tak, aby tych kontrolek ani widoków, zostaną pominięte przez czytników ekranu lub innych narzędzi ułatwień dostępu.

Udostępnia Apple [dostęp — wytyczne](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/EnhancingtheAccessibilityofStandardAppKitControls.html) objaśniający najlepsze rozwiązania dotyczące tekst etykiety i pomocy ułatwień dostępu.

## <a name="custom-controls"></a>Formanty niestandardowe

Odnoszą się do firmy Apple [wytyczne dotyczące dostępny Kontrolki niestandardowe](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/ImplementingAccessibilityforCustomControls.html) szczegółowe informacje na temat wymaga dodatkowych kroków.

## <a name="testing-accessibility"></a>Testowanie ułatwień dostępu

System macOS zapewnia **inspektora ułatwień dostępu** czy pomaga przetestować funkcje ułatwień dostępu. Inspektor znajduje się w środowisku Xcode.

Przy pierwszym uruchomieniu, **inspektora ułatwień dostępu** wymaga uprawnień do sterowania komputera za pośrednictwem ułatwień dostępu:

![Dostępność inspektora żąda uprawnienia do uruchamiania](accessibility-images/accessibility-inspector-1.png "żąda uprawnienia do uruchamiania inspektora ułatwień dostępu")

Odblokowania ekranu ustawienia (Jeśli wymagane w lewym dolnym) i znaczników **inspektora ułatwień dostępu**:

![Ekran ustawień, aby włączyć inspektora dostępności](accessibility-images/accessibility-inspector-2.png "ustawień ekranu, aby włączyć inspektora ułatwień dostępu")

Po włączeniu inspektor jest wyświetlany jako okno przestawne, które mogą być przenoszone między ekranu. Poniższy zrzut ekranu przedstawia inspektora systemem obok Mac przykładową aplikację. W miarę przenoszenia kursor nad okno, Inspektor wyświetla dostępne właściwości każdego formantu:

[![Przykład inspektora ułatwień dostępu, uruchamiania](accessibility-images/accessibility-example.png "uruchomiona inspektora przykład ułatwień dostępu")](accessibility-images/accessibility-example-large.png#lightbox)

Aby uzyskać więcej informacji, przeczytaj [testowania dostępności przewodnika OS X](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXTestingApps.html).



## <a name="related-links"></a>Linki pokrewne

- [Dostępność i platform](~/cross-platform/app-fundamentals/accessibility.md)
- [Mac ułatwień dostępu](https://www.apple.com/accessibility/mac/)
