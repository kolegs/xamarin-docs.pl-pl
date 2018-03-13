---
title: "Podsumowanie rozdziale 10. Rozszerzenia znaczników XAML"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 575EAE55-BD4D-470F-A583-3D065FA102E2
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 3e9f630fbfc9f7a1d6346b6dd8308504a6806e1a
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-10-xaml-markup-extensions"></a>Podsumowanie rozdziale 10. Rozszerzenia znaczników XAML

Zwykle analizator XAML konwertuje dowolny ciąg jako wartość atrybutu ustawiono typ właściwości oparte na standardowych konwersji dla podstawowych typów danych .NET lub [ `TypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverter/) zależnych dołączona do właściwości lub jego typ za pomocą [`TypeConverterAttribute`](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverterAttribute/).

Ale jest czasami wygodniej atrybut z innego źródła, na przykład element słownika lub wartość właściwości statycznej lub pola, lub obliczania jakieś.

To zadanie z *rozszerzenie znaczników w XAML*. Niezależnie od nazwy, są rozszerzenia znaczników XAML *nie* rozszerzeniem XML. XAML jest zawsze prawne XML.

## <a name="the-code-infrastructure"></a>Infrastruktura kodu

Rozszerzenie znaczników w XAML jest klasa implementująca [ `IMarkupExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.IMarkupExtension/) interfejsu. Klasa taka ma często wyraz `Extension` na końcu nazwy ale pojawia się zwykle w XAML bez tego sufiksu.

Następujące rozszerzenia znaczników XAML są obsługiwane przez wszystkie implementacje XAML:

