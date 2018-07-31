---
title: Ogólne — często zadawane pytania
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C7E6E54D-3957-407D-BB87-22B095148C6B
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 06aa6569301d1bfdbf9f6fd1e7397a38a9beb6f6
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350823"
---
# <a name="general-frequently-asked-questions"></a>Ogólne — często zadawane pytania

## <a name="portable-class-libraries"></a>Biblioteki klas przenośnych

### <a name="how-can-i-view-what-libraries-are-supported-in-a-pclpcl-support-librariesmd"></a>[Jak wyświetlić biblioteki obsługiwane w aplikacji PCL?](pcl-support-libraries.md)
Ten przewodnik zawiera listę zasobów i metod do określania, czy istniejącej biblioteki jest obsługiwane przez różne platformy docelowe PCL lub mogą być konwertowane na profil PCL.

### <a name="pcl-reflection-apipcl-reflectionmd"></a>[Interfejs API odbicia aplikacji PCL](pcl-reflection.md)
Firma Microsoft opracowała nowy interfejs API odbicia do wykorzystania w przenośnych bibliotek klas. W przypadku niektórych istniejący kod odbicia, który chcesz przenieść do aplikacji PCL mogą nie działać.

### <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-packagepcl-case-studymd"></a>[Analiza przypadku aplikacji PCL: jak rozwiązać problemy związane z obiektem System.Diagnostics.Tracing dla pakietu NuGet przepływu danych TPL firmy Microsoft?](pcl-case-study.md)
Xamarin.iOS i Xamarin.Android nie należy implementować każdy profil PCL, dzięki czemu ich jako odwołania do poziomu 100%. Dla wygody praktyczne w programie Visual Studio dla komputerów Mac, program Visual Studio i Menedżera pakietów NuGet projekty Xamarin umożliwia kilka profilów, które mają tylko implementacje niekompletne. Na przykład Xamarin.iOS ani Xamarin.Android obecnie zawiera pełną implementację typów w `System.Diagnostics.Tracing` PCL przestrzeni nazw. Można obejść to, przełączając projektu aplikacji, aby odwołać portable-net45 + win8 + wp8 + wpa81 wersję biblioteki przepływ danych TPL.

## <a name="nuget-packages--xamarin-components"></a>Pakiety NuGet i składników platformy Xamarin
### <a name="how-can-i-update-nugetnuget-updatemd"></a>[Jak można zaktualizować narzędzie NuGet?](nuget-update.md)
Aktualizacji programu NuGet, rozszerzeń i dodatków, można znaleźć w obszarze **aktualizacje** karcie **Menedżera pakietów NuGet**. Szczegółowe nawigacji, aby znaleźć aktualizacje w programie Visual Studio dla komputerów Mac i Visual Studio znajduje się w tym przewodniku.

### <a name="how-do-i-downgrade-a-nuget-packagenuget-package-downgrademd"></a>[Jak obniżyć wersję pakietu NuGet?](nuget-package-downgrade.md)
Visual Studio dla komputerów Mac i Visual Studio mają funkcje wyboru starsze wersje pakietów i instalując je automatycznie. Podobnie jak aktualizowanie pakietów działa.

### <a name="missing-packages-error-after-updating-nuget-packagesnuget-packages-missingmd"></a>[Po zaktualizowaniu pakietów Nuget jest wyświetlany błąd dotyczący brakujących pakietów](nuget-packages-missing.md)
Ten problem został zgłoszony głównie na rozwiązania aplikacji przykładowej Xamarin.Forms, ale potencjał dla tego problemu może się zdarzyć w żadnym projekcie, który używa pakietów NuGet.

### <a name="unifying-google-play-services-components-and-nugetgps-components-nugetmd"></a>[Ujednolicanie składników usług Google Play i pakietów NuGet](gps-components-nuget.md)
Używane istnieje kilka składników usług Google Play Services i pakiety NuGet, ale w celu ułatwienia deweloperom, firma Microsoft już teraz unified naszych NuGet i składniki pakietów do dwóch. Prawie w każdym przypadku należy używać usług Google Play. Jedyny przypadek, za pomocą pakietu (Froyo) jest, jeśli aktywnie przeznaczonych do pracy Froyo.

### <a name="where-are-the-components-stored-on-my-machinecomponent-storagemd"></a>[Gdzie są przechowywane składniki na moim komputerze?](component-storage.md)
W celu instalowania składników platformy Xamarin w projekcie aplikacji, pobiera go umieścić w dwóch lokalizacjach, które są wymienione w tym przewodniku.


## <a name="troubleshooting"></a>Rozwiązywanie problemów
### <a name="where-can-i-find-my-version-information-and-logsversion-logsmd"></a>[Gdzie mogę znaleźć informacje o wersji i dzienniki?](version-logs.md)
Ten przewodnik zawiera szczegóły gdzie można znaleźć większość informacji diagnostycznych, który może służyć do rozwiązywania problemów w środowisku Xamarin.

### <a name="when-and-how-should-i-file-a-bug-reporthowto-file-bugmd"></a>[Kiedy i jak należy zgłosić raport o usterce?](howto-file-bug.md)
Ten przewodnik zawiera wskazówki dotyczące zgłaszania raportów usterek wysokiej jakości, czemu nasi inżynierowie będą mogli bardziej efektywnie ustalić przyczyny (i wszystkie potencjalne rozwiązania) dla problemu.

### <a name="why-isnt-jenkins-supported-by-xamarinxamarin-jenkinsmd"></a>[Dlaczego system Jenkins nie jest obsługiwany przez środowisko Xamarin?](xamarin-jenkins.md)
Jenkins to pakiet ciągłej integracji typu open-source; z powodu następująca liczba problemów, które bezpośrednio są spowodowane przez narzędzia Jenkins *sam* będzie musiał zostać zgłoszone jako problemy przed skąd masz kod; takie jak [repozytorium głównym Jenkins](https://github.com/jenkinsci/jenkins), lub repozytorium dla [ Jenkins.App](https://github.com/stisti/jenkins-app).

### <a name="what-project-settings-are-required-for-the-debuggerdebugger-settingsmd"></a>[Jakie ustawienia projektu są wymagane dla debugera?](debugger-settings.md)
Aby debuger ma działać zgodnie z oczekiwaniami (trafień punktów przerwania, dzienniki debugowania wyświetlania itp.) wyświetlanie informacji o Instrumentacji i debugowania dla deweloperów należy włączyć. Ten przewodnik zawiera szczegóły Znajdowanie i aktywować te ustawienia.

