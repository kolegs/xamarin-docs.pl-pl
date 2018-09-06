---
title: Tworzenie niestandardowych formantów na platformie Xamarin.Mac
description: Ten dokument zawiera opis sposobu tworzenia niestandardowych formantów na platformie Xamarin.Mac. Widoczny jest sposób tworzenia kontrolki niestandardowej, śledzić jego stan, rysowania jego interfejs, odpowiadanie na dane wejściowe użytkownika i użyj formantu w aplikacji.
ms.prod: xamarin
ms.assetid: 004534B1-5AEE-452C-BBBE-8C2673FD49B7
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 7180dd80c80515adc7312e2fe30d69852bf04eaa
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780628"
---
# <a name="creating-custom-controls-in-xamarinmac"></a>Tworzenie niestandardowych formantów na platformie Xamarin.Mac

Podczas pracy z C# i .NET w aplikacji platformy Xamarin.Mac, masz dostęp do tej samej użytkownika formantów, które osoba używająca *języka Objective-C*, *Swift* i *Xcode* jest . Ponieważ rozszerzenia Xamarin.Mac integruje się bezpośrednio za pomocą edytora Xcode, można użyć program Xcode _programu Interface Builder_ Aby utworzyć i zachować swojej kontrolki użytkownika (lub opcjonalnie utworzyć je bezpośrednio w kodzie języka C#).

Gdy macOS udostępnia wiele wbudowanych kontrolek użytkownika, może to być razy, które należy utworzyć formant niestandardowy, aby zapewnić funkcjonalność dostarczana nie poza pole lub, aby dopasować niestandardowy motyw interfejsu użytkownika (np. interfejs gry).

