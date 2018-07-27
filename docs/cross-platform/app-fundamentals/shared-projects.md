---
title: Użyj udostępnionych projektów, które umożliwiają współużytkowanie kodu
description: Projekty udostępnione umożliwiają napisanie wspólnej kod, który odwołuje się do niej kilka projektów innej aplikacji. Kod jest kompilowany jako część każdego projektu z odwołaniem i może zawierać dyrektywy kompilatora ułatwiające włączać funkcje specyficzne dla platformy do podstawowej współużytkowanym kodem.
ms.prod: xamarin
ms.assetid: 191c71fb-44a4-4e6c-af4b-7b1107dce6af
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 61096635cd94d0fdd0abe6fda59c4efa41eeceb1
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270218"
---
# <a name="shared-projects-code-sharing"></a>Udostępnione, udostępnianie kodu w projektach

_Projekty udostępnione umożliwiają napisanie wspólnej kod, który odwołuje się do niej kilka projektów innej aplikacji. Kod jest kompilowany jako część każdego projektu z odwołaniem i może zawierać dyrektywy kompilatora ułatwiające włączać funkcje specyficzne dla platformy do podstawowej współużytkowanym kodem._

Projekty udostępnione (nazywanych także projektów udostępnionych zasobów) umożliwiają pisanie kodu, która jest współużytkowana przez wiele projektów docelowych, w tym aplikacje platformy Xamarin.

Obsługiwane są też dyrektywy kompilatora, tak aby warunkowo może zawierać kod specyficzny dla platformy, aby być skompilowane w ramach podzestawu projektów, które odwołują się do projektu udostępnionego. Dostępna jest również obsługa środowiska IDE, które ułatwiają zarządzanie dyrektywy kompilatora i zwizualizować, jak kod wygląda w każdej aplikacji.

Jeśli używano łączenia plików w przeszłości do udostępniania kodu między projektami, udostępnionych projektów działa w podobny sposób, ale znacznie ulepszona obsługa środowiska IDE.

## <a name="what-is-a-shared-project"></a>Co to jest projekt udostępniony?

W przeciwieństwie do większości innych typów projektu udostępnionego projektu nie ma żadnych danych wyjściowych (w formie biblioteki DLL), zamiast tego kod jest kompilowany do każdego projektu, który odwołuje się do niej. Jest to zilustrowane na poniższym diagramie — koncepcyjnie całą zawartość projektu udostępnionego "skopiowana do" każdego projektu z odwołaniem i kompilowane tak, jakby były one częścią.

![](shared-projects-images/sharedassetproject.png "Udostępnionego projektu architektury")

Kod w projekcie udostępniony może zawierać dyrektywy kompilatora, które będzie włączyć lub wyłączyć fragmentów kodu, w zależności od tego, która aplikacja projekcie są używani kod, który jest zalecane w ramkach kolorowe platformy na diagramie.

Projekt udostępniony nie uzyskać skompilować samodzielnie, istnieje wyłącznie jako grupa plików kodu źródłowego, które mogły zostać uwzględnione w innych projektach. Jeśli przywoływany przez inny projekt, kod skutecznie jest kompilowana jako *część* tego projektu. Projekty udostępnione nie mogą odwoływać się innych typów projektów (w tym inne projekty udostępnione).

