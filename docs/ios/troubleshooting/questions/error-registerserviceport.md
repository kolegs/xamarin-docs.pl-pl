---
title: Błąd narzędzia iOS Designer registerserviceport
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 929A0080-B126-4744-BF88-A4A1EFBB6CC2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 9edcc822b170c3463908b9f5fb1db8b798346e3e
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350709"
---
# <a name="ios-designer-error-with-registerserviceport"></a>Błąd narzędzia iOS Designer registerserviceport

## <a name="sample-error"></a>Przykład błędu
> System.AggregateException: Wystąpił jeden lub więcej błędów---> System.SystemException: RegisterServicePort (com.xamarin.MTHosting.2a0b1, com.apple.PowerManagement.control): jądra zwrócone:-308 (-308): serwer (ipc/mig) zostało przerwane

## <a name="explanation"></a>Wyjaśnienie
Błędy `RegisterServicePort` i podobne komunikaty o błędach takie jak powyżej są często wystąpił problem z programami szpiegującymi i złośliwym oprogramowaniem na komputerze. Zastanów się nad [skomentować raport o usterce](https://bugzilla.xamarin.com/show_bug.cgi?id=21907#c4) Aby uzyskać więcej informacji oraz link do [dyskusjach prowadzonych na forum firmy Apple](https://discussions.apple.com/thread/5596008) dotyczących sposobu usuwania możliwe infekcji. 

Aby pomóc w zdiagnozowaniu problemu, Otwórz aplikację z systemem macOS **konsoli** i Usuń każdy plik w obrębie **raporty diagnostyczne użytkownika** sekcji [ http://screencast.com/t/y9i3NKcuMy ](http://screencast.com/t/y9i3NKcuMy). Następnie uruchom program Visual Studio dla komputerów Mac i spróbuj użyć projektanta. Jeśli wszystkie nowe pliki dziennika wyświetlane w tej sekcji po Projektant nie może zainicjować, zapisz je w firmie Microsoft w celu analizowania.  

Należy pamiętać, że ważne jest, aby wyszukać ten plik: 
> /usr/lib/libimckit.dylib

Niezależnie od powyższych wyniki, jeśli ten plik istnieje, wyżej programów szpiegujących i złośliwym oprogramowaniem problem polega na na tym komputerze.  

Kliknięcie następującego łącza zawiera kroki, aby usunąć to programy szpiegujące/złośliwe oprogramowanie: [http://www.thesafemac.com/arg-genieo/](http://www.thesafemac.com/arg-genieo/)  