[![](custom-controls-images/intro01.png "Przykład niestandardowych kontrolek interfejsu użytkownika")](custom-controls-images/intro01.png#lightbox)

W tym artykule omówimy podstaw tworzenia wielokrotnego niestandardowe kontrolki interfejsu użytkownika w aplikacji platformy Xamarin.Mac. Zdecydowanie zalecane jest, pracy za pośrednictwem [Witaj, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programu Xcode i programu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) i [gniazd i akcje](~/mac/get-started/hello-mac.md#outlets-and-actions) sekcje w postaci, w jakiej omawia kluczowe założenia i technik, które będzie używany w tym artykule.

Możesz chcieć spojrzeć na [udostępnianie klasy języka C# / metod języka Objective-C](~/mac/internals/how-it-works.md) części [elementach wewnętrznych rozszerzenia Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentów, jak również wyjaśniono `Register` i `Export` poleceń Umożliwia o komunikacji sieciowej w górę klas języka C# do języka Objective-C obiektów i elementów interfejsu użytkownika.

<a name="Introduction-to-Outline-Views" />

## <a name="introduction-to-custom-controls"></a>Wprowadzenie do formantów niestandardowych

Jak wspomniano powyżej, może być potrzebne do tworzenia niestandardowej kontrolki interfejsu użytkownika, zapewniają unikatowe funkcje interfejsu użytkownika aplikacji rozszerzenia Xamarin.Mac lub utworzyć niestandardowy motyw interfejsu użytkownika (np. interfejs gry) do ponownego użycia.

W takich przypadkach możesz łatwo dziedziczyć od `NSControl` i utworzyć narzędzie niestandardowe albo można dodać do swojej aplikacji interfejsu użytkownika, za pomocą kodu C# lub przez program Xcode Interface Builder. Przez dziedziczenie z `NSControl` niestandardową kontrolkę, automatycznie otrzymują wszystkie standardowe funkcje, które ma wbudowane kontrolki interfejsu użytkownika (takie jak `NSButton`).

Jeśli niestandardową kontrolkę interfejsu użytkownika po prostu wyświetla informacji (takich jak tworzenie wykresów niestandardowych, a narzędzie graficzne), możesz chcieć dziedziczyć `NSView` zamiast `NSControl`.

Niezależnie od tego, która klasa bazowa jest używany podstawowe kroki tworzenia niestandardowej kontrolki jest taka sama.

W spowoduje artykułu Utwórz niestandardowe składnik przerzucić przełącznika, który zapewnia unikatową motyw interfejsu użytkownika i świecić przykładem tworzenia w pełni funkcjonalne, niestandardowe kontrolki interfejsu użytkownika.

<a name="Building-the-Custom-Control" />

## <a name="building-the-custom-control"></a>Tworzenie kontrolki niestandardowej

Ponieważ tworzymy kontrolki niestandardowej będzie odpowiadać na dane wejściowe użytkownika (po lewej stronie przycisku myszą), użyjemy dziedziczyć `NSControl`. W ten sposób nasz kontrolki niestandardowej będą wszystkie funkcje standardowe, które ma wbudowane kontrolki interfejsu użytkownika i automatycznie odpowiadać formantu standardowego systemu macOS.

W programie Visual Studio dla komputerów Mac należy otworzyć projekt platformy Xamarin.Mac, który chcesz utworzyć niestandardowe kontrolki interfejsu użytkownika dla (lub Utwórz nowy). Dodaj nową klasę, a następnie wywołaj ją `NSFlipSwitch`:

[![](custom-controls-images/custom01.png "Dodawanie nowej klasy")](custom-controls-images/custom01.png#lightbox)

Następnie Edytuj `NSFlipSwitch.cs` klasy i przypisz ją wyglądać podobnie do następującego:

```csharp
using Foundation;
using System;
using System.CodeDom.Compiler;
using AppKit;
using CoreGraphics;

namespace MacCustomControl
{
    [Register("NSFlipSwitch")]
    public class NSFlipSwitch : NSControl
    {
        #region Private Variables
        private bool _value = false;
        #endregion

        #region Computed Properties
        public bool Value {
            get { return _value; }
            set {
                // Save value and force a redraw
                _value = value;
                NeedsDisplay = true;
            }
        }
        #endregion

        #region Constructors
        public NSFlipSwitch ()
        {
            // Init
            Initialize();
        }

        public NSFlipSwitch (IntPtr handle) : base (handle)
        {
            // Init
            Initialize();
        }

        [Export ("initWithFrame:")]
        public NSFlipSwitch (CGRect frameRect) : base(frameRect) {
            // Init
            Initialize();
        }

        private void Initialize() {
            this.WantsLayer = true;
            this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
        }
        #endregion

        #region Draw Methods
        public override void DrawRect (CGRect dirtyRect)
        {
            base.DrawRect (dirtyRect);

            // Use Core Graphic routines to draw our UI
            ...

        }
        #endregion

        #region Private Methods
        private void FlipSwitchState() {
            // Update state
            Value = !Value;
        }
        #endregion

    }
}
```

Pierwszą rzeczą, którą należy zwrócić uwagę o naszych niestandardowej klasy, w tym, że firma Microsoft dziedziczą z `NSControl` i przy użyciu **zarejestrować** polecenie, aby udostępnić tej klasy, aby Objective-C i program Xcode Interface Builder:

```csharp
[Register("NSFlipSwitch")]
public class NSFlipSwitch : NSControl
```

W poniższych sekcjach firma Microsoft będzie Spójrz na pozostałą część szczegółom kodu powyżej.

<a name="Tracking-the-Controls-State" />

### <a name="tracking-the-controls-state"></a>Śledzenie stanu formantu

Ponieważ naszych kontrolka niestandardowa jest przełącznikiem, potrzebujemy sposób śledzenia stanu włączania i wyłączania przełącznika. My zajmujemy, używając następującego kodu w `NSFlipSwitch`:

```csharp
private bool _value = false;
...

public bool Value {
    get { return _value; }
    set {
        // Save value and force a redraw
        _value = value;
        NeedsDisplay = true;
    }
}
```

Po zmianie stanu przełącznika, potrzebujemy sposób aktualizacji interfejsu użytkownika. Czynność tę możemy wykonać, wymuszając formant Aby odświeżyć jego interfejsie użytkownika za pomocą `NeedsDisplay = true`.

W razie mamy kontroli więcej niż jednym (na przykład wielostanowy przełącznik z położeniami 3) stanem włączenia/wyłączenia mogliśmy użyć **wyliczenia** do śledzenia stanu. W naszym przykładzie prostego **bool** wykona.

Dodaliśmy także metody pomocnika do wymiany stan przełączanie między włączać i wyłączać:

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
}
```

Firma Microsoft będzie później, rozwiń tej klasy pomocnika, aby poinformować obiekt wywołujący, kiedy zmienił się stan przełączników.

<a name="Drawing-the-Controls-Interface" />

### <a name="drawing-the-controls-interface"></a>Rysowanie formantu interfejsu

Zamierzamy Rysowanie naszych formantu niestandardowego interfejsu użytkownika w czasie wykonywania za pomocą graficznego podstawowe Rysowanie procedury. Przed możemy to zrobić, należy włączyć warstwy naszych kontrolki. Możemy to zrobić za pomocą następującą prywatną metodę:

```csharp
private void Initialize() {
    this.WantsLayer = true;
    this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

Ta metoda jest wywoływana z każdego z konstruktorów kontrolki aby upewnić się, że formant jest skonfigurowane prawidłowo. Na przykład:

```csharp
public NSFlipSwitch (IntPtr handle) : base (handle)
{
    // Init
    Initialize();
}
```

Następnie należy zastąpić `DrawRect` metody i dodawanie podstawowych grafiki procedury narysuj kontrolkę:

```csharp
public override void DrawRect (CGRect dirtyRect)
{
    base.DrawRect (dirtyRect);

    // Use Core Graphic routines to draw our UI
    ...

}
```

Firma Microsoft będzie uatrakcyjniania stawek wizualna reprezentacja dla formantu po zmianie jego stanu (np. z **na** do **poza**). Każdej zmianie stanu, możemy użyć `NeedsDisplay = true` polecenie, aby wymusić formant Aby odświeżyć nowego stanu.

<a name="Responding-to-User-Input" />

### <a name="responding-to-user-input"></a>Odpowiadanie na dane wejściowe użytkownika

Istnieją dwa podstawową metodą, możemy dodać danych wejściowych użytkownika do naszych kontrolki niestandardowej: **zastąpienia procedury obsługi myszy** lub **aparaty rozpoznawania gestów**. Która metoda używamy, będzie zależeć od funkcje wymagane przez naszych formant.

> [!IMPORTANT]
> Dla dowolnego formantu niestandardowego, możesz utworzyć, należy użyć jednej **zastąpienia metody** _lub_ **aparaty rozpoznawania gestów**, ale nie oba jednocześnie czasu, jak mogą one powodować konflikt ze sobą.

<a name="Summary" />

#### <a name="handling-user-input-with-override-methods"></a>Obsługa danych wejściowych użytkownika za pomocą metod przesłonięcia

Obiekty, które dziedziczą z `NSControl` (lub `NSView`) mają kilka Przesłaniaj metody obsługi myszy lub klawiatury danych wejściowych. Nasze kontrolki przykład chcemy, aby przerzucić stan przełączanie między **na** i **poza** po użytkownik klika formant z lewego przycisku myszy. Możemy dodać następujące Przesłaniaj metody na `NSFliwSwitch` klasy do obsługi to:

```csharp
#region Mouse Handling Methods
// --------------------------------------------------------------------------------
// Handle mouse with Override Methods.
// NOTE: Use either this method or Gesture Recognizers, NOT both!
// --------------------------------------------------------------------------------
public override void MouseDown (NSEvent theEvent)
{
    base.MouseDown (theEvent);

    FlipSwitchState ();
}

public override void MouseDragged (NSEvent theEvent)
{
    base.MouseDragged (theEvent);
}

public override void MouseUp (NSEvent theEvent)
{
    base.MouseUp (theEvent);
}

public override void MouseMoved (NSEvent theEvent)
{
    base.MouseMoved (theEvent);
}
## endregion
```

W powyższym kodzie nazywamy `FlipSwitchState` — metoda (zdefiniowany powyżej), aby przerzucić On/Off stan przełącznika w `MouseDown` metody. Spowoduje to wymuszenie również formant mógł być narysowany ponownie, aby odzwierciedlić bieżący stan.

<a name="Handling-User-Input-with-Gesture-Recognizers" />

#### <a name="handling-user-input-with-gesture-recognizers"></a>Obsługa danych wejściowych użytkownika za pomocą aparatów rozpoznawania gestów

Opcjonalnie można użyć aparaty rozpoznawania gestów do obsługi użytkownika obsługującego daną kontrolkę. Usuń przesłonięcia dodanych wcześniej, Edytuj `Initialize` metody i przypisz ją wyglądać podobnie do następującego:

```csharp
private void Initialize() {
    this.WantsLayer = true;
    this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;

    // --------------------------------------------------------------------------------
    // Handle mouse with Gesture Recognizers.
    // NOTE: Use either this method or the Override Methods, NOT both!
    // --------------------------------------------------------------------------------
    var click = new NSClickGestureRecognizer (() => {
        FlipSwitchState();
    });
    AddGestureRecognizer (click);
}
```

Tutaj tworzymy nowy `NSClickGestureRecognizer` i wywoływania naszych `FlipSwitchState` metodę, aby zmienić stan przełącznika, gdy użytkownik kliknie na nim za pomocą lewego przycisku myszy. `AddGestureRecognizer (click)` Metoda dodaje aparat rozpoznawania gestów do kontrolki.

Ponownie jakiej metody użyjemy zależy od próbujemy można wykonać przy użyciu naszego kontrolki niestandardowej. Jeśli będziemy potrzebować niski poziom dostępu do interakcji z użytkownikiem, użyj zastąpienia metody. Jeśli będziemy potrzebować wstępnie zdefiniowane funkcje, takie jak kliknięcie myszą, użyj aparaty rozpoznawania gestów.

<a name="Responding-to-State-Change-Events" />

### <a name="responding-to-state-change-events"></a>Odpowiadanie na zdarzenia zmiany stanu

Gdy użytkownik zmieni stan naszych formant niestandardowy, potrzebujemy sposób reakcji na zmianę stanu, w kodzie (takich jak coś po kliknie przycisk niestandardowy). 

Do tej funkcji, należy edytować `NSFlipSwitch` klasę i Dodaj następujący kod:

```csharp
#region Events
public event EventHandler ValueChanged;

internal void RaiseValueChanged() {
    if (this.ValueChanged != null)
        this.ValueChanged (this, EventArgs.Empty);

    // Perform any action bound to the control from Interface Builder
    // via an Action.
    if (this.Action !=null) 
        NSApplication.SharedApplication.SendAction (this.Action, this.Target, this);
}
## endregion
```

Następnie Edytuj `FlipSwitchState` metody i przypisz ją wyglądać podobnie do następującego:

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
    RaiseValueChanged ();
}
```

Po pierwsze, firma Microsoft zapewnia `ValueChanged` zdarzeń czy możemy dodać program obsługi w kodzie języka C#, aby firma Microsoft wykonaj akcję, gdy użytkownik zmieni stan przełącznika.

Druga, ponieważ dziedziczy po naszym kontrolki niestandardowej `NSControl`, automatycznie ma **akcji** które można przypisać w program Xcode Interface Builder. Aby wywołać ten **akcji** po zmianie stanu, możemy użyć następującego kodu:

```csharp
if (this.Action !=null) 
    NSApplication.SharedApplication.SendAction (this.Action, this.Target, this);
```

Po pierwsze sprawdzenie czy **akcji** przypisany do kontrolki. Następnie nazywamy **akcji** Jeśli została zdefiniowana.

<a name="Using-the-Custom-Control" />

## <a name="using-the-custom-control"></a>Za pomocą formantu niestandardowego

Za pomocą naszych kontrolki niestandardowej w pełni zdefiniowana możemy albo dodać go do naszej aplikacji rozszerzenia Xamarin.Mac interfejsu użytkownika przy użyciu kodu C# lub w program Xcode Interface Builder.

Aby dodać kontrolki przy użyciu programu Interface Builder, najpierw wykonaj czystą kompilację projektu platformy Xamarin.Mac, a następnie kliknij dwukrotnie `Main.storyboard` plik, aby otworzyć w programu Interface Builder na potrzeby edycji:

[![](custom-controls-images/custom02.png "Edytowanie scenorysu w środowisku Xcode")](custom-controls-images/custom02.png#lightbox)

Następnie przeciągnij `Custom View` do projektowania interfejsu użytkownika:

[![](custom-controls-images/custom03.png "Wybierając widok niestandardowy z biblioteki")](custom-controls-images/custom03.png#lightbox)

Przy użyciu widoku niestandardowego, nadal zaznaczone, przełącz się do **Inspektor tożsamości** i zmienić widok **klasy** do `NSFlipSwitch`:

[![](custom-controls-images/custom04.png "Ustawienia widoku klasy")](custom-controls-images/custom04.png#lightbox)

Przełącz się do **edytora Asystenta** i utworzyć **ujścia** dla formantu niestandardowego (upewniając się powiązać go w `ViewControler.h` pliku i nie `.m` plików):

[![](custom-controls-images/custom05.png "Konfigurowanie nowego punktu sprzedaży")](custom-controls-images/custom05.png#lightbox)

Zapisz zmiany, wróć do programu Visual Studio dla komputerów Mac i umożliwienia wprowadzania zmian do synchronizacji. Edytuj `ViewController.cs` pliku i upewnij `ViewDidLoad` wygląd metoda podobne do następującego:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Do any additional setup after loading the view.
    OptionTwo.ValueChanged += (sender, e) => {
        // Display the state of the option switch
        Console.WriteLine("Option Two: {0}", OptionTwo.Value);
    };
}
``` 

W tym miejscu pod uwagę brane `ValueChanged` zdarzeń zdefiniowaliśmy powyżej na `NSFlipSwitch` klasy i zapisać bieżącą **wartość** po użytkownik klika formant.

Opcjonalnie można powrócić do programu Interface Builder i zdefiniuj **akcji** od kontrolki:

[![](custom-controls-images/custom06.png "Konfigurowanie nowej akcji")](custom-controls-images/custom06.png#lightbox)

Ponownie, Edycja `ViewController.cs` pliku i dodaj następującą metodę:

```csharp
partial void OptionTwoFlipped (Foundation.NSObject sender) {
    // Display the state of the option switch
    Console.WriteLine("Option Two: {0}", OptionTwo.Value);
}
```

> [!IMPORTANT]
> Należy używać **zdarzeń** lub zdefiniować **akcji** programu Interface Builder, ale nie należy używać obu tych metod, w tym samym czasie lub mogą powodować konflikty ze sobą.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule jest zajęta szczegółowe omówienie procesu tworzenia wielokrotnego niestandardowe kontrolki interfejsu użytkownika w aplikacji platformy Xamarin.Mac. Firma Microsoft działa jak narysować niestandardowych formantów interfejsu użytkownika, dwa główne sposoby odpowiedzieć na myszy i dane wejściowe użytkownika oraz jak udostępniać nowy formant do akcji w program Xcode Interface Builder.

## <a name="related-links"></a>Linki pokrewne

- [MacCustomControl (przykład)](https://developer.xamarin.com/samples/mac/MacCustomControl/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Powiązanie danych i kodowanie klucz-wartość](~/mac/app-fundamentals/databinding.md)
- [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Obsługa zdarzeń myszy](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/EventOverview/HandlingMouseEvents/HandlingMouseEvents.html)
