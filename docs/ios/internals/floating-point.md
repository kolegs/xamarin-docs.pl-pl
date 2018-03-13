---
title: Liczba zmiennoprzecinkowa
ms.topic: article
ms.prod: xamarin
ms.assetid: 003F25C1-B430-4339-9C95-7DF527EBC699
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 73681a37f15f3dd93c85bafb6bb9d71ab30af85c
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="floating-point"></a>Liczba zmiennoprzecinkowa

Domyślnie Xamarin.iOS wykona 32-bitowe i 64-bitowe operacje zmiennoprzecinkowe przy użyciu 64-bitowa precyzja na ARM.  

Gdy to większą dokładność jest bliżej deweloperzy oczekiwać w przypadku operacji zmiennoprzecinkowych w języku C# na pulpicie na urządzeń przenośnych, ich wpływ na wydajność może mieć znaczący.

Istnieje możliwość skompilować 32-bitowych zmiennoprzecinkowych punktu kod, aby używał operacje zmiennoprzecinkowe 32-bitowych.  Aby to zrobić, należy użyć co najmniej Xamarin.iOS 8.10 i zestawu w dla systemu iOS rozbudowywać opcje w panelu "mtouch dodatkowe argumenty" wpis wierszu następującą wartość:

     --aot-options=-O=float32

To poinformuje kompilatory statyczne (Mono przez wbudowane kompilatora statycznych lub zasilane LLVM jeden) można wykonywać operacje zmiennoprzecinkowe przy użyciu 32-bitowych liczb zmiennoprzecinkowych.
