---
title: "Tworzenie niestandardowych formantów"
description: "W tym artykule opisano sposób tworzenia niestandardowych formantów i pracować z nimi w Konstruktorze interfejsu."
ms.topic: article
ms.prod: xamarin
ms.assetid: 004534B1-5AEE-452C-BBBE-8C2673FD49B7
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 3ea88810384dfe8b1a08080953db19caddf25d6a
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="creating-custom-controls"></a>Tworzenie niestandardowych formantów

_W tym artykule opisano sposób tworzenia niestandardowych formantów i pracować z nimi w Konstruktorze interfejsu._

Podczas pracy z C# i .NET w aplikacji Xamarin.Mac, masz dostęp do tego samego użytkownika formantów, które osoba używająca *Objective-C*, *Swift* i *Xcode* jest . Ponieważ Xamarin.Mac integruje się bezpośrednio z Xcode, można użyć w środowisku Xcode _konstruktora interfejsu_ do tworzenia i obsługi formantów użytkownika (lub opcjonalnie utworzyć je bezpośrednio w kodzie języka C#).

System macOS zawiera wiele wbudowanych kontrolki użytkownika, natomiast może być razy, które należy utworzyć niestandardowego formantu w ramach zapewnienia funkcji nie podano poza pole lub odpowiadające motywu niestandardowego interfejsu użytkownika (na przykład gier interface).

