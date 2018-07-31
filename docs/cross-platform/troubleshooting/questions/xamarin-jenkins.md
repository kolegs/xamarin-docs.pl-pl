---
title: Dlaczego narzędzia Jenkins nie jest obsługiwany przez środowisko Xamarin?
description: W tym dokumencie opisano na wysokim poziomie interakcji platformy Xamarin w systemie ciągłej integracji narzędzia Jenkins. Omawia także kilka typowych problemów, które pojawiają się podczas pracy z usługą Jenkins.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9951F980-2C6C-47C0-8A35-A78F06C20BEB
author: asb3993
ms.author: amburns
ms.date: 06/05/2018
ms.openlocfilehash: 54947d04d6241120df4b3a0f60947ed5cc2b7ffd
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351162"
---
# <a name="why-isnt-jenkins-supported-by-xamarin"></a>Dlaczego narzędzia Jenkins nie jest obsługiwany przez środowisko Xamarin?

## <a name="jenkins-support-explanation"></a>Wyjaśnienie pomocy technicznej usługi Jenkins

Jenkins to pakiet ciągłej integracji typu open-source; z powodu następująca liczba problemów, które bezpośrednio są spowodowane przez narzędzia Jenkins *sam* będzie musiał zostać zgłoszone jako problemy przed skąd masz kod; takie jak [repozytorium głównym Jenkins](https://github.com/jenkinsci/jenkins), lub repozytorium dla [ Jenkins.App](https://github.com/stisti/jenkins-app).

Wyjątek jest w przypadku problemów, które mogą być izolowane do konkretnego usterek w narzędzia Xamarin. Jeśli podejrzewasz, to w przypadku możesz sprawdzić swoje [opcje pomocy technicznej](~/cross-platform/troubleshooting/support-options.md), ale ponownie, problem może być coś poza Obsługa platformy Xamarin zespołu można *bezpośrednio* Pomoc dotycząca.

## <a name="setup-jenkins-with-xamarin"></a>Konfigurowanie narzędzia Jenkins z platformą Xamarin

Jak wspomniano powyżej problemy narzędzia Jenkins nie są obsługiwane bezpośrednio przez nasz zespół; [przy użyciu narzędzia Jenkins z platformą Xamarin](~/tools/ci/jenkins-walkthrough.md) Przewodnik może służyć do konfigurowania serwera ciągłej integracji narzędzia Jenkins, który jest zintegrowany ze środowiskiem Xamarin. 

## <a name="fixes-for-common-issues"></a>Rozwiązania typowych problemów

### <a name="jenkins-is-unable-to-find-the-android-sdk"></a>Jenkins to nie można odnaleźć zestawu SDK systemu Android

Komunikat o błędzie dla tego problemu jest podobny do poniższego:

> Błąd XA5205: nie można odnaleźć katalogu zestawu SDK systemu Android. Ustaw za pomocą /p:AndroidSdkDirectory

Opcje do ustawiania lokalizacji zestawu SDK mogą się różnić w zależności od dokładnego dodatek Android narzędzia Jenkins, używanego; Przewodnik dotyczący wtyczki jest dobrym miejscem do wyszukania jak to skonfigurować. Na przykład; [Android Emulator wtyczki](https://wiki.jenkins-ci.org/display/JENKINS/Android+Emulator+Plugin#AndroidEmulatorPlugin-Systemconfiguration) automatycznie wyszukuje zestaw SDK, ale jeśli nie można go znaleźć; lokalizacji można również ustawić za pomocą strony konfiguracji systemu Jenkins dla tej wtyczki. 


## <a name="deprecated-errors"></a>Przestarzałe błędy

> [!IMPORTANT]
> Ten problem został rozwiązany w nowszych wersjach platformy Xamarin. Jednak jeśli ten problem występuje w najnowszej wersji oprogramowania, sprawdź plik [nową usterkę](~/cross-platform/troubleshooting/questions/howto-file-bug.md) za pomocą swojej wersji pełne informacje i pełny dziennik dane wyjściowe kompilacji.



### <a name="jenkins-reports-an-invalid-xamarin-license"></a>Jenkins raporty Nieprawidłowa licencja programu Xamarin
Komunikaty o błędach dla tego problemu to zazwyczaj mniej więcej tak

> Błąd XA9008: tworzenie z wiersza polecenia wymaga licencji firmy

lub

> Błąd: Starter wersji rozszerzenia Xamarin.iOS nie obsługuje tworzenia poza programem Xamarin Studio 

Najczęstszą przyczyną tego scenariusza jest użycie narzędzia Jenkins, logując się przy użyciu konta użytkownika, nie jest skojarzona z Twoja licencja programu Xamarin. Najprostszym sposobem rozwiązania tego jest instalacja narzędzia Jenkins w formie aplikacji bezpośrednio za pomocą konta użytkownika. Ten proces i kilka dodatkowych kwestii dotyczących są opisane poniżej: [https://forums.xamarin.com/discussion/comment/99397/#Comment_99397](https://forums.xamarin.com/discussion/comment/99397/#Comment_99397)
