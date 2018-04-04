---
title: Błąd projektanta RegisterServicePort systemu iOS
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 929A0080-B126-4744-BF88-A4A1EFBB6CC2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 62d4a06c62bffb23566f4f59f42a0c980f417c45
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="ios-designer-error-with-registerserviceport"></a>Błąd projektanta RegisterServicePort systemu iOS

## <a name="sample-error"></a>Błąd próbki
> System.aggregateexception —: Wystąpił jeden lub więcej błędów---> System.SystemException: RegisterServicePort (com.xamarin.MTHosting.2a0b1, com.apple.PowerManagement.control): jądra zwrócił:-308 (-308): (ipc/mig) serwera zostało przerwane

## <a name="explanation"></a>Wyjaśnienie
Błędy `RegisterServicePort` i podobne komunikaty o błędach jak powyżej są często problem z programami szpiegującymi i złośliwym oprogramowaniem na komputerze. Rozważ [komentarz na raport o usterce](https://bugzilla.xamarin.com/show_bug.cgi?id=21907#c4) Aby uzyskać więcej informacji oraz łącza do [dyskusjach prowadzonych na forum Apple](https://discussions.apple.com/thread/5596008) dotyczące sposobu usuwania możliwe infekcji. 

Aby pomóc w zdiagnozowaniu problemu, otwarcie aplikacji macOS **konsoli** i Usuń wszystkie pliki wewnątrz **raporty diagnostyczne użytkownika** sekcji [ http://screencast.com/t/y9i3NKcuMy ](http://screencast.com/t/y9i3NKcuMy). Następnie uruchom program Visual Studio dla komputerów Mac i spróbuj użyć projektanta. Jeśli nowe pliki dziennika są wyświetlane w tej sekcji, po Projektant nie może zainicjować, zapisz je firmie Microsoft w celu analizy.  

Należy pamiętać, że ważne jest, aby sprawdzić, czy ten plik jest: 
> /usr/lib/libimckit.dylib

Bez względu na wyniki powyżej czy ten plik istnieje, wyżej wymienione problem programów szpiegujących i złośliwym oprogramowaniem znajduje się na komputerze.  

Poniższe łącze zawiera kroki, aby usunąć ten programów szpiegujących i złośliwym oprogramowaniem: [http://www.thesafemac.com/arg-genieo/](http://www.thesafemac.com/arg-genieo/)  

