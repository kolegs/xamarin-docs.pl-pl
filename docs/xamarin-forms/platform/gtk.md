---
title: 'Instalator platformy GTK #'
description: 'Platformy Xamarin.Forms ma teraz obsługę podglądu platformy GTK #'
ms.prod: xamarin
ms.assetid: 3417FB95-3E4B-47DA-85D0-F34832747236
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
ms.openlocfilehash: a601e74cc274fd57bb2be9af3562b3a7290d7047
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/16/2018
---
# <a name="gtk-platform-setup"></a>Instalator platformy GTK #

![Wersja zapoznawcza](~/media/shared/preview.png)

Platformy Xamarin.Forms ma teraz obsługę podglądu GTK # aplikacji. GTK # to pakiet narzędzi interfejsu graficznego użytkownika łączącego toolkit GTK + i wiele bibliotek GNOME umożliwiającego rozwój pełni natywną GNONE grafiki aplikacji przy użyciu Mono i .NET. W tym artykule przedstawiono sposób dodawania projektu GTK # do rozwiązania platformy Xamarin.Forms.

Aby rozpocząć, Utwórz nowe rozwiązanie platformy Xamarin.Forms lub użyć istniejącego rozwiązania platformy Xamarin.Forms, na przykład [ **GameOfLife**](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife/).

> [!NOTE]
> Chociaż ten artykuł dotyczy dotyczące dodawania aplikacji GTK # do rozwiązania platformy Xamarin.Forms VS2017 i Visual Studio dla komputerów Mac, również może być wykonana w [MonoDevelop](http://www.monodevelop.com/) dla systemu Linux.

## <a name="adding-a-gtk-app"></a>Dodawanie aplikacji GTK #

GTK # dla macOS i Linux jest instalowany jako część [Mono](http://www.mono-project.com/download/stable/). GTK # dla platformy .NET można zainstalować w systemie Windows z [GTK # Instalatora](http://www.mono-project.com/download/stable/#download-win).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Wykonaj te instrukcje, aby dodać aplikację GTK #, który będzie uruchamiany na pulpicie systemu Windows:

1. W programie Visual Studio 2017 r, kliknij prawym przyciskiem myszy nazwę rozwiązania w **Eksploratora rozwiązań** i wybierz polecenie **Dodaj > Nowy projekt...** .

2. W **nowy projekt** okno, w oknie po lewej stronie wybierz **Visual C#** i **Windows Desktop klasycznego**. Lista typów projektów, wybranie **biblioteki klas (.NET Framework)**i upewnij się, że **Framework** listy rozwijanej jest równa co najmniej programu .NET Framework 4.7.

3. Wpisz nazwę projektu z **GTK** rozszerzenie, na przykład **GameOfLife.GTK**. Kliknij przycisk **Przeglądaj** przycisku, wybierz folder zawierający innych platform projektów, a następnie naciśnij klawisz **wybierz Folder**. Spowoduje to umieszczenie GTK projektu w tym samym katalogu co innych projektów w rozwiązaniu.

    ![Dodaj nowy projekt GTK](gtk-images/win/add-new-project.png "dodać nowy projekt GTK")

    Naciśnij klawisz **OK** przycisk, aby utworzyć projekt.

4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nowy projekt GTK i wybierz **Zarządzaj pakietami NuGet**. Wybierz **Przeglądaj** , kliknij pozycję **Uwzględnij wersję wstępną** wyboru i wyszukaj **platformy Xamarin.Forms** 3.0 lub późniejszej.

    ![Wybierz pakiet NuGet platformy Xamarin.Forms](gtk-images/win/select-forms-nuget-package.png "wybierz pakiet NuGet platformy Xamarin.Forms")

    Wybierz pakiet, a następnie kliknij przycisk **zainstalować** przycisku.

5. Wyszukaj teraz **Xamarin.Forms.Platform.GTK** pakietu 3.0 lub nowszej.

    ![Wybierz pakiet Xamarin.Forms.Platform.GTK NuGet](gtk-images/win/select-forms-platform-nuget-package.png "wybierz pakiet Xamarin.Forms.Platform.GTK NuGet")

    Wybierz pakiet, a następnie kliknij przycisk **zainstalować** przycisku.

6. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nazwę rozwiązania i wybierz **Zarządzaj pakietami NuGet dla rozwiązania**. Wybierz **aktualizacji** kartę i **platformy Xamarin.Forms** pakietu. Wybierz wszystkie projekty i zaktualizuj je do tej samej wersji platformy Xamarin.Forms, które jest używane przez projekt GTK.

7. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **odwołania** w projekcie GTK. W **Menedżera odwołań** okno dialogowe, wybierz opcję **projektów** w lewo, a także sprawdź pole wyboru obok projektu .NET Standard, PCL lub udostępnione:

    ![Odwołania do projektu udostępnionego](gtk-images/win/reference-shared-project.png "odwołania do projektu udostępnionego")

