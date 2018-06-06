---
title: Interfejsy użytkownika w systemie iOS
description: Ten dokument zawiera łącza do przewodników zawierających opis sposobu tworzenia interfejsów użytkownika w aplikacji platformy Xamarin.iOS. Przewodniki połączonego obejmuje API wygląd, tworzenia obiektów interfejsu użytkownika, opcje układu i inne.
ms.prod: xamarin
ms.assetid: 1BB46561-F503-491E-A27C-7878E7EBE00B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: a51d3f57106a282ed72b45dedf356739244e247f
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790370"
---
# <a name="user-interfaces-in-ios"></a>Interfejsy użytkownika w systemie iOS

## <a name="appearance-apiintroduction-to-the-appearance-apimd"></a>[Interfejs API wyglądu](introduction-to-the-appearance-api.md)

iOS umożliwia wiele atrybutów visual formantów interfejsu użytkownika w celu można utworzyć motywu, za pomocą interfejsów API UIAppearance.

## <a name="creating-user-interface-objectsiosuser-interfaceios-uicreating-ui-objectsmd"></a>[Tworzenie obiektów interfejsu użytkownika](~/ios/user-interface/ios-ui/creating-ui-objects.md)

Apple grupuje pokrewne elementy funkcji do "platformy", które są równoważne Xamarin.iOS przestrzeni nazw. `UIKit` jest przestrzenią nazw, który zawiera wszystkie kontrolki interfejsu użytkownika dla systemu iOS.

## <a name="layout-optionsiosuser-interfaceios-uilayout-optionsmd"></a>[Opcje układu](~/ios/user-interface/ios-ui/layout-options.md)

Istnieją dwa mechanizmy kontroli układ, gdy widok jest obracana: automatyczna zmiana rozmiaru i Autolayout.

## <a name="providing-haptic-feedbackiosuser-interfaceios-uihaptic-feedbackmd"></a>[Przekazywanie opinii dotykowych](~/ios/user-interface/ios-ui/haptic-feedback.md)

W tym artykule omówiono nowe typy dotykowych opinii, dostępnych w iOS 10 oraz sposób ich wdrażania w platformy Xamarin.iOS.

## <a name="working-with-the-ui-threadiosuser-interfaceios-uiui-threadmd"></a>[Praca z wątkiem interfejsu użytkownika](~/ios/user-interface/ios-ui/ui-thread.md)

Kod należy wykonać tylko wątku zmiany formantów interfejsu z głównej użytkownika (lub interfejsu użytkownika). Wszelkie aktualizacje interfejsu użytkownika, które występują w innym wątku (np. wątek wywołania zwrotnego lub w tle) nie może pobrać wyświetlony na ekranie lub nawet może spowodować awarię.




