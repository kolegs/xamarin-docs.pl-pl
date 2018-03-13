---
title: Czcionki
description: Ustawienia czcionki w platformy Xamarin.Forms
ms.topic: article
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 771e1607bc4e6be8f0991e159b5d34f6d4ea9c02
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="fonts"></a>Czcionki

W tym artykule opisano sposób platformy Xamarin.Forms pozwala określić atrybuty czcionki (w tym wagi i rozmiaru) w formantach, które wyświetlania tekstu. Czcionka informacje mogą być [określona w kodzie](#Setting_Font_in_Code) lub [określony w języku Xaml](#Setting_Font_in_Xaml).
Istnieje również możliwość użycia [niestandardowe czcionki](#Using_a_Custom_Font).

<a name="Setting_Font_in_Code" />

## <a name="setting-font-in-code"></a>Ustawienie czcionki w kodzie

Użyj trzech właściwości związanych z czcionki formantów, które wyświetlania tekstu:

- **FontFamily** &ndash; `string` nazwę czcionki.
- **FontSize** &ndash; rozmiar czcionki jako `double`.
- **FontAttributes** &ndash; ciąg określający styl informacje, takie jak *Kursywa* i **Bold** (przy użyciu `FontAttributes` wyliczenia w języku C#).

Ten kod przedstawia sposób utworzyć etykietę, a następnie określ rozmiar czcionki i wagi do wyświetlenia:

```csharp
var about = new Label {
    FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
    FontAttributes = FontAttributes.Bold,
    Text = "Medium Bold Font"
};
```

<a name="FontSize" />

### <a name="font-size"></a>Rozmiar czcionki

`FontSize` Właściwość można ustawić na wartość podwójną, na przykład:

```csharp
label.FontSize = 24;
```

Można również użyć `NamedSize` wyliczenia, który ma cztery wbudowanych opcji; Platformy Xamarin.Forms wybiera najlepszy rozmiar dla każdej platformy.

-  **Micro**
-  **Małe**
-  **Średnia liczba godzin**
-  **Duże**


`NamedSize` Wyliczenia mogą być używane wszędzie tam, gdzie `FontSize` można określić za pomocą `Device.GetNamedSize` metody można przekonwertować wartości na `double`:

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

<a name="FontAttributes" />

### <a name="font-attributes"></a>Atrybuty czcionki

Style są takie jak **bold** i *kursywą* można ustawić `FontAttributes` właściwości. Obecnie obsługiwane są następujące wartości:

-  **Brak**
-  **Bold**
-  **Kursywa**

`FontAttribute` Wyliczenie może służyć następujący (można określić jeden atrybut lub `OR` je razem):

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="formattedstring"></a>FormattedString

Niektóre formanty platformy Xamarin.Forms (takich jak `Label`) obsługują także atrybutów czcionkę w ciągu przy użyciu `FormattedString` klasy. A `FormattedString` składa się z jednego lub więcej `Span`s, z których każda może mieć własną formatowanie atrybutów.

`Span` Klasa ma następujące atrybuty:

* **Tekst** &ndash; wartości do wyświetlenia
* **FontFamily** &ndash; na nazwę czcionki
* **FontSize** &ndash; rozmiar czcionki
* **FontAttributes** &ndash; informacji o stylu, takich jak *kursywą* i **bold**
* **ForegroundColor** &ndash; kolor tekstu
* **BackgroundColor** &ndash; kolor tła

Przykład tworzenie i wyświetlanie `FormattedString` są wyświetlane poniżej &ndash; należy pamiętać, że jest przypisany do etykiet `FormattedText` właściwości i nie `Text` właściwości.

```csharp
var labelFormatted = new Label ();
var fs = new FormattedString ();
fs.Spans.Add (new Span { Text="Red, ", ForegroundColor = Color.Red, FontSize = 20, FontAttributes = FontAttributes.Italic });
fs.Spans.Add (new Span { Text=" blue, ", ForegroundColor = Color.Blue, FontSize = 32 });
fs.Spans.Add (new Span { Text=" and green!", ForegroundColor = Color.Green, FontSize = 12 });
labelFormatted.FormattedText = fs;
```


### <a name="setting-font-info-per-platform"></a>Ustawianie informacji o czcionki dla każdej platformy

Alternatywnie `Device.RuntimePlatform` właściwości może służyć do Ustaw czcionkę nazwy na każdej platformie, jak pokazano w tym kodu:

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/Lobster-Regular.ttf#Lobster",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

Jest dobrym źródłem informacji czcionki dla systemu iOS [iosfonts.com](http://iosfonts.com).

<a name="Setting_Font_in_Xaml" />

## <a name="setting-the-font-in-xaml"></a>Ustawianie czcionkę w Xaml

Platformy Xamarin.Forms steruje tym tekst wyświetlany, wszystkie mają `Font` właściwości, które można ustawić w języku Xaml. Najprostszym sposobem, aby ustawić czcionkę w języku Xaml jest użycie rozmiar nazwanych wartości wyliczenia, jak pokazano w poniższym przykładzie:

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

Brak wbudowanych konwerter dla `Font` właściwość, która umożliwia wszystkie ustawienia czcionki wyrażane jako wartość ciągu w języku Xaml. W poniższych przykładach pokazano, jak można określić atrybuty czcionki oraz rozmiary w języku Xaml:

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

Aby określić wiele `Font` połączenie ustawień, wymagane ustawienia na ciąg atrybutu jednej czcionki. Ciąg atrybutu czcionek powinien być sformatowany jako `"[font-face],[attributes],[size]"`. Ważna jest kolejność parametrów, wszystkie parametry są opcjonalne, a wielu `attributes` można określić, na przykład:

```xaml
<Label Text="Small bold text" FontAttributes="Bold" FontSize="Micro" />
<Label Text="Really big italic text" FontAttributes="Italic" FontSize="72" />
```

`FormattedString` Klasa może być używana również w języku XAML, jak pokazano poniżej:

```xaml
<Label>
    <Label.FormattedText>
        <FormattedString>
            <FormattedString.Spans>
                <Span Text="Red, " ForegroundColor="Red" FontAttributes="Italic" FontSize="20" />
                <Span Text=" blue, " ForegroundColor="Blue" FontSize="32" />
                <Span Text=" and green! " ForegroundColor="Green" FontSize="12"/>
            </FormattedString.Spans>
        </FormattedString>
    </Label.FormattedText>
</Label>
```

[`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#providing-platform-values) można również w języku XAML do renderowania czcionkę na każdej z platform. W poniższym przykładzie użyto krój czcionki niestandardowych w systemie iOS (<span style="font-family:MarkerFelt-Thin">elastycznej MarkerFelt</span>) i umożliwia określenie tylko rozmiar/atrybutów na innych platformach:

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="MarkerFelt-Thin" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP, WinRT, WinPhone" Value="Assets/Fonts/Lobster-Regular.ttf#Lobster" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

Podczas określania krój czcionki niestandardowego, jest zawsze warto użyć `OnPlatform`, ponieważ jest trudne do znalezienia czcionki, która jest dostępna na wszystkich platformach.

<a name="Using_a_Custom_Font" />

## <a name="using-a-custom-font"></a>Przy użyciu niestandardowych czcionki

Przy użyciu czcionki inne niż wbudowane krojów czcionek wymaga kodowania niektóre specyficzne dla platformy. Ten zrzut ekranu przedstawia niestandardowe czcionki **homara** z [czcionki open source firmy Google](https://www.google.com/fonts) renderowane w systemach iOS, Android i Windows Phone przy użyciu platformy Xamarin.Forms.

 [![Niestandardowe czcionki w systemach iOS i Android](fonts-images/custom-sml.png "przykład czcionki niestandardowe")](fonts-images/custom.png#lightbox "przykład czcionki niestandardowe")

Kroki wymagane dla poszczególnych platform są przedstawione poniżej. W przypadku dołączania plików niestandardowych czcionek z aplikacją, należy upewnić się, że licencja czcionki zezwala na dystrybucji.

### <a name="ios"></a>iOS

Istnieje możliwość wyświetlenia czcionki niestandardowej najpierw zapewnienie, że jest załadowany, a następnie odwołujące się do niego wg nazwy przy użyciu platformy Xamarin.Forms `Font` metody.
Postępuj zgodnie z instrukcjami [ten wpis w blogu](http://blog.xamarin.com/custom-fonts-in-ios/):

1. Dodaj plik czcionki z **Akcja kompilacji: BundleResource**, i
2. Aktualizacja **Info.plist** pliku (**czcionek dostarczanych przez aplikację**, lub `UIAppFonts`, klucza), następnie
3. Odwołuje się do niego według nazwy wszędzie tam, gdzie należy zdefiniować czcionkę w platformy Xamarin.Forms!

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" : null // set only for iOS
}
```

### <a name="android"></a>Android

Platformy Xamarin.Forms dla systemu Android można odwoływać się do niestandardowego czcionki, który został dodany do projektu, postępując określony standard nazewnictwa. Najpierw dodaj plik czcionki **zasoby** folderu projektu aplikacji oraz zestaw *Akcja kompilacji: AndroidAsset*. Następnie użyj pełnej ścieżki i *nazwę czcionki* oddzielone skrótu (#) jako nazwa czcionki w platformy Xamarin.Forms, jak pokazano poniższy fragment kodu:

```csharp
new Label
{
  Text = "Hello, Forms!",
  FontFamily = Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : null // set only for Android
}
```

### <a name="windows"></a>Windows

Niestandardowe czcionki, który został dodany do projektu, postępując określony standard nazewnictwa może odwoływać się platformy Xamarin.Forms dla platform Windows. Najpierw dodaj plik czcionki **/Assets/czcionki/** folderu projektu aplikacji oraz zestaw <span class="UIItem">akcji kompilacji: zawartość</span>. Następnie użyj pełnej ścieżki i czcionki nazwę pliku, następuje wyznaczania wartości skrótu (#) i <span class="UIItem">nazwę czcionki</span>, jak pokazano poniższy fragment kodu:

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.UWP ? "Assets/Fonts/Lobster-Regular.ttf#Lobster" : null // set only for UWP apps
}
```

> [!NOTE]
> Należy pamiętać, że nazwa pliku czcionek i czcionek, mogą być różne. Aby sprawdzić nazwę czcionki w systemie Windows, kliknij prawym przyciskiem myszy plik .ttf i wybierz **Podgląd**. Następnie można określić nazwę czcionki z poziomu okna podglądu.

Typowy kod aplikacji jest teraz ukończona. Specyficzne dla platformy phone telefon kod będzie teraz zaimplementowana jako [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

### <a name="xaml"></a>XAML

Można również użyć [ `Device.RuntimePlatform` ](~/xamarin-forms/platform/device.md#providing-platform-values) w języku XAML do renderowania czcionki niestandardowej:

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="Lobster-Regular" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP, WinRT, WinPhone" Value="Assets/Fonts/Lobster-Regular.ttf#Lobster" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

Platformy Xamarin.Forms zawiera ustawienia domyślne proste, aby umożliwić rozmiaru tekst łatwo dla wszystkich obsługiwanych platformach. Umożliwia również określić krój czcionki i rozmiar &ndash; nawet inaczej dla poszczególnych platform &ndash; gdy wymagana jest bardziej szczegółową kontrolę. `FormattedString` Klasa może być używana do konstruowania ciąg zawierający specyfikacje czcionkę przy użyciu `Span` klasy.

Można również określić informacje dotyczące czcionek w języku Xaml, przy użyciu atrybutów czcionki poprawnie sformatowany lub `FormattedString` element z `Span` elementów podrzędnych.


## <a name="related-links"></a>Linki pokrewne

- [FontsSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFonts/)
- [Tekst (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/)
