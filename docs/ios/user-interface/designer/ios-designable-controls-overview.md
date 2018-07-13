---
title: Kontrolki niestandardowe w Projektancie platformy Xamarin dla systemu iOS
description: Projektant platformy Xamarin dla systemu iOS obsługuje renderowanie kontrolek niestandardowych utworzone w projekcie lub odwołanie do ze źródeł zewnętrznych, takich jak Xamarin Component Store.
ms.prod: xamarin
ms.assetid: D8F07D63-B006-4050-9D1B-AC6FCDA71B99
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 05b190f4bfd4058e9e2f6e465e6026fa76dce6f4
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995700"
---
# <a name="custom-controls-in-the-xamarin-designer-for-ios"></a>Kontrolki niestandardowe w Projektancie platformy Xamarin dla systemu iOS

_Projektant platformy Xamarin dla systemu iOS obsługuje renderowanie kontrolek niestandardowych utworzone w projekcie lub odwołanie do ze źródeł zewnętrznych, takich jak Xamarin Component Store._

Projektant platformy Xamarin dla systemu iOS to zaawansowane narzędzie do wizualizacji w interfejsie użytkownika aplikacji i umożliwia obsługę większości iOS widoków i kontrolerów widoku do edycji w trybie WYSIWYG. Aplikacja może również zawierać niestandardowe formanty, które rozszerzają te wbudowane z systemem iOS. Jeśli kontrolki niestandardowe są zapisywane przy użyciu kilku zaleceń, pamiętając, również mogą one renderowane przez projektanta, zapewniając jeszcze większe doświadczenie w edytowaniu dla systemu iOS. W tym dokumencie przyjmuje się z tymi wytycznymi.

## <a name="requirements"></a>Wymagania

Na powierzchni projektowej będzie renderowany formant, który spełnia następujące wymagania:

