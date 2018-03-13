---
title: Wprowadzenie do DependencyService
description: "Dowiedz się, jak działa DependencyService dostępu do natywnego platformy funkcji"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5d019604-4f6f-4932-9b26-1fce3b4d88f8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
ms.openlocfilehash: 74b22f31fabf70885eca732ef021232124df71bb
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="introduction-to-dependencyservice"></a>Wprowadzenie do DependencyService

## <a name="overview"></a>Omówienie

[`DependencyService`](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) Umożliwia aplikacjom wywołują funkcje specyficzne dla platform z udostępnionym kodu. Ta funkcja umożliwia aplikacji platformy Xamarin.Forms wykonywania żadnych czynności, które można wykonać aplikacji natywnej.

`DependencyService` jest rozpoznawania zależności. W praktyce zdefiniowano interfejsu i `DependencyService` znajdzie implementacją interfejsu z różnych projektów platformy.

## <a name="how-dependencyservice-works"></a>Jak działa DependencyService

Aplikacje platformy Xamarin.Forms muszą trzy składniki używane `DependencyService`:

- **Interfejs** &ndash; wymaganej funkcjonalności jest definiowana za pomocą interfejsu w kodzie udostępnionego.
- **Implementacja na platformie** &ndash; klas implementujących interfejs musi zostać dodany do każdego projektu platformy.
- **Rejestracja** &ndash; każdej implementującej klasy musi być zarejestrowana w `DependencyService` za pomocą atrybutu metadanych. Rejestracja umożliwia `DependencyService` można znaleźć implementującej klasy i dostarczenia go zamiast interfejsu w czasie wykonywania.
- **Wywołanie DependencyService** &ndash; udostępniony kod musi jawnie wywołać `DependencyService` poprosić o implementacji interfejsu.

Należy pamiętać, że implementacje należy podać dla każdego projektu platformy w rozwiązaniu. Projekty platformy bez implementacji zakończy się niepowodzeniem w czasie wykonywania.

Poniższy diagram tłumaczy strukturę aplikacji:

![](introduction-images/overview-diagram.png "Struktura aplikacji DependencyService")

### <a name="interface"></a>Interface

Interfejs, który projektowania definiują sposób interakcji z funkcji specyficznych dla platformy. Należy zachować ostrożność, jeśli tworzysz składnik do udostępnienia jako składnik lub pakietu Nuget. Projekt interfejsu API można lub przerwanie pakietu. W poniższym przykładzie określa prosty interfejs mówiąc tekst, który umożliwia elastyczność określania wyrazy wymawiane, jednak nie pozostawia wykonania można dostosować dla każdej platformy:

```csharp
public interface ITextToSpeech {
    void Speak ( string text ); //note that interface members are public by default
}
```

### <a name="implementation-per-platform"></a>Wdrożenia dla każdej platformy

Gdy odpowiedni interfejs został zaprojektowany, ten interfejs musi być implementowana w projekcie dla każdej platformy, która ma być przeznaczona dla. Na przykład następujące klasy wdrożenie `ITextToSpeech` interfejs Windows Phone:

```csharp
namespace TextToSpeech.WinPhone
{
  public class TextToSpeechImplementation : ITextToSpeech
  {
      public TextToSpeechImplementation() {}

      public async void Speak(string text)
      {
          SpeechSynthesizer synth = new SpeechSynthesizer();
          await synth.SpeakTextAsync(text);
      }
  }
}
```

Należy pamiętać, co implementacji musi mieć konstruktora domyślnego (bezparametrowego) aby `DependencyService` aby można było utworzyć. Nie można definiować konstruktorów bez parametrów przez interfejs.

### <a name="registration"></a>Rejestracja

Każda implementacja interfejsu musi być zarejestrowany w usłudze `DependencyService` z atrybutu metadanych. Poniższy kod rejestruje implementację dla Windows Phone:

```csharp
using TextToSpeech.WinPhone;

[assembly: Xamarin.Forms.Dependency (typeof (TextToSpeechImplementation))]
namespace TextToSpeech.WinPhone {
  ...
```

Wprowadzanie wszystkich elementów, implementacja specyficzna dla platformy wygląda następująco:

```csharp
[assembly: Xamarin.Forms.Dependency (typeof (TextToSpeechImplementation))]
namespace TextToSpeech.WinPhone {
  public class TextToSpeechImplementation : ITextToSpeech
  {
      public TextToSpeechImplementation() {}

      public async void Speak(string text)
      {
          SpeechSynthesizer synth = new SpeechSynthesizer();
          await synth.SpeakTextAsync(text);
      }
  }
}
```

Uwaga: rejestracji wykonywane na poziomie przestrzeni nazw, a nie poziomie klasy.

#### <a name="universal-windows-platform-net-native-compilation"></a>Natywnej kompilacji .NET platformy uniwersalnej systemu Windows

Projekty platformy uniwersalnej systemu Windows, które należy użyć opcji kompilacji platformy .NET Native należy stosować [nieco inne konfiguracji](~/xamarin-forms/platform/windows/installation/universal.md#target-invocation-exception) podczas inicjowania platformy Xamarin.Forms. Kompilacja platformy .NET native wymaga także nieco inne rejestracji zależności usług.

W **App.xaml.cs** plików, należy ręcznie zarejestrować każda usługa zależności zdefiniowane w projekcie platformy uniwersalnej systemu Windows przy użyciu `Register<T>` metody, jak pokazano poniżej:

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// register the dependencies in the same
Xamarin.Forms.DependencyService.Register<TextToSpeechImplementation>();
```

Uwaga: ręcznej rejestracji przy użyciu `Register<T>` jest efektywne tylko w wersji kompilacje przy użyciu kompilacji platformy .NET Native. W przypadku pominięcia tego wiersza, kompilacji do debugowania będą nadal działać, ale kompilacje wersji zakończy się niepowodzeniem można załadować zależności usługi.

### <a name="call-to-dependencyservice"></a>Wywołanie DependencyService

Kiedy projekt nie został skonfigurowany z implementacji dla każdej platformy i wspólny interfejs, za pomocą `DependencyService` uzyskać prawo implementacji w czasie wykonywania:

```csharp
DependencyService.Get<ITextToSpeech>().Speak("Hello from Xamarin Forms");
```

`DependencyService.Get<T>` znajdziesz implementacją interfejsu `T`.

### <a name="solution-structure"></a>Struktura rozwiązania

[Przykładowe rozwiązanie UsingDependencyService](https://developer.xamarin.com/samples/UsingDependencyService/) jest dla systemów iOS i Android, z opisanych powyżej zmian w kodzie wyróżnione.

 [![iOS i Android rozwiązania](introduction-images/solution-sml.png "DependencyService Przykładowa struktura rozwiązania")](introduction-images/solution.png#lightbox "DependencyService Przykładowa struktura rozwiązania")

> [!NOTE]
> Możesz **musi** zapewniać implementację w każdym projekcie platformy. Jeśli nie zarejestrowano żadnej implementacji interfejsu, a następnie `DependencyService` nie będzie można rozwiązać `Get<T>()` metody w czasie wykonywania.


## <a name="related-links"></a>Linki pokrewne

- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
