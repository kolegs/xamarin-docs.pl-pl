---
title: Wprowadzenie do ciągłej integracji z platformą Xamarin
description: Ciągła Integracja jest rozwiązaniem engineering oprogramowania, w którym automatycznych kompilacji kompiluje i opcjonalnie testuje aplikację, jeśli kod jest dodane lub zmienione przez deweloperów w repozytorium kontroli wersji projektu. W tym artykule przedstawimy ogólne koncepcje ciągłej integracji i niektóre z dostępnych opcji dla ciągłej integracji z projektami Xamarin.
ms.prod: xamarin
ms.assetid: 99484E96-DC69-4697-8BBB-1B44C5CBB5ED
author: topgenorth
ms.author: toopge
ms.date: 05/04/2017
ms.openlocfilehash: 34838a1527cb3661e8e5ed51b5950f26026e9433
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="introduction-to-continuous-integration-with-xamarin"></a>Wprowadzenie do ciągłej integracji z platformą Xamarin

_Ciągła Integracja jest rozwiązaniem engineering oprogramowania, w którym automatycznych kompilacji kompiluje i opcjonalnie testuje aplikację, jeśli kod jest dodane lub zmienione przez deweloperów w repozytorium kontroli wersji projektu. W tym artykule przedstawimy ogólne koncepcje ciągłej integracji i niektóre z dostępnych opcji dla ciągłej integracji z projektami Xamarin._

> [!Video https://youtube.com/embed/wXgnh2Q7Uv8]


##  <a name="introduction-to-continuous-integrationtoolsciintro-to-cimd"></a>[Wprowadzenie do ciągłej integracji](~/tools/ci/intro-to-ci.md)

W tej sekcji omówiono różne składniki związane z ciągłej integracji i relacji między nimi. Opisano go środowiska ciągłej integracji, które zostały omówione w określonych sekcji poniżej.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="working-with-continuous-integration-environments"></a>Praca w środowiskach ciągłej integracji


### <a name="using-app-center-build-with-xamarinappcenterbuildxamarin"></a>[Korzystanie z kompilacji usług App Center w środowisku Xamarin](/appcenter/build/xamarin/)

Tworzenie rozwiązań platformy Xamarin.iOS i Xamarin.Android w Centrum aplikacji, bezpośrednio z usługi GitHub, programu VSTS lub Bitbucket.

### <a name="using-teamcity-with-xamarintoolsciteamcitymd"></a>[Korzystanie z serwera TeamCity w środowisku Xamarin](~/tools/ci/teamcity.md)

W tym przewodniku opisano kroki związane z używaniem TeamCity do kompilowania aplikacji mobilnych i przesłać je do aplikacji Centrum testu.

### <a name="using-jenkins-with-xamarintoolscijenkins-walkthroughmd"></a>[Korzystanie z systemu Jenkins w środowisku Xamarin](~/tools/ci/jenkins-walkthrough.md)

W tym przewodniku przedstawiono sposób ustawiania Wpięć jako serwer ciągłej integracji i automatyzacji Kompilowanie aplikacji mobilnych utworzone za pomocą platformy Xamarin. Przedstawiono sposób instalowania Wpięć na OS X, jest skonfigurowana i skonfigurować zadania do skompilowania aplikacji platformy Xamarin.iOS i Xamarin.Android podczas zmiany zostały zastosowane w systemie kontroli wersji.
