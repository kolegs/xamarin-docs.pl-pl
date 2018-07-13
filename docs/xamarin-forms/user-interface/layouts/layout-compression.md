---
title: Kompresja układu
description: Kompresja układu usuwa określony układy z drzewa wizualnego w celu zwiększenia wydajności renderowania strony. W tym artykule omówiono sposób umożliwienia kompresja układu oraz potrzebne korzyści może przynieść.
ms.prod: xamarin
ms.assetid: da9e1b26-9d31-4762-94c3-4039f306b7f2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/13/2017
ms.openlocfilehash: ba9be51daa32be1034e2bdfafafe80c45d00d83c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995235"
---
# <a name="layout-compression"></a>Kompresja układu

_Kompresja układu usuwa określony układy z drzewa wizualnego w celu zwiększenia wydajności renderowania strony. W tym artykule omówiono sposób umożliwienia kompresja układu oraz potrzebne korzyści może przynieść._

## <a name="overview"></a>Omówienie

Zestaw narzędzi Xamarin.Forms wykonuje układu za pomocą dwóch serii rekursywne wywołania metody:

- Układ, który rozpoczyna się w górnej części drzewa wizualnego ze stroną, a następnie przechodzi przez wszystkie gałęzie drzewa wizualnego w celu objęcia każdy element wizualny na stronie. Elementy, które są elementy nadrzędne do innych elementów są odpowiedzialne za rozmiary i rozmieszczenia ich elementy podrzędne względem siebie.
- Unieważnieniu polega na za pomocą którego zmiana elementu na stronie wyzwala nowy cykl układu. Elementy są traktowane jako nieprawidłowy, gdy nie będzie ona miała właściwego rozmiaru lub położenia. Każdego elementu w drzewie wizualnym, który ma elementy podrzędne są alerty zawsze wtedy, gdy jeden z jego elementów podrzędnych zmiany rozmiarów. W związku z tym Zmień rozmiar elementu w drzewie wizualnym może spowodować zmiany, które ripple w górę drzewa.

Aby uzyskać więcej informacji na temat jak Xamarin.Forms wykonuje układ zobacz [tworzenie układu niestandardowego](~/xamarin-forms/user-interface/layouts/custom.md).

W wyniku procesu układ jest hierarchię natywne kontrolki. Ta hierarchia zawiera jednak programy renderujące dodatkowe kontenera i otoki dla platform programy renderujące, dodatkowo pompowania Wyświetl hierarchię zagnieżdżania. Głębiej poziom zagnieżdżenia, tym większa ilość pracy Xamarin.Forms musi wykonać, aby wyświetlić stronę. Dla złożonych układów wyświetlanie hierarchii może być zarówno głębokie i szeroka, wiele poziomów zagnieżdżenia.

Na przykład rozważmy poniższy przycisk z przykładowej aplikacji do zalogowania się do usługi Facebook:

![](layout-compression-images/facebook-button.png "Facebook Button")

Ten przycisk jest określony jako kontrolkę niestandardową przy użyciu następującej hierarchii widok XAML:

```xaml
<ContentView ...>
    <StackLayout>
        <StackLayout ...>
            <AbsoluteLayout ...>
                <Button ... />    
                <Image ... />
                <Image ... />
                <BoxView ... />
                <Label ... />
                <Button ... />
            </AbsoluteLayout>
        </StackLayout>
        <Label ... />
    </StackLayout>    
</ContentView>
```

Można zbadać wynikowy hierarchii widoku zagnieżdżonego o [narzędzia Xamarin Inspector](~/tools/inspector/index.md). W systemie Android zagnieżdżone wyświetlanie hierarchii zawiera widoki 17:

![](layout-compression-images/no-compression.png "Wyświetl hierarchię dla przycisku usługi Facebook")

Kompresja układu, która jest dostępna dla aplikacji platformy Xamarin.Forms w systemach iOS i Android platform, ma na celu spłaszczenia widoku zagnieżdżania przez usunięcie określonego układy z drzewa wizualnego, co może poprawić wydajność renderowania strony. Korzyści wydajności, która jest dostarczana różni się w zależności od złożoności strony, wersja używanego systemu operacyjnego i urządzenia, na którym działa aplikacja. Jednakże wydajność będzie widoczna w starszych urządzeń.

> [!NOTE]
> Chociaż ten artykuł koncentruje się na rezultaty zastosowania kompresja układu w systemie Android, jest równie mające zastosowanie do systemu iOS.

## <a name="layout-compression"></a>Kompresja układu

