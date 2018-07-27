---
title: Rozpoznawanie zależności w interfejsie Xamarin.Forms
description: W tym artykule wyjaśniono, jak iniekcję metoda rozpoznawania zależności do zestawu narzędzi Xamarin.Forms, tak aby kontenera iniekcji zależności aplikacji ma kontrolę nad konstrukcji i okresem istnienia niestandardowe programy renderujące, rzeczy i DependencyService implementacji.
ms.prod: xamarin
ms.assetid: 491B87DC-14CB-4ADC-AC6C-40A7627B2524
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/23/2018
ms.openlocfilehash: 2379c8ddc4bea6dd97bc4febd055dd8dfef39beb
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270491"
---
# <a name="dependency-resolution-in-xamarinforms"></a>Rozpoznawanie zależności w interfejsie Xamarin.Forms

_W tym artykule wyjaśniono, jak iniekcję metoda rozpoznawania zależności do zestawu narzędzi Xamarin.Forms, tak aby kontenera iniekcji zależności aplikacji ma kontrolę nad konstrukcji i okresem istnienia niestandardowe programy renderujące, rzeczy i DependencyService implementacji. Przykłady kodu są pobierane z [rozpoznawania zależności](https://developer.xamarin.com/samples/xamarin-forms/Advanced/DependencyResolution/) próbki._

W kontekście aplikacji platformy Xamarin.Forms, która używa wzorca Model-View-ViewModel (MVVM) kontenera iniekcji zależności można rejestrowania i wyświetlanie modeli rozpoznawania i rejestrowania usług i wprowadza je do modeli widoków. Podczas tworzenia modelu widoku kontenera wprowadza wszelkie zależności, które są wymagane. Jeśli te zależności nie zostały utworzone, kontener tworzy i jest rozpoznawana jako zależności. Aby uzyskać więcej informacji na temat wstrzykiwanie zależności, włącznie z przykładami wstrzykiwania zależności do modeli widoków, zobacz [wstrzykiwanie zależności](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md).

Kontrolę nad tworzenia i okresem istnienia typów w projektach platformy tradycyjnie odbywa się przez zestaw narzędzi Xamarin.Forms, która używa `Activator.CreateInstance` metodę w celu utworzenia wystąpienia elementu niestandardowe programy renderujące, efekty, i [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) implementacje. Niestety ogranicza to deweloperowi kontroli nad tworzeniem i okresem istnienia tych typów i możliwość iniekcji zależności do nich. Jednak to zachowanie można zmienić przez iniekcję metoda rozpoznawania zależności do zestawu narzędzi Xamarin.Forms kontrolować, jak tworzyć typy — kontenera iniekcji zależności aplikacji lub zestawu narzędzi Xamarin.Forms.

> [!NOTE]
> Nie istnieje wymóg iniekcję metoda rozpoznawania zależności do zestawu narzędzi Xamarin.Forms. Zestaw narzędzi Xamarin.Forms będzie tworzenie i zarządzanie okresem istnienia typów w projektach platformy, jeśli metoda rozpoznawania zależności nie są wstrzykiwane.

## <a name="injecting-a-dependency-resolution-method"></a>Wprowadzanie metoda rozpoznawania zależności

[ `DependencyResolver` ](xref:Xamarin.Forms.Internals.DependencyResolver) Klasy pozwala na iniekcję metoda rozpoznawania zależności do zestawu narzędzi Xamarin.Forms przy użyciu jednej z [ `ResolveUsing` ](Xamarin.Forms.Internals.DependencyResolver.ResolveUsing*) metody. Następnie podczas Xamarin.Forms wymaga wystąpienia określonego typu, metoda rozpoznawania zależności jest możliwość zapewnienie wystąpienia. Jeśli metoda rozpoznawania zależności zwraca `null` dla żądanego typu zestawu narzędzi Xamarin.Forms powróci do próby utworzenia typu wystąpienia swoim imieniu, używając `Activator.CreateInstance` metody.

Poniższy przykład pokazuje, jak ustawić metodę rozpoznawania zależności za pomocą [ `ResolveUsing` ](Xamarin.Forms.Internals.DependencyResolver.ResolveUsing*) metody:

```csharp
using Autofac;
using Xamarin.Forms.Internals;
...

public partial class App : Application
{
    // IContainer and ContainerBuilder are provided by Autofac
    static IContainer container;
    static readonly ContainerBuilder builder = new ContainerBuilder();

    public App()
    {
        ...
        DependencyResolver.ResolveUsing(type => container.IsRegistered(type) ? container.Resolve(type) : null);
        ...
    }
    ...
}
```

W tym przykładzie metoda rozpoznawania zależności jest równa Wyrażenie lambda, które używa kontenera iniekcji zależności Autofac, aby rozwiązać wszelkie typy, które zostały zarejestrowane w kontenerze. W przeciwnym razie `null` zostaną zwrócone, co spowoduje próba rozpoznania typu zestawu narzędzi Xamarin.Forms.

> [!NOTE]
> Interfejs API używany przez kontener iniekcji zależności jest charakterystyczne dla kontenera. Przykłady kodu, w tym artykule pełnić kontenera iniekcji zależności, co zapewnia Autofac `IContainer` i `ContainerBuilder` typów. Kontenery iniekcji zależności alternatywnych może być również stosowane, ale będzie używać różnych interfejsów API, nie są prezentowane.

Należy pamiętać, że nie jest wymagane do ustawiania metoda rozpoznawania zależności podczas uruchamiania aplikacji. Można ustawić w dowolnym momencie. Jedynym ograniczeniem jest, że Xamarin.Forms musi wiedzieć o metoda rozpoznawania zależności według czasu, która aplikacja próbuje używanie typów przechowywanych w kontenera iniekcji zależności. W związku z tym w przypadku usług w kontenera iniekcja zależności, w którym aplikacja będzie wymagać podczas uruchamiania, metoda rozpoznawania zależności będą muszą być ustawione na wczesnym etapie cyklu życia aplikacji. Podobnie jeśli kontenera iniekcji zależności zarządza tworzeniem i okresem istnienia określonego [ `Effect` ](xref:Xamarin.Forms.Effect), musisz wiedzieć o metoda rozpoznawania zależności, zanim spróbuje utworzyć widok zestawu narzędzi Xamarin.Forms, użyty `Effect`.

> [!WARNING]
> Rejestrowanie i rozpoznawanie typów z kontenera iniekcji zależności ma wydajności kosztów z powodu użycie odbicia kontenera do tworzenia każdego typu, zwłaszcza wtedy, gdy są są odtworzone zależności dla każdego nawigowania po stronach w aplikacji. W przypadku wielu lub szczegółowe zależności, znacznie wzrasta koszt tworzenia.

## <a name="registering-types"></a>Rejestrowanie typów

Typy muszą być zarejestrowane przy użyciu kontenera iniekcji zależności można rozwiązać je za pomocą metody rozpoznawania zależności. Poniższy przykład kodu pokazuje metody rejestracji, czy przykładowej aplikacji przedstawia w `App` klasy kontenera Autofac:

```csharp
using Autofac;
using Autofac.Core;
...

public partial class App : Application
{
    static IContainer container;
    static readonly ContainerBuilder builder = new ContainerBuilder();
    ...

    public static void RegisterType<T>() where T : class
    {
        builder.RegisterType<T>();
    }

    public static void RegisterType<TInterface, T>() where TInterface : class where T : class, TInterface
    {
        builder.RegisterType<T>().As<TInterface>();
    }

    public static void RegisterTypeWithParameters<T>(Type param1Type, object param1Value, Type param2Type, string param2Name) where T : class
    {
        builder.RegisterType<T>()
               .WithParameters(new List<Parameter>()
        {
            new TypedParameter(param1Type, param1Value),
            new ResolvedParameter(
                (pi, ctx) => pi.ParameterType == param2Type && pi.Name == param2Name,
                (pi, ctx) => ctx.Resolve(param2Type))
        });
    }

    public static void BuildContainer()
    {
        container = builder.Build();
    }
    ...
}
```

Gdy aplikacja używa metody rozpoznawania zależności rozwiązanie typów z kontenera, typu rejestracji są zazwyczaj wykonywane z projektów platformy. Dzięki temu projekty platformy zarejestrować niestandardowe programy renderujące, efekty, typy i [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) implementacji.

Po rejestracji typu z projektem platformy `IContainer` obiektu muszą zostać skompilowane, które odbywa się przez wywołanie metody `BuildContainer` metody. Ta metoda wywołuje firmy Autofac `Build` metody `ContainerBuilder` wystąpienia, która opiera się nowy kontener iniekcji zależności, który zawiera rejestracji, które zostały wprowadzone.

W kolejnych sekcjach `Logger` klasę, która implementuje `ILogger` interfejsu są wstrzykiwane do konstruktorów klas. `Logger` Klasa implementuje prosty rejestrowania funkcji przy użyciu `Debug.WriteLine` metody i jest używana do pokazują, jak można wstrzyknięte usług do niestandardowe programy renderujące, efekty, i [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) implementacji.

### <a name="registering-custom-renderers"></a>Rejestrowanie niestandardowe programy renderujące

Przykładowa aplikacja zawiera strony odtwarzania wideo w sieci web, którego źródłem XAML jest wyświetlana w poniższym przykładzie:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             ...>
    <video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />
</ContentPage>
```

`VideoPlayer` Widok jest zaimplementowana na każdej platformie przez `VideoPlayerRenderer` klasy, który udostępnia funkcjonalność do odtwarzania wideo. Aby uzyskać więcej informacji na temat tych klas niestandardowego modułu renderowania, zobacz [Implementowanie odtwarzacza wideo](~/xamarin-forms/app-fundamentals/custom-renderer/video-player/index.md).

W systemach iOS i Windows platformy Uniwersalnej `VideoPlayerRenderer` klasy mają następujący Konstruktor, który wymaga `ILogger` argumentu:

```csharp
public VideoPlayerRenderer(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

Na wszystkich trzech platformach, typ rejestracji za pomocą kontenera iniekcji zależności odbywa się przez `RegisterTypes` metody, która jest wywoływana przed platformy podczas ładowania aplikacji przy użyciu `LoadApplication(new App())` metody. W poniższym przykładzie przedstawiono `RegisterTypes` metody na platformy iOS:

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterType<FormsVideoLibrary.iOS.VideoPlayerRenderer>();
    App.BuildContainer();
}
```

W tym przykładzie `Logger` konkretny typ jest zarejestrowany za pomocą mapowania względem jego typ interfejsu i `VideoPlayerRenderer` rejestrowany bezpośrednio, bez mapowania interfejsu. Gdy użytkownik przechodzi do strony `VideoPlayer` widoku, metoda rozpoznawania zależności zostanie wywołany, aby rozwiązać `VideoPlayerRenderer` typu z kontenera iniekcji zależności, co spowoduje także rozwiązać i wstawić `Logger` wpisać w `VideoPlayerRenderer` konstruktora.

`VideoPlayerRenderer` Konstruktora platformy systemu Android jest nieco bardziej skomplikowane, ponieważ wymaga ona `Context` argument oprócz `ILogger` argumentu:

```csharp
public VideoPlayerRenderer(Context context, ILogger logger) : base(context)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

W poniższym przykładzie przedstawiono `RegisterTypes` metody platformy systemu Android:

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterTypeWithParameters<FormsVideoLibrary.Droid.VideoPlayerRenderer>(typeof(Android.Content.Context), this, typeof(ILogger), "logger");
    App.BuildContainer();
}
```

W tym przykładzie `App.RegisterTypeWithParameters` rejestrów metoda `VideoPlayerRenderer` z kontenera iniekcji zależności. Metoda rejestracji zapewnia, że `MainActivity` wystąpienia zostaną dodane jako `Context` argumentu, a `Logger` typu zostaną dodane jako `ILogger` argumentu.

### <a name="registering-effects"></a>Rejestrowanie efekty

Przykładowa aplikacja zawiera stronę, która używa touch, śledzenia wpływu przeciągać [ `BoxView` ](xref:Xamarin.Forms.BoxView) wystąpień wokół strony. [ `Effect` ](xref:Xamarin.Forms.Effect) Jest dodawany do `BoxView` przy użyciu następującego kodu:

```csharp
var boxView = new BoxView { ... };
var touchEffect = new TouchEffect();
boxView.Effects.Add(touchEffect);
```

`TouchEffect` Klasa jest [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) wykonywane na każdej platformie przez `TouchEffect` klasy, która ma `PlatformEffect`. Platforma `TouchEffect` klasa udostępnia funkcje przeciągania `BoxView` wokół strony. Aby uzyskać więcej informacji na temat tych klas efekt zobacz [wywoływanie zdarzenia z efektów](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md).

Na wszystkich trzech platformach `TouchEffect` klasa ma następujący Konstruktor, który wymaga `ILogger` argumentu:

```csharp
public TouchEffect(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

Na wszystkich trzech platformach, typ rejestracji za pomocą kontenera iniekcji zależności odbywa się przez `RegisterTypes` metody, która jest wywoływana przed platformy podczas ładowania aplikacji przy użyciu `LoadApplication(new App())` metody. W poniższym przykładzie przedstawiono `RegisterTypes` metody platformy systemu Android:

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterType<TouchTracking.Droid.TouchEffect>();
    App.BuildContainer();
}
```

W tym przykładzie `Logger` konkretny typ jest zarejestrowany za pomocą mapowania względem jego typ interfejsu i `TouchEffect` rejestrowany bezpośrednio, bez mapowania interfejsu. Gdy użytkownik przechodzi do strony [ `BoxView` ](xref:Xamarin.Forms.BoxView) wystąpienia, która ma `TouchEffect` podłączone do niego, metoda rozpoznawania zależności wywoływane w celu rozwiązania platformy `TouchEffect` typu na podstawie zależności kontenera iniekcji, co spowoduje także rozwiązać i wstawić `Logger` wpisać w `TouchEffect` konstruktora.

### <a name="registering-dependencyservice-implementations"></a>Rejestrowanie DependencyService implementacji

Przykładowa aplikacja zawiera stronę, która używa [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) implementacji na każdej platformie, aby umożliwić użytkownikowi do pobrania zdjęcia z biblioteki obrazów urządzenia. `IPhotoPicker` Interfejs definiuje funkcje, które jest implementowany przez `DependencyService` implementacji i przedstawiono w poniższym przykładzie:

```csharp
public interface IPhotoPicker
{
    Task<Stream> GetImageStreamAsync();
}
```

W każdym projekcie platformy `PhotoPicker` klasy implementuje `IPhotoPicker` interfejs przy użyciu interfejsów API platformy. Aby uzyskać więcej informacji na temat tych usług zależności zobacz [pobrania zdjęcia z biblioteki obrazów](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md).

Na wszystkich trzech platformach `PhotoPicker` klasa ma następujący Konstruktor, który wymaga `ILogger` argumentu:

```csharp
public PhotoPicker(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

Na wszystkich trzech platformach, typ rejestracji za pomocą kontenera iniekcji zależności odbywa się przez `RegisterTypes` metody, która jest wywoływana przed platformy podczas ładowania aplikacji przy użyciu `LoadApplication(new App())` metody. W poniższym przykładzie przedstawiono `RegisterTypes` metoda dla platformy uniwersalnej systemu Windows:

```csharp
void RegisterTypes()
{
    DIContainerDemo.App.RegisterType<ILogger, Logger>();
    DIContainerDemo.App.RegisterType<IPhotoPicker, Services.UWP.PhotoPicker>();
    DIContainerDemo.App.BuildContainer();
}
```

W tym przykładzie `Logger` konkretny typ jest zarejestrowany za pomocą mapowania względem jego typ interfejsu i `PhotoPicker` typ również został zarejestrowany za pomocą mapowania interfejsu. Gdy użytkownik przechodzi do strony pobierania zdjęć i wybierze Wybierz zdjęcie, `OnSelectPhotoButtonClicked` program obsługi jest wykonywana:

```csharp
async void OnSelectPhotoButtonClicked(object sender, EventArgs e)
{
    ...
    var photoPickerService = DependencyService.Resolve<IPhotoPicker>();
    var stream = await photoPickerService.GetImageStreamAsync();
    if (stream != null)
    {
        image.Source = ImageSource.FromStream(() => stream);
    }
    ...
}
```

Gdy [ `DependencyService.Resolve<T>` ](xref:Xamarin.Forms.DependencyService.Resolve*) metoda jest wywoływana, metoda rozpoznawania zależności zostanie wywołany, aby rozwiązać `PhotoPicker` typu z kontenera iniekcji zależności, co spowoduje także rozwiązać i wstawić `Logger` typu do `PhotoPicker` konstruktora.

> [!NOTE]
> [ `Resolve<T>` ](xref:Xamarin.Forms.DependencyService.Resolve*) Metody należy użyć podczas rozpoznawania typu z kontenera iniekcji zależności aplikacji za pośrednictwem [ `DependencyService` ](xref:Xamarin.Forms.DependencyService).

## <a name="related-links"></a>Linki pokrewne

- [Rozpoznawanie zależności (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Advanced/DependencyResolution/)
- [Wstrzykiwanie zależności](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md)
- [Implementowanie odtwarzacza wideo](~/xamarin-forms/app-fundamentals/custom-renderer/video-player/index.md)
- [Wywoływanie zdarzenia z efektów](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
- [Pobrania zdjęcia z biblioteki obrazów](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
