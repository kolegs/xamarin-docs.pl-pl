---
title: Migrowanie powiązanie do ujednoliconego interfejsu API
description: W tym artykule opisano kroki wymagane do istniejącego projektu powiązanie Xamarin do obsługi Unified API dla aplikacji platformy Xamarin.IOS i Xamarin.Mac aktualizacji.
ms.prod: xamarin
ms.assetid: 5E2A3251-D17F-4F9C-9EA0-6321FEBE8577
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 25641671992a125e97bf7feff84b754423527da6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="migrating-a-binding-to-the-unified-api"></a>Migrowanie powiązanie do ujednoliconego interfejsu API

_W tym artykule opisano kroki wymagane do istniejącego projektu powiązanie Xamarin do obsługi Unified API dla aplikacji platformy Xamarin.IOS i Xamarin.Mac aktualizacji._

## <a name="overview"></a>Omówienie

Uruchamianie 1 lutego 2015 Apple wymaga, aby wszystkie nowe oświadczenia iTunes i Mac App Store musi aplikacji 64-bitowych. W związku z tym wszystkie nowej aplikacji platformy Xamarin.iOS lub Xamarin.Mac będzie konieczne przy użyciu nowego interfejsu API Unified zamiast istniejących MonoTouch klasycznego i interfejsów API MonoMac obsługuje 64-bitowego.

Ponadto wszystkie powiązania projektu Xamarin również musi obsługiwać nowe Unified API do uwzględnienia w 64-bitowej platformy Xamarin.iOS lub Xamarin.Mac projektu. W tym artykule opisano kroki wymagane do istniejącego projektu powiązanie używanie interfejsu API Unified aktualizacji.

## <a name="requirements"></a>Wymagania

Następujące wymagane jest wykonanie czynności przedstawionych w tym artykule:

 -  **Visual Studio for Mac** — najnowszej wersji programu Visual Studio for Mac zainstalowany i skonfigurowany na komputerze dewelopera.
 -  **Mac firmy Apple** — mac jest wymagane do utworzenia powiązania projektów dla systemu iOS firmy Apple i komputerów Mac.

Projekty powiązania nie są obsługiwane w programie Visual studio na komputerze z systemem Windows.

## <a name="modify-the-using-statements"></a>Zmodyfikuj instrukcje przy użyciu

Interfejsy API Unified ułatwia niż kiedykolwiek współużytkowanie kodu Mac i z systemem iOS, jak również umożliwiając obsługę 32- i 64 bitowych aplikacji o tej samej binarnego. Przez usunięcie _MonoMac_ i _MonoTouch_ prefiksy z przestrzeni nazw, łatwiejsze udostępnianie odbywa się w projektach aplikacji Xamarin.Mac i platformy Xamarin.iOS.

W związku z tym musimy zmodyfikuj dowolny z naszych umów powiązania (i innych `.cs` pliki w projekcie naszych powiązanie) do usunięcia _MonoMac_ i _MonoTouch_ prefiksy z naszych `using` instrukcje.

Przykładowo, podana następujące instrukcje using w kontrakcie powiązania:

```csharp
using System;
using System.Drawing;
using MonoTouch.Foundation;
using MonoTouch.UIKit;
using MonoTouch.ObjCRuntime;
```

Firma Microsoft będzie paska `MonoTouch` prefiksu, co zapewnia następujące:

```csharp
using System;
using System.Drawing;
using Foundation;
using UIKit;
using ObjCRuntime;
```

Ponownie, musimy to zrobić dla każdego `.cs` pliku w projekcie naszych powiązania. Z tą zmianą w miejscu następnym krokiem jest zaktualizować naszych powiązania projektu do natywnej nowe typy danych.

Aby uzyskać więcej informacji na temat interfejsu API Unified, zobacz [Unified API](~/cross-platform/macios/unified/index.md) dokumentacji. Więcej tła na obsługa 32- i 64 bitowych aplikacji, jak i informacji na temat struktury zobacz [32 i 64-bitowy zagadnień dotyczących platformy](~/cross-platform/macios/32-and-64/index.md) dokumentacji.

## <a name="update-to-native-data-types"></a>Aktualizacja do typów danych w trybie macierzystym

Mapuje Objective-C `NSInteger` typ danych `int32_t` w systemach 32-bitowe i do `int64_t` w systemach 64-bitowych. Aby dopasować to zachowanie, nowy interfejs API Unified zastępuje wcześniejszych zastosowań `int` (który w .NET jest zdefiniowany jako zawsze `System.Int32`) na nowy typ danych: `System.nint`.

Wraz z nową `nint` typu danych interfejsu API Unified wprowadza `nuint` i `nfloat` typów, aby mapowanie `NSUInteger` i `CGFloat` również typy.

