---
title: "Dlaczego moja aplikacja systemu iOS 9 powiedzie z: System.Exception: nie można skierować obiektu języka Objective-C?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8805ABEC-48D4-4CCB-A226-3A5B2ECE4BF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: baba2526eefa1b69d47da47b73ea0bd417ecdc57
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-object"></a>Dlaczego moja aplikacja systemu iOS 9 powiedzie z: System.Exception: nie można skierować obiektu języka Objective-C?

Może zostać wyświetlony błąd formularza:

> System.Exception: Nie można skierować obiektu języka Objective-C... Nie można odnaleźć istniejącego wystąpienia zarządzanego dla tego obiektu...

Zmiany interfejsu API dla systemu iOS 9 wymagają, że Konstruktor wywołania zwrotnego używane podczas wywoływania kodu niezarządzanego, jako podstawowy interfejs API teraz oczekuje. Aby dodać wywołanie zwrotne konstruktora do klasy, należy użyć następującego: 

`public foo (IntPtr handle) : base (handle) ` 

### <a name="next-steps"></a>Następne kroki

Aby uzyskać dodatkową pomoc, skontaktuj się z nami, lub jeśli ten problem pozostanie nawet po użyciu powyższe informacje, zobacz [jakie opcje pomocy technicznej są dostępne dla platformy Xamarin?](~/cross-platform/troubleshooting/support-options.md) informacji o opcjach kontaktu, masz sugestie, oraz jak Plik nowe usterki, w razie potrzeby. 
