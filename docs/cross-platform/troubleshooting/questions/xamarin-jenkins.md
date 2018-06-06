---
title: Dlaczego Wpięć nie jest obsługiwany przez program Xamarin
description: W tym dokumencie opisano na wysokim poziomie, Xamarin w interakcję z systemem Wpięć elementu CI. Zawiera omówienie również kilka typowych problemów, które znaleziona podczas pracy z Wpięć.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9951F980-2C6C-47C0-8A35-A78F06C20BEB
author: asb3993
ms.author: amburns
ms.openlocfilehash: cf1a59d3084f178187209fdf3999af10efe6203a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782453"
---
# <a name="why-isnt-jenkins-supported-by-xamarin"></a>Dlaczego Wpięć nie jest obsługiwany przez program Xamarin

## <a name="jenkins-support-explanation"></a>Wyjaśnienie Wpięć pomocy technicznej

Wpięć to zestaw CI open source; z powodu następująca liczba problemów, które są bezpośrednio spowodowane Wpięć *sam* musi być zgłaszane jako problemów, z którym masz kod; takich jak [głównym repozytorium Wpięć](https://github.com/jenkinsci/jenkins), lub repozytorium dla [ Jenkins.App](https://github.com/stisti/jenkins-app).

Wyjątkiem jest problemów, które mogą być izolowane do konkretnego usterki w programie Xamarin dla narzędzia. Jeśli podejrzewasz, to w przypadku można sprawdzić Twoje [opcje pomocy technicznej](~/cross-platform/troubleshooting/support-options.md), mimo że ponownie, problem może być coś poza Obsługa platformy Xamarin zespołu można *bezpośrednio* Pomoc.

## <a name="setup-jenkins-with-xamarin"></a>Instalator Wpięć za pomocą platformy Xamarin

Jak wspomniano powyżej problemy z Wpięć nie są obsługiwane bezpośrednio przez nasz zespół; [przy użyciu Wpięć za pomocą platformy Xamarin](~/tools/ci/jenkins-walkthrough.md) Przewodnik może służyć do konfigurowania serwera Wpięć elementu konfiguracji, który jest zintegrowany z platformą Xamarin. 

## <a name="fixes-for-common-issues"></a>Poprawki dla typowych problemów

### <a name="jenkins-is-unable-to-find-the-android-sdk"></a>Nie można odnaleźć zestawu SDK systemu Android jest Wpięć

Komunikat o błędzie dla tego problemu jest podobny do następującego:

> Błąd XA5205: nie można odnaleźć katalogu zestawu SDK systemu Android. Ustaw za pośrednictwem /p:AndroidSdkDirectory

Opcje ustawiania lokalizacji zestawu SDK może się różnić w zależności od dokładnej wtyczki Android Wpięć używanego; dobrym miejscem do wyszukania ustawiania to znajduje się w przewodniku wtyczki. Na przykład; [wtyczki emulatora systemu Android](https://wiki.jenkins-ci.org/display/JENKINS/Android+Emulator+Plugin#AndroidEmulatorPlugin-Systemconfiguration) automatycznie wyszukuje zestaw SDK, ale jeśli nie można go znaleźć; lokalizacji można również ustawić za pomocą strony Wpięć systemu konfiguracji dla tej wtyczki. 


## <a name="deprecated-errors"></a>Błędy przestarzałe

> [!IMPORTANT]
> Ten problem został rozwiązany w nowszych wersjach programu Xamarin. Jednak jeśli problem wystąpi w najnowszej wersji oprogramowania, pliku [nowych usterek](~/cross-platform/troubleshooting/questions/howto-file-bug.md) z Twojej wersji pełne informacje i pełny dziennik dane wyjściowe kompilacji.



### <a name="jenkins-reports-an-invalid-xamarin-license"></a>Wpięć raporty nieprawidłową licencją Xamarin
Komunikaty o błędach tego problemu są zwykle wyglądać mniej więcej tak

> Błąd XA9008: tworzenie z wiersza polecenia wymaga licencji biznesowa

lub

> Błąd: Starter Edition Xamarin.iOS nie obsługuje kompilowania poza Xamarin Studio 

Najczęstszą przyczyną tego scenariusza jest korzystanie z Wpięć, logując się przy użyciu konta użytkownika nie są skojarzone z licencji programu Xamarin. Najprostszym sposobem rozwiązywania tego jest zainstalowanie Wpięć jako aplikacji bezpośrednio za pomocą konta użytkownika. Ten proces i kilka dodatkowych kwestii dotyczących są opisane w tym miejscu: [https://forums.xamarin.com/discussion/comment/99397/#Comment_99397](https://forums.xamarin.com/discussion/comment/99397/#Comment_99397)
