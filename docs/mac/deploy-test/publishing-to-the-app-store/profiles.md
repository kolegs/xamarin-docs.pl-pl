---
title: Profile aprowizacji
description: "Ten przewodnik przeprowadzi Cię przez proces tworzenia niezbędne profile inicjowania obsługi, które będą wymagane do publikowania aplikacji Xamarin.Mac."
ms.topic: article
ms.prod: xamarin
ms.assetid: bdff6c32-f7e3-4a97-a093-dbda48be8227
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/12/2017
ms.openlocfilehash: dab923f6150bdf005e9468add6d26d4fdb691a93
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="provisioning-profiles"></a>Profile aprowizacji

Profile inicjowania obsługi administracyjnej pozwalają dewelopera włączenie kilka funkcji określonych macOS (wcześniej znane jako systemu Mac OS X) (takie jak iCloud i powiadomień wypychanych) do swoich aplikacji Xamarin.Mac. Muszą one tworzenie, Pobierz i zainstaluj profil inicjowania obsługi komputerów Mac dla każdej aplikacji, które są one tworzenie korzystające z tych funkcji.

[![](profiles-images/certif13.png "Portalu inicjowania obsługi administracyjnej firmy Apple")](profiles-images/certif13.png#lightbox)

<a name="Development_Provisioning_Profile" />

## <a name="development-provisioning-profile"></a>Programowanie profilu inicjowania obsługi administracyjnej

Programowanie profil udostępniania umożliwia docelowe Mac App Store aplikacji ma zostać przetestowana na określonych komputerach, które zostały skonfigurowanych w profilu. Jest to szczególnie istotne w przypadku, gdy za pomocą funkcji macOS, takich jak iCloud i powiadomień wypychanych.

> [!NOTE]
> Deweloper musi już utworzono certyfikatu deweloperskiego Mac przed mogą utworzyć profil inicjowania obsługi programowania. Uzupełnij informacje szczegółowe, jak pokazano na tym zrzucie ekranu pokazano do generowania **profilu inicjowania obsługi administracyjnej programowanie** można utworzyć kompilacji. Musi istnieć certyfikat programowanie Mac dostępne do wyboru w **certyfikatu** pole i co najmniej jeden system, w zarejestrowany do testowania.

Wykonaj następujące czynności:

1. Wybierz typ inicjowania obsługi administracyjnej profil, który do tworzenia i kliknij przycisk **Kontynuuj** przycisk: 

     [![](profiles-images/certif14.png "Wybieranie typu profilu")](profiles-images/certif14.png#lightbox)
2. Wybierz identyfikator aplikacji, aby utworzyć profil dla, a następnie kliknij przycisk **Kontynuuj** przycisk: 

     [![](profiles-images/certif15.png "Wybieranie identyfikator aplikacji")](profiles-images/certif15.png#lightbox)
3. Wybierz projektanta identyfikator używany do podpisywania profilu i kliknij przycisk **Kontynuuj**: 

     [![](profiles-images/certif16.png "Wybieranie Identyfikator dewelopera")](profiles-images/certif16.png#lightbox)
4. Wybierz komputery, na których ten profil może być używany na i kliknij przycisk **Kontynuuj**: 

     [![](profiles-images/certif17.png "Wybieranie dozwolone komputery")](profiles-images/certif17.png#lightbox)
5. Teraz, wprowadź **nazwa profilu** i kliknij przycisk **Generuj** przycisk: 

     [![](profiles-images/certif18.png "Generowanie profilu")](profiles-images/certif18.png#lightbox)
6. Kliknij przycisk **Pobierz** przycisk, aby pobrać nowy profil: 

     [![](profiles-images/certif19.png "Pobieranie profilu")](profiles-images/certif19.png#lightbox)
7. Programowanie profile inicjowania obsługi administracyjnej są instalowane w okienku preferencji profile Mac **preferencjach systemowych** aplikacji: 

     [![](profiles-images/certif20.png "Instalowanie profilu")](profiles-images/certif20.png#lightbox)
8. W okienku preferencji profilu wyświetli wszystkie zainstalowane profile: 

     [![](profiles-images/image47.png "Profile przedstawiający zainstalowany")](profiles-images/image47.png#lightbox)
9. Profilu są również wyświetlane w **narzędzie certyfikat dewelopera** w przypadku, gdy musi zostać pobrana ponownie: 

     [![](profiles-images/image48.png "Narzędzia Developer do obsługi certyfikatów")](profiles-images/image48.png#lightbox)

Nowy profil inicjowania obsługi administracyjnej Programowanie będzie muszą zostać utworzone dla każdej nowej aplikacji lub do testowania na jest dodawany nowy komputer.

<a name="Production_Provisioning_Profile" />

## <a name="production-provisioning-profile"></a>Profil inicjowania obsługi administracyjnej produkcji

Profile inicjowania obsługi administracyjnej produkcji są wymagane do utworzenia pakietu do przesłania do sklepu Mac App Store.

Wykonaj następujące czynności:

1. Wybierz typ profilu, aby utworzyć i kliknij przycisk **Kontynuuj** przycisk: 

    [![](profiles-images/certif21.png "Wybierając typ profilu")](profiles-images/certif21.png#lightbox)
2. Wybierz identyfikator aplikacji, aby utworzyć profil dla, a następnie kliknij przycisk **Kontynuuj** przycisk: 

    [![](profiles-images/certif15.png "Wybieranie identyfikator aplikacji")](profiles-images/certif15.png#lightbox)
3. Wybierz identyfikator firmy podpisać profilu, a następnie kliknij przycisk **Kontynuuj** przycisk: 

    [![](profiles-images/certif23.png "Wybieranie identyfikator firmy")](profiles-images/certif23.png#lightbox)
4. Wprowadź **nazwa profilu** i kliknij przycisk **Generuj** przycisk: 

    [![](profiles-images/certif24.png "Generowanie profilu")](profiles-images/certif24.png#lightbox)
5. Kliknij przycisk **Pobierz** można pobrać pliku profilu inicjowania obsługi administracyjnej (rozszerzenie `.provisionprofile`): 

    [![](profiles-images/certif25.png "Pobieranie profilu")](profiles-images/certif25.png#lightbox)
6. Przeciągnij go do **organizatora Xcode** lub kliknij dwukrotnie, aby zainstalować. Profil zostanie następnie wyświetlona w środowisku Xcode organizatora: 

    [![](profiles-images/image51.png "Instalowanie profilu")](profiles-images/image51.png#lightbox)
7. Profil inicjowania obsługi administracyjnej pojawi się także na liście: 

    [![](profiles-images/certif26.png "Wyświetlanie zainstalowanych profilów")](profiles-images/certif26.png#lightbox)


Jeśli dewelopera zmieni się kiedykolwiek funkcje używane przez identyfikator aplikacji (np.) Włączanie usługi iCloud lub wypychania powiadomień), a następnie ponownie należy utworzyć profile udostępniania dla danego identyfikatora aplikacji.

## <a name="related-links"></a>Linki pokrewne

- [Instalacja](~//mac/get-started/installation.md)
- [Witaj, przykładowe Mac](~//mac/get-started/hello-mac.md)
- [Dystrybucję aplikacji ze sklepu Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [Przewodnik narzędzi: Podpisywania aplikacji kodu](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [Identyfikator deweloperów i strażnika](https://developer.apple.com/resources/developer-id/)
