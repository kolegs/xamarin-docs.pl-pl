---
title: "Obsługa systemu Windows UrhoSharp"
description: "Określone Instalatora systemu Windows oraz funkcje dla UrhoSharp."
ms.topic: article
ms.prod: xamarin
ms.assetid: A4F36014-AE4E-4F07-A1AC-F264AAA68ACF
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.openlocfilehash: 0deefe0e00ec96a21317bfa2e8bf4894d5deae47
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="urhosharp-windows-support"></a>Obsługa systemu Windows UrhoSharp

_Windows określonych ustawień i funkcji_

Biblioteka klas przenośnych jest Urho i zezwala na tego samego interfejsu API do użycia przez różne platformy dla logiki gier, nadal należy zainicjować Urho w sterowniku określonych platform, a w niektórych przypadkach, można wykorzystać funkcje specyficzne dla platformy .

Na stronach przyjęto założenie, że `MyGame` jest podklasą `Application` klasy.

**Obsługiwane architektury:** tylko 64-bitowym systemie Windows.

Zawiera pełną przykłady przedstawiający sposób użycia to w naszym [próbek](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples)

# <a name="standalone-project"></a>Autonomiczny projektu

### <a name="creating-a-project"></a>Tworzenie projektu

Utwórz projekt konsoli, Urho NuGet odwołania, a następnie upewnij się, czy można zlokalizować zasoby (katalogi zawierającym katalog danych).

### <a name="configuring-and-launching-urho"></a>Konfigurowanie i uruchamianie Urho

Aby uruchomić aplikację, wykonaj następujące czynności:

```csharp
DesktopUrhoInitializer.AssetsDirectory = "../Assets";
new MyGame().Run();
```
### <a name="example"></a>Przykład

[Pełny przykład](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/Desktop)

## <a name="integrated-with-wpf"></a>Zintegrowana z WPF

### <a name="creating-a-project"></a>Tworzenie projektu

Utwórz projekt WPF, Urho NuGet odwołania, a następnie upewnij się, czy można zlokalizować zasoby (katalogi zawierającym katalog danych).

### <a name="configuring-and-launching-urho-from-wpf"></a>Konfigurowanie i uruchamianie Urho z WPF

Utwórz podklasę `Window` i skonfigurować zasobów następująco:

```csharp
    public partial class MainWindow : Window
    {
        Application currentApplication;

        public MainWindow()
        {
            InitializeComponent();
            DesktopUrhoInitializer.AssetsDirectory = @"../../Assets";
            Loaded += (s,e) => RunGame (new MyGame ());
        }

        async void RunGame(MyGame game)
        {
            var urhoSurface = new Panel { Dock = DockStyle.Fill };
            WindowsFormsHost.Child = urhoSurface;
            WindowsFormsHost.Focus();
            urhoSurface.Focus();
            await Task.Yield();
            var appOptions = new ApplicationOptions(assetsFolder: "Data")
                {
                    ExternalWindow = RunInSdlWindow.IsChecked.Value ? IntPtr.Zero : urhoSurface.Handle,
                    LimitFps = false, //true means "limit to 200fps"
                };
            currentApplication = Urho.Application.CreateInstance(value.Type, appOptions);
            currentApplication.Run();
        }
    }
```

### <a name="example"></a>Przykład

[Pełny przykład](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/WPF)

## <a name="integrated-with-uwp"></a>Zintegrowana z platformy uniwersalnej systemu Windows

### <a name="creating-a-project"></a>Tworzenie projektu

Tworzenie projektu platformy uniwersalnej systemu Windows, Urho NuGet odwołania, a następnie upewnij się, czy można zlokalizować zasoby (katalogi zawierającym katalog danych).

### <a name="configuring-and-launching-urho-from-uwp"></a>Konfigurowanie i uruchamianie Urho z platformy uniwersalnej systemu Windows

Utwórz podklasę `Window` i skonfigurować zasobów następująco:

```csharp
{
            InitializeComponent();
            GameTypes = typeof(Sample).GetTypeInfo().Assembly.GetTypes()
                .Where(t => t.GetTypeInfo().IsSubclassOf(typeof(Application)) && t != typeof(Sample))
                .Select((t, i) => new TypeInfo(t, $"{i + 1}. {t.Name}", ""))
                .ToArray();
            DataContext = this;
            Loaded += (s, e) => RunGame (new MyGame ());
        }

        public void RunGame(TypeInfo value)
        {
            //at this moment, UWP supports assets only in pak files (see PackageTool)
            currentApplication = UrhoSurface.Run(value.Type, "Data.pak");
        }
    }
```

### <a name="example"></a>Przykład

[Pełny przykład](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/UWP)

## <a name="integrated-with-windowsforms"></a>Zintegrowana z Windows.Forms

### <a name="creating-a-project"></a>Tworzenie projektu

Tworzenie projektu Windows.Forms, Urho NuGet odwołania, a następnie upewnij się, czy można zlokalizować zasoby (katalogi zawierającym katalog danych).

### <a name="configuring-and-launching-urho-from-windowsforms"></a>Konfigurowanie i uruchamianie Urho z Windows.Forms

Uruchamianie Urho w formularzu, zobacz [kompletnego przykładu](https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/WinForms/SamplesForm.cs)

