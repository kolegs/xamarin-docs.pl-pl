---
title: Debugowanie na urządzeniu
description: W tym artykule wyjaśniono, jak można debugować aplikacji platformy Xamarin.Android na urządzeniu fizycznym z systemem Android.
ms.prod: xamarin
ms.assetid: 153D3746-A27F-198B-48FE-D219C0133A79
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 1848bb624bf5f4bd627441a17fd077843c94edb9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="debug-on-device"></a>Debugowanie na urządzeniu

_W tym artykule wyjaśniono, jak można debugować aplikacji platformy Xamarin.Android na urządzeniu fizycznym z systemem Android._

## <a name="debug-on-device-overview"></a>Debugowanie na urządzeniu — omówienie

Istnieje możliwość debugowania aplikacji platformy Xamarin.Android na urządzeniu z systemem Android przy użyciu programu Visual Studio for Mac lub Visual Studio. Przed debugowania mogą wystąpić na urządzeniu, musi być on [ustawień dla rozwoju](~/android/get-started/installation/set-up-device-for-development.md) i podłączone do komputera lub komputerów Mac.


## <a name="debug-application"></a>Debugowanie aplikacji

Gdy urządzenie jest podłączone do komputera, debugowanie aplikacji platformy Xamarin.Android odbywa się w taki sam sposób jak inne Xamarin produktu lub aplikacji .NET. Upewnij się, że **debugowania** konfiguracji i zewnętrzne urządzenie jest zaznaczona w IDE, daje to pewność, że symboli debugowania konieczne są dostępne i czy IDE połączenie działającej aplikacji: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Wybrana konfiguracja debugowania](debug-on-device-images/image1-vs.png)

Następnie punkt przerwania jest ustawiony w kodzie:

![Punkt przerwania ustawiony w wierszu kodu](debug-on-device-images/image2-vs.png)

Po wybraniu urządzenia platformy Xamarin.Android będzie nawiąż połączenie z urządzeniem, Wdróż aplikację, a następnie uruchom go. Po osiągnięciu punktu przerwania debugera przestanie aplikacji, umożliwiając aplikacji do debugowania w sposób podobny do żadnych innych aplikacji C#: 

![Osiągnięto punktu przerwania](debug-on-device-images/image3-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Wybrana konfiguracja debugowania](debug-on-device-images/image1-xs.png)

Następnie punkt przerwania jest ustawiony w kodzie:

![Punkt przerwania ustawiony w wierszu kodu](debug-on-device-images/image2-xs.png)

Po wybraniu urządzenia platformy Xamarin.Android będzie nawiąż połączenie z urządzeniem, Wdróż aplikację, a następnie uruchom go. Po osiągnięciu punktu przerwania debugera przestanie aplikacji, umożliwiając aplikacji do debugowania w sposób podobny do żadnych innych aplikacji C#: 

![Osiągnięto punktu przerwania](debug-on-device-images/image3-xs.png)

-----



## <a name="summary"></a>Podsumowanie

W tym dokumencie opisano sposób debugowania aplikacji platformy Xamarin.Android ustawienia punktu przerwania i wybierając urządzenia docelowego.


## <a name="related-links"></a>Linki pokrewne

- [Konfigurowanie urządzenia na potrzeby programowania](~/android/get-started/installation/set-up-device-for-development.md)
- [Ustawienie atrybutu możliwością debugowania](~/android/deploy-test/debuggable-attribute.md)
