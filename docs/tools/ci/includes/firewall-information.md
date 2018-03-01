## <a name="firewall-configuration"></a>Konfiguracja zapory

Aby testy do przesłania do chmury testowej Xamarin przesyłanie testy komputera musi być mogły komunikować się z serwerami chmury testowej. Zapory muszą być skonfigurowane tak, aby zezwolić na ruch sieciowy do i z serwerów znajdujących się na **testcloud.xamarin.com** na porty 80 i 443. Ten punkt końcowy jest zarządzany przez serwer DNS i adres IP mogą ulec zmianie. 

W niektórych sytuacjach testu (lub urządzeniu z systemem testu) muszą komunikować się z serwerami sieci web chronionych przez zaporę. W tym scenariuszu zapora musi być skonfigurowana zezwalająca na ruch z następujących adresów IP:

* **195.249.159.238**
* **195.249.159.239**
