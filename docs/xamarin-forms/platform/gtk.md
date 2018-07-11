---
title: 'Instalator platformy GTK #'
description: 'Zestaw narzędzi Xamarin.Forms ma teraz obsługę wersji zapoznawczej platformy GTK #'
ms.prod: xamarin
ms.assetid: 3417FB95-3E4B-47DA-85D0-F34832747236
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
ms.openlocfilehash: 7f68b7c8affc11b50bdb4a2fc9589f8dcbfb45ec
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38830483"
---
# <a name="gtk-platform-setup"></a>Instalator platformy GTK #

![Wersja zapoznawcza](~/media/shared/preview.png)

Zestaw narzędzi Xamarin.Forms ma teraz obsługę aplikacji biblioteki GTK # w wersji zapoznawczej. Biblioteki GTK # to zestaw narzędzi interfejsu graficznego, który stanowi łącze do zestawu narzędzi GTK + i różnych bibliotekach GNOME pozwalające na rozwój całkowicie natywne aplikacje grafiki GNONE przy użyciu platformy Mono i .NET. W tym artykule pokazano, jak dodać projekt GTK # do rozwiązania platformy Xamarin.Forms.

Przed rozpoczęciem, Utwórz nowe rozwiązanie Xamarin.Forms lub użyć istniejącego rozwiązania Xamarin.Forms, na przykład [ **GameOfLife**](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife/).

> [!NOTE]
> Chociaż ten artykuł koncentruje się na temat dodawania aplikacji biblioteki GTK # do rozwiązania platformy Xamarin.Forms w programie VS2017 i Visual Studio dla komputerów Mac, również może być wykonana w [MonoDevelop](http://www.monodevelop.com/) dla systemu Linux.

## <a name="adding-a-gtk-app"></a>Dodawanie aplikacji biblioteki GTK #

Biblioteki GTK # dla systemu macOS i Linux jest instalowany jako część [Mono](http://www.mono-project.com/download/stable/). Biblioteki GTK # dla platformy .NET można zainstalować na Windows za pomocą [GTK # Instalatora](http://www.mono-project.com/download/stable/#download-win).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Wykonaj te instrukcje, aby dodać aplikację GTK #, który będzie uruchamiany na pulpicie Windows:

1. W programie Visual Studio 2017, kliknij prawym przyciskiem myszy nazwę rozwiązania w **Eksploratora rozwiązań** i wybierz polecenie **Dodaj > Nowy projekt...** .

2. W **nowy projekt** okna, w polu po lewej stronie wybierz **Visual C#** i **Windows Classic Desktop**. Na liście typów projektów, wybierz opcję **biblioteki klas (.NET Framework)** i upewnij się, że **Framework** listy rozwijanej jest równa co najmniej programu .NET Framework 4.7.

3. Wpisz nazwę dla projektu z **GTK** rozszerzenia, na przykład **GameOfLife.GTK**. Kliknij przycisk **Przeglądaj** przycisku, wybierz folder zawierający inne platformy projektów, a następnie naciśnij klawisz **wybierz Folder**. Spowoduje to umieszczenie projekt GTK w tym samym katalogu co inne projekty w rozwiązaniu.

    ![Dodaj nowy projekt GTK](gtk-images/win/add-new-project.png "Dodaj nowy projekt GTK")

    Naciśnij klawisz **OK** przycisk, aby utworzyć projekt.

4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nowy projekt GTK i wybierz **Zarządzaj pakietami NuGet**. Wybierz **Przeglądaj** kartę i wyszukaj **Xamarin.Forms** wersji 3.0 lub nowszej.

    ![Wybierz pakiet Xamarin.Forms NuGet](gtk-images/win/select-forms-nuget-package.png "wybierz pakiet Xamarin.Forms NuGet")

    Wybierz pakiet, a następnie kliknij przycisk **zainstalować** przycisku.

5. Teraz wyszukać **Xamarin.Forms.Platform.GTK** pakiet w wersji 3.0 lub nowszej.

    ![Wybierz pakiet Xamarin.Forms.Platform.GTK NuGet](gtk-images/win/select-forms-platform-nuget-package.png "wybierz pakiet Xamarin.Forms.Platform.GTK NuGet")

    Wybierz pakiet, a następnie kliknij przycisk **zainstalować** przycisku.

6. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nazwę rozwiązania i wybierz **Zarządzaj pakietami NuGet dla rozwiązania**. Wybierz **aktualizacji** kartę i **Xamarin.Forms** pakietu. Wybierz wszystkie projekty i zaktualizować je do tej samej wersji zestawu narzędzi Xamarin.Forms, które jest używane przez projekt GTK.

7. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **odwołania** w projekcie biblioteki GTK. W **Menadżer odwołań** okno dialogowe, wybierz opcję **projektów** po lewej stronie i sprawdź pole wyboru obok projektu .NET Standard lub udostępnione:

    ![Odwoływać się do projektu udostępnionego](gtk-images/win/reference-shared-project.png "odwoływać się do projektu udostępnionego")

8. W **Menadżer odwołań** okno dialogowe, naciśnij klawisz **Przeglądaj** przycisk, a następnie przejdź do **C:\Program Files (x86)\GtkSharp\2.12\lib** i wybierz polecenie  **ATK sharp.dll**, **gdk sharp.dll**, **glade sharp.dll**, **glib sharp.dll**, **gtk-dotnet.dll**, **gtk sharp.dll** plików.

    ![Odwołania do bibliotek GTK #](gtk-images/win/reference-gtk-libraries.png "odwołania do bibliotek GTK #")

    Naciśnij klawisz **OK** przycisk, aby dodać odwołania.

9. W projekcie biblioteki GTK, Zmień nazwę **Class1.cs** do **Program.cs**.

10. W projekcie biblioteki GTK Edytuj **Program.cs** plików tak, aby wyglądała jak poniższy kod:

    ```csharp
    using System;
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.GTK;

    namespace GameOfLife.GTK
    {
        class MainClass
        {
            [STAThread]
            public static void Main(string[] args)
            {
                Gtk.Application.Init();
                Forms.Init();

                var app = new App();
                var window = new FormsWindow();
                window.LoadApplication(app);
                window.SetApplicationTitle("Game of Life");
                window.Show();

                Gtk.Application.Run();
            }
        }
    }
    ```

    Ten kod inicjuje GTK # i platformy Xamarin.Forms, tworzy okno aplikacji i uruchamia aplikację.

11. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt GTK i wybierz **właściwości**.

12. W **właściwości** wybierz **aplikacji** kartę i zmień **typ danych wyjściowych** menu rozwijane **aplikacji Windows**.

    ![Zmień typ danych wyjściowych projektu](gtk-images/win/change-project-output-type.png "zmienić typ danych wyjściowych projektu")

13. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt WPF i wybierz **Ustaw jako projekt startowy**. Naciśnij klawisz F5, aby uruchomić program za pomocą debugera programu Visual Studio na pulpicie Windows:

    ![Gra GTK # cyklu życia](gtk-images/win/gtk-gameoflife.png "gry GTK # cyklu życia")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Wykonaj te instrukcje, aby dodać aplikację GTK #, który będzie uruchamiany na komputerze Mac:

1. W programie Visual Studio dla komputerów Mac, kliknij prawym przyciskiem myszy na rozwiązanie Xamarin.Forms, a następnie wybierz **Dodaj > Dodaj nowy projekt...** .

2. W **nowy projekt** wybierz okno **innych > .NET > Projekt Gtk # 2.0** i naciśnij klawisz **dalej**.

3. Wpisz nazwę dla projektu z **GTK** rozszerzenia, na przykład **GameOfLife.GTK**i naciśnij klawisz **Utwórz**.

4. W **konsoli rozwiązania**, kliknij prawym przyciskiem myszy **pakiety > Dodawanie pakietów...**  dla biblioteki GTK projektu i dodawanie pakietu NuGet w wersji wstępnej Xamarin.Forms 3.0 lub nowszej.

    ![Wybierz pakiet Xamarin.Forms NuGet](gtk-images/mac/select-forms-nuget-package.png "wybierz pakiet Xamarin.Forms NuGet")

5. W **konsoli rozwiązania**, kliknij prawym przyciskiem myszy **pakiety > Dodawanie pakietów...**  dla biblioteki GTK projektu i dodawanie pakietu NuGet w wersji wstępnej Xamarin.Forms.Platform.GTK 3.0 lub nowszej.

    ![Wybierz pakiet Xamarin.Forms.Platform.GTK NuGet](gtk-images/mac/select-forms-platform-nuget-package.png "wybierz pakiet Xamarin.Forms.Platform.GTK NuGet")

6. Zaktualizuj inne projekty platformy, aby użyć tej samej wersji zestawu narzędzi Xamarin.Forms jako używane przez projekt GTK.

7. W **konsoli rozwiązania**, kliknij prawym przyciskiem myszy **odwołania > Edytuj odwołania...**  dla biblioteki GTK projektu, a następnie dodaj odwołanie do projektu Xamarin.Forms (.NET Standard lub projektu udostępnionego).

    ![Odwoływać się do projektu udostępnionego](gtk-images/mac/reference-shared-project.png "odwoływać się do projektu udostępnionego")

8. Edytuj **Program.cs** pliku projektu biblioteki GTK, tak że przypomina następujący kod:

    ```csharp
    using System;
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.GTK;

    namespace GameOfLife.GTK
    {
        class MainClass
        {
            [STAThread]
            public static void Main(string[] args)
            {
                Gtk.Application.Init();
                Forms.Init();

                var app = new App();
                var window = new FormsWindow();
                window.LoadApplication(app);
                window.SetApplicationTitle("Game of Life");
                window.Show();

                Gtk.Application.Run();
            }
        }
    }
    ```

    Ten kod inicjuje GTK # i platformy Xamarin.Forms, tworzy okno aplikacji i uruchamia aplikację.

9. W **konsoli rozwiązania**, kliknij prawym przyciskiem myszy projekt GTK i wybierz **Ustaw jako projekt startowy**.

10. W programie Visual Studio dla komputerów Mac paska narzędzi, naciśnij klawisz **Start** (przycisk trójkątna przypominającą przycisk Odtwórz) do uruchamiania aplikacji.

    ![Gra GTK # cyklu życia](gtk-images/mac/gtk-gameoflife.png "gry GTK # cyklu życia")

-----

## <a name="next-steps"></a>Następne kroki

### <a name="platform-specifics"></a>Funkcje specyficzne dla platformy

Można określić platformy aplikacji Xamarin.Forms jest uruchamiana na z XAML lub kodu. Dzięki temu można zmienić właściwości programu uruchamianego na GTK #. W kodzie, porównanie wartości `Device.RuntimePlatform` z `Device.GTK` — stała (która jest równa ciąg "GTK"). Jeśli istnieje dopasowanie, aplikacja jest uruchomiona na GTK #.

W XAML, można użyć `OnPlatform` tag, aby wybrać wartość właściwości specyficzne dla platformy:

```xaml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White" />
        <On Platform="macOS" Value="White" />
        <On Platform="Android" Value="Black" />
        <On Platform="GTK" Value="Blue" />
    </OnPlatform>
</Button.TextColor>
```

### <a name="application-icon"></a>Ikona aplikacji

Ikona aplikacji można ustawić przy uruchamianiu:

```csharp
window.SetApplicationIcon("icon.png");
```

### <a name="themes"></a>Motywy

Są dostępne dla biblioteki GTK # szerokiej gamy motywy i mogą być używane z aplikacji platformy Xamarin.Forms:

```csharp
GtkThemes.Init ();
GtkThemes.LoadCustomTheme ("Themes/gtkrc");
```

### <a name="native-forms"></a>Formularze natywne

Formularze natywne umożliwia Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-pochodnych stron, które mają być używane przez natywne projekty, w tym projekty biblioteki GTK #. Można to zrobić przez utworzenie wystąpienia [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-pochodnych strony i konwertowania go do natywnej biblioteki GTK # typu przy użyciu `CreateContainer` — metoda rozszerzenia:

```csharp
var settingsView = new SettingsView().CreateContainer();
vbox.PackEnd(settingsView, true, true, 0);
```

Aby uzyskać więcej informacji na temat formularze natywne zobacz [formularze natywne](~/xamarin-forms/platform/native-forms.md).

## <a name="issues"></a>Problemy

To jest podgląd, więc należy się spodziewać, że nie wszystko jest już gotowe do produkcji. Dla bieżącego stanu wdrożenia, zobacz [stan](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Status.md)i uzyskać obecnie znane problemy, zobacz [oczekujące & znane problemy dotyczące](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Issues-Pending.md).
