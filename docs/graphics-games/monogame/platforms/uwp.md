---
title: Tworzenie projektu platformy UWP MonoGame
description: MonoGame może służyć do tworzenia gier i aplikacji dla platformy uniwersalnej systemu Windows, elementów docelowych, które codebase wiele urządzeń z jednym i jednego zestawu zawartości.
ms.prod: xamarin
ms.assetid: C6B99E44-00C1-4139-A1B7-FCFBE8749AB1
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: ee4ee83c07cf01d1324b5f127d4f77ced0df2afe
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/18/2018
---
# <a name="creating-a-monogame-uwp-project"></a>Tworzenie projektu platformy UWP MonoGame

_MonoGame może służyć do tworzenia gier i aplikacji dla platformy uniwersalnej systemu Windows, elementów docelowych, które codebase wiele urządzeń z jednym i jednego zestawu zawartości._

Ten przewodnik obejmuje tworzenie projektu MonoGame Windows platformy Uniwersalnej i ładowania zawartości. Aplikacji platformy uniwersalnej systemu Windows można uruchamiać na wszystkich urządzeniach systemu Windows 10, w tym komputerów stacjonarnych, tablety, telefony z systemem Windows i konsoli Xbox One.

W tym przewodniku tworzy pusty projekt, który wyświetla *cornflower blue* tła (kolor tła tradycyjnych XNA aplikacji).

## <a name="requirements"></a>Wymagania

Tworzenie aplikacji platformy uniwersalnej systemu Windows MonoGame wymaga:

