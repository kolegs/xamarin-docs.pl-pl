---
title: Ekran powitalny
description: "Aplikację systemu Android dopiero po pewnym czasie, aby mogła się uruchomić, szczególnie gdy aplikacja jest uruchamiana pierwszy raz na urządzeniu. Ekran powitalny może być wyświetlany start postępu użytkownika lub wskazywać znakowania w górę."
ms.topic: article
ms.prod: xamarin
ms.assetid: 26480465-CE19-71CD-FC7D-69D0990D05DE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 9acb1ad6ab1425edb98b938e8c03edc3704f50ae
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="splash-screen"></a>Ekran powitalny

_Aplikację systemu Android dopiero po pewnym czasie, aby mogła się uruchomić, szczególnie gdy aplikacja jest uruchamiana pierwszy raz na urządzeniu. Ekran powitalny może być wyświetlany start postępu użytkownika lub wskazywać znakowania w górę._

<a name="overview" />

## <a name="overview"></a>Omówienie

Android — aplikacja wymaga pewnego czasu w celu uruchomienia, szczególnie podczas pierwszego uruchomienia aplikacji na urządzeniu (czasami jest to określane jako _zimny start_). Może być wyświetlany ekran powitalny rozpocząć postępu dla użytkownika lub mogą być wyświetlane informacje o znakowaniu do identyfikowania i wspieranie aplikacji.

W tym przewodniku opisano jeden technikę do zaimplementowania ekranu powitalnego w aplikacji systemu Android. Obejmuje następujące kroki:

1.  Tworzenie obiektów drawable zasobów dla ekranu powitalnego.

2.  Definiowanie nowy motyw wyświetlanych obiektów drawable zasobów.

3.  Dodawanie nowego działania do aplikacji, która będzie służyć jako ekran powitalny zdefiniowane przez motywu utworzony w poprzednim kroku.

[![Przykład Xamarin logo ekranu powitalnego następuje ekranu aplikacji](splash-screen-images/splashscreen-01-sml.png)](splash-screen-images/splashscreen-01.png)


<a name="requirements" />

## <a name="requirements"></a>Wymagania

W tym przewodniku założono, że aplikacja jest przeznaczony dla poziom interfejsu API systemu Android (Android 4.0.3) 15 lub nowszej. Aplikacja musi mieć również **Xamarin.Android.Support.v4** i **Xamarin.Android.Support.v7.AppCompat** pakiety NuGet dodane do projektu.

