---
title: Wprowadzenie do ciągłej integracji za pomocą platformy Xamarin
description: Ten dokument prowadzi do przewodników, które opisują ciągłej integracji za pomocą platformy Xamarin. Połączona zawartość zawiera omówienie ciągłej integracji i w tym artykule omówiono tworzenie Centrum aplikacji, TeamCity i Jenkins.
ms.prod: xamarin
ms.assetid: 99484E96-DC69-4697-8BBB-1B44C5CBB5ED
author: lobrien
ms.author: laobri
ms.date: 10/03/2018
ms.openlocfilehash: 06392448682a5b3be02562578542919b42242c13
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2018
ms.locfileid: "34793747"
---
# <a name="introduction-to-continuous-integration-with-xamarin"></a>Wprowadzenie do ciągłej integracji za pomocą platformy Xamarin

> [!Video https://youtube.com/embed/wXgnh2Q7Uv8]

## <a name="introduction-to-continuous-integrationtoolsciintro-to-cimd"></a>[Wprowadzenie do ciągłej integracji](~/tools/ci/intro-to-ci.md)

W tej sekcji opisano różne składniki związane z ciągłą integrację i ich wzajemne relacje. Przedstawia środowisk ciągłej integracji, które zostały omówione w poniższych sekcjach określone.

## <a name="working-with-continuous-integration-environments"></a>Praca ze środowiskami ciągłej integracji

### <a name="build-xamarin-apps-with-azure-pipelineshttpsdocsmicrosoftcomazuredevopspipelineslanguagesxamarin"></a>[Kompiluj aplikacje platformy Xamarin z potokiem, Azure](https://docs.microsoft.com/azure/devops/pipelines/languages/xamarin/)

Użyj potoki usługi Azure, aby automatycznie tworzyć aplikacje platformy Xamarin dla systemów Android i iOS.

### <a name="build-xamarin-apps-using-app-centerhttpsdocsmicrosoftcomappcenterbuildxamarin"></a>[Twórz aplikacje platformy Xamarin przy użyciu platformy App Center](https://docs.microsoft.com/appcenter/build/xamarin/)

Xamarin.iOS i Xamarin.Android tworzyć rozwiązania wspólnie z Centrum aplikacji bezpośrednio z usługi GitHub, DevOps platformy Azure lub Bitbucket.

### <a name="build-xamarin-apps-with-teamcitytoolsciteamcitymd"></a>[Twórz aplikacje platformy Xamarin przy użyciu TeamCity](~/tools/ci/teamcity.md)

W przewodniku opisano kroki związane z używaniem TeamCity na potrzeby kompilowania aplikacji mobilnych i przesyłanie ich do App Center Test.

### <a name="build-xamarin-apps-with-jenkinstoolscijenkins-walkthroughmd"></a>[Kompiluj aplikacje platformy Xamarin przy użyciu narzędzia Jenkins](~/tools/ci/jenkins-walkthrough.md)

Ten przewodnik przedstawia sposób konfiguracji narzędzia Jenkins jako serwer ciągłej integracji i automatyzacji kompilowania aplikacji mobilnych tworzone za pomocą platformy Xamarin. Przedstawiono sposób instalowania usługi Jenkins w systemie OS X, jest skonfigurowana i skonfigurować zadania do kompilowania aplikacji platformy Xamarin.iOS i Xamarin.Android, gdy zmiany zostaną zatwierdzone do systemu kontroli wersji.
