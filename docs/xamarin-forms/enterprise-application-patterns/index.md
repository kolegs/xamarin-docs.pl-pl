---
title: Wzorce aplikacji dla przedsiębiorstw przy użyciu zestawu narzędzi Xamarin.Forms — Książka elektroniczna
description: W tej książce elektronicznej zawiera wskazówki dotyczące architektury do tworzenia potężnej, łatwego w utrzymaniu i testowaniu aplikacji Xamarin.Forms dla przedsiębiorstw.
ms.prod: xamarin
ms.assetid: 28cfed6c-6175-4223-a8cc-798d40bf0832
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: ecfe99f66e16eafabc3117036ff065e3a35259c3
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994351"
---
# <a name="enterprise-application-patterns-using-xamarinforms-ebook"></a>Wzorce aplikacji dla przedsiębiorstw przy użyciu zestawu narzędzi Xamarin.Forms — Książka elektroniczna

_Wskazówki dotyczące architektury do tworzenia potężnej, łatwego w utrzymaniu i testowaniu aplikacji Xamarin.Forms dla przedsiębiorstw_

![](images/cover-sml.png "Wzorce aplikacji dla przedsiębiorstw przy użyciu zestawu narzędzi Xamarin.Forms — Książka elektroniczna")

W tej książce elektronicznej znajdują się wskazówki dotyczące implementowania wzorca Model-View-ViewModel (MVVM), wstrzykiwanie zależności, nawigacji, weryfikacji i zarządzanie konfiguracją przy zachowaniu Poluzuj powiązania. Ponadto istnieje również wskazówki dotyczące przeprowadzania uwierzytelniania i autoryzacji za pomocą IdentityServer, uzyskiwanie dostępu do danych z konteneryzowane mikrousługi i testy jednostkowe.

## <a name="prefaceprefacemd"></a>[Wstęp](preface.md)

W tym rozdziale wyjaśniono cel i zakres przewodnika i który ma na celu.

## <a name="introductionintroductionmd"></a>[Wprowadzenie](introduction.md)

Deweloperzy aplikacji dla przedsiębiorstw stoją w obliczu kilka kwestii, które można zmieniać architektury aplikacji podczas programowania. Dlatego ważne jest do tworzenia aplikacji, tak aby można modyfikować i rozszerzone wraz z upływem czasu. Projektowanie pod kątem takich zdolności adaptacyjnych może być trudne, ale zwykle obejmuje partycjonowanie aplikacji na osobne, luźno powiązane składniki, które można łatwo zintegrować ze sobą w aplikacji.

## <a name="mvvmmvvmmd"></a>[MVVM](mvvm.md)

Wzorzec Model-View-ViewModel (MVVM) pomaga oddzielić nie pozostawia żadnych śladów logiki biznesowej i prezentacji aplikacji za pomocą jego interfejsu użytkownika (UI). Utrzymywanie czystą separacji między logiki aplikacji i interfejsu użytkownika pozwala rozwiązać wiele problemów rozwoju i może ułatwić aplikacji do testowania, obsługa i rozwijać. Może również znacznie zwiększyć możliwości ponownego użycia kodu i umożliwia deweloperom i projektanci interfejsu użytkownika, aby łatwo współpracować podczas tworzenia ich odpowiednich części aplikacji.

## <a name="dependency-injectiondependency-injectionmd"></a>[Wstrzykiwanie zależności](dependency-injection.md)

Wstrzykiwanie zależności umożliwia oddzielenie konkretne typy z kodu, który zależy od rodzaju. Zazwyczaj używa kontener, który zawiera listę rejestracji i mapowania między interfejsy i typy abstrakcyjne i konkretnych typów, które implementuje lub rozszerzyć pewne tych typów.

Kontenery iniekcji zależności zmniejszyć sprzężenia między obiektami, zapewniając możliwość utworzenia wystąpienia wystąpień klas i zarządzanie ich okres istnienia, na podstawie konfiguracji kontenera. Podczas tworzenia obiektów kontenera wprowadza wszelkie zależności, które wymaga obiektu do niego. Jeśli te zależności nie zostały jeszcze utworzone, kontener tworzy i jest rozpoznawana jako ich zależności.

## <a name="communicating-between-loosely-coupled-componentscommunicating-between-loosely-coupled-componentsmd"></a>[Komunikacja między luźno sprzężonymi składnikami](communicating-between-loosely-coupled-components.md)

Xamarin.Forms [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) klasa implementuje Publikuj — Subskrybuj wzorzec, dzięki czemu oparta na komunikatach komunikacji między składnikami, które są używane przez odwołania do obiektu i typu nie można użyć. Ten mechanizm pozwala wydawcy i subskrybenci do komunikowania się bez konieczności odwołanie do siebie, co pomogło w zmniejszeniu zależności między składnikami w zachowaniu składniki niezależne opracowany i przetestowany.

## <a name="navigationnavigationmd"></a>[Nawigacja](navigation.md)

Zestaw narzędzi Xamarin.Forms obsługuje nawigowania po stronach, co zwykle powoduje z interakcji użytkownika z interfejsu użytkownika lub aplikacji, w wyniku zmiany stanu na podstawie logiki wewnętrznej. Jednak nawigacji może być skomplikowane, aby zaimplementować w aplikacji, które używają wzorca MVVM.

