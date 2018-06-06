---
title: Generowanie kodu .xib w Xamarin.iOS
description: W tym dokumencie opisano, jak Xamarin.iOS generuje kod do mapowania plików .xib C#, udostępnienie visual formanty w programowo.
ms.prod: xamarin
ms.assetid: 365991A8-E07A-0420-D28E-BC4D32065E1A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 064e17393747a36cd761cb2464e3239cfc17141c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786151"
---
# <a name="xib-code-generation-in-xamarinios"></a>Generowanie kodu .xib w Xamarin.iOS

> [!IMPORTANT]
>  W tym dokumencie opisano Visual Studio dla komputerów Mac integracji z usługą tylko w środowisku Xcode interfejsu konstruktora jako akcje i gniazda nie są używane w Projektancie Xamarin dla systemu iOS. Aby uzyskać więcej informacji w systemie iOS projektanta, zapoznaj się z tematem [iOS projektanta](~/ios/user-interface/designer/index.md) dokumentu.

Narzędzia Apple konstruktora interfejsu ("IB") można zaprojektować wizualnie interfejsów użytkownika. Definicje interfejsu utworzone przez IB są zapisywane w **.xib** plików. Elementy widget i innych obiektów w **.xib** pliki mogą mieć "klasy tożsamości", który może być zdefiniowana przez użytkownika typu niestandardowego. Dzięki temu można dostosować zachowanie elementów widget i zapisują niestandardowe elementy widget.

Te klasy użytkownika są zazwyczaj podklas klas kontrolera interfejsu użytkownika. Mają one *gniazda* (odpowiednikiem właściwości) i *akcje* (odpowiednikiem zdarzeń) które mogą być połączone z obiektami interfejsu. W czasie wykonywania podczas ładowania pliku IB obiekty są tworzone i gniazda i akcje są podłączone do różnych obiektów interfejsu użytkownika dynamicznie. Podczas definiowania tych klas zarządzanych, należy zdefiniować wszystkie akcje i gniazda, aby dopasować te, które IB oczekuje. Visual Studio for Mac korzysta z modelu plik CodeBehind podobne do uproszczenia tego procesu. Jest to podobne do Xcode jest dla języka Objective-C, ale model generowanie kodu i konwencje ma zostały tweaked więcej znać deweloperom platformy .NET.

Praca z **.xib** plików nie jest obecnie obsługiwane w Xamarin.iOS dla programu Visual Studio.

## <a name="xib-files-and-custom-classes"></a>pliki .xib i klas niestandardowych

Oraz przy użyciu istniejących typów z Cocoa Touch, istnieje możliwość definiowania niestandardowych typów w **.xib** plików. Istnieje również możliwość użycia których typy zostały zdefiniowane w innych **.xib** pliki lub zdefiniowany wyłącznie w kodzie języka C#. Konstruktor interfejsu nie jest obecnie znane Szczegóły typów zdefiniowanych poza bieżącą **.xib** pliku zostanie umieszczone na liście lub nie Pokaż ich gniazda niestandardowych i akcje. Usunięcie tego ograniczenia planowane pewnego czasu w przyszłości.

Można zdefiniować klas niestandardowych w **.xib** plików za pomocą polecenia "Dodaj podklasy" na karcie "Klasy" interfejsu konstruktora. Firma Microsoft można znaleźć je jako klasy "Plik CodeBehind". Jeśli **.xib** plik ma ". xib.designer.cs" odpowiednikiem pliku w projekcie, a następnie programu Visual Studio for Mac zostanie automatycznie wypełnić go definicje klas częściowych dla klas niestandardowych w **.xib**. Nazywamy tych klas częściowych "projektanta klas".

## <a name="generating-code"></a>Generowanie kodu

Dla każdego  **{0}.xib** pliku z akcją kompilacji *strony*, jeśli  **{0}. xib.designer.cs** również istnieje plik projektu programu Visual Studio dla komputerów Mac wygeneruje częściowej klasy w pliku projektanta dla wszystkich klas użytkowników można znaleźć w **.xib** pliku, z gniazda właściwości i metod częściowych dla wszystkich działań. Generowanie kodu zostało włączone po prostu przez występowanie tego pliku.

