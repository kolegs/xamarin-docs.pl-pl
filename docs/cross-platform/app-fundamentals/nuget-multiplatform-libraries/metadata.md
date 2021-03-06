---
title: Edytowanie metadanych NuGet
description: Ten dokument zawiera opis sposobu edycji metadanych NuGet dla biblioteki dla wielu platform przy użyciu opcji projektu. Zawarto informacje metadanych zarówno wymaganych i opcjonalnych.
ms.prod: xamarin
ms.assetid: 147BA370-67A7-4E6C-BF17-AA7C536C0A48
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 3680b02003a844668b0b5c97e5d4c0d296ae3500
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34779973"
---
# <a name="editing-nuget-metadata"></a>Edytowanie metadanych NuGet

_Opcje projektu umożliwia edytowanie NuGet metadanych dla biblioteki dla wielu platform_

Biblioteka typów projektów (na przykład PCL lub .NET Standard lub nowy typ projektu NuGet) ma **pakietu NuGet** sekcji **opcje projektu** okna.

**Metadanych** sekcji konfiguruje wartości użyte w [ **.nuspec** pliku manifestu pakietu NuGet](https://docs.microsoft.com/nuget/create-packages/creating-a-package#the-role-and-structure-of-the-nuspec-file).

## <a name="required-information"></a>Wymagane informacje

**Ogólne** karta zawiera cztery pola, które należy podać można wygenerować pakietu NuGet:

[![](metadata-images/metadata-general-sml.png "Okno wymaganych metadanych pakietu NuGet")](metadata-images/metadata-general.png#lightbox)

- **Identyfikator** — identyfikator pakietu, która powinna być unikatowa w obrębie Nuget.org (lub wszędzie tam, gdzie dystrybucji pakietu). Po tym [wskazówki](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) i używać tylko znaków, które są dozwolone w adresie URL (bez spacji i znaków specjalnych uniknąć).
- **Wersja** — wybierz numer wersji zgodnie z [reguły kontroli wersji NuGet](https://docs.microsoft.com/nuget/create-packages/dependency-versions).
- **Autorzy** — rozdzielana przecinkami lista nazw.
- **Opis elementu** — Przegląd funkcji pakietu, która jest wyświetlana, gdy użytkownicy wybierają pakietu.

> [!NOTE]
> Pamiętaj, aby zwiększyć numer wersji, podczas tworzenia nowych wersji w celu dystrybucji do NuGet lub innych użytkowników.

Aby uzyskać więcej informacji, zobacz [odwołania wymagane elementy](https://docs.microsoft.com/nuget/schema/nuspec#required-metadata-elements) uzyskać więcej informacji, a także jak te szczegółowe instrukcje na [wybranie identyfikator unikatowy pakiet i ustawianie numeru wersji](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) i [ Ustawienie typu pakietu](https://docs.microsoft.com/nuget/create-packages/creating-a-package#setting-a-package-type).

> [!IMPORTANT]
> Należy wprowadzić wszystkie pola na tej karcie; w przeciwnym razie zostanie wyświetlony komunikat o błędzie: _"projektu nie ma metadanych NuGet dlatego pakietu NuGet nie zostanie utworzona. Metadane pakietów NuGet można określić w sekcji Opcje projektu w metadanych"_

## <a name="optional-metadata"></a>Opcjonalne metadane

**Szczegóły** karta zawiera pola opcjonalne, które mają zostać uwzględnione w pliku manifestu pakietu NuGet.

[![](metadata-images/metadata-detail-sml.png "Okno opcjonalne metadane pakietów NuGet")](metadata-images/metadata-detail.png#lightbox)

Zapoznaj się [opcjonalne informacje dodatkowe elementy](https://docs.microsoft.com/nuget/schema/nuspec#optional-metadata-elements) Aby uzyskać więcej informacji o polach wymaganych i opcjonalnych.

> [!NOTE]
> Jeśli pakiet NuGet jest dystrybuowany na [NuGet.org](https://www.nuget.org) zaleca się Podaj jak najwięcej informacji.


## <a name="related-links"></a>Linki pokrewne

- [odwołanie .nuspec](https://docs.microsoft.com/nuget/schema/nuspec#general-form-and-schema)
