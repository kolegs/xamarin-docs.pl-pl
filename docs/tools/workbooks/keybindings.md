---
title: Skróty klawiaturowe w edytorze skoroszyty platformy Xamarin
description: W tym dokumencie opisano skróty klawiaturowe są dostępne do użycia w edytorze Xamarin Workbooks. W szczególności patrzy na różne sposoby, używane klawisz ENTER.
ms.prod: xamarin
ms.assetid: 6375A371-3215-4A7C-B97B-A19E58BE96D6
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: c2b4a8c1bcb8f7b88ab2ae1e2906b1c9c702b76a
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351681"
---
# <a name="xamarin-workbooks-editor-keyboard-shortcuts"></a>Skróty klawiaturowe w edytorze skoroszyty platformy Xamarin

## <a name="the-return-key-and-its-nuances"></a>Zwraca klucz i jego wszystkie szczegóły

W poniższej tabeli opisano różne powiązań klawiszy dla wykonania kodu i tworzenia języka znaczników markdown. Podejmujemy szczególną uwagę na wybranie rozsądne i spójny klawiszy, które są znane i płynny.

|Powiązanie klawiszy|Komórkę kodu|Komórka języka znaczników markdown|
|--- |--- |--- |
|<kbd>Return</kbd>|<p>Jeśli komórka może go pomyślnie przeanalizować karetka znajduje się pod koniec buforu komórki, zostaną wykonane wyniki zostaną wyświetlone poniżej buforu i nową komórkę kodu zostanie wstawiony i koncentruje się komórki komórką wykonane.</p><p>Jeśli podczas analizowania nie jest pomyślna, znakiem nowego wiersza zostanie wstawiony do buforu. Diagnostyka kompilatora nie zostaną wygenerowane, jeśli analiza kodu nie powiedzie się.</p>|<p><kbd>Zwróć</kbd> wykazuje różny sposób w zależności od kontekstu Markdown pod karetką.</p><ul><li>Jeśli karetka znajduje się w bloku kodu Markdown, jest wstawiany literału nowy wiersz.</li><li>Jeśli karetka znajduje się w bloku listy języka znaczników Markdown, Utwórz nowy element listy lub Podziel bieżącego elementu listy.</li><li>Jeśli karetka znajduje się w dowolny inny typ bloku kodu Markdown, Utwórz nowy blok akapitu lub Podziel bieżący blok.</li></ul>|
|<dl><dt>Mac</dt><dd><kbd>Command‑Return</kbd></dd><dt>Win</dt><dd><kbd>Control‑Return</kbd></dd></dl>|<p>Zawsze próbuje przeanalizować i wykonaj zawartość komórki. Jeśli kompilacja zakończy się pomyślnie, wyniki (w tym wyjątki wykonywania) zostaną wyświetlone poniżej buforu, a w przypadku żadnych kolejnych komórek, zostanie utworzona i koncentruje się nową.</p><p>W przypadku jakichkolwiek błędów kompilacji diagnostyki zostanie wyświetlony, a bufor pozostanie wąsko zdefiniowany za pomocą pozycji karetkę bez zmian.</p>|Wstawia i koncentruje się nową komórkę kodu po bieżącej komórki w języku znaczników markdown.|
|<dl><dt>Mac</dt><dd><kbd>Command‑Shift‑Return</kbd><dd><dt>Win</dt><dd><kbd>Control‑Shift‑Return</kbd></dd></dl>|Wstawia i koncentruje się nową komórkę kodu markdown po bieżącej komórki.|Takie samo zachowanie jako <kbd>zwracają</kbd>|
|<kbd>Shift‑Return</kbd>|Zawsze powoduje wstawienie nowego wiersza, niezależnie od lokalizacji karetki lub zawartości.|Wstawia podział wiersza w bieżącym bloku języka znaczników Markdown.|
