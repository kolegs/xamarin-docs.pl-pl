---
title: Tworzenie CryptoObject
ms.prod: xamarin
ms.assetid: 4D159B80-FFF4-4136-A7F0-F8A5C6B86838
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: f7a8ab7a43c0a3258cf6e737b0d235cbe7a1c747
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="creating-a-cryptoobject"></a>Tworzenie CryptoObject

Integralność wyniki uwierzytelniania odcisk palca ważne jest, aby aplikacja &ndash; jest jak aplikacja zna tożsamości użytkownika. Użytkownik może teoretycznie dla złośliwego oprogramowania innych firm w celu przechwycenia i manipulowanie wyników zwróconych przez skaner linii papilarnych. W tej sekcji będzie omawiać jeden technika zachowuje ważność wyników linii papilarnych. 

`FingerprintManager.CryptoObject` Jest otokę kryptografii Java interfejsów API i jest używany przez `FingerprintManager` do ochrony integralności żądania uwierzytelnienia. Zazwyczaj `Javax.Crypto.Cipher` obiektu to mechanizm służący do szyfrowania wyników linii papilarnych. `Cipher` Sam obiekt będzie używać klucza, który jest tworzony przez aplikację przy użyciu interfejsów API w magazynie kluczy systemu Android.

Aby zrozumieć, jak te wszystkie klasy współdziałać ze sobą, najpierw Przyjrzyjmy się następujący kod, który demonstruje sposób tworzenia `CryptoObject`, a następnie wyjaśnić bardziej szczegółowo:

```csharp
public class CryptoObjectHelper
{
    // This can be key name you want. Should be unique for the app.
    static readonly string KEY_NAME = "com.xamarin.android.sample.fingerprint_authentication_key";

    // We always use this keystore on Android.
    static readonly string KEYSTORE_NAME = "AndroidKeyStore";

    // Should be no need to change these values.
    static readonly string KEY_ALGORITHM = KeyProperties.KeyAlgorithmAes;
    static readonly string BLOCK_MODE = KeyProperties.BlockModeCbc;
    static readonly string ENCRYPTION_PADDING = KeyProperties.EncryptionPaddingPkcs7;
    static readonly string TRANSFORMATION = KEY_ALGORITHM + "/" +
                                            BLOCK_MODE + "/" +
                                            ENCRYPTION_PADDING;
    readonly KeyStore _keystore;

    public CryptoObjectHelper()
    {
        _keystore = KeyStore.GetInstance(KEYSTORE_NAME);
        _keystore.Load(null);
    }

    public FingerprintManagerCompat.CryptoObject BuildCryptoObject()
    {
        Cipher cipher = CreateCipher();
        return new FingerprintManagerCompat.CryptoObject(cipher);
    }

    Cipher CreateCipher(bool retry = true)
    {
        IKey key = GetKey();
        Cipher cipher = Cipher.GetInstance(TRANSFORMATION);
        try
        {
            cipher.Init(CipherMode.EncryptMode | CipherMode.DecryptMode, key);
        } catch(KeyPermanentlyInvalidatedException e)
        {
            _keystore.DeleteEntry(KEY_NAME);
            if(retry)
            {
                CreateCipher(false);
            } else
            {
                throw new Exception("Could not create the cipher for fingerprint authentication.", e);
            }
        }
        return cipher;
    }

    IKey GetKey()
    {
        IKey secretKey;
        if(!_keystore.IsKeyEntry(KEY_NAME))
        {
            CreateKey();
        }

        secretKey = _keystore.GetKey(KEY_NAME, null);
        return secretKey;
    }

    void CreateKey()
    {
        KeyGenerator keyGen = KeyGenerator.GetInstance(KeyProperties.KeyAlgorithmAes, KEYSTORE_NAME);
        KeyGenParameterSpec keyGenSpec =
            new KeyGenParameterSpec.Builder(KEY_NAME, KeyStorePurpose.Encrypt | KeyStorePurpose.Decrypt)
                .SetBlockModes(BLOCK_MODE)
                .SetEncryptionPaddings(ENCRYPTION_PADDING)
                .SetUserAuthenticationRequired(true)
                .Build();
        keyGen.Init(keyGenSpec);
        keyGen.GenerateKey();
    }
}
```

Przykładowy kod tworzy nową `Cipher` dla każdego `CryptoObject`, przy użyciu klucza, który został utworzony przez aplikację. Klucz jest identyfikowany przez `KEY_NAME` zmiennej, która została ustawiona na początku `CryptoObjectHelper` klasy. Metoda `GetKey` będzie spróbuj i pobrać klucz przy użyciu interfejsów API systemu Android magazynu kluczy. Jeśli klucz nie istnieje, następnie metoda `CreateKey` utworzy nowy klucz dla aplikacji.

Szyfr zostanie uruchomiony przy użyciu wywołania do `Cipher.GetInstance`, biorąc _przekształcania_ (wartość ciągu, która zawiera informacje dotyczące szyfrowania i odszyfrowywania danych szyfrowania). Wywołanie `Cipher.Init` ukończy inicjowania szyfrowania, zapewniając klucza z aplikacji. 

