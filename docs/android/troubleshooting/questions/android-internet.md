---
title: "Dlaczego moja kompilacji wydania systemu Android nie może połączyć się z Internetem"
ms.topic: article
ms.prod: xamarin
ms.assetid: A6FE770B-A19A-4BF8-95E9-2CF880D4AFC5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: c37a417b84db4028ccea3848a6c152b8ff8d8ead
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="why-cant-my-android-release-build-connect-to-the-internet"></a>Dlaczego moja kompilacji wydania systemu Android nie może połączyć się z Internetem

## <a name="cause"></a>Przyczyna

Najczęstszą przyczyną tego problemu jest to, że **INTERNET** uprawnienie jest automatycznie uwzględnione w kompilacji debugowania, ale należy ręcznie ustawić dla kompilacji wydania. Wynika to z faktu uprawnień internetowych umożliwia debugera dołączyć do procesu, zgodnie z opisem dla "DebugSymbols" [tutaj](~/android/deploy-test/building-apps/build-process.md).


## <a name="fix"></a>Poprawka

Aby rozwiązać ten problem, można wymagać uprawnień internetowych w manifestu systemu Android. Można to zrobić za pomocą edytora manifestu lub sourcecode manifestu:

-   Napraw w edytorze: W projekcie systemu Android, przejdź do **AndroidManifest.xml -> Właściwości -> wymagane uprawnienia** i sprawdź **Internet**

-   Naprawa w Sourcecode: Otwórz AndroidManifest w edytorze źródła i dodać tag uprawnienia wewnątrz `<Manifest>` tagów:

    ```xml
    <Manifest>
    ...
    <uses-permission android:name="android.permission.INTERNET" />
    </Manifest>
    ```
