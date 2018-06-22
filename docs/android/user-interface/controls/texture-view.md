---
title: TextureView
ms.prod: xamarin
ms.assetid: DD1F3D68-5DD8-4644-8A13-08AE7719DE30
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/30/2017
ms.openlocfilehash: 1cd332894deac1e1fb076f647bb0120bda2306da
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763608"
---
# <a name="textureview"></a>TextureView

`TextureView` Klasa jest widoku, który używa 2D renderowanie przyspieszane sprzętowo, aby umożliwić wideo lub OpenGL strumień zawartości, który będzie wyświetlany. Na przykład poniższy zrzut ekranu przedstawia `TextureView` wyświetlanie źródło danych na żywo z aparatu fotograficznego urządzenia:

[![Zrzut ekranu na żywo obrazu z aparatu fotograficznego urządzenia](texture-view-images/22-textureviewcamera.png)](texture-view-images/22-textureviewcamera.png#lightbox)

W odróżnieniu od `SurfaceView` klasy, które mogą służyć do wyświetlenia OpenGL lub zawartości wideo, TextureView nie są odtwarzane w osobnym oknie.
W związku z tym `TextureView` umożliwiającą obsługę przekształcenia widoku, podobnie jak inne widoki. Na przykład, obracanie `TextureView` można osiągnąć wystarczy wybrać ustawienie jej `Rotation` właściwości, przezroczystość przez ustawienie jej `Alpha` właściwości i tak dalej.

W związku z tym z `TextureView` możemy teraz wykonywać następujące czynności wyświetlania strumień na żywo z aparatu fotograficznego i przekształcenie go, jak pokazano w poniższym kodzie:

```csharp
public class TextureViewActivity : Activity,
    TextureView.ISurfaceTextureListener
{
    Camera _camera;
    TextureView _textureView;
       
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        _textureView = new TextureView (this);
        _textureView.SurfaceTextureListener = this;
           
        SetContentView (_textureView);
    }
       
    public void OnSurfaceTextureAvailable (
        Android.Graphics.SurfaceTexture surface,
        int width, int height)
    {
        _camera = Camera.Open ();
        var previewSize = _camera.GetParameters ().PreviewSize;
        _textureView.LayoutParameters =
            new FrameLayout.LayoutParams (previewSize.Width,
                previewSize.Height, (int)GravityFlags.Center);

        try {
            _camera.SetPreviewTexture (surface);
            _camera.StartPreview ();
        } catch (Java.IO.IOException ex) {
            Console.WriteLine (ex.Message);
        }
           
        // this is the sort of thing TextureView enables
        _textureView.Rotation = 45.0f;
        _textureView.Alpha = 0.5f;
    }
    …
}
```

Powyższy kod tworzy `TextureView` wystąpienia w działaniu `OnCreate` — metoda i ustawia działanie jako `TextureView`w `SurfaceTextureListener`. Być `SurfaceTextureListener`, działanie implementuje `TextureView.ISurfaceTextureListener` interfejsu. System będzie wywoływać `OnSurfaceTextAvailable` metody podczas `SurfaceTexture` jest gotowy do użycia. W przypadku tej metody traktujemy `SurfaceTexture` czy jest przekazano i ustaw ją na tekstury Podgląd aparatu. Następnie możemy wolnego wykonywanie zwykłych operacji na podstawie widoku, takie jak ustawienie `Rotation` i `Alpha`, jak w powyższym przykładzie. Wynikowa aplikacji, uruchamianie na urządzeniu, przedstawiono poniżej:

[![Przykład aplikacji działającej na urządzeniu, wyświetlanie obrazów](texture-view-images/17-textureviewdemo.png)](texture-view-images/17-textureviewdemo.png#lightbox)

Aby użyć `TextureView`, przyspieszanie sprzętowe musi być włączona, który będzie domyślnie począwszy od 14 poziom interfejsu API. Również na to, że w tym przykładzie użyto aparatu, zarówno `android.permission.CAMERA` uprawnień i `android.hardware.camera` funkcji musi być ustawiona w **AndroidManifest.xml**.



## <a name="related-links"></a>Linki pokrewne

- [TextureViewDemo (przykład)](https://developer.xamarin.com/samples/monodroid/TextureViewDemo/)
- [Wprowadzenie Sandwich lodów](http://www.android.com/about/ice-cream-sandwich/)
- [Platformy systemu android 4.0](http://developer.android.com/sdk/android-4.0.html)
