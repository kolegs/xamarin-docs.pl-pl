---
title: "Wdrażanie i testowanie"
description: "Testowanie na urządzeniach i przekazywania do sklepu z aplikacjami"
ms.topic: article
ms.prod: xamarin
ms.assetid: 98257399-E9B3-4BAB-9204-0E89117DEA6D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: fdd4311072efd5571724fbe00d12a96921054fa2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="deployment-and-testing"></a>Wdrażanie i testowanie

## <a name="deployment-checklist"></a>Lista kontrolna wdrażania

Czy podczas wdrażania w teście czujki lub przekazywania do sklepu z aplikacjami, należy wykonać kroki na tej stronie:

- W **Centrum deweloperów systemu iOS**:
  - [Identyfikatory aplikacji](#App_IDs) zostały utworzone.
  - [Grupy aplikacji](#App_Groups) skonfigurowane (jeśli jest to wymagane).
  - [*Dystrybucji* profile inicjowania obsługi administracyjnej](#Provisioning_Profiles) utworzony.

- W rozwiązaniu:

  - Sprawdź [identyfikatorów pakietów i odwołań do projektu](~/ios/watchos/get-started/installation.md) są ustawione.
  - Sprawdź, czy ikony [skonfigurowany poprawnie](~/ios/watchos/app-fundamentals/icons.md).
  - Sprawdź dopasowanie numery wersji pakietu we wszystkich projektach.
  - Skonfiguruj **Entitlements.plist** dla grupy aplikacji (jeśli jest to wymagane).

* Następnie postępuj zgodnie z instrukcjami, aby:
  - [Wdrażanie na Apple Watch testowania](~/ios/watchos/deploy-test/device.md), lub
  - [Przekaż do sklepu z aplikacjami](~/ios/watchos/deploy-test/appstore.md).


## <a name="app-ids"></a>Identyfikatory aplikacji

Zgodnie z opisem w [instrukcje instalacji](~/ios/watchos/get-started/installation.md), wszystkie trzy projekty w aplikacji czujki pokrewne identyfikatorów pakietu, takich jak:

- Projekt Unified Xamarin.iOS — `com.xamarin.WatchKitCatalog`
- Projekt rozszerzenia WatchKit- `com.xamarin.WatchKitCatalog.watchkitextension`
- Projekt aplikacji Czujka — `com.xamarin.WatchKitCatalog.watchkitapp`

Wszystkie trzy projekty wymagają zgodnego dystrybucji inicjowania obsługi profilu, albo za pomocą jawnie identyfikatorów aplikacji dla każdej lub symbolem wieloznacznym identyfikator aplikacji.

### <a name="explicit-app-ids"></a>Identyfikatory aplikacji jawne

Utwórz **identyfikator aplikacji** identyfikator pakietu każdego każdego projektu (co będzie wyglądać następująco w Centrum deweloperów systemu iOS):

![Identyfikatorów pakietów w Centrum deweloperów systemu iOS](images/appids-specific-sml.png)

Podczas tworzenia lub konfigurowania identyfikatorów aplikacji, należy włączyć określonych funkcji aplikacji wymaga. Mogą one obejmować powiadomień wypychanych i grupy aplikacji.

Musisz utworzyć profil inicjowania obsługi administracyjnej dystrybucji dla każdego identyfikatora aplikacji.

### <a name="wildcard-app-id"></a>Identyfikator aplikacji symboli wieloznacznych

Alternatywnie można utworzyć symbol wieloznaczny **identyfikator aplikacji** , które odpowiadają wszystkich trzech projektach, takich jak `com.xamarin.*`.

Należy pamiętać, że niektóre funkcje nie można użyć z symbolem wieloznacznym Identyfikatora aplikacji (na przykład powiadomienia wypychane). Jeśli aplikacja wymaga tych funkcji, należy utworzyć jawne identyfikatorów aplikacji.

Do dystrybucji tylko należy utworzyć jeden dystrybucji profil udostępniania dla symbolu wieloznacznego identyfikator aplikacji.

<a name="app-groups" />

## <a name="app-groups"></a>Grupy aplikacji

Grupy aplikacji umożliwia udostępnianie danych między aplikacjami dla systemu iOS i rozszerzenia czujki. Należy upewnić się, że ma rozwiązania:

- Skonfigurowane **grupy aplikacji** w portalu dla deweloperów firmy Apple **certyfikaty, identyfikatory & profile** sekcji.

- Włączone **grupy aplikacji** (i podać **identyfikator grupy aplikacji**) w *zarówno* aplikacja systemu iOS i rozszerzenia czujki **identyfikator aplikacji** i  **Entitlements.plist**.

### <a name="certificates-identifiers--profiles"></a>Certyfikaty, identyfikatory i profili

Aby użyć grupy aplikacji, Utwórz wpis w **grupy aplikacji** ekranu. W poniższym przykładzie grupie nosi nazwę z tej samej styl wstecznego DNS, który jest powszechnie używany dla identyfikatorów aplikacji, ale z `group.` prefiks z (wymagane):

![Identyfikator](images/appgroups-new-sml.png)

Grupa aplikacji pojawi się na liście:

![Lista identyfikatorów](images/appgroups-setup-sml.png)

Po utworzeniu grupy może być przywoływany w Twojej **identyfikator aplikacji** konfiguracji. Pamiętaj, aby uwzględnić go zarówno systemu iOS aplikacji i rozszerzenie czujki **identyfikatorów aplikacji**.

![Dostępne consifurations](images/appgroups-sml.png)

Czy **nie** włączyć grupy aplikacji w identyfikator Apple Watch aplikacji. Nie jest to wymagane w czujki, sama.

### <a name="entitlementsplist"></a>Entitlements.plist

Niektóre funkcje aplikacji (np.) Grupy aplikacji) trzeba ustawić uprawnienia użytkownika.
Kliknij dwukrotnie, aby edytować **Entitlements.plist** pliku w tych projektach:

- Projekt aplikacji dla systemu iOS
- Obejrzyj projekt rozszerzenia

.![Edytor Entitlements.plist](images/entitlements-plist-sml.png)

Czy **nie** Włącz uprawnień w projekcie czujki aplikacji. Nie jest to wymagane w czujki, sama.



## <a name="related-links"></a>Linki pokrewne

- [Przewodnik przesyłanie WatchKit firmy Apple](https://developer.apple.com/app-store/watch/)
