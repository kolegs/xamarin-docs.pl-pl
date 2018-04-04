---
title: Często zadawane pytania
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 89364175-53BA-4A09-B3E2-44AC67DD971C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: c0c8a6f4736bdcbb028425296f2e05dd500294d9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="frequently-asked-questions"></a>Często zadawane pytania


## <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-packageupdate-forms-templatemd"></a>[Czy mogę zaktualizować domyślny szablon zestawu narzędzi Xamarin.Forms do nowszego pakietu NuGet?](update-forms-template.md)
W tym przewodniku używa szablonu platformy Xamarin.Forms PCL, na przykład, ale tej samej metody ogólne również będzie działać dla szablonu projektu udostępnionego platformy Xamarin.Forms. 

## <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-filesforms-xaml-designermd"></a>[Dlaczego projektant XAML programu Visual Studio nie działa dla plików XAML zestawu narzędzi Xamarin.Forms?](forms-xaml-designer.md)
Platformy Xamarin.Forms nie obsługuje obecnie wizualnych projektantów plików XAML.

## <a name="android-build-error-the-linkassemblies-task-failed-unexpectedlyandroid-linkassemblies-errormd"></a>[Błąd kompilacji systemu Android: zadanie „LinkAssemblies” nieoczekiwanie zakończyło się niepowodzeniem](android-linkassemblies-error.md)
Zobaczysz komunikat o błędzie `The "LinkAssemblies" task failed unexpectedly` podczas kompilowania projektu platformy Xamarin.Android, który używa formularzy. Dzieje się tak podczas konsolidator jest aktywne (zwykle na *wersji* kompilacji, aby zmniejszyć rozmiar pakietu aplikacji); i występuje, ponieważ docelowych z systemem Android nie są zaktualizowane do najnowszej framework. 


## <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik--unexpected-top-level-errormaps-compiletodalvik-errormd"></a>["Dlaczego projektu Xamarin.Forms.Maps Android powiedzie z COMPILETODALVIK: nieoczekiwany błąd na najwyższym poziomie?"](maps-compiletodalvik-error.md)
Tego błędu mogą być widoczne w konsoli błąd programu Visual Studio dla komputerów Mac lub w oknie wyników kompilacji programu Visual Studio; w projektach systemu Android przy użyciu Xamarin.Forms.Maps. Najczęściej problemu przez odpowiednie zwiększenie rozmiaru sterty Java w projekcie platformy Xamarin.Android.

