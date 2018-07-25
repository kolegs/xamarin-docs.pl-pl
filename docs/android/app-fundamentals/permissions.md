---
title: Permissions In Xamarin.Android
ms.prod: xamarin
ms.assetid: 3C440714-43E3-4D31-946F-CA59DAB303E8
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/09/2018
ms.openlocfilehash: 3e12aa47404d8ee4e52ddada3d99f91250e6c54d
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242267"
---
# <a name="permissions-in-xamarinandroid"></a>Permissions In Xamarin.Android


## <a name="overview"></a>Omówienie

Aplikacje dla systemu android są uruchamiane we własnej piaskownicy i zabezpieczeń powodów nie mają dostępu do niektórych zasobów systemowych lub sprzętu na urządzeniu. Użytkownik musi jawnie udzielić uprawnień do aplikacji przed może użyć tych zasobów. Na przykład aplikacja nie może uzyskać dostępu GPS w urządzeniu bez wyraźnej zgody przez użytkownika. System android będzie zgłaszać wyjątek `Java.Lang.SecurityException` Jeśli próbuje uzyskać dostęp do chronionego zasobu bez uprawnienia aplikacji.

Uprawnienia są deklarowane w **AndroidManifest.xml** przez dewelopera aplikacji, gdy aplikacja jest opracowywana. Android ma dwie różne przepływów pracy na potrzeby uzyskania zgody użytkownika na te uprawnienia:
 
* W przypadku aplikacji ukierunkowane na 5.1 dla systemu Android (poziom 22 interfejsu API) lub małych żądań o uprawnienia podczas aplikacja została zainstalowana. Jeśli użytkownik nie udostępni uprawnień, następnie aplikacja nie zostanie zainstalowany. Po zainstalowaniu aplikacji nie istnieje żaden sposób, aby można było odwołać uprawnienia z wyjątkiem przez odinstalowanie aplikacji.
* Począwszy od systemu Android 6.0 (poziom 23 interfejsu API), użytkownicy otrzymanych większą kontrolę nad uprawnienia. można udzielić lub odwołać uprawnienia, tak długo, jak aplikacja jest zainstalowana na urządzeniu. Ten zrzut ekranu przedstawia ustawienia uprawnień dla aplikacji kontakty Google. Jej wymieniono różne uprawnienia i umożliwia użytkownikowi włączanie lub wyłączanie uprawnień:

![Przykładowy ekran uprawnień](permissions-images/01-permissions-check.png) 

Aplikacje dla systemu android musi sprawdzić w czasie wykonywania, aby zobaczyć, jeśli mają uprawnienia dostępu do chronionego zasobu. Jeśli aplikacja nie ma uprawnień, należy go upewnij żądań przy użyciu nowych interfejsów API dostarczonych przez zestawu SDK systemu Android dla użytkownika, aby udzielić uprawnień. Uprawnienia są podzielone na dwie kategorie:

