---
title: Wprowadzenie do niestandardowe programy renderujące
description: Ten artykuł zawiera wprowadzenie do niestandardowe programy renderujące oraz opisano proces tworzenia niestandardowego modułu renderowania.
ms.prod: xamarin
ms.assetid: 264314BE-1C5C-4727-A14E-F6F98151CDBD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/19/2016
ms.openlocfilehash: 180a196e0b95854815c8a74ef1d2df63407dd04f
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998007"
---
# <a name="introduction-to-custom-renderers"></a>Wprowadzenie do niestandardowe programy renderujące

_Niestandardowe programy renderujące zapewniają zaawansowanych sposobów dostosowywania wygląd i zachowanie kontrolki zestawu narzędzi Xamarin.Forms. One może służyć do zmiany stylów małych lub zaawansowanych układ specyficzne dla platformy i dostosowywania zachowania. Ten artykuł zawiera wprowadzenie do niestandardowe programy renderujące oraz opisano proces tworzenia niestandardowego modułu renderowania._

Zestaw narzędzi Xamarin.Forms [stron, układy i kontrolek](~/xamarin-forms/user-interface/controls/index.md) istnieje wspólny interfejs API do opisywania interfejsów użytkownika mobilnego dla wielu platform. Każdej strony układu i kontroli jest inaczej renderowane na każdej platformie przy użyciu `Renderer` klasę, która z kolei tworzy formant natywnego (odpowiadających na reprezentację w postaci zestawu narzędzi Xamarin.Forms) rozmieszcza go na ekranie i dodaje określone w zachowanie udostępniony kod.

Deweloperzy mogą implementować własne niestandardowe `Renderer` klasy, aby dostosować wygląd lub zachowanie kontrolki. Niestandardowe programy renderujące dla danego typu mogą być dodawane do projektu jedną aplikację, aby dostosować formant w jednym miejscu, a jednocześnie zachowanie domyślne na innych platformach; lub różnych niestandardowe programy renderujące, można dodać do każdego projektu aplikacji, aby utworzyć inny wygląd i działanie w systemach iOS, Android i platformy uniwersalnej Windows (UWP). Jednak implementacja klasy niestandardowego modułu renderowania do wykonania dostosowywania prosty formant jest często odpowiedzi ciężki. Efekty upraszcza ten proces i są zwykle używane do zmiany stylów małe. Aby uzyskać więcej informacji, zobacz [efekty](~/xamarin-forms/app-fundamentals/effects/index.md).

## <a name="examining-why-custom-renderers-are-necessary"></a>Orzeczenie Dlaczego niestandardowe programy renderujące są wymagane

Zmienianie wyglądu kontrolki zestawu narzędzi Xamarin.Forms, bez korzystania z niestandardowego modułu renderowania jest procesem dwuetapowym, która obejmuje utworzenie niestandardowej kontrolki przez tworzenie podklasy, a następnie wykorzystywania formant niestandardowy, zamiast oryginalnego formantu. Poniższy przykład kodu pokazuje przykład podklasy `Entry` sterowania:

```csharp
public class MyEntry : Entry
{
  public MyEntry ()
  {
    BackgroundColor = Color.Gray;
  }
}
```

`MyEntry` Formant jest `Entry` kontrolują lokalizację `BackgroundColor` jest ustawiona na szary, a może odwoływać się w języku Xaml deklarowanie przestrzeni nazw dla lokalizacji i przy użyciu prefiksu przestrzeni nazw w elemencie kontrolki. Poniższy kod przedstawia przykładowy sposób, w jaki `MyEntry` kontrolki niestandardowej mogą być używane przez `ContentPage`:

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

`local` Prefiks przestrzeni nazw może być dowolna. Jednak `namespace` i `assembly` wartości muszą być zgodne szczegóły kontrolki niestandardowej. Po zadeklarowaniu przestrzeń nazw prefiks jest używany do odwołania kontrolki niestandardowej.

> [!NOTE]
> Definiowanie `xmlns` jest znacznie prostsze w projektach .NET Standard biblioteki niż projekty udostępnione. Biblioteki .NET Standard jest skompilowany w zestawie, dzięki czemu można łatwo określić, co `assembly=CustomRenderer` . wartość powinna być. Korzystając z udostępnionych projektów, wszystkie udostępnione zasoby (w tym XAML) są kompilowane do wszystkich projektów odwołujący się, co oznacza że jeśli dla systemu iOS, Android i platformy uniwersalnej systemu Windows projekty mają swoje własne *nazw zestawów* nie jest możliwe do zapisu `xmlns` deklaracji, ponieważ wartość musi być różne dla każdej aplikacji. Kontrolki niestandardowe w XAML dla udostępnionych projektów będzie wymagać co projekt aplikacji należy skonfigurować z taką samą nazwą zestawu.

