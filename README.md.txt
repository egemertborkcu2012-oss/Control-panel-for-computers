# Sistem Takip ve Görev Yöneticisi

Windows ve Linux masaüstü bilgisayarları için çevrimdışı çalışabilen grafik arayüzlü (GUI) uygulama. **Sistem** sekmesinde CPU, RAM, disk ve destekleniyorsa işlemci sıcaklığını gösterir; CPU/RAM grafiğini çizer. **Görev Yöneticisi** sekmesinde çalışan işlemleri listeler ve seçilen işlemi sonlandırmayı sağlar. **Dosya Güvenliği** sekmesi ClamAV kullanarak dosya/klasör tarar, Linux/Windows kullanıcısının `Downloads` / `İndirilenler` klasörüne yeni gelen dosyaları izler ve tespit edilen tehditlerin dosya adını, ClamAV tehdit adını, tarihini ve tam yolunu kaydeder.

> Bu, tam kapsamlı bir antivirüs veya indirme koruması değildir. Uygulama dosyaları kendisi indirmez. Otomatik izleme yalnızca kullanıcının standart `~/Downloads` klasörünü kapsar; başka klasörleri Dosya Güvenliği sekmesinden elle seçip tarayın. ClamAV imzalarına göre tespit yapar, bilinmeyen/yeniden paketlenmiş zararlıları kaçırabilir ve yanlış pozitif verebilir. Dosyaları otomatik silmez veya karantinaya almaz. Güncel bir antivirüsün yerine geçmez.

## Linux Mint — hızlı kurulum

İlk kurulum için internet bağlantısı gerekir. Terminal'i (**Ctrl+Alt+T**) açın. Proje dosyalarını (`system_monitor.py`, `requirements.txt`, `README.md`) tek klasöre koyun ve Terminal'de o klasöre geçin. Örneğin:

```bash
cd ~/İndirilenler/system-monitor
```

Ardından temel Python/Tkinter paketlerini ve ClamAV'ı kurun:

```bash
sudo apt update
sudo apt install python3 python3-venv python3-tk python3-pip clamav clamav-freshclam
```

Parola istenirse Linux kullanıcı parolanızı yazıp Enter'a basın (yazarken ekranda görünmez). Onay sorusuna `Y` yazıp Enter'a basın. ClamAV imzalarını güncelleyin:

```bash
sudo freshclam
```

Sonra proje klasöründe Python paketlerini kurup uygulamayı çalıştırın:

```bash
python3 -m venv .venv
. .venv/bin/activate
python -m pip install -r requirements.txt
python system_monitor.py
```

Son komut bir masaüstü GUI penceresi açar. Terminal yalnızca başlatmak içindir. İlk kurulumdan sonra, sonraki açılışlarda:

```bash
cd ~/İndirilenler/system-monitor
. .venv/bin/activate
python system_monitor.py
```

Uygulamayı kapatınca isterseniz `deactivate` komutuyla sanal ortamdan çıkabilirsiniz.

### Linux Mint adımlarının açıklaması

1. **Proje klasörü:** Üç proje dosyası aynı klasörde olmalı. Klasör yolunuz örnekten farklıysa `cd` komutundaki yolu değiştirin. Dosya yöneticisinde klasöre sağ tıklayıp **Terminalde Aç** seçeneğini de kullanabilirsiniz.
2. **Python ve Tkinter:** `python3`, `python3-venv`, `python3-pip` uygulama çalıştırma/kurulum araçlarıdır. `python3-tk`, grafik pencerenin açılması için gerekir.
3. **ClamAV ve imzalar:** `clamav`, yerel `clamscan` tarayıcısını; `clamav-freshclam` tehdit imzalarını güncelleme aracını kurar. İmza güncelliği tespit başarısı için önemlidir. `freshclam` veritabanının başka bir servis tarafından kullanıldığı uyarısını verirse sistemdeki `clamav-freshclam` hizmetinin güncellemesini bekleyip tekrar deneyin.
4. **Sanal ortam:** `.venv` Python bağımlılıklarını proje klasöründe tutar; Mint'in sistem Python kurulumunu değiştirmez. Terminal satırının başında `(.venv)` görünmesi etkin olduğu anlamına gelir.
5. **Paketler:** `pip install -r requirements.txt`, psutil, Pillow, pystray ve watchdog paketlerini kurar. Bunlar kurulduktan sonra uygulamanın arayüzü ve sistem takibi çevrimdışı çalışır. Zararlı yazılım taraması için ayrıca ClamAV ve güncel imza veritabanı gerekir.

## Diğer Linux dağıtımları

Dağıtımınızın paket yöneticisiyle Python 3, pip, venv, Tkinter/Tk ve ClamAV kurun. Paket adları/sürüm numaraları dağıtıma göre değişebilir. Sonrasında Linux Mint bölümündeki sanal ortam ve `pip install` komutlarını proje klasöründe çalıştırın.

### Ubuntu / Debian

```bash
sudo apt update
sudo apt install python3 python3-venv python3-tk python3-pip clamav clamav-freshclam
sudo freshclam
```

### Fedora

```bash
sudo dnf install python3 python3-pip python3-tkinter clamav clamav-update
sudo freshclam
```

### Arch Linux

```bash
sudo pacman -S python tk clamav
sudo freshclam
```

Bazı dağıtımlarda imzalar otomatik güncellenen bir servis tarafından yönetilir; `freshclam` veritabanının kullanımda olduğunu söylerse servis güncellemesini bekleyin. Linux'ta Tkinter kurulumunu `python3 -m tkinter` komutuyla sınayabilirsiniz; küçük bir test penceresi açılmalıdır. Uygulama grafik masaüstü oturumu gerektirir.

