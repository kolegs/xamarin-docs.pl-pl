---
title: Wprowadzenie do korzystania ze skoroszytami Xamarin zestawu SDK
ms.prod: xamarin
ms.assetid: FAED4445-9F37-46D8-B408-E694060969B9
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: ad7341776c2f37d4eb6238a26d5b12ee9e340ff7
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/18/2018
---
# <a name="getting-started-with-the-xamarin-workbooks-sdk"></a>Wprowadzenie do korzystania ze skoroszytami Xamarin zestawu SDK

Ten dokument zawiera wprowadzenie do rozwoju integracji dla skoroszytów Xamarin przewodnika. Większość to będzie działać z stabilna skoroszyty Xamarin, ale **ładowania integracji za pośrednictwem pakietów NuGet jest obsługiwany tylko w skoroszytach 1.3**, w kanału alfa w czasie zapisywania.

## <a name="general-overview"></a>Ogólne omówienie

Skoroszyty Xamarin integracji są małe bibliotek, korzystających z [ `Xamarin.Workbooks.Integrations` NuGet] [ nuget] zestaw SDK, aby zintegrować z skoroszytów Xamarin i Inspektora agentów w celu zapewnienia rozszerzonych środowisk.

Istnieją 3 głównych kroków wprowadzenie do rozwoju integracji — firma Microsoft będzie konspektu je tutaj.

## <a name="creating-the-integration-project"></a>Tworzenie projektu integracji