Mając na uwadze powyższe, należy przejrzeć interfejsie API i upewnij się, że wszystkie wystąpienia `NSInteger`, `NSUInteger` i `CGFloat` który mamy poprzednio zamapowany na `int`, `uint` i `float` zostać zaktualizowane do nowej `nint`, `nuint` i `nfloat` typów.

Na przykład Podana definicja metody Objective-C:

```csharp
-(NSInteger) add:(NSInteger)operandUn and:(NSInteger) operandDeux;
```

Jeśli poprzednie kontraktu powiązania ma następującą definicję:

```csharp
[Export("add:and:")]
int Add(int operandUn, int operandDeux);
```

Zaktualizujemy nowego powiązania należy:

```csharp
[Export("add:and:")]
nint Add(nint operandUn, nint operandDeux);
```
Jeśli firma Microsoft mapowanie do nowszej wersji 3 strona biblioteki niż co możemy była początkowo połączony, musimy Przejrzyj `.h` pliki nagłówkowe, biblioteki i sprawdź, czy wszystkie istniejących, jawnego wywołania `int`, `int32_t`, `unsigned int`, `uint32_t` lub `float` uaktualniona do `NSInteger`, `NSUInteger` lub `CGFloat`. Jeśli tak, te same zmiany na `nint`, `nuint` i `nfloat` typów należy do ich także mapowania.

Aby dowiedzieć się więcej o tych zmianach typu danych, zobacz [natywnych typów](~/cross-platform/macios/nativetypes.md) dokumentu.

## <a name="update-the-coregraphics-types"></a>Aktualizowanie typów CoreGraphics

Typy danych punktu, rozmiar i prostokąt, które są używane z `CoreGraphics` Użyj 32 lub 64 bitów w zależności od urządzenia, są uruchamiane na. Gdy Xamarin pierwotnie powiązana z systemami iOS i Mac interfejsów API użyliśmy istniejącej struktury danych, które stało się zgodne z typami danych w `System.Drawing` (`RectangleF` na przykład).

Ze względu na wymagania obsługuje 64-bitowy i nowych typów danych w trybie macierzystym, trzeba wprowadzone do istniejącego kodu podczas wywoływania metody następujące dostosowania `CoreGraphic` metod:

- **CGRect** -użyj `CGRect` zamiast `RectangleF` podczas definiowania zmiennoprzecinkową punktu prostokątne regionów.
- **CGSize** -użyj `CGSize` zamiast `SizeF` podczas definiowania przestawne rozmiarach (szerokość i wysokość).
- **CGPoint** -użyj `CGPoint` zamiast `PointF` po definiowanie, liczbą zmiennoprzecinkową wskaż lokalizację (współrzędne X i Y).

Mając na uwadze powyższe, musimy Przejrzyj interfejsie API i upewnij się, że wszystkie wystąpienia `CGRect`, `CGSize` lub `CGPoint` był wcześniej powiązany z `RectangleF`, `SizeF` lub `PointF` można zmienić na typ macierzysty `CGRect`, `CGSize` lub `CGPoint` bezpośrednio.

Na przykład, dla danego języka Objective-C inicjatora elementu:

```csharp
- (id)initWithFrame:(CGRect)frame;
```

Jeśli naszych poprzednie powiązanie zawiera następujący kod:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (RectangleF frame);

```

Zaktualizujemy kod do:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);

```

Biorąc pod uwagę zmiany kodu w miejscu należy zmodyfikować naszych powiązania projektu lub pliku do powiązania z interfejsów API Unified.

## <a name="modify-the-binding-project"></a>Modyfikowanie powiązania projektu

Ostatni krok do aktualizowania naszych powiązania projektu do interfejsów API Unified, należy zmodyfikować `MakeFile` używanego do kompilacji projektu lub typu projektu Xamarin (jeśli mamy są powiązanie z poziomu programu Visual Studio dla komputerów Mac) oraz poinstruuj _btouch_  do powiązania API Unified zamiast klasycznego z nich.


### <a name="updating-a-makefile"></a>Aktualizowanie pliku reguł programu make

Jeśli użyto pliku reguł programu make do tworzenia projektu naszych powiązania do platformy Xamarin. Biblioteki DLL, musimy dołączyć `--new-style` opcji wiersza polecenia i wywołanie `btouch-native` zamiast `btouch`.

Dlatego podane następujące `MakeFile`:

