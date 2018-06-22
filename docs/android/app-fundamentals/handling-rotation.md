---
title: Obsługa obrotu
description: W tym temacie opisano, jak obsługiwać zmiany orientacji urządzenia platformie Xamarin.Android. Uwzględniono również sposób pracy z systemem Android zasobów można automatycznie załadować zasobów w orientacji określonego urządzenia, jak programowo obsługi orientacji zmiany.
ms.prod: xamarin
ms.assetid: 6D33ADF7-ED81-0256-479D-D9E3787A76B0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 1cdc7928c45b99cdd8c8149b3ae9b06e790deeca
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30768899"
---
# <a name="handling-rotation"></a>Obsługa obrotu

_W tym temacie opisano, jak obsługiwać zmiany orientacji urządzenia platformie Xamarin.Android. Uwzględniono również sposób pracy z systemem Android zasobów można automatycznie załadować zasobów w orientacji określonego urządzenia, jak programowo obsługi orientacji zmiany._


## <a name="overview"></a>Omówienie

Ponieważ urządzenia przenośne są łatwo obracane, wbudowane obrotu jest standardowa funkcja przenośnych systemów operacyjnych. Android udostępnia zaawansowane framework zajmujących się obracania w aplikacji, czy interfejs użytkownika jest tworzona deklaratywnie w formacie XML lub programowo w kodzie. Podczas obsługi automatycznie zmiany układu deklaratywne obrócony urządzenia, aplikacji mogą korzystać z ścisłej integracji z systemem Android zasobów. Programowe układu zmiany należy ręcznie obsługiwane. Umożliwia to bardziej precyzyjną kontrolę w czasie wykonywania, ale kosztem więcej pracy dewelopera. Aplikację można również zrezygnować z działania ponownego uruchomienia komputera oraz przejęcie kontroli ręcznej zmiany orientacji.

W tym przewodniku sprawdza następujące tematy orientacji:

-   **Deklaracyjne obrotu układu** &ndash; jak używać systemu Android zasobów do tworzenia aplikacji obsługujących orientacji, łącznie ze sposobem obciążenia zarówno układy i drawables dla konkretnego orientacji.

-   **Programowe obrotu układu** &ndash; jak programowo Dodaj formanty, a także sposób obsługi zmiany orientacji ręcznie.


## <a name="handling-rotation-declaratively-with-layouts"></a>Obsługa obrotu deklaratywnie układy

W tym plików w folderach, które należy wykonać konwencji nazewnictwa, Android automatycznie ładuje odpowiednie pliki zmiany orientacji.
Obejmuje to obsługę:

-   *Zasoby układu* &ndash; określenie, które pliki układu są zwiększony dla każdego orientacji.

-   *Zasoby obiektów drawable* &ndash; określenie, które drawables są ładowane do każdej orientacji.


### <a name="layout-resources"></a>Układ zasobów

Domyślnie pliki XML dla systemu Android (AXML) zawarte w **zasobów/układ** folderu są używane do renderowania widoków dla działania. Ten folder zasobów są używane dla orientacji pionowej, zarówno i poziomej, jeśli żadne zasoby dodatkowe układu są udostępniane specjalnie z myślą o orientacji poziomej. Należy wziąć pod uwagę struktury projektu utworzonych przez domyślny szablon projektu:

