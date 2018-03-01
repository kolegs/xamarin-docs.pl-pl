---
title: Hello, Watch
description: Wprowadzenie do platformy Xamarin i watchOS
ms.topic: article
ms.prod: xamarin
ms.assetid: AD1DA488-51AB-420A-A0B7-3AE69A964A40
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/14/2016
ms.openlocfilehash: 88f9a86173756738d44f099b13177489226fa0e1
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="hello-watch"></a>Hello, Watch

Po utworzeniu rozwiązania zgodnie z krokami w [instalacja i Konfiguracja](~/ios/watchos/get-started/installation.md), będzie miał 3 projektów:

- Nadrzędny aplikacji dla systemu iOS, który służy do instalacji lub innych zadań administracyjnych na urządzeniu. (Z innymi typami iOS rozszerzeń, to jest często określany jako "Kontener" aplikacji.) Przy użyciu aplikacji wyrażenie kontrolne, będzie możliwe dla użytkowników uruchomić aplikację czujki bez **kiedykolwiek** aplikację nadrzędnego;
- Rozszerzenie czujki, które zawiera kod programu app czujki; i
- Aplikacja czujki zawiera zasoby scenorysu i obrazów, które mają być renderowane na czujki.

Sprawdź, czy Twoje [odwołania są poprawne](~/ios/watchos/get-started/project-references.md): czy aplikacji nadrzędnej zawiera odwołanie do rozszerzenia i że rozszerzenie ma odwołanie do aplikacji czujki.

Upewnij się, że postępuj zgodnie z identyfikatorów pakietu \*.watchkitextension \*.watchkitapp Konwencji i że plik Info.plist rozszerzenia nie ma **identyfikator pakietu WKApp** wartość ustawiona na identyfikator pakietu Obejrzyj aplikacji.

Można teraz uruchomić aplikację czujki, ale ponieważ plik scenorysu w Twojej aplikacji wyrażenie kontrolne jest pusty, nie będą mogli powiedzieć.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-watch-images/projectstructure.png "Eksplorator rozwiązań")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-watch-images/vs-projectstructure.png "Eksplorator rozwiązań")

-----

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
    
Kliknij dwukrotnie Interface.storyboard czujki aplikacji w celu uruchomienia systemu Xamarin iOS projektanta (Jeśli na komputerze Mac można również prawym przyciskiem myszy i **Otwórz za pomocą > konstruktora interfejsu Xcode**)


1.  Upewnij się, **przybornika** i **właściwości** tablety są widoczne,
1.  Kliknij, aby wybrać kontroler interfejsu
1.  Ustaw identyfikator i tytuł kontrolera interfejsu do **interfaceController** i **czujki Hi**,
1.  Sprawdź **klasy** ustawiono **InterfaceController**

    ![](hello-watch-images/interfacecontrollerattributes.png "Ustaw identyfikator i tytuł kontrolera interfejsu interfaceController i Hi czujki")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Kliknij dwukrotnie Interface.storyboard czujki aplikacji w celu edytować za pomocą platformy Xamarin iOS Projektant w programie Visual Studio:

1.  Otwórz w okienku właściwości;
1.  Zmień klasę, aby **InterfaceController**;
1.  Kliknij kontroler interfejsu; i
1.  Ustaw identyfikator i tytuł kontrolera interfejsu do **interfaceController** i **czujki Hi**.

    ![](hello-watch-images/vs-interfacecontrollerattributes.png "Ustaw identyfikator i tytuł kontrolera interfejsu interfaceController i Hi czujki")

-----


Tworzenie interfejsu użytkownika:

1. Z **przybornika** konsoli
1. Przeciągnij i upuść **przycisk** i **etykiety** na scenie, i
1. Ustawianie tekstu i atrybuty kontrolki, jak pokazano:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-watch-images/draganddrop.png "Ustawianie tekstu i atrybuty kontrolki, jak pokazano")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-watch-images/vs-draganddrop.png "Ustawianie tekstu i atrybuty kontrolki, jak pokazano")

-----

