---
title: "Lokalizacja aplikacji i zasobów ciągu"
ms.topic: article
ms.prod: xamarin
ms.assetid: 374A9DA6-1853-8B98-6954-7FE3F591C07C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/30/2017
ms.openlocfilehash: a8d25d8780a62e54780d7aa03d81f89fa0f668a4
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="application-localization-and-string-resources"></a>Lokalizacja aplikacji i zasobów ciągu

Lokalizacja aplikacji jest czynnością zapewnienia alternatywnych zasobów pod kątem określonego regionu lub ustawień regionalnych. Na przykład można określić języka zlokalizowane ciągi dla różnych krajów lub można zmienić kolory lub układ do dopasowania określonej kultury. Android obciążenia, a następnie użyć zasobów właściwych dla ustawień regionalnych urządzenia w chwili środowiska uruchomieniowego bez wprowadzania żadnych zmian do kodu źródłowego.

Na przykład na poniższym obrazie pokazano tej samej aplikacji działającej w trzech różnych urządzeniach ustawień regionalnych, ale tekst wyświetlany na każdym przycisku jest specyficzne dla ustawień regionalnych, z których każde urządzenie ma ustawioną wartość:

[![Przykłady trzech różnych ustawień regionalnych](application-localization-images/01-click-me-sml.png)](application-localization-images/01-click-me.png#lightbox)

W tym przykładzie zawartość pliku układu **Main.axml** wygląda następująco:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
   android:orientation="vertical"
   android:layout_width="fill_parent"
   android:layout_height="fill_parent"
   >
<Button  
   android:id="@+id/myButton"
   android:layout_width="fill_parent"
   android:layout_height="wrap_content"
android:text="@string/hello"
   />
</LinearLayout>
```

W powyższym przykładzie ciąg dla przycisku został załadowany z zasobów, podając identyfikator zasobu ciągu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Ciągi zasobów dla trzech językach](application-localization-images/02-resource-strings-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Ciągi zasobów dla trzech językach](application-localization-images/02-resource-strings-xs.png)
 
-----
 
## <a name="localizing-android-apps"></a>Lokalizowanie aplikacji systemu Android

Odczyt [wprowadzenie do lokalizacji](~/cross-platform/app-fundamentals/localization.md) porady i wskazówki dotyczące lokalizowania aplikacji mobilnych.

[Lokalizowania aplikacji systemu Android](~/android/app-fundamentals/localization.md) przewodnik zawiera bardziej szczegółowe przykłady dotyczące translacji ciągów i Lokalizowanie obrazów za pomocą platformy Xamarin.Android.



## <a name="related-links"></a>Linki pokrewne

- [Lokalizowanie aplikacji systemu Android](~/android/app-fundamentals/localization.md)
- [Informacje o lokalizacji i Platform](~/cross-platform/app-fundamentals/localization.md)
