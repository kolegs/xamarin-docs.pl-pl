---
title: Witaj, zużycia
description: Tworzenie pierwszej aplikacji platformy systemu Android nosić i uruchom go na urządzeniu lub emulatorze zużycia. Ten przewodnik zawiera instrukcje krok po kroku dotyczące tworzenia małych projekt Android nosić, który obsługuje kliknięcia przycisków i Wyświetla licznik kliknij na urządzeniu zużycia. Wyjaśniono, jak debugować aplikację za pomocą emulatora zużycia lub urządzenia zużycia, który jest połączony za pośrednictwem połączenia Bluetooth na telefonie z systemem Android. Udostępnia również zestaw porad debugowania dla systemu Android zużycia.
ms.prod: xamarin
ms.assetid: 86BCD0E7-E9DC-40F1-9B44-887BC51BB48D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 742a10ce0042d2bbf6d5690cb7a7a6eca529a57e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="hello-wear"></a>Witaj, zużycia

_Tworzenie pierwszej aplikacji platformy systemu Android nosić i uruchom go na urządzeniu lub emulatorze zużycia. Ten przewodnik zawiera instrukcje krok po kroku dotyczące tworzenia małych projekt Android nosić, który obsługuje kliknięcia przycisków i Wyświetla licznik kliknij na urządzeniu zużycia. Wyjaśniono, jak debugować aplikację za pomocą emulatora zużycia lub urządzenia zużycia, który jest połączony za pośrednictwem połączenia Bluetooth na telefonie z systemem Android. Udostępnia również zestaw porad debugowania dla systemu Android zużycia._

![Zrzut ekranu przedstawiający zużycia aplikacji należy wykonać w tym samouczku](hello-wear-images/example.png)

## <a name="your-first-wear-app"></a>Pierwszej aplikacji zużycie

Wykonaj następujące kroki, aby tworzenie pierwszej aplikacji platformy Xamarin.Android zużycia:

### <a name="1-create-a-new-android-project"></a>1. Utwórz nowy projekt dla systemu Android

