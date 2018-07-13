---
title: Dostosowywanie wpisu
description: Formant wpisu zestawu narzędzi Xamarin.Forms umożliwia pojedynczy wiersz tekstu do edycji. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla kontrolki wprowadzania, dzięki czemu deweloperzy mogą zastąpić domyślne renderowanie natywnych, dostosowując swoje własne specyficzne dla platformy.
ms.prod: xamarin
ms.assetid: 7B5DD10D-0411-424F-88D8-8A474DF16D8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 30326b8d52f39268015bdcbee1b84b9d9e5516b9
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998963"
---
# <a name="customizing-an-entry"></a>Dostosowywanie wpisu

_Formant wpisu zestawu narzędzi Xamarin.Forms umożliwia pojedynczy wiersz tekstu do edycji. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla kontrolki wprowadzania, dzięki czemu deweloperzy mogą zastąpić domyślne renderowanie natywnych, dostosowując swoje własne specyficzne dla platformy._

Każdy formant zestawu narzędzi Xamarin.Forms ma towarzyszący modułu renderowania dla każdej platformy, która tworzy wystąpienie kontrolki natywne. Gdy [ `Entry` ](xref:Xamarin.Forms.Entry) renderowania formantu przez aplikację platformy Xamarin.Forms w systemie iOS `EntryRenderer` tworzenia wystąpienia klasy, które w chwili tworzy macierzystej `UITextField` kontroli. Na platformie Android `EntryRenderer` tworzy wystąpienie klasy `EditText` kontroli. Na Universal Windows Platform (platformy UWP), `EntryRenderer` tworzy wystąpienie klasy `TextBox` kontroli. Aby uzyskać więcej informacji na temat renderowania i klasy natywnych kontrolek, mapowane kontrolek zestawu narzędzi Xamarin.Forms, zobacz [natywne kontrolki i klasy podstawowej renderowania](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Na poniższym diagramie przedstawiono relację między [ `Entry` ](xref:Xamarin.Forms.Entry) kontroli i odpowiednie kontrolki natywne, które ją implementują:

![](entry-images/entry-classes.png "Relacja między wpisu kontroli i implementowanie natywne kontrolki")

Proces renderowania może podjąć zalet do zaimplementowania dostosowań specyficzne dla platformy przez utworzenie niestandardowego modułu renderowania dla [ `Entry` ](xref:Xamarin.Forms.Entry) kontroli na każdej platformie. Proces ten jest w następujący sposób:

1. [Utwórz](#Creating_the_Custom_Entry_Control) formantu niestandardowego zestawu narzędzi Xamarin.Forms.
1. [Używanie](#Consuming_the_Custom_Control) kontrolki niestandardowej z zestawu narzędzi Xamarin.Forms.
1. [Utwórz](#Creating_the_Custom_Renderer_on_each_Platform) niestandardowego modułu renderowania kontrolki na każdej platformie.

Każdy element zostaną teraz dokładniej omówione w implementacji [ `Entry` ](xref:Xamarin.Forms.Entry) formantu, który ma inny kolor tła na każdej platformie.

<a name="Creating_the_Custom_Entry_Control" />

## <a name="creating-the-custom-entry-control"></a>Tworzenie formantu niestandardowego wpisu

Niestandardowy [ `Entry` ](xref:Xamarin.Forms.Entry) kontrolki mogą być tworzone przez podklasy `Entry` kontrolować, jak pokazano w poniższym przykładzie kodu:

```csharp
public class MyEntry : Entry
{
}
```

`MyEntry` Kontrolki została utworzona w projekcie biblioteki .NET Standard oraz jest po prostu [ `Entry` ](xref:Xamarin.Forms.Entry) kontroli. Dostosowywanie formantu będą przeprowadzane w niestandardowego modułu renderowania, dzięki czemu nie dodatkowe implementacja jest wymagana w `MyEntry` kontroli.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Korzystanie z kontrolki niestandardowej

`MyEntry` Kontroli może być przywoływany w XAML w projekcie biblioteki .NET Standard deklarowanie przestrzeni nazw dla lokalizacji i używając prefiksu przestrzeni nazw w elemencie kontrolki. Poniższy kod przedstawia przykładowy sposób, w jaki `MyEntry` kontrolki mogą być wykorzystane przez strony XAML:

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <local:MyEntry Text="In Shared Code" />
    ...
</ContentPage>
```

`local` Prefiks przestrzeni nazw może mieć nazwę niczego. Jednak `clr-namespace` i `assembly` wartości muszą być zgodne szczegóły kontrolki niestandardowej. Po zadeklarowaniu przestrzeń nazw prefiks jest używany do odwołania kontrolki niestandardowej.

Poniższy kod przedstawia przykładowy sposób, w jaki `MyEntry` kontrolki mogą być wykorzystane przez strony C#:

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

Ten kod tworzy nową [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) obiekt, który będzie wyświetlany [ `Label` ](xref:Xamarin.Forms.Label) i `MyEntry` kontroli wyśrodkowany zarówno w pionie i w poziomie na stronie.

Teraz można dodać niestandardowego modułu renderowania do każdego projektu aplikacji, aby dostosować wygląd formantu na każdej platformie.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Tworzenie niestandardowego modułu renderowania na każdej platformie

Proces tworzenia klasy niestandardowego modułu renderowania jest następująca:

1. Utwórz podklasę `EntryRenderer` klasę, która renderuje kontrolkę natywnych.
1. Zastąp `OnElementChanged` metodę, która renderuje natywnych kontroli i zapisu logiki, aby dostosować formant. Ta metoda jest wywoływana, gdy zostanie utworzony odpowiedni formant zestawu narzędzi Xamarin.Forms.
1. Dodaj `ExportRenderer` atrybutów do klasy niestandardowego modułu renderowania, aby określić, że będą używane do renderowania kontrolki zestawu narzędzi Xamarin.Forms. Ten atrybut służy do rejestrowania niestandardowego modułu renderowania przy użyciu zestawu narzędzi Xamarin.Forms.

> [!NOTE]
> Jest to opcjonalne zapewnić niestandardowego modułu renderowania w każdym projekcie platformy. Jeśli nie jest zarejestrowany niestandardowego modułu renderowania, domyślne renderowanie dla klasy podstawowej kontrolki będą używane.

Na poniższym diagramie przedstawiono obowiązki każdego projektu w przykładowej aplikacji, oraz relacje między nimi:

![](entry-images/solution-structure.png "MyEntry niestandardowego modułu renderowania projektu obowiązki")

`MyEntry` Renderowania formantu przez specyficzne dla platformy `MyEntryRenderer` klas, które wynikają z `EntryRenderer` klasy dla każdej platformy. Skutkuje to każda `MyEntry` sterowania są renderowane przy użyciu koloru tła specyficzne dla platformy, jak pokazano na poniższych zrzutach ekranu:

![](entry-images/screenshots.png "Kontrolka MyEntry na każdej platformie")

`EntryRenderer` Klasy ujawnia `OnElementChanged` metody, która jest wywoływana podczas tworzenia kontrolki zestawu narzędzi Xamarin.Forms do renderowania odpowiedniej kontrolki natywne. Ta metoda przyjmuje `ElementChangedEventArgs` parametr, który zawiera `OldElement` i `NewElement` właściwości. Te właściwości reprezentują element zestawu narzędzi Xamarin.Forms, modułu renderowania *został* dołączyć i element zestawu narzędzi Xamarin.Forms, modułu renderowania *jest* dołączone do, odpowiednio. W przykładowej aplikacji `OldElement` właściwość będzie miała `null` i `NewElement` właściwość będzie zawierać odwołanie do `MyEntry` kontroli.

Zastąpione wersję `OnElementChanged` method in Class metoda `MyEntryRenderer` klasy jest w tym miejscu Przeprowadź Dostosowywanie kontrolki natywne. Wpisane odwołania do kontrolki natywne używane na platformie jest możliwy za pośrednictwem `Control` właściwości. Ponadto można uzyskać odwołanie do formantu Xamarin.Forms, który jest renderowany przy użyciu `Element` właściwości, mimo że nie jest używany w przykładowej aplikacji.

Każda klasa niestandardowego modułu renderowania zostanie nadany `ExportRenderer` atrybut, który rejestruje modułu renderowania za pomocą zestawu narzędzi Xamarin.Forms. Ten atrybut przyjmuje dwa parametry — Nazwa typu kontrolki zestawu narzędzi Xamarin.Forms renderowanego i nazwę typu niestandardowego modułu renderowania. `assembly` Prefiks atrybutu określa, że ten atrybut ma zastosowanie do całego zestawu.

W poniższych sekcjach omówiono wykonania każdego specyficzne dla platformy `MyEntryRenderer` klasy niestandardowego modułu renderowania.

### <a name="creating-the-custom-renderer-on-ios"></a>Tworzenie niestandardowego modułu renderowania w systemie iOS

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

Wywołania do klasy bazowej `OnElementChanged` metoda tworzy wystąpienie systemu iOS `UITextField` kontroli w odniesieniu do kontrolki przypisywanych do renderowania `Control` właściwości. Kolor tła zostanie następnie ustawiona purpurowy światła z `UIColor.FromRGB` metody.

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

Wywołania do klasy bazowej `OnElementChanged` metoda tworzy wystąpienie aplikacji Android `EditText` kontroli w odniesieniu do kontrolki przypisywanych do renderowania `Control` właściwości. Kolor tła zostanie następnie ustawiona jasnozielony z `Control.SetBackgroundColor` metody.

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

Wywołania do klasy bazowej `OnElementChanged` metoda tworzy wystąpienie `TextBox` kontroli w odniesieniu do kontrolki przypisywanych do renderowania `Control` właściwości. Kolor tła zostanie następnie ustawiona błękitny, tworząc `SolidColorBrush` wystąpienia.

## <a name="summary"></a>Podsumowanie

W tym artykule ma pokazano sposób tworzenia modułu renderowania formantu niestandardowego dla Xamarin.Forms [ `Entry` ](xref:Xamarin.Forms.Entry) kontrolki, dzięki czemu deweloperzy mogą zastąpić domyślne renderowanie natywne z renderowaniem własne specyficzne dla platformy. Niestandardowe programy renderujące zapewniają zaawansowane metody umożliwiające dostosowanie wyglądu kontrolki zestawu narzędzi Xamarin.Forms. One może służyć do zmiany stylów małych lub zaawansowanych układ specyficzne dla platformy i dostosowywania zachowania.


## <a name="related-links"></a>Linki pokrewne

- [CustomRendererEntry (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/entry/)
