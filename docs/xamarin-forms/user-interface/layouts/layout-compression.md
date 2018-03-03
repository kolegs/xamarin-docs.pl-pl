---
title: "Kompresja układu"
description: "Kompresja układu usuwa określony układów z drzewa wizualnego w celu zwiększenia wydajności renderowania strony. W tym artykule wyjaśniono, jak włączyć kompresję układ i korzyści, które można przełączyć go."
ms.topic: article
ms.prod: xamarin
ms.assetid: da9e1b26-9d31-4762-94c3-4039f306b7f2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/13/2017
ms.openlocfilehash: 15f198465c544989b347fe534978956741478ed2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="layout-compression"></a>Kompresja układu

_Kompresja układu usuwa określony układów z drzewa wizualnego w celu zwiększenia wydajności renderowania strony. W tym artykule wyjaśniono, jak włączyć kompresję układ i korzyści, które można przełączyć go._

## <a name="overview"></a>Omówienie

Platformy Xamarin.Forms wykonuje układu za pomocą dwóch serii cykliczne wywołania metody:

- Układ rozpoczyna się w górnej części drzewa wizualnego ze stroną, a obejmującego wszystkie gałęzie drzewa wizualnego na każdy element wizualny na stronie. Elementy nadrzędne z innymi elementami są zobowiązani do zmiany rozmiaru i pozycjonowania ich elementy podrzędne względem siebie.
- Unieważnienie to proces, za pomocą której zmiana elementu na stronie wyzwala nowy cykl układu. Elementy są uznawane za nieprawidłowe, gdy mają one już prawidłowe rozmiaru lub położenia. Każdego elementu w drzewie wizualnym elementów podrzędnych jest alert przy każdej zmianie jednego z jego elementów podrzędnych rozmiary. W związku z tym zmianę w rozmiarze elementu w drzewie wizualnym może spowodować zmiany, które ripple w górę drzewa.

Aby uzyskać więcej informacji o sposobie platformy Xamarin.Forms wykonuje układu, zobacz [tworzenie układu niestandardowego](~/xamarin-forms/user-interface/layouts/custom.md).

W wyniku procesu układ jest hierarchii kontrolki natywne. Jednak tej hierarchii zawiera dodatkowe kontenera renderowania i otoki dla platformy moduły renderowania, dalsze pompowania hierarchii widoku zagnieżdżania. Lepszy poziom zagnieżdżenia, tym większa ilość pracy platformy Xamarin.Forms musi wykonać, aby wyświetlić stronę. W złożonych układów widoku hierarchii może być zarówno bezpośrednich i szeroka, zawierające wiele poziomów zagnieżdżenia.

Rozważmy na przykład poniższy przycisk z przykładowej aplikacji do logowania do usługi Facebook:

![](layout-compression-images/facebook-button.png "Facebook Button")

Ten przycisk jest określony jako kontrolkę niestandardową przy użyciu następujących hierarchii widoku XAML:

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

Wynikowy hierarchia zagnieżdżonych widoku można zbadać z [inspektora Xamarin](~/tools/inspector/index.md). W systemie Android hierarchia zagnieżdżonych widok zawiera widoki 17:

![](layout-compression-images/no-compression.png "Wyświetlanie hierarchii dla przycisku Facebook")

Układ kompresji, która jest dostępna dla aplikacji platformy Xamarin.Forms w systemach iOS i Android platform, ma na celu spłaszczanie zagnieżdżania przez usunięcie określonego układów z drzewa wizualnego, co może poprawić wydajność renderowania stron widoku. Korzyści wydajności, która jest dostarczana różni się w zależności od złożoności strony, wersja używanego systemu operacyjnego i urządzenia, na którym jest uruchomiona aplikacja. Jednakże wydajność będzie widoczny na starszych urządzeń.

> [!NOTE]
> **Uwaga**: gdy ten artykuł skupia się na wynikach stosowania układu kompresji w systemie Android, jest równie dotyczą systemu iOS.

## <a name="layout-compression"></a>Kompresja układu

