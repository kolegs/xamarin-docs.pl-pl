---
title: Dostosowywanie wpis
description: Formant wpis platformy Xamarin.Forms umożliwia pojedynczy wiersz tekstu do edycji. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla formantu wpis umożliwiają deweloperom zastąpienie renderowania natywnego domyślne z własne dostosowania specyficzne dla platformy.
ms.prod: xamarin
ms.assetid: 7B5DD10D-0411-424F-88D8-8A474DF16D8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: c93681c3bfd8de8d813cbe98a7ac28b3ee8b74fc
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2018
---
# <a name="customizing-an-entry"></a>Dostosowywanie wpis

_Formant wpis platformy Xamarin.Forms umożliwia pojedynczy wiersz tekstu do edycji. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla formantu wpis umożliwiają deweloperom zastąpienie renderowania natywnego domyślne z własne dostosowania specyficzne dla platformy._

Formant co platformy Xamarin.Forms ma towarzyszący renderowania dla każdej platformy, która tworzy wystąpienie macierzystego formantu. Gdy [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) renderowania formantu przez aplikację platformy Xamarin.Forms w systemie iOS `EntryRenderer` tworzenia wystąpienia klasy, który włącza tworzy natywny `UITextField` formantu. Na platformie Android `EntryRenderer` tworzy wystąpienie klasy `EditText` formantu. W systemie Windows platformy Uniwersalnej, `EntryRenderer` tworzy wystąpienie klasy `TextBox` formantu. Aby uzyskać więcej informacji na temat klasy macierzystego formantu, mapowane na formanty platformy Xamarin.Forms i renderowania, zobacz [renderowania klasy podstawowej i kontrolki natywne](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Na poniższym diagramie przedstawiono związek między [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) kontroli i odpowiednie natywnego formantów, które implementuje ona:

![](entry-images/entry-classes.png "Relacja między wpisu kontroli i wdrażanie kontrolki natywne")

Proces renderowania można podjąć zaletą do zaimplementowania dostosowań specyficzne dla platformy przez utworzenie niestandardowego modułu renderowania dla [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) sterowania na każdej z platform. Proces ten wygląda następująco:

1. [Utwórz](#Creating_the_Custom_Entry_Control) kontrolkę niestandardową platformy Xamarin.Forms.
1. [Korzystać z](#Consuming_the_Custom_Control) kontrolka niestandardowa z platformy Xamarin.Forms.
1. [Utwórz](#Creating_the_Custom_Renderer_on_each_Platform) niestandardowego modułu renderowania dla formantu w każdej z platform.

Każdy element teraz omówione zostaną z kolei, aby zaimplementować [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) formantu, który ma inny kolor tła na każdej z platform.

<a name="Creating_the_Custom_Entry_Control" />

## <a name="creating-the-custom-entry-control"></a>Tworzenie formantu niestandardowego wpisu

Niestandardowy [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) formant może zostać utworzony przez podklasy `Entry` kontroli, jak pokazano w poniższym przykładzie:

```csharp
public class MyEntry : Entry
{
}
```

`MyEntry` Kontroli jest tworzony w .NET Standard projektu biblioteki i jest po prostu [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) formantu. Dostosowywanie formantu zostanie przeprowadzone w niestandardowego modułu renderowania, więc nie dodatkowe implementacja jest wymagana w `MyEntry` formantu.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Korzystanie z formantu niestandardowego

`MyEntry` Formant może być przywoływany w języku XAML w .NET Standard projektu biblioteki deklarowanie przestrzeni nazw dla lokalizacji, a następnie użyć prefiksu przestrzeni nazw w elemencie formantu. Poniższy kod przedstawia przykład sposobu `MyEntry` formant może być zużyte przez strony XAML:

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <local:MyEntry Text="In Shared Code" />
    ...
</ContentPage>
```

`local` Prefiks przestrzeni nazw może mieć nazwę żadnych czynności. Jednak `clr-namespace` i `assembly` wartości muszą być zgodne szczegóły kontrolki niestandardowej. Po zadeklarowaniu obszaru nazw prefiks jest używany do odwołania kontrolki niestandardowej.

Poniższy kod przedstawia przykład sposobu `MyEntry` formant może być zużyte przez stronę C#:

```csharp
public class MainPage : ContentPage
{
  public MainPage ()
  {
    Content = new StackLayout {
      Children = {
        new Label {
          Text = "Hello, Custom Renderer !",
        },
        new MyEntry {
          Text = "In Shared Code",
        }
      },
      VerticalOptions = LayoutOptions.CenterAndExpand,
      HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
  }
}
```

Ten kod tworzy nową [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) obiekt, który będzie wyświetlana [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) i `MyEntry` kontroli skupia się zarówno w poziomie i w pionie na stronie.

Teraz można dodać niestandardowego modułu renderowania do każdego projektu aplikacji, aby dostosować wygląd formantu na każdej z platform.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Tworzenie niestandardowego modułu renderowania na każdej platformie

Proces tworzenia klasy niestandardowego modułu renderowania wygląda następująco:

1. Utwórz podklasę `EntryRenderer` klasy, która renderuje macierzystego formantu.
1. Zastąpienie `OnElementChanged` metodę, która renderuje Dostosowywanie formantu natywnego logika kontroli i zapisu. Ta metoda jest wywoływana po utworzeniu odpowiedniego formantu platformy Xamarin.Forms.
1. Dodaj `ExportRenderer` atrybutu klasy niestandardowego modułu renderowania, aby określić, że będą używane do renderowania kontrolki na platformy Xamarin.Forms. Ten atrybut służy do rejestrowania niestandardowego modułu renderowania z platformy Xamarin.Forms.

> [!NOTE]
> Jest to pozycja opcjonalna zapewnienie niestandardowego modułu renderowania w każdym projekcie platformy. Jeśli nie jest zarejestrowany niestandardowego modułu renderowania, domyślne renderowanie dla klasy podstawowej formantu będzie używany.

Na poniższym diagramie przedstawiono obowiązki każdego projektu w przykładowej aplikacji, oraz relacje między nimi:

![](entry-images/solution-structure.png "Obowiązki MyEntry niestandardowe renderowania projektu:")

`MyEntry` Renderowania formantu przez specyficzne dla platformy `MyEntryRenderer` klasy, które wynikają z `EntryRenderer` klasy dla każdej platformy. Powoduje to w każdym `MyEntry` kontrolować renderowanego kolorem tła specyficzne dla platformy, jak pokazano na poniższych zrzutach ekranu:

![](entry-images/screenshots.png "Formant MyEntry na każdej platformie")

`EntryRenderer` Klasy ujawnia `OnElementChanged` metodę, która jest wywoływana po utworzeniu formantu platformy Xamarin.Forms renderowanie odpowiednich macierzystego formantu. Ta metoda przyjmuje `ElementChangedEventArgs` parametr, który zawiera `OldElement` i `NewElement` właściwości. Te właściwości reprezentuje elementu platformy Xamarin.Forms który renderującego *został* dołączyć i elementu platformy Xamarin.Forms który renderującego *jest* dołączony do, odpowiednio. W przykładowej aplikacji `OldElement` właściwość będzie `null` i `NewElement` właściwości będzie zawierać odwołanie do `MyEntry` formantu.

Zastąpiona wersja `OnElementChanged` metoda `MyEntryRenderer` klasy jest miejscem, aby wykonać dostosowanie macierzystego formantu. Typu odwołanie do macierzystego formantu używanego na platformie jest możliwy za pośrednictwem `Control` właściwości. Ponadto można uzyskać odwołanie do formantu platformy Xamarin.Forms, który jest renderowany przy użyciu `Element` właściwości, mimo że nie jest on używany w przykładowej aplikacji.

Każda klasa niestandardowego modułu renderowania zostanie nadany `ExportRenderer` atrybut, który rejestruje mechanizm renderujący platformy Xamarin.Forms. Atrybut przyjmuje dwa parametry — Nazwa typu formantu platformy Xamarin.Forms renderowanego i nazwę typu niestandardowego modułu renderowania. `assembly` Prefiks atrybutu określa, że ten atrybut ma zastosowanie do całego zestawu.

W poniższych sekcjach omówiono wdrożenia każdego specyficzne dla platformy `MyEntryRenderer` klasy niestandardowego modułu renderowania.

### <a name="creating-the-custom-renderer-on-ios"></a>Tworzenie modułu renderowania niestandardowe w systemie iOS

Poniższy przykład kodu pokazuje niestandardowego modułu renderowania dla platformy iOS:

```csharp
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer (typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.iOS
{
    public class MyEntryRenderer : EntryRenderer
    {
        protected override void OnElementChanged (ElementChangedEventArgs<Entry> e)
        {
            base.OnElementChanged (e);

            if (Control != null) {
                // do whatever you want to the UITextField here!
                Control.BackgroundColor = UIColor.FromRGB (204, 153, 255);
                Control.BorderStyle = UITextBorderStyle.Line;
            }
        }
    }
}
```

Wywołania do klasy podstawowej `OnElementChanged` metoda tworzy iOS `UITextField` kontroli w odniesieniu do kontrolki jest przypisywany do renderowania `Control` właściwości. Kolor tła jest następnie ustawioną światła purpurowy z `UIColor.FromRGB` metody.

### <a name="creating-the-custom-renderer-on-android"></a>Tworzenie niestandardowego modułu renderowania w systemie Android

Poniższy przykład kodu pokazuje niestandardowego modułu renderowania dla platformy systemu Android:

```csharp
using Xamarin.Forms.Platform.Android;

[assembly: ExportRenderer(typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.Android
{
    class MyEntryRenderer : EntryRenderer
    {
        public MyEntryRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<Entry> e)
        {
            base.OnElementChanged(e);

            if (Control != null)
            {
                Control.SetBackgroundColor(global::Android.Graphics.Color.LightGreen);
            }
        }
    }
}
```

Wywołania do klasy podstawowej `OnElementChanged` metoda tworzy Android `EditText` kontroli w odniesieniu do kontrolki jest przypisywany do renderowania `Control` właściwości. Kolor tła jest następnie ustawioną jasnozielony z `Control.SetBackgroundColor` metody.

### <a name="creating-the-custom-renderer-on-uwp"></a>Tworzenie niestandardowego modułu renderowania na platformy uniwersalnej systemu Windows

Poniższy przykład kodu pokazuje niestandardowego modułu renderowania dla platformy uniwersalnej systemu Windows:

```csharp
[assembly: ExportRenderer(typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.UWP
{
    public class MyEntryRenderer : EntryRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Entry> e)
        {
            base.OnElementChanged(e);

            if (Control != null)
            {
                Control.Background = new SolidColorBrush(Colors.Cyan);
            }
        }
    }
}
```

Wywołania do klasy podstawowej `OnElementChanged` metoda tworzy `TextBox` kontroli w odniesieniu do kontrolki jest przypisywany do renderowania `Control` właściwości. Kolor tła następnie ustaw błękitny tworząc `SolidColorBrush` wystąpienia.

## <a name="summary"></a>Podsumowanie

W tym artykule wykazała tworzenie renderowanie formantu niestandardowego dla platformy Xamarin.Forms [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) formantu umożliwiają deweloperom zastąpienie renderowania natywnego domyślne z własnych renderowania specyficzne dla platformy. Niestandardowe moduły renderowania zapewniają zaawansowane podejście do dostosowywania wyglądu formantów platformy Xamarin.Forms. Służy do zmiany małych stylów lub Zaawansowane układu specyficzne dla platformy i dostosowywania zachowania.


## <a name="related-links"></a>Linki pokrewne

- [CustomRendererEntry (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/entry/)
