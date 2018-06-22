---
title: Gdzie można znaleźć pliku .dSYM symbolicate dzienniki awarii systemu iOS?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CB8607B9-FFDA-4617-8210-8E43EC512588
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/09/2018
ms.openlocfilehash: 60d897be8739ff5b78a322bc4ea3f43011785bb5
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
ms.locfileid: "33920660"
---
# <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logs"></a>Gdzie można znaleźć pliku .dSYM symbolicate dzienniki awarii systemu iOS?

Podczas tworzenia aplikacji systemu iOS przy użyciu programu Visual Studio dla komputerów Mac lub Visual Studio 2017, plik .dSYM potrzebne do symbolicate raporty awarii zostaną umieszczone w tej samej hierarchii katalogu co plik projektu aplikacji (.csproj). Lokalizacja zależy od ustawień kompilacji projektu:

- Jeśli włączono kompilacje specyficzne dla urządzenia .dSYM można znaleźć w następującym katalogu:

    **&lt;katalog projektu&gt;/bin/&lt;platformy&gt;/&lt;konfiguracji&gt;/device-builds /&lt;urządzenia&gt; - &lt; wersja systemu operacyjnego&gt;/**

    Na przykład:
  
    **TestApp/bin/iPhone/Release/device-builds/iphone8.4-11.3.1/**

- Jeśli nie włączono kompilacje specyficzne dla urządzenia, .dSYM można znaleźć w następującym katalogu:

    **&lt;katalog projektu&gt;/bin/&lt;platformy&gt;/&lt;konfiguracji&gt;/**

    Na przykład:

    **TestApp/bin/iPhone/Release /**

> [!NOTE]
> Jako część procesu kompilacji Visual Studio 2017 kopiuje plik .dSYM z hosta kompilacji Mac do systemu Windows. Jeśli nie ma pliku .dSYM w systemie Windows, należy skonfigurowano ustawienia kompilacji aplikacji [Utwórz plik IPA](~/ios/deploy-test/app-distribution/ipa-support.md).

## <a name="see-also"></a>Zobacz także

- [Symbolicating iOS awarii plików (Xamarin.iOS)](http://jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/)
- [Demystifying dzienniki awarii aplikacji systemu iOS](https://www.raywenderlich.com/23704/demystifying-ios-application-crash-logs)