`MyEntry` Formantu niestandardowego jest następnie renderowany na każdej platformie, na szarym tle, jak pokazano na poniższych zrzutach ekranu:

![](introduction-images/screenshots.png "Kontrolka niestandardowa MyEntry na każdej platformie")

Zmiana koloru tła kontrolki na każdej platformie zostało zrobione wyłącznie poprzez tworzenie podklasy kontrolki. Ta technika jest ograniczony, co można uzyskać, ponieważ nie jest możliwe korzystanie z zalet dostosowań i rozszerzeń specyficznych dla platformy. Gdy są one wymagane, niestandardowe programy renderujące musi zostać wdrożone.

## <a name="creating-a-custom-renderer-class"></a>Tworzenie klasy niestandardowego modułu renderowania

Proces tworzenia klasy niestandardowego modułu renderowania jest następująca:

1. Utwórz podklasę klasy modułu renderowania, która renderuje kontrolkę natywnych.
1. Zastąp metodę, która renderuje kontrolkę natywnych i pisanie logiki, aby dostosować formant. Często `OnElementChanged` metoda jest używana do renderowania kontrolki natywne, które jest wywoływane, gdy zostanie utworzony odpowiedni formant zestawu narzędzi Xamarin.Forms.
1. Dodaj `ExportRenderer` atrybutów do klasy niestandardowego modułu renderowania, aby określić, że będą używane do renderowania kontrolki zestawu narzędzi Xamarin.Forms. Ten atrybut służy do rejestrowania niestandardowego modułu renderowania przy użyciu zestawu narzędzi Xamarin.Forms.

> [!NOTE]
> W przypadku większości elementów zestawu narzędzi Xamarin.Forms jest opcjonalny w celu zapewnienia niestandardowego modułu renderowania w każdym projekcie platformy. Jeśli nie jest zarejestrowany niestandardowego modułu renderowania, domyślne renderowanie dla klasy podstawowej kontrolki będą używane. Jednakże, niestandardowe programy renderujące są wymagane w każdym projekcie platformy podczas renderowania [widoku](xref:Xamarin.Forms.View) lub [ViewCell](xref:Xamarin.Forms.ViewCell) elementu.

Tematy w tej serii zapewni pokazów i wyjaśnienia ten proces dla różnych elementów zestawu narzędzi Xamarin.Forms.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Jeśli formant niestandardowy znajduje się w projekt biblioteki .NET Standard, który został dodany do rozwiązania (czyli nie biblioteki .NET Standard utworzone przez program Visual Studio dla szablonu projektu aplikacji platformy Xamarin.Forms systemu Mac/Visual Studio), wyjątek może wystąpić w systemie iOS podczas Podjęto próbę dostępu do formantu niestandardowego. Jeśli ten problem występuje, może zostać rozpoznana przez utworzenie odwołania do formantu niestandardowego z `AppDelegate` klasy:

```csharp
var temp = new ClassInPCL(); // in AppDelegate, but temp not used anywhere
```

Zmusza to kompilatora, rozpoznawał `ClassInPCL` typu przez rozwiązania. Alternatywnie `Preserve` atrybutu można dodać do `AppDelegate` klasy, aby osiągnąć ten sam wynik:

```csharp
[assembly: Preserve (typeof (ClassInPCL))]
```

Spowoduje to utworzenie odwołania do `ClassInPCL` typu, wskazujący, że jest to wymagane w czasie wykonywania. Aby uzyskać więcej informacji, zobacz [kodu przy zachowaniu](~/ios/deploy-test/linker.md).

## <a name="summary"></a>Podsumowanie

Ten artykuł ma pod warunkiem wprowadzenie do niestandardowe programy renderujące i ma opisano proces tworzenia niestandardowego modułu renderowania. Niestandardowe programy renderujące zapewniają zaawansowanych sposobów dostosowywania wygląd i zachowanie kontrolki zestawu narzędzi Xamarin.Forms. One może służyć do zmiany stylów małych lub zaawansowanych układ specyficzne dla platformy i dostosowywania zachowania.


## <a name="related-links"></a>Linki pokrewne

- [Efekty](~/xamarin-forms/app-fundamentals/effects/index.md)
