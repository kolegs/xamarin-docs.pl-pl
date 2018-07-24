---
title: Edytor zestawu narzędzi Xamarin.Forms
description: W tym artykule wyjaśniono, jak akceptować wprowadzanie tekstu wielowierszowego w aplikacji przy użyciu kontrolki zestawu narzędzi Xamarin.Forms edytora.
ms.prod: xamarin
ms.assetid: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/13/2018
ms.openlocfilehash: 2ec9ba6e39673b5a60911f9a9ae70474dbe2443b
ms.sourcegitcommit: 4c0093ee5d4aeb16c0e6f0c740c4796736971651
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39203114"
---
# <a name="xamarinforms-editor"></a>Edytor zestawu narzędzi Xamarin.Forms

_Wprowadzanie tekstu wielowierszowego_

[ `Editor` ](xref:Xamarin.Forms.Editor) Formant jest używany do przyjmowania danych wejściowych wielowierszowego pola. W tym artykule omówiono:

- **[Dostosowywanie](#customization)**  &ndash; klawiatury i opcje koloru.
- **[Interakcyjność](#interactivity)**  &ndash; zdarzenia, które mogą być wysłuchaliśmy dla zapewniają interakcyjność.

## <a name="customization"></a>Dostosowywanie

### <a name="setting-and-reading-text"></a>Ustawianie i odczytywanie tekstu

[ `Editor` ](xref:Xamarin.Forms.Editor), Podobnie jak inne widoki przedstawiania tekstu, udostępnia `Text` właściwości. Ta właściwość umożliwia ustawianie i odczytywanie tekstu przedstawiony przez `Editor`. W poniższym przykładzie pokazano ustawienie `Text` właściwości w XAML:

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

### <a name="auto-sizing-an-editor"></a>Automatyczna zmiana rozmiaru edytora

[ `Editor` ](xref:Xamarin.Forms.Editor) Można nawiązać automatycznie Dopasuj do jego zawartości, ustawiając [ `Editor.AutoSize` ](xref:Xamarin.Forms.Editor.AutoSize) właściwości [ `TextChanges` ](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges), który ma wartość [ `EditoAutoSizeOption` ](xref:Xamarin.Forms.EditorAutoSizeOption) wyliczenia. To wyliczenie ma dwie wartości:

- [`Disabled`](xref:Xamarin.Forms.EditorAutoSizeOption.Disabled) Wskazuje, że automatyczną zmianę rozmiaru jest wyłączone i jest wartością domyślną.
- [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges) Wskazuje, że włączono automatyczną zmianę rozmiaru.

Można to zrobić w kodzie w następujący sposób:

```xaml
<Editor Text="Enter text here" AutoSize="TextChanges" />
```

```csharp
var editor = new Editor { Text = "Enter text here", AutoSize = EditorAutoSizeOption.TextChanges };
```

Po włączeniu automatycznej zmiany rozmiaru, wysokość [ `Editor` ](xref:Xamarin.Forms.Editor) zwiększy po użytkownik wypełnia go tekstem i wysokość zmniejszy się jako użytkownik usunie tekst.

> [!NOTE]
> [ `Editor` ](xref:Xamarin.Forms.Editor) Będzie if nie autodopasowania wielkości [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) właściwość została ustawiona.

### <a name="customizing-the-keyboard"></a>Dostosowywanie klawiatury

Klawiatura, które są prezentowane podczas interakcji z [ `Editor` ](xref:Xamarin.Forms.Editor) można ustawić programowo za pośrednictwem [ `Keyboard` ](xref:Xamarin.Forms.InputView.Keyboard) właściwości do jednej z następujących właściwości z [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) klasy:

- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat) — używany do badań i miejsc, w którym emoji są przydatne.
- [`Default`](xref:Xamarin.Forms.Keyboard.Default) — Klawiatura domyślne.
- [`Email`](xref:Xamarin.Forms.Keyboard.Email) — używany podczas wprowadzania adresu e-mail.
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric) — używany podczas wprowadzania liczb.
- [`Plain`](xref:Xamarin.Forms.Keyboard.Plain) — używany podczas wprowadzania tekstu, bez jakichkolwiek [ `KeyboardFlags` ](xref:Xamarin.Forms.KeyboardFlags) określony.
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone) — używany podczas wprowadzania numerów telefonów.
- [`Text`](xref:Xamarin.Forms.Keyboard.Text) — używany podczas wprowadzania tekstu.
- [`Url`](xref:Xamarin.Forms.Keyboard.Url) — używany do wprowadzania ścieżki do plików i adresy witryn sieci web.

Można to zrobić w XAML w następujący sposób:

```xaml
<Editor Keyboard="Chat" />
```

Jest równoważny kod C#:

```csharp
var editor = new Editor { Keyboard = Keyboard.Chat };
```

Przykłady każdego klawiatury można znaleźć w naszej [przepisy](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/) repozytorium.

[ `Keyboard` ](xref:Xamarin.Forms.Keyboard) Ma również klasy [ `Create` ](xref:Xamarin.Forms.Keyboard.Create*) metoda fabryki, który może służyć do dostosowywania klawiatury, określając zachowanie wielkości liter, Sprawdź pisownię i sugestii. [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags) wartości wyliczenia podaną jako argumenty do metody, przy użyciu dostosowanych `Keyboard` zwracanego. `KeyboardFlags` Wyliczenie zawiera następujące wartości:

- [`None`](xref:Xamarin.Forms.KeyboardFlags.None) — żadne funkcje nie są dodawane do klawiatury.
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence) — Wskazuje, że pierwszą literę pierwszy wyraz każdego wpisanego zdania będą automatycznie wpisać wielkimi literami.
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck) — Wskazuje tego sprawdzania pisowni zostanie przeprowadzone wpisanego tekstu.
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions) — Wskazuje słowo uzupełnienia będą oferowane na wpisanego tekstu.
- [`CapitalizeWord`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeWord) — Wskazuje, że pierwszą literę każdego wyrazu będzie automatycznie wpisać wielkimi literami.
- [`CapitalizeCharacter`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeCharacter) — Wskazuje, że każdy znak zostanie automatycznie wpisać wielkimi literami.
- [`CapitalizeNone`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeNone) — Wskazuje, nastąpi nie automatyczne wielkie litery.
- [`All`](xref:Xamarin.Forms.KeyboardFlags.All) — Wskazuje, czy sprawdzanie pisowni, zakończenia słowa i zdania wielkość liter wystąpią na wprowadzony tekst.

Poniższy przykład kodu XAML pokazuje, jak dostosować domyślny [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) oferują uzupełnienia programu word i korzystaj każdego wprowadzonego znaku:

```xaml
<Editor>
    <Editor.Keyboard>
        <Keyboard x:FactoryMethod="Create">
            <x:Arguments>
                <KeyboardFlags>Suggestions,CapitalizeCharacter</KeyboardFlags>
            </x:Arguments>
        </Keyboard>
    </Editor.Keyboard>
</Editor>
```

Jest równoważny kod C#:

```csharp
var editor = new Editor();
editor.Keyboard = Keyboard.Create(KeyboardFlags.Suggestions | KeyboardFlags.CapitalizeCharacter);
```

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