Pliku projektanta zostanie automatycznie zaktualizowane **.xib** pliku zmiany i Visual Studio dla komputerów Mac odzyskał fokus. Pliku projektanta nie powinien być modyfikowany ręcznie, jak zmiany zostaną zastąpione następnym Visual Studio dla komputerów Mac aktualizacji pliku.

## <a name="registration-and-namespaces"></a>Rejestracja i przestrzenie nazw

Visual Studio for Mac generuje projektanta klas przy użyciu domyślnej przestrzeni nazw projektu do lokalizacji pliku projektanta, aby był zgodny z normalnym namespacing projektu platformy .NET. Przestrzeń nazw plików projektanta wynikają z projektu "domyślnej przestrzeni nazw" i jego ustawienia "Zasady nazewnictwa platformy .NET". Należy pamiętać, że w przypadku zmiany projektu domyślnej przestrzeni nazw, MD spowoduje ponowne wygenerowanie klas w nowej przestrzeni nazw, dlatego może się okazać, że z klasy częściowe nie są zgodne.

Klasa stał się wykrywalny przez środowisko uruchomieniowe języka Objective-C, Visual Studio dla komputerów Mac dotyczy `[Register (name)]` do klasy atrybutu. Mimo że Xamarin.iOS automatycznie rejestruje `NSObject`-pochodzi z klasy, używa nazwy .NET w pełni kwalifikowane. Atrybut zastosowany przez program Visual Studio dla komputerów Mac zastępuje to w celu zapewnienia każdej klasy został zarejestrowany za pomocą nazwę używaną w **.xib** pliku. Jeśli używasz klas niestandardowych w IB bez korzystania z programu Visual Studio for Mac na wygenerowanie plików projektanta, może mieć jej zastosowania ręcznie, aby utworzyć sieci zarządzanej klasy pasują do oczekiwanej nazwy klas języka Objective-C.

Klas nie może być zdefiniowana w więcej niż jednym **.xib**, lub może powodować konflikt.

## <a name="non-designer-class-parts"></a>Części z systemem innym niż projektanta klas

Projektant klasy częściowe nie są przeznaczone do użycia jako — jest. Gniazda są prywatne, a nie podstawowy nie jest określona. Oczekuje się, że każda klasa będzie miało odpowiedniej części "z systemem innym niż projektanta" klasy w innym pliku, który ustawia klasę podstawową, używa lub udostępnia punkty i definiuje konstruktorów, które są wymagane do utworzenia wystąpienia klasy z kodu macierzystego podczas ładowania **.xib**. Wartość domyślna **.xib** szablony to zrobić, ale dla żadnych dodatkowych klas niestandardowych można zdefiniować w **.xib**, należy ręcznie dodać części z systemem innym niż projektanta.

Przyczyną tego jest potrzebę elastyczności. Na przykład wiele klas został można wspólnego zarządzane klasa abstrakcyjna, która podklasy klasy, która ma być podklasą klasy przez IB podklasy.

Jest konwencjonalnej można umieścić  **{0}. xib.cs** pliku obok  **{0}. xib.designer.cs** pliku projektanta.

<a name="generated" />

## <a name="generated-actions-and-outlets"></a>Generowane akcje i gniazda

Częściowe projektanta klas Visual Studio for Mac generuje właściwości odpowiadającej żadnych punktów połączonych zdefiniowane w IB i metody częściowe odpowiadającego do wszystkich połączonych akcji.

### <a name="outlet-properties"></a>Właściwości gniazda

Projektanta klas zawiera właściwości odpowiadającej gniazda wszystkie zdefiniowane dla klasy niestandardowej. Fakt, że są one właściwości jest implementacja szczegółowości Xamarin.iOS Mostek Objective C, aby umożliwić wiązanie opóźnieniem. Należy rozważyć ich równoważne pola prywatne, może być używany tylko z klasy plik CodeBehind. Jeśli chcesz były publicznych, jak w przypadku innych pole prywatne dodać właściwości metody dostępu do części z systemem innym niż projektanta klas.

