---
title: Podpisywanie pakietu aplikacji dla systemu Android
description: Jak utworzyć pakiet aplikacji dla systemu Android (APK) publikacji
ms.prod: xamarin
ms.assetid: 8E3EFBB2-F8AD-C126-5F32-7FD140791E53
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/02/2018
ms.openlocfilehash: 4afcf42750cd9366bfd9fa5855fe1e7c0f114162
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403315"
---
# <a name="signing-the-android-application-package"></a>Podpisywanie pakietu aplikacji dla systemu Android

W [przygotowanie aplikacji do wydania](~/android/deploy-test/release-prep/index.md) **Menedżer archiwów** został użyty do tworzenia aplikacji i umieszczania jej w archiwum dotyczące podpisywania i publikowania. W tej sekcji wyjaśniono, jak utworzyć tożsamość do podpisywania systemu Android, Utwórz nowy certyfikat podpisywania dla aplikacji systemu Android i publikowanie aplikacji zarchiwizowane *ad hoc* na dysku. Wynikowy plik APK mogą być pobierane lokalnie na urządzeniach z systemem Android bez konieczności zwracania się za pośrednictwem sklepu z aplikacjami.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

W [archiwum dla publikowania](~/android/deploy-test/release-prep/index.md#archive), **kanał dystrybucji** okna dialogowego wyświetlone dwa wybory dla dystrybucji. Wybierz **Ad-Hoc**:

[![Okno dialogowe kanał dystrybucji](images/vs/01-distribution-channel-sml.png)](images/vs/01-distribution-channel.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

W [archiwum dla publikowania](~/android/deploy-test/release-prep/index.md#archive), **podpisywanie i dystrybucja...**  okna dialogowego napotkaliśmy dwa wybory dla dystrybucji. Wybierz **Ad-Hoc** i kliknij przycisk **dalej**:

[![Podpisywanie i dystrybucja okna dialogowego](images/xs/01-select-ad-hoc-sml.png)](images/xs/01-select-ad-hoc.png#lightbox)

-----

<a name="newcertvs" />
<a name="newcert" />
<a name="newcertxs" />

## <a name="create-a-new-certificate"></a>Utwórz nowy certyfikat

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Po **Ad-Hoc** jest zaznaczone, Visual Studio zostanie otwarta **tożsamość podpisywania** stronie okna dialogowego, jak pokazano w następnym zrzucie ekranu. Aby opublikować. Plik APK, najpierw musi być podpisany przy użyciu klucza podpisywania (nazywane również certyfikatu).

Istniejący certyfikat może służyć przez kliknięcie przycisku **importu** przycisk, a następnie kontynuowanie [podpisać zestawu APK](#signapkvs). W przeciwnym razie kliknij przycisk kliknięcie **+** przycisk, aby utworzyć nowy certyfikat:

[![Ad Hoc tożsamości podpisywania](images/vs/02-ad-hoc-signing-identity-vs-sml.png)](images/vs/02-ad-hoc-signing-identity-vs.png#lightbox)

**Utwórz Android klucz Store** zostanie wyświetlone okno dialogowe; Użyj tego okna dialogowego, aby utworzyć nowy certyfikat podpisywania, który służy do podpisywania aplikacji systemu Android. Wprowadź wymagane informacje (opisany na czerwono), jak pokazano w tym oknie dialogowym:

[![Tworzenie okna dialogowego Store klucz systemu Android](images/vs/03-create-android-key-store-vs-sml.png)](images/vs/03-create-android-key-store-vs.png#lightbox)

Poniższy przykład ilustruje rodzaju informacje, które musi zostać podana. Kliknij przycisk **Utwórz** do utworzenia nowego certyfikatu:

[![Tworzenie nowego certyfikatu](images/vs/04-key-store-example-vs-sml.png)](images/vs/04-key-store-example-vs.png#lightbox)

Wynikowy magazynu kluczy znajduje się w następującej lokalizacji:

**C:\\użytkowników\\*USERNAME*\\AppData\\lokalnego\\Xamarin\\Mono dla systemu Android\\magazynu kluczy\\ *ALIAS*\\*ALIAS*pomocą magazynu kluczy**

Na przykład za pomocą **chimp** jako alias, powyższe kroki mogą utworzyć nowy klucz podpisywania w następującej lokalizacji:

**C:\\użytkowników\\*USERNAME*\\AppData\\lokalnego\\Xamarin\\Mono dla systemu Android\\magazynu kluczy\\chimp\\chimp.keystore**

> [!NOTE]
> Pamiętaj utworzyć kopię zapasową wynikowego pliku magazynu kluczy i hasła w bezpiecznym miejscu &ndash; nie znajduje się w rozwiązaniu. W przypadku utraty magazyn kluczy w pliku (na przykład, ponieważ zostanie przeniesiony do innego komputera lub ponownej instalacji Windows), można do podpisywania aplikacji przy użyciu tego samego certyfikatu w poprzednich wersjach.

Aby uzyskać więcej informacji na temat magazynu kluczy, zobacz [znajdowaniu Twojego magazynu kluczy MD5 lub SHA1 podpisu](~/android/deploy-test/signing/keystore-signature.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Po kliknięciu przycisku **Ad-Hoc**, program Visual Studio for Mac zostanie otwarta **tożsamość podpisywania systemu Android** okna dialogowego, jak pokazano w następnym zrzucie ekranu. Aby opublikować. Plik APK, należy najpierw go on podpisany przy użyciu klucza podpisywania (nazywane również certyfikatu). Jeśli certyfikat już istnieje, kliknij przycisk **Importuj istniejący klucz** przycisk, aby go zaimportować, a następnie przejść do [podpisać zestawu APK](#signapkxs) w przeciwnym razie kliknij przycisk **Utwórz nowy klucz** przycisk, aby Utwórz nowy certyfikat: 

[![Android dialogowe tożsamości podpisywania](images/xs/02-android-signing-identity-sml.png)](images/xs/02-android-signing-identity.png#lightbox)

**Utwórz nowy certyfikat** okno dialogowe służy do tworzenia nowego certyfikatu podpisywania, który może służyć do podpisywania aplikacji systemu Android. Kliknij przycisk **OK** po wprowadzeniu niezbędnych informacji:

[![Tworzenie okna dialogowego Nowy certyfikat](images/xs/03-create-new-certificate-sml.png)](images/xs/03-create-new-certificate.png#lightbox)

Wynikowy magazynu kluczy znajduje się w następującej lokalizacji:

**~/Library/Developer/Xamarin/Keystore/alias/alias.keystore**

Powyższe kroki mogą na przykład utworzyć nowy klucz podpisywania w następującej lokalizacji:

**~/Library/Developer/Xamarin/Keystore/chimp/chimp.keystore**


> [!NOTE]
> Pamiętaj utworzyć kopię zapasową wynikowego pliku magazynu kluczy i hasła w bezpiecznym miejscu &ndash; nie znajduje się w rozwiązaniu. W przypadku utraty magazyn kluczy w pliku (na przykład, ponieważ zostanie przeniesiony do innego komputera lub ponowna instalacja systemu macOS), można do podpisywania aplikacji przy użyciu tego samego certyfikatu w poprzednich wersjach.

Aby uzyskać więcej informacji na temat magazynu kluczy, zobacz [znajdowaniu Twojego magazynu kluczy MD5 lub SHA1 podpisu](~/android/deploy-test/signing/keystore-signature.md).

-----

<a name="signapkvs" />

## <a name="sign-the-apk"></a>Zaloguj się zestawu APK

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Gdy **Utwórz** zostanie kliknięty nowy klucz magazynu (zawierający nowego certyfikatu) zostanie zapisany i wyświetlane w obszarze **tożsamość podpisywania** pokazane na następnym zrzucie ekranu. Aby opublikować aplikację w sklepie Google Play, kliknij przycisk **anulować** i przejdź do [publikowanie w sklepie Google Play](~/android/deploy-test/publishing/publishing-to-google-play/index.md).
Aby opublikować *ad-hoc*, wybierz tożsamości podpisywania do użycia podczas podpisywania, a następnie kliknij przycisk **Zapisz jako** Aby opublikować aplikację do dystrybucji niezależne. Na przykład **chimp** (utworzone wcześniej) tożsamość do podpisywania jest wybrany na tym zrzucie ekranu:

[![Przykład tożsamości podpisywania](images/vs/05-save-as-vs-sml.png)](images/vs/05-save-as-vs.png#lightbox)

Następnie **Menedżer archiwów** Wyświetla postęp publikowania. Po ukończeniu procesu publikowania **Zapisz jako** zostanie otwarte okno dialogowe, aby poprosić o lokalizację gdzie wygenerowany. Plik APK ma być przechowywany:

[![Okno dialogowe Zapisz jako](images/vs/06-save-as-dialog-vs-sml.png)](images/vs/06-save-as-dialog-vs.png#lightbox)

Przejdź do odpowiedniej lokalizacji, a następnie kliknij przycisk **Zapisz**. Jeśli hasło klucza jest nieznany, **hasło podpisywania** okno dialogowe pojawi się monit o podanie hasła dla wybranego certyfikatu:

[![Okno dialogowe dotyczące hasła logowania](images/vs/07-signing-password-vs-sml.png)](images/vs/07-signing-password-vs.png#lightbox)

Po zakończeniu procesu podpisywania, kliknij przycisk **Otwórz dystrybucję**:

[![Przycisk Otwórz dystrybucji](images/vs/08-open-distribution-sml.png)](images/vs/08-open-distribution.png#lightbox)

Powoduje to Eksploratora Windows, aby otworzyć folder zawierający wygenerowanego pliku APK. W tym momencie program Visual Studio został skompilowany aplikacji platformy Xamarin.Android w pliku APK, który jest gotowy do przekazania.
Poniższy zrzut ekranu przedstawia przykład aplikacji gotowych do opublikowania **MyApp.MyApp.apk**:

[![Plik APK wyświetlane w Eksploratorze Windows](images/vs/09-generated-app-vs-sml.png)](images/vs/09-generated-app-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


Jak pokazano w tym miejscu nowy certyfikat został dodany do magazynu kluczy. Aby opublikować aplikację w sklepie Google Play, kliknij przycisk **anulować** i przejdź do [publikowanie w sklepie Google Play](~/android/deploy-test/publishing/publishing-to-google-play/index.md).
W przeciwnym razie kliknij **dalej** Aby opublikować aplikację *ad-hoc* (w przypadku niezależnych dystrybucja), jak pokazano w poniższym przykładzie:

[![Podpisywanie i dystrybucja okna dialogowego](images/xs/04-select-identity-sml.png)](images/xs/04-select-identity.png#lightbox)

**Opublikuj jako Ad Hoc** okno dialogowe zawiera podsumowanie podpisaną aplikację przed jego opublikowaniem. Jeśli te informacje są poprawne, kliknij przycisk **Publikuj**.

[![Opublikuj jako Ad Hoc okna dialogowego](images/xs/05-publish-ad-hoc-sml.png)](images/xs/05-publish-ad-hoc.png#lightbox)

**Plik wyjściowy APK** okna dialogowego zostanie zapisany plik APK do określonej ścieżki. Kliknij przycisk **Zapisz**.

![Okno dialogowe pliku APK danych wyjściowych](images/xs/06-output-apk-file.png)

Następnie wprowadź hasło dla certyfikatu (hasła, który był używany w **Utwórz nowy certyfikat** okna dialogowego) i kliknij przycisk **OK**: 

![Wprowadź hasło certyfikatu](images/xs/07-signing-certificate.png)

Zestawu APK jest podpisany przy użyciu certyfikatu i zapisać w określonej lokalizacji. Kliknij przycisk **Odsłoń w programie Finder**:

[![Okno dialogowe zakończone powodzeniem publikacji](images/xs/08-app-is-ready-sml.png)](images/xs/08-app-is-ready.png#lightbox)

Spowoduje to otwarcie wyszukiwania do lokalizacji podpisanego pliku APK:

[![Plik APK wyświetlane w programie Finder](images/xs/09-show-in-finder-sml.png)](images/xs/09-show-in-finder.png#lightbox)

Plik APK jest gotowy do skopiowania finder i wysyłać do ostatecznego miejsca przeznaczenia. To dobry pomysł, aby zainstalować zestawu APK na urządzeniu z systemem Android i wypróbuj to przed dystrybucji. Zobacz [niezależnie publikowania](~/android/deploy-test/publishing/publishing-independently.md) Aby uzyskać więcej informacji dotyczących publikowania *ad-hoc* pliku APK.

-----



## <a name="next-steps"></a>Następne kroki

Po pakiet aplikacji został podpisany w wersji, należy go opublikować. W poniższych sekcjach opisano kilka sposobów, aby opublikować aplikację.
