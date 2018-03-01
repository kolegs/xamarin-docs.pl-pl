---
title: Permissions In Xamarin.Android
ms.topic: article
ms.prod: xamarin
ms.assetid: 3C440714-43E3-4D31-946F-CA59DAB303E8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: d20b2aa7df17f2000e2de9cb67f091c52989719b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="permissions-in-xamarinandroid"></a>Permissions In Xamarin.Android

<a name="overview" />

## <a name="overview"></a>Omówienie

Aplikacje systemu android są uruchamiane w ich własnych piaskownicy i zabezpieczeń powodów nie mają dostępu do niektórych zasobów systemowych lub sprzętu na urządzeniu. Użytkownik musi jawnie udzielić uprawnień do aplikacji, zanim może używać tych zasobów. Na przykład aplikacja nie może uzyskać dostępu GPS na urządzeniu bez wyraźnej zgody użytkownika. Android zgłosi `Java.Lang.SecurityException` Jeśli aplikacja próbuje dostępu do chronionego zasobu bez uprawnienia.

Uprawnienia są zadeklarowane w **AndroidManifest.xml** przez dewelopera aplikacji, gdy aplikacja jest opracowana. Android ma dwie różne przepływy pracy dla uzyskania zgody użytkownika na tych uprawnień:
 
* W przypadku aplikacji, które objęte 5.1 systemu Android (interfejs API na poziomie 22) i małe żądania uprawnień wystąpił podczas instalowania aplikacji. Jeśli użytkownik nie udostępni uprawnienia, następnie aplikacja może nie można zainstalować. Po zainstalowaniu aplikacji nie istnieje sposób odwołać uprawnienia, z wyjątkiem przez odinstalowanie aplikacji.
* Począwszy od systemu Android 6.0 (interfejs API na poziomie 23), użytkownicy określono większą kontrolę nad uprawnienia. można udzielić lub odwołać uprawnienia tak długo, jak aplikacja jest instalowana na urządzeniu. Ten zrzut ekranu przedstawia ustawienia uprawnień dla aplikacji Google kontaktów. Zawiera listę różnych uprawnień i zezwala użytkownikowi na włączanie lub wyłączanie uprawnień:

![Przykładowe uprawnienia ekranu](permissions-images/01-permissions-check.png) 

Aplikacje dla systemu android musi sprawdzić w czasie wykonywania, czy mają one uprawnienia do uzyskania dostępu do chronionego zasobu. Jeśli aplikacja nie ma uprawnień, musi ona upewnij żądań przy użyciu nowych interfejsów API dostarczonych przez zestaw SDK systemu Android dla użytkownika, aby udzielić uprawnień. Uprawnienia są podzielone na dwie kategorie:

* **Uprawnienia normalny** &ndash; są to uprawnienia, które stanowią małego zagrożenie dla bezpieczeństwa lub prywatności użytkownika. System android 6.0 automatycznie przyzna normalnymi uprawnieniami w czasie instalacji. Zapoznaj się z dokumentacją systemu Android dla [pełną listę normalnymi uprawnieniami](https://developer.android.com/guide/topics/permissions/normal-permissions.html).
* **Niebezpieczne uprawnienia** &ndash; w przeciwieństwie do normalnymi uprawnieniami niebezpieczne uprawnienia są służących do ochrony bezpieczeństwa i ochrony prywatności użytkownika. Explictly muszą być przyznane przez użytkownika. Wysyłanie i odbieranie wiadomości SMS jest przykładem działania wymagające niebezpieczne uprawnienia.

> [!IMPORTANT]
> Uprawnienie należącego do kategorii może ulec zmianie.  Istnieje możliwość, że uprawnienia, który został skategoryzowany jako uprawnienie "normal" może być w przyszłości poziomy interfejsu API awansowani do poziomu niebezpieczne uprawnienia.

Niebezpieczne uprawnienia podrzędna podzielono na [ _grup uprawnień_](https://developer.android.com/guide/topics/permissions/requesting.html#perm-groups). Grupy uprawnień będą przechowywane uprawnienia logicznie powiązanych. Użytkownik przyznaje uprawnienia do jednego elementu członkowskiego grupy uprawnień, Android automatycznie udziela uprawnień do wszystkich członków tej grupy. Na przykład [ `STORAGE` ](https://developer.android.com/reference/android/Manifest.permission_group.html#STORAGE) grupy uprawnień pełni `WRITE_EXTERNAL_STORAGE` i `READ_EXTERNAL_STORAGE` uprawnienia. Jeśli użytkownik udziela uprawnień do `READ_EXTERNAL_STORAGE`, a następnie `WRITE_EXTERNAL_STORAGE` automatycznie otrzymuje uprawnienia w tym samym czasie.

Przed wysłaniem żądania jedno lub więcej uprawnień, jest najlepszym rozwiązaniem podać uzasadnienie klienckiemu, dlaczego aplikacja wymaga uprawnień przed wysłaniem żądania uprawnień. Gdy użytkownik rozumie uzasadnienie, aplikacji mogą żądać uprawnienia od użytkownika. Zrozumienie uzasadnienie, użytkownik może podjąć świadomej decyzji, jeśli chcesz udzielić uprawnień i zrozumieć skutki, jeśli nie. 

Całego przepływu pracy sprawdzanie i żąda uprawnienia nosi nazwę _uprawnienia wykonawcze_ Sprawdź i można podsumować na poniższym diagramie: 

[ ![Schemat blokowy wyboru uprawnień środowiska wykonawczego](permissions-images/02-permissions-workflow-sml.png)](permissions-images/02-permissions-workflow.png)

Backports biblioteki obsługi systemu Android niektóre z nowych interfejsów API dla: uprawnienia do starszych wersji systemu android. Te backported interfejsów API automatycznie będzie sprawdzać wersją systemu android na urządzeniu, więc nie można sprawdzić interfejsu API poziomu zawsze.  

Ten dokument będzie omawiać temat Dodaj uprawnienia do aplikacji platformy Xamarin.Android i w jaki sposób aplikacje, których miejscem docelowym jest system Android 6.0 (interfejs API na poziomie 23) lub wyższej należy wykonać sprawdzanie uprawnień w czasie wykonywania.


> [!NOTE]
> **Uwaga:** jest możliwe, że uprawnienia dla sprzętu może mieć wpływ na sposób filtrowania aplikacji w sklepie Google Play. Na przykład jeśli aplikacja wymaga uprawnień dla aparatu, następnie Google Play nie będą widoczne aplikacji w sklepie Google Play na urządzeniu nie ma zainstalowanej kamery.


<a name="requirements" />

## <a name="requirements"></a>Wymagania

Zdecydowanie zaleca się, że projekty platformy Xamarin.Android obejmuje [Xamarin.Android.Support.Compat](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/) pakietu NuGet. Ten pakiet zostanie Poprawka usterki systemu uprawnienia, które poszczególnych interfejsów API do starszych wersji systemu android, zapewniając wspólny jeden interfejs bez konieczności stale sprawdź wersję systemu android, że aplikacja jest uruchomiona na.

<a name="requesting_permissions" />

## <a name="requesting-system-permissions"></a>Żądanie uprawnień systemu

Pierwszym krokiem podczas pracy z systemem Android uprawnień jest do deklarowania uprawnień w programie Android pliku manifestu. To musi odbywać się niezależnie od tego, poziom interfejsu API to dotyczących aplikacji.

Aplikacje, które są stosowane do systemu Android 6.0 lub nowszy nie zakłada, że ponieważ uprawnienie przyznane przez użytkownika w pewnym momencie w przeszłości, że uprawnienie będzie obowiązywać przy następnym. Aplikacja, która jest przeznaczony dla systemu Android 6.0 zawsze należy wykonać sprawdzanie uprawnień czasu wykonywania. Aplikacje przeznaczone dla systemu Android 5.1 lub niższy nie trzeba sprawdzić uprawnienia wykonawcze.

> [!NOTE]
> **Uwaga:** aplikacji tylko powinien zażądać uprawnień, których wymagają.

<a name="declaring_permissions_in_the_manifest" />

### <a name="declaring-permissions-in-the-manifest"></a>Deklarowanie uprawnienia w manifeście

Uprawnienia są dodawane do **AndroidManifest.xml** z `uses-permission` elementu. Na przykład w przypadku aplikacji można znaleźć pozycji urządzenia, jego wymaga prawidłowo i kursu uprawnienia lokalizacji. Następujące dwa elementy są dodawane do manifestu: 

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Istnieje możliwość zadeklarować uprawnienia za pomocą narzędzia pomocy technicznej, wbudowane w program Visual Studio:

1. Kliknij dwukrotnie **właściwości** w **Eksploratora rozwiązań** i wybierz **manifestu systemu Android** karty w oknie właściwości:

    [![Wymagane uprawnienia na karcie manifestu systemu Android](permissions-images/04-required-permissions-vs-sml.png)](permissions-images/04-required-permissions-vs.png)

2. Jeśli aplikacja nie ma pliku AndroidManifest.xml, kliknij przycisk **AndroidManifest.xml nie znaleziono. Kliknij, aby dodać jedną** w sposób przedstawiony poniżej:

    [![Żaden komunikat AndroidManifest.xml](permissions-images/05-no-manifest-vs-sml.png)](permissions-images/05-no-manifest-vs.png)

3. Wybierz wszystkie uprawnienia tę aplikację z **wymagane uprawnienia** listy i Zapisz:

    [![Przykład aparatu uprawnienia wybrane](permissions-images/06-selected-permission-vs-sml.png)](permissions-images/06-selected-permission-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Istnieje możliwość zadeklarować uprawnienia za pomocą narzędzia pomocy technicznej, wbudowane w program Visual Studio dla komputerów Mac:

1. Kliknij dwukrotnie plik projektu w **konsoli rozwiązania** i wybierz **Opcje > kompilacji > aplikacji systemu Android**:

    [![Wymaganej sekcji uprawnień pokazano](permissions-images/04-required-permissions-xs-sml.png)](permissions-images/04-required-permissions-xs.png)

2. Kliknij przycisk **dodać manifestu systemu Android** przycisku, jeśli projekt nie ma jeszcze **AndroidManifest.xml**:

    [![Brak projektu manifestu systemu Android](permissions-images/05-no-manifest-xs-sml.png)](permissions-images/05-no-manifest-xs.png)

3. Wybierz wszystkie uprawnienia tę aplikację z **wymagane uprawnienia** listy i kliknij przycisk **OK**:

    [![Przykład aparatu uprawnienia wybrane](permissions-images/03-select-permission-xs-sml.png)](permissions-images/03-select-permission-xs.png)
    
-----

Xamarin.Android automatycznie doda niektóre uprawnienia w czasie kompilacji do kompilacji do debugowania. Spowoduje to debugowanie aplikacji łatwiejsze. W szczególności są dwa uprawnienia zauważalne `INTERNET` i `READ_EXTERNAL_STORAGE`. Te uprawnienia automatycznie zestaw nie będzie widoczna dla można włączyć w **wymagane uprawnienia** listy. Wersja kompilacji, jednak, należy użyć tylko uprawnienia, które są jawnie ustawione w **wymagane uprawnienia** listy. 

Dla aplikacji przeznaczonych dla 5.1 systemu Android (interfejs API na poziomie 22) lub mniej nie ma nic więcej, które należy wykonać. Aplikacje, systemem Android 6.0 (23 interfejsu API na poziomie 23) lub nowszy powinny działać na następną sekcję na temat sposobu wykonywania sprawdza uprawnienia do wykonania. 

<a name="run_time_permission_checks" />

### <a name="runtime-permission-checks-in-android-60"></a>Sprawdza uprawnienia środowiska uruchomieniowego w systemie Android 6.0

`ContextCompat.CheckSelfPermission` — Metoda (dostępne z biblioteki obsługi systemu Android) służy do sprawdzania, czy udzielono odpowiedniego uprawnienia. Ta metoda zwróci [ `Android.Content.PM.Permission` ](https://developer.xamarin.com/api/type/Android.Content.PM.Permission/) wyliczenia, która zawiera jedną z dwóch wartości:

* **`Permission.Granted`** &ndash; Udzielono określonego uprawnienia.
* **`Permission.Denied`** &ndash; Nie udzielono uprawnienia określony.

Następujący fragment kodu jest przykładem sprawdzić uprawnienia do kamery w działaniu: 

```csharp
if (ContextCompat.CheckSelfPermission(this, Manifest.Permission.Camera) == (int)Permission.Granted) 
{
    // We have permission, go ahead and use the camera.
} 
else 
{
    // Camera permission is not granted. If necessary display rationale & request.
}
```

Jest najlepszym rozwiązaniem w celu poinformowania użytkownika o to, dlaczego uprawnienie jest wymagane dla aplikacji, aby można było wprowadzić podjąć świadomą decyzję przyznanie uprawnień. Przykładem tego może być aplikację, która przyjmuje zdjęć i tagi geograficznie je. Jest jasne dla użytkownika, że wymagane jest uprawnienie aparatu, ale może nie być wyczyść Dlaczego aplikacja potrzebuje również lokalizację urządzenia. Decyzja powinien być wyświetlany komunikat pomóc użytkownikowi zrozumieć, dlaczego uprawnienie lokalizacji jest pożądane i że wymagane jest uprawnienie aparatu.

`ActivityCompat.ShouldShowRequestPermissionRational` Metoda jest używana do określenia, czy decyzja powinny być wyświetlane dla użytkownika. Ta metoda zwróci `true` Jeśli uzasadnienie danego uprawnień powinna być wyświetlana. Ten zrzut ekranu przedstawia przykład Snackbar, wyświetlane przez aplikację, która wyjaśnia, dlaczego aplikacja musi znać lokalizację urządzenia:

![Uzasadnienie lokalizacji](permissions-images/07-rationale-snackbar.png) 

Jeśli użytkownik udziela uprawnień, `ActivityCompat.RequestPermissions(Activity activity, string[] permissions, int requestCode)` powinna zostać wywołana metoda. Ta metoda wymaga następujących parametrów:

* **działanie** &ndash; to działanie, która żąda uprawnienia i jest informowany przez system Android wyników.
* **uprawnienia** &ndash; listę uprawnień, które są żądane.
* **czy metoda requestCode** &ndash; służy do dopasowania do wyników żądania uprawnień na wartość całkowitą `RequestPermissions` wywołania. Ta wartość powinna być większa niż zero.

Następujący fragment kodu jest przykład dwóch metod, które zostały omówione. Dokonuje najpierw, aby ustalić uzasadnienie uprawnienia mają być wyświetlane. Jeśli decyzja ma być wyświetlana, Snackbar zostanie wyświetlony z uzasadnienie. Gdy użytkownik kliknie **OK** w Snackbar, następnie aplikacja będzie zażądać uprawnień. Jeśli użytkownik nie akceptuje uzasadnienie, aplikacji nie powinien kontynuować Aby zażądać uprawnień. Jeśli nie ma uzasadnienie, działanie zażąda uprawnienia:

```csharp
if (ActivityCompat.ShouldShowRequestPermissionRationale(this, Manifest.Permission.AccessFineLocation)) 
{
    // Provide an additional rationale to the user if the permission was not granted
    // and the user would benefit from additional context for the use of the permission.
    // For example if the user has previously denied the permission.
    Log.Info(TAG, "Displaying camera permission rationale to provide additional context.");

    var requiredPermissions = new String[] { Manifest.Permission.AccessFineLocation };
    Snackbar.Make(layout, 
                   Resource.String.permission_location_rationale,
                   Snackbar.LengthIndefinite)
            .SetAction(Resource.String.ok, 
                       new Action<View>(delegate(View obj) {
                           ActivityCompat.RequestPermissions(this, requiredPermissions, REQUEST_LOCATION);
                       }    
            )
    ).Show();
}
else 
{
    ActivityCompat.RequestPermissions(this, new String[] { Manifest.Permission.Camera }, REQUEST_LOCATION);
}
```

`RequestPermission` może zostać wywołana, nawet jeśli użytkownik ma już uprawnienia. Kolejne wywołania nie są konieczne, ale ich użytkownika o potwierdzenie (lub cofnąć) uprawnienia. Gdy `RequestPermission` jest wywoływana, kontroli jest przekazany do systemu operacyjnego, co powoduje wyświetlenie interfejsu użytkownika dla akceptując uprawnienia:  

![Okno dialogowe Permssion](permissions-images/08-location-permission-dialog.png)

Po zakończeniu pracy użytkownika dla systemu Android zostanie zwracają wyniki do działania za pomocą metody wywołania zwrotnego, `OnRequestPermissionResult`. Ta metoda jest elementem interfejsu `ActivityCompat.IOnRequestPermissionsResultCallback` musi być implementowana przez działanie. Ten interfejs zawiera tylko jedną metodę `OnRequestPermissionsResult`, który zostanie wywołany przez system Android informują działania opcji użytkownika. Jeśli użytkownik ma uprawnienia, aplikacja może Przejdź dalej i użyj chronionego zasobu. Przykładowy sposób implementowania `OnRequestPermissionResult` przedstawiono poniżej: 

```csharp
public override void OnRequestPermissionsResult(int requestCode, string[] permissions, Permission[] grantResults)
{
    if (requestCode == REQUEST_LOCATION) 
    {
        // Received permission result for camera permission.
        Log.Info(TAG, "Received response for Location permission request.");

        // Check if the only required permission has been granted
        if ((grantResults.Length == 1) && (grantResults[0] == Permission.Granted)) {
            // Location permission has been granted, okay to retrieve the location of the device.
            Log.Info(TAG, "Location permission has now been granted.");
            Snackbar.Make(layout, Resource.String.permision_available_camera, Snackbar.LengthShort).Show();            
        } 
        else 
        {
            Log.Info(TAG, "Location permission was NOT granted.");
            Snackbar.Make(layout, Resource.String.permissions_not_granted, Snackbar.LengthShort).Show();
        }
    } 
    else 
    {
        base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
    }
}
```  

<a name="summary" />

## <a name="summary"></a>Podsumowanie

W tym przewodniku opisano sposób dodawania i sprawdź, czy uprawnienia w urządzeniu z systemem Android. Różnice w sposób działania uprawnień między starym aplikacji systemu Android (interfejs API na poziomie < 23) i nowych aplikacji dla systemu Android (interfejs API na poziomie > 22). Omówione go jak do sprawdzania uprawnień w czasie wykonywania w systemie Android w wersji 6.0.


## <a name="related-links"></a>Linki pokrewne

- [Lista normalnymi uprawnieniami](https://developer.android.com/guide/topics/permissions/normal-permissions.html)
- [Środowisko uruchomieniowe uprawnienia przykładowej aplikacji](https://github.com/xamarin/monodroid-samples/tree/master/android-m/RuntimePermissions)
- [Obsługa uprawnień w platformy Xamarin.Android](https://developer.xamarin.com/recipes/android/general/projects/add_permissions_to_android_manifest)
