---
title: Ogólne — często zadawane pytania
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C7E6E54D-3957-407D-BB87-22B095148C6B
author: asb3993
ms.author: amburns
ms.openlocfilehash: b81f5c09ea06acf87449448f43b6c0474a6737b0
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="general-frequently-asked-questions"></a>Ogólne — często zadawane pytania

## <a name="portable-class-libraries"></a>Biblioteki klas przenośnych
### <a name="how-can-i-view-what-libraries-are-supported-in-a-pclpcl-support-librariesmd"></a>[Jak wyświetlić biblioteki obsługiwane w aplikacji PCL?](pcl-support-libraries.md)
Ten przewodnik zawiera listę zasobów i metody sprawdzania, czy istniejące biblioteki jest obsługiwany przez na różnych platformach docelowych PCL lub mogą być konwertowane do profilu PCL.

### <a name="pcl-reflection-apipcl-reflectionmd"></a>[Interfejs API odbicia aplikacji PCL](pcl-reflection.md)
Firma Microsoft opracowała nowy interfejs API odbicia do użycia w przenośnych bibliotekach klas. Jeśli niektóre istniejący kod odbicia, który chcesz przenieść do PCL, może nie działać.

### <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-packagepcl-case-studymd"></a>[Analiza przypadku aplikacji PCL: jak rozwiązać problemy związane z obiektem System.Diagnostics.Tracing dla pakietu NuGet przepływu danych TPL firmy Microsoft?](pcl-case-study.md)
Xamarin.iOS i Xamarin.Android nie implementują 100% każdy profil PCL umożliwiająca jako odwołania. Dla wygody praktyczne w programie Visual Studio for Mac, Visual Studio i Menedżer pakietów NuGet projekty Xamarin zezwala na używanie kilku profilów, które mają tylko implementacje niekompletne. Na przykład Xamarin.iOS ani Xamarin.Android obecnie zawiera pełną implementację typów w `System.Diagnostics.Tracing` PCL przestrzeni nazw. Można obejść to przełączając projektu aplikacji, aby odwołać portable net45 + win8 + wp8 + wpa81 wersji biblioteki przepływu danych TPL.

## <a name="nuget-packages--xamarin-components"></a>Pakiety NuGet & składników Xamarin
### <a name="how-can-i-update-nugetnuget-updatemd"></a>[Jak można zaktualizować narzędzie NuGet?](nuget-update.md)
NuGet aktualizacje, rozszerzenia i dodatki znajduje się w obszarze **aktualizacje** karcie **Menedżera pakietów NuGet**. Szczegółowe nawigacji, aby znaleźć aktualizacje w programie Visual Studio for Mac & Visual Studio jest w tym przewodniku.

### <a name="how-do-i-downgrade-a-nuget-packagenuget-package-downgrademd"></a>[Jak obniżyć wersję pakietu NuGet?](nuget-package-downgrade.md)
Visual Studio for Mac & Visual Studio ma funkcje wyboru starsze wersje pakietów i instalowania ich automatycznie; Podobnie jak aktualizowanie pakietów działa.

### <a name="missing-packages-error-after-updating-nuget-packagesnuget-packages-missingmd"></a>[Po zaktualizowaniu pakietów Nuget jest wyświetlany błąd dotyczący brakujących pakietów](nuget-packages-missing.md)
Ten problem został zgłoszony głównie na platformy Xamarin.Forms przykładowej aplikacji rozwiązania, ale potencjalnych ten problem może się zdarzyć w żadnym projekcie, który używa pakietów NuGet.

### <a name="unifying-google-play-services-components-and-nugetgps-components-nugetmd"></a>[Ujednolicanie składników usług Google Play i pakietów NuGet](gps-components-nuget.md)
Można używać kilka Google Play składników usług i pakiety NuGet, ale w celu ułatwienia dla deweloperów, firma Microsoft zostały teraz unified naszych składników i NuGet pakietów do dwóch. Prawie w każdym przypadku należy używać usług Google Play. Tylko przyczyny umożliwia użycie pakietu (Froyo) jest, jeśli aktywnie przeznaczonych Froyo.

### <a name="where-are-the-components-stored-on-my-machinecomponent-storagemd"></a>[Gdzie są przechowywane składniki na moim komputerze?](component-storage.md)
Podczas instalowania składników Xamarin do projektu aplikacji, pobiera umieszczone w dwóch lokalizacjach, które są wymienione w tym przewodniku.


## <a name="troubleshooting"></a>Rozwiązywanie problemów
### <a name="where-can-i-find-my-version-information-and-logsversion-logsmd"></a>[Gdzie mogę znaleźć informacje o wersji i dzienniki?](version-logs.md)
Ten przewodnik zawiera szczegóły gdzie można znaleźć większość informacji diagnostycznych, które mogą służyć do rozwiązywania problemów Xamarin.

### <a name="when-and-how-should-i-file-a-bug-reporthowto-file-bugmd"></a>[Kiedy i jak należy zgłosić raport o usterce?](howto-file-bug.md)
Ten przewodnik zawiera wskazówki dotyczące składanie raportów usterki wysokiej jakości tak, aby naszych inżynierów mogą się efektywniej określić przyczynę (i wszystkie potencjalne rozwiązania) problemu.

### <a name="why-isnt-jenkins-supported-by-xamarinxamarin-jenkinsmd"></a>[Dlaczego system Jenkins nie jest obsługiwany przez środowisko Xamarin?](xamarin-jenkins.md)
Wpięć to zestaw CI open source; z powodu następująca liczba problemów, które są bezpośrednio spowodowane Wpięć *sam* musi być zgłaszane jako problemów, z którym masz kod; takich jak [głównym repozytorium Wpięć](https://github.com/jenkinsci/jenkins), lub repozytorium dla [ Jenkins.App](https://github.com/stisti/jenkins-app).

### <a name="what-project-settings-are-required-for-the-debuggerdebugger-settingsmd"></a>[Jakie ustawienia projektu są wymagane dla debugera?](debugger-settings.md)
Aby debuger mógł działać zgodnie z oczekiwaniami (trafień punktów przerwania, wyświetlanie dzienników debugowania itd.) wyświetlane informacje instrumentacja i debugowania dewelopera należy zarówno włączyć. Ten przewodnik zawiera szczegóły Znajdowanie i aktywować te ustawienia.

