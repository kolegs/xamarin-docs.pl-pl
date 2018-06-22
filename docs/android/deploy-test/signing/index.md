---
title: Podpisywanie pakietu aplikacji systemu Android
ms.prod: xamarin
ms.assetid: 8E3EFBB2-F8AD-C126-5F32-7FD140791E53
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/21/2018
ms.openlocfilehash: 6a4164ea4a56ee7c1b3c1abd05f7b1bb95aede4f
ms.sourcegitcommit: 9f8e7393019791bbd6af4fefaa24a1602adabb4e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/23/2018
ms.locfileid: "34458804"
---
# <a name="signing-the-android-application-package"></a>Podpisywanie pakietu aplikacji systemu Android

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

W tej sekcji opisano zintegrowane publikowania przepływu pracy do podpisania APK dostarczane przez program Visual Studio. W [przygotowania aplikacji do wersji](~/android/deploy-test/release-prep/index.md) **Menedżera archiwum** został użyty do tworzenia aplikacji i umieszczanie jej w archiwum do podpisywania i publikowania. W tej sekcji wyjaśniono, jak tworzenie Android tożsamość podpisywania, Utwórz nowy certyfikat podpisywania dla aplikacji systemu Android i publikowanie aplikacji zarchiwizowane *ad hoc* na dysku.
Wynikowa APK mogą być ładowane bezpośrednio do urządzenia z systemem Android bez pośrednictwa sklepu z aplikacjami.