W XAML, można włączyć kompresji układu, ustawiając `CompressedLayout.IsHeadless` dołączonych właściwości `true` w klasie układu:

```xaml
<StackLayout CompressedLayout.IsHeadless="true">
  ...
</StackLayout>   
```

Alternatywnie, można ją włączyć w języku C#, określając wystąpienie układ jako pierwszy argument `CompressedLayout.SetIsHeadless` metody:

```csharp
CompressedLayout.SetIsHeadless(stackLayout, true);
```

> [!IMPORTANT]
> Ponieważ kompresja układu usuwa układu z drzewa wizualnego, nie jest odpowiednia dla układów, które mają wygląd lub który uzyskać wprowadzanie dotykowe. W związku z tym, układy, ustaw [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) właściwości (takie jak [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor), [ `IsVisible` ](xref:Xamarin.Forms.VisualElement.IsVisible), [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation), [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale), [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX) i [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) lub który zaakceptować gestów, nie są kandydatami do układu kompresja. Jednak włączenie kompresji układu dla układu, który ustawia właściwości wyglądu lub akceptujący gestów, nie spowoduje błąd kompilacji lub środowiska uruchomieniowego. Zamiast tego należy kompresja układu, zostaną zastosowane i właściwości wyglądu i rozpoznawania gestów po cichu nie będzie.

Dla przycisku Facebook kompresja układu można włączyć dla klasy trzy układu:

```xaml
<StackLayout CompressedLayout.IsHeadless="true">
    <StackLayout CompressedLayout.IsHeadless="true" ...>
        <AbsoluteLayout CompressedLayout.IsHeadless="true" ...>
            ...
        </AbsoluteLayout>
    </StackLayout>
    ...
</StackLayout>  
```

W systemie Android powoduje to hierarchia widoku zagnieżdżonego 14 widoki:

![](layout-compression-images/layout-compression.png "Wyświetl hierarchię dla przycisku Facebook z kompresja układu")

Zmniejszenie liczby wyświetleń 17% w porównaniu do oryginalnej hierarchii widoku zagnieżdżonego 17 widoków, reprezentuje. Gdy redukcja może pojawić się nieważny redukcji widok za pośrednictwem całej strony może być bardziej znaczące.

### <a name="fast-renderers"></a>Szybkie programy renderujące

Szybkie programy renderujące skrócić inflacji i koszty renderowanie kontrolek zestawu narzędzi Xamarin.Forms w systemie Android spłaszczając wynikowy hierarchii natywnych widoku. Dodatkowo to zwiększa wydajność, tworząc mniej obiektów, co z kolei powoduje w drzewie wizualnym mniej skomplikowany i mniej wykorzystania pamięci. Aby uzyskać więcej informacji na temat szybkie programy renderujące, zobacz [szybkie programy renderujące](~/xamarin-forms/internals/fast-renderers.md).

Dla przycisku usługi Facebook w przykładowej aplikacji łącząc kompresja układu i szybkie programy renderujące generuje hierarchii widoku zagnieżdżonego 8 widoków:

![](layout-compression-images/layout-compression-with-fast-renderers.png "Wyświetl hierarchię dla przycisku Facebook kompresja układu i szybkie programy renderujące")

W porównaniu do oryginalnej hierarchii widoku zagnieżdżonego 17 widoków, reprezentuje zmniejszenie o 52%.

Przykładowa aplikacja zawiera stronę wyodrębnione z rzeczywistych aplikacji. Bez kompresji układ i szybkie programy renderujące strony tworzy hierarchię widoku zagnieżdżonego 130 widoków w systemie Android. Włączenie szybkie programy renderujące i kompresja układu w klasach odpowiedni układ zmniejsza zagnieżdżone wyświetlanie hierarchii do 70 widoków, redukcji 46%.

## <a name="summary"></a>Podsumowanie

Kompresja układu usuwa określony układy z drzewa wizualnego w celu zwiększenia wydajności renderowania strony. Korzyści wydajności, które ten dostarcza różni się w zależności od złożoności strony, wersja używanego systemu operacyjnego i urządzenia, na którym działa aplikacja. Jednakże wydajność będzie widoczna w starszych urządzeń.


## <a name="related-links"></a>Linki pokrewne

- [Tworzenie niestandardowego układu](~/xamarin-forms/user-interface/layouts/custom.md)
- [Szybkie programy renderujące](~/xamarin-forms/internals/fast-renderers.md)
- [LayoutCompression (przykład)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/layoutcompression/)