[![Domyślnej struktury szablonu projektu](handling-rotation-images/00.png)](handling-rotation-images/00.png#lightbox)

Ten projekt tworzy pojedynczy **Main.axml** w pliku **zasobów/układ** folderu. Podczas działania `OnCreate` metoda jest wywoływana, nadyma on zdefiniowany w widoku **Main.axml,** która deklaruje przycisku, jak pokazano w poniższym XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:orientation="vertical"
  android:layout_width="fill_parent"
  android:layout_height="fill_parent">
<Button  
  android:id="@+id/myButton"
  android:layout_width="fill_parent" 
  android:layout_height="wrap_content" 
  android:text="@string/hello"/>
</LinearLayout>
```

Jeśli urządzenie jest obracana w orientacji poziomej, działanie w `OnCreate` metoda jest wywoływana ponownie i tym samym **Main.axml** zwiększony pliku, jak pokazano na poniższym zrzucie ekranu:

[![Sam, ale w ekranie orientacji poziomej](handling-rotation-images/01-sml.png)](handling-rotation-images/01.png#lightbox)


#### <a name="orientation-specific-layouts"></a>Układy specyficzne dla orientacji

Oprócz folderu układu (co domyślnie pionowa i może również być jawnie nazwane *układu portu* przez dołączenie do folderu o nazwie `layout-land`), aplikację można zdefiniować widoki musi się on w orientacji poziomej bez żadnego kodu zmiany.

Załóżmy, że **Main.axml** plik zawiera następujący kod XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:layout_width="fill_parent"
  android:layout_height="fill_parent">
  <TextView
    android:text="This is portrait"
    android:layout_height="wrap_content"
    android:layout_width="fill_parent" />
</RelativeLayout>
```

Jeśli folder o nazwie ziemi układu, który zawiera dodatkowe **Main.axml** plik zostanie dodany do projektu, pompowania układu w orientacji poziomej teraz spowoduje Android ładowania nowo dodanego **Main.axml.** Należy wziąć pod uwagę wersja pozioma **Main.axml** plik zawierający następujący kod (dla uproszczenia plik XML jest podobny do domyślnej wersji pionowa kodu, ale korzysta z innego ciągu w `TextView`):

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:layout_width="fill_parent"
  android:layout_height="fill_parent">
  <TextView
    android:text="This is landscape"
    android:layout_height="wrap_content"
    android:layout_width="fill_parent" />
</RelativeLayout>
```

Uruchomienie tego kodu i obracanie urządzeń z pionowej na poziomą przedstawiono nowe ładowanie XML, jak pokazano poniżej:

[![Zrzuty ekranu pionowej i poziomej drukowania w trybie portret](handling-rotation-images/02.png)](handling-rotation-images/02.png#lightbox)


### <a name="drawable-resources"></a>Obiektów drawable zasobów

Podczas obracanie Android traktuje zasobów obiektów drawable podobnie do układu zasobów. W takim przypadku system pobiera drawables z **obiektów drawable/zasoby** i **zasobów/obiektów drawable ziemi** folderów, odpowiednio.

Na przykład projekt zawiera obraz o nazwie Monkey.png w **obiektów drawable/zasoby** folderu, w którym obiektów drawable jest wywoływany przez `ImageView` w kodzie XML w następujący sposób:

```xml
<ImageView
  android:layout_height="wrap_content"
  android:layout_width="wrap_content"
  android:src="@drawable/monkey"
  android:layout_centerVertical="true"
  android:layout_centerHorizontal="true" />
```

Umożliwia dalsze przyjęto założenie, że inna wersja **Monkey.png** znajduje się w obszarze **zasobów/obiektów drawable ziemi**. Tylko jak z plikami układu, gdy urządzenie jest obrócony zmian obiektów drawable dla danego orientacji, jak pokazano poniżej:

[![Inna wersja Monkey.png wyświetlany w trybie portret i orientacji poziomej](handling-rotation-images/03.png)](handling-rotation-images/03.png#lightbox)


## <a name="handling-rotation-programmatically"></a>Obsługa obrotu programowo

Czasami definiujemy układów w kodzie. Może to nastąpić z różnych powodów, takich jak ograniczeń technicznych, preferencji Deweloper itd. Gdy firma Microsoft Dodaj formanty programowo aplikacji musi ręcznie konta dla orientacji urządzenia, które odbywa się automatycznie po używamy zasobów XML.


### <a name="adding-controls-in-code"></a>Dodawanie formantów w kodzie

Do dodawania formantów programowo, aplikacja musi wykonać następujące czynności:

-  Utwórz układ.
-  Ustaw układ parametrów.
-  Tworzenie formantów.
-  Ustaw parametry układ formantu.
-  Dodawanie formantów do układu.
-  Ustaw układ jako widok zawartości.

Rozważmy na przykład interfejs użytkownika składającą się z pojedynczej `TextView` dodane do kontroli `RelativeLayout`, jak pokazano w poniższym kodzie.

```csharp
protected override void OnCreate (Bundle bundle)
{
  base.OnCreate (bundle);
                        
  // create a layout
  var rl = new RelativeLayout (this);

  // set layout parameters
  var layoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.FillParent);
  rl.LayoutParameters = layoutParams;
        
  // create TextView control
  var tv = new TextView (this);

  // set TextView's LayoutParameters
  tv.LayoutParameters = layoutParams;
  tv.Text = "Programmatic layout";

  // add TextView to the layout
  rl.AddView (tv);
        
  // set the layout as the content view
  SetContentView (rl);
}
```

Ten kod tworzy wystąpienie `RelativeLayout` klasy i ustawia jej `LayoutParameters` właściwości. `LayoutParams` Klasa jest sposób firmy Android zawierający położenie formantów w sposób wielokrotnego użytku. Po utworzeniu wystąpienia układu kontrolek można tworzyć i dodać do niego. Formanty również mieć `LayoutParameters`, takich jak `TextView` w tym przykładzie. Po `TextView` utworzeniu dodanie go do `RelativeLayout` i ustawienie `RelativeLayout` jako widoku zawartości powoduje wyświetlanie aplikacji `TextView` pokazany:

[![Przycisk licznika przyrostu wyświetlany w trybach pionowa i pozioma](handling-rotation-images/04.png)](handling-rotation-images/04.png#lightbox)


### <a name="detecting-orientation-in-code"></a>Wykrywanie orientacji w kodzie

Jeśli aplikacja próbuje załadować interfejsu innego użytkownika, dla każdego orientacji podczas `OnCreate` nosi nazwę (nastąpi to każdorazowo, urządzenie jest obracana), musi wykryć orientację i załaduje kod interfejsu żądanego użytkownika. Android zawiera klasy o nazwie `WindowManager`, które mogą służyć do określenia bieżącego obracanie urządzeń za pośrednictwem `WindowManager.DefaultDisplay.Rotation` właściwości, jak pokazano poniżej:

```csharp
protected override void OnCreate (Bundle bundle)
{
  base.OnCreate (bundle);
                        
  // create a layout
  var rl = new RelativeLayout (this);

  // set layout parameters
  var layoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.FillParent);
  rl.LayoutParameters = layoutParams;
                        
  // get the initial orientation
  var surfaceOrientation = WindowManager.DefaultDisplay.Rotation;
  // create layout based upon orientation
  RelativeLayout.LayoutParams tvLayoutParams;
                
  if (surfaceOrientation == SurfaceOrientation.Rotation0 || surfaceOrientation == SurfaceOrientation.Rotation180) {
    tvLayoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
  } else {
    tvLayoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
    tvLayoutParams.LeftMargin = 100;
    tvLayoutParams.TopMargin = 100;
  }
                        
  // create TextView control
  var tv = new TextView (this);
  tv.LayoutParameters = tvLayoutParams;
  tv.Text = "Programmatic layout";
        
  // add TextView to the layout
  rl.AddView (tv);
        
  // set the layout as the content view
  SetContentView (rl);
}
```

Ten kod ustawia `TextView` do rozmieszczanych 100 pikseli od góry po lewej części ekranu automatycznie animacji na nowy układ podczas obracania w poziomie, jak pokazano poniżej:

[![Stan widoku jest zachowywany w trybie portret i orientacji poziomej](handling-rotation-images/05.png)](handling-rotation-images/05.png#lightbox)


### <a name="preventing-activity-restart"></a>Uniemożliwia ponowne uruchomienie działania

Oprócz obsługi wszystkich elementów w `OnCreate`, aplikację można również uniemożliwić działanie uruchamiany ponownie po zmianie orientację przez ustawienie `ConfigurationChanges` w `ActivityAttribute` w następujący sposób:

```csharp
[Activity (Label = "CodeLayoutActivity", ConfigurationChanges=Android.Content.PM.ConfigChanges.Orientation | Android.Content.PM.ConfigChanges.ScreenSize)]
```
Teraz podczas obracania urządzenia działania nie zostanie ponownie uruchomiony. Aby ręcznie w tym przypadku Obsługa zmiany orientacji, można zastąpić działanie `OnConfigurationChanged` — metoda i orientację z `Configuration` obiekt, który jest przekazywany w, tak jak nowej implementacji działania poniżej:

```csharp
[Activity (Label = "CodeLayoutActivity", ConfigurationChanges=Android.Content.PM.ConfigChanges.Orientation | Android.Content.PM.ConfigChanges.ScreenSize)]
public class CodeLayoutActivity : Activity
{
  TextView _tv;
  RelativeLayout.LayoutParams _layoutParamsPortrait;
  RelativeLayout.LayoutParams _layoutParamsLandscape;
                
