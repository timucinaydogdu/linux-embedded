# direnv Guide

direnv, proje klasörüne girdiğinde Python sanal ortamını otomatik olarak
aktive eden, çıkınca deaktive eden bir araçtır.

---

## 1. Homebrew Kurulu mu Kontrol Et

brew --version

Çıktı vermiyorsa önce Homebrew kur:

/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

---

## 2. direnv Kur

brew install direnv

---

## 3. Shell'e Bağla

~/.zshrc dosyasını aç:

code ~/.zshrc

En alta şu satırı ekle:

eval "$(direnv hook zsh)"

Değişikliği uygula:

source ~/.zshrc

---

## 4. Proje Klasörü Hazırla

mkdir my-project
cd my-project

---

## 5. Python Sanal Ortamı Oluştur

python -m venv .venv

---

## 6. .envrc Dosyası Oluştur

echo "source .venv/bin/activate" > .envrc

---

## 7. direnv'e İzin Ver

direnv allow

Artık bu klasöre her girişte .venv otomatik aktive olur,
çıkışta otomatik kapanır.

---

## 8. Kontrol Et

direnv status
which python

which python çıktısı .venv içindeki Python'ı gösteriyorsa kurulum başarılı.

---

## 9. .gitignore Ayarı

echo ".venv/" >> .gitignore
echo ".envrc" >> .gitignore