1.  Jest podklasą bezpośrednich lub pośrednich [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/) lub [UIViewController](https://developer.xamarin.com/api/type/UIKit.UIView/Controller). Inne [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) podklasy pojawi się jako ikony na powierzchni projektowej.
2.  Ma ona [RegisterAttribute](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) je ujawnić Objective-c.
3.  Ma ona [wymaganego konstruktora IntPtr](~/ios/internals/api-design/index.md).
4.  Albo implementuje [IComponent](xref:System.ComponentModel.IComponent) interfejs lub ma [DesignTimeVisibleAttribute](xref:System.ComponentModel.DesignTimeVisibleAttribute) ustawiona na wartość True.

Po ich zawierającego projekt jest kompilowany dla symulatorze, formanty zdefiniowane w kodzie, które spełniają powyższe wymagania pojawi się w projektancie. Domyślnie wszystkie kontrolki niestandardowe będą wyświetlane w **składnikami niestandardowymi** części **przybornika**. Jednak [CategoryAttribute](xref:System.ComponentModel.CategoryAttribute) można zastosować do klasy formantu niestandardowego, aby określić inną sekcję.

Projektant nie obsługuje ładowania bibliotek języka Objective-C innych firm.

## <a name="custom-properties"></a>Właściwości niestandardowe

Zadeklarowana przez kontrolkę niestandardową właściwość pojawi się na panelu Właściwości, jeśli są spełnione następujące warunki:

1.  Właściwość ma publicznej metody pobierającej i ustawiającej.
1.  Właściwość ma [ExportAttribute](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) , a także [BrowsableAttribute](xref:System.ComponentModel.BrowsableAttribute) ustawiona na wartość True.
1.  Typ właściwości to typ liczbowy, typ wyliczeniowy, string, bool, [SizeF](xref:System.Drawing.SizeF), [UIColor](https://developer.xamarin.com/api/type/UIKit.UIColor/), lub [UIImage](https://developer.xamarin.com/api/type/UIKit.UIImage/). Ta lista obsługiwanych typów można rozwinąć w przyszłości.


Właściwość również może być dekorowane za pomocą [DisplayNameAttribute](xref:System.ComponentModel.DisplayNameAttribute) określić etykietę, która jest dla niego wyświetlany na panelu Właściwości.

## <a name="initialization"></a>Inicjalizacja

Aby uzyskać `UIViewController` podklasy, należy użyć [ViewDidLoad](https://developer.xamarin.com/api/member/UIKit.UIViewController.ViewDidLoad/) metody dla kodu, który jest zależny od widoki utworzone w projektancie.

Aby uzyskać `UIView` i innych `NSObject` podklasy, [AwakeFromNib](https://developer.xamarin.com/api/member/Foundation.NSObject.AwakeFromNib/) metodą jest zalecane miejsce przeprowadzić inicjowanie kontrolki niestandardowej po załadowaniu danych z pliku układu. Jest to spowodowane niestandardowe właściwości na panelu Właściwości nie zostanie ustawiona, gdy Konstruktor formantu jest uruchamiany, ale zostanie ustawiona przed `AwakeFromNib` nosi nazwę:


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

Kontrolka jest też przeznaczona do utworzenia bezpośrednio z kodu, można utworzyć metodę, która ma kod inicjalizacji wspólne, takie jak to:

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

## <a name="property-initialization-and-awakefromnib"></a>Inicjowanie właściwości i AwakeFromNib

Należy zachować ostrożność, kiedy i gdzie można zainicjować designable właściwości w niestandardowych składników, aby nie zastąpić wartości, które zostały ustawione w narzędziu iOS Designer. Na przykład wykonaj następujący kod:

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

`CustomView` Składnik udostępnia `Counter` właściwość, która może być ustawiona przez dewelopera w narzędziu iOS Designer. Jednakże, niezależnie od tego, jaka wartość jest ustawiona w projektancie, a wartość `Counter` właściwość zawsze będzie miała wartość zero (0). Oto Dlaczego:

-  Wystąpienie `CustomControl` jest zwiększony z pliku scenorysu.
-  Ustawiono żadnych właściwości zmodyfikowane w narzędzia iOS designer (np. ustawienie wartości `Counter` do dwóch (2), na przykład).
-  `AwakeFromNib` Metody jest wykonywane i wykonywane jest wywołanie do składnika `Initialize` metody.
-  Wewnątrz `Initialize` wartość `Counter` resetowania właściwości na wartość zero (0).


Aby rozwiązać problem powyżej sytuacji, albo zainicjować `Counter` gdzie indziej właściwości, (na przykład w Konstruktorze składnika) lub nie zastąpisz `AwakeFromNib` metody i wywołania `Initialize` Jeśli składnik są wymagane nie dalsze inicjowania poza co obecnie jest obsługiwane przez jego konstruktorów.

## <a name="design-mode"></a>Tryb projektowania

Na powierzchni projektowej formant niestandardowy spełnić kilka ograniczeń:

-  Zasoby pakietu aplikacji nie są dostępne w trybie projektowania. Obrazy są dostępne, gdy ładowanych za pośrednictwem [metody UIImage](https://developer.xamarin.com/api/type/UIKit.UIImage/%2fM) .
-  Asynchroniczne operacje, takie jak żądania sieci web, nie należy wykonać w trybie projektowania. Powierzchni projektu nie obsługuje animacji lub innych aktualizacji asynchronicznych do kontrolki interfejsu użytkownika.


Można zaimplementować formant niestandardowy [IComponent](xref:System.ComponentModel.IComponent) i użyj [tryb projektowania w systemie](xref:System.ComponentModel.ISite.DesignMode) właściwość, aby sprawdzić, czy jest na powierzchni projektowej. W tym przykładzie etykiety spowoduje wyświetlenie "Tryb projektowania" na powierzchni projektowej i "Środowiska uruchomieniowego" w czasie wykonywania:

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

Należy zawsze sprawdzić `Site` właściwość `null` przed podjęciem próby dostępu do żadnego z jej członków. Jeśli `Site` jest `null`, jest bezpiecznie przyjąć założenie, formant nie jest uruchomiona w projektancie.
W trybie projektowania `Site` zostanie ustawiona, po uruchomieniu formantu Konstruktor i przed `AwakeFromNib` jest wywoływana.

## <a name="debugging"></a>Debugowanie

Formant, który spełnia powyższe wymagania są wyświetlane w przyborniku i renderowane na powierzchni.
Jeśli formant nie są odtwarzane, sprawdź, czy błędy w formant lub jednej z jego zależności.

Powierzchni projektowej często może przechwytywać wyjątki zgłaszane przez poszczególnych formantów, przy jednoczesnym dalszym innych kontrolek Renderuj. Zastępuje uszkodzony kontrolki z czerwonym symbolem zastępczym, a śledzenia wyjątków można wyświetlić, klikając ikonę wykrzyknika:

 ![](ios-designable-controls-overview-images/exception-box.png "Formant wadliwe czerwony symbol zastępczy oraz szczegóły wyjątku")

Jeżeli symbole debugowania są dostępne dla formantu, śledzenia mają, nazwy i numery wierszy. Dwukrotne kliknięcie wiersza w śladzie stosu spowoduje przejście do tego wiersza w kodzie źródłowym.

Jeśli projektant nie może wyizolować wadliwe kontrolki, w górnej części powierzchni projektowej pojawi się komunikat ostrzegawczy:

 ![](ios-designable-controls-overview-images/info-bar.png "Komunikat ostrzegawczy w górnej części powierzchni projektowej")

Renderowanie pełną zostanie wznowione po udostępnieniu wadliwe kontroli ma ustalony rozmiar lub usunięte z powierzchni projektowej.

## <a name="summary"></a>Podsumowanie

Ten artykuł zawiera wprowadzenie, tworzenia i stosowania niestandardowych formantów w Projektancie dla systemu iOS. Po raz pierwszy opisany wymagań, które kontrolki musi spełniać, aby być renderowana na powierzchni projektowej i udostępnianie niestandardowych właściwości na panelu Właściwości. Następnie wyglądał w kodzie — inicjowanie kontrolki i właściwości tryb projektowania w systemie. Na koniec opisane co się stanie, gdy wyjątki zostaną zgłoszone i jak rozwiązać ten problem.