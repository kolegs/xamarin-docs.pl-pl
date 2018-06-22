---
title: 'Dlaczego moja kompilacji systemu iOS powiedzie z: nie prawidłowy iPhone podpisywania kluczy można odnaleźć kodu w łańcuchu kluczy?'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9DF24C46-D521-4112-9B21-52EA4E8D90D0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 5334e3009906896644caa47c715f912fa379c627
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30776693"
---
# <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychain"></a>Dlaczego moja kompilacji systemu iOS powiedzie z: nie prawidłowy iPhone podpisywania kluczy można odnaleźć kodu w łańcuchu kluczy?

## <a name="cause-of-the-error"></a>Przyczyny tego błędu
Ten komunikat o błędzie występuje, gdy danego projektu jest szuka prawidłowe poświadczenia podpisywania kodu, ale można je znaleźć. Podpisywanie kodu jest wymagane do testowania i wdrażania na urządzeniach iOS fizycznych; Podobnie jak Ad hoc & aplikacji magazynu kompilacji. 


### <a name="provisioning-devices"></a>Inicjowanie obsługi urządzeń
Jeśli nie zostało to jeszcze obsługi administracyjnej urządzeniu z systemem iOS przed, następującymi wskazówkami poprowadzi Cię przez proces pełnej krok po kroku proces: [przewodnik inicjowania obsługi urządzeń](~/ios/get-started/installation/device-provisioning/index.md)


## <a name="bug-when-using-ios-simulator"></a>Usterki, przy użyciu symulatora systemu iOS

> [!NOTE]
> Ten problem został rozwiązany w nowszych wersjach platformy Xamarin dla Visual Studio. Jednak jeśli problem wystąpi w najnowszej wersji oprogramowania, pliku [nowych usterek](~/cross-platform/troubleshooting/questions/howto-file-bug.md) z Twojej wersji pełne informacje i pełny dziennik dane wyjściowe kompilacji.


Wystąpił błąd podczas Xamarin.Visual Studio 3.11, który spowodował projekt dla systemu iOS w szablonie platformy Xamarin.Forms, aby dodać codesign Entitlements.plist do symulatora kompilacji; blokowanie skutecznie testowania przy użyciu symulatora.

### <a name="how-to-fix"></a>Jak rozwiązać
Można obejść ten problem przez usunięcie `<CodesignEntitlements>` flagę debugowania kompilacji w pliku .csproj. Można to zrobić w następujący sposób:

*Ostrzeżenie: Błędy w plikach .csproj mogą być dzielone projektu, więc warto przed kolejną próbą wykonania tej kopii zapasowej plików.*

1. Kliknij prawym przyciskiem myszy projekt iOS, w okienku rozwiązania i wybierz **Zwolnij projekt**
2. Kliknij prawym przyciskiem myszy projekt ponownie i wybierz **Edytuj .csproj [ProjectName]**
3. Zlokalizuj PropertyGroups debugowania, powinny rozpoczynać flagi, które wyglądają następująco:
   - Debugowanie: `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">`
   - Zlecenia: `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhoneSimulator' ">`
4. W każdej kompilacji, które używają symulatorze Usuń lub komentarz następującej właściwości: `<CodesignEntitlements>Entitlements.plist</CodesignEntitlements>`
5. Załaduj ponownie projekt i powinno być możliwe do wdrożenia na symulatorze.

### <a name="next-steps"></a>Następne kroki
Aby uzyskać dodatkową pomoc, skontaktuj się z nami, lub jeśli ten problem pozostanie nawet po użyciu powyższe informacje, zobacz [jakie opcje pomocy technicznej są dostępne dla platformy Xamarin?](~/cross-platform/troubleshooting/support-options.md) informacji o opcjach kontaktu, masz sugestie, oraz jak Plik nowe usterki, w razie potrzeby. 
