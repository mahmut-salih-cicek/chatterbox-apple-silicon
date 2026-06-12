# chatterbox-apple-silicon

HOW TO START

python3 -c "import app; print(app.__file__); print(hasattr(app, 'main')); app.main()"



Nuitka ile `--mode=module` kullanarak derleme yaptığında, kodların bağımsız bir çalıştırılabilir dosya (executable) yerine, sistemindeki Python sürümüne ve işletim sistemine sıkı sıkıya bağlı C uzantılı Python modülleri (`.so` dosyaları) haline gelir.

Dosya isimlerindeki `cpython-310-darwin.so` ibaresi net bir gerçeği işaret ediyor: **Bu dosyalar yalnızca Apple Silicon (M1/M2/M3) işlemcili ve tam olarak Python 3.10 yüklü bir Mac'te çalışır.** Hedef cihazda Python 3.11 veya farklı bir sürüm varsa `import app` komutu hata verecektir. `--mode=module` bağımlılıkları içine gömmediği için, gerekli tüm kütüphaneleri hedef cihazda da kurman gerekir.

İşte yeni M1 MacBook Pro'da sistemi ayağa kaldırmak için yapman gerekenler:

### 1. `requirements.txt` Dosyası

Hedef cihazda oluşturman gereken `requirements.txt` dosyasının içeriği:

```text
gradio>=4.0.0
soundfile>=0.12.1
numpy>=1.24.0
mlx>=0.16.0
mlx-audio>=0.1.0
requests>=2.31.0

```

### 2. Hedef Cihazda Kurulum Adımları

Yeni cihazda sırasıyla şu adımları izle:

**Adım 1:** Derlediğin dosyaları (`.so` uzantılı modüller ve istersen tip tanımlamaları için `.pyi` dosyaları) yeni Mac'te bir klasöre kopyala (örneğin `~/Desktop/chatterbox-app`).

**Adım 2:** Terminali açıp o klasöre git:

```bash
cd ~/Desktop/chatterbox-app

```

**Adım 3:** Cihazda Python 3.10 olduğundan emin ol. Varsa, bu projeye özel temiz bir sanal ortam oluştur ve aktif et:

```bash
python3.10 -m venv venv
source venv/bin/activate

```

**Adım 4:** Yukarıda hazırladığımız `requirements.txt` dosyasını kullanarak bağımlılıkları yükle:

```bash
pip install --upgrade pip
pip install -r requirements.txt

```

**Adım 5:** Kendi verdiğin komutla uygulamayı başlat:

```bash
python3 -c "import app; print(app.__file__); print(hasattr(app, 'main')); app.main()"

```

**Önemli Not:** `app.py` kodunun içerisinde `MODEL_ROOT = "/Users/macpro/chatterbox"` yolu sabit (hardcoded) olarak verilmiş. Yeni Mac'te kullanıcı adı `macpro` değilse veya model dosyaları o dizinde yoksa, program modeli bulamayacağı için çalışma zamanında hata verecektir. Modelleri yeni cihazda da tam olarak o yola koyduğundan veya çalıştırmadan önce kodu yeni yola göre güncelleyip tekrar derlediğinden emin ol.
