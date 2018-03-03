---
title: "Opcje udostępniania kodu"
description: "Ten dokument porównuje różne metody udostępniania kodu między projektami i platform: udostępnionych projektów, przenośnej biblioteki klas i .NET Standard, w tym zalety i wady każdego z nich."
ms.topic: article
ms.prod: xamarin
ms.assetid: B73675D2-09A3-14C1-E41E-20352B819B53
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 044dc0f3c0b5a86944fc852cdd97f8affcb8e874
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="sharing-code-options"></a>Opcje udostępniania kodu

_Ten dokument porównuje różne metody udostępniania kodu między projektami i platform: udostępnionych projektów, przenośnej biblioteki klas i .NET Standard, w tym zalety i wady każdego z nich._

## <a name="overview"></a>Omówienie

Istnieją trzy metody alternatywnej dla udostępniania kodu między aplikacjami i platform:

-   [**Udostępnionych projektów** ](#Shared_Projects) — typ projektu udostępnionego zasobów umożliwia organizowanie kodu źródłowego i użyj dyrektywy kompilatora #if wymagane do zarządzania wymagań specyficznych dla platformy.
-   [**Biblioteki klas przenośnych** ](#Portable_Class_Libraries) — tworzenie dotyczących przenośnej biblioteki klasy (PCL) obsługiwanych platform i używać interfejsów umożliwiają korzystanie z funkcji specyficznych dla platformy.
-   [**Standardowych bibliotek .NET** ](#Net_Standard) — projekty .NET Standard działają podobnie do PCLs korzystania z interfejsów iniekcję funkcje specyficzne dla platformy.

Celem strategii udostępniania kodu jest do obsługi architektury wyświetlany na tym diagramie, gdzie pojedynczego codebase mogą zostać użyte przez wiele platform.

 ![](code-sharing-images/conceptualarchitecture.png "Architektura aplikacji udostępnionych kodu")

W tym artykule porównuje trzy metody pomaga wybrać typ projektu dla aplikacji.

<a name="Shared_Projects" />

# <a name="shared-projects"></a>Udostępnionych projektów

Najprostszym sposobem udostępniania plików kodu jest używania projektu udostępnionego (wprowadzone w programie Xamarin Studio 5 i Visual Studio 2013 Update 2). Udostępnionych projektów są [szczegółowo opisana w tym miejscu](~/cross-platform/app-fundamentals/shared-projects.md).

Ten zrzut ekranu przedstawia plik rozwiązania zawierający trzy projekty aplikacji (dla systemu Android, iOS i Windows Phone), z **Shared** projekt, który zawiera typowe C# — pliki kodu źródłowego:

 ![](code-sharing-images/sharedsolution.png "Rozwiązania projektu udostępnionego")

Architektura koncepcyjna przedstawiono na poniższym diagramie, w którym każdy projekt zawiera wszystkie pliki udostępnione źródła:

 ![](code-sharing-images/sharedassetproject.png "Diagram projektu udostępnionego")


## <a name="example"></a>Przykład

Aplikacji międzyplatformowego, która obsługuje systemy iOS, Android i Windows Phone wymagają projekt aplikacji dla każdej platformy. Typowy kod znajduje się w projekcie udostępnionym.

Przykładowe rozwiązanie zawiera następujące foldery i projekty (nazwy projektu zostały wybrane dla wyrazistość, projektów nie należy przestrzegać następujących wytycznych nazw):

-   **Udostępnione** — udostępnionego projektu zawierającego kod, które są wspólne dla wszystkich projektów.
-   **AppAndroid** — projekt aplikacji platformy Xamarin.Android.
-   **AppiOS** — projekt aplikacji platformy Xamarin.iOS.
-   **AppWinPhone** — projekt aplikacji Windows Phone.


W ten sposób projekty aplikacji trzy współużytkują tego samego kodu źródłowego (C# pliki w współużytkowane). Wszystkie edycje do udostępnionego kodu będzie ono współużytkowane przez wszystkie trzy projekty.


## <a name="benefits"></a>Zalety

-  Umożliwia udostępnianie kodu w wielu projektach.
-  Udostępniony kod może rozgałęzionych oparte na platformie za pomocą dyrektywy kompilatora (np.) przy użyciu `#if __ANDROID__` , zgodnie z opisem w [tworzenie aplikacji dla wielu Platform](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) dokumentu).
-  Projekty aplikacji mogą zawierać odwołań specyficzne dla platformy, które mogą korzystać z udostępnionego kodu (np. przy użyciu `Community.CsharpSqlite.WP7` w próbce Tasky dla Windows Phone).



## <a name="disadvantages"></a>Wady

-  W przeciwieństwie do większości innych typów projektów projektu udostępnionego ma nie zestawu "output". Podczas kompilacji pliki są traktowane jako część odwołaniem do projektu i kompilowane do tego zestawu. Jeśli chcesz udostępniać kodu jako zestawu przenośnej biblioteki klas lub .NET Standard to lepszym rozwiązaniem.
-  Refaktoryzacje, które mają wpływ na kod wewnątrz dyrektywy kompilatora "inactive" nie może zaktualizować kodu.


 <a name="Shared_Remarks" />

## <a name="remarks"></a>Uwagi

Dobre rozwiązanie dla deweloperów aplikacji pisania kodu, który jest przeznaczone tylko dla udostępniania w aplikacji (i nie dystrybucja inni deweloperzy).

 <a name="Portable_Class_Libraries" />


# <a name="portable-class-libraries"></a>Biblioteki klas przenośnych


Biblioteki klas przenośnych są [szczegółowo opisana w tym miejscu](~/cross-platform/app-fundamentals/pcl.md).

 ![](code-sharing-images/portableclasslibrary.png "Diagram biblioteki klas przenośnych")


## <a name="benefits"></a>Zalety

-  Umożliwia udostępnianie kodu w wielu projektach.
-  Operacje refaktoryzacji zawsze aktualizują wszystkie odwołania.


## <a name="disadvantages"></a>Wady

-  Nie można użyć dyrektywy kompilatora.
-  Tylko podzbiór programu .NET framework jest dostępna do użycia, określany przez wybrany profil (zobacz [wprowadzenie do PCL](~/cross-platform/app-fundamentals/pcl.md) Aby uzyskać więcej informacji).


## <a name="remarks"></a>Uwagi

Dobrym rozwiązaniem, jeśli ma być używany wspólnie z innymi deweloperami wynikowego zestawu.



<a name="Net_Standard" />

# <a name="net-standard-libraries"></a>Standardowych bibliotek .NET

.NET standard jest [szczegółowo opisana w tym miejscu](~/cross-platform/app-fundamentals/net-standard.md).

![](code-sharing-images/netstandard.png "Diagram .NET standard")

## <a name="benefits"></a>Zalety

-  Umożliwia udostępnianie kodu w wielu projektach.
-  Operacje refaktoryzacji zawsze aktualizują wszystkie odwołania.
-  Większe powierzchni biblioteki klasy podstawowej platformy .NET (BCL) jest dostępna niż PCL profile.

## <a name="disadvantages"></a>Wady

 -  Nie można użyć dyrektywy kompilatora.

## <a name="remarks"></a>Uwagi

.NET standard jest podobny do PCL, ale z modelem łatwiejsze do obsługi platform i większej liczby klas z BCL.



# <a name="summary"></a>Podsumowanie

Kod udostępnianie strategii wybrane będzie uzależniona platform docelowych. Wybierz metodę, która działa najlepiej w projekcie.

PCL lub .NET Standard powinno się do tworzenia biblioteki zabezpieczać kodu (szczególnie publikowanie na NuGet). Udostępnionych projektów działa także dla deweloperów aplikacji, planowane jest używanie w swoich aplikacjach platforma między wiele funkcji z platformą.


## <a name="related-links"></a>Linki pokrewne

- [Tworzenie Cross Platform aplikacji (głównego dokumentu)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Biblioteki klas przenośnych](~/cross-platform/app-fundamentals/pcl.md)
- [Projekty udostępnione](~/cross-platform/app-fundamentals/shared-projects.md)
- [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [Analiza przypadku: Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Przykładowe tasky (github)](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [Przykładowe tasky przy użyciu PCL (github)](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)
- [Udostępnione Menedżera odwołań projektu dla Visual Studio 2013](http://visualstudiogallery.msdn.microsoft.com/315c13a7-2787-4f57-bdf7-adae6ed54450)
