---
title: Udostępnionych projektów
description: Udostępnionych projektów umożliwiają pisanie wspólnej kodu, który odwołuje się wiele projektów różnych aplikacji. Kod jest skompilowana jako część każdego odwołaniem do projektu i może zawierać dyrektywy kompilatora, aby włączać funkcje specyficzne dla platformy do udostępnionego kodem.
ms.prod: xamarin
ms.assetid: 191c71fb-44a4-4e6c-af4b-7b1107dce6af
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 2910ada16aad2d8cb19cf0ee3a059c7df22b630e
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="shared-projects"></a>Udostępnionych projektów

_Udostępnionych projektów umożliwiają pisanie wspólnej kodu, który odwołuje się wiele projektów różnych aplikacji. Kod jest skompilowana jako część każdego odwołaniem do projektu i może zawierać dyrektywy kompilatora, aby włączać funkcje specyficzne dla platformy do udostępnionego kodem._

Projekty Shared (nazywanych także udostępnionych projektów zasobów) umożliwiają pisania kodu, który jest współużytkowana przez wiele projektów docelowych, w tym aplikacje platformy Xamarin.

Obsługują one dyrektywy kompilatora, tak aby warunkowo mogą obejmować specyficzne dla platformy kodu do kompilacji do podzbioru projektów, które odwołują się do projektu udostępnionego. Dostępna jest również obsługa środowiska IDE, aby pomóc w zarządzaniu dyrektywy kompilatora i wizualizacji wygląd kod w każdej aplikacji.

Jeśli użyto łączenie plików w przeszłości udostępnianie kodu między projektami, udostępnionych projektów działa w podobny sposób, ale znacznie ulepszona obsługa środowiska IDE.



## <a name="what-is-a-shared-project"></a>Co to jest projekt udostępniony?

W przeciwieństwie do większości innych typów projektu udostępnionego projektu nie ma żadnych danych wyjściowych (w postaci bibliotek DLL), zamiast tego kodu jest kompilowany do każdego projektu, który odwołuje się on. Jest to zilustrowane na poniższym diagramie — koncepcyjnie jest cała zawartość projektu udostępnionego "kopiowane do" każdego projektu odwołującego się i skompilowanych tak, jakby był część z nich.

 ![](shared-projects-images/sharedassetproject.png "Architektura projektu udostępnionego")

Kod w projekcie udostępnionym może zawierać dyrektywy kompilatora, które będą włączyć lub wyłączyć fragmentów kodu, w zależności od tego, która aplikacja projekcie są używani kod, który jest zasugerowany przez platformy kolorowe pola na diagramie.

Projekt udostępniony nie pobrać skompilowany samodzielnie, istnieje wyłącznie jako grupa plików kodu źródłowego, które mogą znajdować się w innych projektach. Jeśli przywoływany przez inny projekt, kod skutecznie jest kompilowana jako *części* tego projektu. Udostępnionych projektów nie może odwoływać się żadnym innym typem projektu (łącznie z innych projektów udostępnionych).