- `x:Static` obsługiwane przez [`StaticExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/)
- `x:Reference` obsługiwane przez [`ReferenceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/)
- `x:Type` obsługiwane przez [`TypeExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TypeExtension/)
- `x:Null` obsługiwane przez [`NullExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.NullExtension/)
- `x:Array` obsługiwane przez [`ArrayExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ArrayExtension/)

Te cztery rozszerzenia znaczników XAML są obsługiwane przez wiele implementacji XAML, łącznie z platformy Xamarin.Forms:

- `StaticResource` obsługiwane przez [`StaticResourceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticResourceExtension/)
- `DynamicResource` obsługiwane przez [`DynamicResourceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.DynamicResourceExtension/)
- `Binding` obsługiwane przez [ `BindingExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) &mdash;omówione w [rozdziale 16. Powiązanie danych](#chapter16)
- `TemplateBinding` obsługiwane przez [ `TemplateBindingExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TemplateBindingExtension/) &mdash;nieuwzględnionego w książce

Dodatkowe rozszerzenia znaczników XAML jest uwzględniona w platformy Xamarin.Forms w połączeniu z [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/):

- [`ConstraintExpression`](https://developer.xamarin.com/api/type/Xamarin.Forms.ConstraintExpression/)&mdash;nieuwzględnione w książce

## <a name="accessing-static-members"></a>Dostęp do statycznych elementów członkowskich

Użyj [ `x:Static` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/) elementu, aby ustawić atrybut na wartość publiczne statyczne właściwości, pole lub wyliczenia elementu członkowskiego. Ustaw [ `Member` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.StaticExtension.Member/) właściwości statycznego elementu członkowskiego. Zazwyczaj ułatwia określenie `x:Static` i nazwę elementu członkowskiego w nawiasach klamrowych. Nazwa `Member` właściwości muszą być uwzględnione, tylko ten element członkowski. Ta składnia wspólnej jest wyświetlany w obszarze [ **SharedStatics** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SharedStatics) próbki. Pól statycznego są zdefiniowane w [ `AppConstants` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter10/SharedStatics/SharedStatics/SharedStatics/AppConstants.cs) klasy. Ta technika pozwala ustanowić stałe używane za pośrednictwem programu.

Z dodatkowych deklaracji przestrzeni nazw XML, można odwoływać się właściwości publiczne statyczne, pola lub elementy członkowskie wyliczenia zdefiniowanych w programie .NET framework, jak pokazano w [ **SystemStatics** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SystemStatics) próbki .

## <a name="resource-dictionaries"></a>Słowniki zasobów

`VisualElement` Klasa definiuje właściwość o nazwie [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) ustawionej dla obiektu typu [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). W języku XAML, można przechowywać elementów w tym słowniku i zidentyfikować je za pomocą `x:Key` atrybutu. Elementy przechowywane w słowniku zasobów są współużytkowane przez wszystkie odwołania do elementu.

### <a name="staticresource-for-most-purposes"></a>StaticResource w większości przypadków

W większości przypadków użyjesz [ `StaticResource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticResourceExtension/) rozszerzenie znaczników, aby odwołać się do elementu ze słownika zasobów, jak pokazano w [ **ResourceSharing** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceSharing) próbki . Można użyć `StaticResourceExtension` element lub `StaticResource` w nawiasy klamrowe:

[![Potrójna zrzut ekranu przedstawiający udostępniania zasobów](images/ch10fg03-small.png "udostępniania zasobów")](images/ch10fg03-large.png#lightbox "udostępniania zasobów")

Nie należy mylić `x:Static` — rozszerzenie znaczników i `StaticResource` — rozszerzenie znaczników.

### <a name="a-tree-of-dictionaries"></a>Drzewo słowników

Gdy wystąpi analizator XAML `StaticResource`, rozpocznie się wyszukiwanie w górę drzewa wizualnego dla odpowiedniego klucza, a następnie wyszukuje w `ResourceDictionary` w aplikacji `App` klasy. Dzięki temu elementy ze słownika zasobów głębiej w drzewie wizualnym, aby zastąpić wyżej w drzewie wizualnym słownika zasobów. To jest przedstawiona w [ **ResourceTrees** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceTrees) próbki.

### <a name="dynamicresource-for-special-purposes"></a>DynamicResource do celów specjalnych

`StaticResource` — Rozszerzenie znaczników powoduje, że element mają zostać pobrane ze słownika, podczas tworzenia drzewa wizualnego podczas `InitializeComponent` wywołania. Zamiast `StaticResource` jest [ `DynamicResource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.DynamicResourceExtension/), który obsługuje łącze do klucza słownika i aktualizuje element docelowy, gdy element odwołuje się zmiany klucza.

Różnica między `StaticResource` i `DynamicResource` jest przedstawiona w [ **DynamicVsStatic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) próbki.

Właściwości ustawione przez `DynamicResource` musi był zabezpieczony za pomocą właściwości możliwej do wiązania, zgodnie z opisem w [działu 11 powiązania infrastruktury](chapter11.md).

## <a name="lesser-used-markup-extensions"></a>Rozszerzenia znaczników rzadziej używanych

Użyj [ `x:Null` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.NullExtension/) — rozszerzenie znaczników ustawioną właściwość `null`.

Użyj [ `x:Type` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TypeExtension/) — rozszerzenie znaczników ustawioną właściwość .NET `Type` obiektu.

Użyj [ `x:Array` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ArrayExtension/) do definiowania tablicy. Określ typ tablicy elementów członkowskich, ustawiając [`Type`] właściwości `x:Type` — rozszerzenie znaczników.

## <a name="a-custom-markup-extension"></a>Rozszerzenie znacznika niestandardowych

Można utworzyć własne rozszerzenia znaczników XAML zapisywania klasy, która implementuje [ `IMarkupExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.IMarkupExtension/) interfejsu z [ `ProvideValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Xaml.IMarkupExtension.ProvideValue/p/System.IServiceProvider/) metody.

[ `HslColorExtension` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/HslColorExtension.cs) Klasy spełnia te wymagania. Tworzy wartość typu `Color` na podstawie wartości właściwości o nazwie `H`, `S`, `L`, i `A`. Ta klasa jest pierwszym elementem w bibliotece platformy Xamarin.Forms o nazwie [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) czy jest tworzone i używane w trakcie tego podręcznika.

[ **CustomExtensionDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/CustomExtensionDemo) przykładowych pokazano, jak odwołuje się do tej biblioteki i rozszerzenie znacznika niestandardowego.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 10 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf)
- [Przykłady rozdziale 10](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10)
