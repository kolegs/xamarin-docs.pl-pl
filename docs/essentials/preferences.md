---
title: Preferencje Xamarin.Essentials
description: Klasa preferencje zapisuje Preferencje aplikacji w magazynie kluczy i wartości.
ms.assetid: AA81BCBD-79BA-448F-942B-BA4415CA50FF
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 6cca9413cee40fde5b8bb8967db52db7a3a3382f
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-preferences"></a>Preferencje Xamarin.Essentials

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Preferencje** klasy pomaga do przechowywania preferencji aplikacji w magazynie kluczy i wartości.

## <a name="using-secure-storage"></a>Przy użyciu bezpiecznego magazynu

Dodaj odwołanie do Xamarin.Essentials w swojej klasy:

```csharp
using Xamarin.Essentials;
```

Aby zapisać wartości dla danego _klucza_ w preferencjach:

```csharp
Preferences.Set("my_key", "my_value");
```

Aby pobrać wartość z preferencji lub wartości domyślnej, jeśli nie należy ustawić:

```csharp
var myValue = Preferences.Get("my_key", "default_value");
```

Aby usunąć _klucza_ z preferencji:

```csharp
Preferences.Remove("my_key");
```

Aby usunąć wszystkie preferencje:

```csharp
Preferences.Clear();
```

Oprócz tych metod każdego zająć w opcjonalny `sharedName` można tworzyć dodatkowe kontenery preferencji. Przeczytaj poniższe szczegóły implementacji platformy.

## <a name="supported-data-types"></a>Obsługiwane typy danych

Obsługiwane są następujące typy danych w **preferencje**:

- **bool**
- **double**
- **int**
- **float**
- **long**
- **string**

## <a name="platform-implementation-specifics"></a>Szczegóły implementacji platformy

# <a name="androidtabandroid"></a>[Android](#tab/android)

Wszystkie dane są przechowywane w [udostępnionych preferencji](https://developer.android.com/training/data-storage/shared-preferences.html). Jeśli nie `sharedName` określono są używane wartości domyślne udostępnionych preferencji, przeciwnym razie nazwa jest używany do pobierania **prywatnej** udostępnionych preferencji o określonej nazwie.

# <a name="iostabios"></a>[iOS](#tab/ios)

[NSUserDefaults](https://docs.microsoft.com/en-us/xamarin/ios/app-fundamentals/user-defaults) jest używany do przechowywania wartości na urządzeniach z systemem iOS. Jeśli nie `sharedName` określono `StandardUserDefaults` są używane, przeciwnym razie nazwa jest używana do tworzenia nowego `NSUserDefaults` o określonej nazwie, które są używane do `NSUserDefaultsType.SuiteName`.

# <a name="uwptabuwp"></a>[PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS](#tab/uwp)

[ApplicationDataContainer](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdatacontainer) jest używany do przechowywania wartości na urządzeniu. Jeśli nie `sharedName` określono `LocalSettings` są używane, else nazwa jest używana do utworzenia nowego kontenera wewnątrz `LocalSettings`.

--------------

## <a name="limitations"></a>Ograniczenia

W przypadku przechowywania ciąg, ten interfejs API jest przeznaczony do przechowywania niewielkich ilości tekstu.  Wydajność może być niepoprawne, Jeśli spróbujesz używany do przechowywania dużych ilości tekstu.

## <a name="api"></a>interfejs API

- [Preferencje kodu źródłowego](https://github.com/xamarin/Essentials/tree/master/Essentials/Preferences)
- [Dokumentacja interfejsu API preferencji](xref:Xamarin.Essentials.Preferences)
