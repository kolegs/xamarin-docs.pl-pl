---
title: Edytor
description: Wprowadzanie tekstu wielowierszowego
ms.prod: xamarin
ms.assetid: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 035365a22c487039ff811756d91ca0a8d392d628
ms.sourcegitcommit: c024f29ff730ae20c15e99bfe0268a0e1c9d41e5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/23/2018
---
# <a name="editor"></a>Edytor

_Wprowadzanie tekstu wielowierszowego_

`Editor` Kontroli służy do przyjmowania danych wejściowych wiele wierszy. W tym artykule opisano:

- **[Dostosowywanie](#customization)**  &ndash; opcje klawiatury i kolor.
- **[Interakcyjności](#interactivity)**  &ndash; zdarzenia, które mogą być nasłuch dla zapewnienie interakcji.

## <a name="customization"></a>Dostosowywanie

### <a name="setting-and-reading-text"></a>Ustawienie i odczytywanie tekstu

Przedstawia edytora, jak inne widoki prezentacji tekstu `Text` właściwości. `Text` można ustawić i przeczytaj tekst przedstawiony przez `Editor`. W poniższym przykładzie pokazano ustawienie tekstu w języku XAML:

```xaml
<Editor Text="I am an Editor" />
```

W języku C#:

```csharp
var MyEditor = new Editor { Text = "I am an Editor" };
```

Aby odczytać tekstu, Uzyskaj dostęp do `Text` właściwości w języku C#:

```csharp
var text = MyEditor.Text;
```

### <a name="keyboards"></a>Klawiatury

Klawiatury, które są prezentowane, gdy użytkownicy korzystają z `Editor` można ustawić programowo przy użyciu [ ``Keyboard`` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Keyboard/) właściwości.

Dostępne są następujące opcje dla typu klawiatury:

- **Domyślne** &ndash; klawiatury domyślne
- **Rozmowa** &ndash; używany do badań & miejsca gdzie emoji są przydatne
- **Wiadomości e-mail** &ndash; używane podczas wprowadzania adresu e-mail
- **Liczbowa** &ndash; używany podczas wprowadzania liczb
- **Telefon** &ndash; używane podczas wprowadzania numerów telefonów
- **Adres URL** &ndash; używane do wprowadzania ścieżki do plików i adresy witryn sieci web

Brak [przykład każdego klawiatury](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/) w naszej sekcji przepisami.

### <a name="colors"></a>Kolory

`Editor` można ustawić do używania niestandardowy kolor tła za pośrednictwem `BackgroundColor` właściwości. Szczególną jest niezbędne do zapewnienia, że kolorów będzie można używać na każdej z platform. Ponieważ różne wartości domyślne dla kolor tekstu dotyczy wszystkich platform, konieczne może być ustawiony niestandardowy kolor tła dla każdej platformy. Zobacz [Praca z platformy Tweaks](~/xamarin-forms/platform/device.md) uzyskać więcej informacji o optymalizacji interfejsu użytkownika dla każdej platformy.

W języku C#:

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        //dark blue on UWP & Android, light blue on iOS
        var editor = new Editor { BackgroundColor = Device.RuntimePlatform == Device.iOS ? Color.FromHex("#A4EAFF") : Color.FromHex("#2c3e50") };
        layout.Children.Add(editor);
    }
}
```

W języku XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="TextSample.EditorPage"
    Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor>
                <Editor.BackgroundColor>
                    <OnPlatform x:TypeArguments="x:Color">
                        <On Platform="iOS" Value="#a4eaff" />
                        <On Platform="Android, UWP" Value="#2c3e50" />
                    </OnPlatform>
                </Editor.BackgroundColor>
            </Editor>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

![](editor-images/textbackgroundcolor.png "Edytor BackgroundColor przykładu")

Upewnij się, że wybrane kolory tła i tekstu nadają się na każdej platformie i nie może zasłaniać tekst zastępczy.

## <a name="interactivity"></a>Interakcyjność

`Editor` udostępnia dwa zdarzenia:

- [TextChanged](http://developer.xamarin.com/api/event/Xamarin.Forms.Editor.TextChanged/) &ndash; wywoływane, gdy tekst zostanie zmieniony w edytorze. Zawiera tekst przed i po zmianie.
- [Ukończono](http://developer.xamarin.com/api/event/Xamarin.Forms.Editor.Completed/) &ndash; wywoływane, gdy użytkownik zakończył dane wejściowe, naciskając klawisz return na klawiaturze.

### <a name="completed"></a>Zakończone

`Completed` Zdarzenie służy do reagowania na zakończenia interakcji z `Editor`. `Completed` jest wywoływane, gdy użytkownik zakończy dane wejściowe z pola, wprowadzając klawisz return na klawiaturze. Program obsługi dla zdarzenia jest obsługi zdarzeń rodzajowych, biorąc nadawcy i `EventArgs`:

```csharp
void EditorCompleted (object sender, EventArgs e)
{
    var text = ((Editor)sender).Text; // sender is cast to an Editor to enable reading the `Text` property of the view.
}
```

Zakończonego zdarzenia może być subskrybowana w kodzie i pliku XAML:

W języku C#:

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        var editor = new Editor ();
        editor.Completed += EditorCompleted;
        layout.Children.Add(editor);
    }
}
```

W języku XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.EditorPage"
Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor Completed="EditorCompleted" />
        </StackLayout>
    </ContentPage.Content>
</Contentpage>
```

### <a name="textchanged"></a>TextChanged

`TextChanged` Zdarzeń służy do reagowania na zmiany w zawartości pola.

`TextChanged` jest wywoływane zawsze, gdy `Text` z `Editor` zmiany. Program obsługi dla zdarzenia przełączy wystąpienia `TextChangedEventArgs`. `TextChangedEventArgs` zapewnia dostęp do starej i nowej wartości `Editor` `Text` za pośrednictwem `OldTextValue` i `NewTextValue` właściwości:

```csharp
void EditorTextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

Zakończonego zdarzenia może być subskrybowana w kodzie i pliku XAML:

W kodzie:

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        var editor = new Editor ();
        editor.TextChanged += EditorTextChanged;
        layout.Children.Add(editor);
    }
}
```

W języku XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.EditorPage"
Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor TextChanged="EditorTextChanged" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```


## <a name="related-links"></a>Linki pokrewne

- [Tekst (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Edytor interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/)
