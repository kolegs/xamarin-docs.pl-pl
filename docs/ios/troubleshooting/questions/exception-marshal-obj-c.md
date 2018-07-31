---
title: 'Dlaczego moja aplikacja systemu iOS 9 powiedzie z: System.Exception: Marshalingu obiektu języka Objective-C?'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8805ABEC-48D4-4CCB-A226-3A5B2ECE4BF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 93666fcb39f0cd717c14eb07e6407801e9f0642e
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350602"
---
# <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-object"></a>Dlaczego moja aplikacja systemu iOS 9 powiedzie z: System.Exception: Marshalingu obiektu języka Objective-C?

Może zostać wyświetlony błąd następującą postać:

> System.Exception: Nie można skierować obiektu języka Objective-C... Nie można odnaleźć istniejącego wystąpienia zarządzanego dla tego obiektu...

Zmiany interfejsu API w systemu iOS 9 wymagają, że Konstruktor wywołanie zwrotne używane podczas wywoływania niezarządzanego kodu jako podstawowy interfejs API teraz oczekuje. Aby dodać wywołanie zwrotne konstruktora do klasy, należy użyć następującego: 

`public foo (IntPtr handle) : base (handle) ` 

### <a name="next-steps"></a>Następne kroki

Aby uzyskać dalszą pomoc, skontaktuj się z nami lub jeśli ten problem pozostaje nawet po zakończeniu wykorzystujących powyższe informacje, zobacz [jakie opcje pomocy technicznej są dostępne dla programu Xamarin?](~/cross-platform/troubleshooting/support-options.md) uzyskać informacji na temat opcji kontaktu, sugestie, a także jak Zgłoś usterkę nowy, jeśli to konieczne. 
