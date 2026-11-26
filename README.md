# Bilişim Kulübü Web Sunucusu (BSAT Dönem Ödevi)

- **Ad Soyad:** Batu Ergül
- **Port:** `8011`

## Adım Adım Yapılanlar

- **Adım 0 — Depo ve SSH Kurulumu:** GitHub'da herkese açık `bsat-kulup-sunucu` deposunu oluşturdum. WSL2 Ubuntu ortamında Ed25519 türünde SSH anahtar çifti üretip açık anahtarı GitHub hesabıma ekledim, Git kullanıcı bilgilerimi tanımladım ve depoyu `~/kulup-sunucu` dizinine klonlayıp `kanitlar/` klasörünü açtım.
- **Adım 1 — Makineyi Tanıma:** `free -h`, `df -h /`, `nproc` ve `ip -br addr` komutlarıyla sistemin RAM (15 GiB), kök disk doluluğu (%1), çekirdek sayısı (12) ve WSL ağ arayüz/IP (`172.28.106.75/20`) bilgilerini sırayla `kanitlar/1-makine.txt` dosyasına kaydettim.
- **Adım 2 — Kulüp Klasörü ve İzinler:** `kulup` grubunu oluşturup kendi kullanıcımı (`batue`) `-aG` seçeneğiyle mevcut gruplarımı kaybetmeden bu gruba ekledim. `/srv/kulup` dizinini açıp sahipliğini `batue:kulup`, izinlerini ise `775` (`drwxrwxr-x`) olarak ayarladım ve kanıtları `kanitlar/2-klasor.txt` dosyasına aldım.
- **Adım 3 — Web Sunucusu (Nginx):** `apt` ile Nginx'i kurdum ve `/etc/nginx/sites-available/default` dosyasında IPv4/IPv6 dinleme portlarını `8011`, kök dizini ise `/srv/kulup` yaptım. `sudo nginx -t` ile ayarı doğruladıktan sonra servisi yeniden başlatıp açılışta otomatik başlayacak şekilde etkinleştirdim (`kanitlar/3-nginx.txt`, `kanitlar/3-nginx-ayar.txt`).
- **Adım 4 — Duyuru Betiği ve Cron:** `duyurular.txt` içine kulüp duyurularını yazdım; `duyuru.sh` betiğindeki satır sayısı (`cat "$KAYNAK" | wc -l`) ve tarih/saat (`date '+%F %T'`) kısımlarını tamamlayıp çalıştırma izni (`chmod +x`) verdim. Betiği `crontab` ile her 5 dakikada bir tam yoluyla çalışacak şekilde zamanladım, yeni duyuru ekleyerek `/srv/kulup/index.html` dosyasının otomatik güncellendiğini doğruladım (`kanitlar/4-cron.txt`).
- **Adım 5 — Ağdan Kontrol:** `ss -tln | grep 8011` ile Nginx'in `8011` portunu dinlediğini, `curl -sI http://localhost:8011` ile `HTTP/1.1 200 OK` yanıtı verdiğini kanıtladım (`kanitlar/5-ag.txt`) ve Windows tarayıcısından sayfayı açıp görüntüledim.
- **Adım 6 — Günlükler:** `journalctl -u nginx` ve `/var/log/nginx/access.log` günlüklerinin son 5 satırını `kanitlar/6-gunluk.txt` dosyasına kaydedip erişim kayıtlarındaki istemci IP, zaman, istek yolu ve HTTP durum kodlarını inceledim.
- **Adım 7 — Yedekleme:** `~/yedek` klasörünü oluşturup `/srv/kulup` dizinini `tar -czf` ile bugünün tarihini taşıyan `kulup-2026-11-26.tar.gz` arşivine yedekledim ve `tar -tzf` ile arşivi açmadan içeriğini doğrulayıp `kanitlar/7-yedek.txt` dosyasına yazdım.

## Yapay Zekâ Kullanımı

Ödev adımlarını kendim takip edip uygularken, özellikle `duyuru.sh` betiğindeki sözdizimi ve komut seçeneklerinin doğrulanması ile terminal çıktılarının yorumlanmasında Gemini Neon modelinden faydalandım. Her adımı ve kanıt dosyalarını tek tek kontrol ederek ayrı kayıtlar (commit) hâlinde GitHub'a gönderdim.