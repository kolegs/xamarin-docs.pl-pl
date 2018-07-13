---
title: Wprowadzenie do DependencyService
description: W tym artykule opisano sposób działania funkcji platformy natywnej dostępu klasy DependencyService zestawu narzędzi Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 5d019604-4f6f-4932-9b26-1fce3b4d88f8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
ms.openlocfilehash: 558a05b5fdc4c4f08194b708de886bca342dd860
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995417"
---
# <a name="introduction-to-dependencyservice"></a>Wprowadzenie do DependencyService

## <a name="overview"></a>Omówienie

[`DependencyService`](xref:Xamarin.Forms.DependencyService) Umożliwia aplikacjom wywoływać funkcje specyficzne dla platformy z udostępnionego kodu. Ta funkcja umożliwia aplikacji Xamarin.Forms robić wszystko, co można zrobić w aplikacji natywnej.

`DependencyService` jest mechanizm rozpoznawania zależności. W praktyce zdefiniowano interfejs i `DependencyService` znajdzie poprawność implementacji interfejsu w różnych projektach platformy.

## <a name="how-dependencyservice-works"></a>Jak działa DependencyService

Aplikacje Xamarin.Forms muszą cztery składniki używane `DependencyService`:

- **Interfejs** &ndash; wymaganych funkcji jest definiowany przez interfejs w współużytkowanym kodem.
- **Wdrożenia na platformie** &ndash; klas, które implementują interfejs musi zostać dodany do każdego projektu platformy.
- **Rejestracja** &ndash; każdej klasy implementującej muszą być zarejestrowane w usłudze `DependencyService` za pomocą atrybutu metadanych. Rejestracja umożliwia `DependencyService` można znaleźć implementującej klasy i podać zamiast interfejsu, w czasie wykonywania.
- **Wywołanie DependencyService** &ndash; udostępnionego kodu musi jawnie wywołać `DependencyService` poprosić o implementacji interfejsu.

Pamiętaj, że implementacje musi być podana dla każdego projektu platformy w rozwiązaniu. Projekty platform bez implementacji zakończy się niepowodzeniem w czasie wykonywania.

Poniższy diagram tłumaczy strukturę aplikacji:

![](introduction-images/overview-diagram.png "DependencyService struktury aplikacji")

### <a name="interface"></a>Interface

Interfejs, który projektowania omówiono definiowanie sposobu interakcji użytkowników z funkcjami specyficznymi dla platformy. Należy zachować ostrożność, jeśli tworzysz składnik do udostępnienia jako składnik lub pakietu Nuget. Projekt interfejsu API może poprawić lub obniżyć pakietu. W poniższym przykładzie prosty interfejs wypowiedzi tekst, który umożliwia elastyczność podczas określania wyrazy wymawiane, ale pozostawia implementacji można dostosować dla każdej platformy:

```csharp
public interface ITextToSpeech {
    void Speak ( string text ); //note that interface members are public by default
}
```

### <a name="implementation-per-platform"></a>Implementacja na każdą platformę

Po odpowiedni interfejs został zaprojektowany tak, ten interfejs musi zaimplementować w projekcie dla każdej z platform docelowych. Na przykład, następujące klasy implementuje `ITextToSpeech` interfejs w systemie iOS:

```csharp
namespace UsingDependencyService.iOS
{
    public class TextToSpeech_iOS : ITextToSpeech
    {
        public void Speak (string text)
        {
            var speechSynthesizer = new AVSpeechSynthesizer ();

            var speechUtterance = new AVSpeechUtterance (text) {
                Rate = AVSpeechUtterance.MaximumSpeechRate/4,
                Voice = AVSpeechSynthesisVoice.FromLanguage ("en-US"),
                Volume = 0.5f,
                PitchMultiplier = 1.0f
            };

            speechSynthesizer.SpeakUtterance (speechUtterance);
        }
    }
}
```

### <a name="registration"></a>Rejestracja

Każda implementacja interfejsu muszą być zarejestrowane przy użyciu `DependencyService` za pomocą atrybutu metadanych. Poniższy kod rejestruje wdrożenia dla systemu iOS:

```csharp
[assembly: Dependency (typeof (TextToSpeech_iOS))]
namespace UsingDependencyService.iOS
{
  ...
}
```

Łączenie wszystkiego razem, implementacja specyficzna dla platformy wygląda następująco:

```csharp
[assembly: Dependency (typeof (TextToSpeech_iOS))]
namespace UsingDependencyService.iOS
{
    public class TextToSpeech_iOS : ITextToSpeech
    {
        public void Speak (string text)
        {
            var speechSynthesizer = new AVSpeechSynthesizer ();

            var speechUtterance = new AVSpeechUtterance (text) {
                Rate = AVSpeechUtterance.MaximumSpeechRate/4,
                Voice = AVSpeechSynthesisVoice.FromLanguage ("en-US"),
                Volume = 0.5f,
                PitchMultiplier = 1.0f
            };

            speechSynthesizer.SpeakUtterance (speechUtterance);
        }
    }
}
```

Uwaga:, rejestracja odbywa się na poziomie przestrzeni nazw, a nie na poziomie klasy.

#### <a name="universal-windows-platform-net-native-compilation"></a>Natywnej kompilacji .NET platformy Universal Windows

Należy stosować w projektach platformy uniwersalnej systemu Windows, korzystających z platformy .NET Native opcję kompilacji [nieco innej konfiguracji](~/xamarin-forms/platform/windows/installation/index.md#target-invocation-exception) podczas inicjowania zestawu narzędzi Xamarin.Forms. Kompilacja .NET native wymaga również nieco rejestracji dla zależności usług.

W **App.xaml.cs** plików, należy ręcznie zarejestrować każda usługa zależności zdefiniowane w projekt platformy uniwersalnej systemu Windows za pomocą `Register<T>` metody, jak pokazano poniżej:

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// register the dependencies in the same
Xamarin.Forms.DependencyService.Register<TextToSpeechImplementation>();
```

Uwaga: użycie ręcznej rejestracji `Register<T>` jest efektywne tylko w wersji kompilacji, przy użyciu kompilacja .NET Native. Jeśli pominiesz ten wiersz, kompilacjach debugowania będą nadal działać, ale kompilacji wersji zakończy się niepowodzeniem do ładowania usługi zależności.

### <a name="call-to-dependencyservice"></a>Wywołanie DependencyService

Po projektu skonfigurowano przy użyciu wspólnego interfejsu i implementacje dla poszczególnych platform, użyj `DependencyService` uzyskać odpowiednie implementacji w czasie wykonywania:

```csharp
DependencyService.Get<ITextToSpeech>().Speak("Hello from Xamarin Forms");
```

`DependencyService.Get<T>` znajdzie implementacją interfejsu `T`.

### <a name="solution-structure"></a>Struktura rozwiązania

[Przykładowe rozwiązanie UsingDependencyService](https://developer.xamarin.com/samples/UsingDependencyService/) jest wyświetlane poniżej dla systemów iOS i Android, opisanych powyżej zmian w kodzie wyróżnione.

 [![iOS i rozwiązania dla systemu Android](introduction-images/solution-sml.png "DependencyService przykładowe rozwiązanie struktury")](introduction-images/solution.png#lightbox "DependencyService przykładowe rozwiązanie struktury")

> [!NOTE]
> Możesz **musi** zapewniać implementację w każdym projekcie platformy. Jeśli nie zarejestrowano żadnej implementacji interfejsu, a następnie `DependencyService` nie będzie można rozwiązać `Get<T>()` metody w czasie wykonywania.


## <a name="related-links"></a>Linki pokrewne

- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