Wszystkie kodu i XML w tym przewodniku można znaleźć w [ekranu powitalnego](https://developer.xamarin.com/samples/monodroid/SplashScreen) przykładowy projekt dotyczący tego przewodnika.

<a name="implement" />

## <a name="implementing-a-splash-screen"></a>Implementowanie ekranu powitalnego

Najszybszy sposób renderowania i wyświetlania ekranu powitalnego jest utworzyć niestandardowy motyw i zastosować je do działania, która wykazuje ekranu powitalnego. Podczas renderowania działania ładuje motyw i stosuje zasobu obiektów drawable (odwołuje się motywu) do działania w tle. Takie podejście pozwala uniknąć konieczności tworzenia pliku układu.

Ekran powitalny jest zaimplementowany jako działanie, które wyświetla marką obiektów drawable, wykonuje żadnych operacji inicjowania i uruchamiania żadnych zadań. Gdy aplikacja ma załadować, ekran powitalny działanie uruchamia działanie główne i usuwa sam stosie przechodzenia wstecz aplikacji.

<a name="drawable" />

### <a name="creating-a-drawable-for-the-splash-screen"></a>Tworzenie obiektów Drawable dla ekranu powitalnego

Ekran powitalny wyświetli obiektów drawable tła ekranu powitalnego działania XML. Należy użyć obrazu mapy bitowej (na przykład plik PNG lub JPG) dla obrazu do wyświetlenia.

W tym przewodniku używamy [listy warstw](http://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList) do środka obraz ekranu powitalnego aplikacji. Poniższy fragment jest przykładem `drawable` zasobów przy użyciu `layer-list`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
  <item>
    <color android:color="@color/splash_background"/>
  </item>
  <item>
    <bitmap
        android:src="@drawable/splash"
        android:tileMode="disabled"
        android:gravity="center"/>
  </item>
</layer-list>
```

To `layer-list` będzie Centrum obraz ekranu powitalnego **splash.png** na tle określony przez `@color/splash_background` zasobów.
Umieść ten plik w **obiektów drawable/zasoby** folder (na przykład **Resources/drawable/splash_screen.xml**).

Po utworzeniu obiektów drawable ekran powitalny, następnym krokiem jest utworzyć motyw dla ekranu powitalnego.

<a name="theme" />

### <a name="implementing-a-theme"></a>Implementowanie motywu

Aby utworzyć niestandardowy motyw dla ekranu powitalnego działania, Edytuj (lub dodać) pliku **values/styles.xml** i utworzyć nową `style` element na ekranie powitalnym. Przykładowe **values/style.xml** plików są wyświetlane poniżej z `style` o nazwie **MyTheme.Splash**:

```xml
<resources>
  <style name="MyTheme.Base" parent="Theme.AppCompat.Light">
  </style>

  <style name="MyTheme" parent="MyTheme.Base">
  </style>

  <style name="MyTheme.Splash" parent ="Theme.AppCompat.Light.NoActionBar">
    <item name="android:windowBackground">@drawable/splash_screen</item>
    <item name="android:windowNoTitle">true</item>
    <item name="android:windowFullscreen">true</item>
  </style>
</resources>
```

**MyTheme.Splash** jest bardzo spartan &ndash; go deklaruje tło okna, jawnie usuwa pasek tytułu okna i deklaruje, że jest pełnego ekranu. Jeśli chcesz utworzyć ekran powitalny, która emuluje interfejsu użytkownika aplikacji, zanim działania nadyma pierwszego układu, możesz użyć `windowContentOverlay` zamiast `windowBackground` w definicji stylu. W takim przypadku należy również zmodyfikować **splash_screen.xml** obiektów drawable, tak aby wyświetlone emulacji interfejsu użytkownika.

<a name="activity" />

### <a name="create-a-splash-activity"></a>Utwórz działanie powitalny

Teraz potrzebujemy nowe działanie dla systemu Android uruchomić naszych obraz powitalnego i wykonuje wszystkie zadania uruchamiania. Następujący kod jest przykładem implementacji ekranu powitalnego pełną:

```csharp
[Activity(Theme = "@style/MyTheme.Splash", MainLauncher = true, NoHistory = true)]
public class SplashActivity : AppCompatActivity
{
    static readonly string TAG = "X:" + typeof(SplashActivity).Name;

    public override void OnCreate(Bundle savedInstanceState, PersistableBundle persistentState)
    {
        base.OnCreate(savedInstanceState, persistentState);
        Log.Debug(TAG, "SplashActivity.OnCreate");
    }

    // Launches the startup task
    protected override void OnResume()
    {
        base.OnResume();
        Task startupWork = new Task(() => { SimulateStartup(); });
        startupWork.Start();
    }

    // Simulates background work that happens behind the splash screen
    async void SimulateStartup ()
    {
        Log.Debug(TAG, "Performing some startup work that takes a bit of time.");
        await Task.Delay (8000); // Simulate a bit of startup work.
        Log.Debug(TAG, "Startup work is finished - starting MainActivity.");
        StartActivity(new Intent(Application.Context, typeof (MainActivity)));
    }
}
```

`SplashActivity` jawnie używa motywu, który został utworzony w poprzedniej sekcji, Zastępowanie domyślnego motywu aplikacji.
Nie istnieje potrzeba załadować układu w `OnCreate` jako motyw deklaruje obiektów drawable w tle.

Ważne jest, aby ustawić `NoHistory=true` atrybutu, dzięki czemu działanie zostanie usunięty z stosie przechodzenia wstecz. Aby uniemożliwić anulowanie procesu uruchamiania przycisku Wstecz, możesz też przesłonić `OnBackPressed` i nic nie rób:

```csharp
public override void OnBackPressed() { }
```

Praca uruchamiania jest wykonywane asynchronicznie w `OnResume`. Jest to konieczne, aby pracy uruchamiania działa wolno lub nie opóźnienie wyglądu ekranu uruchamiania. Po ukończeniu pracy `SplashActivity` uruchomi `MainActivity` i użytkownik może rozpocząć interakcji z aplikacją.

Nowy `SplashActivity` jest ustawiony jako działania uruchamiania aplikacji przez ustawienie `MainLauncher` atrybutu `true`. Ponieważ `SplashActivity` jest teraz działania uruchamiania, należy edytować `MainActivity.cs`i Usuń `MainLauncher` atrybutu z `MainActivity`:

```csharp
[Activity(Label = "@string/ApplicationName")]
public class MainActivity : AppCompatActivity
{
    // Code omitted for brevity
}
```

<a name="summary" />

## <a name="summary"></a>Podsumowanie

W tym przewodniku opisano jeden sposób implementowania ekranu powitalnego aplikacji platformy Xamarin.Android; to znaczy stosowanie niestandardowy motyw do uruchamiania działania.


## <a name="related-links"></a>Linki pokrewne

- [Ekranu powitalnego (przykład)](https://developer.xamarin.com/samples/monodroid/SplashScreen)
- [Drawable listy warstw](http://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList)
- [ Wzorce projektowe materiału - ekrany uruchamiania](https://www.google.com/design/spec/patterns/launch-screens.html)
