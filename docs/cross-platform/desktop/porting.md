---
ms.assetid: 814857C5-D54E-469F-97ED-EE1CAA0156BB
title: Wskazówki dotyczące przenoszenia aplikacji komputerowej
description: Proste wyjaśnienie sposobu oddziel istniejące formularze systemu Windows lub aplikacji WPF, aby utworzyć wieloplatformowych aplikacji do uruchamiania na macOS, iOS, Android, jak również platformy uniwersalnej systemu Windows i Windows 10.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: b9cfad9c046d4f2ad89506f7172a0418e90478f5
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781049"
---
# <a name="desktop-app-porting-guidance"></a>Wskazówki dotyczące przenoszenia aplikacji komputerowej

Większość kodu aplikacji można podzielić na jedną z następujących obszarów:

* Kod interfejsu użytkownika (np.) przyciski i system Windows)
* 3 formantów strony (np.) wykresy)
* Logika biznesowa (np.) reguły sprawdzania poprawności)
* Lokalne przechowywanie danych i dostępem
* Zdalny dostęp do danych i usług sieci Web

Dla formularzy systemu Windows i WPF dostęp dane lokalne aplikacji napisany za pomocą języka C# (lub języku Visual) zaskakująco dużo logiki biznesowej, i kod usług sieci web może być udostępniony na platformach.

## <a name="net-portability-analyzer"></a>Analizator przenośność .NET

Techniczną usługi Visual Studio 2015 i 2017 [analizator przenośność .NET](https://docs.microsoft.com/en-us/dotnet/articles/standard/portability-analyzer) ([Pobierz dla systemu Windows](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer)) co Sprawdź istniejące aplikacje i informujące o tym, ile kodu można przenieść "w jakim jest" do innych platform . Więcej informacji na ten temat można znaleźć z tego [wideo z witryny Channel 9](https://channel9.msdn.com/Blogs/Seth-Juarez/A-Brief-Look-at-the-NET-Portability-Analyzer).

Istnieje również narzędzia wiersza polecenia można pobrać z [analizator przenośność w serwisie GitHub](https://github.com/Microsoft/dotnet-apiport) i umożliwia zapewnienie tego samego raportów.

## <a name="x-of-my-code-is-portable-what-next"></a>"x przenośność jest % mój kod. Co dalej?"

Miejmy nadzieję, że analizatora zawiera dużą część kodu jest portable, ale na pewno ma być niektórych części każda aplikacja który _nie_ można przenieść na innych platformach.

Różne fragmentów kodu będzie prawdopodobnie należą do jednego z tych zasobników, co omówiono bardziej szczegółowo poniżej:

* Kod przenośny recyklingowi
* Kod, który wymaga wprowadzenia zmian
* Kod, który nie jest przenośny i wymaga ponownego zapisu

### <a name="re-useable-portable-code"></a>Kod przenośny recyklingowi

Kod .NET, który jest napisanych pod kątem interfejsami API dostępnymi na wszystkich platformach może zostać pobrany i platform bez zmian. W idealnym przypadku będzie mogła Przenieś ten kod do przenośnej biblioteki klas, biblioteki udostępnione lub biblioteki standardowej .NET i przetestować go w istniejących aplikacji.

Następnie można dodać tej biblioteki udostępnionej z projektami aplikacji dla innych platform (takich jak Android, iOS, system macOS).

### <a name="code-that-requires-changes"></a>Kod, który wymaga wprowadzenia zmian

Niektóre funkcje interfejsu API platformy .NET nie mogą być dostępne na wszystkich platformach. Jeśli te interfejsy API w kodzie, należy ponownie napisać te sekcje użycia interfejsów API i platform.

Przykłady to między innymi używać interfejsów API odbicia, które są dostępne w wersji 4.6 .NET, ale nie są dostępne na wszystkich platformach.

Po wpisaniu ponownie kod przy użyciu przenośnych interfejsów API, można pakiet kodu w bibliotece udostępnionej i przetestować go w istniejących aplikacji.

### <a name="code-that-isnt-portable-and-requires-a-re-write"></a>Kod, który nie jest przenośny i wymaga ponownego zapisu

Przykłady kodu, który nie może być i platform:

- **Interfejs użytkownika** — ekrany formularzy systemu Windows lub programu WPF nie można użyć w projektach na Android lub iOS, np. Interfejs użytkownika będzie musiał ponownie zapisane, za pomocą tej [porównywania formanty](~/cross-platform/desktop/controls/index.md) jako odwołanie.

- **Specyficzne dla platformy magazynu** -kod, który korzysta z technologii specyficznych dla platformy (na przykład w lokalnej bazie programu SQL Server Express). Musisz ponownie zapisać za pomocą alternatywnej między platformami, (na przykład SQLite dla aparatu bazy danych).
Niektóre operacje systemu plików może być również konieczne można dostosować, ponieważ platformy uniwersalnej systemu Windows ma nieco inne interfejsy API systemu Android i iOS (np.) Niektóre systemy plików jest rozróżniana wielkość liter i inne osoby nie).

- **składniki 3** — Sprawdź, czy 3 składniki aplikacji są dostępne na innych platformach. Inne, takie jak pakiety NuGet niewidoczne, może być dostępne, ale inne osoby (szczególnie visual formantach, takich jak wykresy lub nośnik odtwarzacze)

## <a name="tips-for-making-code-portable"></a>Wskazówki dotyczące wprowadzania kodu przenośnego

- **Iniekcji zależności** — Podaj różnych implementacji dla każdej platformy i

- **Rozwiązania warstwowego** — czy MVVM, MVC, MVP lub innych wzorzec, który pomaga oddzielić kod przenośny z kodu specyficzne dla platformy.

- **Obsługa wiadomości** — przekazywanie komunikatów w kodzie służy do usuwania połączenie interakcje między poszczególnymi częściami aplikacji.
