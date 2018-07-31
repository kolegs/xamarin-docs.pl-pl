---
title: 'Xamarin.Essentials: Bezpieczny magazyn'
description: W tym dokumencie opisano klasy SecureStorage w Xamarin.Essentials, co ułatwia bezpieczne przechowywanie par klucz wartość proste. Omówiono w nim sposób użycia klasy, funkcje specyficzne dla implementacji platformy i ograniczeń.
ms.assetid: 78856C0D-76BB-406E-A880-D5A3987B7D64
author: redth
ms.author: jodick
ms.date: 05/04/2018
ms.openlocfilehash: 2dfdb7051b269e73c68290a557849b9ae606c165
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353298"
---
# <a name="xamarinessentials-secure-storage"></a>Xamarin.Essentials: Bezpieczny magazyn

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**SecureStorage** klasy ułatwia bezpieczne przechowywanie par klucz wartość proste.

## <a name="getting-started"></a>Wprowadzenie

Aby uzyskać dostęp do **SecureStorage** , następujące konfiguracje specyficzne dla platformy jest wymagane:

# <a name="androidtabandroid"></a>[Android](#tab/android)

Żadna dodatkowa konfiguracja wymagana.

# <a name="iostabios"></a>[iOS](#tab/ios)

Podczas tworzenia w symulatorze systemu iOS, Włącz **pęku kluczy** uprawnienia i Dodaj grupy dostępu łańcucha kluczy, aby uzyskać identyfikator pakietu aplikacji.

Otwórz **plik Entitlements.plist** w projekcie dla systemu iOS i Znajdź **pęku kluczy** uprawnień i włącz ją. To spowoduje automatyczne dodanie identyfikator aplikacji jako grupą.

We właściwościach projektu w obszarze **podpisywanie pakietu systemu iOS** ustaw **niestandardowe uprawnienia** do **plik Entitlements.plist**.

# <a name="uwptabuwp"></a>[PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS](#tab/uwp)

Żadna dodatkowa konfiguracja wymagana.

-----

## <a name="using-secure-storage"></a>Korzystanie z bezpiecznego magazynu

Dodaj odwołanie do Xamarin.Essentials w klasie:

```csharp
using Xamarin.Essentials;
```

Aby zapisać wartość dla danego _klucza_ w bezpiecznym magazynie:

```csharp
try
{
  await SecureStorage.SetAsync("oauth_token", "secret-oauth-token-value");
}
catch (Exception ex)
{
  // Possible that device doesn't support secure storage on device.
}
```

Aby pobrać wartość z bezpiecznego magazynu:

```csharp
try
{
  var oauthToken = await SecureStorage.GetAsync("oauth_token");
}
catch (Exception ex)
{
  // Possible that device doesn't support secure storage on device.
}
```

> [!NOTE]
> Jeśli nie istnieje wartość skojarzoną z kluczem żądanego `GetAsync` zwróci `null`.

Aby usunąć określony klucz, należy wywołać:

```csharp
SecureStorage.Remove("oauth_token");
```

Aby usunąć wszystkie klucze, należy wywołać:

```csharp
SecureStorage.RemoveAll();
```


## <a name="platform-implementation-specifics"></a>Funkcje specyficzne dla implementacji platformy

# <a name="androidtabandroid"></a>[Android](#tab/android)

[Magazynu kluczy systemu Android](https://developer.android.com/training/articles/keystore.html) służy do przechowywania klucza szyfrowania używany do szyfrowania wartość, zanim zostaną zapisane w [preferencje udostępniane](https://developer.android.com/training/data-storage/shared-preferences.html) przy użyciu nazwy pliku z **.xamarinessentials [YOUR-APP-pakietu-ID]** .  Klucz używany w pliku udostępnionych preferencji jest _Skrót MD5_ klucza, który został przekazany do `SecureStorage` interfejsów API.

## <a name="api-level-23-and-higher"></a>Poziom interfejsu API 23 lub nowszy

W nowszych poziomy interfejsu API **AES** klucz jest uzyskiwany z magazynu kluczy systemu Android i używane z **AES/GCM/NoPadding** szyfrowania można zaszyfrować wartości przed są przechowywane w pliku udostępnionych preferencji.

## <a name="api-level-22-and-lower"></a>Poziom 22 interfejsu API i niższy

W starszych poziomy interfejsu API, magazynu kluczy systemu Android obsługuje tylko przechowywania **RSA** klucze, które jest używane z **RSA/ECB/PKCS1Padding** szyfrowania do szyfrowania **AES** klucza (losowo generowane w czasie wykonywania) i przechowywane w pliku udostępnionych preferencji w kluczu _SecureStorageKey_, jeśli nie został już wygenerowany.

**SecureStorage** używa [preferencje](preferences.md) interfejsu API i jest zgodny z tej samej funkcji trwałości danych opisanych w [preferencje](preferences.md#persistence) dokumentacji. Jeśli urządzenie uaktualnienia z poziomu interfejsu API 22 lub niższy poziom interfejsu API 23 lub nowszy, ten typ szyfrowania będą w dalszym ciągu używać, chyba że aplikacja zostanie odinstalowana lub **RemoveAll** jest wywoływana.

# <a name="iostabios"></a>[iOS](#tab/ios)

[Pęk kluczy](https://developer.xamarin.com/api/type/Security.SecKeyChain/) służy do przechowywania wartości w bezpieczny sposób na urządzeniach z systemem iOS.  `SecRecord` Używane do przechowywania wartości ma `Service` wartość **.xamarinessentials [YOUR-APP-pakietu-ID]**.

W niektórych przypadkach dane łańcucha kluczy są synchronizowane z usługi iCloud, a odinstalowywanie aplikacji nie może usunąć bezpiecznych wartości z usługi iCloud i innych urządzeń użytkownika.

# <a name="uwptabuwp"></a>[PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS](#tab/uwp)

[DataProtectionProvider](https://docs.microsoft.com/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider) służy do zaszyfrowanych wartości w bezpieczny sposób na urządzeniach platformy uniwersalnej systemu Windows.

Zaszyfrowane wartości są przechowywane w `ApplicationData.Current.LocalSettings`, wewnątrz kontenera o nazwie **.xamarinessentials [YOUR-APP-ID]**.

**SecureStorage** używa [preferencje](preferences.md) interfejsu API i jest zgodny z tej samej funkcji trwałości danych opisanych w [preferencje](preferences.md#persistence) dokumentacji.

-----

## <a name="limitations"></a>Ograniczenia

Ten interfejs API jest przeznaczone do przechowywania niewielkich ilości tekstu.  Wydajność może być powolne, jeśli zostanie podjęta próba go użyć do przechowywania dużych ilości tekstu.

## <a name="api"></a>interfejs API

- [Kod źródłowy SecureStorage](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/SecureStorage)
- [Dokumentacja interfejsu API SecureStorage](xref:Xamarin.Essentials.SecureStorage)
