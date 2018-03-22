---
title: Reprezentacje w skoroszytach Xamarin
ms.topic: article
ms.prod: xamarin
ms.assetid: 5C7A60E3-1427-47C9-A022-720F25ECB031
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: a311bace159a450dc27e15baa8ef1c260a90c36e
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="representations-in-xamarin-workbooks"></a>Reprezentacje w skoroszytach Xamarin

## <a name="representations"></a>oświadczenia

W ramach sesji skoroszytu lub Inspektora kodu, który jest wykonywany i zwraca wynik (np. Metoda zwraca wartość lub wynik wyrażenia) są przetwarzane przez potok reprezentacja w agencie. Wszystkie obiekty, z wyjątkiem elementów podstawowych, takich jak liczby całkowite, zostaną odzwierciedlone wygenerowało wykresy interakcyjne elementu członkowskiego i przeszli przez proces, aby zapewnić alternatywne oświadczenia, które klient może renderować więcej Bogato. Obiekty o dowolnej wielkości i głębokość bezpiecznie są obsługiwane (w tym i elementy wyliczalne nieskończone cykle) z powodu odbicia opóźnieniem i interaktywne i komunikacji zdalnej.

Skoroszyty Xamarin udostępnia kilka typów wspólnych dla wszystkich agentów i klientów, umożliwiające renderowanie sformatowanego wyników. [`Color`][xir-color] jest przykładem takiego typu, w której na przykład w systemie iOS agenta jest odpowiedzialny za konwertowanie `CGColor` lub `UIColor` obiektów do `Xamarin.Interactive.Representations.Color` obiektu.

Oprócz typowych reprezentacje integracji zestawu SDK udostępnia interfejsy API do szeregowania niestandardowego reprezentacje w agencie i renderowania reprezentacje w kliencie.

## <a name="external-representations"></a>Reprezentacje zewnętrznych

[`Xamarin.Interactive.IAgent.RepresentationManager`][repman] zapewnia możliwość zarejestrowania [ `RepresentationProvider` ] [ repp], który integracji musi implementować konwersji z dowolnych obiektów o niesprecyzowanym formularza do renderowania. Formularze o niesprecyzowanym musi implementować [ `ISerializableObject` ] [ serobj] interfejsu.

Implementowanie `ISerializableObject` interfejsu dodaje metodę serializacja dokładnie kontrolki, w jaki sposób obiekty są serializowane. `Serialize` Metoda oczekuje dewelopera dokładnie będzie określać właściwości, które mają być serializowane i jakie będą ostatecznej nazwy. Spojrzenie na `Person` obiektu w naszym [`KitchenSink` próbki] [przykład], zobaczysz, jak to działa:

```csharp
public sealed class Person : ISerializableObject
{
    public string Name { get; }

    // Rest of the code is omitted…

    void ISerializableObject.Serialize (ObjectSerializer serializer)
        => serializer.Property (nameof (Name), Name);
}
```

Jeśli trzeba podać nadzbiorem lub podzbiór właściwości z obiektu oryginalnego, firma Microsoft może to zrobić z `Serialize`. Na przykład może wyglądać mniej więcej tak to, aby wstępnie obliczonych jak `Age` właściwość `Person`:

```csharp
public sealed class Person : ISerializableObject
{
  public string Name { get; set; }
  public DateTime DateOfBirth { get; set; }

  // <snip>

  void ISerializableObject.Serialize (ObjectSerializer serializer)
  {
    serializer.Property (nameof (Name), Name);
    serializer.Property (nameof (DateOfBirth), DateOfBirth);

    // Let's pre-compute an Age property that's the person's age in years,
    // so we don't have to compute it in the renderer.
    var age = (DateTime.MinValue + (DateTime.Now - DateOfBirth)).Year - 1;
    serializer.Property ("Age", age)
  }
}
```

> [!NOTE]
> Interfejsy API, który tworzy `ISerializableObject` obiektów bezpośrednio nie muszą być obsługiwane przez `RepresentationProvider`. Jeśli obiekt ma być wyświetlany jest **nie** `ISerializableObject`, można obsłużyć zawijania go w sieci `RepresentationProvider`.

### <a name="rendering-a-representation"></a>Renderowanie reprezentację

Moduły renderowania są implementowane w języku JavaScript i będzie mieć dostęp do wersji JavaScript obiektu reprezentowanego przez `ISerializableObject`. Kopiuj JavaScript będą także mieć `$type` ciągu właściwość, która wskazuje nazwę typu .NET.

Zalecamy używanie języka TypeScript dla klienta integracji kod, który kompiluje oczywiście do waniliowe JavaScript. W obu przypadkach SDK udostępnia [typów wyeksportowanych] [ typings] które bezpośrednio odwołuje maszynie lub po prostu określonych ręcznie zapisuje waniliowe JavaScript jest preferowana.

Punkt integracji głównego celu renderowania jest `xamarin.interactive.RendererRegistry`:

```js
xamarin.interactive.RendererRegistry.registerRenderer(
  function (source) {
    if (source.$type === "SampleExternalIntegration.Person")
      return new PersonRenderer;
    return undefined;
  }
);
```

W tym miejscu `PersonRenderer` implementuje `Renderer` interfejsu. Zobacz [typów wyeksportowanych] [ typings] więcej szczegółów.

[typings]: https://github.com/xamarin/Workbooks/blob/master/SDK/typings/xamarin-interactive.d.ts
[xir-color]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.Color/
[repman]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.IRepresentationManager/
[repp]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.RepresentationProvider/
[serobj]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Serialization.ISerializableObject/
