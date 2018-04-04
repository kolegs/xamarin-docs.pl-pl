---
title: Niestandardowe formanty w Projektancie Xamarin dla systemu iOS
description: Projektant Xamarin dla systemu iOS obsługuje renderowanie formantów niestandardowych utworzone w projekcie, lub odwołanie z zewnętrznych źródeł, takich jak magazyn składników Xamarin.
ms.prod: xamarin
ms.assetid: D8F07D63-B006-4050-9D1B-AC6FCDA71B99
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 113fab2fd0d1a055d566606885cefbafe3185529
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="custom-controls-in-the-xamarin-designer-for-ios"></a>Niestandardowe formanty w Projektancie Xamarin dla systemu iOS

_Projektant Xamarin dla systemu iOS obsługuje renderowanie formantów niestandardowych utworzone w projekcie, lub odwołanie z zewnętrznych źródeł, takich jak magazyn składników Xamarin._

Projektant Xamarin dla systemu iOS jest zaawansowanym narzędziem do wizualizacji interfejsu użytkownika aplikacji i zapewnia WYSIWYG edycji obsługę większości widoków dla systemu iOS i kontrolerów widoku. Aplikacja może także zawierać niestandardowe formanty rozszerzające z nich z systemem iOS. Jeśli kontrolki niestandardowe są zapisywane z kilku zaleceń, pamiętając, mogą one również renderowane przez system iOS projektanta, zapewniając doświadczenie w edytowaniu bardziej szczegółowego. Ten dokument ma przyjrzeć się tymi wytycznymi.

## <a name="requirements"></a>Wymagania

Formant, który spełnia następujące wymagania będzie odtwarzany na powierzchni projektu:

