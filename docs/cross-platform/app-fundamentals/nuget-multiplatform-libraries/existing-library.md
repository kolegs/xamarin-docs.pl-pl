---
title: Tworzenie NuGet z istniejących projektów biblioteki
description: Ten dokument zawiera opis sposobu tworzenia pakietu NuGet z istniejącego projektu biblioteki, dzięki czemu kod, aby być współużytkowana z innymi deweloperami.
ms.prod: xamarin
ms.assetid: EDAC3E5E-DB7D-40A9-AE28-45C52ADA854E
author: asb3993
ms.author: amburns
ms.date: 04/20/2017
ms.openlocfilehash: 7f407b22d1793d585ae40aeae8c2d9b7616784e6
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34779986"
---
# <a name="creating-a-nuget-from-existing-library-projects"></a>Tworzenie NuGet z istniejących projektów biblioteki

Bibliotek PCL istniejących lub .NET Standard mogą być uwzględniane NuGets za pośrednictwem **opcje projektu** okno:

1. Kliknij prawym przyciskiem myszy na projekt biblioteki w **konsoli rozwiązania** i wybierz polecenie **opcje**.

2. Przejdź do **pakietu NuGet > metadanych** i wprowadź wszystkie [wymagane informacje](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) w **ogólne** karty:

  [![](existing-library-images/existing-metadata-sml.png "Wprowadź wymagane metadane")](existing-library-images/existing-metadata.png#lightbox)

3. Opcjonalnie [dodać dodatkowe metadane](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) w **szczegóły** kartę.

4. Po skonfigurowaniu metadanych można kliknij prawym przyciskiem myszy projekt i wybierz **tworzenia pakietu NuGet** i **.nupkg** plik pakietu NuGet, które zostaną zapisane w **/bin/** folder (debugowania i wydania, w zależności od konfiguracji).

  ![](existing-library-images/create-nuget-package.png "Wybierz pozycję Utwórz pakiet NuGet w menu kontekstowym")

5. Aby utworzyć pakiet NuGet w _co_ kompilacji lub wdrożenia, przejdź do **pakietu NuGet > kompilacji** sekcji i znaczników **tworzenia pakietów NuGet podczas kompilowania projektu**:

    [![](existing-library-images/existing-tickbox-sml.png "Znaczników do utworzenia pakietu NuGet")](existing-library-images/existing-tickbox.png#lightbox)

> [!NOTE]
> Tworzenie NuGet pakietu może to spowolnić proces kompilacji. Jeśli nie zaznaczono to pole, można nadal wygenerować pakietu NuGet ręcznie w dowolnym momencie z menu kontekstowego projektu (przedstawionym w kroku 4 powyżej).

## <a name="verifying-the-output"></a>Weryfikowanie danych wyjściowych

Pakiety NuGet są także pliki ZIP, więc można przeprowadzać inspekcję wewnętrznej struktury wygenerowany pakiet.

Ten zrzut ekranu przedstawia zawartość na podstawie PCL NuGet —, tylko w jednym zestawie PCL wchodzi:

![](existing-library-images/nuget-output.png "Pliki zawarte w pakiecie NuGet")


## <a name="related-links"></a>Linki pokrewne

- [Przewodnik po metadanych](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
