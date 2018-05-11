---
title: Tworzenie nowej biblioteki dla wielu platform dla NuGet
ms.prod: xamarin
ms.assetid: E7B55354-9BBE-4122-BCE3-3506B79090DD
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: ee508d40423e3757f7e2934b7682f840ebf8b86a
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2018
---
# <a name="creating-a-new-multiplatform-library-for-nuget"></a>Tworzenie nowej biblioteki dla wielu platform dla NuGet

Tworzenie projektu biblioteki Multiplatform używa PCL lub .NET Standard oznacza, że wynikowa NuGet można dodać do żadnego projektu platformy .NET obsługującej docelowy profil, w tym projekty programu ASP.NET lub aplikacji klasycznych przy użyciu WinForms WPF i platformy uniwersalnej systemu Windows.

Biblioteka może zawierać tylko kod obsługiwany przez wybrany profil PCL lub .NET Standard, jak również innych NuGets, które zostaną dodane.
Nadaje się do logiki biznesowej i algorytmy, które mogą być wyrażone w całości biblioteki klasy podstawowej platformy .NET.

Pojedynczy zestaw jest tworzony i wbudowane pakietu NuGet.

Jeśli później potrzebne funkcje specyficzne dla platformy [specyficzne dla platformy projekty mogą zostać dodane](#add-platforms).

## <a name="steps-to-create-a-multiplatform-library-nuget"></a>Kroki umożliwiające utworzenie NuGet biblioteki dla wielu platform

1. Wybierz **Plik > nowe rozwiązanie** (lub istniejącego rozwiązania kliknij prawym przyciskiem myszy i wybierz polecenie **Dodaj > Nowy projekt**).

2. Wybierz **biblioteki Multiplatform** z **Multiplatform > biblioteki** sekcji:

  [![](single-codebase-images/mulitplatform-library-sml.png "Konfigurowanie biblioteki obejmującego wiele platform dla podstawy pojedynczy kod")](single-codebase-images/mulitplatform-library.png#lightbox)

3. Wprowadź **nazwa** i **opis**i wybierz polecenie **pojedynczego dla wszystkich platform**:

  [![](single-codebase-images/single-configure-sml.png "Konfigurowanie biblioteki obejmującego wiele platform dla podstawy pojedynczy kod")](single-codebase-images/single-configure.png#lightbox)

4. Ukończ pracę kreatora. Projekt jedna biblioteka jest tworzony w rozwiązaniu.

5. Kliknij prawym przyciskiem myszy dla nowego projektu biblioteki, a następnie wybierz **opcje**. **Kompilacji > Ogólne** sekcja umożliwia **platformy docelowej** można ustawić — wybierz .NET Standard wersji lub profilu .NET PCL przenośnych:

  [![](single-codebase-images/single-choose-type-sml.png "Wybierz PCL lub .NET Standard dla biblioteki typów")](single-codebase-images/single-choose-type.png#lightbox)

6. Również w **opcje projektu** okna, otwórz **pakietu NuGet > metadanych** sekcji, a następnie wprowadź [wymaganych metadanych](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) (a także wszystkich metadanych, opcjonalny):

  [![](single-codebase-images/single-metadata-sml.png "Wprowadź wymagane metadane")](single-codebase-images/single-metadata.png#lightbox)

7. Kliknij prawym przyciskiem myszy na projekt Biblioteka i wybierz polecenie **tworzenia pakietu NuGet** (lub kompilacji lub wdrożenia rozwiązania) i **.nupkg** plik pakietu NuGet, które zostaną zapisane w **/bin/** folder (debugowania i wydania, w zależności od konfiguracji):

  ![](single-codebase-images/create-nuget-package.png "Plik pakietu NuGet zostanie zapisany w folderze bin debugowania i wydania, w zależności od konfiguracji")


## <a name="verifying-the-output"></a>Weryfikowanie danych wyjściowych

Pakiety NuGet są także pliki ZIP, więc można przeprowadzać inspekcję wewnętrznej struktury wygenerowany pakiet.

Ten zrzut ekranu przedstawia zawartość na podstawie PCL NuGet —, tylko w jednym zestawie PCL wchodzi:

![](single-codebase-images/nuget-output.png "Pliki zawarte w pakiecie NuGet")

<a name="add-platforms" />

## <a name="adding-platform-specific-code"></a>Dodawanie kodu specyficzne dla platformy

Na podstawie PCL i .NET Standard na podstawie projektów nie mogą zawierać odwołań specyficzne dla platformy (na przykład dla systemu iOS lub funkcji systemu Android).

Jeśli istniejący projekt PCL lub .NET Standard projektu musi można rozszerzyć, aby uwzględnić kod specyficzne dla platformy, można to zrobić przez kliknięcie prawym przyciskiem myszy projekt i wybierając **Dodaj > Dodaj implementację platformy...** :

[![](single-codebase-images/add-later-sml.png "Dodawanie menu implementacji platformy")](single-codebase-images/add-later.png#lightbox)

Co najmniej jeden projekt platformy można dodać do rozwiązania, a opcjonalnie można przekonwertować istniejącej biblioteki PCL lub .NET Standard do projektu udostępnionego:

[![](single-codebase-images/add-later-platforms-sml.png "Dodaj platformę opcje, takie jak systemu iOS, Android i projektu udostępnionego")](single-codebase-images/add-later-platforms-sml.png#lightbox)

Po przekonwertowaniu do projektu udostępnionego, odwiedź stronę **opcje projektu > Pakiet NuGet > Zestawy referencyjne**
[sekcji](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/platform-specific.md) i upewnij się, wszystkie wymagane zaznaczono profile (, aby NuGet w dalszym ciągu jest niezgodny z projektów, który był wcześniej używany w).


## <a name="related-links"></a>Linki pokrewne

- [Przewodnik po metadanych](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
