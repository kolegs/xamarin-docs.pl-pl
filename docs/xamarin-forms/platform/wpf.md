---
title: Instalator platformy WPF
description: Platformy Xamarin.Forms ma teraz obsługę podglądu na platformie WPF
ms.prod: xamarin
ms.assetid: 650723F2-4279-4B7B-B0A1-D7F8FF26BF1E
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 04/05/2018
ms.openlocfilehash: 2e2bbf12cd7b4abab4609349b549fde1bcea09e8
ms.sourcegitcommit: a69439ad4c9fd0abe759143687d3b23582573d90
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/28/2018
---
# <a name="wpf-platform-setup"></a>Instalator platformy WPF

![Wersja zapoznawcza](~/media/shared/preview.png)

Platformy Xamarin.Forms ma teraz Podgląd pomocy technicznej dla systemu Windows Presentation Foundation (WPF). W tym artykule przedstawiono sposób dodawania projekt WPF do rozwiązania platformy Xamarin.Forms.

Aby rozpocząć, Utwórz nowe rozwiązanie platformy Xamarin.Forms w Visual Studio 2017 lub użyć istniejącego rozwiązania platformy Xamarin.Forms, na przykład [ **BoxViewClock**](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock/). Można dodawać tylko aplikacje WPF do rozwiązania platformy Xamarin.Forms w systemie Windows.

## <a name="adding-a-wpf-app"></a>Dodawanie aplikacji WPF

Wykonaj te instrukcje, aby dodać aplikację WPF, który będzie uruchamiany na komputery stacjonarne z systemem Windows 7, 8 i 10:

1. W programie Visual Studio 2017 r, kliknij prawym przyciskiem myszy nazwę rozwiązania w **Eksploratora rozwiązań** i wybierz polecenie **Dodaj > Nowy projekt...** .

2. W **nowy projekt** okno, w oknie po lewej stronie wybierz **Visual C#** i **Windows Desktop klasycznego**. Lista typów projektów, wybranie **WPF aplikacji (.NET Framework)**. 

3. Wpisz nazwę projektu z **WPF** rozszerzenie, na przykład **BoxViewClock.WPF**. Kliknij przycisk **Przeglądaj** przycisku Wybierz **BoxViewClock** folderu, a następnie naciśnij klawisz **wybierz Folder**. Spowoduje to umieszczenie projekt WPF w tym samym katalogu co innych projektów w rozwiązaniu.

    ![Dodaj nowy projekt WPF](wpf-images/add-new-project.png "dodać nowy projekt WPF")

    Naciśnij przycisk OK, aby utworzyć projekt.

4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nowy **BoxViewClock.WPF** projekt i wybierz **Zarządzaj pakietami NuGet**. Wybierz **Przeglądaj** , kliknij pozycję **Uwzględnij wersję wstępną** wyboru i wyszukaj **platformy Xamarin.Forms**.

    ![Wybierz pakiet NuGet](wpf-images/select-nuget-package.png "wybierz pakiet NuGet")

    Wybierz pakiet, a następnie kliknij przycisk **zainstalować** przycisku.

5. Wyszukaj teraz **Xamarin.Forms.Platform.WPF** pakietu i że również zainstalować. Upewnij się, że pakiet pochodzi z firmy Microsoft!

6. Kliknij prawym przyciskiem myszy nazwę rozwiązania w **Eksploratora rozwiązań** i wybierz **Zarządzaj pakietami NuGet dla rozwiązania**. Wybierz **aktualizacji** kartę i **platformy Xamarin.Forms** pakietu. Wybierz wszystkie projekty i zaktualizuj je do tej samej wersji platformy Xamarin.Forms:

    ![Zaktualizuj pakiet NuGet](wpf-images/update-nuget-package.png "zaktualizuj pakiet NuGet") 

7. W projekcie WPF, kliknij prawym przyciskiem myszy **odwołania**. W **Menedżera odwołań** okno dialogowe, wybierz opcję **projekty** po lewej stronie, a także sprawdź pole wyboru obok **BoxViewClock** projektu:

    ![Odwołania do projektu udostępnionego](wpf-images/reference-shared-project.png "odwołania do projektu udostępnionego")

8. Edytuj **MainWindow.xaml** pliku projektu WPF. W `Window` tagów, dodawanie deklaracji przestrzeni nazw XML dla **Xamarin.Forms.Platform.WPF** zestawu i przestrzeni nazw:

    ```xaml
    xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
    ```

    Teraz zmienić `Window` znacznika `wpf:FormsApplicationPage`. Zmień `Title` ustawienie na nazwę aplikacji, na przykład **BoxViewClock**. Ukończono pliku XAML powinna wyglądać następująco:

    ```xaml
    <wpf:FormsApplicationPage x:Class="BoxViewClock.WPF.MainWindow"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
            xmlns:local="clr-namespace:BoxViewClock.WPF"
            mc:Ignorable="d"
            Title="BoxViewClock" Height="450" Width="800">
        <Grid>
        
        </Grid>
    </wpf:FormsApplicationPage>
    ```

9. Edytuj **MainWindow.xaml.cs** pliku projektu WPF. Dodaj dwa nowe `using` dyrektywy:

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;
    ```

    Zmień klasę podstawową `MainWindow` z `Window` do `FormsApplicationPage`. Po `InitializeComponent` wywołać, Dodaj następujące instrukcje dwóch:

    ```csharp
    Forms.Init();
    LoadApplication(new BoxViewClock.App());
    ```
    
    Z wyjątkiem komentarzy i nieużywanych `using` dyrektywy, pełną **MainWindows.xaml.cs** pliku powinna wyglądać następująco:

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;

    namespace BoxViewClock.WPF
    {
        public partial class MainWindow : FormsApplicationPage
        {
            public MainWindow()
            {
                InitializeComponent();

                Forms.Init();
                LoadApplication(new BoxViewClock.App());
            }
        }
    }
    ```

10. Kliknij prawym przyciskiem myszy projekt WPF w **Eksploratora rozwiązań** i wybierz **Ustaw jako projekt startowy**. Naciśnij klawisz F5, aby uruchomić program z debuger programu Visual Studio na pulpicie systemu Windows:

    ![Zegar BoxView WPF](wpf-images/wpf-boxviewclock.png "zegara BoxView WPF" )

## <a name="next-steps"></a>Następne kroki

### <a name="platform-specifics"></a>Szczegóły platformy

Można określić platformy aplikacji platformy Xamarin.Forms jest zasilany z kodu lub XAML. Dzięki temu można zmienić właściwości programu, gdy jest uruchomiona na WPF. W kodzie, porównanie wartości `Device.RuntimePlatform` z `Device.WPF` stała (która jest równa ciąg "WPF"). Jeśli istnieje dopasowanie, aplikacja jest uruchomiona na WPF.

W języku XAML, można użyć `OnPlatform` tag, aby wybrać wartość właściwości specyficzne dla platformy:

```xaml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White" />
        <On Platform="macOS" Value="White" />
        <On Platform="Android" Value="Black" />
        <On Platform="WPF" Value="Blue" />
    </OnPlatform>
</Button.TextColor>
```

### <a name="window-size"></a>Rozmiar okna

Można dostosować początkowy rozmiar okna na platformie WPF **MainWindow.xaml** pliku:

```xaml
Title="BoxViewClock" Height="450" Width="800"
```

## <a name="issues"></a>Problemy

To jest podgląd, dlatego należy oczekiwać, że nie wszystko jest gotowe produkcji. Nie wszystkie pakiety NuGet dla platformy Xamarin.Forms są gotowe do WPF i niektóre funkcje mogą nie pełni działać.

