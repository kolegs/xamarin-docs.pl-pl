---
title: Która wersja platformy Xamarin.Android dodano obsługę interfejs typu lizak?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63B6E10C-098D-4C82-9253-07CA62EA85A5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 065c68a373f67bb352b59dc88ef89daec8b51ef8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="what-version-of-xamarinandroid-added-lollipop-support"></a>Która wersja platformy Xamarin.Android dodano obsługę interfejs typu lizak?

**Uwaga:** pierwotnie sformatowano w tym przewodniku L systemu Android w wersji zapoznawczej.

-   [Xamarin.Android 4.17](https://developer.xamarin.com/releases/android/xamarin.android_4/xamarin.android_4.17/) dodano obsługę systemu Android L podglądu.
-   [Xamarin.Android 4.20](https://developer.xamarin.com/releases/android/xamarin.android_4/xamarin.android_4.20/) dodano obsługę systemu Android interfejs typu lizak.

Program Xamarin obsługuje tylko aktywnie bieżącego stabilna wersja narzędzia Xamarin. Poniższe informacje są dostarczane "jako — jest" dla wcześniejszych wersji narzędzi. Aby uzyskać najnowsze informacje o wersjach Xamarin, sprawdź, czy [tutaj](http://releases.xamarin.com/).

## <a name="missing-androidjar-for-api-level-21-in-android-l-preview"></a>"Brak android.jar poziom interfejsu API 21" w wersji zapoznawczej L systemu Android

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Następujący komunikat o błędzie (lub podobny) mogą być wyświetlane w:

```cmd
Error 1 Could not find android.jar for API Level 21.
```

Ten komunikat oznacza, że Platforma 21 poziom interfejsu API zestawu SDK systemu Android nie jest zainstalowany. Zainstaluj go w programie Android SDK Manager (Narzędzia > Open Android SDK Manager...), lub Zmień projekt platformy Xamarin.Android pod kątem zainstalowanej wersji interfejsu API.

Istnieje kilka obejścia tego problemu:

1. Zmień projekt tak, aby jego celem interfejsu API 19 lub niższy.

2. Zmień nazwę folderu android 21 od 21 systemu android do L. systemu android (W najlepszym to powinien służyć jedynie jako tymczasowego poprawki i może nie działać bardzo dobrze w ogóle.)

   **%LOCALAPPDATA%\\Android\\android-sdk\\platforms\\android-21**

3. Tymczasowo obniżyć do podglądu Android 21 poziom interfejsu API "L" [1]:

    1.  Usuń **LOCALAPPDATA %\\Android\\zestawu sdk systemu android\\platform\\21 systemu android** 
    2.  Wyodrębnij do [1] **C:\\użytkowników\\<username>\\AppData\\lokalnego\\Android\\zestawu sdk systemu android\\platform** do utworzenia **android-L** folderu.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Następujący komunikat o błędzie (lub podobny) mogą być wyświetlane w:

```bash
/Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: 
Error: Could not find android.jar for API Level 21.**
```

Oznacza to, że Platforma 21 poziom interfejsu API zestawu SDK systemu Android nie jest zainstalowany. Zainstaluj go w programie Android SDK Manager (Narzędzia > SDK Manager), lub Zmień projekt platformy Xamarin.Android pod kątem zainstalowanej wersji interfejsu API.

Istnieje kilka obejścia tego problemu:

1. Zmień projekt tak, aby jego celem interfejsu API 19 lub niższy.

2. Zmień nazwę folderu android 21 od 21 systemu android do L. systemu android (W najlepszym to powinien służyć jedynie jako tymczasowego poprawki i może nie działać bardzo dobrze w ogóle.)

   **~/Library/Developer/Xamarin/android-sdk-macosx/android-21**

3. Tymczasowo obniżyć do podglądu Android 21 poziom interfejsu API "L" [1]:

    1.  Usuń **/Users/username/Library/Developer/Xamarin/android-sdk-macosx/android-21**
    2.  Wyodrębnij do [1] **/Users/username/Library/Developer/Xamarin/android-sdk-macosx** utworzyć **android-L** folderu.

-----


[1] - [https://dl-ssl.google.com/android/repository/android-L_r04.zip](https://dl-ssl.google.com/android/repository/android-L_r04.zip)
