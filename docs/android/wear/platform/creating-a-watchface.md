---
title: "Tworzenie krój czujki"
description: "W tym przewodniku objaśniono sposób implementacji usługi krój czujki niestandardowych dla systemu Android nosić. Krok po kroku instrukcje tworzenia pozbawionego włókien dół usługi krój czujki cyfrowych, następuje więcej kod w celu utworzenia krój czujki analogowy stylu."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4D3F9A40-A820-458D-A12A-D784BB11F643
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: fb3a2a9e60bda2a99a719bf75d23c29d42a94bdb
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="creating-a-watch-face"></a>Tworzenie krój czujki

_W tym przewodniku objaśniono sposób implementacji usługi krój czujki niestandardowych dla systemu Android nosić. Krok po kroku instrukcje tworzenia pozbawionego włókien dół usługi krój czujki cyfrowych, następuje więcej kod w celu utworzenia krój czujki analogowy stylu._

## <a name="overview"></a>Omówienie 

W tym przewodniku usługi krój czujki podstawowa jest tworzony w celu zilustrowania essentials tworzenia niestandardowych systemu Android nosić krój czujki. Usługa krój początkowej czujki Wyświetla prosty czujki cyfrowe wyświetlający bieżącego czasu w godzinach i minutach: 

[![Cyfrowe czujki krój](creating-a-watchface-images/01-initial-face.png "zrzut ekranu powierzchni początkowej cyfrowe czujki")](creating-a-watchface-images/01-initial-face.png#lightbox)

Po tej powierzchni czujki cyfrowego jest opracowany i przetestowany, uaktualnić go do bardziej zaawansowane opcje analogowy czujki krojów z trzech ręce więcej kodu został dodany: 

[![Obejrzyj analogowy krój](creating-a-watchface-images/02-example-watchface.png "zrzut ekranu powierzchni końcowego analogowy czujki")](creating-a-watchface-images/02-example-watchface.png#lightbox)

Obejrzyj krój usługi są powiązane i zainstalowane w ramach aplikacji zużycia. W poniższych przykładach `MainActivity` zawiera więcej niż kodu z szablonu aplikacji zużycia tak, aby usługi krój czujki można spakowany i wdrożone inteligentne czujki w ramach aplikacji. W efekcie tej aplikacji będzie służyć wyłącznie jako pierwsze usługi krój czujki ładowane do zużycia urządzenia (lub w emulatorze) narzędzia do debugowania i testowania. 

## <a name="requirements"></a>Wymagania

Do wdrożenia usługi krój czujki, wymagane są następujące elementy:

-   Android 5.0 (poziom interfejsu API 21) lub nowszej na zużycia urządzenia lub emulatora.

-   [Xamarin Android nosić bibliotek obsługi](https://www.nuget.org/packages/Xamarin.Android.Wear) musi zostać dodany do projektu platformy Xamarin.Android. 

Mimo że Android 5.0 jest minimalny interfejs API na poziomie implementacji usługi krój watch, Android 5.1 lub nowszy jest zalecane. Android nosić urządzeń z systemem Android w wersji 5.1 (22 interfejsu API) lub wyższej Zezwalaj aplikacjom zużycia kontrolować, co jest wyświetlane na ekranie, gdy urządzenie jest w niskiego poboru energii *otoczenia* tryb. Jeśli urządzenie opuści niskiego poboru energii *otoczenia* tryb, jest on *interakcyjne* tryb. Aby uzyskać więcej informacji o tych trybach, zobacz [utrzymywanie Your App widoczne](https://developer.android.com/training/wearables/apps/always-on.html). 


## <a name="start-an-app-project"></a>Projekt aplikacji

Utwórz nowy projekt dla systemu Android nosić o nazwie **WatchFace** (Aby uzyskać więcej informacji na temat tworzenia nowych projektów platformy Xamarin.Android, zobacz [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md)):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Okno dialogowe nowego projektu](creating-a-watchface-images/03-wear-project-vs-sml.png "wybierz nosić aplikacji w oknie dialogowym Nowy projekt")](creating-a-watchface-images/03-wear-project-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Okno dialogowe nowego projektu](creating-a-watchface-images/03-wear-project-xs-sml.png "wybierz nosić aplikacji w oknie dialogowym Nowy projekt")](creating-a-watchface-images/03-wear-project-xs.png#lightbox)

-----


Ustaw nazwę pakietu `com.xamarin.watchface`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Ustawienie nazwy pakietu](creating-a-watchface-images/04-package-name-vs.png "Ustaw nazwę pakietu com.xamarin.watchface")](creating-a-watchface-images/04-package-name-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Ustawienie nazwy pakietu](creating-a-watchface-images/04-package-name-xs.png "Ustaw nazwę pakietu com.xamarin.watchface")](creating-a-watchface-images/04-package-name-xs.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Ponadto, przewiń w dół i włączyć **INTERNET** i **WAKE_LOCK** uprawnienia: 

[![Wymagane uprawnienia](creating-a-watchface-images/05-required-permissions-vs.png "włączyć internetowe i WAKE_LOCK uprawnień")](creating-a-watchface-images/05-required-permissions-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Ustaw wersję Minimum Android do **5.1 systemu Android (interfejs API na poziomie 22)**. Ponadto Włącz **Internet** i **WakeLock** uprawnienia:

[![Wymagane uprawnienia](creating-a-watchface-images/05-required-permissions-xs.png "włączyć internetowe i WakeLock uprawnień")](creating-a-watchface-images/05-required-permissions-xs.png#lightbox)

-----

Następnie należy pobrać [preview.png](creating-a-watchface-images/preview.png) &ndash; zostanie ona dodana do **drawables** folderu w dalszej części tego przewodnika.


## <a name="add-the-xamarin-android-wear-package"></a>Dodaj pakiet zużycia dla systemu Xamarin Android

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Uruchom Menedżera pakietów NuGet (w programie Visual Studio, kliknij prawym przyciskiem myszy **odwołania** w **Eksploratora rozwiązań** i wybierz **Zarządzaj pakietami NuGet...** ). Aktualizowanie projektu do najnowszej wersji stabilnej **Xamarin.Android.Wear**: 

[![Dodaj Menedżera pakietów NuGet](creating-a-watchface-images/06-add-wear-pkg-vs-sml.png "Dodaj pakiet Xamarin.Android.Wear")](creating-a-watchface-images/06-add-wear-pkg-vs.png#lightbox)

Następnie, jeśli **Xamarin.Android.Support.v13** jest zainstalowane, odinstaluj go:

[![Usuń Menedżera pakietów NuGet](creating-a-watchface-images/07-uninstall-v13-sml.png "Usuń Xamarin.Support.v13")](creating-a-watchface-images/07-uninstall-v13.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Uruchom Menedżera pakietów NuGet (w programie Visual Studio for Mac, kliknij prawym przyciskiem myszy **pakiety** w **okienko rozwiązania** i wybierz **Dodawanie pakietów** ). Aktualizowanie projektu do najnowszej wersji stabilnej **Xamarin.Android.Wear**: 

[![Dodaj Menedżera pakietów NuGet](creating-a-watchface-images/06-add-wear-pkg-xs-sml.png "Dodaj pakiet Xamarin.Android.Wear")](creating-a-watchface-images/06-add-wear-pkg-xs.png#lightbox)

-----


Tworzenie i uruchamianie aplikacji na zużycia urządzenia lub emulatora (Aby uzyskać więcej informacji o tym, jak to zrobić, zobacz [wprowadzenie](~/android/wear/get-started/index.md) przewodniku). Powinien zostać wyświetlony następujący ekran aplikacji na urządzeniu zużycia:

[![Zrzut ekranu aplikacji](creating-a-watchface-images/08-app-screen.png "ekranu aplikacji na urządzeniu zużycie")](creating-a-watchface-images/08-app-screen.png#lightbox)

Podstawowa aplikacja zużycia nie ma w tym momencie czujki krój funkcji, ponieważ nie dostarcza jeszcze implementacji usługi krój czujki. Ta usługa zostanie dodany dalej. 

 
## <a name="canvaswatchfaceservice"></a>CanvasWatchFaceService

Android implementuje zużycia Obejrzyj kroje za pośrednictwem `CanvasWatchFaceService` klasy. `CanvasWatchFaceService` jest pochodną `WatchFaceService`, które jest pochodną `WallpaperService` jak pokazano na poniższym diagramie: 

[![Diagram dziedziczenia](creating-a-watchface-images/09-inheritance-diagram-sml.png "CanvasWatchFaceService diagram dziedziczenia")](creating-a-watchface-images/09-inheritance-diagram.png#lightbox)

`CanvasWatchFaceService` zawiera zagnieżdżoną `CanvasWatchFaceService.Engine`; metoda tworzy `CanvasWatchFaceService.Engine` obiektu, który wykonuje faktyczną pracę rysowania kroju czujki. `CanvasWatchFaceService.Engine` jest pochodną `WallpaperService.Engine` jak pokazano na powyższym diagramie. 

Nie są wyświetlane na tym diagramie jest `Canvas` który `CanvasWatchFaceService` używa do rysowania kroju czujki &ndash; to `Canvas` przekazany za pośrednictwem `OnDraw` metody zgodnie z poniższym opisem. 

W poniższych sekcjach zostaną utworzone usługi krój niestandardowych czujki wykonaj następujące czynności: 

1.  Zdefiniuj klasy o nazwie `MyWatchFaceService` która jest pochodną `CanvasWatchFaceService`. 

2.  W ramach `MyWatchFaceService`, Utwórz zagnieżdżone klasy o nazwie `MyWatchFaceEngine` która jest pochodną `CanvasWatchFaceService.Engine`. 

3.  W `MyWatchFaceService`, wdrożenie `CreateEngine` metodę, która tworzy `MyWatchFaceEngine` i zwraca go.

4.  W `MyWatchFaceEngine`, wdrożenie `OnCreate` metody do tworzenia stylu powierzchni wyrażenie kontrolne i wykonywanie innych zadań inicjowania. 

5.  Implementowanie `OnDraw` metody `MyWatchFaceEngine`. Ta metoda jest wywoływana, gdy kroju czujki musi być narysowany ponownie (tj. *unieważniona*). `OnDraw` jest to metoda rysuje (i ponownie rysuje) czujki przedniej elementów, takich jak godzinę, minutę i drugi ręce. 

6.  Implementowanie `OnTimeTick` metody `MyWatchFaceEngine`. 
    `OnTimeTick` jest wywoływana co najmniej raz na minutę (w trybach otoczenia i interaktywne) lub zmiany daty/godziny. 

Aby uzyskać więcej informacji na temat `CanvasWatchFaceService`, zobacz Android [CanvasWatchFaceService](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.html) dokumentacji interfejsu API.
Podobnie [CanvasWatchFaceService.Engine](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.Engine.html) wyjaśniono rzeczywistego wykonania kroju czujki.


### <a name="add-the-canvaswatchfaceservice"></a>Dodaj CanvasWatchFaceService

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Dodaj nowy plik o nazwie **MyWatchFaceService.cs** (w programie Visual Studio, kliknij prawym przyciskiem myszy **WatchFace** w **Eksploratora rozwiązań**, kliknij przycisk **Dodaj > Nowy element...** i wybierz **klasy**).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Dodaj nowy plik o nazwie **MyWatchFaceService.cs** (w programie Visual Studio for Mac, kliknij prawym przyciskiem myszy **WatchFace** projektu, kliknij przycisk **Dodaj > Nowy plik...** i wybierz **pustą klasę**). 

-----

Zastąp zawartość pliku następującym kodem: 

```csharp
using System;
using Android.Views;
using Android.Support.Wearable.Watchface;
using Android.Service.Wallpaper;
using Android.Graphics;

namespace WatchFace
{
    class MyWatchFaceService : CanvasWatchFaceService
    {
        public override WallpaperService.Engine OnCreateEngine()
        {
            return new MyWatchFaceEngine(this);
        }

        public class MyWatchFaceEngine : CanvasWatchFaceService.Engine
        {
            CanvasWatchFaceService owner;
            public MyWatchFaceEngine (CanvasWatchFaceService owner) : base(owner)
            {
                this.owner = owner;
            }
        }
    }
}
```

`MyWatchFaceService` (pochodną `CanvasWatchFaceService`) to "główny program" kroju czujki. `MyWatchFaceService` implementuje tylko jedna metoda `OnCreateEngine`, które tworzy i zwraca `MyWatchFaceEngine` obiektu (`MyWatchFaceEngine` jest pochodną `CanvasWatchFaceService.Engine`). Skonkretyzowanym `MyWatchFaceEngine` musi być zwracany obiekt jako `WallpaperService.Engine`. Zawieranie `MyWatchFaceService` obiekt został przekazany do konstruktora. 

`MyWatchFaceEngine` jest implementacją krój rzeczywiste czujki &ndash; zawiera kod, który pobiera kroju czujki. Obsługuje on również zdarzenia systemowe, takie jak zmiany ekranu (otoczenia/interactive tryby ekranu wyłączenie itp.). 

 
### <a name="implement-the-engine-oncreate-method"></a>Zaimplementuj metodę OnCreate aparatu

`OnCreate` Metoda inicjuje kroju czujki. Dodaj następujące pole do `MyWatchFaceEngine`: 

```csharp
Paint hoursPaint;
```

To `Paint` obiekt będzie używany do rysowania bieżący czas na powierzchni czujki. Następnie dodaj następującą metodę do `MyWatchFaceEngine`: 

```csharp
public override void OnCreate(ISurfaceHolder holder)
{
    base.OnCreate (holder);

    SetWatchFaceStyle (new WatchFaceStyle.Builder(owner)
        .SetCardPeekMode (WatchFaceStyle.PeekModeShort)
        .SetBackgroundVisibility (WatchFaceStyle.BackgroundVisibilityInterruptive)
        .SetShowSystemUiTime (false)
        .Build ());

    hoursPaint = new Paint();
    hoursPaint.Color = Color.White;
    hoursPaint.TextSize = 48f;
}
```

`OnCreate` nazywa się wkrótce po `MyWatchFaceEngine` została uruchomiona. Konfiguruje `WatchFaceStyle` (która kontroluje sposób zużycia urządzenia interakcji z użytkownikiem) i tworzy `Paint` obiekt, który będzie używany do wyświetlania czasu. 

Wywołanie `SetWatchFaceStyle` wykonuje następujące czynności:

1.  Ustawia *trybu podglądu* do `PeekModeShort`, co powoduje, że powiadomienia są wyświetlane jako karty małe "peek" na ekranie. 

2.  Ustawia widoczność tła `Interruptive`, co powoduje, że tło karty peek ma być wyświetlany tylko na krótko Jeśli reprezentuje interruptive powiadomień.

3.  Wyłącza domyślny czas interfejsu użytkownika systemu z sformułowane na powierzchni czujki tak, aby kroju czujki niestandardowych można zamiast tego wyświetlenie czasu.

Aby uzyskać więcej informacji na temat tych i innych opcji styl krój czujki, zobacz Android [WatchFaceStyle.Builder](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceStyle.Builder.html) dokumentacji interfejsu API.

Po `SetWatchFaceStyle` zakończeniu `OnCreate` tworzy `Paint` obiektu (`hoursPaint`) i ustawia kolor biały i jego rozmiar tekstu do 48 pikseli ([wartość parametru TextSize](https://developer.android.com/reference/android/graphics/Paint.html#setTextSize%28float%29) musi zostać określona w pikselach). 


### <a name="implement-the-engine-ondraw-method"></a>Zaimplementuj metodę OnDraw aparatu

`OnDraw` Metody jest prawdopodobnie najważniejsze `CanvasWatchFaceService.Engine` metody &ndash; jest metoda, że faktycznie rysuje Obejrzyj przedniej elementów, takich jak cyfr i zegara ręce twarzy na obrazie. W poniższym przykładzie go rysuje ciąg godziny na powierzchni czujki.
Dodaj następującą metodę do `MyWatchFaceEngine`:

```csharp
public override void OnDraw (Canvas canvas, Rect frame)
{
    var str = DateTime.Now.ToString ("h:mm tt");
    canvas.DrawText (str, 
        (float)(frame.Left + 70), 
        (float)(frame.Top  + 80), hoursPaint);
}
```

Gdy wywołuje Android `OnDraw`, przechodzą w `Canvas` wystąpienia i granice, w których mogą być wystawiane kroju. W powyższym przykładzie kodu `DateTime` służy do obliczania bieżącego czasu w godzinach i minutach (w formacie 12-godzinnym). Wynikowy ciąg czasu za pomocą rysowania na kanwie `Canvas.DrawText` metody. Ciąg pojawi się 70 pikseli za pośrednictwem od lewej krawędzi i 80 pikseli w dół od górnej krawędzi. 

Aby uzyskać więcej informacji na temat `OnDraw` metody, zobacz Android [onDraw](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.Engine.html#onDraw(android.graphics.Canvas, android.graphics.Rect)) dokumentacji interfejsu API.


### <a name="implement-the-engine-ontimetick-method"></a>Zaimplementuj metodę OnTimeTick aparatu

Android okresowo wywołuje `OnTimeTick` metody, aby zaktualizować wyświetlane przez kroju czujki czas. Jest to, na co najmniej raz na minutę (w trybach otoczenia i interakcyjne), lub gdy zmieniono daty/godziny i strefy czasowej. Dodaj następującą metodę do `MyWatchFaceEngine`: 

```csharp
public override void OnTimeTick()
{
    Invalidate();
}
```

Ta implementacja `OnTimeTick` po prostu wywołuje `Invalidate`. `Invalidate` Harmonogramy metody `OnDraw` ponowne kroju czujki. 

Aby uzyskać więcej informacji na temat `OnTimeTick` metody, zobacz Android [onTimeTick](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onTimeTick()) dokumentacji interfejsu API.

 
## <a name="register-the-canvaswatchfaceservice"></a>Zarejestruj CanvasWatchFaceService

`MyWatchFaceService` musi być zarejestrowana w **AndroidManifest.xml** skojarzonych aplikacji zużycia. Aby to zrobić, Dodaj następujący kod XML `<application>` sekcji: 

```xml
<service 
    android:name="watchface.MyWatchFaceService" 
    android:label="Xamarin Sample" 
    android:allowEmbedded="true" 
    android:taskAffinity="" 
    android:permission="android.permission.BIND_WALLPAPER">
    <meta-data 
        android:name="android.service.wallpaper" 
        android:resource="@xml/watch_face" />
    <meta-data 
        android:name="com.google.android.wearable.watchface.preview" 
        android:resource="@drawable/preview" />
    <intent-filter>
        <action android:name="android.service.wallpaper.WallpaperService" />
        <category android:name="com.google.android.wearable.watchface.category.WATCH_FACE" />
    </intent-filter>
</service>
```

Plik XML wykonuje następujące czynności:

1.  Ustawia `android.permission.BIND_WALLPAPER` uprawnienia. To uprawnienie daje uprawnienia usługi krój czujki zmienić tapety systemu na urządzeniu. Należy pamiętać, że to uprawnienie musi być ustawiona w `<service>` sekcji, a nie w zewnętrznego `<application>` sekcji. 

2.  Definiuje `watch_face` zasobów. Ten zasób jest krótki plik XML, który deklaruje `wallpaper` zasobów (ten plik zostanie utworzony w następnej sekcji). 

3.  Deklaruje obiektów drawable obrazu o nazwie `preview` który będzie wyświetlany ekran wyboru czujki selektora. 

4.  Obejmuje `intent-filter` aby umożliwić Android pamiętać, że `MyWatchFaceSevice` będą wyświetlane krój czujki. 

Na tym kończy się kod podstawowe `WatchFace` przykład. Następnym krokiem jest, aby dodać zasoby niezbędne.

 
## <a name="add-resource-files"></a>Dodaj pliki zasobów

Przed uruchomieniem usługi czujki, należy dodać **watch_face** zasobów i obrazu podglądu. Najpierw utwórz nowy plik XML na **Resources/xml/watch_face.xml** i zastąp jego zawartość XML następujące: 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<wallpaper xmlns:android="http://schemas.android.com/apk/res/android" />
```

Ten plik Akcja kompilacji ustawioną **AndroidResource**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Akcja kompilacji](creating-a-watchface-images/10-android-resource-vs.png "zestaw akcji do AndroidResource kompilacji")](creating-a-watchface-images/10-android-resource-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Akcja kompilacji](creating-a-watchface-images/10-android-resource-xs.png "zestaw akcji do AndroidResource kompilacji")](creating-a-watchface-images/10-android-resource-xs.png#lightbox)

-----

Plik zasobów definiuje prosty `wallpaper` element, który będzie używany dla powierzchni czujki. 

Jeśli nie zostało to jeszcze zrobione, Pobierz [preview.png](creating-a-watchface-images/preview.png). Zainstaluj ją na **Resources/drawable/preview.png**. Pamiętaj dodać ten plik do `WatchFace` projektu. Ten obraz podglądu jest wyświetlany użytkownikowi w selektorze krój czujki zużycia urządzenia. Aby utworzyć obraz podglądu dla powierzchni własne wyrażenie kontrolne, można wykonać zrzut ekranu kroju wyrażenie kontrolne jest uruchomiona. (Aby uzyskać więcej informacji o pobierania zrzuty ekranu urządzenia zużycia zobacz [biorąc zrzuty ekranu](~/android/wear/deploy-test/debug-on-device.md#screenshots)). 


## <a name="try-it"></a>Wypróbuj!

Tworzenie i wdrażanie aplikacji do zużycia urządzenia. Powinien zostać wyświetlony ekran aplikacji zużycia pojawiają się jak poprzednio. Wykonaj następujące polecenie, aby włączyć nowe kroju czujki: 

1.  Przejdź do prawej, aż zostanie wyświetlony tła ekranu czujki. 

2.  Touch i przytrzymaj dowolnym tła ekranu dla dwóch sekund.

3.  Przejdź od lewej do prawej strony, aby przeglądać różne kroje czujki. 

4.  Wybierz **próbki Xamarin** Obejrzyj twarzy na obrazie (wyświetlane po prawej stronie): 

    [![Selektor Watchface](creating-a-watchface-images/11-watchface-picker.png "przejdź do zlokalizowania krój czujki próbki Xamarin")](creating-a-watchface-images/11-watchface-picker.png#lightbox)

5.  Wybierz **próbki Xamarin** Obejrzyj krój, aby go wybrać. 

Spowoduje to zmianę kroju czujki zużycia urządzenia ma być używana usługa krój niestandardowych czujki dotąd zaimplementowana: 

[![Cyfrowe czujki krój](creating-a-watchface-images/12-digital-watchface.png "niestandardowe czujki cyfrowe działającej na urządzeniu zużycie")](creating-a-watchface-images/12-digital-watchface.png#lightbox)

To krój stosunkowo surowych czujki, ponieważ wdrożenia aplikacji jest więc minimalne (na przykład nie zawierają czujki twarzy na obrazie tła, a nie jej wywołać `Paint` metody wygładzanie poprawy wygląd). Jednak implementuje funkcje kości bez systemu operacyjnego, które są wymagane do utworzenia krój czujki niestandardowych. 

W następnej sekcji tego krój czujki zostanie uaktualniona do bardziej złożone wdrożenia. 

 
## <a name="upgrading-the-watch-face"></a>Uaktualnianie kroju czujki

W dalszej części tego przewodnika `MyWatchFaceService` zostaje uaktualniony do wyświetlenia krój czujki analogowy stylu i rozszerzony do obsługi więcej funkcji. Następujące funkcje zostaną dodane do tworzenia kroju czujki uaktualniony: 

1.  Wskazuje godzinę, z analogowy godzinę, minutę i drugi ręce.

2.  W przypadku zmiany widoczności.

3.  Reaguje na zmiany między trybem otoczenia i tryb interaktywny. 

4.  Odczytuje właściwości podstawowej zużycia urządzenia.

5.  Automatycznie aktualizuje czasu, gdy odbywa się zmiany strefy czasowej. 

Przed wdrożeniem poniższe zmiany kodu, Pobierz [drawable.zip](https://github.com/xamarin/monodroid-samples/blob/master/wear/WatchFace/Resources/drawable.zip?raw=true), rozpakowania go i Przenieś pliki PNG rozpakowane **obiektów drawable/zasoby** (zastąpienie poprzedniej **preview.png**). Dodaj nowe pliki PNG `WatchFace` projektu.


### <a name="update-engine-features"></a>Funkcje aparatu aktualizacji

Następnym krokiem jest uaktualnienie **MyWatchFaceService.cs** implementacji rysuje analogowy czujki krój, który obsługuje nowe funkcje. Zastąp zawartość **MyWatchFaceService.cs** wersją analogowy kodu krój czujki w [MyWatchFaceService.cs](https://github.com/xamarin/monodroid-samples/blob/master/wear/WatchFace/WatchFace/MyWatchFaceService.cs) (można wyciąć i wkleić tego źródła do istniejącego  **MyWatchFaceService.cs**). 

Ta wersja **MyWatchFaceService.cs** dodaje więcej kod do metody i są dostępne dodatkowe przesłoniętej metody dodać więcej funkcji. Poniższe sekcje zawierają przewodnik kodu źródłowego.

#### <a name="oncreate"></a>OnCreate

Zaktualizowany interfejs **OnCreate** metody konfiguruje stylu powierzchni czujki jak poprzednio, ale obejmuje kilka dodatkowych kroków: 

1.  Ustawia obraz tła **xamarin_background** zasobów, która znajduje się w **Resources/drawable-hdpi/xamarin_background.png**. 

2.  Inicjuje `Paint` obiektów rysowania wskazówki godzin, minut i drugiej wskazówki.

3.  Inicjuje `Paint` obiekt do rysowania Takty godzina wokół krawędzi kroju czujki. 

4.  Tworzy czasomierz tego wywołania `Invalidate` — metoda (ponownego rysowania), aby sekund zostanie narysowany ponownie co sekundę. Należy pamiętać, że ten czasomierz jest niezbędne ponieważ `OnTimeTick` wywołania `Invalidate` tylko raz co minutę.

Ten przykład zawiera tylko jeden **xamarin_background.png** obraz; jednak warto utworzyć obraz tła różne dla każdego gęstość ekranu, obsługujących Twoje krój czujki niestandardowych. 

#### <a name="ondraw"></a>OnDraw

Zaktualizowany interfejs **OnDraw** metody rysuje krój czujki analogowy stylu w następujących krokach: 

1.  Pobiera bieżący czas jest teraz obsługiwane w `time` obiektu. 

2.  Określa granice powierzchni do rysowania i jego center.

3.  Rysuje tła skalowane w celu dopasowania urządzenia, gdy tło jest rysowane.

4.  Rysuje dwanaście *Takty* wokół kroju zegara (odpowiadający godzin na powierzchni zegara). 

5.  Oblicza wartość kąta obrotu i długość dla każdej strony czujki.

6.  Rysuje każdej strony na powierzchni czujki. Należy zwrócić uwagę na to, z drugiej strony nie jest rysowana Jeśli wyrażenie kontrolne jest w trybie otoczenia. 

 
#### <a name="onpropertieschanged"></a>OnPropertiesChanged

Ta metoda jest wywoływana w celu poinformowania `MyWatchFaceEngine` o właściwościach zużycia urządzenia (takie jak tryb otoczenia niski bitowe i nagrywania w ochrony). W `MyWatchFaceEngine`, ta metoda sprawdza tylko dla małej bit trybu otoczenia (w trybie otoczenia niski-bitowym, ekranu obsługuje mniejszej liczby bitów dla każdego koloru). 

Aby uzyskać więcej informacji na temat tej metody, zobacz Android [onPropertiesChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onPropertiesChanged%28android.os.Bundle%29) dokumentacji interfejsu API.


#### <a name="onambientmodechanged"></a>OnAmbientModeChanged

Ta metoda jest wywoływana, gdy urządzenie zużycia wprowadza lub opuszcza tryb otoczenia. W `MyWatchFaceEngine` implementacji kroju czujki wyłącza wygładzanie podczas jest w trybie otoczenia. 

Aby uzyskać więcej informacji na temat tej metody, zobacz Android [onAmbientModeChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onAmbientModeChanged%28boolean%29) dokumentacji interfejsu API.


#### <a name="onvisibilitychanged"></a>OnVisibilityChanged

Ta metoda jest wywoływana, gdy czujki staje się widoczny, czy ukryty. W `MyWatchFaceEngine`, ta metoda rejestruje/wyrejestrowuje odbiornika strefy czasowej (opisanych poniżej), zgodnie z stan widoczności. 

Aby uzyskać więcej informacji na temat tej metody, zobacz Android [onVisibilityChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onVisibilityChanged%28boolean%29) dokumentacji interfejsu API.

 
### <a name="time-zone-feature"></a>Funkcja strefy czasowej

Nowy **MyWatchFaceService.cs** obejmuje również funkcje, aby zaktualizować bieżący czas zawsze, gdy czas strefy zmiany (na przykład podczas podróży w różnych strefach czasowych). Pod koniec **MyWatchFaceService.cs**, czasu, zmienić strefę `BroadcastReceiver` zdefiniowano obsługująca zmienić strefę czasową konwersji obiektów: 

```csharp
public class TimeZoneReceiver: BroadcastReceiver
{
    public Action<Intent> Receive { get; set; }
    public override void OnReceive (Context context, Intent intent)
    {
        if (Receive != null)
            Receive (intent);
    }
}
```

`RegisterTimezoneReceiver` i `UnregisterTimezoneReceiver` metody są wywoływane `OnVisibilityChanged` metody. 
`UnregisterTimezoneReceiver` jest wywoływane, gdy stan widoczności powierzchni wyrażenie kontrolne jest zmieniany na ukryte. Gdy kroju czujki jest widoczna, `RegisterTimezoneReceiver` nosi nazwę (zobacz `OnVisibilityChanged` metody). 

Aparat `RegisterTimezoneReceiver` metody deklaruje obsługi dla tego odbiornika strefy czasowej `Receive` zdarzeń; ten program obsługi aktualizacji `time` obiektu o nowej czasu przekroczeniu strefy czasowej: 

```csharp
timeZoneReceiver = new TimeZoneReceiver ();
timeZoneReceiver.Receive = (intent) => {
    time.Clear (intent.GetStringExtra ("time-zone"));
    time.SetToNow ();
};
```

Filtr konwersji jest tworzony i zarejestrowany dla odbiornika strefa czasowa:

```csharp
IntentFilter filter = new IntentFilter(Intent.ActionTimezoneChanged);
Application.Context.RegisterReceiver (timeZoneReceiver, filter);
```

`UnregisterTimezoneReceiver` Metody wyrejestrowuje odbiornika strefa czasowa:

```csharp
Application.Context.UnregisterReceiver (timeZoneReceiver);
```

### <a name="run-the-improved-watch-face"></a>Uruchom kroju ulepszone czujki

Skompiluj i ponownie wdrożyć aplikację do zużycia urządzenia. Wybierz krój czujki z selektora krój czujki jako przed. Podgląd w selektorze wyrażenie kontrolne jest wyświetlany po lewej stronie, a nowy kroju czujki jest wyświetlana po prawej stronie:

[![Obejrzyj analogowy krój](creating-a-watchface-images/13-analog-watchface.png "ulepszone analogowy krój w selektorze i na urządzeniu")](creating-a-watchface-images/13-analog-watchface.png#lightbox)

W tym zrzucie ekranu pokazano sekund jest przenoszona raz na sekundę. Po uruchomieniu tego kodu na urządzeniu zużycia drugiej wskazówki zniknie po czujki przejdzie do trybu otoczenia.

 
## <a name="summary"></a>Podsumowanie

W tym przewodniku niestandardowych watchface nosić systemu Android została zaimplementowana i przetestowane. `CanvasWatchFaceService` i `CanvasWatchFaceService.Engine` klasy zostały wprowadzone i podstawowych metod klasy aparat zostały wprowadzone do utworzenia krój proste wyrażenie kontrolne cyfrowego. Ta implementacja został zaktualizowany o więcej funkcji tworzenia krój analogowy czujki i dodatkowe metody zostały wprowadzone do obsługi zmian w widoczność, tryb otoczenia i różnic we właściwościach urządzenia. Na koniec odbiornik emisji strefy czasowej został wdrożony, aby czujki automatycznie aktualizuje czas, po przekroczeniu strefę czasową. 


## <a name="related-links"></a>Linki pokrewne

- [Tworzenie kroje czujki](https://developer.android.com/training/wearables/watch-faces/index.html)
- [Przykładowe WatchFace](https://developer.xamarin.com/samples/monodroid/wear/WatchFace)
- [WatchFaceService.Engine](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html)
