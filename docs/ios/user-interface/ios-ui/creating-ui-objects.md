---
title: Tworzenie obiektów interfejsu użytkownika platformy Xamarin.iOS
description: Ten dokument zawiera omówienie sposobu tworzenia interfejsu użytkownika w Xamarin.iOS. Zawarto informacje iOS projektanta, konstruktora interfejsu Xcode, C# i scenorys.
ms.prod: xamarin
ms.assetid: 4D6B136C-744A-4936-8655-A77E62BA7A60
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: c688dcdf7498b0a2860d1878d893beae4f5cf8fc
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790155"
---
# <a name="creating-user-interface-objects-in-xamarinios"></a>Tworzenie obiektów interfejsu użytkownika platformy Xamarin.iOS

Apple grupuje pokrewne elementy funkcji do "platformy", które są równoważne Xamarin.iOS przestrzeni nazw. `UIKit` jest przestrzenią nazw, który zawiera wszystkie kontrolki interfejsu użytkownika dla systemu iOS.

Zawsze, gdy kod musi odwoływać kontrolki interfejsu użytkownika, takie jak etykiety lub przycisku, pamiętaj, aby uwzględnić następujące instrukcję using:

```csharp
using UIKit;
```

Wszystkie opcje omówionych w tym rozdziale znajdują się w przestrzeni nazw UIKit i ma nazwę klasy formantu każdego użytkownika `UI` prefiks.

Można edytować kontrolki interfejsu użytkownika i układy na trzy sposoby:

-  **[Xamarin iOS projektanta](~/ios/user-interface/designer/index.md)**  — Xamarin Użyj wbudowanego układu projektanta do projektowania ekranów. Kliknij dwukrotnie scenorysu lub pliki XIB można edytować za pomocą wbudowanych projektanta.
-  **Konstruktor interfejsu Xcode** — przeciągnij formanty z konstruktora interfejsu układów ekranu. Otwórz plik scenorysu lub XIB w środowisku Xcode, klikając prawym przyciskiem myszy plik w **konsoli rozwiązania** i wybierając polecenie **Otwórz za pomocą > konstruktora interfejsu Xcode**.
-  **Przy użyciu języka C#** — formanty również można skonstruować z kodem programowo i dodane do hierarchii widoku.

Można dodawać nowe pliki scenorysu i XIB prawym przyciskiem myszy projekt iOS, a następnie wybierając **Dodaj > Nowy plik...** .

Niezależnie od wybranej metody, właściwości formantu i zdarzenia nadal można modyfikować za pomocą języka C# w logiki aplikacji.

## <a name="using-xamarin-ios-designer"></a>Za pomocą platformy Xamarin iOS projektanta