* **Uprawnienia normalny** &ndash; są to uprawnienia, które stanowią niewielkie zagrożenie bezpieczeństwa lub prywatności użytkownika. System android 6.0 automatycznie spowoduje przyznanie normalnymi uprawnieniami w czasie instalacji. Zapoznaj się z dokumentacją systemu Android dla [pełną listę normalnymi uprawnieniami](https://developer.android.com/guide/topics/permissions/normal-permissions.html).
* **Niebezpieczne uprawnienia** &ndash; w przeciwieństwie do normalnymi uprawnieniami niebezpieczne uprawnienia są służących do ochrony bezpieczeństwa i ochrony prywatności firmy użytkownika. Należy jawnie przyznane przez użytkownika. Wysyłać ani odbierać wiadomości SMS znajduje się przykład akcji wymagających niebezpieczne uprawnienia.

> [!IMPORTANT]
> Kategorii, której uprawnienia należy mogą ulec zmianie wraz z upływem czasu.  Istnieje możliwość, że uprawnienie, który został skategoryzowany jako uprawnienie "normal" może być z podwyższonym poziomem uprawnień w przyszłości poziomy interfejsu API do niebezpieczne uprawnienia.

Niebezpieczne uprawnienia podrzędnych podzielono na [ _grup uprawnień_](https://developer.android.com/guide/topics/permissions/requesting.html#perm-groups). Grupy uprawnień będzie przechowywać uprawnienia, które są logicznie powiązane. Gdy użytkownik udziela uprawnień do jednego członka grupy uprawnień systemu Android automatycznie udziela uprawnień do wszystkich członków tej grupy. Na przykład [ `STORAGE` ](https://developer.android.com/reference/android/Manifest.permission_group.html#STORAGE) grupy uprawnień zawiera zarówno `WRITE_EXTERNAL_STORAGE` i `READ_EXTERNAL_STORAGE` uprawnienia. Jeśli użytkownik udziela uprawnień do `READ_EXTERNAL_STORAGE`, a następnie `WRITE_EXTERNAL_STORAGE` uprawnienie jest automatycznie przyznawane w tym samym czasie.

Przed zażądaniem co najmniej jednego uprawnienia, jest najlepszym rozwiązaniem, aby podać uzasadnienie, aby Dlaczego aplikacja wymaga uprawnień przed zażądaniem uprawnienia. Po użytkownik rozumie uzasadnienie, aplikacja może żądać uprawnienia od użytkownika. Dzięki zrozumieniu uzasadnienie, użytkownik może podjąć świadomą decyzję, jeśli chcą przyznanie uprawnień i zrozumieć wpływ, jeśli tak nie jest. 

Całego przepływu pracy weryfikacji oraz żądające uprawnień jest znany jako _uprawnienia środowiska wykonawczego_ wyboru, a można podzielić na poniższym diagramie: 

[![Schemat blokowy Sprawdzanie uprawnień w czasie wykonywania](permissions-images/02-permissions-workflow-sml.png)](permissions-images/02-permissions-workflow.png#lightbox)

Backports biblioteki obsługi systemu Android niektóre z nowych interfejsów API dla: uprawnienia do starszych wersji systemu android. Te interfejsy API backported automatycznie sprawdzi wersji systemu Android na urządzeniu, więc nie można sprawdzić interfejsu API poziomu każdorazowo.  

W tym dokumencie omówiono dodanie uprawnień dla aplikacji platformy Xamarin.Android i w jaki sposób aplikacje, przeznaczonych dla systemu Android 6.0 (poziom 23 interfejsu API) lub nowszym należy wykonać sprawdzanie uprawnień w czasie wykonywania.


> [!NOTE]
> Istnieje możliwość, że uprawnienia dla sprzętu może mieć wpływ na sposób filtrowania aplikacji w sklepie Google Play. Na przykład jeśli aplikacja wymaga uprawnień dla aparatu, następnie sklepu Google Play nie wyświetli aplikacji w Store Play Google na urządzeniu, które nie ma zainstalowanej kamery.


<a name="requirements" />

## <a name="requirements"></a>Wymagania

Zdecydowanie zaleca się, że projekty platformy Xamarin.Android obejmują [Xamarin.Android.Support.Compat](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/) pakietu NuGet. To uprawnienie Poprawka usterki systemu będzie pakietu, który określonych interfejsów API do starszych wersji systemu android, zapewniając jednej wspólnej interfejsu bez konieczności stale sprawdź wersję systemu Android, która aplikacja jest uruchomiona na.


## <a name="requesting-system-permissions"></a>Żądanie uprawnień systemu

Pierwszym krokiem w pracy z uprawnieniami systemu Android jest aby zadeklarować, że plik manifestu uprawnienia w systemie Android. To musi odbywać się niezależnie od tego, poziom interfejsu API, aplikacja jest elementem docelowym.

Aplikacje, których platformą docelową jest system Android 6.0 lub nowszej nie można zakładać, że ponieważ uprawnienie przyznane przez użytkownika w pewnym momencie w przeszłości, że uprawnienie będzie obowiązywać przy następnym. Aplikacja, która jest przeznaczony dla systemu Android 6.0 muszą zawsze być wykonywane sprawdzenie uprawnień środowiska uruchomieniowego. Aplikacji przeznaczonych dla systemów Android, 5.1 lub niższą nie trzeba wykonać sprawdzanie uprawnień w czasie wykonywania.

> [!NOTE]
> Aplikacje tylko powinien zażądać uprawnień, których potrzebują.


### <a name="declaring-permissions-in-the-manifest"></a>Deklarowanie uprawnień w manifeście

Uprawnienia są dodawane do **AndroidManifest.xml** z `uses-permission` elementu. Na przykład w przypadku aplikacji do zlokalizowania pozycji urządzenia, jego wymaga prawidłowo i kurs uprawnienia do lokalizacji. Następujące dwa elementy są dodawane do manifestu: 

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Użytkownik może zadeklarować uprawnień za pomocą narzędzia wbudowaną w programie Visual Studio:

1. Kliknij dwukrotnie **właściwości** w **Eksploratora rozwiązań** i wybierz **manifestu systemu Android** karty w oknie dialogowym właściwości:

    [![Wymagane uprawnienia w karcie Manifest systemu Android](permissions-images/04-required-permissions-vs-sml.png)](permissions-images/04-required-permissions-vs.png#lightbox)

2. Jeśli aplikacja nie ma pliku AndroidManifest.xml, kliknij przycisk **AndroidManifest.xml nie można odnaleźć. Kliknij, aby dodać jeden** jak pokazano poniżej:

    [![Brak komunikatu pliku AndroidManifest.xml](permissions-images/05-no-manifest-vs-sml.png)](permissions-images/05-no-manifest-vs.png#lightbox)

3. Wybierz wszystkie uprawnienia, Twoja aplikacja potrzebuje z **wymagane uprawnienia** listy, a następnie zapisz:

    [![Wybrane uprawnienia aparatu przykład](permissions-images/06-selected-permission-vs-sml.png)](permissions-images/06-selected-permission-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Użytkownik może zadeklarować uprawnień za pomocą narzędzia pomocy technicznej, wbudowane w program Visual Studio dla komputerów Mac:

1. Kliknij dwukrotnie projektu w **konsoli rozwiązania** i wybierz **Opcje > kompilacji > Aplikacja dla systemu Android**:

    [![Wymaganej sekcji uprawnień pokazano](permissions-images/04-required-permissions-xs-sml.png)](permissions-images/04-required-permissions-xs.png#lightbox)

2. Kliknij przycisk **Dodaj manifestu systemu Android** przycisk, jeśli projekt nie ma jeszcze **AndroidManifest.xml**:

    [![Brak manifestu systemu Android projektu](permissions-images/05-no-manifest-xs-sml.png)](permissions-images/05-no-manifest-xs.png#lightbox)

3. Wybierz wszystkie uprawnienia, Twoja aplikacja potrzebuje z **wymagane uprawnienia** listy, a następnie kliknij przycisk **OK**:

    [![Wybrane uprawnienia aparatu przykład](permissions-images/03-select-permission-xs-sml.png)](permissions-images/03-select-permission-xs.png#lightbox)
    
-----

Platforma Xamarin.Android automatycznie doda niektóre uprawnienia w czasie kompilacji do kompilacji do debugowania. Spowoduje to debugowanie aplikacji, łatwiejsze. W szczególności są dwa istotne uprawnienia `INTERNET` i `READ_EXTERNAL_STORAGE`. Te automatycznie Ustaw uprawnienia nie będą widoczne w **wymagane uprawnienia** listy. Wersja kompilacji, jednak, należy użyć tylko uprawnienia, które są jawnie ustawione w **wymagane uprawnienia** listy. 

W przypadku aplikacji przeznaczonych dla 5.1 dla systemu Android (poziom 22 interfejsu API) lub mniej nie ma nic więcej, należy wykonać. Aplikacje działające w systemie Android 6.0 (poziom 23 interfejsu API 23) lub nowszym należy przejść do następnej sekcji na temat wykonywania w czasie wykonywania sprawdza uprawnienia. 


### <a name="runtime-permission-checks-in-android-60"></a>Sprawdza, czy uprawnienie środowiska uruchomieniowego w systemie Android w wersji 6.0

`ContextCompat.CheckSelfPermission` — Metoda (dostępne z biblioteki obsługi systemu Android) jest używane do sprawdzenia, jeśli określone uprawnienie zostało przyznane. Ta metoda zwróci [ `Android.Content.PM.Permission` ](https://developer.xamarin.com/api/type/Android.Content.PM.Permission/) wyliczenia, która ma jedną z dwóch wartości:

* **`Permission.Granted`** &ndash; Określone uprawnienie zostało przyznane.
* **`Permission.Denied`** &ndash; Nie przyznano określone uprawnienie.

Ten fragment kodu jest przykładem sposobu sprawdzania uprawnień aparatu w działaniu: 

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

Jest najlepszym rozwiązaniem, aby informować użytkownika dotyczącą Dlaczego uprawnienie jest niezbędne dla aplikacji, tak aby wprowadzone świadomej decyzji przyznanie uprawnień. Przykładem tego może być aplikacja, która przyjmuje zdjęcia i tagi geograficzne je. Jest wyraźną wskazówką dla użytkownika, że uprawnienia aparat są niezbędne, ale może nie być jasne Dlaczego aplikacja również potrzebuje lokalizacji urządzenia. Uzasadnienie takiego powinien być wyświetlany komunikat, aby pomóc użytkownikowi zrozumieć, dlaczego uprawnienie lokalizacji jest pożądane i że wymagane jest uprawnienie aparatu.

`ActivityCompat.ShouldShowRequestPermissionRational` Metoda jest używana do określenia, jeśli uzasadnienie powinny być wyświetlane użytkownikowi w. Ta metoda zwróci `true` Jeśli uzasadnienie dla danego uprawnienia powinny być wyświetlane. Ten zrzut ekranu przedstawia przykład Snackbar wyświetlany przez aplikację, która wyjaśnia, dlaczego aplikacja musi znać lokalizację urządzenia:

![Racjonalne uzasadnienie dla lokalizacji](permissions-images/07-rationale-snackbar.png) 

Jeśli użytkownik udziela uprawnień, `ActivityCompat.RequestPermissions(Activity activity, string[] permissions, int requestCode)` powinna być wywoływana metoda. Ta metoda wymaga następujących parametrów:

* **działanie** &ndash; to działanie, które żąda uprawnienia i jest informowany przez system Android wyników.
* **uprawnienia** &ndash; listę uprawnień, które są żądane.
* **czy metoda requestCode** &ndash; wartość całkowitą, która jest używana w celu dopasowania do wyników żądania o uprawnienia do `RequestPermissions` wywołania. Ta wartość powinna być większa od zera.

Ten fragment kodu jest przykładem dwie metody, które zostały omówione. Po pierwsze dokonuje ustalenie uzasadnienie uprawnienie powinien być wyświetlany. W przypadku uzasadnienie mają być wyświetlane, Snackbar jest wyświetlany z uzasadnienie. Jeśli użytkownik kliknie **OK** w Snackbar, następnie aplikacja będzie żądać uprawnienia. Jeśli użytkownik nie akceptuje uzasadnienie, aplikacja nie należy kontynuować celu zażądania uprawnień. Jeśli uzasadnienie nie jest wyświetlany, działanie zażąda uprawnień:

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

`RequestPermission` może być wywoływana, nawet jeśli użytkownik ma już uprawnienia. Kolejne wywołania nie są konieczne, ale mogą udostępniać użytkownikom możliwość upewnij się, (lub cofnąć) uprawnienia. Gdy `RequestPermission` jest wywoływana, formant jest przekazywane do systemu operacyjnego, co spowoduje wyświetlenie interfejsu użytkownika do akceptowania uprawnienia:  

![Okno dialogowe Permssion](permissions-images/08-location-permission-dialog.png)

Po zakończeniu pracy użytkownika systemu Android zwróci wyniki do działania w za pośrednictwem metody wywołania zwrotnego, `OnRequestPermissionResult`. Ta metoda jest częścią interfejsu `ActivityCompat.IOnRequestPermissionsResultCallback` musi zostać wdrożone przez działanie. Ten interfejs zawiera jedną metodę, `OnRequestPermissionsResult`, który zostanie wywołany przez systemu Android, aby poinformować działania wyborów użytkownika. Jeśli użytkownik ma uprawnienia, aplikacja może Przejdź dalej i użyj chronionego zasobu. Przykładowy sposób implementacji `OnRequestPermissionResult` znajdują się poniżej: 

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


## <a name="summary"></a>Podsumowanie

W przewodniku omówiono sposób dodawania i sprawdź, czy uprawnień w urządzeniu z systemem Android. Różnice między stare aplikacje dla systemu Android (< 23 poziom interfejsu API) i nowe aplikacje dla systemu Android, jak działają uprawnienia (interfejsu API na poziomie > 22). Jego omówiono sposób wykonywania sprawdza uprawnienia środowiska wykonawczego w systemie Android w wersji 6.0.


## <a name="related-links"></a>Linki pokrewne

- [Lista normalnymi uprawnieniami](https://developer.android.com/guide/topics/permissions/normal-permissions.html)
- [Środowisko uruchomieniowe uprawnienia przykładowej aplikacji](https://github.com/xamarin/monodroid-samples/tree/master/android-m/RuntimePermissions)
- [Obsługa uprawnień w platformy Xamarin.Android](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest)