W [archiwum publikowania](~/android/deploy-test/release-prep/index.md#archive), **kanału dystrybucji** okna dialogowego prezentowane dwa wybory dla dystrybucji. Wybierz **Ad Hoc**:

[![Okno dialogowe kanału dystrybucji](images/vs/01-distribution-channel-sml.png)](images/vs/01-distribution-channel.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

W tej sekcji użyjemy programu Visual Studio for Mac firmy zintegrowanego publikowania przepływu pracy do podpisania plik APK. W [przygotowania aplikacji do wersji](~/android/deploy-test/release-prep/index.md), użyliśmy **Menedżera archiwum** do tworzenia aplikacji i umieszczanie jej w archiwum do podpisywania i publikowania. W tej sekcji omówiono firma Microsoft sposobu tworzenia Android tożsamość podpisywania, Utwórz nowy certyfikat podpisywania dla aplikacji systemu Android i publikowanie aplikacji zarchiwizowane *ad hoc* na dysku. Wynikowa APK mogą być ładowane bezpośrednio do urządzenia z systemem Android bez pośrednictwa sklepu z aplikacjami.

W [archiwum publikowania](~/android/deploy-test/release-prep/index.md#archive), **utworzyć i dystrybuować...**  okna dialogowego prezentowane nam dwie opcje do dystrybucji. Wybierz **Ad Hoc** i kliknij przycisk **dalej**:

[![Okno dialogowe logowania i dystrybucji](images/xs/01-select-ad-hoc-sml.png)](images/xs/01-select-ad-hoc.png#lightbox)

-----

<a name="newcertvs" />
<a name="newcert" />
<a name="newcertxs" />

## <a name="create-a-new-certificate"></a>Utwórz nowy certyfikat

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Po **Ad Hoc** jest wybrany, Visual Studio zostanie otwarte **tożsamość podpisywania** strony okna dialogowego, jak pokazano w następnym zrzut ekranu. Aby opublikować. APK, najpierw muszą być podpisane przy użyciu klucza podpisywania (nazywane również certyfikatu).

Istniejący certyfikat mogą być używane przez kliknięcie przycisku **importu** przycisk, a następnie kontynuowanie [podpisać plik APK](#signapkvs). W przeciwnym razie kliknij przycisk kliknięcie **+** przycisk, aby utworzyć nowy certyfikat:

[![Ad Hoc tożsamości podpisywania](images/vs/02-ad-hoc-signing-identity-vs-sml.png)](images/vs/02-ad-hoc-signing-identity-vs.png#lightbox)

**Magazynu kluczy systemu Android Create** zostanie wyświetlone okno dialogowe; Użyj tego okna dialogowego, aby utworzyć nowy certyfikat podpisywania, które mogą być używane do podpisywania aplikacji systemu Android. Wprowadź wymagane informacje (opisane na czerwono), jak pokazano w tym oknie dialogowym:

[![Tworzenie okna dialogowego magazynu kluczy systemu Android](images/vs/03-create-android-key-store-vs-sml.png)](images/vs/03-create-android-key-store-vs.png#lightbox)

Poniższy przykład przedstawia rodzaju informacje, które należy podać. Kliknij przycisk **Utwórz** można utworzyć nowego certyfikatu:

[![Tworzenie nowego certyfikatu](images/vs/04-key-store-example-vs-sml.png)](images/vs/04-key-store-example-vs.png#lightbox)

Wynikowa magazynu kluczy znajduje się w następującej lokalizacji:

**C:\\użytkowników\\*USERNAME*\\AppData\\lokalnego\\Xamarin\\Mono dla systemu Android\\alias\\alias.keystore**

Na przykład powyższe kroki może utworzyć nowego klucza podpisywania w następującej lokalizacji:

**C:\\użytkowników\\*USERNAME*\\AppData\\lokalnego\\Xamarin\\Mono dla systemu Android\\chimp\\chimp.keystore**

> [!NOTE]
> Pamiętaj utworzyć kopię zapasową wynikowego pliku magazynu kluczy w bezpiecznym miejscu &ndash; nie jest zawarty w rozwiązaniu. W przypadku utraty Twojego magazynu kluczy (na przykład pliku, ponieważ plik został przeniesiony na inny komputer, lub ponownej instalacji systemu Windows), będą mogli się zalogować aplikacji za pomocą tego samego certyfikatu, ponieważ poprzednie wersje.

Aby uzyskać więcej informacji na temat magazynu kluczy, zobacz [znajdowanie MD5 lub SHA1 podpisu Twojego magazynu kluczy](~/android/deploy-test/signing/keystore-signature.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Po kliknięciu przycisku **Ad Hoc**, Visual Studio for Mac otwiera **Android tożsamość podpisywania** okna dialogowego, jak pokazano w następnym zrzut ekranu. Aby opublikować. APK, musi on zostać podpisany go przy użyciu klucza podpisywania (nazywane również certyfikatu). Jeśli certyfikat już istnieje, kliknij przycisk **zaimportować istniejący klucz** przycisk, aby go zaimportować, a następnie przejdź do [podpisać plik APK](#signapkxs) w przeciwnym razie kliknij przycisk **Utwórz nowy klucz** przycisk, aby Utwórz nowy certyfikat: 

[![Android tożsamość podpisywania okna dialogowego](images/xs/02-android-signing-identity-sml.png)](images/xs/02-android-signing-identity.png#lightbox)

**Tworzenie nowego świadectwa** okno dialogowe służy do tworzenia nowego certyfikatu podpisywania, który może służyć do podpisywania aplikacji systemu Android. Kliknij przycisk **OK** po wprowadzeniu w niezbędnych informacji:

[![Utwórz nowy certyfikat w oknie dialogowym](images/xs/03-create-new-certificate-sml.png)](images/xs/03-create-new-certificate.png#lightbox)

Wynikowa magazynu kluczy znajduje się w następującej lokalizacji:

**~/Library/Developer/Xamarin/Keystore/alias/alias.keystore**

Na przykład powyższe kroki może utworzyć nowego klucza podpisywania w następującej lokalizacji:

**~/Library/Developer/Xamarin/Keystore/chimp/chimp.keystore**


> [!NOTE]
> Pamiętaj utworzyć kopię zapasową wynikowego pliku magazynu kluczy w bezpiecznym miejscu &ndash; nie jest zawarty w rozwiązaniu. W przypadku utraty Twojego magazynu kluczy (na przykład pliku, ponieważ plik zostanie przeniesiony na inny komputer lub ponownej instalacji komputera Mac), będą mogli się zalogować aplikacji za pomocą tego samego certyfikatu, ponieważ poprzednie wersje.

Aby uzyskać więcej informacji na temat magazynu kluczy, zobacz [znajdowanie MD5 lub SHA1 podpisu Twojego magazynu kluczy](~/android/deploy-test/signing/keystore-signature.md).

-----

<a name="signapkvs" />

## <a name="sign-the-apk"></a>Podpisać plik APK

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Gdy **Utwórz** kliknięciu klucza magazyn (nowy certyfikat zawierający) zostanie zapisany i kategorii **tożsamość podpisywania** jak pokazano w następnym zrzut ekranu. Aby opublikować aplikację w witrynie Google Play, kliknij przycisk **anulować** i przejdź do [publikowania do witryny Google Play](~/android/deploy-test/publishing/publishing-to-google-play/index.md).
Aby opublikować *ad hoc*, wybierz tożsamości podpisywania dla podpisywania i kliknij przycisk **Zapisz jako** do publikowania aplikacji niezależnie od dystrybucji. Na przykład **chimp** podpisywania tożsamości (wcześniej utworzone) wybrano tego zrzutu ekranu:

[![Przykład tożsamości podpisywania](images/vs/05-save-as-vs-sml.png)](images/vs/05-save-as-vs.png#lightbox)

Następnie **Menedżera archiwum** Wyświetla postęp publikowania. Po zakończeniu procesu publikowania, **Zapisz jako** poprosić o lokalizacji zostanie otwarte okno dialogowe gdzie wygenerowany. Plik APK ma być przechowywany:

[![Okno dialogowe Zapisz jako](images/vs/06-save-as-dialog-vs-sml.png)](images/vs/06-save-as-dialog-vs.png#lightbox)

Przejdź do odpowiedniej lokalizacji, a następnie kliknij przycisk **zapisać**. Jeśli hasło klucza jest nieznany, **podpisywania hasła** wyświetli się okno dialogowe z monitem o hasło dla wybranego certyfikatu:

[![Podpisywanie hasło w oknie dialogowym](images/vs/07-signing-password-vs-sml.png)](images/vs/07-signing-password-vs.png#lightbox)

Po zakończeniu procesu podpisywania, kliknij przycisk **dystrybucji Otwórz**:

[![Przycisk Otwórz dystrybucji](images/vs/08-open-distribution-sml.png)](images/vs/08-open-distribution.png#lightbox)

Powoduje to, że program Eksplorator Windows otwórz folder zawierający wygenerowanego pliku APK. W tym momencie program Visual Studio został skompilowany aplikacji platformy Xamarin.Android APK, który jest gotowy do dystrybucji.
Poniższy zrzut ekranu przedstawia przykład gotowe do publikowania aplikacji, **MyApp.MyApp.apk**:

[![APK wyświetlany w Eksploratorze Windows](images/vs/09-generated-app-vs-sml.png)](images/vs/09-generated-app-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


Jak wspomniano w tym miejscu nowy certyfikat został dodany do magazynu kluczy. Aby opublikować aplikację w witrynie Google Play, kliknij przycisk **anulować** i przejdź do [publikowania do witryny Google Play](~/android/deploy-test/publishing/publishing-to-google-play/index.md).
W przeciwnym razie kliknij przycisk **dalej** do publikowania aplikacji *ad hoc* (dla niezależnie od dystrybucji) jak w poniższym przykładzie:

[![Okno dialogowe logowania i dystrybucji](images/xs/04-select-identity-sml.png)](images/xs/04-select-identity.png#lightbox)

**Publikuj jako Ad Hoc** okno dialogowe zawiera podsumowanie podpisaną aplikację przed opublikowaniem. Jeśli te informacje są poprawne, kliknij przycisk **publikowania**.

[![Publikuj jako okna dialogowego Ad Hoc](images/xs/05-publish-ad-hoc-sml.png)](images/xs/05-publish-ad-hoc.png#lightbox)

**Pliku APK dane wyjściowe** okna dialogowego zapisze plik APK na określonej ścieżce. Kliknij przycisk **zapisać**.

![Okno dialogowe pliku APK danych wyjściowych](images/xs/06-output-apk-file.png)

Następnie wprowadź hasło dla certyfikatu (hasło, którego użyto w **Tworzenie nowego świadectwa** okna dialogowego) i kliknij przycisk **OK**: 

![Wprowadź hasło certyfikatu](images/xs/07-signing-certificate.png)

Plik APK jest podpisany przy użyciu certyfikatu i zapisany w określonej lokalizacji. Kliknij przycisk **ujawnić w wyszukiwarce**:

[![Publikowanie zakończyło się pomyślnie w oknie dialogowym](images/xs/08-app-is-ready-sml.png)](images/xs/08-app-is-ready.png#lightbox)

Spowoduje to otwarcie wyszukiwania do lokalizacji podpisany plik APK:

[![APK pokazano wyszukiwania](images/xs/09-show-in-finder-sml.png)](images/xs/09-show-in-finder.png#lightbox)

Plik APK jest gotowy do skopiowania finder i wysłać do ostatecznego miejsca przeznaczenia. Należy dobrze Zainstaluj plik APK na urządzeniu z systemem Android i spróbować limit przed dystrybucją. Zobacz [niezależnie publikowania](~/android/deploy-test/publishing/publishing-independently.md) więcej informacji dotyczących publikowania *ad hoc* APK.

-----



## <a name="next-steps"></a>Następne kroki

Po pakiet aplikacji został podpisany w wersji, musi zostać opublikowany. W poniższych sekcjach opisano kilka sposobów, aby opublikować aplikację.
