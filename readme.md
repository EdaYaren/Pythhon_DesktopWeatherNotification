# ☁️ Hava Durumu Bildirim Uygulaması

Bu uygulama, kullanıcıya belirli bir saatte günlük hava durumu bildirimleri göndermek için tasarlanmıştır. Uygulama, **requests** ve **win10toast** kütüphanelerini kullanarak hava durumu verilerini alır ve masaüstünde bildirim olarak gösterir.

## 📦 Kullanılan Kütüphaneler
- `requests`: Hava durumu verilerini almak için.
- `win10toast`: Windows için masaüstü bildirimleri oluşturmak için.

## 📋 Notlar
- Uygulama, Windows işletim sisteminde çalışmakta ve masaüstü bildirimleri göndermek için `win10toast` kütüphanesini kullanmaktadır.
- Eğer Windows işletim sistemine sahip değilseniz, aşağıdaki kütüphaneleri kullanabilirsiniz:
  - **Linux:** `notify2` kütüphanesi, masaüstü bildirimleri göndermek için kullanılabilir. Ancak, çalışabilmesi için sistemde `dbus` modülünün bulunması gerekmektedir.
  - **macOS:** `osascript` veya `pynotifier` gibi kütüphaneler kullanılabilir. Bu kütüphaneler, macOS üzerinde bildirim göndermek için tasarlanmıştır.
- `notify2` kütüphanesi, masaüstü ortamına bağımlı olduğu için Colab gibi tarayıcı tabanlı platformlarda çalışmaz. Colab, masaüstü bildirimlerini desteklemez ve `notify2` gibi kütüphanelerin çalışması için gerekli olan D-Bus ve GUI desteği yoktur.
- `notify2` kütüphanesi, bildirimleri gönderebilmek için Linux sistemlerinde `dbus` modülüne ihtiyaç duyar. `dbus` genellikle Linux sistemlerinde varsayılan olarak bulunur, ancak bazı durumlarda eksik olabilir ya da yüklü olmayabilir. Windows veya macOS gibi diğer işletim sistemlerinde `dbus` desteği olmaması nedeniyle `notify2` kütüphanesi çalışmaz. Alternatif olarak, Windows’ta bildirimleri göndermek için `plyer` veya `win10toast` gibi kütüphaneleri kullanabilirsiniz.


## 🔑 API Anahtarı
Hava durumu verilerini alabilmek için bir API anahtarına ihtiyacınız var. [WeatherAPI](https://www.weatherapi.com/) sitesinden ücretsiz olarak kayıt olabilir ve API anahtarınızı edinebilirsiniz.

## ⏰ Bildirim Zamanı
Bildirimler, her gün 17:06'da gönderilecektir. Bu süreyi ihtiyacınıza göre değiştirebilirsiniz.

## 🚀 Nasıl Çalışır?
Bu uygulama, hava durumu verilerini almak ve bildirim olarak göstermek için aşağıdaki adımları takip eder:

1. **API'ye İstek Gönderme**: 
   - Kullanıcıdan alınan API anahtarı ve şehir bilgisi ile birlikte, uygulama hava durumu verilerini almak için WeatherAPI'ye bir istek gönderir.

2. **Veri Alma**:
   - Uygulama, API'den gelen yanıtın durumunu kontrol eder. Eğer yanıt başarılıysa (HTTP 200), hava durumu verileri JSON formatında alınır.
   - Bu veriler arasında sıcaklık, hava durumu durumu (örneğin, açık, yağmurlu) gibi bilgileri bulabilirsiniz.

3. **Verileri Çıkarma**:
   - Alınan JSON verisi içinden sıcaklık (`temp_c`) ve hava durumu durumu (`condition`) bilgileri çıkarılır.
   - Bu bilgiler kullanıcıya anlamlı bir formatta gösterilmek üzere hazırlanır.

4. **Masaüstünde Bildirim Gösterme**:
   - `win10toast` kütüphanesi kullanılarak, çıkarılan sıcaklık ve hava durumu bilgileri masaüstünde bir bildirim olarak gösterilir.
   - Bildirim, belirli bir süre (10 saniye) ekranda kalır ve kullanıcıya hava durumu bilgisini hızlıca iletir.

      ### Bildirim:
   <img src="DesktopNotification.png" alt="DesktopNotification" width="600" height="400">

5. **Döngü ile Güncelleme**:
   - Uygulama, belirlenen zaman diliminde (örneğin, her gün saat 17:06) hava durumu bildirimlerini alabilmek için sonsuz bir döngü içerisinde çalışır.
   - Hedef zaman geçildiyse, bir gün ileri alınarak bir sonraki gün için bildirim planlanır.

Bu süreç, kullanıcının güncel hava durumu bilgilerini hızlı ve etkili bir şekilde almasını sağlar. Uygulama sürekli olarak çalıştığından, kullanıcı belirlenen saatte otomatik olarak bildirim alır.
