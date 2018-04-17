---
title: Możliwości usługi iCloud
description: Dodawanie funkcji do aplikacji często wymaga dodatkowej konfiguracji inicjowania obsługi administracyjnej. W tym przewodniku opisano ustawienia wymagane dla funkcji w ramach usługi iCloud.
ms.prod: xamarin
ms.assetid: 3CBAC982-D8DE-48DD-97CD-32B551D9DB85
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: e426423854e7c569576c374ea1284c4de099a2d1
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/16/2018
---
# <a name="icloud-capabilities"></a>Możliwości usługi iCloud

_Dodawanie funkcji do aplikacji często wymaga dodatkowej konfiguracji inicjowania obsługi administracyjnej. W tym przewodniku opisano ustawienia wymagane dla funkcji w ramach usługi iCloud._

iCloud zapewnia użytkownicy systemu iOS wygodne i prosty sposób, aby przechowywać zawartość i udostępniać je między urządzeniami. Istnieją cztery metody deweloperzy mogą używać iCloud zapewnienie środków magazynu dla użytkowników: magazyn kluczy i wartości, magazynu UIDocument CoreData i przy użyciu CloudKit bezpośrednio w celu zapewnienia magazynu dla poszczególnych plików i katalogów. Aby uzyskać więcej informacji o tych dotyczą [wprowadzenie do usługi iCloud](~/ios/data-cloud/introduction-to-icloud.md) przewodnik.

Dodawanie w ramach usługi iCloud możliwość do aplikacji jest nieco trudniejsze niż innych aplikacji usługi z powodu _kontenery_. Kontenery są używane w ramach usługi iCloud do przechowywania informacji o aplikacji, a następnie poczekanie wszystkie informacje zawarte w rozdzielony — podobnie jak sandboxing na urządzeniu z systemem iOS użytkownika przy użyciu konta pojedynczego iCloud. Aby uzyskać więcej informacji na kontenery dotyczą [wprowadzenie do CloudKit](~/ios/data-cloud/intro-to-cloudkit.md) przewodnik.