8. W **Menedżera odwołań** okno dialogowe, naciśnij klawisz **Przeglądaj** przycisk, a następnie przejdź do **C:\Program Files (x86)\GtkSharp\2.12\lib** i wybierz polecenie  **ATK sharp.dll**, **gdk sharp.dll**, **glade sharp.dll**, **glib sharp.dll**, **gtk-dotnet.dll**, **gtk sharp.dll** plików.

    ![Odwołania do bibliotek GTK #](gtk-images/win/reference-gtk-libraries.png "odwołania do bibliotek GTK #")

    Naciśnij klawisz **OK** przycisk, aby dodać odwołania.

9. W projekcie GTK, Zmień nazwę **Class1.cs** do **Program.cs**.

10. W projekcie GTK Edytuj **Program.cs** plików w celu podobny do następującego kodu:

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

12. W **właściwości** wybierz **aplikacji** karcie i zmień **Output typu** listy rozwijanej **aplikacji systemu Windows**.

    ![Zmień typ danych wyjściowych projektu](gtk-images/win/change-project-output-type.png "zmienić typ danych wyjściowych projektu")

13. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt WPF i wybierz **Ustaw jako projekt startowy**. Naciśnij klawisz F5, aby uruchomić program z debuger programu Visual Studio na pulpicie systemu Windows:

    ![Gry GTK # życia](gtk-images/win/gtk-gameoflife.png "gry GTK # życia")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Wykonaj te instrukcje, aby dodać aplikację GTK #, który będzie uruchamiany na komputerach Mac:

1. W programie Visual Studio for Mac, kliknij prawym przyciskiem myszy w ramach rozwiązania platformy Xamarin.Forms i wybierz polecenie **Dodaj > Dodawanie nowego projektu...** .

2. W **nowy projekt** wybierz okno **innych > .NET > Projekt 2.0 Gtk #** i naciśnij klawisz **dalej**.

3. Wpisz nazwę projektu z **GTK** rozszerzenie, na przykład **GameOfLife.GTK**i naciśnij klawisz **Utwórz**.

4. W **konsoli rozwiązania**, kliknij prawym przyciskiem myszy **pakietów > Dodawanie pakietów...**  dla GTK projektu, a następnie dodaj pakiet NuGet wstępną platformy Xamarin.Forms 3.0 lub nowszej.

    ![Wybierz pakiet NuGet platformy Xamarin.Forms](gtk-images/mac/select-forms-nuget-package.png "wybierz pakiet NuGet platformy Xamarin.Forms")

5. W **konsoli rozwiązania**, kliknij prawym przyciskiem myszy **pakietów > Dodawanie pakietów...**  dla GTK projektu, a następnie dodaj pakiet NuGet wstępną Xamarin.Forms.Platform.GTK 3.0 lub nowszej.

    ![Wybierz pakiet Xamarin.Forms.Platform.GTK NuGet](gtk-images/mac/select-forms-platform-nuget-package.png "wybierz pakiet Xamarin.Forms.Platform.GTK NuGet")

6. Zaktualizuj innych projektów platformy, aby używać tej samej wersji platformy Xamarin.Forms jako używane przez projekt GTK.

7. W **konsoli rozwiązania**, kliknij prawym przyciskiem myszy **odwołania > Edytuj odwołania...**  dla GTK projektu i Dodaj odwołanie do projektu platformy Xamarin.Forms (.NET Standard, PCL lub udostępnionego projektu).

    ![Odwołania do projektu udostępnionego](gtk-images/mac/reference-shared-project.png "odwołania do projektu udostępnionego")

8. Edytuj **Program.cs** pliku projektu GTK, tak aby podobny do następującego kodu:

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

10. W programie Visual Studio dla komputerów Mac narzędzi, naciśnij klawisz **Start** (przycisk trójkątny podobny przycisk Odtwórz) można uruchomić aplikacji.

    ![Gry GTK # życia](gtk-images/mac/gtk-gameoflife.png "gry GTK # życia")

-----

## <a name="next-steps"></a>Następne kroki

### <a name="platform-specifics"></a>Szczegóły platformy

Można określić platformy aplikacji platformy Xamarin.Forms jest zasilany z XAML lub kodu. Dzięki temu można zmienić właściwości programu, gdy jest uruchomiona na GTK #. W kodzie, porównanie wartości `Device.RuntimePlatform` z `Device.GTK` stała (która jest równa ciąg "GTK"). Jeśli istnieje dopasowanie, aplikacja jest uruchomiona na GTK #.

W języku XAML, można użyć `OnPlatform` tag, aby wybrać wartość właściwości specyficzne dla platformy:

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

Ikona aplikacji można ustawić podczas uruchamiania:

```csharp
window.SetApplicationIcon("icon.png");
```

### <a name="themes"></a>Motywy

Istnieje wiele różnych kompozycji GTK # i mogą być używane z aplikacji platformy Xamarin.Forms:

```csharp
GtkThemes.Init ();
GtkThemes.LoadCustomTheme ("Themes/gtkrc");
```

### <a name="native-forms"></a>Formularze natywnego

Formularze natywnych umożliwia platformy Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-pochodnych stron, które mają być zużyte przez natywnego projektów, w tym projekty GTK #. Można to zrobić przez utworzenie wystąpienia [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-pochodnych strony i konwertowana do natywnej GTK # typu przy użyciu `CreateContainer` — metoda rozszerzenia:

```csharp
var settingsView = new SettingsView().CreateContainer();
vbox.PackEnd(settingsView, true, true, 0);
```

Aby uzyskać więcej informacji o natywnych formularzy, zobacz [natywnego formularze](~/xamarin-forms/platform/native-forms.md).

## <a name="issues"></a>Problemy

To jest podgląd, dlatego należy oczekiwać, że nie wszystko jest gotowe produkcji. Bieżący stan wdrożenia dla [stan](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Status.md)oraz obecnie znane problemy, zobacz [oczekujące & znanych problemów](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Issues-Pending.md).
