---
title: Operacji zmiennoprzecinkowych w Xamarin.iOS
description: Ten dokument w tym artykule opisano, jak Xamarin.iOS obsługuje 32-bitowe i 64-bitowe dokładności operacji zmiennoprzecinkowych i omówiono skojarzone wpływ na wydajność.
ms.prod: xamarin
ms.assetid: 003F25C1-B430-4339-9C95-7DF527EBC699
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ea5d69b52cbd4c76abb236bd1a272633dde440b7
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786164"
---
# <a name="floating-point-operations-in-xamarinios"></a>Operacji zmiennoprzecinkowych w Xamarin.iOS

Domyślnie Xamarin.iOS wykona 32-bitowe i 64-bitowe operacje zmiennoprzecinkowe przy użyciu 64-bitowa precyzja na ARM.  

Gdy to większą dokładność jest bliżej deweloperzy oczekiwać w przypadku operacji zmiennoprzecinkowych w języku C# na pulpicie na urządzeń przenośnych, ich wpływ na wydajność może mieć znaczący.

Istnieje możliwość skompilować 32-bitowych zmiennoprzecinkowych punktu kod, aby używał operacje zmiennoprzecinkowe 32-bitowych.  Aby to zrobić, należy użyć co najmniej Xamarin.iOS 8.10 i zestawu w dla systemu iOS rozbudowywać opcje w panelu "mtouch dodatkowe argumenty" wpis wierszu następującą wartość:

     --aot-options=-O=float32

To poinformuje kompilatory statyczne (Mono przez wbudowane kompilatora statycznych lub zasilane LLVM jeden) można wykonywać operacje zmiennoprzecinkowe przy użyciu 32-bitowych liczb zmiennoprzecinkowych.
