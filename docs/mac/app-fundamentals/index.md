---
title: Podstawy aplikacji rozszerzenia Xamarin.Mac
description: Ten dokument prowadzi do przewodników, które opisują różne koncepcje, które należy rozumieć w przypadku tworzenia aplikacji platformy Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 5A36B3A7-F197-4AC3-A40D-B2C49362FF06
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/17/2015
ms.openlocfilehash: e085cf33615d216e1ce9963254050ef2b623ae40
ms.sourcegitcommit: d0da5ce4158239abd2b36f67550e9b475055766a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39320807"
---
# <a name="xamarinmac-application-fundamentals"></a>Podstawy aplikacji rozszerzenia Xamarin.Mac

## <a name="common-patterns-and-idiomsmacapp-fundamentalspatternsmd"></a>[Wspólne wzorce i idiomy](~/mac/app-fundamentals/patterns.md)

W całym interfejsów API firmy Apple, udostępniane za pośrednictwem języka C# niektórych idiomy i wzorców jest znacznie wielokrotnie. Jeśli masz środowisko programowania, korzystając z platformy Xamarin.iOS, one wyglądać znajomo. Dokumentacja często dotyczy te wzorce i idiomy wielokrotnie, posiadanie dobrego zrozumienia ich pomoże Ci zrozumienie dokumentacji, które znajdujesz.

## <a name="understanding-mac-apismacapp-fundamentalsmac-apismd"></a>[Interfejsy API interpretacji Mac](~/mac/app-fundamentals/mac-apis.md)

Przez większość czasu opracowywanie zawartości przy użyciu platformy Xamarin.Mac można traktować, odczytu i zapisu w języku C# w zapewnieniu wiele z podstawowych interfejsów API języka Objective-C. Jednak czasami będziesz potrzebujesz w dokumentacji interfejsu API firmy Apple, tłumaczenia odpowiedź na podstawie Stack Overflow do rozwiązania problemu lub porównania istniejący przykład.

## <a name="console-appsmacapp-fundamentalsconsolemd"></a>[Aplikacje konsoli](~/mac/app-fundamentals/console.md)

Możesz również tworzyć aplikacje konsoli "bezobsługowe", uzyskujących dostęp do natywnych interfejsów API systemu macOS za pomocą platformy Xamarin.Mac.

## <a name="working-with-xib-filesmacapp-fundamentalsxibmd"></a>[Praca z .pliki XIb](~/mac/app-fundamentals/xib.md)

Ten artykuł dotyczy pracy z plikami .pliki utworzonymi w program Xcode Interface Builder umożliwia tworzenie i obsługę interfejsów użytkownika dla aplikacji platformy Xamarin.Mac.

## <a name="storyboardxib-less-user-interface-designmacapp-fundamentalsxibless-uimd"></a>[.Storyboard/.XIb mniej Projektowanie interfejsu użytkownika](~/mac/app-fundamentals/xibless-ui.md)

W tym artykule opisano tworzenie interfejsu użytkownika aplikacji dla platformy Xamarin.Mac bezpośrednio w kodzie języka C# bez użycia program Xcode Interface Builder .storyboard lub .pliki plików.

## <a name="working-with-imagesmacapp-fundamentalsimagemd"></a>[Praca z obrazami](~/mac/app-fundamentals/image.md)

Ten artykuł dotyczy pracy z obrazów i ikon w aplikacji platformy Xamarin.Mac. Obejmuje on tworzenia i utrzymywania obrazów potrzebne do utworzenia ikony aplikacji i używanie obrazów w kodzie języka C# i program Xcode Interface Builder.

## <a name="data-binding-and-key-value-codingmacapp-fundamentalsdatabindingmd"></a>[Powiązanie danych i kodowanie klucz wartość](~/mac/app-fundamentals/databinding.md)

W tym artykule opisano, przy użyciu kodowania i pary klucz wartość, monitorowanie, aby umożliwić wiązanie danych do elementów interfejsu użytkownika w program Xcode Interface Builder pary klucz wartość. Ta technika znacznie zmniejsza ilość kodu C#, która musi być napisane dla aplikacji platformy Xamarin.Mac. 

## <a name="working-with-databasesmacapp-fundamentalsdatabasesmd"></a>[Praca z bazami danych](~/mac/app-fundamentals/databases.md)

W tym artykule opisano, przy użyciu kodowania i obserwowania umożliwiający powiązanie danych za pomocą bezpośredniego dostępu do bazy danych SQLite w celu elementy interfejsu użytkownika w program Xcode Interface Builder pary klucz wartość pary klucz wartość. Obejmuje ona również przy użyciu Relacyjnego biblioteki SQLite.NET w celu zapewnienia dostępu do danych SQLite.

## <a name="working-with-copy-and-pastemacapp-fundamentalscopy-pastemd"></a>[Praca z kopiowania i wklejania](~/mac/app-fundamentals/copy-paste.md)

Ten artykuł dotyczy pracy z obszar roboczy, aby zapewnić kopiowania i Wklej w aplikacji platformy Xamarin.Mac. Pokazuje, jak pracować z typami danych w warstwie standardowa, które mogą być współużytkowane między wiele aplikacji i sposób obsługi danych niestandardowych w ramach aplikacji zapewniają.

## <a name="sandboxing-a-xamarinmac-appmacapp-fundamentalssandboxingmd"></a>[Tryb piaskownicy aplikacji rozszerzenia Xamarin.Mac](~/mac/app-fundamentals/sandboxing.md)

W tym artykule opisano tryb piaskownicy aplikacji Xamarin.Mac do wydania w Store aplikacji. Obejmuje wszystkie elementy, które bardziej szczegółowo piaskownicy: katalogów kontenera, uprawnień, uprawnienia określone przez użytkownika, separacji uprawnień i wymuszania jądra.

## <a name="playing-sound-with-avaudioplayermacapp-fundamentalssoundsmd"></a>[Odtwarzanie dźwięku za pomocą programu AVAudioPlayer](~/mac/app-fundamentals/sounds.md)

W tym artykule pokazano, jak używać klasy pomocnika do kontrolowania odtwarzanie dźwięku za pomocą pomocą odtwarzacza AVAudioPlayer.

## <a name="reporting-bugsmacapp-fundamentalstroubleshootingmd"></a>[Raportowania błędów](~/mac/app-fundamentals/troubleshooting.md)

Czasami wszyscy zatrzymywane, podczas pracy nad projektem z brakiem, aby uzyskać interfejs API umożliwiający pracę tak, firma Microsoft lub podczas próby obejść usterkę. Naszym celem w środowisku Xamarin jest dla Ciebie zakończy się powodzeniem w pisaniu aplikacji mobilnych i klasycznych i przygotowaliśmy kilka zasobów, aby pomóc.
