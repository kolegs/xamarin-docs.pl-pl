---
title: Instalator platformy WPF
description: Zestaw narzędzi Xamarin.Forms ma teraz obsługę platformy WPF w wersji zapoznawczej
ms.prod: xamarin
ms.assetid: 650723F2-4279-4B7B-B0A1-D7F8FF26BF1E
ms.technology: xamarin-forms
ms.custom: xamu-video
author: charlespetzold
ms.author: chape
ms.date: 04/05/2018
ms.openlocfilehash: 416e33f131c6e1ef144608f98964fd8372f454f8
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831322"
---
# <a name="wpf-platform-setup"></a>Instalator platformy WPF

![Wersja zapoznawcza](~/media/shared/preview.png)

Zestaw narzędzi Xamarin.Forms dodano wsparcie dla Windows Presentation Foundation (WPF). W tym artykule przedstawiono sposób dodawania projekt WPF do rozwiązania platformy Xamarin.Forms.

Przed rozpoczęciem, Utwórz nowe rozwiązanie Xamarin.Forms w programie Visual Studio 2017 lub użyć istniejącego rozwiązania Xamarin.Forms, na przykład [ **BoxViewClock**](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock/). Aplikacje WPF można dodawać tylko do rozwiązania Xamarin.Forms w Windows.

## <a name="add-a-wpf-project-to-a-xamarinforms-app-with-xamarinuniversity"></a>Dodaj projekt WPF do aplikacji platformy Xamarin.Forms przy użyciu Xamarin.University

> [!VIDEO https://youtube.com/embed/Fy9N6OSxK64]

**Produkt Xamarin.Forms 3.0 WPF obsługuje, przez [platformy Xamarin University](https://university.xamarin.com/)**

## <a name="adding-a-wpf-app"></a>Dodawanie aplikacji WPF

Wykonaj te instrukcje, aby dodać aplikację WPF, który będzie uruchamiany na komputerach stacjonarnych Windows 7, 8 i 10:

1. W programie Visual Studio 2017, kliknij prawym przyciskiem myszy nazwę rozwiązania w **Eksploratora rozwiązań** i wybierz polecenie **Dodaj > Nowy projekt...** .

2. W **nowy projekt** okna, w polu po lewej stronie wybierz **Visual C#** i **Windows Classic Desktop**. Na liście typów projektów, wybierz opcję **aplikacja WPF (.NET Framework)**. 

3. Wpisz nazwę dla projektu z **WPF** rozszerzenia, na przykład **BoxViewClock.WPF**. Kliknij przycisk **Przeglądaj** przycisku Wybierz **BoxViewClock** folder, a następnie naciśnij klawisz **wybierz Folder**. Spowoduje to umieszczenie projekt WPF, w tym samym katalogu co inne projekty w rozwiązaniu.

    ![Dodaj nowy projekt WPF](wpf-images/add-new-project.png "Dodaj nowy projekt WPF")

    Naciśnij przycisk OK, aby utworzyć projekt.

4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nowy **BoxViewClock.WPF** projektu, a następnie wybierz **Zarządzaj pakietami NuGet**. Wybierz **Przeglądaj** kliknij pozycję **Uwzględnij wersję wstępną** zaznacz pole wyboru i wyszukaj **Xamarin.Forms**.

    ![Wybierz pakiet NuGet](wpf-images/select-nuget-package.png "wybierz pakiet NuGet")

    Wybierz pakiet, a następnie kliknij przycisk **zainstalować** przycisku.

5. Teraz wyszukać **Xamarin.Forms.Platform.WPF** pakietów i zainstalowania jednego także. Upewnij się, że pakiet pochodzi od firmy Microsoft.

6. Kliknij prawym przyciskiem myszy nazwę rozwiązania w **Eksploratora rozwiązań** i wybierz **Zarządzaj pakietami NuGet dla rozwiązania**. Wybierz **aktualizacji** kartę i **Xamarin.Forms** pakietu. Wybierz wszystkie projekty i zaktualizować je do tej samej wersji zestawu narzędzi Xamarin.Forms:

    ![Aktualizuj pakiet NuGet](wpf-images/update-nuget-package.png "aktualizacji pakietu NuGet") 

7. W projekcie WPF, kliknij prawym przyciskiem myszy **odwołania**. W **Menadżer odwołań** okno dialogowe, wybierz opcję **projektów** po lewej stronie i sprawdź pole wyboru obok **BoxViewClock** projektu:

    ![Odwoływać się do projektu udostępnionego](wpf-images/reference-shared-project.png "odwoływać się do projektu udostępnionego")

8. Edytuj **MainWindow.xaml** pliku projektu WPF. W `Window` tag, dodawanie deklaracji przestrzeni nazw XML **Xamarin.Forms.Platform.WPF** zestawu i przestrzeni nazw:

    ```xaml
    xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
    ```

    Teraz Zmień `Window` tag `wpf:FormsApplicationPage`. Zmiana `Title` ustawienie na nazwę aplikacji, na przykład **BoxViewClock**. Ukończone pliku XAML powinien wyglądać następująco:

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

9. Edytuj **MainWindow.xaml.cs** pliku projektu WPF. Dodaj dwie nowe `using` dyrektywy:

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;
    ```

    Zmień klasę bazową `MainWindow` z `Window` do `FormsApplicationPage`. Następujące `InitializeComponent` wywołania, Dodaj dwie poniższe instrukcje:

    ```csharp
    Forms.Init();
    LoadApplication(new BoxViewClock.App());
    ```
    
    Z wyjątkiem komentarze i nieużywanych `using` dyrektyw, pełne **MainWindows.xaml.cs** plik powinien wyglądać następująco:

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

10. Kliknij prawym przyciskiem myszy projekt WPF w **Eksploratora rozwiązań** i wybierz **Ustaw jako projekt startowy**. Naciśnij klawisz F5, aby uruchomić program za pomocą debugera programu Visual Studio na pulpicie Windows:

    ![Zegara BoxView WPF](wpf-images/wpf-boxviewclock.png "zegara BoxView WPF" )

## <a name="next-steps"></a>Następne kroki

### <a name="platform-specifics"></a>Funkcje specyficzne dla platformy

Można określić platformy aplikacji Xamarin.Forms jest uruchamiana na z kodu lub XAML. Dzięki temu można zmienić właściwości programu uruchamianego na WPF. W kodzie, porównanie wartości `Device.RuntimePlatform` z `Device.WPF` — stała (która jest równa ciąg "WPF"). Jeśli istnieje dopasowanie, aplikacja jest uruchomiona na WPF.

W XAML, można użyć `OnPlatform` tag, aby wybrać wartość właściwości specyficzne dla platformy:

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

Można dostosować początkowy rozmiar okna w WPF **MainWindow.xaml** pliku:

```xaml
Title="BoxViewClock" Height="450" Width="800"
```

## <a name="issues"></a>Problemy

To jest podgląd, więc należy się spodziewać, że nie wszystko jest już gotowe do produkcji. Nie wszystkie pakiety NuGet dla platformy Xamarin.Forms są gotowe do WPF, a niektóre funkcje mogą nie pełni działać.

