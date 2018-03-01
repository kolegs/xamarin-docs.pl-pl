---
title: "Błąd kompilacji systemu android — zadanie LinkAssemblies nieoczekiwanie nie powiodło się"
ms.topic: article
ms.prod: xamarin
ms.assetid: EB3BE685-CB72-48E3-89D7-C845E76B9FA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 667381e7f27f08f8b1d8b6c1979a7d0b26775177
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="android-build-error--the-linkassemblies-task-failed-unexpectedly"></a>Błąd kompilacji systemu android — zadanie LinkAssemblies nieoczekiwanie nie powiodło się

Zobaczysz komunikat o błędzie `The "LinkAssemblies" task failed unexpectedly` podczas kompilowania projektu platformy Xamarin.Android, który używa formularzy. Dzieje się tak podczas konsolidator jest aktywne (zwykle na *wersji* kompilacji, aby zmniejszyć rozmiar pakietu aplikacji); i występuje, ponieważ docelowych z systemem Android nie są zaktualizowane do najnowszej framework. (Więcej informacji: [platformy Xamarin.Forms dla systemu Android wymagania](~/xamarin-forms/get-started/installation.md#android))

Rozwiązanie tego problemu jest upewnienie się, ma najnowszych obsługiwanych wersji zestawu SDK systemu Android, a ustawiona **platformy docelowej** do **Użyj zainstalowana najnowsza wersja platformy**. Zalecane jest również, że ustawisz **docelową wersję systemu Android** do **wersji docelowej Framework** i **minimalna wersja systemu Android** do interfejsu API 15 lub nowszym. To jest traktowany jako obsługiwanej konfiguracji.

## <a name="setting-in-visual-studio-for-mac"></a>Ustawienie w programie Visual Studio dla komputerów Mac

1.  Kliknij prawym przyciskiem myszy projekt Android.
2.  Przejdź do **kompilacji > Ogólne > platformy docelowej**.
3.  Ustaw **platformy docelowej: Użyj zainstalowana najnowsza wersja platformy**.
4.  Nadal opcje projektu, przejdź do **kompilacji > aplikacji systemu Android**.
5.  Ustaw **Minimum Android wersji** poziom interfejsu API 15 lub nowszej & **wersji docelowej Android** do **automatyczny — Użyj docelowej framework w wersji**.

## <a name="setting-in-visual-studio"></a>Ustawienie w programie Visual Studio

1.  Kliknij prawym przyciskiem myszy projekt Android.
2.  Przejdź do **aplikacji** w opcji projektu.
3.  Ustaw **skompilować przy użyciu wersji dla systemu Android** & **wersji docelowej Android** ustawienia, aby **użyj najnowszej platformy** / **użycia Kompilowanie przy użyciu zestawu SDK w wersji**.
4.  Ustaw **Minimum Android do docelowego** ustawienie do interfejsu API 15 lub nowszym.

Po zaktualizowaniu tych ustawień, czyszczenia i ponownie skompiluj projekt, aby upewnić się, że zmiany są pobierane.