[![](custom-controls-images/intro01.png "Przykład niestandardowych formantu interfejsu użytkownika")](custom-controls-images/intro01.png#lightbox)

W tym artykule firma Microsoft będzie kroki te obejmują podstawy tworzenia wielokrotnego użytku niestandardowe kontrolki interfejsu użytkownika w aplikacji Xamarin.Mac. Zdecydowanie zaleca się pracę za pośrednictwem [Hello, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programów Xcode i kompilatora interfejsu](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) i [gniazda i akcje](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sekcje, w jakiej omawia kluczowe założenia i techniki, które będzie używana w tym artykule.

Może zajść potrzeba Przyjrzyjmy się [udostępnianie klasy języka C# / metody Objective-C](~/mac/internals/how-it-works.md) sekcji [wewnętrzne Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentu również wyjaśniono `Register` i `Export` poleceń używany do przesyłania w górę klas C# do obiektów języka Objective C i elementy interfejsu użytkownika.

<a name="Introduction-to-Outline-Views" />

## <a name="introduction-to-custom-controls"></a>Wprowadzenie do formantów niestandardowych

Jak wspomniano powyżej, może być potrzebne do tworzenia niestandardowych kontrolki interfejsu użytkownika do zapewnienia funkcji interfejsu użytkownika aplikacji Xamarin.Mac lub Utwórz motyw niestandardowy interfejs użytkownika (na przykład gier interface) do ponownego użycia.

W takiej sytuacji użytkownik może łatwo dziedziczyć `NSControl` i utworzyć niestandardowe narzędzie, które albo mogą być dodawane do interfejsu użytkownika aplikacji, za pomocą kodu C# lub przez konstruktora interfejsu w środowisku Xcode. Poprzez dziedziczenie z `NSControl` niestandardowego formantu zostanie automatycznie mają wszystkie standardowe funkcje, które ma wbudowane kontrolki interfejsu użytkownika (takich jak `NSButton`).

Jeśli niestandardowego formantu interfejsu użytkownika wyświetla tylko informacje (takich jak niestandardowe wykresy i graficzne narzędzia), może być dziedziczona z `NSView` zamiast `NSControl`.

Niezależnie od tego, która klasa podstawowa jest używana podstawowe kroki tworzenia kontrolki niestandardowej jest taka sama.

W spowoduje artykułu Utwórz niestandardowy składnik Przerzuć przełącznika udostępniający unikatowy motywu interfejsu użytkownika i zapoznać się przykładem tworzenia funkcjonalnej formantu niestandardowego interfejsu użytkownika.

<a name="Building-the-Custom-Control" />

## <a name="building-the-custom-control"></a>Tworzenie formantu niestandardowego

Ponieważ formant niestandardowy tworzymy będzie odpowiadać, aby dane wejściowe użytkownika (po lewej stronie przycisku kliknięcia), zamierzamy dziedziczyć `NSControl`. W ten sposób naszych kontrolki niestandardowej automatycznie wszystkie standardowe funkcje, które ma wbudowane kontrolki interfejsu użytkownika i odpowiadać formantu standardowego macOS.

W programie Visual Studio dla komputerów Mac Otwórz projekt Xamarin.Mac, który chcesz utworzyć niestandardowe kontrolki interfejsu użytkownika dla (lub Utwórz nową). Dodaj nową klasę i nadaj mu `NSFlipSwitch`:

[![](custom-controls-images/custom01.png "Dodawanie nowej klasy")](custom-controls-images/custom01.png#lightbox)

Następnie Edytuj `NSFlipSwitch.cs` klasy i zapewnić ich wyglądać następująco:

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

Najpierw należy zauważyć o naszych niestandardowej klasy, w tym możemy dziedziczących `NSControl` i przy użyciu **zarejestrować** polecenia do udostępnienia tej klasy do konstruktora interfejsu Objective-C i w środowisku Xcode:

```csharp
[Register("NSFlipSwitch")]
public class NSFlipSwitch : NSControl
```

W poniższych sekcjach przeniesiemy wygląd magazynowane powyżej kodu szczegółowo.

<a name="Tracking-the-Controls-State" />

### <a name="tracking-the-controls-state"></a>Śledzenie stanu formantu

Ponieważ naszych formant niestandardowy jest połączony z przełącznikiem, potrzebujemy sposobem śledzenia stanu On lub wyłącza przełącznika. Który chronimy w następującym kodem `NSFlipSwitch`:

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

Po zmianie stanu przełącznika, potrzebujemy sposób zaktualizowany w Interfejsie użytkownika. Przejdziemy, powodując, że kontrolka ponowne jej interfejsu użytkownika z `NeedsDisplay = true`.

Jeśli mamy kontroli wymagane do więcej niż jeden Włącz/Wyłącz stan (na przykład wiele stanów przełącznika z pozycji 3), można użyliśmy **wyliczenia** śledzenie stanu. W naszym przykładzie prosty **bool** będzie wykonywać.

Dodaliśmy również metody pomocnika do wymiany stan przełączanie między włączać i wyłączać:

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
}
```

Firma Microsoft będzie później, rozwiń ta klasa pomocy do informowania wywołującego zmiany stanu przełączników.

<a name="Drawing-the-Controls-Interface" />

### <a name="drawing-the-controls-interface"></a>Rysowanie formantu interfejsu

Firma Microsoft będzie używany Core graficzne rysowania procedury do rysowania naszych formantu niestandardowego interfejsu użytkownika w czasie wykonywania. Zanim firma Microsoft musimy włączyć warstwy naszych kontrolki. Firma Microsoft to zrobić przy użyciu następującej metody prywatnych:

```csharp
private void Initialize() {
    this.WantsLayer = true;
    this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

Ta metoda jest wywoływana z każdej z konstruktorów formantu, aby upewnić się, że formant jest skonfigurowany prawidłowo. Na przykład:

```csharp
public NSFlipSwitch (IntPtr handle) : base (handle)
{
    // Init
    Initialize();
}
```

Następnie należy zastąpić `DrawRect` — metoda i dodać podstawowe graficzne procedur, by narysować kontrolkę:

```csharp
public override void DrawRect (CGRect dirtyRect)
{
    base.DrawRect (dirtyRect);

    // Use Core Graphic routines to draw our UI
    ...

}
```

Firma Microsoft będzie można dostosowywania wizualnej reprezentacji formantu po zmianie jego stanu (np. z **na** do **poza**). Każdej zmianie stanu, możemy użyć `NeedsDisplay = true` polecenie, aby wymusić kontroli ponowne nowego stanu.

<a name="Responding-to-User-Input" />

### <a name="responding-to-user-input"></a>Odpowiada na dane wejściowe użytkownika

Istnieją dwa podstawową metodą, czy można dodać danych wejściowych użytkownika na naszych kontrolki niestandardowej: **zastąpienia procedury obsługi myszy** lub **aparaty rozpoznawania gestów**. Metody firma Microsoft, w oparciu funkcje wymagane przez naszych formant.

> [!IMPORTANT]
> Dla każdego formantu niestandardowego należy utworzyć, należy używać albo **zastąpienie metody** _lub_ **aparaty rozpoznawania gestów**, ale nie oba jednocześnie czasu, jak mogą powodować konflikty ze sobą.

<a name="Summary" />

#### <a name="handling-user-input-with-override-methods"></a>Obsługa danych wejściowych użytkownika za pomocą metod zastąpienia

Obiekty, które dziedziczą z `NSControl` (lub `NSView`) mają kilka Przesłaniaj metody obsługi myszy lub klawiatury danych wejściowych. Nasze kontrolki przykład chcemy Przerzuć stan przełączanie między **na** i **poza** po kliknięciu przez użytkownika w formancie z lewego przycisku myszy. Można dodać następujące zastąpienie metody `NSFliwSwitch` klasy do obsługi to:

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

W powyższym kodzie nazywamy `FlipSwitchState` — metoda (zdefiniowany powyżej) można przerzucić On/Wyłącz stan przełącznika w `MouseDown` metody. Spowoduje to również wymuszenie formantu ma być narysowany ponownie, aby odzwierciedlały bieżący stan.

<a name="Handling-User-Input-with-Gesture-Recognizers" />

#### <a name="handling-user-input-with-gesture-recognizers"></a>Obsługa danych wejściowych użytkownika z aparatów rozpoznawania gestów

Opcjonalnie można użyć aparaty rozpoznawania gestów do obsługi użytkowników, interakcji z formantem. Usuń zastąpienia dodanych wcześniej, Edytuj `Initialize` — metoda i zapewnić ich wyglądać następująco:

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

W tym miejscu tworzymy nową `NSClickGestureRecognizer` i wywoływania naszych `FlipSwitchState` metodę, aby zmienić stan przełącznika, gdy użytkownik kliknie na nim z lewego przycisku myszy. `AddGestureRecognizer (click)` Metoda dodaje aparat rozpoznawania gestów do formantu.

Ponownie którego metoda używamy zależy od próbujemy do wykonania z naszej kontrolki niestandardowej. Jeśli potrzebujemy niski poziom dostępu do interakcji z użytkownikiem, użyj metody zastąpienia. Potrzebujemy wstępnie zdefiniowane funkcje, takie jak kliknięcie myszą użyć aparaty rozpoznawania gestów.

<a name="Responding-to-State-Change-Events" />

### <a name="responding-to-state-change-events"></a>Odpowiadanie na zdarzenia zmiany stanu

Gdy użytkownik zmieni stan naszej kontrolki niestandardowej, potrzebujemy sposobem Odpowiedz na zmianę stanu w kodzie (takich jak robi po kliknięciu przycisku niestandardowego). 

Do tej funkcji, należy edytować `NSFlipSwitch` klasy i Dodaj następujący kod:

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

Następnie Edytuj `FlipSwitchState` — metoda i zapewnić ich wyglądać następująco:

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
    RaiseValueChanged ();
}
```

Po pierwsze, firma Microsoft udostępnia `ValueChanged` zdarzeń, że można dodać do obsługi w kodzie języka C#, aby firma Microsoft wykonywania akcji, gdy użytkownik zmieni stan przełącznika.

Drugie, ponieważ dziedziczy po naszej kontrolki niestandardowej `NSControl`, automatycznie ma **akcji** które można przypisać w Konstruktorze interfejsu w środowisku Xcode. Aby wywołać tę **akcji** po zmianie stanu, możemy użyć poniższego kodu:

```csharp
if (this.Action !=null) 
    NSApplication.SharedApplication.SendAction (this.Action, this.Target, this);
```

Najpierw musimy sprawdzić czy **akcji** przypisany do formantu. Następnie nazywamy **akcji** Jeśli została ona zdefiniowana.

<a name="Using-the-Custom-Control" />

## <a name="using-the-custom-control"></a>Za pomocą formantu niestandardowego

Z naszych kontrolki niestandardowej w pełni zdefiniowana firma Microsoft albo dodać go do naszej aplikacji Xamarin.Mac interfejsu użytkownika przy użyciu kodu C# lub konstruktora interfejsu w środowisku Xcode.

Aby dodać formantu przy użyciu narzędzia Konstruktor interfejsu, najpierw wykonaj czystą kompilacji projektu Xamarin.Mac, a następnie kliknij dwukrotnie `Main.storyboard` plik, aby otworzyć go w Konstruktorze interfejs do edycji:

[![](custom-controls-images/custom02.png "Edytowanie scenorysu w środowisku Xcode")](custom-controls-images/custom02.png#lightbox)

Następnie przeciągnij `Custom View` do projektu interfejsu użytkownika:

[![](custom-controls-images/custom03.png "Wybieranie widok niestandardowy z biblioteki")](custom-controls-images/custom03.png#lightbox)

Z widoku niestandardowym nadal zaznaczone, przełącz się do **inspektora tożsamości** i zmienić widok **klasy** do `NSFlipSwitch`:

[![](custom-controls-images/custom04.png "Ustawienie klasy widoku")](custom-controls-images/custom04.png#lightbox)

Przełącz się do **Edytor Asystenta** i Utwórz **gniazda** dla formantu niestandardowego (upewnieniu się powiązać go w `ViewControler.h` pliku i nie `.m` pliku):

[![](custom-controls-images/custom05.png "Konfigurowanie nowego gniazda")](custom-controls-images/custom05.png#lightbox)

Zapisz zmiany, wróć do programu Visual Studio for Mac i pozwala na wprowadzanie zmian do synchronizacji. Edytuj `ViewController.cs` plików i upewnij `ViewDidLoad` wygląd metody podobne do poniższych:

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

W tym miejscu odpowiemy na `ValueChanged` zdarzeń zdefiniowanego powyżej na `NSFlipSwitch` klasy i zapisać bieżący **wartość** po kliknięciu przez użytkownika w formancie.

Opcjonalnie można możemy powrót do konstruktora interfejsu i zdefiniuj **akcji** w formancie:

[![](custom-controls-images/custom06.png "Konfigurowanie nowej akcji")](custom-controls-images/custom06.png#lightbox)

Ponownie, Edytuj `ViewController.cs` pliku i dodaj następującą metodę:

```csharp
partial void OptionTwoFlipped (Foundation.NSObject sender) {
    // Display the state of the option switch
    Console.WriteLine("Option Two: {0}", OptionTwo.Value);
}
```

> [!IMPORTANT]
> Należy używać **zdarzeń** lub zdefiniuj **akcji** w Konstruktorze interfejsu, ale nie należy używać obu metod, w tym samym czasie lub mogą powodować konflikty ze sobą.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule trwało szczegółowe przyjrzeć się tworzenie wielokrotnego użytku niestandardowe kontrolki interfejsu użytkownika w aplikacji Xamarin.Mac. Widzieliśmy sposób rysowania niestandardowych formantów interfejsu użytkownika, dwie główne metody odpowiedzieć na myszy i danych wprowadzonych przez użytkownika i ujawnia nowy formant do działań w Konstruktorze interfejsu w środowisku Xcode.

## <a name="related-links"></a>Linki pokrewne

- [MacCustomControl (przykład)](https://developer.xamarin.com/samples/mac/MacCustomControl/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Powiązanie danych i kodowanie klucz-wartość](~/mac/app-fundamentals/databinding.md)
- [OS X człowieka Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Obsługa zdarzeń myszy](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/EventOverview/HandlingMouseEvents/HandlingMouseEvents.html)
