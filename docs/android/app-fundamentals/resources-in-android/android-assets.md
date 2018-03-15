---
title: "Korzystanie z zasobów systemu Android"
ms.topic: article
ms.prod: xamarin
ms.assetid: 70ECDDC9-FA40-03B4-BF04-E7CFFFE4260D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/13/2018
ms.openlocfilehash: e1890575f5c3a5bd2e0c0de0712ba459607e6139
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="using-android-assets"></a>Korzystanie z zasobów systemu Android

_Zasoby_ umożliwiają uwzględnienie dowolnych plików, takich jak text, xml, czcionki, muzyka i wideo w aplikacji. Jeśli spróbujesz uwzględniają te pliki jako "zasoby" Android będzie przetwarzać je do jej zasobów systemu i nie można pobrać danych pierwotnych. Jeśli chcesz uzyskać dostęp do danych bez zmian, zasoby są jednym z tych metod.

Podobnie jak system plików, który może odczytywać przy użyciu Twojej aplikacji zostaną wyświetlone zasoby dodane do projektu [AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/).
W tym prosty pokaz zamierzamy Dodawanie zasobów pliku tekstowego do naszej projektu odczytu za pomocą `AssetManager`i wyświetlać go w element TextView.


## <a name="add-asset-to-project"></a>Dodawanie zasobów do projektu

Zasoby go w programie `Assets` folderu projektu. Dodaj nowy plik tekstowy do tego folderu o nazwie `read_asset.txt`. Umieść w nim fragment tekstu, np. "I pochodzi od środka trwałego!".

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio powinien mieć ustawiony **Akcja kompilacji** dla tego pliku do **AndroidAsset**:

![Ustawienie akcji kompilacji AndroidAsset](android-assets-images/asset-properties-vs.png) 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Powinien mieć ustawiony programu Visual Studio for Mac **Akcja kompilacji** dla tego pliku do **AndroidAsset**:

[![Ustawienie akcji kompilacji AndroidAsset](android-assets-images/asset-properties-xs-sml.png)](android-assets-images/asset-properties-xs.png#lightbox)

-----

Wybieranie odpowiedniego **BuildAction** gwarantuje, że plik zostaną umieszczone w plik APK w czasie kompilacji.


## <a name="reading-assets"></a>Odczytywanie zasobów

Zasoby są odczytywane przy użyciu [AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/). Wystąpienie `AssetManager` jest dostępne po zalogowaniu się do [zasoby](https://developer.xamarin.com/api/property/Android.Content.Context.Assets/) właściwość `Android.Content.Context`, takie jak działania.
W poniższym kodzie możemy otworzyć naszych **read_asset.txt** zasobów, wczytanie zawartości i wyświetl ją przy użyciu element TextView.

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Create a new TextView and set it as our view
    TextView tv = new TextView (this);
    
    // Read the contents of our asset
    string content;
    AssetManager assets = this.Assets;
    using (StreamReader sr = new StreamReader (assets.Open ("read_asset.txt")))
    {
        content = sr.ReadToEnd ();
    }

    // Set TextView.Text to our asset content
    tv.Text = content;
    SetContentView (tv);
}
```


## <a name="running-the-application"></a>Uruchamianie aplikacji

Uruchom aplikację i powinien zostać wyświetlony poniżej:

![Zrzut ekranu](android-assets-images/screenshot.png)


## <a name="related-links"></a>Linki pokrewne

- [AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/)
- [Kontekst](https://developer.xamarin.com/api/type/Android.Content.Context/)
