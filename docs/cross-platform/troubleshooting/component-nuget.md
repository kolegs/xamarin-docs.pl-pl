---
title: Aktualizacja odwołania do składników do NuGet
description: Zamień odwołania do składnika pakietów NuGet do zabezpieczenie przyszłych potrzeb aplikacji.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9E6C986F-3FBA-4599-8367-FB0C565C0ADE
author: asb3993
ms.author: amburns
ms.date: 04/18/2018
ms.openlocfilehash: e9c056d37577a280b66bb3aa7ded19e966bca43b
ms.sourcegitcommit: 9f8e7393019791bbd6af4fefaa24a1602adabb4e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/23/2018
---
# <a name="updating-component-references-to-nuget"></a>Aktualizacja odwołania do składników do NuGet

> [!IMPORTANT]
> Magazyn składników nie jest obsługiwany począwszy od 15 maja 2018 (ta zamknięcia pierwotnie [ogłoszenia](https://blog.xamarin.com/hello-nuget-new-home-xamarin-components/) w listopadzie 2017).
>
> Xamarin składniki nie są już obsługiwane w programie Visual Studio i powinna zostać zastąpiona pakietów NuGet. Postępuj zgodnie z instrukcjami poniżej, aby ręcznie usunąć odwołania do składników z projektów.

Dotyczą te instrukcje dotyczące dodawania pakietów NuGet w [Windows](https://docs.microsoft.com/nuget/quickstart/use-a-package) lub [Mac](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).

Lista popularnych Xamarin [wtyczek i bibliotek](https://github.com/xamarin/XamarinComponents/blob/master/README.md) jest dostępny do znajdowania alternatywy dla składników, które są dostępne, ponieważ pakietów NuGet.

## <a name="manually-removing-component-references"></a>Ręczne usuwanie odwołań składnika

15.6 wydania programu Visual Studio i 7.4 wydania programu Visual Studio dla komputerów Mac nie obsługują składników w projekcie. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Po załadowaniu projektu w programie Visual Studio, zostanie wyświetlone następujące okno dialogowe z informacjami o tym, że należy usunąć wszystkie składniki z projektu ręcznie:

![Alert okna dialogowego wyjaśniający, czy składnik zostały znalezione w projekcie i muszą zostać usunięte](component-nuget-images/component-alert-vs.png)

Aby usunąć składnik z projektu:

1. Otwórz **.csproj** pliku. Aby to zrobić, kliknij prawym przyciskiem myszy nazwę projektu i wybierz **Zwolnij projekt**. 

2. Ponownie kliknij prawym przyciskiem myszy projekt zwolnione i wybierz **Edytuj .csproj {nazwa swój projekt}**.

3. Znajdź wszystkie odwołania w pliku `XamarinComponentReference`. Powinien wyglądać podobnie do poniższego przykładu:

    ```xml
    <ItemGroup>
      <XamarinComponentReference Include="advancedcolorpicker">
        <Version>2.0.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="gunmetaltheme">
        <Version>1.4.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="signature-pad">
        <Version>2.2.0</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
    </ItemGroup>
    ```

4. Usuń odwołania do `XamarinComponentReference` i Zapisz plik. W powyższym przykładzie jest bezpiecznie usunąć całą `ItemGroup`.

5. Po zapisaniu pliku, kliknij prawym przyciskiem myszy nazwę projektu i wybierz **Załaduj ponownie projekt**.

6. Powtórz powyższe kroki dla każdego projektu w rozwiązaniu.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Po załadowaniu projektu w programie Visual Studio dla komputerów Mac, zostanie wyświetlone następujące okno dialogowe z informacjami o tym, że należy usunąć wszystkie składniki z projektu ręcznie:

![Alert okna dialogowego wyjaśniający, czy składnik zostały znalezione w projekcie i muszą zostać usunięte](component-nuget-images/component-alert.png)

Aby usunąć składnik z projektu:

1. Otwórz pliku .csproj. Aby to zrobić, kliknij prawym przyciskiem myszy nazwę projektu i wybierz **Narzędzia > Edytuj plik**.

2. Znajdź wszystkie odwołania w pliku `XamarinComponentReference`. Powinien wyglądać podobnie do poniższego przykładu:

    ```xml
    <ItemGroup>
      <XamarinComponentReference Include="advancedcolorpicker">
        <Version>2.0.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="gunmetaltheme">
        <Version>1.4.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="signature-pad">
        <Version>2.2.0</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
    </ItemGroup>
    ```

3. Usuń odwołania do `XamarinComponentReference` i Zapisz plik. W powyższym przykładzie jest bezpiecznie usunąć całą `ItemGroup`

4. Powtórz powyższe kroki dla każdego projektu w rozwiązaniu.

-----

> [!WARNING]
> Poniższe instrukcje działają tylko ze starszymi wersjami programu Visual Studio.
> **Składniki** węzeł nie jest już dostępne w bieżącej wersji programu Visual Studio 2017 lub Visual Studio dla komputerów Mac.

W poniższych sekcjach opisano sposób aktualizacji w istniejących rozwiązaniach Xamarin zmienić składnika odwołania do pakietów NuGet.

- [Składniki, które zawierają pakietów NuGet](#contain)
- [Składniki z zamiany NuGet](#replace)

Większość składników należą do jednego z powyższych kategorii.
Jeśli używasz składnik, który nie ma odpowiednik pakietu NuGet, przeczytaj [składników, bez ścieżki migracji NuGet](#require-update) poniższej sekcji.

<a name="contain" />

## <a name="components-that-contain-nuget-packages"></a>Składniki, które zawierają pakietów NuGet

Wiele składników zawiera już pakiety NuGet, a ścieżka migracji jest po prostu Usuń odwołanie do składnika.

Aby ustalić, czy składnik już zawiera pakiet NuGet przez dwukrotne kliknięcie składnika w rozwiązaniu:

![Węzeł składników rozwinięty](component-nuget-images/solution-sml.png)

**Pakiety** kartę Wyświetla wszystkie zawarte w składniku pakiety NuGet:

![Karta pakietów zawiera NuGet](component-nuget-images/packages-tab-sml.png)

Należy pamiętać, że **zestawy** karta jest pusta:

![Karta zestawów jest pusta](component-nuget-images/assemblies-tab-empty-sml.png)

### <a name="updating-the-solution"></a>Aktualizowanie rozwiązania

Aby zaktualizować rozwiązania, należy usunąć **składnika** wpisu z rozwiązania:

![Usuwanie składnika](component-nuget-images/delete-component-sml.png)

Pakiet NuGet pozostanie na liście w **pakiety** węzeł i aplikacja będzie skompilować i uruchomić w zwykły sposób. W przyszłych aktualizacji do tego pakietu będą wykonywane za pośrednictwem **Nuget** funkcji aktualizacji:

![Pakiet NuGet aktualizacji](component-nuget-images/nuget-update-sml.png)


<a name="replace" />

## <a name="components-with-nuget-replacements"></a>Składniki z zamiany NuGet

Jeśli strona informacje o składnika **zestawy** karta zawiera wpisy, jak pokazano poniżej, konieczne będzie ręczne znaleźć równoważne pakietu NuGet.

![Zawiera zestawy](component-nuget-images/assemblies-tab-sml.png)

Należy pamiętać, że **pakiety** karta prawdopodobnie będzie pusta:

![](component-nuget-images/packages-tab-empty-sml.png)

_Może on zawierać zależności NuGet, ale można je zignorować._

Aby potwierdzić zastępczy istnieje pakiet NuGet, wyszukaj [NuGet.org](https://www.nuget.org/packages), przy użyciu nazwy składnika lub alternatywnie przez autora.

Na przykład można znaleźć popularnych **sqlite-net-pcl** pakietu, wyszukując:

- [`sqlite-net-pcl`](https://www.nuget.org/packages?q=sqlite-net-pcl) — Nazwa produktu.
- [`praeclarum`](https://www.nuget.org/packages?q=praeclarum) — profilu autora.

### <a name="updating-the-solution"></a>Aktualizowanie rozwiązania

Po potwierdzeniu, że składnik ten jest dostępny w NuGet, wykonaj następujące kroki:

#### <a name="delete-the-component"></a>Usuwanie składnika

Kliknij prawym przyciskiem myszy składnik w rozwiązaniu i wybierz polecenie **Usuń**:

![Usuwanie składnika](component-nuget-images/remove-component-sml.png)

Spowoduje to usunięcie składnik i wszystkie odwołania. Spowoduje to przerwanie kompilacji, do momentu dodania równoważne pakietu NuGet, aby go zastąpić.

#### <a name="add-the-nuget-package"></a>Dodaj pakiet NuGet

1. Kliknij prawym przyciskiem myszy **pakiety** węzeł i wybierz polecenie **Dodawanie pakietów...** .
2. Wyszukaj według nazwy lub autora zastąpienia NuGet:

  ![](component-nuget-images/nuget-search-sml.png)

3. Naciśnij klawisz **Dodaj pakiet**.

Pakiet NuGet zostaną dodane do projektu, oraz wszelkie zależności.
To powinno rozwiązać kompilacji. Jeśli kompilacja zakończy się niepowodzeniem, sprawdź każdy błąd, aby zobaczyć, czy wystąpiły interfejsu API różnice między działania składnika i pakietu NuGet.

<a name="require-update" />

## <a name="components-without-a-nuget-migration-path"></a>Składniki bez ścieżki migracji NuGet

Nie trzeba się troszczyć, jeśli nie znajdziesz natychmiast zastępuje składniki używane w aplikacji. Istniejące elementy nadal będą działać w programie Visual Studio 15,5 cala i **składniki** węzła pojawi się w rozwiązaniu w zwykły sposób.

Przyszłych wersjach programu Visual Studio, jednak będą _nie_ Przywróć lub zaktualizować składniki.
Oznacza to, po otwarciu rozwiązania na nowym komputerze składnik nie będzie można pobrane i zainstalowane; i nie będzie mogła dostarczać aktualizacji autora. Należy zaplanować:

* Wyodrębnij zestawy ze składnika i odwoływać je bezpośrednio w projekcie.
* Skontaktuj się z autorem składnika i poproś o planach migrację do NuGet.
* Zbadaj alternatywnych pakietów NuGet lub wyszukiwanie kodu źródłowego, jeżeli kondycja składnika jest open source.

Wielu dostawców składnika nadal jest wykonywane na temat migracji do NuGet i innych użytkowników (w tym dostępnych na rynku produktów) może badanie opcje dostawy.


## <a name="related-links"></a>Linki pokrewne
- [Lista popularnych wtyczek Xamarin i biblioteki](https://github.com/xamarin/XamarinComponents/blob/master/README.md)
- [Zainstalować i używać pakietu NuGet (system Windows)](https://docs.microsoft.com/nuget/quickstart/use-a-package)
- [W tym pakietu NuGet (Mac)](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)