Należy pamiętać, że projekty aplikacji systemu Android nie mogą odwoływać się inne projekty aplikacji systemu Android — na przykład Android jednostkowy projekt testowy nie może odwoływać się projekt aplikacji systemu Android. Aby uzyskać więcej informacji na temat tego ograniczenia, zobacz ten [dyskusjach prowadzonych na forum](http://forums.xamarin.com/discussion/comment/98092/).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)



## <a name="visual-studio-for-mac-walkthrough"></a>Visual Studio for Mac wskazówki


W tej sekcji przedstawiono sposób tworzenia i używania projektu udostępnionego przy użyciu programu Visual Studio dla komputerów Mac. Odwołuje się do [udostępniony przykładowy projekt](#Shared_Project_Example) sekcji pełny przykład.


## <a name="creating-a-shared-project"></a>Tworzenie projektu udostępnionego


Aby utworzyć nowy projekt udostępniony przejdź do **Plik > Nowy rozwiązania...**  i wybierz nazwę.


![](shared-projects-images/xs-newsolution.png "Nowe rozwiązanie")


Nowy projekt udostępniony można dodać do rozwiązania, klikając prawym przyciskiem myszy plik rozwiązania i wybierając **Dodaj > Dodawanie nowego projektu...** . Nowy wygląd Projekt udostępniony w sposób przedstawiony poniżej — zauważyć, istnieją żadnych odwołań lub węzłów składników; te nie są obsługiwane dla projektów udostępnionych.


![](shared-projects-images/xs-empty.png "Pusty projekt udostępniony")


W projekcie udostępnionym powinna być użyteczna musi odwoływać się co najmniej jeden projekt może kompilacji (np. systemu iOS lub Android aplikacji lub biblioteki lub projektu PCL). Projekt udostępniony nie pobrać skompilowany, jeśli go nie ma nic odwołujące się do jego, tak składni (lub innych) błędy nie będą podświetlane, dopóki odwołano przez coś innego.



Dodawanie odwołania do projektu udostępnionego odbywa się ten sam sposób jak odwołujące się do regularnych projektu biblioteki. Ten zrzut ekranu przedstawia projektu platformy Xamarin.iOS odwołuje się do projektu udostępnionego.


![](shared-projects-images/xs-reference.png "Odwołanie projektu do projektu udostępnionego")


Po projektu udostępnionego odwołuje się do niego inny biblioteki lub aplikacji można Skompiluj rozwiązanie i wyświetlanie błędów w kodzie. Gdy odwołuje się do projektu udostępnionego _dwóch lub więcej_ inne projekty, zostanie wyświetlone menu w lewej górnej Edytor kodu źródłowego czy pokazuje Wybierz projekty, do których odwołują się tego pliku.



## <a name="shared-project-options"></a>Udostępniane opcje projektu


Gdy kliknij prawym przyciskiem myszy w projekcie udostępnionym i wybierz polecenie **opcje** brak ustawienia mniej niż inne typy projektów. Ponieważ udostępnionych projektów nie są kompilowane (samodzielnie), nie można ustawić opcji danych wyjściowych lub kompilatora, konfiguracje projektu, podpisywanie zestawu lub polecenia niestandardowych. Kod w projekcie udostępnionym skutecznie dziedziczy te wartości niezależnie od odwołuje się do nich.



**Opcje** ekran jest wyświetlany poniżej - projekt **nazwa** i **domyślne Namespace** tylko dwa ustawienia, które będą zwykle zmieniane.


![](shared-projects-images/xs-sharedprojectoptions.png "Udostępniane opcje projektu")



# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)



## <a name="visual-studio-walkthrough"></a>Visual Studio Walkthrough


W tej sekcji przedstawiono sposób tworzenia i używania projektu udostępnionego przy użyciu programu Visual Studio. Odwołuje się do [udostępniony przykładowy projekt](#Shared_Project_Example) sekcji zakończenia wdrożenia.


### <a name="creating-a-shared-project"></a>Tworzenie projektu udostępnionego


Aby utworzyć nowy projekt udostępniony przejdź do **Plik > Nowy rozwiązania...**  i wybierz nazwę dla projektu i rozwiązania.


![](shared-projects-images/vs-newsolution.png "Nowe rozwiązanie")


Nowy projekt udostępniony można dodać do rozwiązania, klikając prawym przyciskiem myszy plik rozwiązania i wybierając **Dodaj > Nowy projekt...** . Nowy projekt udostępniony wygląda jak pokazano poniżej (po dodaniu pliku klasy) — zauważyć istnieją żadnych odwołań lub węzłów składników; te nie są obsługiwane dla projektów udostępnionych.


![](shared-projects-images/vs-empty.png "Pusty projekt udostępniony")


W projekcie udostępnionym powinna być użyteczna musi odwoływać się co najmniej jeden projekt może kompilacji (np. systemu iOS lub Android aplikacji lub biblioteki lub projektu PCL). Projekt udostępniony nie pobrać skompilowany, jeśli go nie ma nic odwołujące się do jego, tak składni (lub innych) błędy nie będą podświetlane, dopóki odwołano przez coś innego.



Dodawanie odwołania do projektu udostępnionego odbywa się ten sam sposób jak odwołujące się do regularnych projektu biblioteki. Ten zrzut ekranu przedstawia projektu platformy Xamarin.iOS odwołuje się do projektu udostępnionego.


![](shared-projects-images/vs-reference.png "Odwołanie projektu do projektu udostępnionego")


Po projektu udostępnionego odwołuje się do niego inny biblioteki lub aplikacji można Skompiluj rozwiązanie i wyświetlanie błędów w kodzie. Gdy odwołuje się do projektu udostępnionego _dwóch lub więcej_ inne projekty, w lewym górnym Edytor kodu źródłowego, aby wyświetlić projekty, do których odwołują się bieżący plik kodu zostanie wyświetlone menu.


### <a name="shared-project-properties"></a>Właściwości projektu udostępnionego


Po wybraniu udostępnionego projektu istnieje mniej ustawienia w panelu Właściwości od innych typów projektu. Ponieważ udostępnionych projektów nie są kompilowane (samodzielnie), nie można ustawić opcji danych wyjściowych lub kompilatora, konfiguracje projektu, podpisywanie zestawu lub polecenia niestandardowych. Kod w projekcie udostępnionym skutecznie dziedziczy te wartości niezależnie od odwołuje się do nich.



**Właściwości** panelu są wyświetlane poniżej - **Namespace głównego** jest tylko ustawienie, które można zmienić.


![](shared-projects-images/vs-sharedprojectproperties.png "Właściwości projektu udostępnionego")



-----

<a name="Shared_Project_Example"/>

## <a name="shared-project-example"></a>Przykład projektu udostępnionego

[Tasky](https://github.com/xamarin/mobile-samples/tree/master/Tasky) przykład używa projektu udostępnionego zawiera typowy kod używany przez obie systemu iOS, Android i Windows Phone aplikacje. Zarówno `SQLite.cs` i `TaskRepository.cs` plików kodu źródłowego wykorzystywać dyrektywy kompilatora (np.) `#if __ANDROID__`) do generowania danych wyjściowych różnych dla każdej z aplikacji, które odwołują się do nich.

Poniżej przedstawiono struktury kompletnego rozwiązania (w programie Visual Studio for Mac i Visual Studio odpowiednio):

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 ![](shared-projects-images/xs-examplesolution.png "Programu Visual Studio for Mac rozwiązania")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

 ![](shared-projects-images/vs-examplesolution.png "Rozwiązania Visual Studio")

-----

Projekt Windows Phone może zostać przesłane z poziomu programu Visual Studio dla komputerów Mac, mimo że ten typ projektu nie jest obsługiwany dla kompilacji w programie Visual Studio dla komputerów Mac.

Uruchamianie aplikacji są wyświetlane poniżej.

 ![](shared-projects-images/example.png "systemy iOS, Android, Windows Phone przykłady")



## <a name="summary"></a>Podsumowanie

Ten dokument opisano, jak działają udostępnionych projektów, jak mogą być tworzone i używane w Visual Studio dla komputerów Mac i Visual Studio i wprowadzono proste przykładową aplikację prezentującą projektu udostępnionego w akcji.

## <a name="related-links"></a>Linki pokrewne

- [Tasky przykładowej aplikacji](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [Przenośnej biblioteki klas (przykład)](~/cross-platform/app-fundamentals/pcl.md)
- [Udostępnianie kodu opcje (przykład)](~/cross-platform/app-fundamentals/code-sharing.md)
