---
title: Podstawy aplikacji
description: "Ten dokument prowadzi do prowadnic, które opisano pojęcia różnych zrozumienie podczas tworzenia aplikacji Xamarin.Mac."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5A36B3A7-F197-4AC3-A40D-B2C49362FF06
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/17/2015
ms.openlocfilehash: 9b64647fdb455978c3833ce1bc37f5e8784b9a78
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="application-fundamentals"></a>Podstawy aplikacji

## <a name="common-patterns-and-idiomsmacapp-fundamentalspatternsmd"></a>[Wspólne wzorce i idioms](~/mac/app-fundamentals/patterns.md)

W całej firmy Apple interfejsach API udostępnianych za pośrednictwem C# niektórych idioms i wzorce znaleziona wielokrotnie. Jeśli masz doświadczenie z programowania za pomocą platformy Xamarin.iOS te mogą wyglądać znajomo. Dokumentacja będzie często odnosić się do tych wzorców i idioms wielokrotnie, więc o stałych zrozumienie ich pomoże Ci sensu dokumentację można znaleźć.

## <a name="understanding-mac-apismacapp-fundamentalsmac-apismd"></a>[Interfejsy API interpretacji Mac](~/mac/app-fundamentals/mac-apis.md)

Dla wielu programowania z użyciem Xamarin.Mac czasu można wziąć pod uwagę, odczytać i zapisać w języku C# bez dużo problem z podstawowych interfejsów API języka Objective-C. Jednak czasami będzie można odczytać w dokumentacji interfejsu API firmy Apple, tłumaczenia odpowiedź od przepełnienie stosu do rozwiązania problemu, lub porównania istniejący przykład.

## <a name="working-with-xib-filesmacapp-fundamentalsxibmd"></a>[Praca z plikami .xib](~/mac/app-fundamentals/xib.md)

Ten artykuł dotyczy pracy z plikami .xib utworzone w Konstruktorze interfejsu w programie Xcode, można tworzyć i obsługiwać interfejsy użytkownika dla aplikacji Xamarin.Mac.

## <a name="storyboardxib-less-user-interface-designmacapp-fundamentalsxibless-uimd"></a>[.Storyboard/.XIb mniej projektu interfejsu użytkownika](~/mac/app-fundamentals/xibless-ui.md)

W tym artykule opisano tworzenie interfejsu użytkownika aplikacji Xamarin.Mac bezpośrednio w kodzie języka C# bez użycia w środowisku Xcode interfejsu konstruktora z plikami .storyboard lub .xib.

## <a name="working-with-imagesmacapp-fundamentalsimagemd"></a>[Praca z obrazami](~/mac/app-fundamentals/image.md)

Ten artykuł dotyczy pracy z obrazów i ikon w aplikacji Xamarin.Mac. Obejmuje tworzenie i obsługa obrazów potrzebne do utworzenia aplikacji ikony, jak i używanie obrazów w kodzie C# i kompilatora interfejsu w środowisku Xcode.

## <a name="data-binding-and-key-value-codingmacapp-fundamentalsdatabindingmd"></a>[Powiązanie danych i kluczy i wartości kodowania](~/mac/app-fundamentals/databinding.md)

W tym artykule omówiono przy użyciu kluczy i wartości kodowania i klucz wartość obserwowania, aby umożliwić wiązanie danych do elementów interfejsu użytkownika w Konstruktorze interfejsu w środowisku Xcode. Ta technika, można znacznie zmniejszyć ilość kodu C#, który musi być napisane dla aplikacji Xamarin.Mac. 

## <a name="working-with-databasesmacapp-fundamentalsdatabasesmd"></a>[Praca z bazami danych](~/mac/app-fundamentals/databases.md)

W tym artykule omówiono przy użyciu kluczy i wartości kodowania i klucz wartość obserwowania umożliwia do wiązania danych za pomocą bezpośredniego dostępu do bazy danych SQLite do elementów interfejsu użytkownika w Konstruktorze interfejsu w środowisku Xcode. Obejmuje ona również przy użyciu SQLite.NET ORM w celu zapewnienia dostępu do danych SQLite.

## <a name="working-with-copy-and-pastemacapp-fundamentalscopy-pastemd"></a>[Praca z kopiowania i wklejania](~/mac/app-fundamentals/copy-paste.md)

Ten artykuł dotyczy pracy z obszar roboczy, aby zapewnić kopiowania i wklejania w aplikacji Xamarin.Mac. Widoczny jest sposób pracy z typami standardowych danych, które mogą być współużytkowane między wiele aplikacji i sposobu obsługi danych niestandardowych w aplikacji należy podać.

## <a name="sandboxing-a-xamarinmac-appmacapp-fundamentalssandboxingmd"></a>[Sandboxing aplikacji Xamarin.Mac](~/mac/app-fundamentals/sandboxing.md)

W tym artykule omówiono sandboxing aplikacją Xamarin.Mac w wersji ze sklepu App Store. Obejmuje wszystkie elementy, które wchodzą w sandboxing: kontener katalogów, uprawnień, uprawnienia użytkownika, separacji uprawnień i wymuszania jądra.

## <a name="playing-sound-with-avaudioplayermacapp-fundamentalssoundsmd"></a>[Odtwarzanie dźwięku z AVAudioPlayer](~/mac/app-fundamentals/sounds.md)

W tym artykule pokazano, jak użyć Klasa pomocy do sterowania odtwarzanie dźwięku za pomocą AVAudioPlayer.

## <a name="reporting-bugsmacapp-fundamentalstroubleshootingmd"></a>[Raportowanie błędów](~/mac/app-fundamentals/troubleshooting.md)

Czasami możemy wszystkie zostać zablokowane podczas pracy nad projektem na braku możliwości można uzyskać interfejsu API w sposób którą chcemy udostępnić lub podczas próby obejść usterki. Naszym celem na platformie Xamarin jest dla Ciebie w pisanie aplikacji mobilnych i klasycznych było pomyślne, a przygotowaliśmy niektórych zasobów, aby pomóc.