  protected override void OnCreate (Bundle bundle)
  {
    // create a layout
    // set layout parameters
    // get the initial orientation

    // create portrait and landscape layout for the TextView
    _layoutParamsPortrait = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
                
    _layoutParamsLandscape = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
    _layoutParamsLandscape.LeftMargin = 100;
    _layoutParamsLandscape.TopMargin = 100;
                        
    _tv = new TextView (this);
                        
    if (surfaceOrientation == SurfaceOrientation.Rotation0 || surfaceOrientation == SurfaceOrientation.Rotation180) {
      _tv.LayoutParameters = _layoutParamsPortrait;
    } else {
      _tv.LayoutParameters = _layoutParamsLandscape;
    }
                        
    _tv.Text = "Programmatic layout";
    rl.AddView (_tv);
    SetContentView (rl);
  }
                
  public override void OnConfigurationChanged (Android.Content.Res.Configuration newConfig)
  {
    base.OnConfigurationChanged (newConfig);
                        
    if (newConfig.Orientation == Android.Content.Res.Orientation.Portrait) {
      _tv.LayoutParameters = _layoutParamsPortrait;
      _tv.Text = "Changed to portrait";
    } else if (newConfig.Orientation == Android.Content.Res.Orientation.Landscape) {
      _tv.LayoutParameters = _layoutParamsLandscape;
      _tv.Text = "Changed to landscape";
    }
  }
}
```

W tym miejscu `TextView's` układu parametry są inicjowane dla orientacji poziomej i pionowej. Zmienne klasy przechowywania parametrów, wraz z `TextView` siebie, ponieważ działanie nie zostanie ponownie utworzona po zmianie orientacji. Nadal używa kod `surfaceOrientartion` w `OnCreate` można ustawić początkowej układ `TextView`. Po wykonaniu tej `OnConfigurationChanged` obsługuje wszystkie zmiany układu kolejne.

