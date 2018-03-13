---
title: "Kompilowanie dla różnych urządzeń"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3B259248-887E-3E4F-E09C-7AD28C2A8CEE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 12b8f51156c2ed750c59ef79522121c6c5d2c03c
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="compiling-for-different-devices"></a>Kompilowanie dla różnych urządzeń

Można skonfigurować właściwości kompilacji pliku wykonywalnego w projekcie **kompilacji systemu iOS** stronę właściwości, który znajduje się przez kliknięcie prawym przyciskiem myszy nazwę projektu i przechodząc do **Opcje > iOS kompilacji** w Visual Studio for Mac, i **właściwości** w programie Visual Studio:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


[![](compiling-for-different-devices-images/image1.png "Strony właściwości kompilacji projektów dla systemu iOS")](compiling-for-different-devices-images/image1.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](compiling-for-different-devices-images/image1a.png "Strony właściwości kompilacji projektów dla systemu iOS")](compiling-for-different-devices-images/image1a.png#lightbox)

-----

Oprócz opcje konfiguracji dostępne w interfejsie użytkownika, można również przekazać własny zestaw opcji wiersza polecenia, aby [Xamarin.iOS kompilacji narzędzia (mtouch)](~/ios/deploy-test/mtouch.md).

[http://iossupportmatrix.com/](http://iossupportmatrix.com/) jest przydatne zasób, który może służyć do upewnij się, łącznie z wszystkich wymaganych urządzeń, architektury i iOS wersji.

 <a name="SDK_Options" />


## <a name="sdk-options"></a>Opcje zestawu SDK

Visual Studio for Mac można skonfigurować dwie ważne właściwości powiązanych z zestawu SDK: wersja zestawu SDK dla systemu iOS, używany do tworzenia oprogramowania i cel wdrożenia (lub wersja minimalna wymagana dla systemu iOS).

IOS **zestawu SDK w wersji** opcja pozwala na użycie różnych wersji programu Apple opublikowane zestawu SDK, to kieruje Xamarin.iOS kompilatory, linkery i bibliotek, powinien odwoływać się podczas kompilacji. 

**Cel wdrożenia** ustawienie służy do wybierania minimalnej wymaganej wersji systemu operacyjnego, na którym aplikacja jest uruchamiana. To jest ustawiana w pliku Info.plist projektu. Należy wybrać minimalną wersją ma wszystkich interfejsów API, które są potrzebne do uruchomienia aplikacji.

Ogólnie rzecz biorąc, interfejsu API platformy Xamarin.iOS udostępnia wszystkie metody dostępne w najnowszej wersji zestawu SDK, a w razie potrzeby firma Microsoft udostępnia właściwości wygody, które pozwalają wykryć, czy funkcja jest dostępna w czasie wykonywania (na przykład `UIDevice.UserInterfaceIdiom` i `UIDevice.IsMultitaskingSupported` zawsze pracować na Xamarin.iOS, jak całą pracę w tle).

 <a name="Linking" />


## <a name="linking"></a>Konsolidacja

Zobacz stronę dotyczącą dedykowanych na [konsolidatora](~/ios/deploy-test/linker.md) Aby dowiedzieć się więcej na temat jak konsolidator pomaga zmniejszyć rozmiar Twoje pliki wykonywalne i aby dowiedzieć się, jak skutecznie go używać.

 <a name="Code_Generation_Engine" />


## <a name="code-generation-engine"></a>Aparat generowania kodu

Począwszy od platformy Xamarin.iOS 4.0, istnieją dwie zapleczy generacji kodu do platformy Xamarin.iOS. Aparatu generowania kodu Mono regularnych i oparty na LLVM optymalizacji kompilatora. Każdy aparat ma, jego zalet i wad.

Zazwyczaj podczas procesu projektowania prawdopodobnie użyjesz aparatu generowania kodu Mono jak będzie można szybko przejść. Wersja kompilacji i wdrażania sklepu z aplikacjami należy przełączyć się do aparatu generowania kodu LLVM.

LLVM optymalizacji aparatu wewnętrznej bazy danych tworzy kod zarówno szybsze i większego niż Mono aparatu, kosztem kompilacji długi czas.

Można włączyć je z iOS opcje kompilacji w programie Visual Studio dla komputerów Mac lub Visual Studio.

[![](compiling-for-different-devices-images/image2.png "Włączanie LLVM")](compiling-for-different-devices-images/image2.png#lightbox)

[![](compiling-for-different-devices-images/image2a.png "Włączanie LLVM")](compiling-for-different-devices-images/image2a.png#lightbox)

 <a name="ARMV7_and_ARMV7s_support" />


## <a name="architecture-support"></a>Wsparcie dla architektury

<a name="armv6-discontinued" />

### <a name="armv6-xamarinios-discontinued-support-for-armv6-with-v810"></a>ARMv6 (Xamarin.iOS zaprzestać obsługę ARMv6 v8.10)

- iPhone (oryginalny), 3G
- iPod generacji 1, 2

### <a name="armv7"></a>ARMv7

- iPhone 3GS, 4, 4S
- iPad 1, 2, 3, Mini
- iPod 3, 4, 5. Generowanie

### <a name="armv7s"></a>ARMv7s

- iPhone 5
- iPhone 5c
- iPad 4

W przypadku skierowania tylko procesor ARMv7s, kod wygenerowany zostanie nieco szybciej, ale go nie będzie działać w systemach ARMv7 lub ARMv6, chyba że skompilować fat binary, który zawiera wiele plików wykonywalnych w pakiecie.

### <a name="arm64-xamarinios-started-supporting-arm64-in-v86"></a>ARM64 (Xamarin.iOS uruchomiony Obsługa ARM64 w v8.6)

- iPhone 5s
- iPhone SE
- 6, iPhone 6 Plus
- telefonów iPhone 6s, 6s Plus
- iPhone 7, 7 Plus
- iPhone 8, 8 Plus
- iPhone X
- iPad lotniczego
- iPad 2 lotniczego
- iPad Mini 2, 3, 4
- iPad Pro (wszystkie)

Należy pamiętać, że wszelkie kompilacje przesłane do sklepu z aplikacjami musi zawierać Obsługa 64-bitowych jest to wymagane, ustawione przez [Apple](https://developer.apple.com/news/?id=12172014b). Ponadto iOS 11 obsługuje aplikacji 64-bitowych.

 <a name="ARM_Thumb_Support" />


### <a name="arm-thumb-2-support"></a>Obsługa usługi ARM Thumb-2

Jest używany przez procesorów ARM mniejszych zestaw instrukcji. Przez włączenie obsługi podglądu, można zmniejszyć rozmiar pliku wykonywalnego, kosztem wolniejsze czasu wykonywania. Thumb jest obsługiwana w ARMv7 i ARMv7s.

 <a name="Conditional_framwork_useage" />


## <a name="conditional-framework-usage"></a>Użycie warunkowego Framework

Jeśli projekt chce korzystać z niektóre funkcje w sklepie iOS nowsze wersje, konieczne może być warunkowo zależne od pewnych nowych struktur. Podstawowym przykładem jest pożądane uruchamiał iAd w systemie iOS w wersji 4.0 lub nowszej, ale nadal obsługuje 3.x urządzeń. W tym celu należy powiadomić Xamarin.iOS wiedzieć, czy musisz połączyć przed framework iAd "słabo". Słabe powiązania upewnij się, czy platformę tylko są ładowane na żądanie pierwszym razem, gdy klasa w ramach jest wymagana.

W tym celu należy wykonać następujące czynności:

-  Otwórz z **opcje projektu** i przejdź do **kompilacji systemu iOS** okienka.
-  Dodaj `'-gcc_flags "-weak_framework iAd"'` do **dodatkowe opcje** dla każdej konfiguracji chcesz słabo łącza:


[![](compiling-for-different-devices-images/image3.png "Dodatkowe opcje")](compiling-for-different-devices-images/image3.png#lightbox)


Oprócz tego należy zabezpieczyć użycie typów z korzystają ze starszych wersji systemu IOS, w którym mogą nie istnieć. Istnieje kilka metod, w tym celu, ale jeden z nich podczas analizowania `UIDevice.CurrentDevice.SystemVersion`.



## <a name="related-links"></a>Linki pokrewne

- [Konsolidator](~/ios/deploy-test/linker.md)
- [Zewnętrzne - macierz obsługi systemu iOS](http://iossupportmatrix.com/)
