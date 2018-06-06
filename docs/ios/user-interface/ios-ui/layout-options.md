---
title: Opcje układu w Xamarin.iOS
description: W tym dokumencie opisano różne sposoby układ interfejsu użytkownika w Xamarin.iOS. Zawarto informacje automatyczna zmiana rozmiaru i układu automatycznego.
ms.prod: xamarin
ms.assetid: D8180FEC-F300-42C0-B029-66803E0C1A5F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: bad29eae308c8ca9f7228a1cbdfd69940894cf34
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790119"
---
# <a name="layout-options-in-xamarinios"></a>Opcje układu w Xamarin.iOS

Istnieją dwa mechanizmy kontroli układ, gdy widok jest obracana:

-  **Automatyczna zmiana rozmiaru** — inspektora automatyczna zmiana rozmiaru w Projektancie pozwala ustawić `AutoresizingMask` właściwości. Umożliwi to formant jest zakotwiczony krawędziami kontenera ich i/lub usuń ich rozmiar. Automatyczna zmiana rozmiaru działa we wszystkich wersjach systemu IOS. Jest to opisane bardziej szczegółowo poniżej
-  **Automatyczny układ** — funkcja wprowadzona w systemie iOS 6, który umożliwia szczegółową kontrolę nad relacje kontrolek interfejsu użytkownika. Umożliwia kontrolę nad położenia elementów do innych elementów na powierzchni projektu. W tym temacie omówiono bardziej szczegółowo w [układ automatyczny z Xamarin iOS projektanta](~/ios/user-interface/designer/designer-auto-layout.md) przewodnik.

## <a name="autosizing"></a>Automatyczna zmiana rozmiaru

Gdy użytkownik zmienia rozmiar okna, takie jak kiedy urządzenie jest obracana i zmiany orientacji system będzie automatycznie zmieniać rozmiar widoków wewnątrz tego okna, zgodnie z regułami ich automatyczna zmiana rozmiaru. Te reguły można ustawić w języku C# przy użyciu `AutoresizingMask` właściwość `UIView` lub **konsoli właściwości** systemu IOS w projektancie, jak przedstawiono poniżej:

 [![](layout-options-images/image41.png "Visual Studio for Mac projektanta")](layout-options-images/image41.png#lightbox)

Gdy jest wybrana kontrolka dzięki temu można ręcznie określić lokalizację i wymiarów kontrolki, a także wybór **automatyczna zmiana rozmiaru** zachowanie. Jak pokazano na poniższym zrzucie ekranu, możemy użyć w formancie automatyczna zmiana rozmiaru sprężyny i poprzeczne do definiowania relacji wybranego widoku do jego obiektu nadrzędnego:

 [![](layout-options-images/image42.png "Visual Studio for Mac projektanta")](layout-options-images/image42.png#lightbox)

Dopasowywanie *spring* spowoduje, że widok, aby zmienić rozmiar na podstawie szerokości lub wysokości widoku nadrzędnym. Dopasowywanie *strut* spowoduje, że obsługa stałą odległość między sobą i widoku nadrzędnym, w szczególności krawędzi widoku.

Te ustawienia można również ustawić w kodzie:

```csharp
textfield1.Frame = new RectangleF(15, 277, 79, 27);
textfield1.AutoresizingMask = UIViewAutoresizing.FlexibleRightMargin | UIViewAutoresizing.FlexibleBottomMargin;
```


Aby przetestować ustawienia Automatyczna zmiana rozmiaru, Włącz różnych **orientacji urządzenia obsługiwane** opcje projektu:

 [![](layout-options-images/image43a.png "Ustawienia automatyczna zmiana rozmiaru")](layout-options-images/image43a.png#lightbox)

W kodzie możemy użyć poniższego kodu, co powoduje, że dwa formantami zmiany rozmiaru w poziomie:

```csharp
textview1.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
textfield1.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
imageview1.AutoresizingMask = UIViewAutoresizing.FlexibleTopMargin | UIViewAutoresizing.FlexibleLeftMargin;
```


Możemy również dostosować formantów za pomocą projektanta. Wybieranie poprzeczne jako wystawiane poniżej spowoduje, że obraz, aby pozostać wyrównany bez przycinania u dołu widoku:

 [![](layout-options-images/autoresize.png "AutoRotation")](layout-options-images/autoresize.png#lightbox)

Te zrzuty ekranu pokazują, jak formanty Zmienianie rozmiaru lub położenia sobie, podczas obracania ekranu:

 [![](layout-options-images/image44a.png "AutoRotation")](layout-options-images/image44a.png#lightbox)

Widok tekstu i pola tekstowego zarówno rozciągają się, aby używać tego samego pozostałych natomiast prawy margines, ze względu na `FlexibleWidth` ustawienie. Obraz ma wartość górnego i lewego marginesu elastyczne, co oznacza, że zachowanie dolnej i prawego marginesu — mając na uwadze obrazu podczas obracania ekranu. Układy złożonych zwykle wymagają kombinacji tych ustawień na każdego formantu widoczne, aby zapewnić spójny interfejs użytkownika i zapobiec formantów z nakładającymi się granicami widoku zmiany (z powodu obrotu bądź inne zdarzenie zmiany rozmiaru).





## <a name="related-links"></a>Linki pokrewne

- [Formanty (przykład)](https://developer.xamarin.com/samples/Controls/)
