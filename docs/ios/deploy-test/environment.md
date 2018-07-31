---
title: Środowisko wykonywania dla aplikacji platformy Xamarin.iOS
description: W tym dokumencie opisano, jak skonfigurować zmienne środowiskowe tymczasowe i stałe dla aplikacji platformy Xamarin.iOS. Zmienne można określić we właściwościach projektu lub jako dodatkowe argumenty mtouch narzędzia pakowania.
ms.prod: xamarin
ms.assetid: 9801644A-89BB-4491-AD28-7F3B97D2CD62
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2017
ms.openlocfilehash: 5296f03cae28e1025c760004c520a2b415ec493d
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351590"
---
# <a name="execution-environment-for-xamarinios-apps"></a>Środowisko wykonywania dla aplikacji platformy Xamarin.iOS

*Środowiska wykonawczego* to zbiór zmiennych środowiskowych, które wpływają na działanie programu. Zmienne środowiskowe można ustawić tymczasowo we właściwościach projektu lub trwałe, określając dodatkowe argumenty mtouch narzędzia pakowania.

## <a name="temporary-environment-variables"></a>Zmienne środowiskowe tymczasowego

Zmienne środowiskowe tymczasowe są ustawiane w projekcie **właściwości**/**opcje** okna **Uruchom > Ogólne** sekcji. Zmienne środowiskowe są tylko wtedy, gdy aplikacja jest uruchamiana przy użyciu programu Visual Studio dla komputerów Mac, jeśli aplikacja jest uruchomiona ręcznie, naciskając go, których te zmienne środowiskowe nie są ustawione.

## <a name="permanent-environment-variables"></a>Zmienne środowiskowe stałych

Zmienne środowiskowe stałe są ustawiane przez określenie dodatkowe argumenty mtouch narzędzia pakowania. Te zmienne środowiskowe są kompilowane do pliku wykonywalnego i zostanie ustawiona, nawet wtedy, gdy aplikacja nie jest uruchamiana z programu Visual Studio dla komputerów Mac.

## <a name="example"></a>Przykład

```csharp
# log all exceptions to the device log
--setenv:MONO_TRACE=E:all

# to pass multipe environment variables, use --setenv multiple times
--setenv:MONO_TRACE=E:all --setenv:GC_DONT_GC=1
```

