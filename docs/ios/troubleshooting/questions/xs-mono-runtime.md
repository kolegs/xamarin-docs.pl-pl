---
title: Jak ustawić zmienne środowiskowe Mono środowiska uruchomieniowego dla projektów dla systemu iOS w programie Xamarin Studio?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1176CEA9-C7F1-411B-8F1A-99374E8AFF33
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/31/2017
ms.openlocfilehash: 5f4f3a2de012d35ddca9c1fa830d599d9d5acb17
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30776121"
---
# <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studio"></a>Jak ustawić zmienne środowiskowe Mono środowiska uruchomieniowego dla projektów dla systemu iOS w programie Xamarin Studio?

Jeśli musisz ustawić zmienne środowiskowe dowolnego środowiska uruchomieniowego dla Mono można można ustawić w **opcje projektu > Uruchom > Ogólne** strony.

Uwaga: Wyrzucanie elementów bezużytecznych zmiennych środowiskowych SGen (MONO\_GC\_PARAMS) zestawu dzięki temu będzie można używać tylko podczas uruchamiania za pomocą platformy Xamarin Studio. W przypadku uruchamiania aplikacji z urządzenia, ustawienia Sgen zostaną zignorowane. 

Aby trwale ustawić wartość zmiennej środowiskowej dla aplikacji, należy dodać to jako argument dodatkowe mtouch (dla wszystkich odpowiednich konfiguracji):

```csharp
   --setenv=NAME=VALUE
```

Aby wyświetlić zmiennych środowiskowych, które można ustawić, można znaleźć na stronie Mono man: [ http://docs.go-mono.com/?link=man%3amono(1) ](http://docs.go-mono.com/?link=man%3amono(1)) Zobacz sekcji: `ENVIRONMENT VARIABLES`

![](xs-mono-runtime-images/environment-variables.jpg "Ustawianie zmiennych środowiskowych dla projektu")
