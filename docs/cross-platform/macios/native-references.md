---
title: Natywny odwołań
description: Natywnego odwołania daje możliwość osadzania natywnego Framework do projektu platformy Xamarin.iOS lub Xamarin.Mac lub powiązania projektu.
ms.prod: xamarin
ms.assetid: E53185FB-CEF5-4AB5-94F9-CC9B57C52300
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: c56e392420debb21998363cfffa288aec51691ea
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="native-references"></a>Natywny odwołań

_Natywnego odwołania daje możliwość osadzania natywnego Framework do projektu platformy Xamarin.iOS lub Xamarin.Mac lub powiązania projektu._


Począwszy od zestawu iOS 8.0 została można utworzyć osadzony platformę, by współużytkowanie kodu rozszerzeń aplikacji i głównej aplikacji w środowisku Xcode. Za pomocą funkcji odwołanie do natywnego będzie możliwe użycie tych platform embedded (utworzone w programie Xcode) w Xamarin.iOS.
 
> [!IMPORTANT]
> Nie będzie można utworzyć struktury osadzone z dowolnego typu Xamarin.iOS lub Xamarin.Mac projektów, natywnego odwołania umożliwiają tylko użycia istniejącej struktury natywnego (Objective-C).




<a name="Terminology" />

## <a name="terminology"></a>Terminologia

W systemie iOS 8 (lub nowszy) **struktury osadzone** może być jednocześnie osadzone struktury statycznie połączone i dynamicznie połączony. Aby rozpowszechnić je poprawnie, należy to zrobić do struktury "fat", które są dołączone wszystkie ich _wycinków_ dla każdej architektury urządzenia, które mają być obsługiwane z aplikacją.

<a name="Static-vs-Dynamic-Frameworks" />

### <a name="static-vs-dynamic-frameworks"></a>Statyczne vs. Dynamiczne struktur