```csharp
BINDDIR=/src/binding
XBUILD=/Applications/Xcode.app/Contents/Developer/usr/bin/xcodebuild
PROJECT_ROOT=XMBindingLibrarySample
PROJECT=$(PROJECT_ROOT)/XMBindingLibrarySample.xcodeproj
TARGET=XMBindingLibrarySample
BTOUCH=/Developer/MonoTouch/usr/bin/btouch


all: XMBindingLibrary.dll

libXMBindingLibrarySample-i386.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphonesimulator -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphonesimulator/lib$(TARGET).a $@

libXMBindingLibrarySample-arm64.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch arm64 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

libXMBindingLibrarySample-armv7.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch armv7 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

libXMBindingLibrarySampleUniversal.a: libXMBindingLibrarySample-armv7.a libXMBindingLibrarySample-i386.a libXMBindingLibrarySample-arm64.a
    lipo -create -output $@ $^

XMBindingLibrary.dll: AssemblyInfo.cs XMBindingLibrarySample.cs extras.cs libXMBindingLibrarySampleUniversal.a
    $(BTOUCH) -unsafe -out:$@ XMBindingLibrarySample.cs -x=AssemblyInfo.cs -x=extras.cs --link-with=libXMBindingLibrarySampleUniversal.a,libXMBindingLibrarySampleUniversal.a

clean:
    -rm -f *.a *.dll
```

Potrzebujemy przełączyć się z telefoniczną `btouch` do `btouch-native`, dlatego firma Microsoft może dopasować naszym definicji makra w następujący sposób:

```csharp
BTOUCH=/Developer/MonoTouch/usr/bin/btouch-native
```

Zaktualizujemy wywołanie `btouch` i Dodaj `--new-style` opcji w następujący sposób:

```csharp
XMBindingLibrary.dll: AssemblyInfo.cs XMBindingLibrarySample.cs extras.cs libXMBindingLibrarySampleUniversal.a
    $(BTOUCH) -unsafe --new-style -out:$@ XMBindingLibrarySample.cs -x=AssemblyInfo.cs -x=extras.cs --link-with=libXMBindingLibrarySampleUniversal.a,libXMBindingLibrarySampleUniversal.a
```

Teraz możemy wykonywać naszych `MakeFile` normalne do tworzenia nowej wersji 64-bitowym interfejsie API.

### <a name="updating-a-binding-project-type"></a>Aktualizowanie typu powiązania projektu

Jeśli użyto programu Visual Studio for Mac powiązanie szablonu projektu do kompilacji w interfejsie API, musimy aktualizować do nowej wersji Unified API szablonu projektu powiązania. Najprostszym sposobem, w tym celu jest zacząć od początku wszystkich istniejących kodu i ustawienia nowy projekt powiązanie interfejsu API Unified i kopii.

Wykonaj następujące czynności:

1. Uruchom program Visual Studio dla komputerów Mac.
2. Wybierz **pliku** > **nowe** > **rozwiązania...**
3. W oknie dialogowym rozwiązania nowe wybierz **iOS** > **Unified API** > **iOS powiązania projektu**: 

    [![](update-binding-images/image01new.png "W oknie dialogowym usługi nowe rozwiązanie wybrać system iOS / Unified API / Project powiązania z systemem iOS")](update-binding-images/image01new.png#lightbox)
4. W oknie dialogowym z "Konfigurowanie nowego projektu" Wprowadź **nazwa** nowy projekt powiązania i kliknij **OK** przycisku.
5. Zawierają wersję 64-bitowej biblioteki języka Objective-C, która ma być tworzenia powiązań.
6. Kopiowanie za pośrednictwem kodu źródłowego z istniejącego projektu powiązanie klasycznego interfejsu API 32-bitowe (takie jak `ApiDefinition.cs` i `StructsAndEnums.cs` plików).
7. Zmiany powyżej dostrzeżone do plików kodu źródłowego.

Biorąc pod uwagę te zmiany w miejscu można tworzyć nowej wersji 64-bitowych interfejsu API, jak w przypadku 32-bitowej wersji.

## <a name="summary"></a>Podsumowanie

W tym artykule firma Microsoft wykazały zmiany, które należy wprowadzić do istniejącego projektu Xamarin powiązanie do obsługi nowych interfejsów API Unified i urządzeniach 64-bitowe i kroki wymagane do utworzenia nowej wersji 64-bitowych zgodne interfejsu API.



## <a name="related-links"></a>Linki pokrewne

- [Mac i z systemem iOS](~/cross-platform/macios/index.md)
- [Ujednolicony interfejs API](~/cross-platform/macios/nativetypes.md)
- [zagadnienia dotyczące platformy 32/x 64](~/cross-platform/macios/32-and-64/index.md)
- [Uaktualnianie istniejącego aplikacji dla systemu iOS](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Ujednolicony interfejs API](~/cross-platform/macios/unified/index.md)
- [BindingSample](https://developer.xamarin.com/samples/monotouch/BindingSample/)
