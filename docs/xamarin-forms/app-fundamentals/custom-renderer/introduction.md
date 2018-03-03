---
title: Wprowadzenie do renderowania niestandardowych
description: "Niestandardowe moduły renderowania Podaj podejście zaawansowane dostosowywanie wyglądu i zachowania formantów platformy Xamarin.Forms. Służy do zmiany małych stylów lub Zaawansowane układu specyficzne dla platformy i dostosowywania zachowania. Ten artykuł zawiera wprowadzenie do renderowania niestandardowych i opisano proces tworzenia niestandardowego modułu renderowania."
ms.topic: article
ms.prod: xamarin
ms.assetid: 264314BE-1C5C-4727-A14E-F6F98151CDBD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/19/2016
ms.openlocfilehash: a9908429994f4575a9e41936d500bfd8906a843b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-custom-renderers"></a>Wprowadzenie do renderowania niestandardowych

_Niestandardowe moduły renderowania Podaj podejście zaawansowane dostosowywanie wyglądu i zachowania formantów platformy Xamarin.Forms. Służy do zmiany małych stylów lub Zaawansowane układu specyficzne dla platformy i dostosowywania zachowania. Ten artykuł zawiera wprowadzenie do renderowania niestandardowych i opisano proces tworzenia niestandardowego modułu renderowania._

Platformy Xamarin.Forms [stron, układów i kontrolek](~/xamarin-forms/user-interface/controls/index.md) przedstawia wspólnego interfejsu API do opisywania interfejsów i platform przenośnych użytkownika. Każdej strony układu i kontroli jest inaczej renderowane na każdej platformie, przy użyciu `Renderer` klasy, która z kolei tworzy macierzystego formantu (odpowiadającej reprezentacja platformy Xamarin.Forms), rozmieszcza ją na ekranie i dodaje zachowanie określone w udostępniony kod.

Deweloperzy mogą implementować własne niestandardowe `Renderer` klas, aby dostosować wygląd i/lub zachowanie formantu. Niestandardowe moduły renderowania dla danego typu można dodać do projektu jedną aplikację, aby dostosować kontroli w jednym miejscu, umożliwiając domyślne zachowanie na różnych platformach; lub różnych niestandardowe moduły renderowania można dodać do każdego projektu aplikacji, aby utworzyć inną wyglądu i działania na systemu iOS, Android i Windows Phone. Jednak implementacja klasy niestandardowego modułu renderowania do wykonania dostosowywania prostego formantu jest często odpowiedzi ciężki. Efekty upraszcza ten proces i są zwykle używane do zmian małych style. Aby uzyskać więcej informacji, zobacz [efekty](~/xamarin-forms/app-fundamentals/effects/index.md).

## <a name="examining-why-custom-renderers-are-necessary"></a>Konieczne są orzeczenie Dlaczego niestandardowe moduły renderowania

Zmienianie wyglądu formantu platformy Xamarin.Forms, bez korzystania z niestandardowego modułu renderowania jest procesem dwuetapowym, która obejmuje tworzenie niestandardowego formantu za pośrednictwem tworzenie podklas, a następnie realizowaniu kontrolki niestandardowej zamiast oryginalnego formantu. Poniższy przykładowy kod przedstawia przykład podklasy `Entry` sterowania:

```csharp
public class MyEntry : Entry
{
  public MyEntry ()
  {
    BackgroundColor = Color.Gray;
  }
}
```

`MyEntry` Formant jest `Entry` kontrolują lokalizację `BackgroundColor` ustawiono szary i może być przywoływany w Xaml deklarowanie przestrzeni nazw dla lokalizacji, a następnie użyć prefiksu przestrzeni nazw w elemencie formantu. Poniższy kod przedstawia przykład sposobu `MyEntry` kontrolki niestandardowej, może być zużyte przez `ContentPage`:

```xaml
<ContentPage
    ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <local:MyEntry Text="In Shared Code" />
    ...
</ContentPage>
```

`local` Prefiks przestrzeni nazw może być dowolna. Jednak `namespace` i `assembly` wartości muszą być zgodne szczegóły kontrolki niestandardowej. Po zadeklarowaniu obszaru nazw prefiks jest używany do odwołania kontrolki niestandardowej.