1.  Jest podklasą bezpośrednie lub pośrednie [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/) lub [UIViewController](https://developer.xamarin.com/api/type/UIKit.UIView/Controller). Inne [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) podklasy pojawi się ikona na powierzchnię projektu.
2.  Ma ona [RegisterAttribute](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) je ujawnić Objective-c.
3.  Ma ona [wymaganego konstruktora IntPtr](~/ios/internals/api-design/index.md).
4.  Albo implementuje [IComponent](https://developer.xamarin.com/api/type/System.ComponentModel.IComponent/) interfejsu lub ma [DesignTimeVisibleAttribute](https://developer.xamarin.com/api/type/System.ComponentModel.DesignTimeVisibleAttribute/) ustawiona na wartość True.

Formanty zdefiniowane w kodzie, które spełniają powyższe wymagania będą wyświetlane w Projektancie podczas ich zawierający projekt jest kompilowany dla symulatorze. Domyślnie wszystkie kontrolki niestandardowe będą wyświetlane w **niestandardowych składników** sekcji **przybornika**. Jednak [CategoryAttribute](https://developer.xamarin.com/api/type/System.ComponentModel.CategoryAttribute/) może odnosić się do klasy formantu niestandardowego do określenia innej sekcji.

Projektant nie obsługuje ładowania bibliotek języka Objective-C innych firm.

## <a name="custom-properties"></a>Właściwości niestandardowe

Właściwość deklarowana przez kontrolkę niestandardową będą wyświetlane w panelu właściwość, jeśli są spełnione następujące warunki:

1.  Właściwość ma publicznej metody pobierającej i ustawiającej.
1.  Właściwość ma [ExportAttribute](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) , jak również [BrowsableAttribute](https://developer.xamarin.com/api/type/System.ComponentModel.BrowsableAttribute/) ustawiona na wartość True.
1.  Typ właściwości jest typ liczbowy, typu wyliczeniowego, string, bool, [SizeF](https://developer.xamarin.com/api/type/System.Drawing.SizeF/), [UIColor](https://developer.xamarin.com/api/type/UIKit.UIColor/), lub [UIImage](https://developer.xamarin.com/api/type/UIKit.UIImage/). Ta lista obsługiwanych typów może obejmować w przyszłości.


Właściwość może również być dekorowane za [DisplayNameAttribute](https://developer.xamarin.com/api/type/System.ComponentModel.DisplayNameAttribute/) do określenia etykietę, którą jest dla niego wyświetlany w panelu właściwość.

## <a name="initialization"></a>Inicjalizacja

Dla `UIViewController` podklasy, należy użyć [ViewDidLoad](https://developer.xamarin.com/api/member/UIKit.UIViewController.ViewDidLoad/) metody dla kodu, który jest zależny od widoki utworzone w projektancie.

Aby uzyskać `UIView` i innych `NSObject` podklasy, [AwakeFromNib](https://developer.xamarin.com/api/member/Foundation.NSObject.AwakeFromNib/) metoda jest zalecane miejsce do wykonania inicjowania formantu niestandardowego po załadowaniu go z pliku układu. Wynika to z faktu właściwości niestandardowe w panelu właściwość nie zostanie ustawiona, po uruchomieniu formantu konstruktora, ale zostanie ustawiona przed `AwakeFromNib` nosi nazwę:


```csharp
[Register ("CustomView"), DesignTimeVisible (true)]
public class CustomView : UIView {

    public CustomView (IntPtr handle) : base (handle) { }

    public override void AwakeFromNib ()
    {
        // Initialize the view here.
    }
}
```

Jeśli do utworzenia bezpośrednio z kodu jest również zaprojektowana tak formantu, można Tworzenie metody, która ma typowe kod inicjujący, jak to:

```csharp
[Register ("CustomView"), DesignTimeVisible (true)]
public class CustomView : UIView {

    public CustomView (IntPtr handle) : base (handle) { }

    public CustomView ()
    {
        // Called when created from code.
        Initialize ();
    }

    public override void AwakeFromNib ()
    {
        // Called when loaded from xib or storyboard.
        Initialize ();
    }

    void Initialize ()
    {
        // Common initialization code here.
    }
}
```

## <a name="property-initialization-and-awakefromnib"></a>Właściwości inicjalizacji i AwakeFromNib

Należy zachować ostrożność, na kiedy i gdzie należy zainicjować designable właściwości w niestandardowych składników, aby nie zastąpienia wartości, które zostały ustawione w Projektancie z systemem iOS. Na przykład wykonaj następujący kod:

```csharp
[Register ("CustomView"), DesignTimeVisible (true)]
public class CustomView : UIView {
    
    [Export ("Counter"), Browsable (true)]
    public int Counter {get; set;}

    public CustomView (IntPtr handle) : base (handle) { }

    public CustomView ()
    {
        // Called when created from code.
        Initialize ();
    }

    public override void AwakeFromNib ()
    {
        // Called when loaded from xib or storyboard.
        Initialize ();
    }

    void Initialize ()
    {
        // Common initialization code here.
        Counter = 0;
    }
}
```

`CustomView` Przedstawia składnika `Counter` właściwość, którą można ustawić przez dewelopera wewnątrz iOS projektanta. Jednak niezależnie od tego, jakie ma wartość w Projektancie wartość `Counter` właściwości będzie zawsze równa zero (0). Poniżej przedstawiono przyczyny:

-  Wystąpienie `CustomControl` jest zwiększony z pliku scenorysu.
-  Wszystkie właściwości zmodyfikowany w Projektancie iOS są ustawione (np. ustawienie wartości `Counter` do dwóch (2), na przykład).
-  `AwakeFromNib` Metoda jest wykonywana i wywołanie elementu `Initialize` metody.
-  Wewnątrz `Initialize` wartość `Counter` resetowania właściwości zero (0).


Ustalenie powyżej sytuacji albo zainicjować `Counter` w innym miejscu właściwości, (na przykład w Konstruktorze składnika) lub nie powoduje zmiany `AwakeFromNib` — metoda i wywołanie `Initialize` Jeśli składnik są wymagane nie dalsze inicjowania poza co obecnie jest obsługiwane przez jej konstruktorów.

## <a name="design-mode"></a>Tryb projektowania

Na powierzchni projektowej kontrolkę niestandardową muszą być zgodne z kilku ograniczeń:

-  Zasoby pakietu aplikacji nie są dostępne w trybie projektowania. Obrazy są dostępne po załadowaniu przez [metody UIImage](https://developer.xamarin.com/api/type/UIKit.UIImage/%2fM) .
-  Operacje asynchroniczne, takich jak żądania sieci web, nie należy wykonać w trybie projektowania. Powierzchnia projektu nie obsługuje animacji lub inne aktualizacje asynchroniczne do formantu interfejsu użytkownika.


Kontrolkę niestandardową można zaimplementować [IComponent](https://developer.xamarin.com/api/type/System.ComponentModel.IComponent/) i użyj [tryb projektowania w systemie](https://developer.xamarin.com/api/property/System.ComponentModel.ISite.DesignMode/) właściwość, aby sprawdzić, czy jest na powierzchni projektu. W tym przykładzie etykiety wyświetli "Tryb projektowania" na powierzchni projektowej oraz "Runtime" w czasie wykonywania:

```csharp
[Register ("DesignerAwareLabel")]
public class DesignerAwareLabel : UILabel, IComponent {

    #region IComponent implementation

    public ISite Site { get; set; }
    public event EventHandler Disposed;

    #endregion

    public DesignerAwareLabel (IntPtr handle) : base (handle) { }

    public override void AwakeFromNib ()
    {
        if (Site != null &amp;&amp; Site.DesignMode)
            Text = "Design Mode";
        else
            Text = "Runtime";
    }
}
```

Należy zawsze sprawdzić `Site` właściwość `null` przed podjęciem próby ma dostęp do wszystkich jej członków. Jeśli `Site` jest `null`, bezpiecznie przejęcie kontroli nie działa w projektancie.
W trybie projektowania `Site` zostanie ustawiona, po uruchomieniu Konstruktora formantu i przed `AwakeFromNib` jest wywoływana.

## <a name="debugging"></a>Debugowanie

Formant, który spełnia wymagania są wyświetlane w przyborniku i renderowane na powierzchni.
Formant nie są odtwarzane, wyszukaj usterki w programie formant lub jeden z jego zależności.

Powierzchnię projektu można często przechwytuje wyjątków zgłaszanych przez pojedynczych formantów, pozostawiając renderowania inne formanty. Zastępuje uszkodzony formantu z czerwonym symbolem zastępczym i śledzenie wyjątku można wyświetlić, klikając ikonę wykrzyknika:

 ![](ios-designable-controls-overview-images/exception-box.png "Błędny formant jako czerwony symbol zastępczy i szczegóły wyjątku")

Jeśli symboli debugowania są dostępne dla formantu, śledzenie będzie mieć nazwy pliku i numery wierszy. Dwukrotnie klikając wiersz w ślad stosu spowoduje przejście do tej linii w kodzie źródłowym.

Jeśli projektant nie Izoluj błędny formantu w górnej części powierzchni projektu zostanie wyświetlony komunikat ostrzegawczy:

 ![](ios-designable-controls-overview-images/info-bar.png "Komunikat ostrzegawczy w górnej części powierzchni projektowej")

Pełna renderowania zostanie wznowiona, gdy uszkodzony kontroli jest rozwiązany lub usunięty z powierzchni projektu.

## <a name="summary"></a>Podsumowanie

W tym artykule wprowadzono tworzenia i stosowania niestandardowych formantów w Projektancie systemu iOS. Pierwszy opisano wymagania, które kontrolki muszą spełnić, aby być renderowana na powierzchni projektu i ujawnia właściwości niestandardowe w panelu właściwość. Przeglądał następnie kodzie - inicjowania formantu i właściwości tryb projektowania w systemie. Na koniec opisane co się stanie, gdy są zgłaszane wyjątki i jak rozwiązać ten problem.