---
title: Konfiguracja niestandardowych konsolidatora
ms.topic: article
ms.prod: xamarin
ms.assetid: F8A99E3F-2197-4399-AC81-F1DBAB5729C9
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: d5470dfc314677dbe558bca2f3ddfa107354c752
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="custom-linker-configuration"></a>Konfiguracja niestandardowych konsolidatora

Jeśli domyślny zestaw opcji nie jest wystarczająco, proces łączenia można dysku z plikiem XML opisujący ma z konsolidator.

Możesz podać dodatkowe definicje do konsolidatora, aby upewnić się, typ, metod i/lub pola nie są eliminowane z aplikacji. W swoim własnym kodem preferowaną metodą jest użycie `[Preserve]` atrybutu niestandardowego, jak opisano w [konsolidacji w systemie iOS](~/ios/deploy-test/linker.md) i [konsolidacji w systemie Android](~/android/deploy-test/linker.md) przewodników.
Jednak jeśli potrzebujesz definicje z zestawów SDK lub produktu, za pomocą pliku XML może być najlepsze rozwiązania (w przeciwieństwie Dodawanie kodu, który zapewni, że konsolidator nie wyeliminować, co jest potrzebne).

Aby to zrobić, należy zdefiniować plik XML z elementu najwyższego poziomu <linker> zawierającą *zestawu* węzły, które z kolei zawierają *typu* węzły, które z kolei zawierają *—Metoda* i *pola* węzłów.

Po utworzeniu tego pliku opisu konsolidatora, dodaj go do projektu i:

-  **Dla systemu Android** : Ustaw **Akcja kompilacji** do **LinkDescription**
-  **Dla systemu iOS** : Ustaw **Akcja kompilacji** do **LinkDescription**


W poniższym przykładzie przedstawiono plik XML wygląda następująco:

```xml
<linker>
        <assembly fullname="mscorlib">
                <type fullname="System.Environment">
                        <field name="mono_corlib_version" />
                        <method name="get_StackTrace" />
                </type>
        </assembly>
        <assembly fullname="My.Own.Assembly">
                <type fullname="Foo" preserve="fields">
                        <method name=".ctor" />
                </type>
                <type fullname="Bar">
                        <method signature="System.Void .ctor(System.String)" />
                        <field signature="System.String _blah" />
                </type>
                <namespace fullname="My.Own.Namespace" />
                <type fullname="My.Other*" />
        </assembly>
</linker>
```

W powyższym przykładzie konsolidator odczytuje i stosowane zgodnie z instrukcjami na `mscorlib.dll` (dostarczane z Mono dla systemu Android) i `My.Own.Assembly` zestawy (kod użytkownika).

Pierwsza sekcja dla `mscorlib.dll`, sprawdzi, czy `System.Environment` typu zachowa jego pola o nazwie `mono_corlib_version` i jego `get_StackTrace` metody.
Należy pamiętać, metody pobierającej i/lub jak konsolidator pracuje IL i nie rozpoznaje właściwości języka C# należy stosować nazwy metody ustawiającej.

Druga sekcja dla `My.Own.Assembly.dll`, sprawdzi, czy `Foo` typu zachowa jego pól (tzn. `preserve="fields"` atrybutu) i wszystkie jego konstruktorów (tzn. wszystkie metody o nazwie `.ctor` IL). `Bar` Typu zachowa określonych podpisów (a nie nazw) dla jednego konstruktora, (który akceptuje parametr będący pojedynczym ciągiem) i polem ciągu określonego `_blah`.
`My.Own.Namespace` Przestrzeni nazw zachowa wszystkie typy, które zawiera.
Ponadto dowolnego typu, którego Pełna nazwa (włącznie z przestrzenią nazw) zgodny z wzorcem symbolu wieloznacznego "My.Other\*" zachowa wszystkich swoich pól i metod. Symbol wieloznaczny `*` można dołączyć wiele razy w ciągu wzorca "wpisz imię i nazwisko".



## <a name="related-links"></a>Linki pokrewne

- [Łączenie w systemie iOS](~/ios/deploy-test/linker.md)
- [Łączenie w systemie Android](~/android/deploy-test/linker.md)
