---
title: Za pomocą Visual języku platformy Xamarin.Forms
description: Szablon projektu platformy Xamarin.Forms PCL można zmodyfikować w taki sposób, aby używać języka Visual Basic dla zestawu głównego, efektywnie co pozwala na tworzenie wieloplatformowych aplikacji mobilnych za pomocą VB.NET.
ms.prod: xamarin
ms.assetid: da4b4ba9-9205-47dc-8bae-23272ede2c50
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: b858e26de95d2abbc23917b1ed5a1de65105cd8d
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinforms-using-visual-basicnet"></a>Za pomocą Visual języku platformy Xamarin.Forms

Xamarin nie obsługuje bezpośredniego Visual Basic — postępuj zgodnie z instrukcjami na tej stronie, aby utworzyć rozwiązanie PCL platformy Xamarin.Forms C#, a następnie zastąpić projektu wspólnego PCL kodu języka Visual Basic.

[![](xamarin-forms-images/hero-sml.png "Tworzenie rozwiązania PCL platformy Xamarin.Forms i zamienić projektu wspólnego PCL kodu języka Visual Basic")](xamarin-forms-images/hero.png#lightbox)

> [!NOTE]
> Aby program za pomocą Visual Basic, należy użyć programu Visual Studio w systemie Windows.

## <a name="xamarinforms-with-visual-basic-walkthrough"></a>Platformy Xamarin.Forms z przewodnikiem Visual Basic

Wykonaj następujące kroki, aby utworzyć prosty projekt platformy Xamarin.Forms, który używa języka Visual Basic:

1. Utwórz nową *platformy Xamarin.Forms C#* rozwiązania obejmującego przenośne klasy bibliotek PCL ().
Przejdź do **Plik > Nowy projekt** i w **nowy projekt** okna przejdź do **zainstalowana > Szablony > Visual C# > Cross Platform** wybierz  **Dla wielu Platform aplikacji (platformy Xamarin.Forms lub natywne) > platformy Xamarin.Forms**.

2. Kliknij prawym przyciskiem myszy w ramach rozwiązania i **Dodaj > Nowy projekt**.

3. Wybierz **Visual Basic > biblioteki klas (przenośna)** typ projektu:

   [![](xamarin-forms-images/add-vb-2-sml.png "Dodawanie nowego projektu biblioteki klas przenośnych")](xamarin-forms-images/add-vb-2.png#lightbox)

4. Wybierz platformy, jak skonfigurować poprawnego profilu PCL (należy uwzględnić Xamarin.iOS i Xamarin.Android):

   ![](xamarin-forms-images/add-vb-3-sml.png "Wybierz platformy, do obsługi")

5. Kliknij prawym przyciskiem myszy na projekt Visual Basic i wybierz polecenie **właściwości**, następnie zmienić **domyślny obszar nazw** odpowiadające istniejących C# projektów:

   ![](xamarin-forms-images/add-vb-4s-sml.png "Upewnij się, że aplikacji platformy Xamarin.Forms odpowiada głównej przestrzeni nazw w Visual Basic")

6. Kliknij prawym przyciskiem myszy nowy projekt Visual Basic i wybierz polecenie **Zarządzaj pakietami Nuget**, następnie zainstalować **platformy Xamarin.Forms** i zamknąć okno Menedżera pakietów.

   [![](xamarin-forms-images/add-vb-4-sml.png "Formularze, a następnie zamknij okno Menedżera pakietów")](xamarin-forms-images/add-vb-4.png#lightbox)

7. Zmień nazwę domyślnego **Class1** pliku *i* klasy do `App`:

   [![](xamarin-forms-images/add-vb-5-sml.png "Zmień nazwę domyślnego pliku Class1 i klasa aplikacji")](xamarin-forms-images/add-vb-5.png#lightbox)

8. Wklej następujący kod do **App.vb** pliku, który będzie punktem początkowym aplikacji platformy Xamarin.Forms. Pamiętaj, aby uwzględnić `Imports Xamarin.Forms` i Dodaj `Inherits Application` do klasy:

    ```vb 
    Imports Xamarin.Forms

    Public Class App
        Inherits Application

        Public Sub New()
            Dim label = New Label With {.HoriztonalTextAlignment = TextAlignment.Center,
                                        .FontSize = Device.GetNamedSize(NamedSize.Medium, GetType(Label)),
                                        .Text = "Welcome to Xamarin.Forms with Visual Basic.NET"}

            Dim stack = New StackLayout With {
                .VerticalOptions = LayoutOptions.Center
            }
            stack.Children.Add(label)

            Dim page = New ContentPage
            page.Content = stack
            MainPage = page

        End Sub

    End Class
    ```

9. Teraz musimy punkt systemów iOS i Android projekty w nowych projektach Visual Basic.
Kliknij prawym przyciskiem myszy **odwołania** węzeł systemu iOS i Android projektów, aby otworzyć **Menedżera odwołań**. NZ — znacznik C# przenośnej biblioteki oraz znaczników VB przenośnej biblioteki (nie zapomnij, wykonaj dla systemów iOS i Android projektów).

   [![](xamarin-forms-images/add-vb-8-sml.png "Usuń stare odwołania do projektu, należy dodać odwołanie w Visual Basic")](xamarin-forms-images/add-vb-8.png#lightbox)

10. Usuń przenoścnego projektu C#. Dodaj nowy **.vb** plików do kompilacji mieszczą się w aplikacji platformy Xamarin.Forms. Szablon dla nowych `ContentPage`s w języku Visual Basic, przedstawiono poniżej:

    ```vb
    Imports Xamarin.Forms

    Public Class Page2
    Inherits ContentPage

        Public Sub New()
            Dim label = New Label With {.HoriztonalTextAlignment = TextAlignment.Center,
                                        .FontSize = Device.GetNamedSize(NamedSize.Medium, GetType(Label)),
                                        .Text = "Visual Basic ContentPage"}

            Dim stack = New StackLayout With {
                .VerticalOptions = LayoutOptions.Center
            }
            stack.Children.Add(label)

            Content = stack
        End Sub
    End Class
    ```

## <a name="limitations-of-visual-basic-in-xamarinforms"></a>Ograniczenia Visual Basic w platformy Xamarin.Forms

Jak wspomniano w [strony przenośnej języku Visual](~/cross-platform/platform/visual-basic/index.md), Xamarin nie obsługuje języka Visual Basic. Oznacza to, że istnieją pewne ograniczenia, na której można użyć języka Visual Basic:

 - W języku Visual Basic nie można zapisać niestandardowe moduły renderowania, ich musi być napisana w języku C# w projektach natywnego platformy.

 - Zależności implementacji usługi nie można zapisać w języku Visual Basic, ich musi być napisana w języku C# w projektach natywnego platformy.

 - Strony XAML nie może występować w projektach Visual Basic — można utworzyć tylko generator związane z kodem C#. Istnieje możliwość uwzględnienia XAML w oddzielnych, do którego istnieje odwołanie, przenośnej biblioteki klas C# i użyj wiązania z danymi w celu wypełnienia plików XAML za pomocą modeli kodu języka Visual Basic (na przykład znajduje się w [próbki](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB/XamlPages)).

 - Xamarin nie obsługuje języka Visual języku.

## <a name="related-links"></a>Linki pokrewne

- [XamarinFormsVB (przykład)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB)
- [Programowanie wieloplatformowych aplikacji za pomocą programu .NET Framework (Microsoft)](http://msdn.microsoft.com/en-us/library/gg597391(v=vs.110).aspx)
