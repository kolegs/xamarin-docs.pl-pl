---
title: "Paski narzędzi"
description: "W tym artykule opisano pracy z paski narzędzi w aplikacji Xamarin.Mac. Obejmuje ona tworzenie i obsługa paski narzędzi w środowisku Xcode i interfejs konstruktora, ujawnienia ich do kodu i pracy z nimi programistycznie."
ms.topic: article
ms.prod: xamarin
ms.assetid: C8D228CE-C860-47E1-85FD-69864BF91F20
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 641f62d5e646607b2ff61db412a5defce63a211d
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="toolbars"></a>Paski narzędzi

_W tym artykule opisano pracy z paski narzędzi w aplikacji Xamarin.Mac. Obejmuje ona tworzenie i obsługa paski narzędzi w środowisku Xcode i interfejs konstruktora, ujawnienia ich do kodu i pracy z nimi programistycznie._

Deweloperzy Xamarin.Mac pracy z programem Visual Studio dla komputerów Mac mają dostęp do tej samej formantów interfejsu użytkownika dostępnych dla deweloperów macOS Praca z Xcode, łącznie z formantem paska narzędzi. Ponieważ Xamarin.Mac integruje się bezpośrednio z Xcode, jest możliwe za pomocą konstruktora interfejsu w środowisku Xcode do tworzenia i obsługi elementów paska narzędzi. Te elementy paska narzędzi można również utworzyć w języku C#.

Paski narzędzi w macOS są dodawane do górnej części okna i zapewnić łatwy dostęp do poleceń związanych z jej funkcji. Paski narzędzi mogą być ukryte, pokazano lub dostosowane przez użytkowników aplikacji, a ich przedstawienia elementów paska narzędzi na różne sposoby.

W tym artykule opisano podstawy pracy z paski narzędzi i elementów paska narzędzi w aplikacji Xamarin.Mac. 

