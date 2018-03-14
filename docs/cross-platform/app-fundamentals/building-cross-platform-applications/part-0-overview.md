---
title: "Tworzenie między platforma aplikacji — omówienie"
ms.topic: article
ms.prod: xamarin
ms.assetid: E442EEFB-FA9C-40E9-9668-5A3F915C8400
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 8e7b25f615939c0ae902e09c728615188d1cb86e
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/13/2018
---
# <a name="building-cross-platform-applications-overview"></a>Tworzenie między platforma aplikacji — omówienie

W tym przewodniku przedstawiono platformy Xamarin i sposobu projektowania aplikacji i platform, aby zmaksymalizować ponownego użycia kodu oraz świadczenia natywnym środowiskiem wysokiej jakości we wszystkich głównych platform urządzeń przenośnych: systemy iOS, Android i Windows Phone.

To podejście używane w tym dokumencie jest ogólnie dotyczy zarówno wydajność aplikacji i gier aplikacji, jednak koncentruje się na wydajność i narzędzia (z systemem innym niż gry aplikacji). Zobacz [wprowadzenie do dokumentu MonoGame](https://developer.xamarin.com/guides/cross-platform/game_development/monogame/introduction/) lub zapoznaj się z [Visual Studio Tools for Unity](https://docs.microsoft.com/en-us/visualstudio/cross-platform/visual-studio-tools-for-unity) wskazówki aplikacji gry dla wielu platform.

Wyrażenie "zapisu — jeden raz, uruchom wszędzie" jest często używany do extol zalet jedną codebase, że działa bez modyfikacji na wielu platformach. Podczas ponownego użycia kodu korzyści, tego podejścia często prowadzi do aplikacji, które mają zestaw funkcji najniższy denominator typowe i wyszukiwania zwykłego interfejsu użytkownika, który nie mieści się dobrze do żadnych docelowych platform.

Program Xamarin, nie jest po prostu "zapisu — jeden raz, uruchom wszędzie" platformy, ponieważ jeden z jego sile jest możliwość wdrażania interfejsów użytkownika natywnego specjalnie dla każdej platformy. Jednak przemyślane projekt jest nadal możliwe udostępnić większość niezwiązanych z użytkownikiem interfejs kodu i najlepsze cechy obu tych środowisk: jednokrotnego zapisu danych kodu logiki magazynu i biznesowych i prezentować UI natywnego na każdej z platform. W tym dokumencie omówiono ogólne architektura na osiągnięcie tego celu.

Poniżej przedstawiono podsumowanie kluczowych do tworzenia aplikacji i platform Xamarin:

-   **Użyj C#** -zapisu aplikacji w języku C#. Istniejący kod napisany w języku C# można przenieść z systemami iOS i Android za pomocą platformy Xamarin bardzo łatwo i oczywiście używany w aplikacjach systemu Windows.
-   **Korzystanie z MVC lub MVVVM wzorce projektowe** — tworzenie aplikacji interfejsu użytkownika przy użyciu wzorca modelu/widoku/kontrolera. Projektowania aplikacji przy użyciu podejścia modelu/widoku/kontrolera lub metody modelu/widok/ViewModel przypadku wyraźnej separacji między "Modelu" i rest. Określenie części aplikacji, które będą przy użyciu elementów interfejsu użytkownika macierzystego dla każdej z platform (systemy iOS, Android, Windows, Mac) i umożliwia przyjąć podzielić na dwa składniki aplikacji: "Core" i "Interfejsu użytkownika".
-   **Tworzenie natywnych UI** — każda aplikacja specyficzne dla systemu operacyjnego zapewnia warstwę inny interfejs użytkownika (realizowane w C# przy pomocy natywnych narzędzi projektowania interfejsu użytkownika):

1.  W systemach iOS należy użyć interfejsów API UIKit do utworzenia aplikacji wyszukiwania w trybie macierzystym, opcjonalnie przy użyciu platformy Xamarin dla systemu iOS projektanta do wizualnego tworzenia interfejsu użytkownika.
1.  W systemie Android należy użyć Android.Views do utworzenia aplikacji wyszukiwania native wykorzystaniu Xamarin w Projektancie interfejsu użytkownika.
1.  W systemie Windows używana będzie XAML dla warstwy prezentacji, utworzone w Projektancie interfejsu użytkownika programu Visual Studio lub mieszanego.
1.  Dla komputerów Mac użyjesz Scenorys warstwę prezentacji, utworzony w programie Xcode.

Projekty platformy Xamarin.Forms są obsługiwane na wszystkich platformach i zezwala na tworzenie interfejsów użytkownika, które mogą być udostępniane między platformami przy użyciu kodu XAML platformy Xamarin.Forms. 

Ilość ponownego użycia kodu zależy przede wszystkim od ilości kodu jest przechowywany w udostępnionego rdzeni i ilości kodu jest interfejs użytkownika określone. Kod core to wszystko, co nie wchodzi w interakcję bezpośrednio z użytkownikiem, ale przekazuje usług dla części aplikacji, które będą zbierać i wyświetlić te informacje.

Aby zwiększyć ilość kodu ponownego użycia, może przyjmować składników między platformami, które świadczenia usług wspólnej we wszystkich tych systemach, takich jak:

1.   [SQLite net](https://www.nuget.org/packages/sqlite-net-pcl/) lokalnego magazynu SQL
1.   [Wtyczki Xamarin](https://xamarin.com/plugins) do uzyskiwania dostępu do możliwości specyficznych dla urządzeń kamery, kontakty i używanie funkcji geolokalizacji, w tym
1.   [Pakiety NuGet](https://nuget.org) zgodnych z projektami Xamarin, takich jak [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/),
1.  Przy użyciu funkcji programu .NET framework dla sieci, usług sieci web, we/wy i inne.


Niektóre z tych składników są implementowane w *Tasky* analizę przypadku.

 <a name="Separate_Reusable_Code_into_a_Core_Library" />


## <a name="separate-reusable-code-into-a-core-library"></a>Oddzielanie do ponownego użycia kodu do podstawowej biblioteki

Wykonując zasady rozdzielenie obowiązków Układanie warstwowo architektury aplikacji, a następnie przenosząc podstawowych funkcji, która jest niezależny od platformy do biblioteki podstawowej wielokrotnego użytku, można zmaksymalizować kodu do udostępniania różnych platform, jak na poniższej ilustracji przedstawia:

 ![](part-0-overview-images/layers2.png "Wykonując zasady rozdzielenie obowiązków Układanie warstwowo architektury aplikacji, a następnie przenosząc podstawowych funkcji, która jest niezależny od platformy do biblioteki podstawowej wielokrotnego użytku, można zmaksymalizować kodu udostępnianie na platformach")

 <a name="Case_Studies" />


## <a name="case-studies"></a>Analizy przypadków:

Istnieje jeden analizę przypadku, który towarzyszy tego dokumentu — *Tasky Pro*. Każdy analizę przypadku omówiono implementacji pojęcia opisane w niniejszym dokumencie w przykładzie rzeczywistych. Kod jest typu open source i udostępniane na [github](https://github.com/xamarin/mobile-samples/).