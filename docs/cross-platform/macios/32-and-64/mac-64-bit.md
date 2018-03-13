---
title: Aktualizowanie Xamarin.Mac Unified aplikacji 64-bitowych
description: "W tym przewodniku opisano sposób aktualizowania aplikacji Xamarin.Mac do docelowego 64-bitowych"
ms.topic: article
ms.prod: xamarin
ms.assetid: C3810A74-539C-4FFB-B47F-68CA5F7BCDAD
ms.technology: xamarin-cross-platform
author: bradumbaugh
ms.author: brumbaug
ms.date: 02/22/2018
ms.openlocfilehash: b53ef7157ffc393759b7e808655ce64b4744dab5
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="updating-xamarinmac-unified-applications-to-64-bit"></a>Aktualizowanie Xamarin.Mac Unified aplikacji 64-bitowych

Począwszy od stycznia 2018, Firma Apple wymaga przez nowych [przesłanych Mac App Store target 64-bitowych](https://developer.apple.com/news/?id=06282017a). Aplikacje, które są już dostępne na Mac App Store musi zostać zaktualizowany do docelowego 64-bitowych przez 2018 czerwca.

**Pliku** > **nowy** Xamarin.Mac szablonu projektu aplikacji 64-bitowych domyślnie tworzy, więc dla wszystkich ostatnio utworzonych aplikacji są już zgodne z 64-bitowych i nie wymaga żadnych zmian.

## <a name="targeting-64-bit"></a>Przeznaczonych dla 64-bitowych

1. Otwórz **opcje projektu** okno automatycznie w przypadku aplikacji Xamarin.Mac:

   ![Menu kontekstowe projektu](mac-64-bit-images/1-contextual_menu-vsmac.png "menu kontekstowe dla projektu")

2. Wybierz **kompilacji Mac** i ustaw **obsługiwane architektury** do **x86\_64**:

   [![Ustawienie obsługiwane architektury x86_64](mac-64-bit-images/2-project_options-vsmac.png "x86_64 ustawienie obsługiwane architektury")](mac-64-bit-images/2-project_options-vsmac-large.png#lightbox)

3. Aplikacja ma wszelkie zależności zewnętrzne takie jak natywnego odwołania lub powiązania projektów, zaktualizuj je do docelowego 64-bitowych.

### <a name="errors"></a>błędy

Po raz pierwszy kompilacji lub uruchamianie aplikacji z obsługą 64-bitowych, mogą wystąpić błędy łącza z clang lub środowisko uruchomieniowe problemy. Te błędy mogą występować, jeśli innych firm zależności — na przykład natywnego odwołań w Xamarin.Mac lub powiązania projektów lub ręcznie załadować systemowe struktury — nie zostały zaktualizowane do 64-bitowej.

> [!TIP]
> Konwertowanie projektu na 64-bitowych jest istotne zmiany i pośrednio może ujawnić różne błędy programowania. W szczególności mogą ulec zmianie, rozmiar i wyrównanie struktur danych, które mogłyby wpłynąć na p/invoke podpisów i kod natywny połączone w projekcie. Należy wziąć pod uwagę recenzowania ostrzeżeń żadnych kompilacji i przetestować aplikację łatwe później do wychwytywania potencjalnych problemów.

#### <a name="example-error-resulting-from-a-dynamically-linked-third-party-dependency-that-does-not-target-64-bit"></a>Błąd przykład wynikające z połączone dynamicznie zależności innych firm, który nie ma 64-bitowe:

```console
ld : warning : ignoring file PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary, 
file was built for i386 which is not the architecture being linked (x86_64): 
PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary 
```

Ten błąd może występować w czasie wykonywania przez `dlopen` zwracanie `IntPtr.Zero` zamiast oczekiwanego dojścia.

#### <a name="example-error-resulting-from-a-statically-linked-third-party-dependency-that-does-not-target-64-bit"></a>Błąd przykład wynikające z statycznie połączone zależności innych firm, który nie ma 64-bitowe:

```console
Undefined symbols for architecture x86_64:
  "_LibraryFunction", referenced from:
     -u command line option
ld: symbol(s) not found for architecture x86_64 
```

Aby skompilować i uruchomić się pomyślnie, zaktualizuj te zależności do 64-bitowej i ponownie skompilować aplikację.