Przed kontynuowaniem zapoznaj się z artykułem [Hello, Mac](~/mac/get-started/hello-mac.md) artykułu — w szczególności [wprowadzenie do programów Xcode i kompilatora interfejsu](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) i [gniazda i akcje](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sekcje — ponieważ omawia kluczowe założenia i techniki, które będą używane w tym przewodniku.

Również Przyjrzyjmy się [udostępnianie klasy języka C# / metody Objective-C](~/mac/internals/how-it-works.md) sekcji [wewnętrzne Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentu. Wyjaśniono `Register` i `Export` atrybuty używane do nawiązania połączenia klas języka Objective-C klas C#.

## <a name="introduction-to-toolbars"></a>Wprowadzenie do paski narzędzi

Wszystkie okna aplikacji macOS mogą obejmować paska narzędzi:

![Przykładowe okno z paskiem narzędzi](toolbar-images/info01.png "okna przykład z paska narzędzi")

Paski narzędzi umożliwiają łatwe dla użytkowników aplikacji szybki dostęp do ważnych lub często używane funkcje. Na przykład aplikację edycji dokumentu może zawierać elementów paska narzędzi dla Ustawianie koloru tekstu, zmiana czcionki lub drukowania bieżącego dokumentu.

Paski narzędzi można wyświetlić elementów na trzy sposoby:

1. **Ikony, jak i tekstu** 

     ![Pasek narzędzi z ikonami i tekst](toolbar-images/info02.png "narzędzi z ikonami i tekstu")

2. **Tylko ikony** 

     ![Tylko do ikony paska narzędzi](toolbar-images/info03.png "tylko do ikony paska narzędzi")

3. **Tylko tekst** 

     ![Pasek narzędzi tekstowy](toolbar-images/info04.png "tylko tekst paska narzędzi")

Przełączania trybów przez kliknięcie prawym przyciskiem myszy pasek narzędzi i wybierając z menu kontekstowe tryb wyświetlania:

![Menu kontekstowe dla paska narzędzi](toolbar-images/info05.png "menu kontekstowe dla paska narzędzi")

Użyj tego samego menu, aby wyświetlać element toolbar na mniejszy rozmiar:

![Pasek narzędzi o małych ikon](toolbar-images/info06.png "narzędzi z małe ikony")

Menu umożliwia również dostosowywanie paska narzędzi:

![Używane do dostosowywania paska narzędzi okna dialogowego](toolbar-images/info07.png "używane do dostosowywania paska narzędzi okna dialogowego")

Podczas konfigurowania narzędzi w Konstruktorze interfejsu w programie Xcode, deweloper może zapewnić narzędzi dodatkowe elementy, które nie są częścią konfiguracji domyślnej. Użytkownicy aplikacji można następnie dostosować narzędzi, dodawanie i usuwanie tych elementów wstępnie zdefiniowane w razie potrzeby. Oczywiście można zresetować paska narzędzi w domyślnej konfiguracji.

Pasek narzędzi automatycznie łączy się z **widoku** menu, dzięki czemu użytkownicy Ukryj go wyświetlić i dostosować go:

![Elementy związane z paska narzędzi w menu Widok](toolbar-images/info08.png "elementów związanych z paska narzędzi, w menu Widok")

Zobacz [wbudowanej funkcji Menu](~/mac/user-interface/menu.md) dokumentację, aby uzyskać więcej szczegółowych informacji.

Ponadto jeśli pasek narzędzi jest prawidłowo skonfigurowany w konstruktora interfejsu, aplikacja będzie automatycznie zachowywane Dostosowywanie paska narzędzi między wiele powoduje uruchomienie aplikacji.

Następne sekcje w tym przewodniku opisano, jak można tworzyć i obsługiwać paski narzędzi z konstruktora interfejsu w środowisku Xcode i jak pracować z nimi w kodzie.

## <a name="setting-a-custom-main-window-controller"></a>Ustawianie kontrolera niestandardowego okna głównego

Do udostępnienia elementy interfejsu użytkownika do kodu C# za pomocą gniazda i akcje, aplikacja Xamarin.Mac użyć kontrolera niestandardowych okien:

1. Otwórz scenorysu aplikacji w Konstruktorze interfejsu w środowisku Xcode.
2. Wybierz kontroler okna na powierzchni projektu.
3. Przełącz się do **inspektora tożsamości** , a następnie wprowadź "WindowController" jako **Nazwa klasy**: 

    [![Ustawienie niestandardowej klasy nazwę kontrolera okna](toolbar-images/windowcontroller01.png "ustawienie niestandardowej klasy nazwę kontrolera okna")](toolbar-images/windowcontroller01-large.png) 

4. Zapisz zmiany i wróć do programu Visual Studio dla komputerów Mac do synchronizacji.
5. A **WindowController.cs** plik zostanie dodany do projektu w **konsoli rozwiązania** w programie Visual Studio dla komputerów Mac: 

    ![Wybieranie WindowController.cs w konsoli rozwiązania](toolbar-images/windowcontroller02.png "wybranie WindowController.cs w konsoli do rozwiązania")

6. Otwórz ponownie scenorysu w Konstruktorze interfejsu w środowisku Xcode.
7. **WindowController.h** pliku będą dostępne do użycia: 

    [![The WindowController.h file](toolbar-images/windowcontroller03.png "The WindowController.h file")](toolbar-images/windowcontroller03-large.png)

## <a name="creating-and-maintaining-toolbars-in-xcode"></a>Tworzenie i obsługę paski narzędzi w środowisku Xcode

Paski narzędzi są tworzone i obsługiwane z konstruktora interfejsu w środowisku Xcode. Aby dodać paska narzędzi do aplikacji, Edytuj scenorysu głównej aplikacji (w takim przypadku **Main.storyboard**) przez dwukrotne kliknięcie w **konsoli rozwiązania**:

![Otwieranie konsoli rozwiązania Main.storyboard](toolbar-images/edit01.png "otwieranie Main.storyboard w konsoli do rozwiązania")

W **inspektora biblioteki**, wprowadź "narzędzia" w **pole wyszukiwania** ułatwiające wszystkie elementy paska narzędzi dostępnych w temacie:

![Inspektor bibliotekę filtrowane w celu wyświetlania elementów paska narzędzi](toolbar-images/edit02.png "inspektora bibliotekę filtrowane w celu wyświetlania elementów paska narzędzi")

Przeciągnij pasek narzędzi okna w **Edytor interfejsu**. Za pomocą narzędzi wybrane, należy skonfigurować jego zachowanie przez ustawienie właściwości **inspektora atrybutów**:

![Inspektor atrybuty dla paska narzędzi](toolbar-images/edit04.png "inspektora atrybuty dla paska narzędzi")

Dostępne są następujące właściwości:

1. **Wyświetl** — Określa, czy pasek narzędzi wyświetla ikony i tekstu
2. **Widoczne podczas uruchamiania** — w przypadku wybrania paska narzędzi jest domyślnie widoczne.
3. **Można dostosować** — Jeśli zaznaczone, użytkownicy mogą edytować i dostosowywanie paska narzędzi.
4. **Separator** -zaznaczenie alokowania linia pozioma oddziela pasek narzędzi od zawartość okna.
5. **Rozmiar** -ustawia rozmiar paska narzędzi
6. **Automatyczne zapisywanie** -zaznaczenie aplikacji zostanie zachowany zmian konfiguracji paska narzędzi przez użytkownika po uruchomieniu aplikacji.

Wybierz **Autosave** opcji i pozostawić wszystkie inne właściwości ustawień domyślnych. 

Po otwarciu narzędzi **hierarchii interfejsów**, wywołaj okno dialogowe dostosowania, wybierając element paska narzędzi:

![Dostosowywanie paska narzędzi](toolbar-images/edit05.png "Dostosowywanie paska narzędzi")

To okno dialogowe można ustawić właściwości dla elementów, które są już częścią narzędzi do projektowania narzędzi domyślnego dla aplikacji, oraz zapewnienie dodatkowych narzędzi elementów dla użytkownika można wybierać podczas dostosowywania paska narzędzi. Aby dodać elementy do paska narzędzi, przeciągnij je z **inspektora biblioteki**:

![Biblioteka inspektora](toolbar-images/edit06.png "inspektora biblioteki")

Mogą być dodawane następujące elementy paska narzędzi:

- **Obraz paska narzędzi elementu** — elementem paska narzędzi z niestandardowego obrazu w postaci ikony.
- **Elastyczne element Toolbar miejsce** -elastyczne miejsca Justify narzędzi kolejnych elementów. Na przykład jeden lub więcej elementów paska narzędzi następuje elementem paska narzędzi miejsca elastyczne i innym elementem paska narzędzi czy Przypnij ostatni element z prawej strony paska narzędzi.
- **Miejsce na pasku narzędzi elementu** -stały odstęp między elementami na pasku narzędzi
- **Element paska narzędzi separatora** -widoczne separatora między co najmniej dwa elementy paska narzędzi, do grupowania
- **Dostosowywanie paska narzędzi elementu** — umożliwia dostosowywanie paska narzędzi
- **Wydrukować element paska narzędzi** — pozwala użytkownikom na drukowanie otwartego dokumentu
- **Pokaż element Toolbar kolory** -Wyświetla próbnika kolorów w standardowym systemie: 

     ![Selektor kolorów systemu](toolbar-images/edit07.png "próbnika kolorów systemu")

- **Pokaż czcionki narzędzi element** -Wyświetla okno dialogowe czcionki w standardowym systemie: 

     ![Selektor czcionki](toolbar-images/edit08.png "selektora czcionki")

> [!IMPORTANT]
> Jak będzie można później, wiele standardowych formantów Cocoa interfejsu użytkownika, takich jak pola wyszukiwania, formanty segmentu i poziomy suwaki można również dodać do paska narzędzi.

### <a name="adding-an-item-to-a-toolbar"></a>Dodawanie elementów do paska narzędzi

Aby dodać element do paska narzędzi, wybierz narzędzi **hierarchii interfejsów** i kliknij jeden z jego elementów, co uniemożliwia wyświetlać okno dialogowe dostosowania. Następnie przeciągnij nowego elementu z **inspektora biblioteki** do **dozwolone elementy paska narzędzi** obszar:

![Elementy paska narzędzi dozwolone w oknie dialogowym dostosowywania paska narzędzi](toolbar-images/add01.png "elementów paska narzędzi dozwolone w oknie dialogowym dostosowywania paska narzędzi")

Aby upewnić się, że nowy element jest częścią narzędzi domyślne, przeciągnij je do **elementy paska narzędzi domyślne** obszar: 

![Zmiana kolejności jest elementem paska narzędzi przez przeciągnięcie](toolbar-images/add02.png "przeciągając zmianę kolejności elementów paska narzędzi")

Aby zmienić kolejność elementów paska narzędzi domyślne, przeciągnij je lewo lub w prawo.

Następnie użyj **inspektora atrybuty** można ustawić właściwości domyślnej dla elementu:

![Dostosowywanie elementem paska narzędzi Inspektor atrybuty](toolbar-images/add03.png "Dostosowywanie elementem paska narzędzi Inspektor atrybutów")

Dostępne są następujące właściwości:

- **Nazwa obrazu** -obrazu do użycia jako ikony dla elementu
- **Etykieta** -tekst do wyświetlenia dla elementu w pasku narzędzi
- **Etykieta palety** -tekst do wyświetlenia dla elementu w **dozwolone elementy paska narzędzi** obszaru
- **Tag** -opcjonalne, unikatowy identyfikator, który pomaga zidentyfikować elementu w kodzie.
- **Identyfikator** — definiuje typ elementu paska narzędzi. Niestandardowa wartość może służyć do wybrania elementem paska narzędzi w kodzie.
- **Wybranie** — po zaznaczeniu elementu będzie działać tak jak przycisk wł. / wył.

> [!IMPORTANT]
> Dodaj element do **dozwolone elementy paska narzędzi** obszaru, ale nie narzędzi domyślne, aby oferować różne opcje dostosowywania dla użytkowników. 

### <a name="adding-other-ui-controls-to-a-toolbar"></a>Dodawanie innych formantów interfejsu użytkownika do paska narzędzi

Kilka elementów interfejsu użytkownika Cocoa, takie jak wyszukiwanie pól i formantów segmentu można również dodać do paska narzędzi.

Aby wypróbować ten, Otwórz pasek narzędzi w **hierarchii interfejsów** i wybierz element paska narzędzi, aby otworzyć okno dialogowe dostosowania. Przeciągnij **pole wyszukiwania** z **inspektora biblioteki** do **dozwolone elementy paska narzędzi** obszar:

![Korzystając z okna dialogowego Dostosowywanie paska narzędzi](toolbar-images/add05.png "za pomocą okna dialogowego Dostosowywanie paska narzędzi")

W tym miejscu Użyj konstruktora interfejsu, aby skonfigurować pola wyszukiwania i pozostawić ją do kodu za pomocą akcji lub gniazda.

## <a name="built-in-toolbar-item-support"></a>Obsługa elementu wbudowanego paska narzędzi

Kilka elementów interfejsu użytkownika Cocoa interakcję z elementami standardowym pasku narzędzi domyślnie. Na przykład, przeciągnij **widoku tekstu** na okna aplikacji i umieść go w celu wypełnienia obszaru zawartości:

[![Dodawanie widoku tekstu do aplikacji](toolbar-images/edit09.png "Dodawanie widoku tekstu do aplikacji")](toolbar-images/edit09-large.png)

Zapisz dokument, wróć do programu Visual Studio for Mac w celu synchronizacji z Xcode, uruchom aplikację, wprowadź tekst, zaznacz go i kliknij **kolory** element paska narzędzi. Należy zauważyć, że widoku tekstu automatycznie współpracuje z próbnika kolorów:

![Wbudowany pasek narzędzi funkcji za pomocą selektora widoku i kolor tekstu](toolbar-images/edit10.png "narzędzi wbudowanych funkcji za pomocą selektora widoku i kolor tekstu")

## <a name="using-images-with-toolbar-items"></a>Przy użyciu obrazów z elementów paska narzędzi

Przy użyciu **element paska narzędzi obrazu**, żadnego obrazu mapy bitowej dodane do **zasobów** folder (i akcją kompilacji **zasobów pakietu**) mogą być wyświetlane na pasku narzędzi jako ikona:

1. W programie Visual Studio dla komputerów Mac w **konsoli rozwiązania**, kliknij prawym przyciskiem myszy **zasobów** i wybierz polecenie **Dodaj** > **Dodaj pliki** .
2. Z **Dodaj pliki** okna dialogowego polu, przejdź do żądanego obrazów, zaznacz je i kliknij przycisk **Otwórz** przycisk: 

    [![Wybieranie obrazów, aby dodać](toolbar-images/edit11.png "wybierania obrazów do dodania")](toolbar-images/edit11-large.png)

3. Wybierz **kopiowania**, sprawdź **dla wszystkich wybranych plików, użyj tego samego akcji**i kliknij przycisk **OK**:

    ![Wybierając akcję kopiowania dla nowych obrazów](toolbar-images/edit12.png "wybierania akcji kopiowania dla nowych obrazów")

4. W **konsoli rozwiązania**, kliknij dwukrotnie **MainWindow.xib** go otworzyć w programie Xcode.

5. Wybieranie narzędzi **hierarchii interfejsów** i kliknij jeden z jego elementów, aby otworzyć okno dialogowe dostosowania.

6. Przeciągnij **element paska narzędzi obrazu** z **inspektora biblioteki** do paska narzędzi **dozwolone elementy paska narzędzi** obszar: 

    ![Element narzędzi Obraz dodany do obszaru dozwolone elementy paska narzędzi](toolbar-images/edit14.png "element narzędzi Obraz dodany do obszaru dozwolone elementy paska narzędzi")

7. W **inspektora atrybutów**, wybierz obraz, który został dodany w programie Visual Studio dla komputerów Mac: 

    ![Ustawienie niestandardowego obrazu dla elementu narzędzi](toolbar-images/edit15.png "ustawienie niestandardowego obrazu dla elementu paska narzędzi")

8. Ustaw **etykiety** do "Kosza" i **etykiety palety** "Wymazania dokumentu": 

    ![Ustawienie na pasku narzędzi elementu etykiety i etykiety palety](toolbar-images/edit16.png "ustawienie na pasku narzędzi elementu etykiety i etykiety palety")

9. Przeciągnij **element paska narzędzi separatora** z **inspektora biblioteki** do paska narzędzi **dozwolone elementy paska narzędzi** obszar: 

    [![Element separatora narzędzi dodany do obszaru dozwolone elementy paska narzędzi](toolbar-images/edit17.png "A separatora narzędzi element dodany do obszaru dozwolone elementy paska narzędzi")](toolbar-images/edit17-large.png)

10. Przeciągnij element separatora i elementu "Kosza" **domyślne elementy paska narzędzi** obszarów i zestaw elementów kolejności paska narzędzi z lewej do prawej w następujący sposób (kolory, czcionki, separatora, Kosz, elastyczne miejsca, drukowania): 

    ![Elementy paska narzędzi domyślne](toolbar-images/edit18.png "domyślne elementy paska narzędzi")

11. Zapisz zmiany i wróć do programu Visual Studio dla komputerów Mac do synchronizacji z usługą Xcode.

Uruchom aplikację, aby sprawdzić, czy domyślnie jest wyświetlany nowy pasek narzędzi:

![Pasek narzędzi z elementami dostosowane domyślne](toolbar-images/edit19.png "narzędzi z elementami dostosowane domyślne")

## <a name="exposing-toolbar-items-with-outlets-and-actions"></a>Udostępnianie elementów paska narzędzi z gniazda i działaniami

Aby uzyskać dostęp do paska narzędzi lub element paska narzędzi w kodzie, musi być dołączony do gniazda lub akcji:

1. W **konsoli rozwiązania**, kliknij dwukrotnie **Main.storyboard** go otworzyć w programie Xcode.
2. Upewnij się, że niestandardowej klasy "WindowController" została przypisana do kontrolera w głównym oknie **inspektora tożsamości**:

    [![Za pomocą Inspektora tożsamości można ustawić niestandardowej klasy kontrolera okna](toolbar-images/edit20a.png "Inspektor tożsamości można ustawić niestandardowej klasy kontrolera okna")](toolbar-images/edit20a-large.png)

3. Następnie wybierz element paska narzędzi w **hierarchii interfejsów**: 

    ![Wybranie elementu paska narzędzi w hierarchii interfejsów](toolbar-images/edit20.png "wybranie elementu paska narzędzi w hierarchii interfejsów")  

4. Otwórz **Asystenta widoku**, wybierz pozycję **WindowController.h** plików i przeciągnij formant z elementu narzędzi do **WindowController.h** pliku.
5. Ustaw **połączenia** typ **akcji**, wprowadź "trashDocument" **nazwa**i kliknij przycisk **Connect** przycisk: 

    [![Konfigurowanie akcji dla elementu narzędzi](toolbar-images/edit23.png "Konfigurowanie akcji dla elementu paska narzędzi")](toolbar-images/edit23-large.png)

6. Udostępnianie **widoku tekstu** jako gniazda o nazwie "documentEditor" w **ViewController.h** pliku: 

    [![Konfigurowanie sposobu wykorzystania widoku tekstu](toolbar-images/edit24.png "Konfigurowanie gniazda dla widoku tekstu")](toolbar-images/edit24-large.png)

7. Zapisz zmiany i wróć do programu Visual Studio for Mac synchronizację w programie Xcode.

W programie Visual Studio dla komputerów Mac, należy edytować **ViewController.cs** pliku i Dodaj następujący kod:

```csharp
public void EraseDocument() {
    documentEditor.Value = "";
}
```

Następnie Edytuj **WindowController.cs** pliku i Dodaj następujący kod do dołu `WindowController` klasy:

```csharp
[Export ("trashDocument:")]
void TrashDocument (NSObject sender) {

    var controller = ContentViewController as ViewController;
    controller.EraseDocument ();
}
```

Podczas uruchamiania aplikacji, **Kosza** elementu toolbar będzie aktywna:

![Pasek narzędzi z elementem Kosza active](toolbar-images/edit25.png "narzędzi z elementem active Kosza")

Zwróć uwagę, że **Kosza** element paska narzędzi można teraz używać do usuwania tekstu.

## <a name="disabling-toolbar-items"></a>Wyłączanie elementów paska narzędzi

Aby wyłączyć element na pasku narzędzi, Utwórz niestandardowe `NSToolbarItem` klasy i zastąpić `Validate` metody. Następnie konstruktora interfejsu przypisać niestandardowego typu elementu, który chcesz włączyć/wyłączyć.

Aby utworzyć niestandardowy `NSToolbarItem` klasy, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **nowego pliku...** . Wybierz **ogólne** > **pustą klasę**, wprowadź "ActivatableItem" **nazwa**i kliknij przycisk **nowy** przycisk: 

![Dodawanie pustą klasę w programie Visual Studio for Mac](toolbar-images/custom01.png "Dodawanie pustą klasę w programie Visual Studio dla komputerów Mac")

Następnie Edytuj **ActivatableItem.cs** pliku do odczytu w następujący sposób:

```csharp
using System;

using Foundation;
using AppKit;

namespace MacToolbar
{
    [Register("ActivatableItem")]
    public class ActivatableItem : NSToolbarItem
    {
        public bool Active { get; set;} = true;

        public ActivatableItem ()
        {
        }

        public ActivatableItem (IntPtr handle) : base (handle)
        {
        }

        public ActivatableItem (NSObjectFlag  t) : base (t)
        {
        }

        public ActivatableItem (string title) : base (title)
        {
        }

        public override void Validate ()
        {
            base.Validate ();
            Enabled = Active;
        }
    }
}
```

Kliknij dwukrotnie **Main.storyboard** go otworzyć w programie Xcode. Wybierz **Kosza** element paska narzędzi utworzone powyżej i zmień jego klasy "ActivatableItem" w **inspektora tożsamości**:

![Ustawienie niestandardowej klasy dla elementu narzędzi](toolbar-images/custom02.png "ustawienie niestandardowej klasy dla elementu paska narzędzi")

Tworzenie gniazda o nazwie `trashItem` dla **Kosza** element paska narzędzi. Zapisz zmiany i wróć do programu Visual Studio dla komputerów Mac do synchronizacji z usługą Xcode. Na koniec Otwórz **MainWindow.cs** i zaktualizuj `AwakeFromNib` metody do odczytu w następujący sposób:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Disable trash
    trashItem.Active = false;
}
```

Uruchom aplikację i zwróć uwagę, że **Kosza** element jest teraz wyłączony na pasku narzędzi:

![Pasek narzędzi z elementem nieaktywne Kosza](toolbar-images/custom03.png "narzędzi z elementem nieaktywne Kosza")

## <a name="summary"></a>Podsumowanie

W tym artykule trwało szczegółowe przyjrzeć się praca z paski narzędzi i elementów paska narzędzi w aplikacji Xamarin.Mac. Go opisano, jak tworzyć i obsługiwać paski narzędzi w Konstruktorze interfejsu w środowisku Xcode, jak niektórych formantów interfejsu użytkownika automatycznie współpracuje z elementów paska narzędzi, jak pracować z paski narzędzi w kodzie języka C# i sposobu włączania i wyłączania elementów paska narzędzi.


## <a name="related-links"></a>Linki pokrewne

- [MacToolbar (przykład)](https://developer.xamarin.com/samples/mac/MacToolbar/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Wskazówki dotyczące interfejsu dla pasków narzędzi](https://developer.apple.com/macos/human-interface-guidelines/windows-and-views/toolbars/)
- [Wprowadzenie do paski narzędzi](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Toolbars/Toolbars.html)
