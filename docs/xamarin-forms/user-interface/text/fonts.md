---
title: Czcionki w interfejsie Xamarin.Forms
description: W tym artykule wyjaśniono, jak Podaj informacje czcionki kontrolek, które wyświetlania tekstu w aplikacjach Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: e6635bc13214a5a4e728fa3e71db86a8ea1c39d6
ms.sourcegitcommit: 4c0093ee5d4aeb16c0e6f0c740c4796736971651
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39202958"
---
# <a name="fonts-in-xamarinforms"></a>Czcionki w interfejsie Xamarin.Forms

W tym artykule opisano, jak Xamarin.Forms pozwala określić atrybuty czcionki (w tym zakresie wagi i rozmiar) na kontrolki, które wyświetlania tekstu. Czcionka informacje mogą być [określić w kodzie](#Setting_Font_in_Code) lub [określone w XAML](#Setting_Font_in_Xaml).
Istnieje również możliwość użycia [niestandardowej czcionki](#Using_a_Custom_Font).

<a name="Setting_Font_in_Code" />

## <a name="setting-font-in-code"></a>Ustawienie czcionki w kodzie

Użyj trzech właściwości związane z czcionki formantów, które wyświetlania tekstu:

- **FontFamily** &ndash; `string` nazwę czcionki.
- **FontSize** &ndash; rozmiar czcionki jako `double`.
- **FontAttributes** &ndash; ciąg określający informacje, takie jak styl *Kursywa* i **Bold** (przy użyciu `FontAttributes` wyliczenia w języku C#).

Ten kod pokazuje, jak utworzyć etykietę, a następnie określ rozmiar czcionki i wagi do wyświetlenia:

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

Można również użyć `NamedSize` wyliczenia, która ma cztery wbudowane opcje; Zestaw narzędzi Xamarin.Forms wybiera najlepszy rozmiar dla każdej platformy.

-  **Micro**
-  **Małe**
-  **Średni**
-  **Duże**

`NamedSize` Wyliczenia mogą być używane wszędzie tam, gdzie `FontSize` można określać z użyciem `Device.GetNamedSize` metodę, aby przekonwertować wartości na `double`:

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

<a name="FontAttributes" />

### <a name="font-attributes"></a>Atrybuty czcionki

Style są takie jak **bold** i *italic* nelze nastavit `FontAttributes` właściwości. Obecnie obsługiwane są następujące wartości:

-  **Brak**
-  **Bold**
-  **Kursywa**

`FontAttribute` Wyliczenie może służyć w następujący sposób (można określić jeden atrybut lub `OR` łącznie):

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="setting-font-info-per-platform"></a>Ustawianie informacji czcionki dla danej platformy

Alternatywnie `Device.RuntimePlatform` właściwość może służyć do Ustaw czcionkę nazwy na każdej platformie, jak pokazano w tym kodzie:

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/Lobster-Regular.ttf#Lobster",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

Jest dobrym źródłem informacji czcionki dla systemu iOS [iosfonts.com](http://iosfonts.com).

<a name="Setting_Font_in_Xaml" />

## <a name="setting-the-font-in-xaml"></a>Ustawianie czcionkę w XAML

Ten tekst wyświetlania wszystkich pochodzi od zestawu narzędzi Xamarin.Forms `Font` właściwości, które można ustawić w XAML. Najprostszym sposobem, aby ustawić czcionkę w XAML jest użycie rozmiaru nazwanych wartości wyliczenia, jak pokazano w poniższym przykładzie:

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

Ma wbudowanych konwerter służący do `Font` właściwość, która zezwala na wszystkie ustawienia czcionki być wyrażone jako wartość ciągu w XAML. W poniższych przykładach pokazano, jak można określić atrybuty czcionki oraz rozmiary w XAML:

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

Aby określić wiele `Font` ustawienia, Połącz wymagane ustawienia w jednym `Font` atrybut ciągu. Czcionka ciągu atrybutu powinny być sformatowane jako `"[font-face],[attributes],[size]"`. Ważna jest kolejność parametrów, wszystkie parametry są opcjonalne, a wiele `attributes` można określić, na przykład:

```xaml
<Label Text="Small bold text" Font="Bold, Micro" />
<Label Text="Medium custom font" Font="MarkerFelt-Thin, 42" />
<Label Text="Really big bold and italic text" Font="Bold, Italic, 72"  />
```

[`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#providing-platform-values) można również w XAML do renderowania czcionkę na każdej platformie. W poniższym przykładzie użyto krój czcionki niestandardowe w systemie iOS (<span style="font-family:MarkerFelt-Thin">alokowanych MarkerFelt</span>) i określa rozmiar/atrybuty tylko na innych platformach:

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="MarkerFelt-Thin" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP" Value="Assets/Fonts/Lobster-Regular.ttf#Lobster" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

Podczas określania krój czcionki niestandardowe, zawsze jest dobry pomysł, aby użyć `OnPlatform`, ponieważ jest trudne do znalezienia czcionkę, która jest dostępna na wszystkich platformach.

<a name="Using_a_Custom_Font" />

## <a name="using-a-custom-font"></a>Przy użyciu niestandardowej czcionki

Przy użyciu czcionki inne niż wbudowane krojów czcionek wymaga kod specyficzny dla platformy. Ten zrzut ekranu przedstawia niestandardowej czcionki **firma Lobster** z [czcionki typu open source firmy Google](https://www.google.com/fonts) renderowany przy użyciu zestawu narzędzi Xamarin.Forms.

 [![Niestandardowej czcionki w systemach iOS i Android](fonts-images/custom-sml.png "przykład czcionki niestandardowe")](fonts-images/custom.png#lightbox "przykład czcionki niestandardowe")

Czynności wymagane dla każdej z platform są przedstawione poniżej. Podczas dołączania plików niestandardowej czcionki z aplikacji, należy koniecznie sprawdź, czy licencja czcionki zezwala dla dystrybucji.

### <a name="ios"></a>iOS

Istnieje możliwość wyświetlić niestandardowej czcionki po pierwsze sprawdzenia, że jest załadowany, a następnie odwołując się do niego według nazwy przy użyciu zestawu narzędzi Xamarin.Forms `Font` metody.
Postępuj zgodnie z instrukcjami w [ten wpis w blogu](http://blog.xamarin.com/custom-fonts-in-ios/):

1. Dodaj plik czcionki z **Build Action: BundleResource**, i
2. Aktualizacja **Info.plist** pliku (**czcionki zapewniane przez aplikację**, lub `UIAppFonts`, key), następnie
3. Odwoływać się do niego według nazwy wszędzie tam, gdzie należy zdefiniować czcionkę w interfejsie Xamarin.Forms!

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" : null // set only for iOS
}
```

### <a name="android"></a>Android

Xamarin.Forms dla systemu Android mogą odwoływać się do niestandardowej czcionki, który został dodany do projektu postępując zgodnie z określonym standard nazewnictwa. Najpierw dodaj plik czcionki do **zasoby** folderu w projekcie aplikacji i zestaw *Build Action: AndroidAsset*. Następnie użyj pełnej ścieżki i *nazwa czcionki* rozdzielone krzyżyka (#) jako nazwa czcionki w interfejsie Xamarin.Forms, tak jak pokazano w poniższym fragmencie kodu:

```csharp
new Label
{
  Text = "Hello, Forms!",
  FontFamily = Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : null // set only for Android
}
```

### <a name="windows"></a>Windows

Zestaw narzędzi Xamarin.Forms dla platform Windows odwoływać się do niestandardowej czcionki, który został dodany do projektu postępując zgodnie z określonym standard nazewnictwa. Najpierw dodaj plik czcionki do **/Assets/czcionki/** folderu w projekcie aplikacji i zestaw <span class="UIItem">kompilacji: zawartość akcji</span>. Następnie użyj pełnej ścieżki i czcionki nazwę pliku, następuje krzyżyka (#) i <span class="UIItem">nazwa czcionki</span>, tak jak pokazano w poniższym fragmencie kodu:

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.UWP ? "Assets/Fonts/Lobster-Regular.ttf#Lobster" : null // set only for UWP apps
}
```

> [!NOTE]
> Należy pamiętać, że nazwa pliku czcionek i czcionek, mogą być różne. Aby sprawdzić nazwę czcionki na Windows, kliknij prawym przyciskiem myszy plik .ttf, a następnie wybierz **Podgląd**. Następnie można ustalić nazwę czcionki na podstawie okno podglądu.

Typowy kod dla aplikacji została zakończona. Kod telefon phone specyficzne dla platformy zostaną teraz zaimplementowane jako [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

### <a name="xaml"></a>XAML

Można również użyć [ `Device.RuntimePlatform` ](~/xamarin-forms/platform/device.md#providing-platform-values) w XAML do renderowania niestandardowej czcionki:

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="Lobster-Regular" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP" Value="Assets/Fonts/Lobster-Regular.ttf#Lobster" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

Xamarin.Forms zawiera ustawienia domyślne proste pozwala rozmiar tekstu w prosty sposób dla wszystkich obsługiwanych platform. Pozwala też określić krój czcionki i rozmiar &ndash; nawet w różny sposób dla poszczególnych platform &ndash; gdy wymagane jest bardziej precyzyjną kontrolę.

Informacje dotyczące czcionek można również określić w XAML przy użyciu atrybutów czcionki poprawnie sformatowany.

## <a name="related-links"></a>Linki pokrewne

- [FontsSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFonts/)
- [Tekst (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/)