**Statyczne struktur** są połączone w czasie kompilacji gdzie **dynamicznych struktur** są połączone w czasie wykonywania i do nich może być modyfikowana bez ponownego połączenia. Użycie dowolnej architektury 3rd firm przed iOS 8 zostały przy użyciu **Framework statycznych** który został skompilowany w ramach aplikacji. Zobacz firmy Apple [programowania dynamicznej biblioteki](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/DynamicLibraries/100-Articles/OverviewOfDynamicLibraries.html#//apple_ref/doc/uid/TP40001873-SW1) dokumentację, aby uzyskać więcej szczegółowych informacji.

<a name="Embedded-vs-System-Frameworks" />

### <a name="embedded-vs-system-frameworks"></a>Osadzony vs. Struktury systemu

**Osadzone struktury** są zawarte w Twojego pakietu aplikacji i dostępnych tylko do określonych aplikacji za pośrednictwem jego piaskownicy. **Struktury systemu** są przechowywane na poziomie systemu operacyjnego i są dostępne dla wszystkich aplikacji na urządzeniu. Obecnie tylko Apple ma możliwość tworzenia struktury poziomu systemu operacyjnego.

<a name="Thin-vs-Fat-Frameworks" />

### <a name="thin-vs-fat-frameworks"></a>Alokowania vs. Struktury FAT

**Elastycznej platformy** zawiera tylko kod skompilowany dla architektury systemu gdzie **struktury Fat** zawierają kod wielu architektur. Każdy codebase architektury skompilowany w ramach jest określany jako _wycinka_. Tak na przykład, gdyby platforma, która została skompilowana dwóch iOS Simulator architektury (i386 i X86_64), jego może zawierać dwóch wycinków.

Jeśli próbowano dystrybucję tego przykładu Framework z aplikacją, ten będzie działać poprawnie w symulatorze, ale się niepowodzeniem na urządzeniu, ponieważ platforma nie zawiera żadnych wycinki kodu specyficzne dla urządzenia z systemem iOS. Aby upewnić się, że platforma będzie działać we wszystkich wystąpieniach, go również musi zawierać wycinków specyficzne dla urządzenia, takie jak arm64, armv7 i armv7s.

<a name="Working-with-Embedded-Frameworks" />

## <a name="working-with-embedded-frameworks"></a>Praca z struktury osadzone

Istnieją dwa kroki, które należy wykonać, aby pracować z struktury osadzone w aplikacji platformy Xamarin.iOS lub Xamarin.Mac: tworzenie Fat Framework i osadzanie strukturze.

<a name="Overview" />

### <a name="creating-a-fat-framework"></a>Tworzenie Fat Framework

Jak wspomniano powyżej, aby można było korzystać z Framework osadzone w aplikacji, musi ona być Fat platforma, która zawiera wszystkie architektur systemów wycinki dla urządzeń, które aplikacja zostanie uruchomiona na.

Gdy w ramach i aplikację odbierającą znajdują się w tym samym projekcie Xcode, to nie jest problem jak Xcode zostanie skompilowany, zarówno w ramach, jak i aplikacji przy użyciu tych samych ustawień kompilacji. Ponieważ aplikacje platformy Xamarin nie można utworzyć struktury osadzone, nie można użyć tej metody.

Aby rozwiązać ten problem, `lipo` narzędzia wiersza polecenia można scalić dwóch lub więcej struktur jeden Framework Fat zawierająca wszystkie niezbędne wycinków. Aby uzyskać więcej informacji na temat pracy z `lipo` polecenia, zobacz nasze [łączenie natywnych bibliotek](~/ios/platform/native-interop.md) dokumentacji.

<a name="Embedding-a-Framework" />

### <a name="embedding-a-framework"></a>Osadzanie struktury

Wykonaj krok jest wymagany do osadzenia struktury w projekcie platformy Xamarin.iOS lub Xamarin.Mac za pomocą natywnego odwołania:

1. Utwórz nową ani otworzyć istniejącego projektu platformy Xamarin.iOS, Xamarin.Mac lub powiązanie.
2. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nazwę projektu i wybierz **Dodaj** > **Dodaj odwołanie do natywnego**: 

    [![](native-references-images/ref01.png "W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nazwę projektu i wybierz opcję Dodaj odwołanie do natywnego")](native-references-images/ref01.png#lightbox)
3. Z **Otwórz** oknie dialogowym Wybierz nazwę platformy natywnego Aby osadzić i kliknij przycisk **Otwórz** przycisk: 

    [![](native-references-images/ref02.png "Wybierz nazwę natywnego Framework osadzania i kliknij przycisk Otwórz")](native-references-images/ref02.png#lightbox)
4. Platformę zostaną dodane do drzewa projektu: 

    [![](native-references-images/ref03.png "Platformę zostaną dodane do drzewa projektów")](native-references-images/ref03.png#lightbox)

Podczas kompilowania projektu natywnego Framework zostanie osadzony w pakiecie aplikacji.

<a name="App-Extensions-and-Embedded-Frameworks" />

## <a name="app-extensions-and-embedded-frameworks"></a>Rozszerzenia aplikację i osadzone struktury

Wewnętrznie Xamarin.iOS mogą korzystać z tej funkcji, aby połączyć się ze środowiskiem uruchomieniowym Mono jako struktury (Jeśli cel wdrożenia jest > = iOS 8.0), co zmniejsza rozmiar aplikacji znacznie aplikacje z rozszerzeniami (ponieważ Mono środowiska uruchomieniowego zostaną uwzględnione tylko raz cały pakiet aplikacji, zamiast raz dla aplikacji kontenera i raz dla każdego rozszerzenia).

Rozszerzenia będą łączyć się z Mono środowiska uruchomieniowego jako platforma, ponieważ wszystkie rozszerzenia wymagają systemu iOS 8.0.

Aplikacje, które nie mają rozszerzenia i aplikacje tego docelowy z systemem iOS 

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule trwało szczegółowe przyjrzeć się osadzanie Framework macierzystego w aplikacji platformy Xamarin.iOS lub Xamarin.Mac.