Ważne jest, aby zrealizować to Brak niektórych sytuacjach, w którym Android może unieważnić klucza: 

* Nowy odcisk palca został zarejestrowany z urządzeniem.
* Nie ma żadnych odciski palców zarejestrowane urządzenia.
* Użytkownik wyłączył blokada ekranu.
* Użytkownik zmienił blokada ekranu (typ screenlock lub numeru PIN wzorzec używany).

W takim przypadku `Cipher.Init` zgłosi [ `KeyPermanentlyInvalidatedException` ](http://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html). W powyższym przykładowym kodzie pułapki ten wyjątek, Usuń klucz i następnie utwórz nową.

Następna sekcja będzie omawiać temat Utwórz klucz i zapisz go na urządzeniu.

## <a name="creating-a-secret-key"></a>Tworzenie klucza tajnego

`CryptoObjectHelper` Klasy używa Android [ `KeyGenerator` ](https://developer.xamarin.com/api/type/Javax.Crypto.KeyGenerator/) Utwórz klucz i zapisz go na urządzeniu. `KeyGenerator` Klasy można utworzyć klucza, ale wymaga niektóre metadane dotyczące typu klucza do utworzenia. Te informacje są dostarczane przez wystąpienie [ `KeyGenParameterSpec` ](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html) klasy. 

A `KeyGenerator` zostanie uruchomiony przy użyciu `GetInstance` metoda fabryki. Przykładowy kod używa [ _Advanced Encryption Standard_ ](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) (_AES_) jako algorytmu szyfrowania. AES podzielić danych na bloki o stałym rozmiarze, a następnie szyfrować każdego z tych bloków.

Następnie `KeyGenParameterSpec` jest tworzony przy użyciu `KeyGenParameterSpec.Builder`. `KeyGenParameterSpec.Builder` Opakowuje następujące informacje na temat klucza, który ma być tworzony:

* Nazwa klucza.
* Klucz musi być ważne dla szyfrowania i odszyfrowywania.
* W przykładowym kodzie `BLOCK_MODE` ustawiono _Cipher Block Chaining_ (`KeyProperties.BlockModeCbc`), co oznacza, że każdy blok jest XORed z poprzedniego bloku (Tworzenie zależności między każdego bloku). 
* `CryptoObjectHelper` Używa [ _publiczny klucz szyfrowania standardowe #7_ ](https://tools.ietf.org/html/rfc2315) (_PKCS7_) do generowania bajtów, które będą wypełniania bloków, aby upewnić się, że są one wszystkich ten sam rozmiar .
* `SetUserAuthenticationRequired(true)` oznacza, że uwierzytelnianie użytkownika jest wymagane przed użyciem klucza.

Raz `KeyGenParameterSpec` jest utworzona, jest używany do inicjowania `KeyGenerator`, która spowoduje wygenerowanie klucza i bezpiecznie przechowywać na urządzeniu. 

## <a name="using-the-cryptoobjecthelper"></a>Przy użyciu CryptoObjectHelper

Teraz, przykładowy kod ma hermetyzowany znacznie logikę tworzenie `CryptoWrapper` do `CryptoObjectHelper` klas, możemy ponownie kod od początku przewodnika i korzystać z `CryptoObjectHelper` do tworzenia szyfrowania i uruchomić skanera odcisk palca: 

```csharp
protected void FingerPrintAuthenticationExample()
{
    const int flags = 0; /* always zero (0) */
    
    CryptoObjectHelper cryptoHelper = new CryptoObjectHelper();
    cancellationSignal = new Android.Support.V4.OS.CancellationSignal();
    
    // Using the Support Library classes for maximum reach
    FingerprintManagerCompat fingerPrintManager = FingerprintManagerCompat.From(this);
    
    // AuthCallbacks is a C# class defined elsewhere in code.
    FingerprintManagerCompat.AuthenticationCallback authenticationCallback = new MyAuthCallbackSample(this);

    // Here is where the CryptoObjectHelper builds the CryptoObject. 
    fingerprintManager.Authenticate(cryptohelper.BuildCryptoObject(), flags, cancellationSignal, authenticationCallback, null);
}
```

Teraz, gdy będziemy mieć przedstawiono sposób tworzenia `CryptoObject`, pozwala przejść do Zobacz jak `FingerprintManager.AuthenticationCallbacks` są używane do przesyłania wyniki usługi skaner linii papilarnych do aplikacji systemu Android.



## <a name="related-links"></a>Linki pokrewne

- [Szyfrowania](https://developer.xamarin.com/api/type/Javax.Crypto.Cipher/)
- [FingerprintManager.CryptoObject](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [KeyGenerator](https://developer.xamarin.com/api/type/Javax.Crypto.KeyGenerator/)
- [KeyGenParameterSpec](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html)
- [KeyGenParameterSpec.Builder](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder.html)
- [KeyPermanentlyInvalidatedException](http://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html)
- [KeyProperties](http://developer.android.com/reference/android/security/keystore/KeyProperties.html)
- [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)
- [RFC 2315 - PCKS #7](https://tools.ietf.org/html/rfc2315)
