---
title: Edytor zestawu narzędzi Xamarin.Forms
description: W tym artykule wyjaśniono, jak akceptować wprowadzanie tekstu wielowierszowego w aplikacji przy użyciu kontrolki zestawu narzędzi Xamarin.Forms edytora.
ms.prod: xamarin
ms.assetid: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: 4879ff88d5bbdab5aa92024bee7f50239a141e3b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995870"
---
# <a name="xamarinforms-editor"></a>Edytor zestawu narzędzi Xamarin.Forms

_Wprowadzanie tekstu wielowierszowego_

`Editor` Kontroli jest używany do przyjmowania danych wejściowych wiele wierszy. W tym artykule omówiono:

- **[Dostosowywanie](#customization)**  &ndash; klawiatury i opcje koloru.
- **[Interakcyjność](#interactivity)**  &ndash; zdarzenia, które mogą być wysłuchaliśmy dla zapewniają interakcyjność.

## <a name="customization"></a>Dostosowywanie

### <a name="setting-and-reading-text"></a>Ustawianie i odczytywanie tekstu

`Editor`, Podobnie jak inne widoki przedstawiania tekstu, udostępnia `Text` właściwości. Ta właściwość umożliwia ustawianie i odczytywanie tekstu przedstawiony przez `Editor`. W poniższym przykładzie pokazano ustawienie `Text` właściwości w XAML:

```xaml
<Editor Text="I am an Editor" />
```

W języku C#:

```csharp
var MyEditor = new Editor { Text = "I am an Editor" };
```

Aby odczytać tekstu, dostęp do `Text` właściwości w języku C#:

```csharp
var text = MyEditor.Text;
```

### <a name="limiting-input-length"></a>Ograniczenie długości danych wejściowych

[ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) Właściwość może służyć do ograniczania długości danych wejściowych, które są dozwolone dla [ `Editor` ](xref:Xamarin.Forms.Editor). Ta właściwość powinna być ustawiona na dodatnią liczbą całkowitą:

```xaml
<Editor ... MaxLength="10" />
```

```csharp
var editor = new Editor { ... MaxLength = 10 };
```

A [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) właściwości wartość 0 oznacza, że może być brak danych wejściowych, a wartość `int.MaxValue`, co jest wartością domyślną dla [ `Editor` ](xref:Xamarin.Forms.Editor), wskazuje, że istnieje nie skuteczne limit liczby znaków, które mogą być wprowadzane.

### <a name="keyboards"></a>Klawiatury

Klawiatura, które są prezentowane podczas interakcji z `Editor` można ustawić programowo za pośrednictwem [ ``Keyboard`` ](xref:Xamarin.Forms.Keyboard) właściwości.

Opcje typu klawiatury są następujące:

- **Domyślne** &ndash; klawiatury domyślne
- **Porozmawiaj** &ndash; używane dla badań & miejsca gdzie przydają się emoji
- **Adres e-mail** &ndash; używane podczas wprowadzania adresu e-mail
- **Liczbowe** &ndash; używane podczas wprowadzania liczb
- **Telefon** &ndash; używane podczas wprowadzania numerów telefonów
- **Adres URL** &ndash; używane do wprowadzania ścieżki do plików i adresy witryn sieci web

Brak [przykładzie każdy klawiatury](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/) w naszej sekcji przepisy.

### <a name="enabling-and-disabling-spell-checking"></a>Włączanie i wyłączanie sprawdzania pisowni

[ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) Formantów właściwości tego, czy sprawdzanie pisowni jest włączone. Domyślnie ustawiono właściwość `true`. Wprowadzania tekstu, są oznaczone błędnie napisanych wyrazów.

Jednak w niektórych scenariuszach wpis tekstu, na przykład wprowadzając nazwę użytkownika, sprawdzanie pisowni zapewnia środowisko ujemne, a tym samym powinny być wyłączone przez ustawienie [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) właściwości `false`:

```xaml
<Editor ... IsSpellCheckEnabled="false" />
```

```csharp
var editor = new Editor { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> Gdy [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) właściwość jest ustawiona na `false`, a klawiatury niestandardowej nie jest używana, moduł sprawdzania pisowni natywny zostanie wyłączona. Jednak jeśli [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) ma został zestaw, który wyłącza pisowni kontroli, takie jak [ `Keyboard.Chat` ](xref:Xamarin.Forms.Keyboard.Chat), `IsSpellCheckEnabled` właściwość jest ignorowana. W związku z tym, aby włączyć sprawdzanie pisowni, w którym nie można użyć właściwości `Keyboard` wyłączają jawnie.

### <a name="colors"></a>Kolory

`Editor` można ustawić do użycia niestandardowy kolor tła za pośrednictwem `BackgroundColor` właściwości. Szczególną jest niezbędne do zapewnienia, że kolory będzie można używać na każdej platformie. Ponieważ każdej z platform ma różne wartości domyślne dla kolor tekstu, konieczne może być ustawiony niestandardowy kolor tła dla każdej platformy. Zobacz [pracę ulepszenia platformy](~/xamarin-forms/platform/device.md) Aby uzyskać więcej informacji na temat optymalizowania interfejsu użytkownika dla każdej platformy.

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

W XAML:

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

Upewnij się, że wybrane kolory tła i tekstu można używać na każdej platformie i nie może zasłaniać dowolny tekst symbolu zastępczego.

## <a name="interactivity"></a>Interakcyjność

`Editor` udostępnia dwa zdarzenia:

- [TextChanged](xref:Xamarin.Forms.Editor.TextChanged) &ndash; wywoływane, gdy tekst zostanie zmieniony w edytorze. Zawiera tekst, przed zmianą i po niej.
- [Ukończono](xref:Xamarin.Forms.Editor.Completed) &ndash; wywoływane, gdy użytkownik zakończyła danych wejściowych, naciskając klawisz ENTER na klawiaturze.

### <a name="completed"></a>Zakończone

`Completed` Zdarzeń służy do reagowania na ukończenie interakcję z `Editor`. `Completed` jest wywoływane, gdy użytkownik zakończy dane wejściowe z polem, wprowadzając klawisz ENTER na klawiaturze. Program obsługi zdarzenia jest program obsługi zdarzeń generycznych, biorąc nadawcy i `EventArgs`:

```csharp
void EditorCompleted (object sender, EventArgs e)
{
    var text = ((Editor)sender).Text; // sender is cast to an Editor to enable reading the `Text` property of the view.
}
```

Ukończone zdarzenie może być subskrybowana w kodzie i XAML:

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

W XAML:

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

### <a name="textchanged"></a>Textchanged.

`TextChanged` Zdarzeń służy do reagowania na zmiany w treści pola.

`TextChanged` jest wywoływane zawsze, gdy `Text` z `Editor` zmiany. Program obsługi zdarzenia przyjmuje wystąpienie klasy `TextChangedEventArgs`. `TextChangedEventArgs` zapewnia dostęp do starej i nowej wartości z `Editor` `Text` za pośrednictwem `OldTextValue` i `NewTextValue` właściwości:

```csharp
void EditorTextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

Ukończone zdarzenie może być subskrybowana w kodzie i XAML:

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

W XAML:

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
- [Edytor interfejsu API](xref:Xamarin.Forms.Editor)
