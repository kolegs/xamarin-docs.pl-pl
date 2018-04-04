---
title: Instrukcje dotyczące konfigurowania zapory Xamarin
description: Lista hostów, które należy do dozwolonych w zaporze zezwolić platformy Xamarin w pracy w firmie.
ms.prod: xamarin
ms.assetid: 658f699b-8cca-48f7-ae54-fa956384b6d6
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: bb551b548f241cacfc4cb700d247684c15f6fcf7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="xamarin-firewall-configuration-instructions"></a>Instrukcje dotyczące konfigurowania zapory Xamarin

_Lista hostów, które należy do dozwolonych w zaporze zezwolić platformy Xamarin w pracy w firmie._

Aby Xamarin produktów do zainstalowania i działają prawidłowo niektóre punkty końcowe muszą być dostępne dla pobrać wymagane narzędzia i aktualizacje oprogramowania. Jeśli użytkownik lub jego firma ustawień zapory strict, mogą wystąpić problemy z instalacji licencji, składników i więcej. W tym dokumencie przedstawiono niektóre znane punktów końcowych, które muszą być białej w zaporze, aby program Xamarin, aby pracować. Ta lista nie zawiera punktów końcowych wymaganych dla narzędzi innych firm do pobrania. Jeśli nadal występują problemy z po przejściu do tej listy, zajrzyj do firmy Apple lub instalacji Android wskazówki dotyczące rozwiązywania problemów.

## <a name="endpoints-to-whitelist"></a>Punkty końcowe listą dozwolonych adresów IP

### <a name="xamarin-installer"></a>Xamarin Installer

Następujące znane adresy potrzebne do dodania w kolejności dla oprogramowania do prawidłowej instalacji, korzystając z najnowszej wersji Instalatora Xamarin:

-  xamarin.com (Instalator manifestów)
-  DL.xamarin.com (Lokalizacja pobierania pakietu)
-  DL.Google.com (do pobrania zestawu SDK systemu Android)
-  Download.Oracle.com (JDK)
-  Visual Studio — (Lokalizacja pobierania pakiety instalacyjne)
-  go.microsoft.com (rozpoznawania adresu URL instalacji)
-  aka.MS (rozpoznawania adresu URL instalacji)

Jeśli używasz Mac i pojawiły się problemy dotyczące instalacji platformy Xamarin.Android, upewnij się, że ten system macOS jest w stanie pobrać Java.


### <a name="components-store-and-nuget-including-xamarinforms"></a>Składniki magazynu i NuGet (w tym platformy Xamarin.Forms)

Następujące adresy należy do dodania do uzyskania dostępu do magazynu składnik Xamarin lub NuGet (platformy Xamarin.Forms jest dostarczana jako NuGet):

-  Components.xamarin.com (aby używał magazynu składników Xamarin)
-  xampubdl.blob.Core.Windows.NET (pobieranie składników magazynu hostów)
-  www.nuget.org (Aby uzyskać dostęp do NuGet)
-  az320820.vo.msecnd.net (NuGet downloads)
-  DL-ssl.google.com (składniki Google)


### <a name="software-updates"></a>Aktualizacje oprogramowania

Następujące adresy potrzebne do dodania do zapewnienia prawidłowo pobierania aktualizacji oprogramowania:

-  Software.xamarin.com (usługa updater)
-  download.visualstudio.microsoft.com
-  dl.xamarin.com

## <a name="xamarin-mac-agent"></a>Xamarin Mac Agent

Do nawiązania połączenia Mac kompilacji programu Visual Studio hosta za pomocą platformy Xamarin Mac Agent wymaga otwarty port SSH. Domyślnie jest to **Port 22**.

## <a name="summary"></a>Podsumowanie

W tym przewodniku opisano punktów końcowych do listy dozwolonych, aby umożliwić Xamarin produktów do instalacji i aktualizacji prawidłowo na tym komputerze.