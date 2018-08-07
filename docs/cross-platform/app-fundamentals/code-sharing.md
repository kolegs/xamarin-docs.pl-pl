---
title: Udostępnianie kodu — omówienie
description: 'W tym dokumencie porównuje różne metody udostępniania kodu między projektami dla platform: udostępnionych projektów, biblioteki klas przenośnych i .NET Standard, w tym zalety i wady każdego z nich.'
ms.prod: xamarin
ms.assetid: B73675D2-09A3-14C1-E41E-20352B819B53
author: conceptdev
ms.author: crdun
ms.date: 08/06/2018
ms.openlocfilehash: 98b5786ae4f071b4d8e8f854561db97aee037fdc
ms.sourcegitcommit: aa7b0182d117e2af66ffaa4fa29b8c214ceecae1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39520288"
---
# <a name="sharing-code-overview"></a>Udostępnianie kodu — omówienie

_W tym dokumencie porównuje różne metody udostępniania kodu między projektami dla platform: .NET Standard, udostępnionych projektów i biblioteki klas przenośnych, w tym zalety i wady każdego z nich._

Istnieją trzy metody udostępniania kodu między aplikacjami na wielu platformach:

- [**Standardowych bibliotek .NET** ](#Net_Standard) — projektów .NET Standard można zaimplementować kod, aby być współużytkowane przez wiele platform i mogą uzyskiwać dostęp do wielu interfejsów API platformy .NET (w zależności od wersji). .NET standard 1.0 w wersji 1.6 zaimplementować stopniowo większych zestawów interfejsów API, a .NET Standard 2.0 zapewnia najlepsze pokrycia BCL platformy .NET (w tym interfejsów API platformy .NET dostępnych w aplikacjach platformy Xamarin).
- [**Projekty udostępnione** ](#Shared_Projects) — typ projektu zasobów udostępnionych umożliwia organizowanie kodu źródłowego i użyj `#if` dyrektywy kompilatora na potrzeby zarządzania wymaganiami specyficznymi dla platformy.
- [**Biblioteki klas przenośnych** ](#Portable_Class_Libraries) (przestarzałe) — biblioteki klas przenośnych (PCLs) można wybierać docelowo wiele platform za pomocą wspólne środowisko interfejsów API i używać interfejsów umożliwiają korzystanie z funkcji specyficznych dla platformy. PCLs zostały zaniechane w najnowszej wersji programu Visual Studio &ndash; zamiast tego użyj .NET Standard.

Jest celem strategii udostępniania kodu do obsługi architektury pokazano na poniższym diagramie, gdzie pojedynczą bazą kodu może być wykorzystywane przez wiele platform.

 ![Udostępniony kod architektury aplikacji](code-sharing-images/conceptualarchitecture.png "udostępnionego kodu architektury aplikacji")

W tym artykule porównano metod, które można wybrać typ projektu dla aplikacji.

<a name="Net_Standard" />

## <a name="net-standard-libraries"></a>Standardowych bibliotek platformy .NET

[.NET standard](~/cross-platform/app-fundamentals/net-standard.md) biblioteki udostępnia również określony zbiór bibliotek klas bazowych, które mogą być przywoływane w różnych typach projektów, w tym projektów dla wielu platform, takich jak platformy Xamarin.Android i Xamarin.iOS. .NET standard 2.0 jest zalecane w przypadku maksymalną zgodność z istniejącego kodu .NET Framework.

![Diagram .NET standard](code-sharing-images/netstandard.png "diagram .NET Standard")

### <a name="benefits"></a>Zalety

- Umożliwia udostępnianie kodu w wielu projektach.
- Operacje refaktoryzacji zawsze aktualizują wszystkie objęte odwołania.
- Większy obszar powierzchni Biblioteka klasy podstawowej platformy .NET (BCL) jest dostępny niż PCL profile. W szczególności .NET Standard 2.0 ma prawie ten sam powierzchni interfejsu API jako .NET Framework i jest zalecana dla nowych aplikacji oraz przenoszenia istniejących PCLs.

### <a name="disadvantages"></a>Wady

- Nie można użyć dyrektywy kompilatora, takich jak `#if __IOS__`.

### <a name="remarks"></a>Uwagi

.NET standard to [podobne do PCL](https://docs.microsoft.com/dotnet/standard/net-standard#comparison-to-portable-class-libraries), ale z modelem prostsza do obsługi platform i większą liczbę klas z BCL.

<a name="Shared_Projects" />

## <a name="shared-projects"></a>Projekty udostępnione

[Projekty udostępnione](~/cross-platform/app-fundamentals/shared-projects.md) zawierają pliki kod i zasoby, które znajdują się w każdym projekcie, który odwołuje się do nich. Udział w projektach nie tworzą skompilowanych danych wyjściowych własnych.

Ten zrzut ekranu przedstawia plik rozwiązania, zawierający trzy projekty aplikacji (dla systemów Android, iOS i Windows), za pomocą **Shared** projektu, który zawiera wspólnego języka C# plików kodu źródłowego:

![Udostępnione projektu rozwiązania](code-sharing-images/sharedsolution.png "udostępnionego projektu rozwiązania")

Architektura koncepcyjna pokazano na poniższym diagramie, gdzie każdy projekt zawiera wszystkie pliki udostępnione źródła:

![Diagram projektu współużytkowanego](code-sharing-images/sharedassetproject.png "diagram Projekt udostępniony")

### <a name="example"></a>Przykład

Aplikację dla wielu platform, która obsługuje systemy iOS, Android i Windows wymagają projektu aplikacji dla każdej platformy. Typowy kod znajduje się w projektu udostępnionego.

Przykładowe rozwiązanie będzie zawierać następujące foldery i projektów (nazwy projektu zostały wybrane dla wyrazistość, projekty nie muszą przestrzegać następujących wytycznych nazewnictwa):

- **Współdzielona** — udostępnionego projektu zawierającego kod, które są wspólne dla wszystkich projektów.
- **AppAndroid** — projekt aplikacji platformy Xamarin.Android.
- **AppiOS** — projekt aplikacji platformy Xamarin.iOS.
- **AppWindows** — projekt aplikacji Windows.

W ten sposób projektów aplikacji trzy udostępniania tego samego kodu źródłowego (C# pliki w udostępniony). Zmiany współużytkowanym kodem będzie ono współużytkowane przez wszystkie trzy projekty.

### <a name="benefits"></a>Zalety

- Umożliwia udostępnianie kodu w wielu projektach.
- Udostępniony kod może rozgałęzione zależnie od platformy za pomocą dyrektywy kompilatora (np.) za pomocą `#if __ANDROID__` , zgodnie z opisem w [tworzenie Cross Platform aplikacji](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) dokumentu).
- Projekty aplikacji mogą zawierać odwołań do specyficznych dla platformy, które mogą korzystać z udostępnionego kodu (np. przy użyciu `Community.CsharpSqlite.WP7` w próbce Tasky dla Windows Phone).

### <a name="disadvantages"></a>Wady

- Refaktoryzacje, które mają wpływ na kod wewnątrz dyrektywy kompilatora "inactive" nie jest odświeżany kod wewnątrz te dyrektywy.
- W przeciwieństwie do większości innych typów projektów udostępnionych projekt ma nie zestawu "output". Podczas kompilacji pliki są traktowane jako część projektu z odwołaniem i kompilowane do tego zestawu. Jeśli chcesz udostępnić swój kod jako zestawu .NET Standard lub biblioteki klas przenośnych to lepszym rozwiązaniem.

<a name="Shared_Remarks" />

### <a name="remarks"></a>Uwagi

Dobrym rozwiązaniem w przypadku programistom pisanie kodu, który jest przeznaczona tylko do udostępniania w ich aplikacji (i nie Koduj innym deweloperom).

<a name="Portable_Class_Libraries" />

## <a name="portable-class-libraries"></a>Biblioteki klas przenośnych

> [!TIP]
> Biblioteki .NET standard 2.0, zaleca się za pośrednictwem biblioteki klas przenośnych.

Biblioteki klas przenośnych są [omówiono szczegółowo w tym miejscu](~/cross-platform/app-fundamentals/pcl.md).

![Diagram biblioteki klas przenośnych](code-sharing-images/portableclasslibrary.png "diagram biblioteki klas przenośnych")

### <a name="benefits"></a>Zalety

- Umożliwia udostępnianie kodu w wielu projektach.
- Operacje refaktoryzacji zawsze aktualizują wszystkie objęte odwołania.

### <a name="disadvantages"></a>Wady

- Przestarzałe w najnowszych wersjach programu Visual Studio, biblioteki .NET Standard są zalecane, zamiast tego. Zapoznaj się z tym [wyjaśnienie różnic](https://docs.microsoft.com/dotnet/standard/net-standard#comparison-to-portable-class-libraries) między PCL i .NET Standard.
- Nie można użyć dyrektywy kompilatora.
- Tylko podzestaw programu .NET framework jest dostępna do użycia, określane przez wybrany profil (zobacz [wprowadzenie do aplikacji PCL](~/cross-platform/app-fundamentals/pcl.md) Aby uzyskać więcej informacji).

### <a name="remarks"></a>Uwagi

Szablon PCL jest uznawane za przestarzałe w najnowszych wersjach programu Visual Studio.

## <a name="summary"></a>Podsumowanie

Udostępnianie wybór strategii kodu będzie uzależniona dla różnych platform docelowych. Wybierz metodę, która działa najlepiej dla Twojego projektu.

.NET standard jest najlepszym wyborem do tworzenia bibliotek współdzielonego kodu (szczególnie publikowanie dla narzędzia NuGet). Projekty udostępnione zadziałać dla deweloperów aplikacji, które planujesz użyć wiele funkcji specyficznych dla platformy w swoich aplikacjach dla wielu platform.

Gdy projektów PCL w dalszym ciągu być obsługiwane w programie Visual Studio, .NET Standard jest zalecane w przypadku nowych projektów.

## <a name="related-links"></a>Linki pokrewne

- [Tworzenie Cross Platform aplikacji (dokumentu głównego)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Biblioteki klas przenośnych](~/cross-platform/app-fundamentals/pcl.md)
- [Projekty udostępnione](~/cross-platform/app-fundamentals/shared-projects.md)
- [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [Analiza przypadku: Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Przykładowe tasky (github)](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [Przykładowe tasky przy użyciu języka PCL (github)](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)