Integracja z biblioteki najlepiej są tworzone jako biblioteki dla wielu platform. Ponieważ chcesz zapewnić najlepsze integracji na wszystkich dostępnych agentów, przeszłych i przyszłych, należy wybrać powszechnie obsługiwany zestaw biblioteki. Zaleca się przy użyciu szablonu "Przenośnej biblioteki" najszerszych pomocy technicznej:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Biblioteka przenośna szablonu programu Visual Studio dla komputerów Mac](images/xamarin-studio-pcl.png)](images/xamarin-studio-pcl.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Biblioteka przenośna szablonu programu Visual Studio](images/visual-studio-pcl.png)](images/visual-studio-pcl.png#lightbox)

W programie Visual Studio należy upewnij się, że wybierz następujące platformy docelowej dla przenośnej biblioteki:

[![Biblioteka przenośna platformy Visual Studio](images/visual-studio-pcl-platforms.png)](images/visual-studio-pcl-platforms.png#lightbox)

-----

Po utworzeniu projektu biblioteki, Dodaj odwołanie do naszej `Xamarin.Workbooks.Integration` NuGet biblioteki za pośrednictwem Menedżera pakietów NuGet.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![NuGet Visual Studio for Mac](images/xamarin-studio-nuget.png)](images/xamarin-studio-nuget.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![NuGet Visual Studio](images/visual-studio-nuget.png)](images/visual-studio-nuget.png#lightbox)

-----

Należy usunąć pustą klasę, która zostanie utworzona automatycznie w ramach projektu, nie będzie można wymagające tego. Po wykonaniu tych czynności możesz rozpocząć tworzenie integracją.

## <a name="building-an-integration"></a>Tworzenie integracji

Firma Microsoft będzie utworzyć prostą integrację. Naprawdę przyłączyć kolor zielony, więc dodamy kolor zielony jako reprezentację każdego obiektu. Najpierw utwórz nową klasę o nazwie `SampleIntegration`i przydzielić mu zaimplementować naszych [ `IAgentIntegration` ] [ integration-type] interfejsu:

```csharp
using Xamarin.Interactive;

public class SampleIntegration : IAgentIntegration
{
    public void IntegrateWith (IAgent agent)
    {
    }
}
```

Chcemy się czy jest dodać [reprezentacja](~/tools/workbooks/sdk/representations.md) dla każdego obiektu, który jest kolor zielony. Firma Microsoft będzie w tym za pomocą dostawcy reprezentacji. Dziedzicz dostawców [ `RepresentationProvider` ] [ reppr] klasy — dla nasza, potrzebujemy zastąpienie [ `ProvideRepresentations` ] [ prrep]:

```csharp
using Xamarin.Interactive.Representations;

class SampleRepresentationProvider : RepresentationProvider
{
    public override IEnumerable<object> ProvideRepresentations (object obj)
    {
        // This corresponds to Pantone 2250 XGC, our favorite color.
        yield return new Color (0.0, 0.702, 0.4314);
    }
}
```

Firma Microsoft jest zwracanie [ `Color` ] [ color], wstępnie skompilowany reprezentacja typu w naszym zestawie SDK.
Można zauważyć, że w tym miejscu typ zwracany jest `IEnumerable<object>` &mdash;jednego dostawcę reprezentacja może zwracać wiele oświadczeń dla obiekt! Wszystkich dostawców reprezentacja są wywoływane dla każdego obiektu tak ważne jest, aby nie przyjmuje żadnych założeń o jakie obiekty są przekazywany do Ciebie.

Ostatnim krokiem jest rzeczywiście zarejestrować naszych dostawcy z agentem i informujące skoroszytów, gdzie można znaleźć typu naszych integracji. Aby zarejestrować dostawcę, Dodaj ten kod do `IntegrateWith` metoda `SampleIntegration` klasy utworzony wcześniej:

```csharp
agent.RepresentationManager.AddProvider (new SampleRepresentationProvider ());
```

Ustawienie typu integracji odbywa się za pośrednictwem atrybutu całego zestawu. Możesz też zaznaczyć w Twojej AssemblyInfo.cs lub w tej samej klasy jako typ integracji dla wygody:

```csharp
[assembly: AgentIntegration (typeof (SampleIntegration))]
````

Podczas tworzenia, może być bardziej wygodne [ `AddProvider` przeciążenia] [ addprovider] na `RepresentationManager` umożliwiające zarejestrować proste wywołania zwrotnego zapewnienie reprezentacje w skoroszycie a następnie przenieść ten kod do Twojej `RepresentationProvider` implementacji po zakończeniu. Przykład do renderowania [ `OxyPlot` ] [ oxyplot] `PlotModel` może wyglądać następująco:

```csharp
InteractiveAgent.RepresentationManager.AddProvider<PlotModel> (
  plotModel => Image (new SvgExporter {
      Width = 300,
      Height = 250
    }.ExportToString (plotModel)));
```

> [!NOTE]
> Te interfejsy API zapewniają szybko rozpocząć pracę, ale nie zalecamy wysyłania całego integracji tylko przy użyciu ich&mdash;zapewniają bardzo mało kontrolę nad jak Twoje typy są przetwarzane przez klienta.

Z reprezentacją zarejestrowany Integracja jest gotowy do wysłania!

## <a name="shipping-your-integration"></a>Wysyłanie integracją

Aby wysłać integracją, należy dodać go do pakietu NuGet.
Przesłanie z istniejącej biblioteki NuGet, lub jeśli tworzysz nowy pakiet, można użyć tego pliku .nuspec szablonu jako punktu wyjścia.
Należy wypełnić w sekcjach dotyczą integracją. Najważniejszy element jest, że wszystkie pliki dla integracją musi być w `xamarin.interactive` katalogu w elemencie głównym pakietu. Pozwala łatwo odnaleźć wszystkie odpowiednie pliki integracją, niezależnie od tego, czy użyć istniejącego pakietu lub Utwórz nową.

```xml
<?xml version="1.0"?>
<package xmlns="http://schemas.microsoft.com/packaging/2010/07/nuspec.xsd">
    <metadata>
      <id>$YourNuGetPackage$</id>
      <version>$YourVersion$</version>
      <authors>$YourNameHere$</authors>
      <projectUrl>$YourProjectPage$</projectUrl>
      <description>A short description of your library.</description>
    </metadata>
    <files>
      <file src="Path\To\Your\Integration.dll" target="xamarin.interactive" />
    </files>
</package>
```

Po utworzeniu pliku .nuspec można pakietu programu NuGet w następujący sposób:

```csharp
nuget pack MyIntegration.nuspec
```

a następnie opublikować go do [NuGet][nugetorg]. Po jest określony, będzie mógł odwołania ze skoroszytu i zobaczyć ją w akcji. Na poniższym zrzucie ekranu możemy zostały opakowane integracji próbki możemy utworzony w tym dokumencie i zainstalowano pakiet NuGet w skoroszycie:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Skoroszyt z integracją z usługą](images/mac-workbooks-integrated.png)](images/mac-workbooks-integrated.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Skoroszyt z integracją z usługą](images/windows-workbooks-integrated.png)](images/windows-workbooks-integrated.png#lightbox)

-----

Powiadomienie, że nie widzą żadnych `#r` dyrektywy lub jakikolwiek zainicjować integrację — skoroszytów ma podjąć obsługę to wszystko dla Ciebie w tle!

## <a name="next-steps"></a>Następne kroki

Zapoznaj się z naszym innych dokumentacji, aby uzyskać więcej informacji na temat przenoszenia elementy wchodzące w skład zestawu SDK, i naszych [przykładowe integracji](~/tools/workbooks/samples/index.md) dla dodatkowych czynności można wykonać z integracją, takich jak na przykład niestandardowych skryptów JavaScript, który jest uruchamiany w Klient skoroszytów.

[integration-type]: https://developer.xamarin.com/api/type/Xamarin.Interactive.IAgentIntegration/
[repman-api]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.IRepresentationManager/
[color]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.Color/
[xir]: https://developer.xamarin.com/api/namespace/Xamarin.Interactive.Representations/
[reppr]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.RepresentationProvider/
[prrep]: https://developer.xamarin.com/api/member/Xamarin.Interactive.Representations.RepresentationProvider.ProvideRepresentations/p/System.Object/
[nugetorg]: https://nuget.org
[nuget]: https://nuget.org/packages/Xamarin.Workbooks.Integration
[addprovider]: https://developer.xamarin.com/api/member/Xamarin.Interactive.Representations.IRepresentationManager.AddProvider/
[oxyplot]: http://www.oxyplot.org/