Jeśli ma typ są zdefiniowane właściwości gniazda `id` (odpowiednikiem `NSObject`), a następnie generatora kodu projektanta Określa aktualnie najwyższego poziomu może typy oparte na obiektach podłączone do tego gniazda dla wygody.
Jednak to może być nieobsługiwana w przyszłych wersjach, dlatego zalecane jest jawnie silnie wpisz gniazda podczas definiowania klasy niestandardowej.

### <a name="action-properties"></a>Właściwości akcji

Projektanta klas zawiera metody częściowe odpowiadający wszystkich akcji zdefiniowane na niestandardowych klasy. Są to metody bez implementacji. Metody częściowe ma dwa cele:

1.  W przypadku wpisania `partial` w treści klasy części z systemem innym niż projektanta klas, Visual Studio for Mac będzie oferować autocomplete sygnatur wszystkie nie zaimplementowano metody częściowe.
2.  Sygnatury metody częściowej mają zastosowany atrybut, który udostępnia je w świecie Objective-C, mogą uzyskać obsługiwane jako działanie.


W razie potrzeby, może zignorować metody częściowej i wdrożenia działań przez zastosowanie atrybutu do innej metody lub pozwól mu przechodzić do klasy podstawowej.

Jeśli akcje są zdefiniowane ma typ nadawcy `id` (odpowiednikiem `NSObject`), a następnie generatora kodu projektanta Określa aktualnie najwyższy typ możliwe oparte na obiektach podłączone do tego działania. Jednak to może być nieobsługiwana w przyszłych wersjach, dlatego zalecane jest jawnie silnie wpisz akcje podczas definiowania klasy niestandardowej.

Należy pamiętać, że te metody częściowe są tworzone tylko dla C#, ponieważ CodeDOM nie obsługuje metody częściowe, nie są generowane dla innych języków.

## <a name="cross-xib-class-usage"></a>Użycie klasy XIB między

Czasami, chcesz odwołać się do tej samej klasy z wielu użytkowników **.xib** plików, na przykład z kontrolerami kartę. Można to zrobić przez explictly odwołuje się do definicji klasy z innego **.xib** plików lub poprzez definiowanie tej samej nazwie klasy ponownie w ciągu sekundy **.xib**.

Drugim przypadku mogą być problemy z powodu Visual Studio do przetwarzania Mac **.xib** pliki pojedynczo. Nie można automatycznie wykryć i scalić zduplikowane definicje, może to spowodować z konfliktami stosowania atrybutu rejestru wielokrotnie zdefiniowane w wielu plikach projektanta na tej samej klasy częściowej. Nowe wersje programu Visual Studio for Mac spróbować rozwiązać ten problem, ale może nie zawsze działać zgodnie z oczekiwaniami. W przyszłości jest mogą być nieobsługiwane i zamiast tego programu Visual Studio for Mac spowoduje wszystkich zdefiniowanych we wszystkich typów **.xib** plików i kod zarządzany w projekcie bezpośrednio widoczne ze wszystkich **.xib** plików.

## <a name="type-resolution"></a>Rozpoznawania typu

Typy używane w IB są nazwy typów języka Objective-C. Te są mapowane na typy CLR za pomocą atrybutów z rejestru. Podczas generowania kodu gniazda i akcji, Visual Studio for Mac rozwiązać odpowiednie typy CLR dla wszystkich typów języka Objective-C opakowane przez podstawowe Xamarin.iOS i pełnej kwalifikacji nazwy typu.

Jednak generatora kodu aktualnie nie można rozpoznać typów CLR z nazwy typów języka Objective-C w kodzie użytkownika lub bibliotek, więc w takich przypadkach danych wyjściowych dosłownego wyrażenia nazwy typu. Oznacza to, że odpowiedni typ CLR musi mieć taką samą nazwę jak typ Objective-C i musi być w tej samej przestrzeni nazw jako kod, który jest używany przez. Planowane jest ustalana pewnego czasu w przyszłości przez uwzględnieniu wszystkich typów języka Objective-C w projekcie podczas generowania kodu.