Uruchomienie aplikacji, Android ładuje zmiany interfejsu użytkownika występuje obracanie urządzeń i ponownego uruchamiania działania.


## <a name="preventing-activity-restart-for-declarative-layouts"></a>Uniemożliwia ponowne uruchomienie działania dla układów deklaratywne

Ponowne uruchomienie działania spowodowane obracanie urządzeń również można zapobiec, jeśli definiujemy układ w kodzie XML. Na przykład możemy użyć tej metody, jeśli chcemy uniknąć ponownego uruchomienia działania (ze względu na wydajność, być może) i nie należy załadować nowych zasobów dla różnych orientacji.

Aby to zrobić, możemy postępuj zgodnie z tym samym korzystające z układem programowe. Wystarczy ustawić `ConfigurationChanges` w `ActivityAttribute`, jak robiliśmy `CodeLayoutActivity` wcześniej. Wszelki kod, który trzeba uruchomić ponownie można zaimplementować zmiany orientacji w `OnConfigurationChanged` metody.


## <a name="maintaining-state-during-orientation-changes"></a>Zachowanie stanu podczas zmiany orientacji

Czy obsługa obrotu deklaratywnie lub programowo, wszystkie aplikacje systemu Android, należy zaimplementować te same techniki zarządzania stanu, gdy zmienia się orientacja urządzenia. Stan zarządzania jest ważne, ponieważ ponownym uruchomieniu systemu uruchomionego działania podczas obracania urządzenia z systemem Android. Android robi to ułatwia załadować alternatywne zasoby, takie jak układy i drawables opracowanych specjalnie z myślą o konkretnym orientacji. Po jej ponownym uruchomieniu działania traci wszelkie stan przejściowy, może mieć przechowywane w zmiennych lokalnych klasy. W związku z tym działanie w przypadku stanu zależne, należy zachować stanu na poziomie aplikacji. Aplikacja musi obsługiwać zapisywanie i przywracanie tutaj stan aplikacji, który chce zachować zachowuje zmiany orientacji.

Aby uzyskać więcej informacji na utrwalanie stanu w systemie Android, zapoznaj się [cyklu życia działania](~/android/app-fundamentals/activity-lifecycle/index.md) przewodnik.


## <a name="summary"></a>Podsumowanie

W tym artykule opisano sposobu korzystania z wbudowanych możliwości dla systemu Android do pracy z obrotu. Najpierw go wyjaśniono sposób użycia systemu Android zasobu do utworzenia orientacji aplikacjom obsługującym. Następnie przedstawiony sposób dodawania kontrolek w kodzie, jak również sposób obsługi zmiany orientacji ręcznie.



## <a name="related-links"></a>Linki pokrewne

- [Wersja demonstracyjna obrotu (przykład)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/RotationDemo/)
- [Cykl życia aktywności](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Obsługa zmiany środowiska uruchomieniowego](http://developer.android.com/guide/topics/resources/runtime-changes.html)
- [Zmiana orientacji ekranu Fast](http://android-developers.blogspot.com/2009/02/faster-screen-orientation-change.html)
