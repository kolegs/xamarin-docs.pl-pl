---
title: Podsumowanie rozdziałów 10. Rozszerzenia znaczników w XAML
description: 'Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms: Podsumowanie rozdziałów 10. Rozszerzenia znaczników w XAML'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 575EAE55-BD4D-470F-A583-3D065FA102E2
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 1ee19d96e39534ccce5238eca3a90ba5c8d9d451
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997518"
---
# <a name="summary-of-chapter-10-xaml-markup-extensions"></a>Podsumowanie rozdziałów 10. Rozszerzenia znaczników w XAML

Normalnie, analizator składni XAML konwertuje dowolny ciąg, Ustaw jako wartość atrybutu do typu właściwości, w oparciu o standardowe definicje konwersji dla typów podstawowych danych .NET lub [ `TypeConverter` ](xref:Xamarin.Forms.TypeConverter) utworów zależnych dołączone do właściwości lub jego typ za pomocą [`TypeConverterAttribute`](xref:Xamarin.Forms.TypeConverterAttribute).

Jednak czasami wygodnie można ustawić atrybutu z innego źródła, na przykład element słownika lub wartości statycznej właściwości lub pola, lub z obliczeń jakieś jest.

To zadanie z *XAML — rozszerzenie znaczników*. Niezależnie od nazwy rozszerzeń struktury znaczników XAML są *nie* rozszerzeniem XML. XAML jest zawsze prawne XML.

## <a name="the-code-infrastructure"></a>Infrastruktura kodu

Rozszerzenie znaczników XAML jest klasa, która implementuje [ `IMarkupExtension` ](xref:Xamarin.Forms.Xaml.IMarkupExtension) interfejsu. Taka klasa często zawiera wyraz `Extension` na końcu nazwy ale pojawia się zwykle w XAML bez tego sufiksu.

Następujące rozszerzenia znaczników w XAML są obsługiwane przez wszystkich implementacjach XAML:

- `x:Static` obsługiwane przez [`StaticExtension`](xref:Xamarin.Forms.Xaml.StaticExtension)
- `x:Reference` obsługiwane przez [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension)
- `x:Type` obsługiwane przez [`TypeExtension`](xref:Xamarin.Forms.Xaml.TypeExtension)
- `x:Null` obsługiwane przez [`NullExtension`](xref:Xamarin.Forms.Xaml.NullExtension)
- `x:Array` obsługiwane przez [`ArrayExtension`](xref:Xamarin.Forms.Xaml.ArrayExtension)

Te cztery rozszerzeń struktury znaczników XAML są obsługiwane przez wiele implementacji XAML, łącznie z zestawu narzędzi Xamarin.Forms:

