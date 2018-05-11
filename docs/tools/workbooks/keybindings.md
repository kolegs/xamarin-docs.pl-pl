---
title: Skróty klawiaturowe Edytor skoroszyty Xamarin
ms.prod: xamarin
ms.assetid: 6375A371-3215-4A7C-B97B-A19E58BE96D6
author: topgenorth
ms.author: toopge
ms.openlocfilehash: 1258a40a527ab47cee78b17454ac818e53c60ad2
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="xamarin-workbooks-editor-keyboard-shortcuts"></a>Skróty klawiaturowe Edytor skoroszyty Xamarin

## <a name="the-return-key-and-its-nuances"></a>Zwraca klucz i wszystkie jego szczegóły

W poniższej tabeli opisano różne powiązania klucza dla wykonywania kodu i tworzenia markdown. Przekierowaliśmy szczególną uwagę, aby wybrać za pośrednictwem i spójny powiązań klucza, które są znane i płynnych.

|Powiązanie klucza|Komórki kodu|Komórki języka znaczników markdown|
|--- |--- |--- |
|<kbd>Return</kbd>|<p>Pomyślnie przeanalizowano komórki karetka znajduje się na końcu buforu komórki, zostaną wykonane wyniki zostaną wyświetlone poniżej buforu i nowej komórki kodu zostanie wstawiony i fokus komórki po wykonanego komórki.</p><p>Jeśli podczas analizowania nie jest pomyślnie, znakiem nowego wiersza zostanie wstawiony w buforze. Diagnostyka kompilatora nie zostaną utworzone, jeśli podczas analizowania zakończy się niepowodzeniem.</p>|<p><kbd>Zwraca</kbd> wykazuje inaczej w zależności od kontekstu Markdown pod karetką.</p><ul><li>Jeśli karetka znajduje się w bloku kodu języka znaczników Markdown, jest wstawiany literału nowy wiersz.</li><li>Jeśli karetka znajduje się w bloku listy Markdown, Utwórz nowy element listy lub Podziel bieżącego elementu listy.</li><li>Jeśli karetka znajduje się w żadnym innym typem bloku Markdown, Utwórz nowy blok akapitu lub Podziel bieżący blok.</li></ul>|
|<dl><dt>Mac</dt><dd><kbd>Command‑Return</kbd></dd><dt>Win</dt><dd><kbd>Control‑Return</kbd></dd></dl>|<p>Zawsze próbuje analizuje i wykonuje zawartości komórki. Jeśli kompilacja zakończy się pomyślnie, wyniki (w tym wyjątki wykonywania) zostaną wyświetlone poniżej buforu, a jeżeli nie zawiera żadnych kolejnych komórek, nowy zostanie utworzona i fokus.</p><p>Jeśli występują błędy kompilacji, Diagnostyka będą wyświetlane, a bufor pozostanie ukierunkowanych z położenia karetki bez zmian.</p>|Wstawia i zespoły nowej komórki kodu po bieżącej komórki markdown.|
|<dl><dt>Mac</dt><dd><kbd>Command‑Shift‑Return</kbd><dd><dt>Win</dt><dd><kbd>Control‑Shift‑Return</kbd></dd></dl>|Wstawia i zespoły nowej komórki markdown po bieżącej komórki.|Jak <kbd>zwracać</kbd>|
|<kbd>Shift‑Return</kbd>|Zawsze Wstawia nowy wiersz, niezależnie od lokalizacji karetki i zawartości.|Wstawia podział wiersza w bieżącym bloku Markdown.|