Należy pamiętać, że projekty aplikacji dla systemu Android nie może odwoływać się do innych projektów aplikacji dla systemu Android — na przykład projekt testów jednostkowych dla systemu Android nie mogą odwoływać się projekt aplikacji systemu Android. Aby uzyskać więcej informacji na temat tego ograniczenia, zobacz ten [dyskusjach prowadzonych na forum](http://forums.xamarin.com/discussion/comment/98092/).

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="visual-studio-for-mac-walkthrough"></a>Program Visual Studio for Mac wskazówki

Ta sekcja zawiera szczegółowe instrukcje dotyczące tworzenia i używania projektu udostępnionego przy użyciu programu Visual Studio dla komputerów Mac. Odnoszą się do [udostępniony przykładowy projekt](#Shared_Project_Example) sekcji, aby uzyskać kompletny przykład.

## <a name="creating-a-shared-project"></a>Tworzenie projektu udostępnionego

Aby utworzyć nowy projekt udostępniony, przejdź do **Plik > nowe rozwiązanie...**  (lub z istniejącego rozwiązania kliknij prawym przyciskiem myszy i wybierając **Dodaj > Dodaj nowy projekt...** ):

[![Nowy projekt udostępniony](shared-projects-images/xs-newsolution-sml.png "nowe rozwiązanie")](shared-projects-images/xs-newsolution.png#lightbox)

Na następnym ekranie Wybierz nazwę projektu, a następnie kliknij przycisk **Utwórz**.

Poniżej przedstawiono nowy projekt udostępniony — należy zauważyć, że są żadnych odwołań lub węzły składników; te nie są obsługiwane w przypadku projektów udostępnionych.

![Pusty projekt udostępniony](shared-projects-images/xs-empty.png "pusty projekt udostępniony")

Dla projektu udostępnionego były przydatne musi ona być przywoływane przez co najmniej jeden projekt kompilacji stanie (na przykład dla systemu iOS lub aplikacji dla systemu Android lub biblioteki lub projektu PCL). Projekt udostępniony nie uzyskać skompilowany, jeśli nie ma nic nie odwołuje się do jego, więc składnia (lub innych) błędy nie będą podświetlane, dopóki nie ma przywołana przez coś innego.

Dodawanie odwołania do projektu udostępnionego odbywa się tak samo, jak odwołanie do projektu biblioteki regularne. Ten zrzut ekranu przedstawia odwołanie do projektu udostępnionego projektu Xamarin.iOS.

![](shared-projects-images/xs-reference.png "Odwołania projektu do projektu udostępnionego")

Po mowa projektu udostępnionego przez inną biblioteki lub aplikację można skompilować rozwiązanie i wyświetlanie błędów w kodzie. Gdy odwołuje się projekt udostępniony _dwóch lub więcej_ inne projekty, zostanie wyświetlone menu w lewym górnym Edytor kodu źródłowego, wybierz opcję pokazuje, które projekty odwoływać się do tego pliku.

## <a name="shared-project-options"></a>Udostępniane opcje projektu

Po kliknij prawym przyciskiem myszy nad projektem udostępnionych i wybraniu **opcje** istnieje mniej ustawień od innych typów projektów. Ponieważ udostępnione projekty nie są kompilowane (samodzielnie), nie można ustawić opcje danych wyjściowych i kompilatora, konfiguracje projektu, podpisywanie zestawu lub polecenia niestandardowe. Kod w projekcie udostępnione skutecznie dziedziczy te wartości niezależnie od rodzaju odwołuje się do nich.



**Opcje** poniżej. projekt jest wyświetlany ekran **nazwa** i **domyślne Namespace** są tylko dwa ustawienia, które zazwyczaj zmieni.


![](shared-projects-images/xs-sharedprojectoptions.png "Udostępniane opcje projektu")



# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)



## <a name="visual-studio-walkthrough"></a>Visual Studio Walkthrough


Ta sekcja zawiera szczegółowe instrukcje dotyczące tworzenia i używania projektu udostępnionego przy użyciu programu Visual Studio. Odnoszą się do [udostępniony przykładowy projekt](#Shared_Project_Example) sekcji, aby uzyskać pełną implementację.

### <a name="creating-a-shared-project"></a>Tworzenie projektu udostępnionego

Aby utworzyć nowy projekt udostępniony, przejdź do **Plik > nowe rozwiązanie...**  i wybierz nazwę projektu i rozwiązania.

![](shared-projects-images/vs-newsolution.png "Nowe rozwiązanie")

Nowy projekt udostępniony można dodać do rozwiązania, klikając prawym przyciskiem myszy plik rozwiązania i wybierając polecenie **Dodaj > Nowy projekt...** . Nowy projekt udostępniony wygląda jak poniżej (po dodaniu pliku klasy) — Zwróć uwagę, nie mają żadnych odwołań lub węzły składników; te nie są obsługiwane w przypadku projektów udostępnionych.

![](shared-projects-images/vs-empty.png "Pusty projekt udostępniony")

Dla projektu udostępnionego były przydatne musi ona być przywoływane przez co najmniej jeden projekt kompilacji stanie (na przykład dla systemu iOS lub aplikacji dla systemu Android lub biblioteki lub projektu PCL). Projekt udostępniony nie uzyskać skompilowany, jeśli nie ma nic nie odwołuje się do jego, więc składnia (lub innych) błędy nie będą podświetlane, dopóki nie ma przywołana przez coś innego.

Dodawanie odwołania do projektu udostępnionego odbywa się tak samo, jak odwołanie do projektu biblioteki regularne. Ten zrzut ekranu przedstawia odwołanie do projektu udostępnionego projektu Xamarin.iOS.

![](shared-projects-images/vs-reference.png "Odwołania projektu do projektu udostępnionego")

Po mowa projektu udostępnionego przez inną biblioteki lub aplikację można skompilować rozwiązanie i wyświetlanie błędów w kodzie. Gdy odwołuje się projekt udostępniony _dwóch lub więcej_ inne projekty menu pojawia się w lewym górnym Edytor kodu źródłowego, aby zobaczyć, które projekty odwoływać się do bieżącego pliku kodu.


### <a name="shared-project-properties"></a>Właściwości projektu udostępnionego


Po wybraniu udostępniony projekt istnieje mniej ustawień w panelu Właściwości od innych typów projektów. Ponieważ udostępnione projekty nie są kompilowane (samodzielnie), nie można ustawić opcje danych wyjściowych i kompilatora, konfiguracje projektu, podpisywanie zestawu lub polecenia niestandardowe. Kod w projekcie udostępnione skutecznie dziedziczy te wartości niezależnie od rodzaju odwołuje się do nich.

**Właściwości** panelu znajdują się poniżej — **Namespace głównego** to jedyne ustawienie, które można zmienić.

![](shared-projects-images/vs-sharedprojectproperties.png "Właściwości projektu udostępnionego")

-----

<a name="Shared_Project_Example"/>

## <a name="shared-project-example"></a>Przykład projektu udostępnionego

[Tasky](https://github.com/xamarin/mobile-samples/tree/master/Tasky) przykład używa projektu udostępnionego, aby zawierała wspólny kod używane przez oba systemy iOS, Android i Windows Phone aplikacje. Zarówno `SQLite.cs` i `TaskRepository.cs` plików kodu źródłowego wykorzystywać dyrektywy kompilatora (np.) `#if __ANDROID__`) Aby uzyskać różne wyniki dla każdej aplikacji, które odwoływać się do nich.

Poniżej przedstawiono strukturę kompletnego rozwiązania (w programie Visual Studio dla komputerów Mac i Visual Studio odpowiednio):

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](shared-projects-images/xs-examplesolution.png "Program Visual Studio for Mac rozwiązania")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](shared-projects-images/vs-examplesolution.png "Rozwiązania Visual Studio")

-----

Projektu Windows Phone można nawigować z poziomu programu Visual Studio dla komputerów Mac, mimo że tego typu projektu nie jest obsługiwana dla kompilacji w programie Visual Studio dla komputerów Mac.

Uruchamianie aplikacji zostały wymienione poniżej:

![](shared-projects-images/example.png "systemy iOS, Android, Windows Phone przykłady")

## <a name="summary"></a>Podsumowanie

W tym dokumencie opisano, jak działają projekty udostępnione, jak mogą być tworzone i używane w Visual Studio dla komputerów Mac i Visual Studio i wprowadzono proste przykładowej aplikacji, która pokazuje projekt udostępniony w działaniu.

## <a name="related-links"></a>Linki pokrewne

- [Tasky przykładowej aplikacji](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [Biblioteki klas przenośnych (przykład)](~/cross-platform/app-fundamentals/pcl.md)
- [Udostępnianie kodu, opcje (przykład)](~/cross-platform/app-fundamentals/code-sharing.md)
