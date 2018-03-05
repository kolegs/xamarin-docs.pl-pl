---
title: "Dlaczego mojego projektu Xamarin.Forms.Maps Android nie z powodu NIEOCZEKIWANEGO błędu najwyższego poziomu COMPILETODALVIK?"
ms.topic: article
ms.prod: xamarin
ms.assetid: C0251EB1-F509-47AD-98D6-846AF46425E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 41cb955ba22a83d0ae2a11ac5c5a114b465ca9b2
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik-unexpected-top-level-error"></a>Dlaczego mojego projektu Xamarin.Forms.Maps Android nie z powodu NIEOCZEKIWANEGO błędu najwyższego poziomu COMPILETODALVIK?

Tego błędu mogą być widoczne w konsoli błąd programu Visual Studio dla komputerów Mac lub w oknie wyników kompilacji programu Visual Studio; w projektach systemu Android przy użyciu Xamarin.Forms.Maps.

Jest to najczęściej rozwiązane przez zwiększenie rozmiaru sterty Java w projekcie platformy Xamarin.Android. Wykonaj następujące kroki, aby zwiększyć rozmiar stosu:

## <a name="visual-studio"></a>Visual Studio

1. Kliknij prawym przyciskiem myszy projekt Android & Otwórz opcje projektu.
2. Przejdź do **Android Opcje -> Zaawansowane**
3. W polu tekstowym Rozmiar sterty Java wprowadź 1 GB/S.
4. Skompiluj ponownie projekt.

![Zrzut ekranu przedstawiający opcje projektu programu Visual Studio](maps-compiletodalvik-error-images/vsjavaheap.png "opcje programu Visual Studio kompilacji systemu Android")

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1.  Kliknij prawym przyciskiem myszy projekt Android & Otwórz opcje projektu.
2.  Przejdź do **kompilacji -> Android kompilacji -> Zaawansowane**
3.  W polu tekstowym Rozmiar sterty Java wprowadź 1 GB/S.
4.  Skompiluj ponownie projekt.  

![Zrzut ekranu przedstawiający programu Visual Studio for Mac opcje projektu](maps-compiletodalvik-error-images/xsjavaheap.png "opcje programu Visual Studio for Mac kompilacji systemu Android")

