---
title: "Aktualizacja odwołania do składników do NuGet"
description: "Zamień odwołania do składnika pakietów NuGet do zabezpieczenie przyszłych potrzeb aplikacji."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9E6C986F-3FBA-4599-8367-FB0C565C0ADE
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 11/22/2017
ms.openlocfilehash: f3dbfb52d4fbcb4dd65f695a862f6b041d2b22c0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="updating-component-references-to-nuget"></a>Aktualizacja odwołania do składników do NuGet

_Zamień odwołania do składnika pakietów NuGet do zabezpieczenie przyszłych potrzeb aplikacji._

W tym przewodniku objaśniono sposób aktualizacji w istniejących rozwiązaniach Xamarin zmienić składnika odwołania do pakietów NuGet.

- [Składniki, które zawierają pakietów NuGet](#contain)
- [Składniki z zamiany NuGet](#replace)

Większość składników należą do jednego z powyższych kategorii.
Jeśli używasz składnik, który nie ma odpowiednik pakietu NuGet, przeczytaj [składników, bez ścieżki migracji NuGet](#require-update) poniższej sekcji.

Odwoływać się do tych stron, aby uzyskać szczegółowe instrukcje dotyczące dodawania pakietów NuGet w [Windows](https://docs.microsoft.com/nuget/quickstart/use-a-package) lub [Mac](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).

<a name="contain" />

## <a name="components-that-contain-nuget-packages"></a>Składniki, które zawierają pakietów NuGet

Wiele składników zawiera już pakiety NuGet, a ścieżka migracji jest po prostu Usuń odwołanie do składnika.

Aby ustalić, czy składnik już zawiera pakiet NuGet przez dwukrotne kliknięcie składnika w rozwiązaniu:

![Węzeł składników rozwinięty](component-nuget-images/solution-sml.png)

**Pakiety** kartę Wyświetla wszystkie zawarte w składniku pakiety NuGet:

![Karta pakietów zawiera NuGet](component-nuget-images/packages-tab-sml.png)

Należy pamiętać, że **zestawy** karta jest pusta:

![Karta zestawów jest pusta](component-nuget-images/assemblies-tab-empty-sml.png)

### <a name="updating-the-solution"></a>Aktualizowanie rozwiązania

Aby zaktualizować rozwiązania, należy usunąć **składnika** wpisu z rozwiązania:

![Usuwanie składnika](component-nuget-images/delete-component-sml.png)

Pakiet NuGet pozostanie na liście w **pakiety** węzeł i aplikacja będzie skompilować i uruchomić w zwykły sposób. W przyszłych aktualizacji do tego pakietu będą wykonywane za pośrednictwem **Nuget** funkcji aktualizacji:

![Pakiet NuGet aktualizacji](component-nuget-images/nuget-update-sml.png)


<a name="replace" />

## <a name="components-with-nuget-replacements"></a>Składniki z zamiany NuGet

Jeśli strona informacje o składnika **zestawy** karta zawiera wpisy, jak pokazano poniżej, konieczne będzie ręczne znaleźć równoważne pakietu NuGet.

![Zawiera zestawy](component-nuget-images/assemblies-tab-sml.png)

Należy pamiętać, że **pakiety** karta prawdopodobnie będzie pusta:

![](component-nuget-images/packages-tab-empty-sml.png)

_Może on zawierać zależności NuGet, ale można je zignorować._


Aby potwierdzić zastępczy istnieje pakiet NuGet, wyszukaj [NuGet.org](https://www.nuget.org/packages), przy użyciu nazwy składnika lub alternatywnie przez autora.

Na przykład można znaleźć popularnych **sqlite-net-pcl** pakietu, wyszukując:

- [`sqlite-net-pcl`](https://www.nuget.org/packages?q=sqlite-net-pcl) — Nazwa produktu.
- [`praeclarum`](https://www.nuget.org/packages?q=praeclarum) — profilu autora.


### <a name="updating-the-solution"></a>Aktualizowanie rozwiązania

Po potwierdzeniu, że składnik ten jest dostępny w NuGet, wykonaj następujące kroki:

#### <a name="delete-the-component"></a>Usuwanie składnika

Kliknij prawym przyciskiem myszy składnik w rozwiązaniu i wybierz polecenie **Usuń**:

![Usuwanie składnika](component-nuget-images/remove-component-sml.png)

Spowoduje to usunięcie składnik i wszystkie odwołania. Spowoduje to przerwanie kompilacji, do momentu dodania równoważne pakietu NuGet, aby go zastąpić.

#### <a name="add-the-nuget-package"></a>Dodaj pakiet NuGet

1. Kliknij prawym przyciskiem myszy **pakiety** węzeł i wybierz polecenie **Dodawanie pakietów...** .
2. Wyszukaj według nazwy lub autora zastąpienia NuGet:

  ![](component-nuget-images/nuget-search-sml.png)

3. Naciśnij klawisz **Dodaj pakiet**.

Pakiet NuGet zostaną dodane do projektu, oraz wszelkie zależności.
To powinno rozwiązać kompilacji. Jeśli kompilacja zakończy się niepowodzeniem, sprawdź każdy błąd, aby zobaczyć, czy wystąpiły interfejsu API różnice między działania składnika i pakietu NuGet.

<a name="require-update" />

## <a name="components-without-a-nuget-migration-path"></a>Składniki bez ścieżki migracji NuGet

Nie trzeba się troszczyć, jeśli nie znajdziesz natychmiast zastępuje składniki używane w aplikacji. Istniejące elementy nadal będą działać w programie Visual Studio 15,5 cala i **składniki** węzła pojawi się w rozwiązaniu w zwykły sposób.

Przyszłych wersjach programu Visual Studio, jednak będą _nie_ Przywróć lub zaktualizować składniki.
Oznacza to, po otwarciu rozwiązania na nowym komputerze składnik nie będzie można pobrane i zainstalowane; i nie będzie mogła dostarczać aktualizacji autora. Należy zaplanować:

* Wyodrębnij zestawy ze składnika i odwoływać je bezpośrednio w projekcie.
* Skontaktuj się z autorem składnika i poproś o planach migrację do NuGet.
* Zbadaj alternatywnych pakietów NuGet lub wyszukiwanie kodu źródłowego, jeżeli kondycja składnika jest open source.

Wielu dostawców składnika nadal jest wykonywane na temat migracji do NuGet i innych użytkowników (w tym dostępnych na rynku produktów) może badanie opcje dostawy.


## <a name="related-links"></a>Linki pokrewne

- [Zainstalować i używać pakietu NuGet (system Windows)](https://docs.microsoft.com/nuget/quickstart/use-a-package)
- [W tym pakietu NuGet (Mac)](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)