- System operacyjny Windows 10
- Dowolna wersja programu Visual Studio 2015
- Narzędzia dla deweloperów systemu Windows 10
- Ustawienia urządzenia tryb dewelopera
- [3.5 MonoGame dla programu Visual Studio](http://www.monogame.net/2016/03/17/monogame-3-5/) lub nowsza

Aby uzyskać więcej informacji, zobacz [strony na temat konfigurowania do tworzenia aplikacji platformy uniwersalnej systemu Windows do systemu Windows 10](https://msdn.microsoft.com/en-us/windows/uwp/get-started/get-set-up).

Gry konsoli Xbox One mogą być opracowane w sieci sprzedaży konsoli Xbox One sprzętu. Na komputerze tworzenie i konsoli Xbox One jest wymagane dodatkowe oprogramowanie. Aby uzyskać informacje na temat konfigurowania konsoli Xbox One do tworzenia gier, zobacz tę stronę na [konfigurowania konsoli Xbox One](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/index).

## <a name="creating-an-empty-template"></a>Tworzenie pustego szablonu

Po zainstalowaniu wszystkich wymaganych zasobów i został włączony tryb dewelopera na komputerze z systemem Windows 10, można utworzyć nowy projekt MonoGame przy użyciu programu Visual Studio, wykonując następujące czynności:

1. Wybierz **pliku** > **nowe** > **projektu...**
1. Wybierz **zainstalowana** > **szablony** > **Visual C#** > **MonoGame** kategorii: 

    ![](uwp-images/image1.png "Kategoria MonoGame")

1. Wybierz **MonoGame systemu Windows 10 uniwersalnych projektów** opcji: 

    ![](uwp-images/image2.png "Wybierz opcję MonoGame systemu Windows 10 uniwersalnych projektów")

1. Wprowadź nazwę dla nowego projektu, a następnie kliknij przycisk **OK**.
Visual Studio są wyświetlane wszystkie błędy po kliknięciu przycisku OK, sprawdź, czy zostały zainstalowane narzędzia systemu Windows 10 i że urządzenie jest w trybie dewelopera.

Po zakończeniu tworzenia szablonu Visual Studio firma Microsoft można uruchomić, aby wyświetlić pusty projekt uruchomiona:

![](uwp-images/image3.png "Po zakończeniu tworzenia szablonu Visual Studio uruchom go, aby zobaczyć pusty projekt systemem")

Liczby w narożnikach dostarczanie informacji diagnostycznych. Te informacje mogą być usunięte przez usuwanie kodu w `App.xaml.cs` w `DEBUG` blok w `OnLaunched` metody:


```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
#if DEBUG
    if (System.Diagnostics.Debugger.IsAttached)
    {
        this.DebugSettings.EnableFrameRateCounter = true;
    }
#endif
    ...
```

## <a name="running-on-xbox-one"></a>Uruchomione na konsoli Xbox One

Projekty platformy UWP można wdrażać na dowolnym urządzeniu z systemem Windows 10 w tym samym projekcie. Po skonfigurowaniu na komputerze deweloperskim systemu Windows 10 i konsoli Xbox One, można wdrożyć aplikacji platformy uniwersalnej systemu Windows przełączania docelowej z komputerem zdalnym i wprowadzając konsoli Xbox One na adres IP:

![](uwp-images/remote.png "Przełączanie docelowej z komputerem zdalnym i wprowadzając adres IP z nich Xbox można wdrożyć aplikacji platformy uniwersalnej systemu Windows")

Na konsoli Xbox One białe obramowania reprezentuje obszar bezpieczne wątkowo telewizorach. Aby uzyskać więcej informacji, zobacz [sekcji bezpieczny obszar](#Safe_Area_on_Xbox_One).

![](uwp-images/safearea.png "Na konsoli Xbox One białe obramowania reprezentuje obszar bezpieczne wątkowo telewizorach")

### <a name="safe-area-on-xbox-one"></a>Bezpieczne obszar na konsoli Xbox One

Tworzenie gier dla konsoli wymaga uwzględnieniu obszar bezpieczne, który jest to obszar w Centrum ekranu, który powinien zawierać wszystkie krytyczne elementy wizualne (np. interfejsu użytkownika lub HUD). Obszar poza obszarem bezpieczne nie jest gwarantowana mają być wyświetlane na wszystkich telewizorach, więc elementy wizualne umieszczone w tym obszarze może być częściowo lub całkowicie niewidoczne w niektórych Wyświetla.

Szablon MonoGame konsoli Xbox One uwzględnia bezpiecznego miejsca i renderuje go jako białe obramowanie. Ten obszar umieszczono także w czasie wykonywania w grze `Window.ClientBounds` właściwości, jak pokazano w tym obrazie okna czujki w programie Visual Studio. Zauważ, że wysokość granic klienta jest 1016 pomimo rozdzielczość 1920 x 1080 pikseli wyświetlania:

![](uwp-images/clientbounds.png "Zauważ, że wysokość granic klienta jest 1016 pomimo rozdzielczość 1920 x 1080 pikseli ekranu")

## <a name="referencing-content-in-uwp-projects"></a>Odwołanie do zawartości w projekty platformy UWP

Zawartość w projektach MonoGame można odwoływać się bezpośrednio z pliku lub za pomocą [potoku zawartości MonoGame](~/graphics-games/cocossharp/content-pipeline/index.md). Małe gier projekty mogą korzystać z prostotę ładowania pliku. Większych projektów będą korzystać z zawartości potok w celu zoptymalizowania zawartości, aby zmniejszyć rozmiar i załadować razy. W odróżnieniu od XNA na konsoli Xbox 360 `System.IO.File` klasy jest dostępny w aplikacji platformy UWP jednej konsoli Xbox.

Aby uzyskać więcej informacji na ładowanie zawartości za pomocą potoku zawartości, zobacz [przewodnik potoku zawartości](~/graphics-games/cocossharp/content-pipeline/index.md). 

### <a name="loading-content-from-file"></a>Ładowanie zawartości z pliku

W przeciwieństwie do systemów iOS i Android projekty platformy UWP może odwoływać się do plików względem pliku wykonywalnego. Proste gry umożliwia tej techniki obciążenia zawartości bez konieczności modyfikowania i tworzenia projektu potoku zawartości.

Aby załadować `Texture2D` z pliku:

1. Dodaj plik PNG do folderu zawartości w projekcie platformy uniwersalnej systemu Windows. Dodawanie zawartości do folderu zawartości jest Konwencji XNA i MonoGame.
1. Kliknij prawym przyciskiem myszy na nowo dodanych PNG i wybierz polecenie Właściwości.
1. Zmień **Kopiuj do katalogu wyjściowego** do **Kopiuj, jeśli nowszy**.
1. Dodaj następujący kod do metody Initialize Twoja gra załadować `Texture2D`:

    ```csharp
    Texture2D texture;
    using (var stream = System.IO.File.OpenRead("Content/YourPngName.png"))
    {
        texture = Texture2D.FromStream(graphics.GraphicsDevice, stream);
    }
    ```

Aby uzyskać więcej informacji na temat używania `Texture2D`, zobacz [wprowadzenie do przewodnika MonoGame](~/graphics-games/monogame/introduction/index.md).

## <a name="summary"></a>Podsumowanie

W tym przewodniku opisano sposób tworzenia nowego projektu platformy uniwersalnej systemu Windows i zagadnienia dotyczące specyficzne dla platformy uniwersalnej systemu Windows podczas ładowania plików. Deweloperzy, którzy planuje się tworzenie pełnej gry platformy uniwersalnej systemu Windows można Dowiedz się więcej o MonoGame w [wprowadzenie do przewodnika MonoGame](~/graphics-games/monogame/introduction/index.md).