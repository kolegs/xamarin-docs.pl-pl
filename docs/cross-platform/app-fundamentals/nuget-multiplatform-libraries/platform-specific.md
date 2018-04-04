---
title: Tworzenie nowych projektów biblioteki specyficzne dla platformy programu NuGet
ms.prod: xamarin
ms.assetid: D8BC4906-805F-4AFB-8D1A-88B7BF87E17F
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 0f244e614a40e444139d51a9466ccc7225a7fe68
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="creating-new-platform-specific-library-projects-for-nuget"></a>Tworzenie nowych projektów biblioteki specyficzne dla platformy programu NuGet

Dla wielu platform projektów bibliotek przeznaczonych dla określonych platform, takich jak systemy iOS i Android, najlepiej z udostępnionych projektów.

NuGet może zawierać zarówno kod specyficznych dla systemu iOS i Android, jak i wspólne dla kodu platformy .NET.

Wiele zestawów są tworzone i wbudowane w jednym pakiecie NuGet. Standardy NuGet upewnij się, że pakiet mogą być dodawane do wszystkich typów obsługiwanych projektu, takie jak Xamarin iOS i Android projektów.

## <a name="steps-to-create-a-cross-platform-library-nuget"></a>Kroki umożliwiające utworzenie NuGet biblioteki i Platform

1. Wybierz **Plik > nowe rozwiązanie** (lub istniejącego rozwiązania kliknij prawym przyciskiem myszy i wybierz polecenie **Dodaj > Nowy projekt**).

2. Wybierz **biblioteki Multiplatform** z **Multiplatform > biblioteki** sekcji:

  [![](platform-specific-images/mulitplatform-library-sml.png "Konfigurowanie biblioteki obejmującego wiele platform dla podstawy pojedynczy kod")](platform-specific-images/multiplatform-library.png#lightbox)

3. Wprowadź **nazwa** i **opis**i wybierz polecenie **określonych Platform**:

  [![](platform-specific-images/specific-configure-sml.png "Konfigurowanie biblioteki specyficzne dla platformy dla systemów iOS i Android")](platform-specific-images/specific-configure.png#lightbox)

4. Ukończ pracę kreatora. Następujące projekty są dodane do rozwiązania:

  - **Projekt systemu android** — Opcjonalnie można dodać kod specyficzne dla systemu Android do tego projektu.
  - **iOS projektu** — Opcjonalnie można dodać kod specyficzne dla systemu iOS do tego projektu.
  - **Projekt NuGet** — kod nie zostanie dodany do tego projektu. Odwołuje się do innych projektów i zawiera konfigurację metadanych dla danych wyjściowych pakietu NuGet.
  - **Projektu współużytkowanych** — typowy kod należy dodać do tego projektu, w tym specyficzne dla platformy kodu wewnątrz `#if` dyrektywy kompilatora.

5. Kliknij prawym przyciskiem myszy na projekt NuGet i wybierz polecenie **opcje**, następnie otwórz **pakietu NuGet > metadanych** sekcji, a następnie wprowadź [wymaganych metadanych](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) (jako oraz wszelkim opcjonalne metadane):

  [![](platform-specific-images/specific-metadata-sml.png "Wprowadź wymagane metadane")](platform-specific-images/specific-metadata.png#lightbox)

6. Również w **opcje projektu** okna, otwórz **zestawy referencyjne** sekcji, a następnie wybierz które profile PCL biblioteki udostępnionej będzie obsługiwać za pośrednictwem "przynęta i przełącznik":

  ![](platform-specific-images/specific-reference-assemblies.png "Również w oknie Opcje projektu otwórz sekcji zestawów odwołań i wybierz polecenie które profile PCL biblioteki udostępnionej będzie obsługiwać za pośrednictwem przełącznika i przynęta")

  > [!NOTE]
> "Przynęta i przełącznik" oznacza, że zestawy PCL będzie zawierać tylko API udostępnianych przez biblioteki (nie może on zawierać kod specyficzne dla platformy). Po dodaniu NuGet do projektu Xamarin, bibliotek udostępnionych zostanie skompilowany przed PCL, ale zestawy specyficzne dla platformy zawiera kod, który będzie faktycznie używana przez system iOS lub Android projektu.

7. Kliknij prawym przyciskiem myszy projekt i wybierz polecenie **tworzenia pakietu NuGet** (lub kompilacji lub wdrożenia rozwiązania) i **.nupkg** plik pakietu NuGet, które zostaną zapisane w **/bin/** (folderu albo debugowanie czy wydanie, w zależności od konfiguracji).

  ![](platform-specific-images/create-nuget-package.png "Plik pakietu NuGet zostanie zapisany w folderze bin debugowania i wydania, w zależności od konfiguracji")


## <a name="verifying-the-output"></a>Weryfikowanie danych wyjściowych

Pakiety NuGet są także pliki ZIP, więc można przeprowadzać inspekcję wewnętrznej struktury wygenerowany pakiet.

Ten zrzut ekranu przedstawia zawartość obsługuje systemy iOS i Android, która ma dwa zestawy odwołań NuGet specyficzne dla platformy wybrane:

![](platform-specific-images/nuget-output.png "Pliki zawarte w pakiecie NuGet")


## <a name="related-links"></a>Linki pokrewne

- [Przewodnik po metadanych](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
