---
title: 'Xamarin.Essentials: preferencje'
description: W tym dokumencie opisano klasy Preferencje w Xamarin.Essentials, co pozwoli zaoszczędzić Preferencje aplikacji w magazynie kluczy/wartości. Omówiono w nim sposób używania klasy i typy danych, które mogą być przechowywane.
ms.assetid: AA81BCBD-79BA-448F-942B-BA4415CA50FF
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 4a45587c79cfbbcd1198f100915e698289f74950
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353753"
---
# <a name="xamarinessentials-preferences"></a>Xamarin.Essentials: preferencje

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Preferencje** klasy pomaga do przechowywania preferencji aplikacji w magazynie kluczy/wartości.

## <a name="using-preferences"></a>Za pomocą preferencji

Dodaj odwołanie do Xamarin.Essentials w klasie:

```csharp
using Xamarin.Essentials;
```

Aby zapisać wartość dla danego _klucza_ w preferencjach:

```csharp
Preferences.Set("my_key", "my_value");
```

Aby pobrać wartość z preferencji lub domyślny, jeśli nie należy ustawić:

```csharp
var myValue = Preferences.Get("my_key", "default_value");
```

Aby usunąć _klucz_ z preferencji:

```csharp
Preferences.Remove("my_key");
```

Aby usunąć wszystkie preferencje:

```csharp
Preferences.Clear();
```

Oprócz tych metod uwzględniać w opcjonalny `sharedName` można tworzyć dodatkowe kontenery preferencji. Przeczytaj poniższe szczegóły implementacji platformy.

## <a name="supported-data-types"></a>Obsługiwane typy danych

Następujące typy danych są obsługiwane w **preferencje**:

- **bool**
- **double**
- **int**
- **float**
- **long**
- **string**
- **DateTime**

## <a name="implementation-details"></a>Szczegóły dotyczące implementacji

Wartości typu `DateTime` są przechowywane w formacie pliku binarnego 64-bitowych (liczba całkowita typu long), przy użyciu dwóch metod zdefiniowanych przez `DateTime` klasy: [ `ToBinary` ](xref:System.DateTime.ToBinary) metoda służy do kodowania `DateTime` wartość i [ `FromBinary` ](xref:System.DateTime.FromBinary(System.Int64)) metoda dekoduje wartość. Zobacz, w dokumentacji tych metod do dostosowania, które zostaną podjęte do dekodowane wartości, gdy `DateTime` jest przechowywane, to znaczy nie wartości do uniwersalny czas koordynowany (UTC).

## <a name="platform-implementation-specifics"></a>Funkcje specyficzne dla implementacji platformy

# <a name="androidtabandroid"></a>[Android](#tab/android)

Wszystkie dane są przechowywane w [preferencje udostępniane](https://developer.android.com/training/data-storage/shared-preferences.html). Jeśli nie `sharedName` określono preferencje domyślne udostępnione są używane, inne nazwy służy do pobierania **prywatnej** udostępnionych preferencji przy użyciu określonej nazwy.

# <a name="iostabios"></a>[iOS](#tab/ios)

[NSUserDefaults](https://docs.microsoft.com/en-us/xamarin/ios/app-fundamentals/user-defaults) służy do przechowywania wartości na urządzeniach z systemem iOS. Jeśli nie `sharedName` określono `StandardUserDefaults` są używane, przeciwnym razie nazwa jest używana do tworzenia nowego `NSUserDefaults` o określonej nazwie, które są używane do `NSUserDefaultsType.SuiteName`.

# <a name="uwptabuwp"></a>[PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS](#tab/uwp)

[ApplicationDataContainer](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdatacontainer) służy do przechowywania wartości na urządzeniu. Jeśli nie `sharedName` określono `LocalSettings` są używane, inne nazwy służy do tworzenia nowego kontenera wewnątrz `LocalSettings`.

--------------

## <a name="persistence"></a>Stan trwały

Odinstalowywanie aplikacji spowoduje, że wszystkie _preferencje_ do usunięcia. Istnieje jeden wyjątek od tej reguły, która w przypadku aplikacjach docelowych, które są uruchomione na system Android 6.0 (poziom 23 interfejsu API) lub nowszej, za pomocą [ __automatyczne kopie zapasowe__](https://developer.android.com/guide/topics/data/autobackup). Ta funkcja jest domyślnie włączona i zachowuje danych aplikacji, w tym __preferencje udostępniane__, to znaczy elementy **preferencje** korzysta z interfejsu API. Tę opcję można wyłączyć przez następujące Google [dokumentacji](https://developer.android.com/guide/topics/data/autobackup).

## <a name="limitations"></a>Ograniczenia

W przypadku przechowywania ciągu, ten interfejs API jest przeznaczone do przechowywania niewielkich ilości tekstu.  Wydajność może być niepoprawne, jeśli zostanie podjęta próba go użyć do przechowywania dużych ilości tekstu.

## <a name="api"></a>interfejs API

- [Kod źródłowy preferencje](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Preferences)
- [Dokumentacja interfejsu API preferencje](xref:Xamarin.Essentials.Preferences)
