---
title: Paski narzędzi na platformie Xamarin.Mac
description: W tym artykule opisano pracę z pasków narzędzi w aplikacji platformy Xamarin.Mac. Obejmuje ona tworzenie i obsługa paski narzędzi w programie Xcode i programu Interface Builder, udostępnianie ich do kodu i pracy z nimi programistycznie.
ms.prod: xamarin
ms.assetid: C8D228CE-C860-47E1-85FD-69864BF91F20
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 06faaf16ffd0adc64063bfa5a264c1895b9ca9cb
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780654"
---
# <a name="toolbars-in-xamarinmac"></a>Paski narzędzi na platformie Xamarin.Mac

_W tym artykule opisano pracę z pasków narzędzi w aplikacji platformy Xamarin.Mac. Obejmuje ona tworzenie i obsługa paski narzędzi w programie Xcode i programu Interface Builder, udostępnianie ich do kodu i pracy z nimi programistycznie._

Deweloperzy platformy Xamarin.Mac pracę z usługą Visual Studio for Mac mają dostęp do tych samych kontrolek interfejsu użytkownika dostępnych dla deweloperów systemu macOS pracy za pomocą edytora Xcode, łącznie z formantem paska narzędzi. Ponieważ rozszerzenia Xamarin.Mac integruje się bezpośrednio za pomocą edytora Xcode, jest możliwe użycie program Xcode Interface Builder do tworzenia i obsługi elementów paska narzędzi. Te elementy paska narzędzi można utworzyć w taki sposób, w języku C#.

Paski narzędzi w systemie macOS są dodawane do górnej części okna i zapewniają łatwy dostęp do poleceń dotyczących jego działanie. Paski narzędzi mogą być ukryte, wyświetlane lub dostosowywane przez użytkowników aplikacji i ich obecny elementów paska narzędzi na różne sposoby.

W tym artykule opisano podstawy pracy z pasków narzędzi i elementów paska narzędzi w aplikacji platformy Xamarin.Mac. 

