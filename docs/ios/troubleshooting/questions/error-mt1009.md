---
title: 'Błąd MT1009: Nie można skopiować zestawu'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F9FEDFF5-C84C-42B4-8F25-E34846E7315A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: de1d7ea8ce4c358ad69150be3da6b38fb6c730ae
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30776082"
---
# <a name="error-mt1009-could-not-copy-the-assembly"></a>Błąd MT1009: Nie można skopiować zestawu

> [!IMPORTANT]
> Ten problem został rozwiązany w nowszych wersjach platformy Xamarin.iOS. Jednak jeśli problem wystąpi w najnowszej wersji oprogramowania, pliku [nowych usterek](~/cross-platform/troubleshooting/questions/howto-file-bug.md) z Twojej wersji pełne informacje i pełny dziennik dane wyjściowe kompilacji.

Zgodnie z opisem w naszym [wersji](https://developer.xamarin.com/releases/ios/xamarin.ios_7/xamarin.ios_7.2/), jest to znany problem w Xamarin.iOS 7.2.6. Ten problem jest następstwem uprawnienia do plików wymagających wyższej uprawnienia po zainstalowaniu platformy Xamarin.iOS przy użyciu konta innego użytkownika następnie dewelopera konta głównego.

Aby obejść ten problem Otwórz Terminal.app na stacji roboczej Mac i uruchom następujące polecenie:

`sudo chmod 0644 /Developer/MonoTouch/usr/lib/mono/2.1/monotouch.dll.mdb`

