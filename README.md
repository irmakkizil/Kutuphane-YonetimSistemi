📚 Akıllı Kütüphane Yönetim Sistemi

PHP ve MySQL ile geliştirilmiş, rol tabanlı bir kütüphane yönetim web uygulamasıdır. Öğrenciler kitapları arayıp ödünç talebi oluşturur, personel talepleri onaylayıp iade süreçlerini yönetir, yöneticiler ise kitapları, kullanıcıları ve raporları kontrol eder.

Üniversite dersi kapsamında geliştirilmiş bir öğrenci projesidir.

🚀 Özellikler
👤 Üç farklı rol
Rol	Yapabilecekleri
Öğrenci	Kitap arama ve filtreleme, kitap detayını görüntüleme, ödünç talebi oluşturma, kendi talep geçmişini takip etme
Personel	Gelen ödünç taleplerini onaylama/reddetme, iade alma, geciken kitapları ve aktif ödünçleri izleme, günlük istatistikleri görme
Admin	Kitap ekleme, düzenleme ve silme, kullanıcı arama/rol değiştirme/silme, sistem raporlarını görüntüleme
🔄 Ödünç süreci
Öğrenci talep oluşturur  →  Beklemede
                               │
              ┌────────────────┴────────────────┐
         Personel onaylar                  Personel reddeder
              │                                  │
         Onaylandı                          Reddedildi
   (stok -1, 15 gün süre verilir)
              │
        İade Talebi  →  İade Edildi (stok +1)
Stokta olmayan kitap için talep oluşturulamaz.
Aynı kitap için aktif (beklemede veya onaylı) talebi olan öğrenci tekrar talep açamaz.
Onaylanan kitaba otomatik 15 günlük iade süresi atanır; süresi geçenler personel panelinde "geciken kitaplar" olarak listelenir.
🔍 Arama ve filtreleme
Kitap adı veya yazara göre arama (Türkçe karakter duyarlı: utf8mb4_turkish_ci)
Yazar ve yıla göre filtreleme
📊 Admin raporları
Günlük, haftalık ve aylık (son 30 gün) ödünç talebi sayıları
En çok ödünç alınan 5 kitap
🔐 Güvenlik
Şifreler password_hash() ile saklanır, password_verify() ile doğrulanır
Veritabanı sorguları PDO prepared statement ile yapılır
Rol bazlı sayfa erişim kontrolü (session tabanlı)
Kayıt formunda doğrulamalar: e-posta formatı, rakamlardan oluşan okul numarası, 11 haneli telefon, en az 8 karakterli şifre (büyük harf, küçük harf ve rakam içermeli)
E-posta ve okul numarası benzersiz olmalıdır
🛠️ Kullanılan Teknolojiler
PHP 8 (PDO ile MySQL bağlantısı, session yönetimi)
MySQL 8
HTML5 / CSS3
JavaScript (temel etkileşimler)
Yerel geliştirme: MAMP
📁 Proje Yapısı
Kutuphane-Yonetim-Sistemi/
├── kutuphane_db.sql          # Veritabanı şeması
└── kutuphane/
    ├── index.php             # Karşılama sayfası
    ├── config/
    │   └── db.php            # Veritabanı bağlantısı
    ├── auth/                 # Giriş, kayıt, çıkış, oturum kontrolü
    ├── student/              # Öğrenci paneli (kitaplar, detay, taleplerim)
    ├── staff/                # Personel paneli (talepler, durum güncelleme)
    ├── admin/                # Admin paneli (kitap, kullanıcı, rapor)
    └── assets/css/           # Stil dosyaları
🗄️ Veritabanı

Üç tablodan oluşur:

users: id, name, email, password, school_no, phone, role (student / staff / admin)
books: id, title, author, category, year, summary, stock, shelf
borrow_requests: id, user_id, book_id, status, request_date, return_date
⚙️ Kurulum
Gereksinimler: PHP 8+, MySQL 8+ ve bir yerel sunucu (MAMP, XAMPP vb.)
Projeyi klonlayın:
bash
   git clone https://github.com/irmakkizil/Kutuphane-Yonetim-Sistemi.git
kutuphane klasörünü sunucunuzun kök dizinine (ör. MAMP için htdocs) kopyalayın.
phpMyAdmin'de kutuphane_db adında bir veritabanı oluşturun ve kutuphane_db.sql dosyasını içe aktarın.
kutuphane/config/db.php dosyasındaki bağlantı bilgilerini kendi ortamınıza göre düzenleyin:
php
   $host   = "localhost";
   $dbname = "kutuphane_db";
   $user   = "root";
   $pass   = "root";   // kendi MySQL şifrenizi yazın
Tarayıcıda http://localhost:8888/kutuphane/ (portunuz farklıysa ona göre) adresini açın.
İlk admin hesabını oluşturma

Kayıt olan her kullanıcı varsayılan olarak öğrenci olur. Admin veya personel yapmak için önce normal bir hesap oluşturun, ardından veritabanında rolünü güncelleyin:

sql
UPDATE users SET role = 'admin' WHERE email = 'ornek@mail.com';

Sonraki rol değişikliklerini admin panelindeki Kullanıcı Yönetimi sayfasından yapabilirsiniz.


🔭 Geliştirme Fikirleri
Durum değiştirme ve silme işlemlerinin GET yerine POST ile yapılması ve CSRF koruması eklenmesi
Veritabanı bilgilerinin ortam değişkenlerine veya git'e eklenmeyen bir yapılandırma dosyasına taşınması
Tablolar arasına yabancı anahtar (foreign key) kısıtlamaları eklenmesi
Gecikme için e-posta bildirimi
Sayfalama ve gelişmiş raporlama
Otomatik testler