W tym rozdziale przedstawiono `NavigationService` klasy, która jest używana do wykonywania widoku pierwszy model nawigacyjnych z modeli widoków. Umieszczenie logiki nawigacji w widoku klas modelu oznacza, że logika może być wykonywana za pomocą zautomatyzowanych testów. Ponadto model widoku można zaimplementować logikę do kontroli nawigacji, aby upewnić się, że niektóre reguły biznesowe są wymuszane.

## <a name="validationvalidationmd"></a>[Walidacja](validation.md)

Dowolną aplikację, która akceptuje dane wejściowe od użytkowników należy upewnić się, że dane wejściowe są prawidłowe. Bez sprawdzania poprawności użytkownik może podawać dane, które powoduje, że aplikacja nie powiedzie się. Sprawdzanie poprawności wymusza stosowanie reguł biznesowych i uniemożliwia osobie atakującej iniekcję złośliwych danych.

W kontekście modelu ViewModel Model (MVVM) wzorca model widoku lub modelu będzie często wymagane do wykonywania sprawdzania poprawności danych i sygnałów wszystkie błędy weryfikacji do widoku, dzięki czemu użytkownik może poprawić je.

## <a name="configuration-managementconfiguration-managementmd"></a>[Zarządzanie konfiguracją](configuration-management.md)

Ustawienia umożliwiają oddzielenie danych, który służy do konfigurowania zachowania aplikacji od kodu, umożliwiając zachowanie, aby go zmienić bez ponownie skompilować aplikację. Ustawienia aplikacji są dane, które tworzy i zarządza aplikacji i ustawienia użytkownika są dostosowywane ustawienia aplikacji, które wpływają na działanie aplikacji i nie wymagają częstego ponownego dopasowania.

## <a name="containerized-microservicescontainerized-microservicesmd"></a>[Konteneryzowane mikrousługi](containerized-microservices.md)

Mikrousługi oferują podejście do projektowania aplikacji i wdrożenia, który jest odpowiedni do wymagań elastyczność, skalowalność i niezawodność nowoczesnych aplikacji w chmurze. Jedną z głównych zalet mikrousług jest, aby można było wydajnego i skalowalnego niezależnie, co oznacza, że określonego obszaru funkcjonalnego może być skalowana, wymaga więcej przetwarzania zasilania lub sieci przepustowość do obsługi żądanie, bez niepotrzebnego skalowania obszarów Aplikacja, która nie występuje zwiększone zapotrzebowanie.

## <a name="authentication-and-authorizationauthentication-and-authorizationmd"></a>[Uwierzytelnianie i autoryzacja](authentication-and-authorization.md)

Dostępnych jest wiele metod do integracji uwierzytelniania i autoryzacji w aplikacji platformy Xamarin.Forms, który komunikuje się z aplikacji sieci web ASP.NET MVC. W tym miejscu uwierzytelnianie i autoryzacja są wykonywane przy użyciu mikrousług konteneryzowanych tożsamości, który używa IdentityServer 4. IdentityServer to architektura typu open source OpenID Connect i OAuth 2.0 dla platformy ASP.NET Core, która integruje się z tożsamości platformy ASP.NET Core w celu przeprowadzenia uwierzytelniania tokenu elementu nośnego.

## <a name="accessing-remote-dataaccessing-remote-datamd"></a>[Uzyskiwanie dostępu do danych zdalnych](accessing-remote-data.md)

Wiele nowoczesnych rozwiązań opartych na sieci web wprowadzić korzystanie z usług sieci web hostowanych na serwerach sieci web w celu zapewnienia funkcji zdalnego klienta aplikacji. Operacje, które uwidacznia Usługa sieci web stanowi interfejs API sieci web i aplikacje klienckie powinno być możliwe korzystanie z interfejsu API sieci web nie wiedząc o tym, jak zaimplementować danych lub operacji, które uwidacznia interfejs API.

## <a name="unit-testingunit-testingmd"></a>[Testowanie jednostek](unit-testing.md)

Testowanie modeli i modeli widoków z modelem MVVM aplikacji jest identyczne z testowaniem innych klas, a te same narzędzia i techniki mogą być używane. Jednak istnieją pewne wzorców, które są typowe dla modelu i widoku klasy modeli, które mogą skorzystać z technik testowania określonej jednostki.

## <a name="feedback"></a>Opinia

W tym projekcie są witryny społeczności, na którym można Publikuj pytania i przekazać opinię. Witryna społeczności znajduje się na [GitHub](https://github.com/dotnet-architecture/eShopOnContainers). Alternatywnie swoją opinię na temat książki elektronicznej mogą otrzymywać pocztą e-mail powiadomienia do [ dotnet-architecture-ebooks-feedback@service.microsoft.com ](mailto:dotnet-architecture-ebooks-feedback@service.microsoft.com).


## <a name="related-links"></a>Linki pokrewne

- [Pobierz książkę elektroniczną (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [ramach aplikacji eShopOnContainers (GitHub) (przykład)](https://github.com/dotnet-architecture/eShopOnContainers)
