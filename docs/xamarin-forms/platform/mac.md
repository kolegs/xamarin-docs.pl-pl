---
title: Instalator platformy Mac
description: "Platformy Xamarin.Forms ma teraz obsługę podglądu platformy Mac"
ms.topic: article
ms.prod: xamarin
ms.assetid: EEC549E0-F182-4F9C-B2BA-B31D19569AA5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/03/2017
ms.openlocfilehash: bda207796d1019f8188176acce055d782cb9e32d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="mac-platform-setup"></a>Instalator platformy Mac

![Wersja zapoznawcza](~/media/shared/preview.png)

Przed rozpoczęciem należy utworzyć (lub użyj istniejącego) projektu platformy Xamarin.Forms.
Można dodawać tylko aplikacje Mac za pomocą programu Visual Studio dla komputerów Mac.

## <a name="adding-a-mac-app"></a>Dodawanie aplikacji Mac

Wykonaj te instrukcje, aby dodać aplikację Mac, który będzie uruchamiany na macOS Sierra i Mac OS X El Capitan:

1. W programie Visual Studio for Mac, kliknij prawym przyciskiem myszy na istniejące rozwiązanie platformy Xamarin.Forms i wybierz polecenie **Dodaj > Dodawanie nowego projektu...**

2. W **nowy projekt** wybierz okno **Mac > aplikacji > aplikacji Cocoa** i naciśnij klawisz **dalej**.

3. Typ **Nazwa aplikacji** (i opcjonalnie wybierz inną nazwę dla elementu dokowania), naciśnij klawisz **dalej**.

4. Przejrzyj konfigurację i naciśnij klawisz **Utwórz**. Poniżej przedstawiono następujące kroki w:

  ![Animowany instrukcje przedstawiający sposób dodawania aplikacji Cocoa](mac-images/add-macos-proj.gif)

5. W projekcie Mac, kliknij prawym przyciskiem myszy **pakietów > Dodawanie pakietów...**  można dodać [Xamarin.Forms/2.3.5.235-pre2](https://www.nuget.org/packages/Xamarin.Forms/2.3.5.235-pre2) NuGet. Należy również zaktualizować innych projektów do tej wersji.

6. W projekcie Mac, kliknij prawym przyciskiem myszy **odwołania** i Dodaj odwołanie do projektu platformy Xamarin.Forms (projektu udostępnionego lub PCL).

  ![Dodaj odwołanie do projektu udostępnionego kodu platformy Xamarin.Forms](mac-images/references-sml.png)

7. Aktualizacja **Main.cs** zainicjować `AppDelegate`:

    ```csharp
    static class MainClass
    {
        static void Main(string[] args)
        {
            NSApplication.Init();
            NSApplication.SharedApplication.Delegate = new AppDelegate(); // add this line
            NSApplication.Main(args);
        }
    }
    ```

8. Aktualizacja `AppDelegate` zainicjować platformy Xamarin.Forms utworzyć okna i obciążenia aplikacji platformy Xamarin.Forms (Pamiętaj o tym ustawić odpowiedni `Title`). _Jeśli masz inne zależności, które muszą zostać zainicjowana zrobić na tej stronie również._

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.MacOS;
    // also add a using for the Xamarin.Forms project, if the namespace is different to this file
    ...
    [Register("AppDelegate")]
    public class AppDelegate : FormsApplicationDelegate
    {
        NSWindow window;
        public AppDelegate()
        {
            var style = NSWindowStyle.Closable | NSWindowStyle.Resizable | NSWindowStyle.Titled;

            var rect = new CoreGraphics.CGRect(200, 1000, 1024, 768);
            window = new NSWindow(rect, style, NSBackingStore.Buffered, false);
            window.Title = "Xamarin.Forms on Mac!"; // choose your own Title here
            window.TitleVisibility = NSWindowTitleVisibility.Hidden;
        }

        public override NSWindow MainWindow
        {
            get { return window; }
        }

        public override void DidFinishLaunching(NSNotification notification)
        {
            Forms.Init();
            LoadApplication(new App());
            base.DidFinishLaunching(notification);
        }
    }
    ```

9. Kliknij dwukrotnie **Main.storyboard** do edycji w środowisku Xcode. Wybierz **okna** i _Usuń zaznaczenie pola wyboru_ **jest początkowej kontrolera** wyboru (jest to ponieważ powyższy kod tworzy okno):

  [ ![Usunąć zaznaczenie pola wyboru jest początkowej kontrolera w środowisku Xcode](mac-images/xcode-init-controller-sml.png)](mac-images/xcode-init-controller.png)

  Można edytować systemu scenorysu, aby usunąć niechciane elementy do menu.

10. Na koniec należy dodać wszystkie zasoby lokalne (np.) pliki obrazów) istniejących projektach platformy, które są wymagane.

11. Projekt Mac powinna teraz uruchomić kod platformy Xamarin.Forms na macOS!

## <a name="next-steps"></a>Następne kroki

### <a name="styling"></a>Ustawianie stylów

Z ostatnimi zmianami wprowadzonymi w `OnPlatform` można teraz wskazać dowolną liczbę platform. Zawierającej macOS.

```xml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White"/>
        <On Platform="macOS" Value="White"/>
        <On Platform="Android" Value="Black"/>
    </OnPlatform>
</Button.TextColor>
```

Należy pamiętać, może również dwukrotnie na platformach, np. to: `<On Platform="iOS, macOS" ...>`.

### <a name="window-size-and-position"></a>Rozmiar i położenie okna

Można dostosować początkowy rozmiar i położenie okna w `AppDelegate`:

```csharp
var rect = new CoreGraphics.CGRect(200, 1000, 1024, 768);  // x, y, width, height
```

## <a name="known-issues"></a>Znane problemy

To jest podgląd, dlatego należy oczekiwać, że nie wszystko jest gotowe produkcji. Poniżej przedstawiono kilka rzeczy, jakie można napotkać podczas dodawania macOS do projektów:

### <a name="not-all-nugets-are-ready-for-macos"></a>Nie wszystkie NuGets są gotowe do macOS

Pakiety muszą wskazywać "xamarinmac20" działa w projekcie macOS. Może się okazać, że niektóre z bibliotek, którego używasz nie jest jeszcze obsługiwany system macOS.

W takim przypadku należy wysłać żądania do elementu utrzymującego projektu, aby dodać go. Nie określono pomocy technicznej, konieczne może być Wyszukaj alternatyw.

### <a name="missing-xamarinforms-features"></a>Brak funkcji platformy Xamarin.Forms

Nie wszystkie funkcje platformy Xamarin.Forms są kompletne w tej wersji zapoznawczej; Poniżej przedstawiono listę niektórych funkcji, która nie została jeszcze zaimplementowana:

* Stopki
* Obraz — proporcji
* ListView — ScrollTo, UnevenRows obsługuje, odświeżania, SeparatorColor, SeparatorVisibility
* MasterDetailPage — BackgroundColor
* Nawigacja — InsertPageBefore
* OpenGLRenderer
* Selektor — Bindable/według implementacji
* TabbedPage – BarBackgroundColor, BarTextColor
* TableView — UnevenRows
* ForceUpdateSize ViewCell — IsEnabled,
* Widok sieci Web — większość WebNavigationEvents


## <a name="related-links"></a>Linki pokrewne

- [Xamarin.Mac](~/mac/index.yml)
