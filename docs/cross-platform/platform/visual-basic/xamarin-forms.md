---
title: Zestaw narzędzi Xamarin.Forms przy użyciu Visual języku
description: Szablon projektu Xamarin.Forms PCL można zmodyfikować w taki sposób, aby używać języka Visual Basic dla zestawu głównego, umożliwiające skutecznie tworzyć aplikacje mobilne dla wielu platform przy użyciu VB.NET.
ms.prod: xamarin
ms.assetid: da4b4ba9-9205-47dc-8bae-23272ede2c50
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 256d5c81475be095c8fa0ab0408cbcf673c6b301
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997087"
---
# <a name="xamarinforms-using-visual-basicnet"></a>Zestaw narzędzi Xamarin.Forms przy użyciu Visual języku

Xamarin nie obsługuje bezpośredniego języka Visual Basic — postępuj zgodnie z instrukcjami na tej stronie, aby utworzyć rozwiązanie PCL Xamarin.Forms w języku C#, a następnie zastąp wspólnego projektu PCL kodu języka Visual Basic.

[![](xamarin-forms-images/hero-sml.png "Utwórz rozwiązanie Xamarin.Forms PCL, a następnie zastąp wspólnego projektu PCL kodu za pomocą Visual Basic")](xamarin-forms-images/hero.png#lightbox)

> [!NOTE]
> Musisz użyć programu Visual Studio na Windows do programu za pomocą Visual Basic.

## <a name="xamarinforms-with-visual-basic-walkthrough"></a>Zestaw narzędzi Xamarin.Forms przy użyciu przewodnik po języku Visual Basic

Wykonaj następujące kroki, aby utworzyć prosty projekt zestawu narzędzi Xamarin.Forms, która używa języka Visual Basic:

1. Utwórz nową *Xamarin.Forms C#* rozwiązania, który używa biblioteki klas przenośnych (PCL).
Przejdź do **Plik > Nowy projekt** i **nowy projekt** okna przejdź do **zainstalowane > Szablony > Visual C# > wiele Platform** wybierz  **Obejmujące wiele Platform aplikacji (Xamarin.Forms lub natywna) > Xamarin.Forms**.

2. Kliknij prawym przyciskiem myszy rozwiązanie i **Dodaj > Nowy projekt**.

3. Wybierz **języka Visual Basic > Biblioteka klas (przenośna)** typ projektu:

   [![](xamarin-forms-images/add-vb-2-sml.png "Dodaj nowy projekt biblioteki klas przenośnych")](xamarin-forms-images/add-vb-2.png#lightbox)

4. Wybierz platformy, jak skonfigurować prawidłowy profil PCL (Upewnij się uwzględnić Xamarin.iOS i Xamarin.Android):

   ![](xamarin-forms-images/add-vb-3-sml.png "Wybierz platformy do obsługi")

5. Kliknij prawym przyciskiem myszy w projekcie języka Visual Basic, a następnie wybierz **właściwości**, następnie zmienić **domyślny obszar nazw** do dopasowania istniejącego języka C# projektów:

   ![](xamarin-forms-images/add-vb-4s-sml.png "Upewnij się, że główna przestrzeń nazw w języku Visual Basic pasuje do aplikacji platformy Xamarin.Forms")

6. Kliknij prawym przyciskiem myszy nad nowym projektem w języku Visual Basic, a następnie wybierz **Zarządzaj pakietami Nuget**, zainstaluj **Xamarin.Forms** i zamknij okno Menedżera pakietów.

   [![](xamarin-forms-images/add-vb-4-sml.png "Formularze, a następnie zamknij okno Menedżera pakietów")](xamarin-forms-images/add-vb-4.png#lightbox)

7. Zmień nazwę domyślną **klasa1** pliku *i* klasy `App`:

   [![](xamarin-forms-images/add-vb-5-sml.png "Zmień nazwę domyślnego pliku Class1 i klasa aplikacji")](xamarin-forms-images/add-vb-5.png#lightbox)

8. Wklej następujący kod do **App.vb** pliku, który stanie się punkt początkowy aplikacji platformy Xamarin.Forms. Pamiętaj, aby uwzględnić `Imports Xamarin.Forms` i Dodaj `Inherits Application` do klasy:

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

9. Teraz musimy wskazywała nowy projekt języka Visual Basic dla systemu iOS i Android projektów.
Kliknij prawym przyciskiem myszy **odwołania** węzła w systemów iOS i Android projektów można otworzyć **Menadżer odwołań**. ONZ znaczników przenośnej biblioteki języka C# i znaczników VB przenośnej biblioteki (nie należy zapominać, zrób to dla systemów iOS i Android projektów).

   [![](xamarin-forms-images/add-vb-8-sml.png "Usuń stare odwołania do projektu, Dodaj odwołanie w Visual Basic")](xamarin-forms-images/add-vb-8.png#lightbox)

10. Usuń przenoścnego projektu C#. Dodaj nowy **.vb** plików do kompilacji poza aplikację platformy Xamarin.Forms. Szablon dla nowych `ContentPage`s w języku Visual Basic jest pokazany poniżej:

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

## <a name="limitations-of-visual-basic-in-xamarinforms"></a>Ograniczenia Visual Basic w interfejsie Xamarin.Forms

Jak wspomniano na [strony przenośnej języku Visual](~/cross-platform/platform/visual-basic/index.md), Xamarin nie obsługuje języka Visual Basic. Oznacza to, że istnieją pewne ograniczenia, w którym można korzystać z języka Visual Basic:

 - Nie można zapisać niestandardowe programy renderujące w języku Visual Basic, ich musi być napisana w języku C# w projektach platformy natywnej.

 - Nie można zapisać implementacji usługi zależności w języku Visual Basic, ich musi być napisana w języku C# w projektach platformy natywnej.

 - Strony XAML nie można uwzględnić w projekcie języka Visual Basic — można utworzyć tylko generator związanym z kodem C#. Istnieje możliwość uwzględnienia XAML w oddzielnych, której dotyczy odwołanie, przenośnej biblioteki klas C#, a następnie użyj powiązań danych do wypełnienia plików XAML, przy użyciu modeli języka Visual Basic (na przykład znajduje się w [przykładowe](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB/XamlPages)).

 - Xamarin nie obsługuje języka Visual języku.

## <a name="related-links"></a>Linki pokrewne

- [XamarinFormsVB (przykład)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB)
- [Programowanie dla wielu Platform za pomocą programu .NET Framework](https://docs.microsoft.com/dotnet/standard/cross-platform/)
