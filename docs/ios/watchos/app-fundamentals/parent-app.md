---
title: Praca z watchOS nadrzędnej aplikacji w programie Xamarin
description: Ten dokument zawiera opis sposobu pracy z watchOS nadrzędnej aplikacji w programie Xamarin. Zawarto informacje WatchKit rozszerzeń aplikacji, aplikacje dla systemu iOS, Magazyn udostępniony i.
ms.prod: xamarin
ms.assetid: 9AD29833-E9CC-41A3-95D2-8A655FF0B511
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 3af2cce0d84e3934eeb89917990f111d29aadef1
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790695"
---
# <a name="working-with-the-watchos-parent-application-in-xamarin"></a>Praca z watchOS nadrzędnej aplikacji w programie Xamarin

> [!IMPORTANT]
> Uzyskiwanie dostępu do aplikacji nadrzędnej tylko za pomocą poniższych przykładów działa na aplikacje czujki watchOS 1.


Istnieją różne sposoby komunikacji między aplikacją czujki i aplikacji dla systemu iOS, które jest powiązane z:

- Obejrzyj rozszerzenia może [wywołania metody](#code) względem aplikacji nadrzędnej, która działa w tle na telefonie iPhone.

- Obejrzyj rozszerzenia może [miejsce przechowywania udziału](#storage) z aplikacją iPhone nadrzędnej.

- Przy użyciu przekazaniem do przekazywania danych z na pierwszy rzut oka lub powiadomienie aplikacji czujki, wysyłanie użytkownika do kontrolera interfejsu określonego w aplikacji.

Aplikacji nadrzędnej jest czasami nazywany kontenera aplikacji.


<a name="code" />

## <a name="run-code"></a>Uruchamianie kodu

Komunikacji między rozszerzenia czujki i aplikacji iPhone nadrzędny jest przedstawiona w [próbki GpsWatch](https://developer.xamarin.com/samples/GpsWatch).
Rozszerzenie czujki może wysłać żądanie dotyczące aplikacji dla systemu iOS nadrzędnego celu przetwarza w jego imieniu przy użyciu `OpenParentApplication` metody.

Jest to szczególnie przydatne w przypadku długotrwałe zadania (takie jak żądania sieci) — tylko nadrzędnej aplikacji dla systemu iOS mogą wykorzystać do wykonania tych zadań i zapisać pobrane dane w lokalizacji dostępnej dla rozszerzenia czujki przetwarzania w tle.



### <a name="watch-kit-app-extension"></a>Obejrzyj zestaw aplikacji rozszerzenia

Wywołanie `WKInterfaceController.OpenParentApplication` w czujki rozszerzenie aplikacji. Zwraca `bool` wskazująca, czy żądanie metody została wysłana pomyślnie lub nie. Sprawdź `error` parametru w odpowiedzi `Action` można określić, czy wszystkie podczas metody uruchamiania w aplikacji iPhone.

```csharp
WKInterfaceController.OpenParentApplication (new NSDictionary (), (replyInfo, error) => {
    if(error != null) {
        Console.WriteLine (error);
        return;
    }
    Console.WriteLine ("parent app responded");
    // do something with replyInfo[] dictionary
});
```


### <a name="ios-app"></a>Aplikacja systemu iOS

Wywołania z rozszerzeniem czujki aplikacji są wysyłane za pośrednictwem aplikacji iPhone `HandleWatchKitExtensionRequest` metody.
Jeśli dokonywania różnych żądań w aplikacji czujki, tej metody należy zbadać `userInfo` słownika w celu ustalenia sposobu przetwarzania żądania.


```csharp
[Register ("AppDelegate")]
public partial class AppDelegate : UIApplicationDelegate
{
    // ... other AppDelegate methods
    public override void HandleWatchKitExtensionRequest
        (UIApplication application, NSDictionary userInfo, Action<NSDictionary> reply)
    {
        var status = 2;
        // do something in the background, and respond
        reply (new NSDictionary (
            "count", NSNumber.FromInt32 ((int)status),
            "value2", new NSString("some-info")
            ));
    }
}
```


<a name="storage" />

## <a name="shared-storage"></a>Magazyn udostępniony

Jeśli skonfigurujesz [grupy aplikacji](~/ios/watchos/app-fundamentals/app-groups.md) , a następnie rozszerzenia systemu iOS 8 (w tym rozszerzenia czujki) mogą udostępniać dane z aplikacji nadrzędnej.

<a name="nsuserdefaults" />

### <a name="nsuserdefaults"></a>NSUserDefaults

Następujący kod, mogą być napisane w zarówno czujki rozszerzenia aplikacji, jak i aplikacji iPhone nadrzędny tak, aby odwołujących się ze wspólnego zestawu `NSUserDefaults`:

```csharp
NSUserDefaults shared = new NSUserDefaults(
        "group.com.your-company.watchstuff",
        NSUserDefaultsType.SuiteName);

// set values
shared.SetInt (2, "count");
shared.Synchronize ();

// get values
shared.Synchronize ();
var count = shared.IntForKey ("count");
```

<a name="files" />

### <a name="files"></a>Pliki

Rozszerzenie czujki i aplikacji systemu iOS można również udostępniać pliki przy użyciu wspólnej ścieżki pliku.

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =
            FileManager.GetContainerUrl ("group.com.your-company.watchstuff");
var appGroupContainerPath = appGroupContainer.Path;
Console.WriteLine ("agcpath: " + appGroupContainerPath);
// use the path to create and update files
```

Uwaga: Jeśli ścieżka jest `null` Sprawdź [konfigurację grupy aplikacji](~/ios/watchos/app-fundamentals/app-groups.md) zapewnienie profilów aprowizacji zostały prawidłowo skonfigurowane i zostały pobrane/zainstalowany na komputerze dewelopera.

Aby uzyskać więcej informacji, zobacz [możliwości grupy aplikacji](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) dokumentacji.

## <a name="wormholesharp"></a>WormHoleSharp

Popularne mechanizm open source watchOS 1 (na podstawie [MMWormHole](https://github.com/mutualmobile/MMWormhole)) do przekazywania danych lub polecenia między aplikacji nadrzędnej i obejrzyj aplikacji.

Można skonfigurować za pomocą grupy aplikacji, takich jak to w aplikacji systemu iOS tunel i obejrzyj rozszerzenia:

```csharp
// AppDelegate (iOS) or InterfaceController (watch extension)
Wormhole wormHole;
// ...
wormHole = new Wormhole ("group.com.your-company.watchstuff", "messageDir");
```

Pobierz wersję języka C# [WormHoleSharp](https://github.com/Clancey/WormHoleSharp).



## <a name="related-links"></a>Linki pokrewne

- [GpsWatch (przykład)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [WormHoleSharp (przykład)](https://github.com/Clancey/WormHoleSharp)
- [Odwołanie WKInterfaceController firmy Apple](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/clm/WKInterfaceController/openParentApplication:reply:)
- [Firmy Apple udostępnianie danych zawierającego aplikacji](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
