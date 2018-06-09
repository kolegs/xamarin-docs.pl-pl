---
title: Edytor platformy Xamarin.Forms
description: W tym artykule opisano sposób użycia formantu Edytor platformy Xamarin.Forms do akceptowania wprowadzanie tekstu wielowierszowego w aplikacji.
ms.prod: xamarin
ms.assetid: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: 7ad8c8aa77e23c5a8fb7649736ecb271f835d1a7
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245527"
---
# <a name="xamarinforms-editor"></a>Edytor platformy Xamarin.Forms

_Wprowadzanie tekstu wielowierszowego_

`Editor` Kontroli służy do przyjmowania danych wejściowych wiele wierszy. W tym artykule opisano:

- **[Dostosowywanie](#customization)**  &ndash; opcje klawiatury i kolor.
- **[Interakcyjności](#interactivity)**  &ndash; zdarzenia, które mogą być nasłuch dla zapewnienie interakcji.

## <a name="customization"></a>Dostosowywanie

### <a name="setting-and-reading-text"></a>Ustawienie i odczytywanie tekstu

`Editor`, Jak inne widoki prezentacji tekst przedstawia `Text` właściwości. Tej właściwości można ustawić i przeczytaj tekst przedstawiony przez `Editor`. W poniższym przykładzie pokazano ustawienie `Text` właściwości w języku XAML:

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

### <a name="limiting-input-length"></a>Ograniczenie długości danych wejściowych

[ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) Właściwości może służyć do ograniczenia długości danych wejściowych, które są dozwolone dla [ `Editor` ](xref:Xamarin.Forms.Editor). Ta właściwość powinna być równa dodatnią liczbą całkowitą:

```xaml
<Editor ... MaxLength="10" />
```

```csharp
var editor = new Editor { ... MaxLength = 10 };
```

A [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) właściwości wartość 0 oznacza, że może być brak danych wejściowych i wartość `int.MaxValue`, która jest wartością domyślną dla [ `Editor` ](xref:Xamarin.Forms.Editor), wskazuje, że istnieje nie skuteczne limit liczby znaków, które mogą być wprowadzane.

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

### <a name="enabling-and-disabling-spell-checking"></a>Włączanie i wyłączanie sprawdzania pisowni

[ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) Formantów właściwość określa, czy sprawdzanie pisowni jest włączone. Domyślnie ma ustawioną właściwość `true`. Wprowadzania tekstu, są oznaczone pisowni.

Jednak niektóre scenariusze wprowadzania tekstu, takie jak wprowadzić nazwę użytkownika sprawdzanie pisowni zapewnia środowisko ujemna i dlatego powinny być wyłączone przez ustawienie [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) właściwości `false`:

```xaml
<Editor ... IsSpellCheckEnabled="false" />
```

```csharp
var editor = new Editor { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> Gdy [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) właściwość jest ustawiona na `false`i niestandardowe klawiatury nie jest używana, moduł sprawdzania pisowni macierzystego zostanie wyłączone. Jednak jeśli [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) ma zostały zestawu, które wyłączają pisowni sprawdzania, takich jak [ `Keyboard.Chat` ](xref:Xamarin.Forms.Keyboard.Chat), `IsSpellCheckEnabled` właściwość jest ignorowana. Właściwości nie można użyć do włączenia sprawdzania pisowni `Keyboard` który jawnie wyłącza.

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
