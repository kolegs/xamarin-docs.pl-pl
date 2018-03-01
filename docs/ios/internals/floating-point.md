---
title: Liczba zmiennoprzecinkowa
ms.topic: article
ms.prod: xamarin
ms.assetid: BE4EE969-C075-4B9A-8465-E393556D8D90
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ea29132ad4ac55f6fb151ac2125ab1add82c8518
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="floating-point"></a>Liczba zmiennoprzecinkowa

Domyślnie Xamarin.iOS wykona 32-bitowe i 64-bitowe operacje zmiennoprzecinkowe przy użyciu 64-bitowa precyzja na ARM.  

Gdy to większą dokładność jest bliżej deweloperzy oczekiwać w przypadku operacji zmiennoprzecinkowych w języku C# na pulpicie na urządzeń przenośnych, ich wpływ na wydajność może mieć znaczący.

Istnieje możliwość skompilować 32-bitowych zmiennoprzecinkowych punktu kod, aby używał operacje zmiennoprzecinkowe 32-bitowych.  Aby to zrobić, należy użyć co najmniej Xamarin.iOS 8.10 i zestawu w dla systemu iOS rozbudowywać opcje w panelu "mtouch dodatkowe argumenty" wpis wierszu następującą wartość:

     --aot-options=-O=float32

To poinformuje kompilatory statyczne (Mono przez wbudowane kompilatora statycznych lub zasilane LLVM jeden) można wykonywać operacje zmiennoprzecinkowe przy użyciu 32-bitowych liczb zmiennoprzecinkowych.