1. Ustaw **nazwa** w **właściwości** konsoli dla każdego formantu. W tym przykładzie mamy używano `myButton` i `myLabel`.

1. Wybierz przycisk dla scenorysu, przejdź do **właściwości** pad **zdarzenia** listy, następnie

1. Utwórz nową **akcji** , wpisując `OnButtonPress` i naciskając klawisz **Enter**.
  Akcja zostanie wyświetlony na liście, a metoda częściowa zostanie automatycznie utworzone w języku C#.

![](hello-watch-images/buttonaction.png "Akcja OnButtonPress dodane do przycisku")

Po zapisaniu scenorysu, **InterfaceController.designer.cs** pobiera zaktualizowane nazw kontrolek i akcje. Jeśli po jego zaktualizował możesz otworzyć ten plik, można zobaczyć sposób `RegisterAttribute` odpowiada kontroler i jak kontrolek interfejsu użytkownika odpowiadają C# zmienne wystąpienia oznaczone `OutletAttribute` i jak akcje są mapowane do metod częściowych oznaczone `ActionAttribute`:

```csharp
// WARNING
//
// This file has been generated automatically by Visual Studio for Mac from the outlets and
// actions declared in your storyboard file.
// Manual changes to this file will not be maintained.
//
[Register ("InterfaceController")]
partial class InterfaceController
{
    [Outlet]
    [GeneratedCode ("iOS Designer", "1.0")]
    WatchKit.WKInterfaceButton myButton { get; set; }

    [Outlet]
    [GeneratedCode ("iOS Designer", "1.0")]
    WatchKit.WKInterfaceLabel myLabel { get; set; }

    [Action ("OnButtonPress:")]
    [GeneratedCode ("iOS Designer", "1.0")]
    partial void OnButtonPress (WatchKit.WKInterfaceButton sender);

    void ReleaseDesignerOutlets ()
    {
        if (myButton != null) {
            myButton.Dispose ();
            myButton = null;
        }
        if (myLabel != null) {
            myLabel.Dispose ();
            myLabel = null;
        }
    }
}
```

Teraz otworzyć **InterfaceController.cs** (*nie* InterfaceController.designer.cs) i Dodaj następujący kod:

```csharp
int clickCount = 0;

partial void OnButtonPress (WatchKit.WKInterfaceButton sender)
{
  var msg = String.Format("Clicked {0} times", ++clickCount);
  myLabel.SetText(msg);
}

```

Ten kod powinien być dość przezroczysty: zmienna wystąpienia `clickCount` jest zwiększany zawsze funkcja `OnButtonPress` jest wywoływana. Tekst `myLabel` jest zmienione w celu uwzględnienia tej liczby; `myLabel`, oczywiście jest nazwą jednego z gniazda utworzony w programie XCode. `partial` Funkcji jest implementacją funkcji skojarzona nazwa akcji określona.

Jeśli nie jest jeszcze projekt startowy

1. Kliknij prawym przyciskiem myszy projekt rozszerzenia czujki i wybierz polecenie **Ustaw jako projekt startowy**,

1. Ustaw cel wdrożenia do obrazu symulatora zgodnego zestawu czujki (na przykład iPhone 6 iOS 8.2),

1. Naciśnij klawisz **debugowania** przycisk, aby wyzwolić uruchomienie kompilacji i symulatora.

    [ ![](hello-watch-images/readytodebug-sml.png "Elementy interfejsu programu Visual Studio")](hello-watch-images/readytodebug.png)

Podczas uruchamiania symulatora, naciśnij przycisk, aby zwiększyć etykiety.
Gratulacje, otrzymasz samodzielnie aplikacji czujki!

![](hello-watch-images/running.png "Aplikacji działających w symulatorze")


## <a name="related-links"></a>Linki pokrewne

- [Wprowadzenie (przykład)](https://developer.xamarin.com/samples/monotouch/WatchKit/GettingStarted/)
- [Instalacja i Konfiguracja](~/ios/watchos/get-started/installation.md)
- [Pierwszy wideo aplikacji czujki](http://blog.xamarin.com/your-first-watch-kit-app/)