Przed kontynuowaniem zapoznaj się z artykułem [Witaj, Mac](~/mac/get-started/hello-mac.md) artykułu — specjalnie [wprowadzenie do programu Xcode i programu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) i [gniazd i akcje](~/mac/get-started/hello-mac.md#outlets-and-actions) sekcje — ponieważ omawia kluczowe założenia i technik, które będą używane w tym przewodniku.

Ponadto zapoznaj się z [udostępnianie klasy języka C# / metod języka Objective-C](~/mac/internals/how-it-works.md) części [elementach wewnętrznych rozszerzenia Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentu. Wyjaśniono `Register` i `Export` atrybuty używane do łączenia z klas języka C# do języka Objective-C klas.

## <a name="introduction-to-toolbars"></a>Wprowadzenie do pasków narzędzi

Wszystkie okna aplikacji z systemem macOS może obejmować paska narzędzi:

![Okno przykład z paska narzędzi](toolbar-images/info01.png "okno przykład z paska narzędzi")

Paski narzędzi umożliwiają łatwe dla użytkowników aplikacji, można szybko uzyskać dostęp do ważnych lub często używane funkcje. Na przykład edytowanie dokumentów aplikacji może zawierać elementy paska narzędzi Ustawianie koloru tekstu, zmiana czcionki lub drukowania bieżącego dokumentu.

Paski narzędzi może wyświetlać elementy na trzy sposoby:

1. **Ikonę i tekst** 

     ![Pasek narzędzi za pomocą ikon i tekstu](toolbar-images/info02.png "narzędzi za pomocą ikon i tekstu")

2. **Tylko ikony** 

     ![Tylko do ikony paska narzędzi](toolbar-images/info03.png "tylko do ikony paska narzędzi")

3. **Tylko tekst** 

     ![Pasek narzędzi tekstowy](toolbar-images/info04.png "tekstowy paska narzędzi")

Przełączać się między tych trybów, klikając prawym przyciskiem myszy pasek narzędzi i wybierając tryb wyświetlania z menu kontekstowego:

![Menu kontekstowe dla paska narzędzi](toolbar-images/info05.png "menu kontekstowe dla paska narzędzi")

Aby wyświetlić pasek narzędzi w mniejszym rozmiarze, należy użyć tego samego menu:

![Pasek narzędzi przy użyciu małych ikon](toolbar-images/info06.png "narzędzi przy użyciu małych ikon")

Menu umożliwia także dostosowywanie paska narzędzi:

![Umożliwia dostosowywanie paska narzędzi okna dialogowego](toolbar-images/info07.png "służące do dostosowywania paska narzędzi okna dialogowego")

Podczas konfigurowania narzędzi w program Xcode Interface Builder, deweloper może zapewnić elementów dodatkowych narzędzi, które nie są częścią konfiguracji domyślnej. Użytkownicy aplikacji można następnie dostosować pasku narzędzi, dodając i usuwając te wstępnie zdefiniowane elementy zgodnie z potrzebami. Oczywiście pasek narzędzi można zresetować do konfiguracji domyślnej.

Pasek narzędzi automatycznie łączy się z **widoku** menu, które umożliwia użytkownikom je ukryć, wyświetlenia i dostosować go:

![Elementy związane z paska narzędzi, w menu Widok](toolbar-images/info08.png "związane z paska narzędzi elementów w menu Widok")

Zobacz [wbudowanej funkcji Menu](~/mac/user-interface/menu.md) dokumentacji, aby uzyskać więcej informacji.

Ponadto jeśli pasek narzędzi jest prawidłowo skonfigurowany w programu Interface Builder, aplikacja będzie automatycznie ulegają zmianie dostosowania paska narzędzi wielu uruchomień aplikacji.

W kolejnych sekcjach tego przewodnika opisano, jak tworzyć i obsługiwać paski narzędzi z program Xcode Interface Builder i jak pracować z nimi w kodzie.

## <a name="setting-a-custom-main-window-controller"></a>Ustawienia kontrolera niestandardowe okno główne

Aby udostępnić elementy interfejsu użytkownika do kodu C# za pośrednictwem gniazd i akcje, aplikacji rozszerzenia Xamarin.Mac w należy użyć kontrolera niestandardowe okno:

1. Otwórz scenorys aplikacji w program Xcode Interface Builder.
2. Wybierz kontroler okna na powierzchni projektowej.
3. Przełącz się do **Inspektor tożsamości** i wprowadzili "WindowController" **Nazwa klasy**: 

    [![Ustawienie nazwę niestandardowej klasy kontroler okna](toolbar-images/windowcontroller01.png "ustawienie nazwę niestandardowej klasy kontroler okna")](toolbar-images/windowcontroller01-large.png#lightbox) 

4. Zapisz zmiany i powrócić do programu Visual Studio dla komputerów Mac do synchronizacji.
5. A **WindowController.cs** plik zostanie dodany do projektu w **konsoli rozwiązania** w programie Visual Studio dla komputerów Mac: 

    ![Wybieranie WindowController.cs w konsoli rozwiązania](toolbar-images/windowcontroller02.png "wybierając WindowController.cs w konsoli rozwiązania")

6. Otwórz scenorys w program Xcode Interface Builder.
7. **WindowController.h** plik będzie dostępny do użytku: 

    [![The WindowController.h file](toolbar-images/windowcontroller03.png "The WindowController.h file")](toolbar-images/windowcontroller03-large.png#lightbox)

## <a name="creating-and-maintaining-toolbars-in-xcode"></a>Utworzenie i utrzymywanie paski narzędzi w środowisku Xcode

Paski narzędzi są tworzone i konserwowane z program Xcode Interface Builder. Aby dodać pasek narzędzi do aplikacji, Edytuj scenorysu głównej aplikacji (w tym przypadku **Main.storyboard**) przez dwukrotne kliknięcie go w **konsoli rozwiązania**:

![Otwieranie Main.storyboard w konsoli rozwiązania](toolbar-images/edit01.png "otwierania Main.storyboard w konsoli rozwiązania")

W **Inspektor biblioteki**, wprowadź "narzędzia" **pole wyszukiwania** ułatwiają Zobacz wszystkich elementów dostępnych narzędzi:

![Inspektor biblioteki filtrowana w celu wyświetlenia elementów paska narzędzi](toolbar-images/edit02.png "Inspector biblioteki, filtrowana w celu wyświetlenia elementów paska narzędzi")

Przeciągnij pasek narzędzi okna **Edytor interfejsu**. Za pomocą narzędzi zaznaczone, skonfiguruj jego zachowanie przez ustawienie właściwości **Inspektor atrybuty**:

![Inspektor atrybuty dla paska narzędzi](toolbar-images/edit04.png "Inspektor atrybuty dla paska narzędzi")

Dostępne są następujące właściwości:

1. **Wyświetlanie** — Określa, czy pasek narzędzi wyświetla ikon i tekstu
2. **Widoczne podczas uruchamiania** — w przypadku wybrania pasek narzędzi jest domyślnie widoczny.
3. **Można dostosować** — Jeśli zaznaczone, użytkownicy mogą edytować i dostosowywanie paska narzędzi.
4. **Separator** — Jeśli zaznaczone, Cienka pozioma linia oddziela pasek narzędzi od zawartość okna.
5. **Rozmiar** -ustawia rozmiar paska narzędzi
6. **Automatyczne zapisywanie** -zaznaczenie, aplikacja będzie ulegają zmianie zmiany konfiguracji narzędzi użytkownika uruchomień aplikacji.

Wybierz **zapisywania** opcji i pozostaw wszystkie pozostałe właściwości ustawień domyślnych. 

Po otwarciu narzędzi **hierarchii interfejsów**, wywołaj okno dialogowe dostosowywania, wybierając element paska narzędzi:

![Dostosowywanie paska narzędzi](toolbar-images/edit05.png "Dostosowywanie paska narzędzi")

Użyj tego okna dialogowego, aby ustawić właściwości dla elementów, które są już częścią narzędzi w projekcie narzędzi domyślnego dla aplikacji, i zapewnianie elementów dodatkowych narzędzi dla użytkownika wybrać, gdy Dostosowywanie paska narzędzi. Aby dodać elementy do paska narzędzi, przeciągnij je z **Inspektor biblioteki**:

![Inspektor biblioteki](toolbar-images/edit06.png "Inspektor biblioteki")

Można dodawać następujące elementy paska narzędzi:

- **Element paska narzędzi obrazów** — element paska narzędzi, za pomocą obrazu niestandardowego jako ikony.
- **Elastyczne element paska narzędzi miejsca** -elastyczne miejsce do uzasadnienie narzędzi kolejnych elementów. Na przykład jeden lub więcej elementów paska narzędzi następuje element paska narzędzi elastycznych miejsca, a inny element paska narzędzi, jak przypinasz ostatniego elementu na prawą stronę paska narzędzi.
- **Element paska narzędzi miejsca** — Naprawiono odstęp między elementami na pasku narzędzi
- **Element paska narzędzi separator** -widoczne separator między co najmniej dwóch elementów paska narzędzi do grupowania
- **Dostosowywanie paska narzędzi elementu** — umożliwia użytkownikom dostosowywanie paska narzędzi
- **Wydrukować element paska narzędzi** — pozwala użytkownikom na drukowanie otwartego dokumentu
- **Pokaż kolory paska narzędzi elementu** -wyświetla selektor kolorów standardowych systemowych: 

     ![Selektor kolorów systemu](toolbar-images/edit07.png "selektor kolorów systemu")

- **Pokaż czcionki narzędzi element** — Wyświetla okno dialogowe czcionki standardowych systemowych: 

     ![Selektor czcionki](toolbar-images/edit08.png "selektor czcionki")

> [!IMPORTANT]
> Co zostało przedstawione później, wiele standardowych kontrolek cocoa dla interfejsu użytkownika, takie jak pola wyszukiwania, poziomy suwaki i kontrolki segmentowane mogą również dodawane do paska narzędzi.

### <a name="adding-an-item-to-a-toolbar"></a>Dodanie elementu do paska narzędzi

Aby dodać element do paska narzędzi, wybierz pasek narzędzi w **hierarchii interfejsów** i kliknij jeden z jego elementów, co uniemożliwia wyświetlać okno dialogowe dostosowywania. Następnie przeciągnij nowy element z **Inspektor biblioteki** do **dozwolone elementy paska narzędzi** obszaru:

![Elementy paska narzędzi dozwolone w oknie dialogowym dostosowywania paska narzędzi](toolbar-images/add01.png "elementów paska narzędzi dozwolone w oknie dialogowym dostosowywania paska narzędzi")

Aby upewnić się, że nowy element jest częścią narzędzi domyślne, przeciągnij ją do **domyślne elementy paska narzędzi** obszaru: 

![Zmiana kolejności elementów paska narzędzi, przeciągając](toolbar-images/add02.png ", przeciągając zmiany kolejności elementów paska narzędzi")

Aby zmienić kolejność elementów paska narzędzi domyślne, przeciągnij je po lewej stronie lub w prawo.

Następnie użyj **Inspektor atrybuty** można ustawić właściwości domyślne dla elementu:

![Dostosowywanie element paska narzędzi, przy użyciu Inspektora atrybuty](toolbar-images/add03.png "Dostosowywanie element paska narzędzi, przy użyciu Inspektora atrybutów")

Dostępne są następujące właściwości:

- **Nazwa obrazu** -obrazu do użycia jako ikona dla elementu
- **Etykieta** — tekst wyświetlany dla elementu w pasku narzędzi
- **Etykieta palety** — tekst wyświetlany dla elementu w **dozwolone elementy paska narzędzi** obszaru
- **Tag** -opcjonalne, unikatowy identyfikator, który ułatwia identyfikowanie elementów w kodzie.
- **Identyfikator** — definiuje typ elementu paska narzędzi. Niestandardowa wartość można zaznaczyć element paska narzędzi w kodzie.
- **Można wybierać** — po zaznaczeniu elementu będą zachowywać się jak przycisk Włącz/Wyłącz.

> [!IMPORTANT]
> Dodaj element do **dozwolone elementy paska narzędzi** obszaru, ale nie narzędzi domyślne opcje dostosowywania dla użytkowników. 

### <a name="adding-other-ui-controls-to-a-toolbar"></a>Dodawanie innych formantów interfejsu użytkownika do paska narzędzi

Kilka elementów interfejsu użytkownika Cocoa, takie jak pola wyszukiwania i kontrolki segmentowane mogą być również dodawane do paska narzędzi.

Aby z tego skorzystać, Otwórz pasek narzędzi w **hierarchii interfejsów** i wybierz element paska narzędzi, aby otworzyć okno dialogowe dostosowywania. Przeciągnij **pole wyszukiwania** z **Inspektor biblioteki** do **dozwolone elementy paska narzędzi** obszaru:

![Za pomocą okna dialogowego Dostosowywanie paska narzędzi](toolbar-images/add05.png "za pomocą okna dialogowego Dostosowywanie paska narzędzi")

W tym miejscu należy użyć programu Interface Builder skonfigurować pole wyszukiwania i udostępnić ją w kodzie za pomocą akcji lub gniazda.

## <a name="built-in-toolbar-item-support"></a>Obsługa elementu wbudowanego paska narzędzi

Kilka elementów interfejsu użytkownika Cocoa wchodzić w interakcje z elementami standardowy pasek narzędzi domyślnie. Na przykład przeciągać **widoku tekstu** na okno aplikacji i umieść go w celu wypełnienia obszaru zawartości:

[![Dodawanie widoku tekstu do aplikacji](toolbar-images/edit09.png "Dodawanie widoku tekstu do aplikacji")](toolbar-images/edit09-large.png#lightbox)

Zapisz dokument, wróć do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode, uruchom aplikację, wprowadź jakiś tekst, zaznacz go i kliknij **kolory** element paska narzędzi. Zwróć uwagę, że widok tekstu ma być automatycznie współpracuje z selektora kolorów:

![Funkcje wbudowanego paska narzędzi, za pomocą selektora widoku i kolor tekstu](toolbar-images/edit10.png "funkcji wbudowanego paska narzędzi, za pomocą selektora widoku i kolor tekstu")

## <a name="using-images-with-toolbar-items"></a>Przy użyciu obrazów z elementów paska narzędzi

Za pomocą **element paska narzędzi obrazów**, dowolny obraz mapy bitowej dodane do **zasobów** folder (a akcję kompilacji **zasobów pakietu**) mogą być wyświetlane na pasku narzędzi w postaci ikony:

1. W programie Visual Studio dla komputerów Mac w **konsoli rozwiązania**, kliknij prawym przyciskiem myszy **zasobów** i wybierz polecenie **Dodaj** > **Dodaj pliki** .
2. Z **Dodaj pliki** okna dialogowego pole, przejdź do obrazy do dodania, zaznacz je i kliknij przycisk **Otwórz** przycisku: 

    [![Wybierania obrazów do dodania](toolbar-images/edit11.png "wybierania obrazów do dodania")](toolbar-images/edit11-large.png#lightbox)

3. Wybierz **kopiowania**, sprawdź **za pomocą tej samej akcji dla wszystkich wybranych plików**i kliknij przycisk **OK**:

    ![Wybierając akcję kopiowania dodano obrazy](toolbar-images/edit12.png "wybierając akcję kopiowania dodano obrazów")

4. W **konsoli rozwiązania**, kliknij dwukrotnie **MainWindow.xib** aby go otworzyć w programie Xcode.

5. Zaznacz pasek narzędzi w **hierarchii interfejsów** i kliknij jeden z jego elementów, aby otworzyć okno dialogowe dostosowywania.

6. Przeciągnij **element paska narzędzi obrazów** z **Inspektor biblioteki** do paska narzędzi **dozwolone elementy paska narzędzi** obszaru: 

    ![Element paska narzędzi obrazów dodane do obszaru dozwolone elementy paska narzędzi](toolbar-images/edit14.png "element paska narzędzi obrazów dodane do obszaru dozwolone elementy paska narzędzi")

7. W **Inspektor atrybuty**, wybierz obraz, który właśnie został dodany w programie Visual Studio dla komputerów Mac: 

    ![Ustawienia niestandardowego obrazu dla elementu narzędzi](toolbar-images/edit15.png "ustawienia niestandardowego obrazu dla elementu paska narzędzi")

8. Ustaw **etykiety** do "Kosza" i **etykiety palety** "Wymazać dokument": 

    ![Ustawienie na pasku narzędzi elementu etykiety i etykiety palety](toolbar-images/edit16.png "ustawienie na pasku narzędzi elementu etykiety i etykiety palety")

9. Przeciągnij **element paska narzędzi Separator** z **Inspektor biblioteki** do paska narzędzi **dozwolone elementy paska narzędzi** obszaru: 

    [![Element paska narzędzi Separator dodane do obszaru dozwolone elementy paska narzędzi](toolbar-images/edit17.png "element paska narzędzi Separator dodane do obszaru dozwolone elementy paska narzędzi")](toolbar-images/edit17-large.png#lightbox)

10. Przeciągnij element separatora i element "Kosza" **domyślne elementy paska narzędzi** obszaru i kolejności paska narzędzi elementów z zestawu, od lewej do prawej, w następujący sposób (kolory, czcionki, Separator, Kosza na śmieci, elastyczne miejsca, drukowania): 

    ![Domyślne elementy paska narzędzi](toolbar-images/edit18.png "domyślne elementy paska narzędzi")

11. Zapisz zmiany i wróć do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode.

Uruchom aplikację, aby sprawdzić, czy domyślnie jest wyświetlany nowy pasek narzędzi:

![Pasek narzędzi z elementami dostosowane domyślne](toolbar-images/edit19.png "narzędzi z elementami dostosowane domyślne")

## <a name="exposing-toolbar-items-with-outlets-and-actions"></a>Udostępnianie elementów paska narzędzi z działaniami i gniazd

Aby uzyskać dostęp do paska narzędzi lub paska narzędzi elementu w kodzie, musi być dołączony do ujścia lub akcji:

1. W **konsoli rozwiązania**, kliknij dwukrotnie **Main.storyboard** aby go otworzyć w programie Xcode.
2. Upewnij się, że klasa niestandardowa "WindowController" przypisany do kontrolera w głównym oknie **Inspektor tożsamości**:

    [![Ustaw klasę niestandardową dla kontroler okna przy użyciu Inspektora tożsamości](toolbar-images/edit20a.png "przy użyciu Inspektora tożsamości można ustawić klasę niestandardową dla kontroler okna")](toolbar-images/edit20a-large.png#lightbox)

3. Następnie wybierz element paska narzędzi w **hierarchii interfejsów**: 

    ![Wybieranie elementów paska narzędzi w hierarchii interfejsów](toolbar-images/edit20.png "wybierając element paska narzędzi w hierarchii interfejsów")  

4. Otwórz **Asystenta ustawień widoku**, wybierz opcję **WindowController.h** plików i przeciągnij formant z elementu paska narzędzi do **WindowController.h** pliku.
5. Ustaw **połączenia** typ **akcji**, wprowadź "trashDocument" **nazwa**i kliknij przycisk **Connect** przycisku: 

    [![Konfigurowanie akcji dla elementu narzędzi](toolbar-images/edit23.png "Konfigurowanie akcji dla elementu paska narzędzi")](toolbar-images/edit23-large.png#lightbox)

6. Udostępnianie **widoku tekstu** jako sposobu o nazwie "documentEditor" w **ViewController.h** pliku: 

    [![Konfigurowanie sposobu wykorzystania widoku tekstu](toolbar-images/edit24.png "Konfigurowanie sposobu wykorzystania widoku tekstu")](toolbar-images/edit24-large.png#lightbox)

7. Zapisz zmiany i powrócić do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode.

W programie Visual Studio dla komputerów Mac, należy edytować **ViewController.cs** pliku i Dodaj następujący kod:

```csharp
public void EraseDocument() {
    documentEditor.Value = "";
}
```

Następnie Edytuj **WindowController.cs** pliku i Dodaj następujący kod na końcu `WindowController` klasy:

```csharp
[Export ("trashDocument:")]
void TrashDocument (NSObject sender) {

    var controller = ContentViewController as ViewController;
    controller.EraseDocument ();
}
```

Po uruchomieniu aplikacji, **Kosza** element paska narzędzi będzie aktywny:

![Pasek narzędzi za pomocą elementu active Kosza](toolbar-images/edit25.png "narzędzi za pomocą elementu active Kosza na śmieci")

Należy zauważyć, że **Kosza** element paska narzędzi może teraz służyć do usuwania tekstu.

## <a name="disabling-toolbar-items"></a>Wyłączanie elementów paska narzędzi

Aby wyłączyć elementu na pasku narzędzi, Utwórz niestandardowe `NSToolbarItem` klasy, a także Przesłoń `Validate` metody. Następnie w programu Interface Builder Przypisz niestandardowego typu elementu, który chcesz włączyć/wyłączyć.

Aby utworzyć niestandardową `NSToolbarItem` klasy, kliknij prawym przyciskiem myszy nad projektem i wybierz **Dodaj** > **nowy plik...** . Wybierz **ogólne** > **pustą klasę**, wprowadź "ActivatableItem" **nazwa**i kliknij przycisk **New** przycisku: 

![Dodawanie pustą klasę w programie Visual Studio dla komputerów Mac](toolbar-images/custom01.png "Dodawanie pustą klasę w programie Visual Studio dla komputerów Mac")

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

Kliknij dwukrotnie **Main.storyboard** aby go otworzyć w programie Xcode. Wybierz **Kosza** element paska narzędzi utworzonego powyżej i zmień jego klasy "ActivatableItem" w **Inspektor tożsamości**:

![Ustawienia niestandardowej klasy dla elementu paska narzędzi](toolbar-images/custom02.png "ustawienie niestandardowej klasy dla elementu paska narzędzi")

Tworzenie ujścia o nazwie `trashItem` dla **Kosza** element paska narzędzi. Zapisz zmiany i wróć do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode. Wreszcie, otwórz **MainWindow.cs** i zaktualizuj `AwakeFromNib` metodę w celu odczytania w następujący sposób:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Disable trash
    trashItem.Active = false;
}
```

Uruchom aplikację i zwróć uwagę, że **Kosza** element jest teraz wyłączony na pasku narzędzi:

![Pasek narzędzi za pomocą elementu nieaktywne Kosza](toolbar-images/custom03.png "narzędzi za pomocą elementu nieaktywne Kosza na śmieci")

## <a name="summary"></a>Podsumowanie

W tym artykule jest zajęta szczegółowe omówienie pracy z pasków narzędzi i elementów paska narzędzi w aplikacji platformy Xamarin.Mac. Jego opisano, jak tworzyć i obsługiwać paski narzędzi w program Xcode Interface Builder, niektóre kontrolki interfejsu użytkownika automatycznie współdziałaniu z elementów paska narzędzi, jak pracować z pasków narzędzi w kodzie języka C# i sposobu włączania i wyłączania elementów paska narzędzi.


## <a name="related-links"></a>Linki pokrewne

- [MacToolbar (przykład)](https://developer.xamarin.com/samples/mac/MacToolbar/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Human Interface wytyczne dotyczące pasków narzędzi](https://developer.apple.com/macos/human-interface-guidelines/windows-and-views/toolbars/)
- [Wprowadzenie do pasków narzędzi](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Toolbars/Toolbars.html)
