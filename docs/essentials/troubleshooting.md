---
title: 'Xamarin.Essentials: Rozwiązywanie problemów'
description: Ten dokument zawiera opis sposobu rozwiązywania problemów napotkanych podczas programowania z użyciem biblioteki Xamarin.Essentials.
ms.assetid: 2E474FAF-F841-4E3C-B815-F7ABD8EE3361
author: jamesmontemagno
ms.author: jamont
ms.date: 06/26/2018
ms.openlocfilehash: 3dba315aec2475cb334110ba7555f773f4165aa1
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066737"
---
# <a name="xamarinessentials-troubleshooting"></a>Xamarin.Essentials: Rozwiązywanie problemów

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

## <a name="error-version-conflict-detected-for-xamarinandroidsupportcompat"></a>Błąd: Konflikt wersji dla Xamarin.Android.Support.Compat wykryte

Następujący błąd może wystąpić podczas aktualizowania pakietów NuGet (lub dodanie nowego pakietu) z projektem platformy Xamarin.Forms, która używa Xamarin.Essentials:

```
NU1107: Version conflict detected for Xamarin.Android.Support.Compat. Reference the package directly from the project to resolve this issue. 
 MyApp -> Xamarin.Essentials 0.7.0.17-preview -> Xamarin.Android.Support.CustomTabs 27.0.2 -> Xamarin.Android.Support.Compat (= 27.0.2) 
 MyApp -> Xamarin.Forms 3.1.0.583944 -> Xamarin.Android.Support.v4 25.4.0.2 -> Xamarin.Android.Support.Compat (= 25.4.0.2).
```

Problem jest niezgodne zależności dla dwóch NuGets. Ten problem można rozwiązać przez ręczne dodanie określonej wersji zależności (w tym przypadku **Xamarin.Android.Support.Compat**) który może obsługiwać obu.

Aby to zrobić, należy dodać NuGet, który jest źródłem konfliktu ręcznie i używać **wersji** listę, aby wybrać określoną wersję. Obecnie wersja 27.0.2 Xamarin.Android.Support.Compat NuGet rozwiąże ten błąd.

Zapoznaj się [ten wpis w blogu](https://redth.codes/how-to-fix-the-dreaded-version-conflict-nuget-error-in-your-xamarin-android-projects/) uzyskać więcej informacji i wideo na temat sposobu rozwiązania problemu.

Jeśli wystąpiły problemy ani Znajdź usterki, zgłoś go na [repozytorium Xamarin.Essentials GitHub](http://github.com/xamarin/Essentials).