## Windows 10/11 kurulumu

1. Python 3.9 veya üstünü [python.org](https://www.python.org/downloads/) adresinden yükleyin. Kurulumda **Add Python to PATH** seçeneğini işaretleyin.
2. ClamAV'ın resmi Windows dağıtımını [clamav.net/downloads](https://www.clamav.net/downloads) adresinden kurun. Kurulum klasöründe `clamscan.exe` olmalı. İmza veritabanını ClamAV'ın `freshclam.exe` aracıyla güncelleyin. Bu araç bulunamazsa ClamAV kurulum yönergelerini izleyin. Uygulama `clamscan.exe` PATH'te ise veya standart `Program Files\ClamAV` klasöründeyse bulur.
3. `system_monitor.py`, `requirements.txt` ve `README.md` dosyalarını aynı klasöre koyun. O klasörde PowerShell açın ve çalıştırın:

```powershell
py -m venv .venv
.venv\Scripts\python -m pip install -r requirements.txt
.venv\Scripts\python system_monitor.py
```

Son komut GUI penceresini açar. Daha sonraki çalıştırmalarda proje klasöründen:

```powershell
.venv\Scripts\python system_monitor.py
```

## Dosya Güvenliği nasıl kullanılır?

1. ClamAV'ı kurup virüs imzalarını güncel tutun. Uygulamanın **Dosya Güvenliği** sekmesinde ClamAV'ın hazır olduğu görülmelidir.
2. Yeni dosyalar kullanıcının `Downloads` klasörüne geldiğinde otomatik tarama denenir. Başka bir klasör/harici disk için **Dosya / klasör tara** düğmesine basıp hedefi seçin. Var olan İndirilenler içeriği için **İndirilenler klasörünü tara** düğmesini kullanın.
3. Bulunan tehditler listede görüntülenir ve CSV kayıt dosyasına eklenir. Tehdit adı, ClamAV'ın imza adıdır; örneğin bir imza/ailenin adını temsil edebilir, ancak her zaman kesin virüs sınıflandırması anlamına gelmez.
4. Kayıt konumunu **Kayıt konumunu göster** düğmesiyle görebilirsiniz. Varsayılan konum Windows'ta `%LOCALAPPDATA%\sistem-takip\malware_detections.csv`, Linux'ta `~/.local/state/sistem-takip/malware_detections.csv` şeklindedir. CSV'yi elektronik tablo uygulamasıyla açabilirsiniz.
5. Tespit edilen dosyayı açmayın/çalıştırmayın. ClamAV veya kullandığınız güvenlik yazılımının önerdiği karantina/kaldırma adımlarını uygulayın. Bu uygulama dosyayı değiştirmez veya silmez.

## Sorun giderme

- **`No module named tkinter`:** Linux Mint/Ubuntu'da `sudo apt install python3-tk`; Fedora'da `sudo dnf install python3-tkinter` komutunu kullanın.
- **`No module named psutil`, `watchdog`, `PIL` veya `pystray`:** Proje klasöründe sanal ortamı etkinleştirin ve `python -m pip install -r requirements.txt` komutunu tekrar çalıştırın.
- **ClamAV bulunamadı / otomatik tarama yok:** ClamAV kurulu ve `clamscan` (Windows'ta `clamscan.exe`) erişilebilir olmalı. ClamAV'ı kurduktan sonra uygulamayı yeniden başlatın. Windows'ta kurulum standart klasörden farklıysa ClamAV'ın `bin` klasörünü PATH'e ekleyin.
- **`freshclam` başarısız:** İnternet bağlantısını, sistem tarihini ve ClamAV güncelleme hizmetinin durumunu kontrol edin. Güncel imza olmadan güvenilir tespit beklenemez.
- **`requirements.txt` bulunamadı:** Terminal/PowerShell'de proje klasörüne geçin; `ls` (Linux) veya `dir` (Windows) ile dosyaların orada olduğunu kontrol edin.
- **Linux'ta pencere açılmıyor:** Grafik masaüstü oturumunda çalıştırın; yalnızca SSH/metin konsolu GUI açamaz.
- **Tepsi simgesi yok:** Tepsi desteği masaüstü ortamına bağlıdır; ana pencere çalışmaya devam eder.
- **Sıcaklık `N/A`:** Cihaz sıcaklık sensörünü işletim sistemine sunmuyor olabilir.

## Özellikler ve sınırlamalar

- Sistem ölçümleri her saniye güncellenir; CPU/RAM grafiği son 60 saniyeyi gösterir. Kaynak kullanımı %90'ı aşarsa arayüz uyarı verir.
- Görev Yöneticisi çalışan süreçleri PID, CPU, bellek ve kullanıcı bilgileriyle gösterir. Bazı sistem süreçlerini görmek/sonlandırmak için yönetici izni gerekebilir. Önemli bir işlemi kapatmak sistemi veya açık dosyaları etkileyebilir.
- Otomatik dosya izleme standart `~/Downloads` dizinini izler; tarama ClamAV kurulu ve imzaları mevcut olduğunda çalışır. Her indirme yolunu takip etmez ve ağ trafiğini indirme sırasında engellemez.
- Tarama sonuçları ClamAV'ın güncel imza veritabanına bağlıdır. Temiz sonucu, dosyanın kesinlikle zararsız olduğu garantisi değildir.
- Tepsi simgesi ve sıcaklık sensörü donanım/masaüstü ortamı desteğine bağlıdır.
