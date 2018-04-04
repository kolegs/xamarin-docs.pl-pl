---
title: Architektury Procesora
description: Xamarin.Android obsługuje kilka architektury Procesora, łącznie z urządzeniami 32-bitowe i 64-bitowych. W tym artykule wyjaśniono, jak wybrać aplikację do co najmniej jeden architektury Procesora obsługiwany system Android.
ms.prod: xamarin
ms.assetid: D4BC889D-9164-49BB-9B7B-F6C4E4E109F1
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: dea5aaa16891893f649d5ec56f3e6b1ee9a18683
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="cpu-architectures"></a>Architektury Procesora

_Xamarin.Android obsługuje kilka architektury Procesora, łącznie z urządzeniami 32-bitowe i 64-bitowych. W tym artykule wyjaśniono, jak wybrać aplikację do co najmniej jeden architektury Procesora obsługiwany system Android._

## <a name="cpu-architectures-overview"></a>Omówienie architektury Procesora

Podczas przygotowywania aplikacji dla wersji, należy określić które architektur platform procesor CPU obsługuje Twojej aplikacji. Pojedynczy APK może zawierać kod maszynowy do obsługi wielu różnych architektur. Każda kolekcja architektury kodu jest skojarzony z *binarny interfejsu aplikacji* (ABI). Każdy ABI definiuje, jak ten kod maszynowy może korzystać z systemu Android w czasie wykonywania.
Aby uzyskać więcej informacji na temat sposobu wykonywania, zobacz [urządzeń wielordzeniowych &amp; Xamarin.Android](~/android/deploy-test/multicore-devices.md).


## <a name="how-to-specify-supported-architectures"></a>Jak określać obsługiwane architektury

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Zazwyczaj należy jawnie wybierz architekturę (lub architektury), gdy aplikacja jest skonfigurowany dla **wersji**. Jeśli aplikacja została skonfigurowana do **debugowania**, **Użyj udostępnionych w czasie wykonywania** i **Użyj szybkiego wdrożenia** opcje są włączone, które wyłącz wybieranie jawne architektury.

W programie Visual Studio, kliknij dwukrotnie **właściwości** w obszarze projektu w **Eksploratora rozwiązań** i wybierz **Android opcje** strony. Kliknij przycisk **pakowania** kartę i sprawdź, czy **Użyj udostępnionych w czasie wykonywania** jest wyłączone (wyłączenie tej opcji można jawnie wybrać które ABIs do obsługi). Kliknij przycisk **zaawansowane** kartę i w obszarze **właściwości zaawansowane**, sprawdź architektury, które mają być obsługiwane:

[![Wybieranie armeabi i armeabi v7a](cpu-architectures-images/vs/01-abi-selections-sml.png)](cpu-architectures-images/vs/01-abi-selections.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Zazwyczaj należy jawnie wybierz architekturę (lub architektury), gdy aplikacja jest skonfigurowany dla **wersji**. Jeśli aplikacja została skonfigurowana do **debugowania**, **użycie wspólnych Mono środowiska uruchomieniowego** i **szybkie wdrożenie zestawu** opcje są włączone, uniemożliwiające jawne architektury Wybór.

W programie Visual Studio dla komputerów Mac, zlokalizować projektu w **rozwiązania** konsoli, kliknij koło zębate ikonę obok projektu i wybierz **opcje**. W **opcje projektu** okna dialogowego, kliknij przycisk **Android kompilacji**. Kliknij przycisk **ogólne** kartę i sprawdź, czy **użycie wspólnych Mono środowiska uruchomieniowego** jest wyłączone (wyłączenie tej opcji można jawnie wybrać które ABIs do obsługi). Kliknij przycisk **zaawansowane** kartę i w obszarze **obsługiwane ABIs**, sprawdź ABIs dla architektury, które mają być obsługiwane:

[![Wybieranie armeabi i armeabi v7a](cpu-architectures-images/xs/01-abi-selections-sml.png)](cpu-architectures-images/xs/01-abi-selections.png#lightbox)

-----


Xamarin.Android obsługuje architektury następujące:

-   **armeabi** &ndash; opartego na architekturze ARM procesorów CPU, które obsługuje co najmniej ARMv5TE zestaw instrukcji. Należy pamiętać, że `armeabi` nie jest bezpieczne wątkowo i nie powinna być używana na urządzeniach Wieloprocesorowych.

-   **armeabi v7a** &ndash; procesorów oparte na usłudze ARM przy użyciu wielu urządzeń procesora CPU (SMP) i operacjach liczb zmiennoprzecinkowych sprzętu. Należy pamiętać, że `armeabi-v7a` kod maszynowy nie będzie uruchamiany na urządzeniach ARMv5.

-   **arm64 v8a** &ndash; oparty na architekturze ARMv8 64-bitowych procesorach.

-   **x86** &ndash; procesorów CPU, które obsługują x86 (lub IA-32) zestaw instrukcji. Ten zestaw instrukcji jest odpowiednikiem Pentium Pro, w tym instrukcje MMX, SSE, SSE2 i SSE3.

-   **X86_64** procesorów CPU, które obsługują x86 64-bitowych (określane również jako *x64* i *AMD64*) zestaw instrukcji.

Domyślnie Xamarin.Android `armeabi-v7a` dla **wersji** kompilacji. To ustawienie zapewnia znacznie lepszą wydajność niż `armeabi`. Jeśli ma być przeznaczona dla platformy ARM 64-bitowej (na przykład 9 węzła), wybierz `arm64-v8a`. Jeśli wdrażasz aplikację x86 urządzenia, wybierz opcję `x86`. Jeśli urządzenie docelowe x86 korzysta z architektury Procesora 64-bitowych, wybierz `x86_64`.

## <a name="targeting-multiple-platforms"></a>Przeznaczenie do wielu platform

Docelowy kilku architektury Procesora, można wybrać więcej niż jeden ABI (kosztem większego rozmiaru pliku APK). Można użyć **Generowanie jeden pakiet (apk) na wybranych ABI** opcji (opisany w [Ustaw właściwości pakowania](~/android/deploy-test/release-prep/index.md#Set_Packaging_Properties)) można utworzyć oddzielne APK dla każdej obsługiwanej architektury.

Nie trzeba wybrać **arm64 v8a** lub **x86_64** pod kątem urządzeniach 64-bitowych; Obsługa 64-bitowych nie jest wymagana do uruchamiania aplikacji na 64-bitowym sprzęcie. Na przykład urządzeniach ARM 64-bitowych (takie jak [9 węzła](http://www.google.com/nexus/9/)) mogą korzystać z aplikacji skonfigurowanych do `armeabi-v7a`. Główną zaletą włączania obsługi 64-bitowych jest aby umożliwić aplikacji w celu rozwiązania więcej pamięci.

> [!NOTE]
> Obsługa 64-bitowego środowiska uruchomieniowego obecnie jest to funkcja eksperymentalna. Należy pamiętać, że 64-bitowego środowiska uruchomieniowe *nie* wymagane do uruchamiania aplikacji na urządzeniach 64-bitowych. 

## <a name="additional-information"></a>Dodatkowe informacje

W niektórych sytuacjach może być konieczne tworzenie oddzielnych APK dla każdej architektury (Aby zmniejszyć rozmiar Twojej APK lub aplikacji ma udostępnione bibliotek, które są specyficzne dla określonej architektury Procesora).
Aby uzyskać więcej informacji na temat tej metody, zobacz [kompilacji APKs specyficzne dla interfejsu ABI](~/android/deploy-test/building-apps/abi-specific-apks.md).
