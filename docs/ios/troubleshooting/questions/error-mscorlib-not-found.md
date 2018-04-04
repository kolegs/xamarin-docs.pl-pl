---
title: 'Błąd czasu wykonywania: mscorlib.dll zestaw nie został znaleziony lub nie można załadować'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1027E16C-2C14-4BB5-AAAB-342F3E28E22E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 1470396e782fd2343de72e9bea042f45cd07e555
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="runtime-error-the-assembly-mscorlibdll-was-not-found-or-could-not-be-loaded"></a>Błąd czasu wykonywania: mscorlib.dll zestaw nie został znaleziony lub nie można załadować

```
<Warning>: The assembly mscorlib.dll was not found or could not be loaded.
<Warning>: It should have been installed in the `/Developer/MonoTouch/Source/monotouch/builds/install/target64/lib/mono/2.0/mscorlib.dll' directory.
<Warning>: Service exited with abnormal code: 1
```

Ten problem występuje, gdy *ukryte* `.monotouch-32` i `.monotouch-64` brakuje folderów `.xcarchive` do podpisywania / tworzenia IPA wyzwalają błąd w czasie wykonywania.

