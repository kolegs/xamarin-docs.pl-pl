---
title: 'Dlaczego moja aplikacja systemu iOS 9 powiedzie z: System.Exception: nie można skierować obiektu języka Objective-C?'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8805ABEC-48D4-4CCB-A226-3A5B2ECE4BF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: f7382ac963249a3f3646a917d8700e3a12873ec9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30775978"
---
# <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-object"></a>Dlaczego moja aplikacja systemu iOS 9 powiedzie z: System.Exception: nie można skierować obiektu języka Objective-C?

Może zostać wyświetlony błąd formularza:

> System.Exception: Nie można skierować obiektu języka Objective-C... Nie można odnaleźć istniejącego wystąpienia zarządzanego dla tego obiektu...

Zmiany interfejsu API dla systemu iOS 9 wymagają, że Konstruktor wywołania zwrotnego używane podczas wywoływania kodu niezarządzanego, jako podstawowy interfejs API teraz oczekuje. Aby dodać wywołanie zwrotne konstruktora do klasy, należy użyć następującego: 

`public foo (IntPtr handle) : base (handle) ` 

### <a name="next-steps"></a>Następne kroki

Aby uzyskać dodatkową pomoc, skontaktuj się z nami, lub jeśli ten problem pozostanie nawet po użyciu powyższe informacje, zobacz [jakie opcje pomocy technicznej są dostępne dla platformy Xamarin?](~/cross-platform/troubleshooting/support-options.md) informacji o opcjach kontaktu, masz sugestie, oraz jak Plik nowe usterki, w razie potrzeby. 
