---
title: Tematy zaawansowane integracji
description: W tym dokumencie opisano Tematy zaawansowane powiązany integracji skoroszyty Xamarin. Zawarto informacje pakietu Xamarin.Workbook.Integrations NuGet i zagrożeń interfejsu API w skoroszycie Xamarin.
ms.prod: xamarin
ms.assetid: 002CE0B1-96CC-4AD7-97B7-43B233EF57A6
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: 1aa6b5d0ca574345e1d349ea53df96f554c06bc4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793839"
---
# <a name="advanced-integration-topics"></a>Tematy zaawansowane integracji

Zestawy integracji powinien odwoływać [ `Xamarin.Workbooks.Integrations` NuGet][nuget]. Zapoznaj się z naszym [szybki start dokumentacji](~/tools/workbooks/sdk/index.md) Aby uzyskać więcej informacji na temat rozpoczynania pracy z pakietu NuGet.

Integracji klienta są również obsługiwane i są inicjowane przez umieszczenie plików JavaScript i CSS z taką samą nazwę jak zestawu integracji agenta w tym samym katalogu. Na przykład, jeśli nosi nazwę zestawu integracji agenta (do którego odwołuje się do NuGet) `SampleExternalIntegration.dll`, następnie `SampleExternalIntegration.js` i `SampleExternalIntegration.css` będzie można zintegrować klienta oraz, jeśli istnieją. Integracji klienta są opcjonalne.

Zewnętrzne integrację sam można dostarczana w NuGet, pod warunkiem i przywoływany bezpośrednio wewnątrz aplikacji, który jest hostem agenta lub po prostu umieszczone obok `.workbook` pliku, który chce go używać.

Zewnętrzne integracji (agenta i klienta) w pakietach NuGet zostanie załadowany automatycznie, gdy pakiet odwołuje się do, zgodnie z dokumentacją szybki start, podczas gdy zestawy integracji wysłane obok skoroszyt, należy w ten sposób odwołania:

```csharp
#r "SampleExternalIntegration.dll"
```

Podczas odwoływania się do integracji w ten sposób, jego nie zostanie załadowany przez klienta od razu&mdash;należy wywoływanie kodu z integracji, aby go załadować. Firma Microsoft będzie tego błędu można adresowania w przyszłości.

`Xamarin.Interactive` PCL zapewnia kilka ważnych integrację interfejsów API. Co integracji musisz podać co najmniej punkt wejścia integracji:

```csharp
using Xamarin.Interactive;

[assembly: AgentIntegration (typeof (AgentIntegration))]

class AgentIntegration : IAgentIntegration
{
    const string TAG = nameof (AgentIntegration);

    public void IntegrateWith (IAgent agent)
    {
        // hook into IAgent APIs
    }
}
```

W tym momencie po integracji zestawu odwołuje się do klienta niejawnie załaduje pliki integracji JavaScript i CSS.

## <a name="apis"></a>interfejsy API

Zgodnie z zestawu, który jest przywoływany przez skoroszytu lub na żywo sprawdzić sesji, żadnego z jego publiczne interfejsy API są dostępne dla sesji. Dlatego ważne jest ma bezpieczne i za pośrednictwem powierzchni interfejsu API dla użytkowników eksplorować.

Integracja zestawu skutecznie jest mostka między aplikacją lub zestawu SDK zainteresowania i sesji. Umożliwia ona nowych interfejsów API, sensu, szczególnie w kontekście skoroszytu lub na żywo sprawdzić sesji, lub podaj nie publiczne interfejsy API i po prostu wykonać zadania w "w tle", takie jak reaguje obiektu [reprezentacje](~/tools/workbooks/sdk/representations.md).

> [!NOTE]
> Interfejsy API, które muszą być publiczne, ale nie powinny być udostępniane za pośrednictwem IntelliSense może być oznaczony przez zwykle `[EditorBrowsable (EditorBrowsableState.Never)]` atrybutu.

[nuget]: https://nuget.org/packages/Xamarin.Workbooks.Integration