Utwórz nową **aplikacji systemu Android nosić**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Tworzenie nowej aplikacji nosić systemu Android w oknie dialogowym Nowy projekt](hello-wear-images/vs/new-solution-sml.png)](hello-wear-images/vs/new-solution.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Tworzenie nowej aplikacji nosić systemu Android w oknie dialogowym nowego rozwiązania](hello-wear-images/xs/new-solution-sml.png)](hello-wear-images/xs/new-solution.png#lightbox)

-----


Ten szablon automatycznie obejmuje **Xamarin Android Wearable biblioteki** NuGet (i zależności), będziesz mieć dostęp do zużycia specyficzne elementy widget. Jeśli nie widzisz szablonu zużycia, przejrzyj [instalacji i](~/android/wear/get-started/installation.md) przewodniku, aby dokładnie sprawdzić, czy zainstalowano obsługiwaną zestawu SDK systemu Android. 

### <a name="2-choose-the-correct-target-framework"></a>2. Wybierz poprawny **platformy docelowej**

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Upewnij się, że **Minimum Android do docelowego** ustawiono **Android 5.0 (interfejs typu lizak)** lub nowszy: 

[![Ustawienie platformy docelowej dla systemu Android 5.0 w programie Visual Studio](hello-wear-images/vs/target-framework-sml.png)](hello-wear-images/vs/target-framework.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Upewnij się, platforma docelowa ma ustawioną wartość **Android 5.0 (interfejs typu lizak)** lub nowszy:

[![Ustawienie platformy docelowej dla systemu Android 5.0 w programie Visual Studio dla komputerów Mac](hello-wear-images/xs/target-framework-sml.png)](hello-wear-images/xs/target-framework.png#lightbox)

-----

Aby uzyskać więcej informacji na temat ustawiania platformę docelową, zobacz [poziomy interfejsu API systemu Android opis](~/android/app-fundamentals/android-api-levels.md).


### <a name="3-edit-the-mainaxml-layout"></a>3. Edytuj **Main.axml** układu

Skonfiguruj układ zawiera `TextView` i `Button` przykładowej: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:layout_width="match_parent"
android:layout_height="match_parent">
  <ScrollView
    android:id="@+id/scroll"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:background="#000000"
    android:fillViewport="true">
    <LinearLayout
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:orientation="vertical">
      <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="2dp"
        android:text="Main Activity"
        android:textSize="36sp"
        android:textColor="#006600" />
      <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="2dp"
        android:textColor="#cccccc"
        android:id="@+id/result" />
      <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:onClick="showNotification"
        android:text="Click Me!"
        android:id="@+id/click_button" />
    </LinearLayout>
  </ScrollView>
</FrameLayout>
```

### <a name="4-edit-the-mainactivitycs-source"></a>4. Edytuj **MainActivity.cs** źródła

Dodaj kod, aby zwiększyć licznik i wyświetl ją, gdy przycisk zostanie kliknięty: 

```csharp
[Activity (Label = "WearTest", MainLauncher = true, Icon = "@drawable/icon")]
public class MainActivity : Activity
{
  int count = 1;

  protected override void OnCreate (Bundle bundle)
  {
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);

    Button button = FindViewById<Button> (Resource.Id.click_button);
    TextView text = FindViewById<TextView> (Resource.Id.result);

    button.Click += delegate {
      text.Text = string.Format ("{0} clicks!", count++);
    };
  }
}
```

### <a name="5-setup-an-emulator-or-device"></a>5. Ustawienia emulatora lub urządzenia

Następnym krokiem jest skonfigurowany do wdrożenia i uruchomienia aplikacji emulator lub urządzenie. Jeśli nie są jeszcze zapoznać się z procesem, wdrażanie i uruchamianie aplikacji platformy Xamarin.Android ogólnie rzecz biorąc, zobacz [Hello, Android szybkiego startu](~/android/get-started/hello-android/hello-android-quickstart.md).

Jeśli nie masz urządzenie Android nosić, takich jak Android Smartwatch nosić, aplikację można uruchomić emulatora. Informacje dotyczące debugowania zużycia aplikacji w emulatorze, zobacz [debugowania nosić systemu Android na emulatorze](~/android/wear/deploy-test/debug-on-emulator.md).

Jeśli masz urządzenie Android nosić, takich jak Android Smartwatch nosić, aplikację można uruchomić na urządzeniu, a nie przy użyciu emulatora. Aby uzyskać więcej informacji na temat debugowania na urządzeniu zużycia, zobacz [debugowania na urządzeniu nosić](~/android/wear/deploy-test/debug-on-device.md).


### <a name="6-run-the-android-wear-app"></a>6. Uruchamianie aplikacji systemu Android zużycie

Urządzenia z systemem Android nosić powinna pojawić się w menu rozwijanym urządzenia. Pamiętaj wybrać poprawne urządzenie Android nosić lub AVD, przed rozpoczęciem debugowania. Po wybraniu urządzenia, kliknij przycisk Odtwórz, aby wdrożyć aplikację na emulator lub urządzenie.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Wybranie w menu programu Visual Studio urządzenia AVD nosić](hello-wear-images/vs/choose-wear-sim.png)](hello-wear-images/vs/choose-wear-sim.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Wybieranie AVD nosić w programie Visual Studio dla menu urządzenia Mac](hello-wear-images/xs/choose-wear-sim.png)](hello-wear-images/xs/choose-wear-sim.png#lightbox)

-----

Może zostać wyświetlony **tylko kilka minut...**  komunikatu (lub międzysegmentowe ekranu) na początku: 

![Obejrzyj emulatora są wyświetlane tylko kilka minut...](hello-wear-images/please-wait.png)

Jeśli używasz emulatora czujki, może upłynąć trochę czasu, aby uruchomić aplikację. Korzystając z funkcji Bluetooth, zajmuje więcej czasu niż USB, wdrażania aplikacji. (Na przykład trwa około 5 minut do wdrożenia tej aplikacji na czujki G LG, która jest połączona Bluetooth na telefon 5 węzła.)

Po pomyślnym wdraża aplikację, ekranu urządzenia zużycia powinien być wyświetlany ekran podobne do poniższych:

[![Ekran początkowy aplikacji zużycie](hello-wear-images/mainactivity-screen.png)](hello-wear-images/mainactivity-screen.png#lightbox)

Wybierz **kliknij mnie!** przycisk rachunku zużycia urządzenia i zobacz, zwiększenie liczby z każdego Wybieranie:

[![Zrzut ekranu nosić aplikacji po kliknięć 3](hello-wear-images/mainactivity-counts.png)](hello-wear-images/mainactivity-counts.png#lightbox)


## <a name="next-steps"></a>Następne kroki

Zapoznaj się z [nosić przykłady](https://developer.xamarin.com/samples/android/Android%20Wear/) Android nosić aplikacji przy użyciu Pomocnika Phone aplikacji.

Gdy wszystko będzie gotowe do dystrybucji aplikacji, zobacz [Praca z opakowania](~/android/wear/deploy-test/packaging.md).


## <a name="related-links"></a>Linki pokrewne

- [Kliknij przycisk mnie aplikacji (przykład)](https://developer.xamarin.com/samples/monodroid/wear/WearTest/)