Aby rozpocząć tworzenie interfejsu użytkownika w systemie iOS projektanta, kliknij dwukrotnie plik scenorysu. Formanty mogą być przeciągnięte na powierzchnię projektu z **przybornika** jak przedstawiono poniżej:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 [![](creating-ui-objects-images/image2b.png "Konsola przybornika")](creating-ui-objects-images/image2b.png#lightbox)
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

 [![](creating-ui-objects-images/image2b-vs.png "Konsola przybornika — Visual Studio")](creating-ui-objects-images/image2b.png#lightbox)
 
-----

Po wybraniu formantu na powierzchni projektowej **konsoli właściwości** wyświetli atrybutów dla tego formantu. **Elementu Widget > tożsamości > nazwa** pola, które zostanie wypełnione na poniższym zrzucie ekranu, jest używany jako *gniazda* nazwy. Jest to, jak można odwoływać się do formantu w języku C#:

 [![](creating-ui-objects-images/image3b.png "Właściwości elementu Widget konsoli")](creating-ui-objects-images/image3b.png#lightbox)

Aby uzyskać bardziej zgłębić temat do przy użyciu narzędzia Projektant z systemem iOS, zobacz [wprowadzenie do projektanta dla systemu iOS](~/ios/user-interface/designer/introduction.md) przewodnik.

## <a name="using-xcode-interface-builder"></a>Za pomocą konstruktora Xcode — interfejs

Jeśli użytkownik nie zna przy użyciu narzędzia Konstruktor interfejsu, można skorzystać z firmy Apple [konstruktora interfejsu](https://developer.apple.com/xcode/interface-builder/) dokumentów.

Aby otworzyć scenorysu w środowisku Xcode, kliknij prawym przyciskiem myszy dostęp do menu kontekstowego dla pliku scenorysu i wybierz opcję otwarcia z **Xcode interfejsu konstruktora**:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 [![](creating-ui-objects-images/imagexcode.png "Menu kontekstowe scenorysu - Xcode")](creating-ui-objects-images/imagexcode.png#lightbox)
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](creating-ui-objects-images/imagexcode-vs.png "Menu kontekstowe scenorysu - Xcode")](creating-ui-objects-images/imagexcode-vs.png#lightbox)

-----

Formanty mogą być przeciągnięte na powierzchnię projektu z **obiekt bibliotece** zostały pokazane poniżej:

 [![](creating-ui-objects-images/image5a.png "Biblioteka Xcode")](creating-ui-objects-images/image5a.png#lightbox)

Podczas projektowania interfejsu użytkownika z interfejsu konstruktora, należy utworzyć **gniazda** dla każdego formantu, który ma zostać odwołanie w C#. Jest to realizowane przez włączenie **Edytor Asystenta** przy użyciu Centrum **edytor** przycisk na pasku narzędzi Xcode:

 [![](creating-ui-objects-images/image6a.png "Przycisk Asystenta edytora")](creating-ui-objects-images/image6a.png#lightbox)

Kliknij obiekt interfejsu użytkownika; następnie **przeciągnij formant** do pliku .h. Aby ** kontrolki przeciągania **, naciśnij i przytrzymaj klawisz sterowania, a następnie kliknij i przytrzymaj za pośrednictwem obiektu interfejsu użytkownika, które tworzysz dla gniazda (lub akcji). Zachowaj przytrzymując naciśnięty klawisz kontroli podczas przeciągania do pliku nagłówka. Zakończ przeciąganie poniżej `@interface` definicji. Niebieska linia powinna zostać wyświetlona z podpis Wstaw gniazda lub kolekcji gniazda, jak pokazano na poniższym zrzucie ekranu.

Po zwolnieniu kliknięcie pojawi się monit o podanie nazwy dla gniazda, która będzie używana do tworzenia właściwości C#, która może być przywoływany w kodzie:

 [![](creating-ui-objects-images/image8a.png "Tworzenie gniazda")](creating-ui-objects-images/image8a.png#lightbox)

Aby uzyskać więcej informacji na jak konstruktora interfejsu w środowisku Xcode integruje się z programem Visual Studio dla komputerów Mac, zapoznaj się [generowania kodu Xib](~/ios/internals/xib-code-generation.md#generated) dokumentu.

##  <a name="using-c"></a>Przy użyciu języka C#

Jeśli zdecydujesz się programowane Tworzenie obiektu interfejsu użytkownika przy użyciu języka C# (w widoku lub widok kontroler, na przykład), wykonaj następujące kroki:

-  Deklarowanie klasy poziomu pola dla obiekt interfejsu użytkownika. Utwórz formancie raz, w `ViewDidLoad` np. Następnie można odwoływać się obiekt w całym cyklu życia metody kontroler widoku (np.
`ViewWillAppear`).
-  Utwórz `CGRect` definiuje ramki formantu (jego współrzędne X i Y na ekranie, a także szerokości i wysokości). Należy się upewnić, że masz `using CoreGraphics` dla tej dyrektywy.
-  Wywołanie konstruktora, aby utworzyć i przypisać formantu.
-  Ustaw właściwości ani programów obsługi zdarzeń.
-  Wywołanie `Add()` można dodać kontrolki do hierarchii widoku.

Poniżej przedstawiono prosty przykład tworzenia `UILabel` w kontrolera widoku przy użyciu języka C#:

```csharp
UILabel label1;
public override void ViewDidLoad () {
    base.ViewDidLoad ();
    var frame = new CGRect(10, 10, 300, 30);
    label1 = new UILabel(frame);
    label1.Text = "New Label";
    View.Add (label1);
}
```

<a name="partial_classes" />

## <a name="using-c-and-storyboards"></a>Przy użyciu języka C# i planów

Woluminowi kontrolerów widok na powierzchnię projektu dwóch odpowiednie pliki C# są tworzone w projekcie. W tym przykładzie `ControlsViewController.cs` i `ControlsViewController.designer.cs` tworzone automatycznie:

 [![](creating-ui-objects-images/image9b.png "Klasy częściowe ViewController")](creating-ui-objects-images/image9b.png#lightbox)

`MainViewController.cs` Plik jest przeznaczony do *kodu*. Jest to, gdy `View` metody cyklu takich jak `ViewDidLoad` i `ViewWillAppear` są implementowane i której można dodać własne właściwości, pól i metod.

`ControlsViewController.designer.cs` Jest wygenerowanego kodu zawierającego klasę częściową. Gdy nazwa formantu w projekcie powierzchni w programie Visual Studio dla komputerów Mac, albo utworzyć gniazda akcji w programie Xcode, odpowiadających im właściwości lub metody częściowej, jest dodawana do pliku projektanta (Designer.cs narzędzie). Poniższy kod przykładowy kod wygenerowany dla dwóch przycisków i widoku tekstu, gdy jeden z przycisków ma również `TouchUpInside` zdarzeń.

Te elementy klasy częściowej Włącz kod odwołują się do formantów i reagowanie na akcje, które są zadeklarowane na powierzchni projektu:

```csharp
[Register ("ControlsViewController")]
    partial class ControlsViewController
    {
        [Outlet]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        UIKit.UIButton Button1 { get; set; }

        [Outlet]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        UIKit.UIButton Button2 { get; set; }

        [Outlet]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        UIKit.UITextField textfield1 { get; set; }

        [Action ("button2_TouchUpInside:")]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        partial void button2_TouchUpInside (UIButton sender);

        void ReleaseDesignerOutlets ()
        {
            if (Button1 != null) {
                Button1.Dispose ();
                Button1 = null;
            }
            if (Button2 != null) {
                Button2.Dispose ();
                Button2 = null;
            }
            if (textfield1 != null) {
                textfield1.Dispose ();
                textfield1 = null;
            }
        }
    }
}
```

`designer.cs` Nie należy ręcznie edytować plik — IDE (Visual Studio for Mac lub Visual Studio) jest odpowiedzialny za on zsynchronizowany z scenorysu.

Jeśli obiekty interfejsu użytkownika są dodawane programowo do `View` lub `ViewController`, wystąpienia i zarządzanie nimi obiekt odwołuje się do siebie i dlatego plik projektanta nie jest wymagana.



## <a name="related-links"></a>Linki pokrewne

- [Formanty (przykład)](https://developer.xamarin.com/samples/Controls/)
