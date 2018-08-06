---
title: Część 3 — konfigurowanie rozwiązania platformy Xamarin między
description: Ten dokument zawiera opis sposobu konfigurowania rozwiązanie i platform Xamarin. Go dyski różnych kodu, takie jak udostępnianie strategii udostępnionych projektów i .NET Standard.
ms.prod: xamarin
ms.assetid: 4139A6C2-D477-C563-C1AB-98CCD0D10A93
author: asb3993
ms.author: amburns
ms.date: 03/27/2017
ms.openlocfilehash: f802e31d851915d33cb6dbf5866f8cba3ab90303
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781275"
---
# <a name="part-3---setting-up-a-xamarin-cross-platform-solution"></a>Część 3 — konfigurowanie rozwiązania platformy Xamarin między

Niezależnie od tego, jakie platformy są używane, wszystkie projekty Xamarin używać tego samego formatu pliku rozwiązania (Visual Studio **.sln** format pliku). Rozwiązania mogą być współużytkowane przez środowisk deweloperskich, nawet wtedy, gdy nie można załadować poszczególnych projektów (na przykład projekt systemu Windows w programie Visual Studio dla komputerów Mac).



Podczas tworzenia nowego cross platform aplikacji, pierwszym krokiem jest utworzenie rozwiązania puste. To sekcja co dalej: Konfigurowanie projektów do tworzenia aplikacji mobilnych międzyplatformowego.

 <a name="Sharing_Code" />


## <a name="sharing-code"></a>Udostępnianie kodu

Zapoznaj się [opcje udostępniania kodu](~/cross-platform/app-fundamentals/code-sharing.md) dokumentu, aby uzyskać szczegółowy opis sposobu wdrażania udostępniania kodu na platformach.

 <a name="Shared_Asset_Projects" />


### <a name="shared-projects"></a>Udostępnionych projektów

Najprostszym sposobem udostępniania plików kodu jest użycie [projektu udostępnionego](~/cross-platform/app-fundamentals/shared-projects.md).

Ta metoda umożliwia udostępnianie tego samego kodu w projektach różnych platform i użyj dyrektywy kompilatora, aby uwzględnić ścieżki kodu inny, specyficzny dla platformy.

 <a name="Portable_Class_Libraries" />


### <a name="portable-class-libraries-pcl"></a>Biblioteki klas przenośnych (PCL)

W przeszłości pliku projektu platformy .NET (i zestaw wynikowy) było skierowane do wersji określonej struktury. Zapobiega to projekt lub zestawu jest udostępniany przez różnych platform.

Przenośnej biblioteki klas (PCL) to specjalny typ projektu, który może być używany na różnych platformach interfejsu wiersza polecenia, takich jak Xamarin.iOS i Xamarin.Android, a także WPF, platformy uniwersalnej systemu Windows i Xbox. Biblioteki może korzystać tylko podzbiór pełną .NET framework, ograniczone przez platformy docelowej.

Więcej o jego Xamarin [obsługę bibliotek klas przenośnych](~/cross-platform/app-fundamentals/pcl.md) i postępuj zgodnie z instrukcjami, aby zobaczyć sposób [próbki TaskyPortable](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable) działa.


### <a name="net-standard"></a>.NET standard

Wprowadzone w 2016 r. [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md) projektów umożliwiają łatwe udostępnianie kodu na platformach, zwracające zestawy, które mogą być używane w systemie Windows, Xamarin platform (systemy iOS, Android, Mac) i Linux.

.NET standardowych bibliotek można tworzyć i używać jak PCLs, z wyjątkiem tego, że z interfejsami API dostępnymi w każdej wersji (od 1.0 do wersji 1.6) łatwiej są odnajdywane i poszczególnych wersji jest wstecznie zgodna z niższym numerów wersji.



 <a name="Populating_the_Solution" />


## <a name="populating-the-solution"></a>Wypełnianie rozwiązania

Niezależnie od tego, którego metoda jest używana do udostępniania kodu ogólną strukturę rozwiązania należy zaimplementować warstwowa architektura, które wspiera udostępniania kodu.
Rozwiązaniem Xamarin jest kod grupy na dwa typy projektu:

-   **Projekt Core** — pisania kodu do ponownego użycia w jednym miejscu, do udostępnienia na różnych platformach. Użyj zasad hermetyzacji Aby ukryć szczegóły implementacji, gdy jest to możliwe.
-   **Projekty aplikacji specyficzne dla platformy** — korzystać do ponownego użycia kodu przy użyciu jako małego sprzężenia, jak to możliwe. Funkcje specyficzne dla platformy są dodawane na tym poziomie w oparciu składników widoczne w projekcie Core.


 <a name="Core_Project" />


### <a name="core-project"></a>Projektów

Projekty udostępniony kod powinien odwoływać się tylko zestawy, które są dostępne na wszystkich platformach —, tj. wspólnej przestrzeni nazw framework, takich jak `System`, `System.Core` i `System.Xml`.

Udostępnionych projektów powinny implementować tyle funkcji bez interfejsu użytkownika, ponieważ jest to możliwe, obejmujące następujące warstwy:

-   **Warstwa danych** — kod, który zajmuje się fizycznej pamięci masowej np.  [SQLite NET](https://github.com/praeclarum/sqlite-net), alternatywnej bazy danych, takich jak [Realm.io](https://realm.io/products/realm-mobile-database/) , a nawet plików XML. Klasy warstwy danych są zazwyczaj używane tylko przez Warstwa dostępu do danych.
-   **Warstwa dostępu do danych** — definiuje interfejs API, który obsługuje operacje danych wymagane dla funkcji aplikacji, takich jak metody do listy danych, pojedyncze elementy danych dostępu, a także tworzyć, edytować i usuń je.
-   **Warstwa dostępu do usługi** — opcjonalnie warstwy zapewnienie chmury usług do aplikacji. Zawiera kod, który uzyskuje dostęp do zasobów sieci zdalnej (usługi sieci web, pliki do pobrania obrazu itp.) i prawdopodobnie buforowanie wyników.
-   **Warstwa biznesowa** — definicja klasy modeli i fasad lub Menedżera klas, które udostępniają funkcje specyficzne dla platformy aplikacji.


 <a name="Platform-Specific_Application_Projects" />


### <a name="platform-specific-application-projects"></a>Projekty aplikacji specyficzne dla platformy

Projekty specyficzne dla platformy musi odwoływać się zestawy wymaganej do powiązania do każdej z platform SDK (Xamarin.iOS, Xamarin.Android, Xamarin.Mac lub Windows) oraz podstawowe projektu udostępnionego kodu.

Projekty specyficzne dla platformy powinny implementować:

-   **Warstwa aplikacji** — funkcje platformy i powiązania/konwersji między obiektami warstwie Business i interfejsie użytkownika.
-   **Warstwę interfejsu użytkownika** — ekrany, formanty niestandardowego interfejsu użytkownika, prezentacji logikę weryfikacji.


<a name="Example" />


### <a name="example"></a>Przykład

Na tym diagramie przedstawiono Architektura aplikacji:

 [ ![](setting-up-a-xamarin-cross-platform-solution-images/conceptualarchitecture.png "Ten diagram przedstawia architektury aplikacji")](setting-up-a-xamarin-cross-platform-solution-images/conceptualarchitecture.png#lightbox)

Ten zrzut ekranu przedstawia konfiguracji rozwiązania, z udostępnionych projektów, iOS i projekty aplikacji systemu Android. Projektu udostępnionego zawiera kod odnoszących się do każdej warstwy architektury (kod firm, usługi, danych i dostępu do danych):

 ![](setting-up-a-xamarin-cross-platform-solution-images/core-solution-example.png "Projektu udostępnionego zawiera kod odnoszących się do każdej warstwy architektury (kod firm, usługi, danych i dostępu do danych)")


 <a name="Project_References" />


## <a name="project-references"></a>Odwołania do projektu

Odwołania do projektu odzwierciedlają zależności w projekcie. Projekty Core ograniczyć ich odwołania do zestawów wspólnych tak, że kod jest łatwe udostępnianie.
Projekty aplikacji specyficzne dla platformy odwoływać się do kodu udostępnionych oraz innych zestawów specyficzne dla platformy, należy korzystać z platformy docelowej.

Projekty aplikacji każdego odwołania projektu współużytkowanych i zawierać kodu interfejsu użytkownika wymagane do obecnych funkcji dla użytkownika, jak pokazano w tych zrzuty ekranu:

![](setting-up-a-xamarin-cross-platform-solution-images/solution-android.png "Każde odwołanie projektu udostępnionych projektów aplikacji") ![](setting-up-a-xamarin-cross-platform-solution-images/solution-ios.png "każdego odwołania projektu udostępnionych projektów aplikacji")


Przykładów struktury projekty są podane w analizy przypadków.

 <a name="Adding_Files" />


## <a name="adding-files"></a>Dodawanie plików

 <a name="Build_Action" />


### <a name="build-action"></a>Akcja kompilacji

Jest ważne, aby ustawić poprawnej akcji kompilacji dla niektórych typów plików. Ta lista zawiera akcji kompilacji dla niektórych typowych typów plików:

-  **Wszystkie pliki C#** — Akcja kompilacji: kompilowanie
-   **Obrazy platformy Xamarin.iOS systemu & Windows** — Akcja kompilacji: zawartości
-   **Pliki XIB i scenorysu w Xamarin.iOS** — Akcja kompilacji: InterfaceDefinition
-   **Obrazy i układy AXML w systemie Android** — Akcja kompilacji: AndroidResource
-  **Pliki XAML w projektach systemu Windows** — Akcja kompilacji: strony
-  **Pliki XAML platformy Xamarin.Forms** — Akcja kompilacji: EmbeddedResource


Ogólnie rzecz biorąc IDE wykryje typu pliku i zaproponuje akcji poprawne kompilacji.

 <a name="Case_Sensitivity" />


### <a name="case-sensitivity"></a>Uwzględniana wielkość liter

Na koniec należy pamiętać, że niektóre platformy (np systemy plików z uwzględnieniem wielkości liter.
iOS i Android) tak można użyć standard nazewnictwa plików spójne i upewnij się, że nazwy plików, użyj w kodzie dokładnie zgodne systemu plików. Jest to szczególnie ważne w przypadku obrazów i inne zasoby, które odwołują się w kodzie.
