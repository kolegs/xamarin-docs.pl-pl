---
title: 'Android.Support.v7.AppCompat - zasobów nie znaleziono odpowiadającego podanej nazwie: attr "android: actionModeShareDrawable"'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5814069C-FC43-41DE-B5A5-024D05E59929
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 07655587642c3e1aa94d035e76f6f6758340546d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawable"></a>Android.Support.v7.AppCompat - zasobów nie znaleziono odpowiadającego podanej nazwie: attr "android: actionModeShareDrawable"

1. Upewnij się, że możesz pobrać najnowsze dodatki, a także Android 5.0 (interfejs API 21) za pośrednictwem Android SDK Manager zestawu SDK.

2. Upewnij się, że Kompilacja aplikacji z ustawioną 21 compileSdkVersion. Opcjonalnie można ustawić targetSdkVersion również 21.

3. Jeśli potrzebujesz poprzedniej wersji, np. interfejsu API 19, Pobierz odpowiedniej wersji znaleźć na stronie Nuget:

[https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)

*Uwaga*: Jeśli ręcznego zainstalowania to za pomocą konsoli Menedżera pakietów, upewnij się, można także zainstalować tę samą wersję programu Xamarin.Android.Support.v4

[https://www.nuget.org/packages/Xamarin.Android.Support.v4/](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)

Odwołanie do przepełnienia stosu: [http://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro](http://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro)

## <a name="see-also"></a>Zobacz też

- [Które pakiety zestawu SDK systemu Android należy zainstalować?](~/android/troubleshooting/questions/install-android-sdk-packages.md)

