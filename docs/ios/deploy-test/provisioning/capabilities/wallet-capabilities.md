---
title: "Możliwości portfela"
description: "Dodawanie funkcji do aplikacji często wymaga dodatkowej konfiguracji inicjowania obsługi administracyjnej. W tym przewodniku opisano ustawienia wymagane dla funkcji portfela."
ms.topic: article
ms.prod: xamarin
ms.assetid: BD9475E6-F586-488C-93D4-8A2A1629B99B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: 952fa7dfa93c1dcb45eafe4eec08c73d2a8571eb
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="wallet-capabilities"></a>Możliwości portfela

_Dodawanie funkcji do aplikacji często wymaga dodatkowej konfiguracji inicjowania obsługi administracyjnej. W tym przewodniku opisano ustawienia wymagane dla funkcji portfela._

Portfela to aplikacja, która przechowuje i wyświetla kodów kreskowych i innej zawartości, dzięki czemu użytkownicy mogą wyświetlać biletów, wstępu i kupony bezpośrednio z urządzenia. Te informacje są przechowywane na _przekazać_. Na przykład wsiadania przebiegu lub pojedynczy biletu byłoby pojedynczej przebiegu. 

Deweloperzy mogą korzystać z portfela na różne sposoby:

*   Aby utworzyć przebiegu, aplikacja nie musi ma zostać utworzony. Passfile jest archiwum zip, który zawiera niektóre pliki w formacie JSON i pliki opcjonalne metadane. Aby przygotować, [przekazać identyfikator typu](~/ios/platform/passkit.md) i [Przekaż certyfikat](~/ios/platform/passkit.md) jest wymagana. Te informacje jest następnie zadeklarowany w pliku JSON. Więcej informacji na temat udostępniania Passfile znajdują się w [wprowadzenie do PassKit](~/ios/platform/passkit.md) przewodnik.

*   Pomocnik aplikacji są zapisywane do dystrybucji przebiegów. Mają one również funkcjonalność do tworzenia, edytowania i przebiegów aktualizacji, a następnie dodać je do aplikacji portfela. Dobrym przykładem tego rodzaju aplikacji będą aplikacja kinowych — po użytkownik zakupi biletu za pośrednictwem aplikacji, że bilet można dodać bezpośrednio z aplikacji do portfela. Aby korzystać z aplikacji pomocnika, profilu inicjowania obsługi administracyjnej musi zawierać identyfikator aplikacji przy użyciu funkcji portfela, które można ustawić, wykonując poniższe kroki. Aplikacja musi także obejmować wymaganych uprawnień.

*   Kanał aplikacje są aplikacje, które nie manipulowania przekazuje bezpośrednio. Mają one minimalnej interakcji z przebiegu poza odebrania go i daje możliwość dodawania ich do portfela w trybie użytkownika. Te aplikacje nie są specjalne udostępniania lub uprawnień, ale będzie używany w przypadku niektórych metod, w ramach PassKit.

## <a name="developer-center"></a>Centrum deweloperów

Aby utworzyć nowy profil inicjowania obsługi administracyjnej do użytku z portfela, wykonaj następujące czynności:

1.  Przejdź do [certyfikaty identyfikatory i profile](https://developer.apple.com/account/ios/certificate/) części portalu dla deweloperów firmy Apple.
2.  W obszarze **identyfikatory**, przejdź do **identyfikatorów aplikacji**: 
    
    ![Wybór identyfikator aplikacji](wallet-capabilities-images/image17.png)

3.  Kliknij przycisk  **+**  ikonę w górnym rogu strony.
4.  Zarejestruj nowy identyfikator aplikacji przez nadanie mu **nazwa** i identyfikatora pakietu. (Należy pamiętać, że ten identyfikator pakietu musi być zgodna z Identyfikatorem pakietu w projekcie):
   
    ![Dodaj szczegóły Identyfikatora aplikacji](wallet-capabilities-images/image18.png)

5.  Wybierz **portfela** usługi aplikacji — z listy usług:
    
    ![Wybierz usługę ekranu](wallet-capabilities-images/image19.png)

6.  Naciśnij klawisz **Kontynuuj**, a następnie **zarejestrować** utworzyć identyfikator aplikacji.

W razie potrzeby można dodać możliwości portfela można edytować istniejące identyfikatorów aplikacji.

Ten identyfikator aplikacji można teraz używać wygenerować lub ponownie wygenerować nowy profil inicjowania obsługi administracyjnej, zgodnie z opisem w [Praca z funkcjami](~/ios/deploy-test/provisioning/capabilities/index.md) przewodnika:

![Przy użyciu nowo utworzony identyfikator aplikacji, aby utworzyć profil inicjowania obsługi administracyjnej](wallet-capabilities-images/image20.png)


Aby uzyskać więcej informacji na temat używania portfela można znaleźć w następujących przewodnikach:

*   [Wprowadzenie do PassKit](~/ios/platform/passkit.md)
 
## <a name="next-steps"></a>Następne kroki
 
Poniższa lista zawiera dodatkowe kroki, które muszą być podjęte:

* Użyj nazw framework w aplikacji.
* Dodawanie uprawnień wymaganych do aplikacji. Informacje dotyczące uprawnień wymaganych i sposobu dodawania ich została szczegółowo opisana w [Praca z uprawnieniami](~/ios/deploy-test/provisioning/entitlements.md) przewodnik.
* W aplikacji **iOS podpisywania pakietu**, upewnij się, że **uprawnień niestandardowych** ustawiono **Entitlements.plist**. Jest to _nie_ tworzy domyślne ustawienie dla debugowania i symulatora systemu iOS.

Jeśli wystąpią problemy związane z usługi aplikacji, zapoznaj się [Rozwiązywanie problemów](~/ios/deploy-test/provisioning/capabilities/index.md) głównym przewodniku.