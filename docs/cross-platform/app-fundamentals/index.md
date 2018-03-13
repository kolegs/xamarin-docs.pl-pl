---
title: Podstawy aplikacji
description: Podstawowe koncepcje aplikacji
ms.topic: article
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: c5b823370e5b65fbcf9ba366cb89c05e003b1a89
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="application-fundamentals"></a>Podstawy aplikacji

Ta sekcja zawiera przewodnik dla niektórych zadań rzeczy częściej lub pojęcia, które deweloperzy muszą znać podczas opracowywania aplikacji dla urządzeń przenośnych.

##  <a name="building-cross-platform-applicationscross-platformapp-fundamentalsbuilding-cross-platform-applicationsindexmd"></a>[Tworzenie aplikacji międzyplatformowych](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)

Wybierając Xamarin i utrzymywanie kilka rzeczy pod uwagę podczas projektowania i opracowywania aplikacji mobilnych, można realizować ogromne kodu udostępnianie platform urządzeń przenośnych, ograniczenie czasu na rynek, wykorzystać istniejące talenty, spełniają popyt na dostęp z urządzeń przenośnych, i zmniejszyć złożoność i platform. &nbsp;w tym dokumencie przedstawiono wskazówki klucza do realizacji tych korzyści narzędzia i wydajności aplikacji.

## <a name="code-sharing-optionscode-sharingmd"></a>[Opcje udostępniania kodu](code-sharing.md)

Więcej informacji na temat kodu różne opcje dostępne dla projektów Xamarin, łącznie z przenośnej biblioteki klas (PCLs), udostępnionych projektów i standardowych bibliotek .NET udostępniania.


## <a name="accessibilityaccessibilitymd"></a>[Ułatwienia dostępu](accessibility.md)

Wskazówki dotyczące tworzenia aplikacji dostępny.


## <a name="localizationlocalizationmd"></a>[Lokalizacja](localization.md)

Wskazówki dotyczące wprowadzania regionalnymi aplikacje, które można przetłumaczyć wielu języków.


##  <a name="portable-class-librariescross-platformapp-fundamentalspclmd"></a>[Biblioteki klas przenośnych](~/cross-platform/app-fundamentals/pcl.md)

Projektów biblioteki klas przenośnych umożliwiają tworzenie i dystrybucji zestawów zawierających udostępniony kod wymagany do uruchomienia na wielu platformach. Tworzenie przenośnej biblioteki klas (lub "PCL") najpierw wybrać platformy, na których docelowego, a następnie pisać kod dla podzbioru .NET Framework, która jest dostępna w profilu zdefiniowane dla tych platform. Ten dokument zawiera opis sposobu tworzenia i używania PCLs za pomocą platformy Xamarin.

##  <a name="shared-projectscross-platformapp-fundamentalsshared-projectsmd"></a>[Projekty udostępnione](~/cross-platform/app-fundamentals/shared-projects.md)

Udostępnionych projektów umożliwiają pisanie wspólnej kodu, który odwołuje się wiele projektów różnych aplikacji. Kod jest skompilowana jako część każdego odwołaniem do projektu i może zawierać dyrektywy kompilatora, aby obsługiwać funkcje specyficzne dla platformy w podstawowym udostępnionego kodu. W tym artykule omówiono sposób działania udostępnionych projektów i jak utworzyć i używać go z projektami Xamarin.

##  <a name="net-standardcross-platformapp-fundamentalsnet-standardmd"></a>[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)

.NET standard to nowa opcja udostępniania kodu na platformach. Działa w sposób podobny do bibliotek klas przenośnych; Kod utworzono przy użyciu określonej wersji (obecnie 1.0 za pośrednictwem 1.6), a następnie mogą być wykorzystane przez inne projekty, które obsługuje tego poziomu lub nowszej. Projekty platformy .NET standard są obsługiwane w Xamarin Studio 6.2, Visual Studio dla systemu Windows i programu Visual Studio dla komputerów Mac.

##  <a name="nuget-projects-multiplatform-libraries-for-code-sharingcross-platformapp-fundamentalsnuget-multiplatform-librariesindexmd"></a>[NuGet projekty: Biblioteki dla wielu platform udostępnianie kodu](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)

Pakiety NuGet, które mogą być automatycznie generowane przez PCL lub platformy .NET standard projektów; i udostępnionych projektów można spakować do pakietów NuGet "przynęta i przełącznik" przy użyciu osobnych typu projektu NuGet. W tej sekcji opisano sposób tworzenia pakietów NuGet dla każdego scenariusza udostępniania kodu.

##  <a name="manually-creating-nuget-packages-for-xamarincross-platformapp-fundamentalsnuget-manualmd"></a>[Ręczne tworzenie pakietów NuGet dla platformy Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md)

Wskazówki dotyczące tworzenia pakietów NuGet, które współpracują z platformą Xamarin.

##  <a name="cross-platform-data-accessxamarin-formsdata-cloudindexmd"></a>[Cross Platform dostępu do danych](~/xamarin-forms/data-cloud/index.md)

Większość aplikacji ma pewne wymagania w celu zapisywania danych na urządzeniu lokalnie. Jeśli nie jest trivially małe ilości danych, zwykle wymaga bazę danych i warstwy danych w aplikacji do zarządzania dostępem do bazy danych. iOS i Android mają aparat bazy danych SQLite "wbudowane" i dostęp do przechowywania i pobierania danych zostało uproszczone dzięki platformie Xamarin w. [Dostępu do danych w systemie Android](~/android/data-cloud/data-access/index.md), [dostęp do danych z systemem iOS](~/ios/data-cloud/data/index.md), i [dostęp do danych platformy Xamarin.Forms](~/xamarin-forms/data-cloud/index.md) przewodniki zawierają przykłady dotyczące dostępu SQLite na każdej z platform.


##  <a name="transport-layer-securitytransport-layer-securitymd"></a>[Transport Layer Security](transport-layer-security.md)

Informacje na temat protokołu SSL/TLS selectingthe implementacją zabezpieczyć połączenie sieciowe z aplikacji.


##  <a name="notificationsxamarin-formsdata-cloudpush-notificationsindexmd"></a>[Powiadomienia](~/xamarin-forms/data-cloud/push-notifications/index.md)

Aplikacje mobilne Użyj powiadomień w sposób dyskretny kod informowania użytkownika, dla którego wykonano niektórych zdarzeń określonych aplikacji. Powiadomienia są zazwyczaj używane w celu powiadomienia użytkownika o stanie procesu aplikacji, która działa w tle. Na przykład może pobierać dużych plików. Ten plik może zająć dużo czasu do pobrania, więc to działanie w przypadku wystąpienia w tle. Po zakończeniu pobierania użytkownik jest informowany faktu przez powiadomienie.
Ponadto powiadomienia arach nie on ograniczony do aplikacji lokalnych. Istnieje również możliwość dla aplikacji serwera opublikować powiadomienia do aplikacji dla urządzeń przenośnych. W tym artykule przedstawimy jak używać powiadomień na Android i iOS.
