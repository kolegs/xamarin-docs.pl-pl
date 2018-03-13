---
title: Wprowadzenie do korzystania z uwierzytelniania linii papilarnych
ms.topic: article
ms.prod: xamarin
ms.assetid: 7BACCB36-8E3E-4E5D-B8EF-56A639839FD2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: e3c986b03408dae98a5a79f257029c10909aeabd
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="getting-started-with-fingerprint-authentication"></a>Wprowadzenie do korzystania z uwierzytelniania linii papilarnych

Aby rozpocząć, umożliwia najpierw opisano, jak skonfigurować projekt platformy Xamarin.Android tak, aby aplikacja jest w stanie odcisku palca uwierzytelniania:

1. Aktualizacja **AndroidManifest.xml** Aby zadeklarować uprawnienia, które wymagają interfejsów API linii papilarnych.
2. Uzyskaj odwołanie do `FingerprintManager`.
3. Sprawdź, czy urządzenie jest w stanie skanowania przy użyciu linii papilarnych.

## <a name="requesting-permissions-in-the-application-manifest"></a>Żądania uprawnień w aplikacji manifestu

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Należy zażądać aplikacji systemu Android `USE_FINGERPRINT` uprawnień w manifeście. Poniższy zrzut ekranu przedstawia sposób dodawania to uprawnienie do aplikacji w programie Visual Studio 2015:

[![Włączanie użycia\_odcisku PALCA na ekranie manifestu systemu Android](get-started-images/fingerprint-01-vs.png)](get-started-images/fingerprint-01-vs.png#lightbox) 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Należy zażądać aplikacji systemu Android `USE_FINGERPRINT` uprawnień w manifeście. Poniższy zrzut ekranu przedstawia sposób dodawania to uprawnienie do aplikacji w programie Visual Studio dla komputerów Mac:

[![Włączanie UseFingerprint na ekranie aplikacji systemu Android](get-started-images/fingerprint-01-xs.png)](get-started-images/fingerprint-01-xs.png#lightbox) 

-----

## <a name="getting-an-instance-of-the-fingerprintmanager"></a>Pierwsze wystąpienie FingerprintManager

Następnie aplikacja musi pobrać wystąpienia `FingerprintManager` lub `FingerprintManagerCompat` klasy. Aby zachować zgodność ze starszymi wersjami systemu android, aplikacji systemu Android powinny używać zgodności interfejsu API znaleziono w pakiecie NuGet w wersji 4 Obsługa systemu Android. Poniższy fragment kodu przedstawia sposób uzyskać odpowiedni obiekt z systemu operacyjnego: 

```csharp
// Using the Android Support Library v4
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);

// Using API level 23:
FingerprintManager fingerprintManager = context.GetSystemService(Context.FingerprintService) as FingerprintManager;
```  

W poprzednich fragment `context` jest żadnych Android `Android.Content.Context`. Zazwyczaj jest to działanie, które wykonuje uwierzytelnianie.

## <a name="checking-for-eligibility"></a>Sprawdzanie kwalifikujących się usług

Aplikacji należy wykonać kilka sprawdza, czy jest możliwe użycie odcisku palca uwierzytelniania. Całkowita liczba istnieją pięć warunków używanych przez aplikację do sprawdzenia kwalifikowania:  
 

**Interfejs API na poziomie 23** &ndash; API odcisk palca wymagany poziom interfejsu API 23 lub nowszej. `FingerprintManagerCompat` Klasa będzie zawijany wyboru poziom interfejsu API dla Ciebie. Z tego powodu jest zalecane, aby użyć **biblioteki obsługi systemu Android w wersji 4** i `FingerprintManagerCompat`; zostanie to konto do jednej z wymienionych testów.

**Sprzęt** &ndash; podczas uruchamiania aplikacji po raz pierwszy, należy sprawdzić obecność linii papilarnych:

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.IsHardwareDetected)
{
    // Code omitted
}
```
    
**Urządzenie jest zabezpieczone** &ndash; użytkownik musi mieć zabezpieczone przy użyciu blokady ekranu urządzenia. Jeśli użytkownik nie ma zabezpieczone urządzenia blokada ekranu i zabezpieczenia są istotne dla aplikacji, użytkownik powinien otrzymać powiadomienie blokada ekranu musi być skonfigurowany. Poniższy fragment kodu przedstawia sposób Sprawdź tej wersji pre-requiste:

```csharp
KeyguardManager keyguardManager = (KeyguardManager) GetSystemService(KeyguardService);
if (!keyguardManager.IsKeyguardSecure)
{
}
```

**Zarejestrowane odciski palców** &ndash; użytkownik musi mieć co najmniej jeden odcisku palca w zarejestrowany w systemie operacyjnym. Sprawdzanie uprawnień powinna występować przed każdą próbę uwierzytelnienia:

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.HasEnrolledFingerprints)
{
    // Can't use fingerprint authentication - notify the user that they need to
    // enroll at least one fingerprint with the device.
}
```

**Uprawnienia** &ndash; aplikacja musi żądać uprawnienia od użytkownika przed rozpoczęciem korzystania z aplikacji. Dla systemu Android 5.0 i dolną użytkownika przyznaje uprawnienia jako warunek instalowania aplikacji. System android 6.0 wprowadzono nowy model uprawnień, który sprawdza uprawnienia w czasie wykonywania. Następujący fragment kodu jest przykładem sprawdzić uprawnienia Android 6.0:

```csharp
// The context is typically a reference to the current activity.
Android.Content.PM.Permission permissionResult = ContextCompat.CheckSelfPermission(context, Manifest.Permission.UseFingerprint);
if (permissionResult == Android.Content.PM.Permission.Granted)
{
    // Permission granted - go ahead and start the fingerprint scanner.
}
else
{
    // No permission. Go and ask for permissions and don't start the scanner. See
    // http://developer.android.com/training/permissions/requesting.html
}
```

Aplikacja ma sprawdzać warunki, 3, 4 i 5 zawsze, gdy chce używać odcisku palca uwierzytelniania. Pierwsze dwa warunki można sprawdzić aplikację po raz pierwszy jest uruchamiana na urządzeniu i zapisać wyniki (w udostępnionych preferencji na przykład).

Aby uzyskać więcej informacji na temat, aby zażądać uprawnień w systemie Android 6.0, zapoznaj się z podręcznikiem Android [żąda uprawnienia w czasie wykonywania](http://developer.android.com/training/permissions/requesting.html).



## <a name="related-links"></a>Linki pokrewne

- [Kontekst](https://developer.xamarin.com/api/type/Android.Content.Context/)
- [ContextCompat](https://developer.xamarin.com/api/type/Android.Support.V4.Content.ContextCompat/)
- [KeyguardManager](https://developer.xamarin.com/api/type/Android.App.KeyguardManager/)
- [FingerprintManager](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
- [Żądanie uprawnień w czasie wykonywania](http://developer.android.com/training/permissions/requesting.html)
