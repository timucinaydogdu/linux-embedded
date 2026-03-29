# Git & Terminal Reference

Git workflow, terminal komutları ve Python ortamı için hızlı başvuru kılavuzu.

---

## İçindekiler

- [1. Git Komutları](#1-git-komutları)
  - [Temel İşlemler](#temel-işlemler)
  - [Branch ve Merge](#branch-ve-merge)
  - [Geçmiş ve Geri Alma](#geçmiş-ve-geri-alma)
  - [Remote İşlemleri](#remote-işlemleri)
  - [Stash](#stash)
  - [Tag](#tag)
- [2. Terminal Komutları](#2-terminal-komutları)
  - [Navigasyon](#navigasyon)
  - [Dosya İşlemleri](#dosya-işlemleri)
  - [İçerik ve Arama](#içerik-ve-arama)
  - [İzin ve Sahiplik](#izin-ve-sahiplik)
  - [Sistem](#sistem)
- [3. Python Ortamı](#3-python-ortamı)
  - [pyenv](#pyenv)
  - [Virtual Environment](#virtual-environment)
  - [direnv](#direnv)

---

## 1. Git Komutları

### Temel İşlemler

```sh
git init                        # Yeni repo başlat
git clone <url>                 # Repoyu klonla
git status                      # Değişiklikleri göster
git add <dosya>                 # Dosyayı stage'e ekle
git add .                       # Tüm değişiklikleri ekle
git commit -m "mesaj"           # Commit oluştur
git push                        # Remote'a gönder
git pull                        # Remote'dan çek
```

### Branch ve Merge

```sh
git branch                      # Branch listesi
git branch <isim>               # Yeni branch oluştur
git checkout <isim>             # Branch'e geç
git checkout -b <isim>          # Oluştur ve geç
git merge <isim>                # Branch'i birleştir
git rebase <isim>               # Branch'i rebase et
git rebase -i HEAD~3            # Son 3 commit'i düzenle
git branch -d <isim>            # Branch sil
```

### Geçmiş ve Geri Alma

```sh
git log                         # Commit geçmişi
git log --oneline               # Kısa geçmiş
git diff                        # Değişiklikleri karşılaştır
git reset HEAD~1                # Son commit'i geri al (dosyalar kalır)
git reset --hard HEAD~1         # Son commit'i tamamen sil
git revert <commit-id>          # Commit'i geri döndür (güvenli)
```

### Remote İşlemleri

```sh
git remote -v                   # Remote listesi
git remote add origin <url>     # Remote ekle
git remote set-url origin <url> # Remote URL güncelle
git fetch                       # Remote değişiklikleri indir
git push -u origin main         # İlk push
```

### Stash

```sh
git stash                       # Değişiklikleri geçici sakla
git stash list                  # Stash listesi
git stash pop                   # Son stash'i geri getir
git stash drop                  # Son stash'i sil
```

### Tag

```sh
git tag                         # Tag listesi
git tag v1.0.0                  # Tag oluştur
git push origin v1.0.0          # Tag'i gönder
```

---

## 2. Terminal Komutları

### Navigasyon

```sh
pwd                             # Bulunduğun dizini göster
ls                              # Dosyaları listele
ls -la                          # Gizli dosyalarla listele
cd <dizin>                      # Dizine geç
cd ..                           # Üst dizine geç
cd ~                            # Home dizinine geç
```

### Dosya İşlemleri

```sh
touch <dosya>                   # Boş dosya oluştur
mkdir <dizin>                   # Klasör oluştur
mkdir -p a/b/c                  # İç içe klasör oluştur
cp <kaynak> <hedef>             # Kopyala
mv <kaynak> <hedef>             # Taşı veya yeniden adlandır
rm <dosya>                      # Dosya sil
rm -rf <dizin>                  # Klasörü sil (dikkatli kullan)
```

### İçerik ve Arama

```sh
cat <dosya>                     # Dosya içeriğini göster
nano <dosya>                    # Nano editörde aç
vim <dosya>                     # Vim editörde aç
grep "kelime" <dosya>           # Dosyada ara
grep -r "kelime" .              # Tüm dizinde ara
find . -name "*.py"             # Dosya bul
```

### İzin ve Sahiplik

```sh
chmod 755 <dosya>               # İzin değiştir
chown user:group <dosya>        # Sahip değiştir
```

### Sistem

```sh
df -h                           # Disk kullanımı
du -sh <dizin>                  # Dizin boyutu
echo "metin"                    # Metin yazdır
sudo <komut>                    # Yönetici olarak çalıştır
```

---

## 3. Python Ortamı

### pyenv

```sh
brew install pyenv              # Kur
pyenv install 3.12.2            # Versiyon indir
pyenv global 3.12.2             # Global versiyon belirle
pyenv local 3.12.2              # Proje versiyonu belirle
pyenv versions                  # Kurulu versiyonlar
```

~/.zshrc dosyasına ekle:

```sh
eval "$(pyenv init -)"
```

### Virtual Environment

```sh
python -m venv .venv            # Sanal ortam oluştur
source .venv/bin/activate       # Aktive et
deactivate                      # Deaktive et
pip install <paket>             # Paket yükle
pip freeze > requirements.txt   # Bağımlılıkları kaydet
pip install -r requirements.txt # Bağımlılıkları yükle
```

### direnv

Klasöre girildiğinde .venv otomatik aktive olur.

```sh
brew install direnv             # Kur
```

~/.zshrc dosyasına ekle:

```sh
eval "$(direnv hook zsh)"
```

Projede kullan:

```sh
echo "source .venv/bin/activate" > .envrc
direnv allow
```

> Detaylı kullanım için: [notes/direnv-guide.md](./notes/direnv-guide.md)