> [!NOTE]
> **Uwaga**: definiowanie `xmlns` jest znacznie prostsza w PCLs niż udostępnionych projektów. PCL jest kompilowany do zestawu, dzięki czemu łatwiej ustalić, co `assembly=CustomRenderer` wartość powinna być. Podczas korzystania z udostępnionych projektów, współużytkowane zasoby (w tym XAML) są kompilowane do każdego z odwołaniem do projektów, które oznacza, że jeśli dla systemu iOS, Android i Windows Phone projekty mają swoje własne *nazwy zestawu* nie jest możliwe Aby zapisać `xmlns` deklaracji, ponieważ wartość musi być inny dla poszczególnych aplikacji. Formanty niestandardowe w języku XAML udostępniony projektów będzie wymagać co projekt aplikacji do skonfigurowania z taką samą nazwę.

`MyEntry` Kontrolki niestandardowej następnie jest renderowany na każdej platformie, na tle szare, jak pokazano na poniższych zrzutach ekranu:

![](introduction-images/screenshots.png "Formant niestandardowy MyEntry na każdej platformie")

Zmiana koloru tła formantu na każdej platformie zostały wykonane wyłącznie przez tworzenie podklasy formantu. Ta technika jest jednak ograniczona co można uzyskać, ponieważ nie jest możliwe skorzystać z ulepszeń specyficzne dla platformy i dostosowania. Gdy są one wymagane, musi być implementowana niestandardowe moduły renderowania.

## <a name="creating-a-custom-renderer-class"></a>Tworzenie klasy niestandardowego modułu renderowania

Proces tworzenia klasy niestandardowego modułu renderowania wygląda następująco:

1. Utwórz podklasą klasy renderowania, który renderuje macierzystego formantu.
1. Zastąp metodę, która renderuje macierzystego formantu i pisanie logiki, aby dostosować formantu. Często `OnElementChanged` metody jest używany do renderowania kontrolki na natywny, która jest wywoływana po utworzeniu odpowiedniego formantu platformy Xamarin.Forms.
1. Dodaj `ExportRenderer` atrybutu klasy niestandardowego modułu renderowania, aby określić, że będą używane do renderowania kontrolki na platformy Xamarin.Forms. Ten atrybut służy do rejestrowania niestandardowego modułu renderowania z platformy Xamarin.Forms.

> [!NOTE]
> **Uwaga**: w przypadku większości elementów platformy Xamarin.Forms jest opcjonalne zapewnić niestandardowego modułu renderowania w każdym projekcie platformy. Jeśli nie jest zarejestrowany niestandardowego modułu renderowania, domyślne renderowanie dla klasy podstawowej formantu będzie używany. Jednak niestandardowe moduły renderowania są wymagane w każdym projekcie platformy podczas renderowania [widoku](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) lub [ViewCell](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) elementu.

Można znaleźć w tematach w tej serii pokazów i wyjaśnienia ten proces dla różnych elementów platformy Xamarin.Forms.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Jeśli formant niestandardowy znajduje się w projekcie PCL dodawanej do rozwiązania (tj. nie PCL utworzony przez program Visual Studio dla aplikacji platformy Xamarin.Forms Mac/Visual Studio szablon projektu), może wystąpić wyjątek w systemie iOS podczas próby dostępu kontrolki niestandardowej. Jeśli wystąpi ten problem można rozwiązać przez utworzenie odwołania do formantu niestandardowego z `AppDelegate` klasy:

```csharp
var temp = new ClassInPCL(); // in AppDelegate, but temp not used anywhere
```

To zmusza kompilator, aby rozpoznać `ClassInPCL` typu rozwiązując go. Alternatywnie `Preserve` atrybutu można dodać do `AppDelegate` klasy uzyskanie takiego samego wyniku:

```csharp
[assembly: Preserve (typeof (ClassInPCL))]
```

Spowoduje to utworzenie odwołania do `ClassInPCL` typu i wskazujący, że są wymagane w czasie wykonywania. Aby uzyskać więcej informacji, zobacz [zachowania kodu](~/ios/deploy-test/linker.md).

## <a name="summary"></a>Podsumowanie

W tym artykule udostępnił wprowadzenie do renderowania niestandardowych i ma określone proces tworzenia niestandardowego modułu renderowania. Niestandardowe moduły renderowania Podaj podejście zaawansowane dostosowywanie wyglądu i zachowania formantów platformy Xamarin.Forms. Służy do zmiany małych stylów lub Zaawansowane układu specyficzne dla platformy i dostosowywania zachowania.


## <a name="related-links"></a>Linki pokrewne

- [Efekty](~/xamarin-forms/app-fundamentals/effects/index.md)
