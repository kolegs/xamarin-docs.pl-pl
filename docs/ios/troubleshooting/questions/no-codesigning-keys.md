---
title: 'Dlaczego moja kompilacja systemu iOS powiedzie z: nie kluczy podpisywania kodu prawidłowy dla telefonu iPhone znalezione w łańcuchu kluczy?'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9DF24C46-D521-4112-9B21-52EA4E8D90D0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 75f9a78fdc7d15df217378491f016478cde369ff
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351104"
---
# <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychain"></a>Dlaczego moja kompilacja systemu iOS powiedzie z: nie kluczy podpisywania kodu prawidłowy dla telefonu iPhone znalezione w łańcuchu kluczy?

## <a name="cause-of-the-error"></a>Przyczyny tego błędu
Ten komunikat o błędzie występuje, gdy w danym projekcie poszukuje o prawidłowe poświadczenia podpisywania kodu, ale nie można go znaleźć. Podpisywanie kodu jest wymagany do testowania i wdrażania na urządzeniach fizycznych z systemem iOS; Podobnie jak Ad-hoc, aplikacji i przechowywania kompilacji. 


### <a name="provisioning-devices"></a>Inicjowanie obsługi administracyjnej urządzeń
Jeśli jeszcze nie przeprowadzono aprowizacji urządzenia z systemem iOS przed, następującymi wskazówkami prowadzi przez proces pełne instrukcje krok po kroku: [przewodnik aprowizacji urządzeń](~/ios/get-started/installation/device-provisioning/index.md)


## <a name="bug-when-using-ios-simulator"></a>Usterkę występującą podczas korzystania z symulatora systemu iOS

> [!NOTE]
> Ten problem został rozwiązany w nowszych wersjach platformy Xamarin dla programu Visual Studio. Jednak jeśli ten problem występuje w najnowszej wersji oprogramowania, sprawdź plik [nową usterkę](~/cross-platform/troubleshooting/questions/howto-file-bug.md) za pomocą swojej wersji pełne informacje i pełny dziennik dane wyjściowe kompilacji.


Wystąpił błąd w 3.11 Studio Xamarin.Visual, który spowodował projekt dla systemu iOS w szablonie zestawu narzędzi Xamarin.Forms, aby dodać codesign plik Entitlements.plist symulatorze kompilacji; skutecznie blokuje testowania przy użyciu symulatora.

### <a name="how-to-fix"></a>Jak naprawić
Można obejść ten problem, usuwając `<CodesignEntitlements>` flagi z poziomu pozycji Debuguj kompilacje w pliku csproj. Można to zrobić w następujący sposób:

*Ostrzeżenie: Błędy w plikach csproj może przerwać projektu, dzięki czemu jest dobrym pomysłem jest tworzenia kopii zapasowych plików przed podjęciem próby wykonania to.*

1. Kliknij prawym przyciskiem myszy projekt iOS, w okienku rozwiązań i wybierz **Zwolnij projekt**
2. Kliknij prawym przyciskiem myszy projekt ponownie i wybierz **edytowanie pliku .csproj [nazwa_projektu]**
3. Znajdź PropertyGroups debugowania, powinien rozpoczynać flagi, które wyglądają następująco:
   - Debugowanie: `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">`
   - Wersja: `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhoneSimulator' ">`
4. W każdej kompilacji, które użyć symulatora należy usunąć lub komentarz z następującymi właściwościami: `<CodesignEntitlements>Entitlements.plist</CodesignEntitlements>`
5. Załaduj ponownie projekt i powinno być możliwe do wdrożenia w symulatorze.

### <a name="next-steps"></a>Następne kroki
Aby uzyskać dalszą pomoc, skontaktuj się z nami lub jeśli ten problem pozostaje nawet po zakończeniu wykorzystujących powyższe informacje, zobacz [jakie opcje pomocy technicznej są dostępne dla programu Xamarin?](~/cross-platform/troubleshooting/support-options.md) uzyskać informacji na temat opcji kontaktu, sugestie, a także jak Zgłoś usterkę nowy, jeśli to konieczne. 
