---
title: 'Xamarin.Essentials: Rozwiązywanie problemów'
description: W tym dokumencie opisano sposób rozwiązywania problemów napotykanych podczas tworzenia za pomocą biblioteki Xamarin.Essentials.
ms.assetid: 2E474FAF-F841-4E3C-B815-F7ABD8EE3361
author: jamesmontemagno
ms.author: jamont
ms.date: 06/26/2018
ms.openlocfilehash: 8cb18ab029d2fd161c60fda7e130f319b8f0c3ab
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947377"
---
# <a name="xamarinessentials-troubleshooting"></a>Xamarin.Essentials: Rozwiązywanie problemów

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

## <a name="error-version-conflict-detected-for-xamarinandroidsupportcompat"></a>Błąd: Wykryto Xamarin.Android.Support.Compat konflikt wersji

Podczas aktualizowania pakietów NuGet może wystąpić następujący błąd (lub dodanie nowego pakietu) przy użyciu projektu Xamarin.Forms, który używa Xamarin.Essentials:

```
NU1107: Version conflict detected for Xamarin.Android.Support.Compat. Reference the package directly from the project to resolve this issue. 
 MyApp -> Xamarin.Essentials 0.8.0-preview -> Xamarin.Android.Support.CustomTabs 27.0.2.1 -> Xamarin.Android.Support.Compat (= 27.0.2.1) 
 MyApp -> Xamarin.Forms 3.1.0.583944 -> Xamarin.Android.Support.v4 25.4.0.2 -> Xamarin.Android.Support.Compat (= 25.4.0.2).
```

Problem polega na niezgodny zależności dla dwóch rozszerzeń Nuget. Ten problem można rozwiązać, ręcznie dodając określoną wersję zależności (w tym przypadku **Xamarin.Android.Support.Compat**) który może obsługiwać obu.

Aby to zrobić, Dodaj pakiet NuGet, który jest źródłem konfliktu ręcznie i użyj **wersji** listę, aby wybrać określoną wersję. Obecnie wersja 27.0.2.1 Xamarin.Android.Support.Compat & Xamarin.Android.Support.Core.Util NuGet rozwiąże ten błąd.

Zapoznaj się [ten wpis w blogu](https://redth.codes/how-to-fix-the-dreaded-version-conflict-nuget-error-in-your-xamarin-android-projects/) więcej informacji oraz film wideo na temat sposobu rozwiązania problemu.

Jeśli wystąpiły wszelkie problemy lub Znajdź usterkę, zgłoś go na [repozytorium Xamarin.Essentials GitHub](http://github.com/xamarin/Essentials).
