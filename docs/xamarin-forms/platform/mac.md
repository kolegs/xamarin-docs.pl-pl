---
title: Instalator platformy Mac
description: W tym artykule wyjaśniono, jak dodać projekt Mac do projektu Xamarin.Forms, która powoduje wygenerowanie aplikacji może uruchamiać w systemach macOS Sierra i macOS El Capitan.
ms.prod: xamarin
ms.assetid: EEC549E0-F182-4F9C-B2BA-B31D19569AA5
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/03/2017
ms.openlocfilehash: ae0fbfc7862a0d2147b2c3bbdbae7dd53dfce78f
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831692"
---
# <a name="mac-platform-setup"></a>Instalator platformy Mac

![Wersja zapoznawcza](~/media/shared/preview.png)

Przed rozpoczęciem należy utworzyć (lub użyć istniejącego) projektu Xamarin.Forms.
Możesz dodawać tylko aplikacji dla komputerów Mac przy użyciu programu Visual Studio dla komputerów Mac.

> [!VIDEO https://youtube.com/embed/mvQ7jzaNseM]

**Po dodaniu projektu z systemem macOS do zestawu narzędzi Xamarin.Forms [Xamarin University](https://university.xamarin.com/)**

## <a name="adding-a-mac-app"></a>Dodawanie aplikacja dla komputerów Mac

Wykonaj te instrukcje, aby dodać aplikację Mac, który będzie uruchamiany w systemach macOS Sierra i macOS El Capitan:

1. W programie Visual Studio dla komputerów Mac, kliknij prawym przyciskiem myszy na istniejące rozwiązanie Xamarin.Forms, a następnie wybierz **Dodaj > Dodaj nowy projekt...**

2. W **nowy projekt** wybierz okno **Mac > aplikacji > Aplikacja cocoa dla** i naciśnij klawisz **dalej**.

3. Typ **nazwy aplikacji** (i opcjonalnie wybierz inną nazwę dla elementu Dock), naciśnij klawisz **dalej**.

4. Przejrzyj konfigurację i naciśnij klawisz **Utwórz**. Te kroki przedstawiono poniżej:

  ![Instrukcje animowany przedstawiająca sposób dodać aplikację Cocoa](mac-images/add-macos-proj.gif)

5. W projekcie Mac, kliknij prawym przyciskiem myszy **pakiety > Dodawanie pakietów...**  dodać [Xamarin.Forms/2.3.5.235-pre2](https://www.nuget.org/packages/Xamarin.Forms/2.3.5.235-pre2) NuGet. Należy również zaktualizować inne projekty do tej wersji.

6. W projekcie Mac, kliknij prawym przyciskiem myszy **odwołania** i Dodaj odwołanie do projektu Xamarin.Forms (projekt biblioteki projektu udostępnionego lub .NET Standard).

  ![Dodaj odwołanie do projektu udostępnionego kodu zestawu narzędzi Xamarin.Forms](mac-images/references-sml.png)

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

8. Aktualizacja `AppDelegate` zainicjować Xamarin.Forms, utworzyć okno i ładowanie aplikacji platformy Xamarin.Forms (pamiętaniu o ustawić odpowiednie `Title`). _Jeśli masz inne zależności, które muszą zostać zainicjowana, to zrobić tutaj również._

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

9. Kliknij dwukrotnie **Main.storyboard** do edycji w środowisku Xcode. Wybierz **okna** i _Usuń zaznaczenie pola wyboru_ **jest początkowa kontrolera** wyboru (jest to ponieważ powyższy kod tworzy okno):

  [![Usunąć zaznaczenie pola wyboru jest początkowa kontrolera w środowisku Xcode](mac-images/xcode-init-controller-sml.png)](mac-images/xcode-init-controller.png#lightbox)

  Możesz edytować system menu w scenorysu do usunięcia niepotrzebnych elementów.

10. Na koniec należy dodać wszystkie zasoby lokalne (np.) pliki obrazów) z istniejących projektów platformy, które są wymagane.

11. Projekt Mac teraz uruchamiać kodu zestawu narzędzi Xamarin.Forms dla systemu macOS!

## <a name="next-steps"></a>Następne kroki

### <a name="styling"></a>Ustawianie stylów

Z ostatnich zmian wprowadzonych do `OnPlatform` można teraz wskazać dowolną liczbę platform. Obejmuje to systemu macOS.

```xml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White"/>
        <On Platform="macOS" Value="White"/>
        <On Platform="Android" Value="Black"/>
    </OnPlatform>
</Button.TextColor>
```

Należy pamiętać, może również dwukrotnie na platformach, w następujący sposób: `<On Platform="iOS, macOS" ...>`.

### <a name="window-size-and-position"></a>Rozmiar i położenie okna

Można dostosować początkowy rozmiar i położenie okna w `AppDelegate`:

```csharp
var rect = new CoreGraphics.CGRect(200, 1000, 1024, 768);  // x, y, width, height
```

## <a name="known-issues"></a>Znane problemy

To jest podgląd, więc należy się spodziewać, że nie wszystko jest już gotowe do produkcji. Poniżej przedstawiono kilka rzeczy, które można napotkać podczas dodawania do swoich projektów systemu macOS:

### <a name="not-all-nugets-are-ready-for-macos"></a>Nie wszystkie rozszerzeń Nuget jest gotowe dla systemu macOS

Pakiety "xamarinmac20" musi być przeznaczony do pracy w projekcie systemu macOS. Może się okazać, że niektóre z bibliotek, które służy nie jest jeszcze obsługiwany z systemem macOS.

W takim przypadku należy wysłać żądanie do projektu Element utrzymujący, aby dodać go. Nie określono pomocy technicznej, może być konieczne Wyszukaj alternatywy.

### <a name="missing-xamarinforms-features"></a>Brakujące funkcje zestawu narzędzi Xamarin.Forms

Nie wszystkie funkcje zestawu narzędzi Xamarin.Forms są kompletne w tej wersji zapoznawczej; Poniżej przedstawiono listę niektórych funkcji, która nie została jeszcze zaimplementowana:

* Stopka
* Obraz — proporcji
* ListView — ScrollTo, UnevenRows pomocy technicznej, odświeżanie SeparatorColor, SeparatorVisibility
* MasterDetailPage — BackgroundColor
* Nawigacja — InsertPageBefore
* OpenGLRenderer
* Selektor — implementacja Bindable/możliwość obserwowania
* TabbedPage – BarBackgroundColor, BarTextColor
* TableView — UnevenRows
* ForceUpdateSize ViewCell — IsEnabled,
* Aplikacja WebView — większość WebNavigationEvents


## <a name="related-links"></a>Linki pokrewne

- [Xamarin.Mac](~/mac/index.yml)