W języku XAML, można włączyć kompresji układu przez ustawienie `CompressedLayout.IsHeadless` dołączona właściwość do `true` w klasie układu:

```xaml
<StackLayout CompressedLayout.IsHeadless="true">
  ...
</StackLayout>   
```

Alternatywnie, można ją włączyć w języku C#, określając wystąpienia układu jako pierwszy argument `CompressedLayout.SetIsHeadless` metody:

```csharp
CompressedLayout.SetIsHeadless(stackLayout, true);
```

> [!IMPORTANT]
> Ponieważ kompresja układu usuwa układ z drzewa wizualnego, nie jest odpowiedni dla układów wygląd, która ma lub który uzyskać wprowadzania dotykowego. W związku z tym układów który ustawić [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) właściwości (takie jak [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/), [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/), [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/), [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/), [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) i [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/)) lub który zaakceptuje gesty, nie są kandydatami do układu kompresji. Jednak włączenie kompresji układu dla układu, który ustawia właściwości wygląd lub która akceptuje gesty, nie spowoduje to błąd kompilacji lub środowisko uruchomieniowe. Zamiast tego układu kompresji zostaną zastosowane i właściwości wygląd i rozpoznawania gestów dyskretnie nie powiedzie się.

Dla przycisku Facebook kompresji układu można włączyć dla układu trzech klas:

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

W systemie Android powoduje to hierarchia zagnieżdżonych widoku widoków 14:

![](layout-compression-images/layout-compression.png "Wyświetlanie hierarchii dla przycisku Facebook z kompresją układu")

W porównaniu do pierwotna hierarchia zagnieżdżonych widoku 17 widoków, ta pozycja reprezentuje zmniejszenie liczby widoków 17%. Gdy to zmniejszenie może pojawić się nieważny ograniczenia widoku w całej strony może mieć bardziej znaczących.

### <a name="fast-renderers"></a>Szybkie renderowania

Szybkie renderowania zmniejszyć inflacji i kosztów renderowanie kontrolek platformy Xamarin.Forms w systemie Android przez spłaszczanie wynikowy hierarchii natywnego widoku. Dalsze to zwiększa wydajność tworzenia mniejszą liczbę obiektów, co z kolei powoduje w drzewie wizualnym mniej złożona i mniej wykorzystania pamięci. Aby uzyskać więcej informacji na temat szybkiego renderowania, zobacz [renderowania szybkiego](~/xamarin-forms/internals/fast-renderers.md).

Dla przycisku Facebook w przykładowej aplikacji połączenie układu kompresji i szybkie renderowania tworzy hierarchii zagnieżdżone wyświetlanie widoków 8:

![](layout-compression-images/layout-compression-with-fast-renderers.png "Wyświetlanie hierarchii dla przycisku Facebook z układu kompresji i szybkie renderowania")

W porównaniu do pierwotna hierarchia zagnieżdżonych widoku 17 widoków, ta pozycja reprezentuje zmniejszenie % 52.

Przykładowa aplikacja zawiera stronę wyodrębniony z rzeczywistej aplikacji. Bez układu kompresji i szybkie renderowania strony tworzy hierarchia zagnieżdżonych widoku 130 widoków w systemie Android. Włączenie renderowania szybkiego i kompresji układu dla klasy odpowiedni układ ogranicza zagnieżdżone wyświetlanie hierarchii do 70 widoków, zmniejszenie 46%.

## <a name="summary"></a>Podsumowanie

Kompresja układu usuwa określony układów z drzewa wizualnego w celu zwiększenia wydajności renderowania strony. Korzyści wydajności, który zapewnia to różni się w zależności od złożoności strony, wersja używanego systemu operacyjnego i urządzenia, na którym jest uruchomiona aplikacja. Jednakże wydajność będzie widoczny na starszych urządzeń.


## <a name="related-links"></a>Linki pokrewne

- [Tworzenie niestandardowego układu](~/xamarin-forms/user-interface/layouts/custom.md)
- [Szybkie programy renderujące](~/xamarin-forms/internals/fast-renderers.md)
- [LayoutCompression (przykład)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/layoutcompression/)