- `StaticResource` obsługiwane przez [`StaticResourceExtension`](xref:Xamarin.Forms.Xaml.StaticResourceExtension)
- `DynamicResource` obsługiwane przez [`DynamicResourceExtension`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension)
- `Binding` obsługiwane przez [ `BindingExtension` ](xref:Xamarin.Forms.Xaml.BindingExtension) &mdash;omówione w [rozdział 16. Powiązanie danych](#chapter16)
- `TemplateBinding` obsługiwane przez [ `TemplateBindingExtension` ](xref:Xamarin.Forms.Xaml.TemplateBindingExtension) &mdash;nieuwzględnione w książce

Dodatkowe rozszerzenia znaczników XAML znajduje się w interfejsie Xamarin.Forms w połączeniu z [ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout):

- [`ConstraintExpression`](xref:Xamarin.Forms.ConstraintExpression)&mdash;nieuwzględnione w książce

## <a name="accessing-static-members"></a>Uzyskiwanie dostępu do statycznych elementów członkowskich

Użyj [ `x:Static` ](xref:Xamarin.Forms.Xaml.StaticExtension) elementu, aby ustawić atrybut na wartość publicznej, statycznej składowej właściwości, pola lub wyliczenia. Ustaw [ `Member` ](xref:Xamarin.Forms.Xaml.StaticExtension.Member) właściwość statyczną składową. Jest zazwyczaj łatwiejsze do określenia `x:Static` i nazwę elementu członkowskiego w nawiasach klamrowych. Nazwa `Member` właściwości nie musi być uwzględniony, po prostu sam element członkowski. To wspólne składnia jest wyświetlana w [ **SharedStatics** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SharedStatics) próbki. Pola statyczne, samodzielnie są zdefiniowane w [ `AppConstants` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter10/SharedStatics/SharedStatics/SharedStatics/AppConstants.cs) klasy. Ta technika pozwala ustanowić stałe używane za pośrednictwem programu.

Za pomocą dodatkowych deklaracji przestrzeni nazw XML, możesz odwoływać się właściwości publiczne statyczne, pól lub elementów członkowskich wyliczenia zdefiniowanych w programie .NET framework, jak pokazano w [ **SystemStatics** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SystemStatics) próbki .

## <a name="resource-dictionaries"></a>Słowniki zasobów

`VisualElement` Klasa definiuje właściwość o nazwie [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) ustawionej dla obiektu typu [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). W ramach XAML, można przechowywać elementów w tym słowniku i zidentyfikować je za pomocą `x:Key` atrybutu. Elementy przechowywane w słowniku zasobów są współużytkowane przez wszystkie odwołania do elementu.

### <a name="staticresource-for-most-purposes"></a>Staticresource — w większości przypadków

W większości przypadków będziesz używać [ `StaticResource` ](xref:Xamarin.Forms.Xaml.StaticResourceExtension) — rozszerzenie znaczników do odwołuje się do elementu ze słownika zasobów, jak pokazano w [ **ResourceSharing** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceSharing) próbki . Możesz użyć `StaticResourceExtension` element lub `StaticResource` w nawiasy klamrowe:

[![Potrójna zrzut ekranu przedstawiający udostępnianie zasobów](images/ch10fg03-small.png "udostępnianie zasobów")](images/ch10fg03-large.png#lightbox "udostępnianie zasobów")

Nie należy mylić `x:Static` — rozszerzenie znaczników i `StaticResource` — rozszerzenie znaczników.

### <a name="a-tree-of-dictionaries"></a>Drzewo słowników

Gdy XAML analizator składni napotka `StaticResource`, rozpocznie się wyszukiwanie w górę drzewa wizualnego dla zgodnego klucza, a następnie przeszukuje `ResourceDictionary` aplikacji w witrynie `App` klasy. Dzięki temu elementów w słowniku zasobów głębiej w drzewie wizualnym, aby zastąpić wyżej w drzewie wizualnym słownika zasobów. Jest to zaprezentowane w [ **ResourceTrees** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceTrees) próbki.

### <a name="dynamicresource-for-special-purposes"></a>Dynamicresource — specjalnego przeznaczenia

`StaticResource` — Rozszerzenie znaczników powoduje, że element do pobrania ze słownika podczas kompilowania drzewa wizualnego podczas `InitializeComponent` wywołania. Alternatywa `StaticResource` jest [ `DynamicResource` ](xref:Xamarin.Forms.Xaml.DynamicResourceExtension), która obsługuje łącze do klucza słownika i aktualizuje element docelowy, gdy element odwołuje się zmian klucza.

Różnica między `StaticResource` i `DynamicResource` została przedstawiona w [ **DynamicVsStatic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) próbki.

Właściwości ustawione przez `DynamicResource` muszą być chronione przez właściwości możliwej do wiązania, zgodnie z opisem w [działu 11 infrastruktura z możliwością wiązania](chapter11.md).

## <a name="lesser-used-markup-extensions"></a>Rozszerzenia znaczników rzadziej używanych

Użyj [ `x:Null` ](xref:Xamarin.Forms.Xaml.NullExtension) — rozszerzenie znaczników do ustawiania właściwości `null`.

Użyj [ `x:Type` ](xref:Xamarin.Forms.Xaml.TypeExtension) — rozszerzenie znaczników do ustawiania właściwości do .NET `Type` obiektu.

Użyj [ `x:Array` ](xref:Xamarin.Forms.Xaml.ArrayExtension) zdefiniować tablicę. Określ typ elementy członkowskie tablicy, ustawiając [`Type`] właściwości `x:Type` — rozszerzenie znaczników.

## <a name="a-custom-markup-extension"></a>Rozszerzenia znaczników niestandardowe

Możesz utworzyć własne rozszerzenia znaczników w XAML, pisząc klasę, która implementuje [ `IMarkupExtension` ](xref:Xamarin.Forms.Xaml.IMarkupExtension) współpracować z usługą [ `ProvideValue` ](xref:Xamarin.Forms.Xaml.IMarkupExtension.ProvideValue(System.IServiceProvider)) metody.

[ `HslColorExtension` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/HslColorExtension.cs) Klasy spełnia te wymagania. Tworzy wartość typu `Color` na podstawie wartości właściwości o nazwie `H`, `S`, `L`, i `A`. Ta klasa jest pierwszy element na nazwę biblioteki rozwiązania Xamarin.Forms [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) , jest tworzone i używane przez tę książkę.

[ **CustomExtensionDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/CustomExtensionDemo) przykład pokazuje, jak odwoływać się do tej biblioteki i za pomocą rozszerzenia niestandardowego kodu znaczników.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 10 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf)
- [Przykłady rozdział 10](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10)