> [!IMPORTANT]
> Apple [udostępnia narzędzia](https://developer.apple.com/support/allowing-users-to-manage-data/) aby pomóc deweloperom poprawnie obsługiwać interfejsów Unii Europejskiej ogólne dane ochrony rozporządzenia (GDPR).

<a name="icloud-developer-center" />

## <a name="developer-center"></a>Centrum deweloperów

Podczas inicjowania obsługi administracyjnej nowej aplikacji za pośrednictwem Centrum deweloperów istnieją dwa kroki, które należy podjąć:

1.  Tworzenie kontenera.
2.  Utworzyć identyfikator aplikacji z możliwością iCloud i Dodaj do niej kontener.
3. Utwórz profil inicjowania obsługi administracyjnej, który zawiera ten identyfikator aplikacji

Poniższe kroki przedstawiono następujące kroki:

1.  Przejdź do [Centrum deweloperów firmy Apple](https://developer.apple.com/account/) i przejdź do sekcji certyfikatów, identyfikator i profili: 
    
     ![Strona główna Centrum deweloperów firmy Apple](icloud-capabilities-images/image22.png)

2.  W obszarze **identyfikatory** wybierz **iCloud kontenery**, a następnie wybierz **+** Aby utworzyć nowy kontener:  
    
    ![ekran kontenera usługi iCloud](icloud-capabilities-images/image23.png)

3.  Wprowadź **opis** oraz unikatową **identyfikator** dla kontenera, w ramach usługi iCloud: 
    
    ![ekran rejestracji kontenera usługi iCloud](icloud-capabilities-images/image24.png)

4.  Naciśnij klawisz **Kontynuuj**, upewnij się, że informacje są poprawne i naciśnij klawisz **zarejestrować** można utworzyć w ramach usługi iCloud kontenera:  
    
    ![ekran rejestracji kontenera usługi iCloud](icloud-capabilities-images/image25.png)

Aby utworzyć nowy identyfikator aplikacji i Dodaj do niej kontener, wykonaj następujące czynności:

1.  W [Centrum deweloperów](https://developer.apple.com/account/), kliknij **identyfikatorów aplikacji** w obszarze **identyfikatory**: 
    
    ![Identyfikator sekcji w Centrum deweloperów](icloud-capabilities-images/image26.png)

2.  Wybierz **+** przycisk, aby dodać nowy identyfikator aplikacji: 
    
    ![Dodaj nowy przycisk Identyfikator aplikacji](icloud-capabilities-images/image27.png)

3.  Wprowadź **nazwa** dla Identyfikatora aplikacji i nadaj mu **jawny identyfikator aplikacji**:
    
    ![Podaj nowe szczegóły Identyfikatora aplikacji](icloud-capabilities-images/image28.png)

4.  W obszarze **usługi aplikacji** wybierz **iCloud** i wybierz polecenie **CloudKit obejmują obsługę**:
    
    ![Wybierz usługi aplikacji usługi iCloud](icloud-capabilities-images/image29.png)

5.  Wybierz **kontynuować** , a następnie **zarejestrować**. Należy pamiętać, że na ekranie potwierdzenia, iCloud wyświetli przy konfigurowalne zaznaczone, przy użyciu symbolu żółty:   
    
    ![Ekran potwierdzenia](icloud-capabilities-images/image30.png)

6.  Wróć do listy identyfikatorów aplikacji i wybierz jedną, która została właśnie utworzona: 
    
    ![Wybierz identyfikator aplikacji ekranu](icloud-capabilities-images/image31.png)

7.  Przewiń w dół w tej sekcji rozwinięte, a następnie kliknij przycisk **Edytuj**:
    
    ![Edytuj identyfikator aplikacji](icloud-capabilities-images/image32.png)

8.  Przewiń w dół listy do usługi iCloud i kliknij przycisk **Edytuj** przycisk:  
    
    ![Edytuj iCloud identyfikator aplikacji](icloud-capabilities-images/image33.png)

9.  Wybierz kontener do użycia z tym Identyfikatorem aplikacji:  
    
    ![Wybierz kontener ekranu](icloud-capabilities-images/image34.png)

10. Potwierdź kontenera przypisania, a następnie naciśnij klawisz **przypisać**.
 
Ten identyfikator aplikacji można teraz używać wygenerować lub ponownie wygenerować nowy profil inicjowania obsługi administracyjnej, zgodnie z opisem w [Praca z funkcjami](~/ios/deploy-test/provisioning/capabilities/index.md) przewodnik. 

Aby uzyskać więcej informacji na temat używania usługi iCloud można znaleźć w następujących przewodnikach:

*   [Wprowadzenie do usługi iCloud](~/ios/data-cloud/introduction-to-icloud.md)
*   [Wprowadzenie do CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)
*   [Wprowadzenie do dokumentu selektora](~/ios/platform/document-picker.md)

## <a name="next-steps"></a>Następne kroki
 
Poniższa lista zawiera dodatkowe kroki, które muszą być podjęte:

* Użyj nazw framework w aplikacji.
* Dodawanie uprawnień wymaganych do aplikacji. Informacje dotyczące uprawnień wymaganych i sposobu dodawania ich została szczegółowo opisana w [Praca z uprawnieniami](~/ios/deploy-test/provisioning/entitlements.md) przewodnik.
* W aplikacji **iOS podpisywania pakietu**, upewnij się, że **uprawnień niestandardowych** ustawiono **Entitlements.plist**. Jest to _nie_ tworzy domyślne ustawienie dla debugowania i symulatora systemu iOS.

Jeśli wystąpią problemy związane z usługi aplikacji, zapoznaj się [Rozwiązywanie problemów](~/ios/deploy-test/provisioning/capabilities/index.md) głównym przewodniku.