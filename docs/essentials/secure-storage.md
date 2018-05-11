---
title: Xamarin.Essentials bezpiecznego magazynu
description: Klasa SecureStorage pomaga bezpiecznie przechowywać pary klucz wartość proste.
ms.assetid: 78856C0D-76BB-406E-A880-D5A3987B7D64
author: redth
ms.author: jodick
ms.date: 05/04/2018
ms.openlocfilehash: 24d1e29ba0203aaafc3e21533478f6c505cc09b3
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-secure-storage"></a>Xamarin.Essentials bezpiecznego magazynu

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**SecureStorage** klasy pozwala bezpiecznie przechowywać pary klucz wartość proste.

## <a name="using-secure-storage"></a>Przy użyciu bezpiecznego magazynu

Dodaj odwołanie do Xamarin.Essentials w swojej klasy:

```csharp
using Xamarin.Essentials;
```

Aby zapisać wartości danego _klucza_ w bezpiecznego magazynu:

```csharp
await SecureStorage.SetAsync("oauth_token", "secret-oauth-token-value");
```

Aby pobrać wartości z bezpiecznego magazynu:

```csharp
var oauthToken = await SecureStorage.GetAsync("oauth_token");
```

## <a name="platform-implementation-specifics"></a>Szczegóły implementacji platformy

# <a name="androidtabandroid"></a>[Android](#tab/android)

[Android KeyStore](https://developer.android.com/training/articles/keystore.html) jest używany do przechowywania kluczy szyfrowania używany do szyfrowania wartość, dopóki zostanie zapisane w [udostępnionych preferencji](https://developer.android.com/training/data-storage/shared-preferences.html) z nazwą pliku z **.xamarinessentials [YOUR-APP-pakiet-ID]** .  Klucz w pliku udostępnionych preferencji jest _wyznaczania wartości skrótu MD5_ klucza przekazany `SecureStorage` interfejsu API.

## <a name="api-level-23-and-higher"></a>Interfejs API na poziomie 23 i nowsze

Na nowsze poziomy interfejsu API **AES** klucza uzyskane z systemem Android magazynu kluczy i używać z **NoPadding-AES/GCM** szyfrowania można zaszyfrować wartości przed są przechowywane w pliku udostępnionych preferencji.

## <a name="api-level-22-and-lower"></a>Interfejs API na poziomie 22 i małe

Na starsze poziomy interfejsu API systemu Android magazynu kluczy obsługuje tylko przechowywania **RSA** klucze, które jest używane z **RSA/ECB/PKCS1Padding** szyfrowania do szyfrowania **AES** (losowo klucza generowane w czasie wykonywania) i przechowywane w pliku udostępnionych preferencji w kluczu _SecureStorageKey_, jeśli nie zostały jeszcze wygenerowane.

Gdy aplikacja zostanie odinstalowana z urządzenia zostaną usunięte wszystkie zaszyfrowane wartości.

# <a name="iostabios"></a>[iOS](#tab/ios)

[Łańcucha kluczy](https://developer.xamarin.com/api/type/Android.Security.KeyChain/) jest używany do przechowywania wartości w bezpieczny sposób na urządzeniach z systemem iOS.  `SecRecord` Używany do przechowywania wartości ma `Service` wartość **.xamarinessentials [YOUR-APP-pakietu-ID]**.

W niektórych przypadkach dane łańcucha kluczy są synchronizowane z usługą iCloud i odinstalowywania aplikacji nie może zostać usunięta bezpiecznego wartości z innych urządzeń użytkownika i usługi iCloud.

# <a name="uwptabuwp"></a>[PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS](#tab/uwp)

[DataProtectionProvider](https://docs.microsoft.com/en-us/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider) służy do wartości encryped bezpiecznie na urządzeniach platformy uniwersalnej systemu Windows.

Encryped wartości są przechowywane w `ApplicationData.Current.LocalSettings`, wewnątrz kontenera o nazwie **.xamarinessentials [YOUR-APP-ID]**.

Odinstalowywanie aplikacji spowoduje, że _LocalSettings_i można również usunąć wszystkie zaszyfrowane wartości.

-----

## <a name="limitations"></a>Ograniczenia

Ten interfejs API jest przeznaczony do przechowywania niewielkich ilości tekstu.  Wydajność może być wolne, Jeśli spróbujesz używany do przechowywania dużych ilości tekstu.

## <a name="api"></a>interfejs API

- [Kod źródłowy SecureStorage](https://github.com/xamarin/Essentials/tree/master/Essentials/SecureStorage)
- [Dokumentacja interfejsu API SecureStorage](xref:Xamarin.Essentials.SecureStorage)
