# macOS Dev Setup

Yazılım geliştirme için temiz macOS kurulum rehberi.
İşletim sistemini **İngilizce** kurun — Türkçe kurulum yazılım hatalarına yol açabilir.

---

## İçindekiler

- [1. OS Ayarları](#1-os-ayarları)
- [2. Homebrew](#2-homebrew)
- [3. Git ve GitHub](#3-git-ve-github)
- [4. Terminal Kurulumu](#4-terminal-kurulumu)
- [5. Uygulama Listesi](#5-uygulama-listesi)
- [6. Sistem Temizliği](#6-sistem-temizliği)

---

## 1. OS Ayarları

### Accessibility
- Display → Reduce Transparency → **Yes**

### Trackpad
- Scroll & Zoom → Natural Scrolling → **No**

### Dock

Dock yerine RayCast ve AltTab kullanıyorum. Mümkün olduğunca küçük ve gizli tutuyorum.

- System Preferences → Desktop & Dock
  - Size → **Small as possible**
  - Position on screen → **Left**
  - Automatically hide and show the Dock → **Yes**
  - Animate opening applications → **No**
  - Show suggested and recent apps in the Dock → **No**

Dock animasyonunu hızlandırmak için:

```sh
defaults write com.apple.dock autohide-delay -float 0
killall Dock
```

Varsayılana döndürmek için:

```sh
defaults delete com.apple.dock autohide-delay
killall Dock
```

### Desktop

Sonoma'nın Stage Manager ve Widget özelliklerini devre dışı bırakıyorum.

- System Preferences → Desktop & Dock → Desktop & Stage Manager
  - Stage Manager → **uncheck**
  - Widgets on Desktop → **uncheck**

### Finder

- Finder → Preferences → General
  - Show these on the desktop → **None**
  - New Finder windows show → **Home Folder**
- Finder → Preferences → Advanced
  - Show all filename extensions → **Yes**
  - Show warning before changing an extension → **No**
  - When performing a search → **Search the current folder**
- View
  - Show Status Bar → **Yes**
  - Show Path Bar → **Yes**
  - Show Tab Bar → **Yes**

---

## 2. Homebrew

macOS için paket yöneticisi.

### Kurulum

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### PATH Ayarı

Kurulum sonrası terminal ekranında PATH komutu görünür, onu çalıştırın.
Görünmezse:

```sh
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

### Güncelleme

```sh
brew update
brew upgrade
```

---

## 3. Git ve GitHub

### Kurulum

```sh
brew install git
```

### Yapılandırma

```sh
git config --global user.name "your-name"
git config --global user.email "your-email"
```

### SSH Key Oluşturma

```sh
ssh-keygen -t ed25519 -C "your-email"
```

### SSH Key GitHub'a Ekleme

```sh
pbcopy < ~/.ssh/id_ed25519.pub
```

GitHub → Settings → SSH and GPG keys → New SSH key → yapıştır → kaydet.

### Bağlantı Testi

```sh
ssh -T git@github.com
```

### Push Hatası Alırsanız

GitHub → Settings → Personal Access Tokens (Classic) → token oluştur → terminalde:

```sh
git remote set-url origin https://YOUR-TOKEN@github.com/username/reponame
```

---

## 4. Terminal Kurulumu

### iTerm2

```sh
brew install --cask iterm2
```

Tercih ettiğim ayarlar:

- Appearance → Theme → **Minimal**
- Appearance → Tabbar Location → **Bottom**
- Profiles → Default → Color Presets → **High Contrast**
- Profiles → Default → Text → Font → NerdFont (nerdfonts.com)
- Profiles → Default → Keys → Key Mappings → **Natural Text Editing**
- Profiles → Default → Window → Style → **Full Height Right of Screen**
- Keys → Hotkey → Show/Hide all windows → kısayol belirle

### Oh My Zsh

```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

### Powerlevel10k

**1. Tema klonla:**

```sh
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

**2. ~/.zshrc dosyasında tema değiştir:**

```sh
ZSH_THEME="powerlevel10k/powerlevel10k"
```

**3. Uygula:**

```sh
source ~/.zshrc
```

Terminal yeniden başlatıldığında yapılandırma sihirbazı otomatik açılır.

### Zsh Eklentileri

**zsh-autosuggestions** — önceki komutlardan otomatik tamamlama

```sh
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

**zsh-syntax-highlighting** — yazarken sözdizimi renklendirme

```sh
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

**zsh-completions** — ek tamamlama komutları

```sh
git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-completions
```

**zsh-history-substring-search** — geçmişte alt dize araması

```sh
git clone https://github.com/zsh-users/zsh-history-substring-search ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-history-substring-search
```

**zsh-interactive-cd** — etkileşimli dizin değiştirme

```sh
git clone https://github.com/changyuheng/zsh-interactive-cd ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-interactive-cd
```

**zsh-docker-aliases** — Docker kısayolları

```sh
git clone https://github.com/felixr/docker-zsh ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/docker
```

**colorls** — simgeli dosya listesi

```sh
sudo gem install colorls
```

~/.zshrc sonuna ekle:

```sh
alias ls='colorls'
```

**z** — sık kullanılan dizinlere hızlı geçiş

```sh
brew install z
```

**autojump** — z'ye alternatif

```sh
brew install autojump
```

Tüm eklentileri ~/.zshrc dosyasında etkinleştir:

```sh
plugins=(git z autojump zsh-autosuggestions zsh-syntax-highlighting zsh-completions zsh-history-substring-search zsh-interactive-cd docker)
```

Değişiklikleri uygula:

```sh
source ~/.zshrc
```

---

## 5. Uygulama Listesi

| Uygulama | Kurulum | Açıklama |
|----------|---------|----------|
| Python | `brew install python` | Python programlama dili |
| Node.js | `brew install node` | JavaScript runtime |
| R | `brew install r` | İstatistik programlama dili |
| Julia | `brew install --cask julia` | Bilimsel programlama dili |
| VS Code | `brew install --cask visual-studio-code` | Kod editörü |
| Docker | `brew install --cask docker` | Konteyner yöneticisi |
| Java | `brew install openjdk` | Java geliştirme kiti |
| SourceTree | `brew install --cask sourcetree` | Git arayüzü |
| Brave | `brew install --cask brave-browser` | Web tarayıcısı |
| iTerm2 | `brew install --cask iterm2` | Terminal emülatörü |
| Xcode | `brew install --cask xcodes` | Apple geliştirme ortamı |
| SF Symbols | `brew install --cask sf-symbols` | Apple sembol kütüphanesi |
| AltTab | `brew install --cask alt-tab` | Windows tarzı uygulama geçişi |
| Rectangle | `brew install --cask rectangle` | Pencere yöneticisi |
| RaindropIO | `brew install --cask raindropio` | Bookmark yöneticisi |
| Bitwarden | `brew install bitwarden` | Şifre yöneticisi |
| AlDente | `brew install --cask aldente` | Batarya yöneticisi |
| Mac Mouse Fix | `brew install --cask mac-mouse-fix` | Mouse ayarları |
| Keeping You Awake | `brew install --cask keepingyouawake` | Uyku engelleme |
| Free Download Manager | `brew install --cask free-download-manager` | İndirme yöneticisi |
| Android File Transfer | `brew install --cask android-file-transfer` | Android bağlantısı |
| VMware Fusion | `brew install --cask vmware-fusion` | Sanal makine |
| Figma | `brew install --cask figma` | Tasarım aracı |
| OBS | `brew install --cask obs` | Ekran kaydı ve yayın |
| Kdenlive | `brew install --cask kdenlive` | Video düzenleme |
| Audacity | `brew install --cask audacity` | Ses düzenleme |
| VLC | `brew install --cask vlc` | Video oynatıcı |
| Keka | `brew install --cask keka` | Arşiv yöneticisi |
| Kap | `brew install --cask kap` | Ekran kaydedici |
| KeyCastr | `brew install --cask keycastr` | Tuş gösterici |
| BetterDisplay | `brew install --cask betterdisplay` | Harici ekran kontrolü |
| LibreOffice | `brew install --cask libreoffice` | Ofis paketi |
| Spotify | `brew install --cask spotify` | Müzik |
| WhatsApp | `brew install --cask whatsapp` | Mesajlaşma |
| Discord | `brew install --cask discord` | Mesajlaşma |
| Slack | `brew install --cask slack` | İş mesajlaşması |
| Zoom | `brew install --cask zoom` | Video konferans |
| Steam | `brew install --cask steam` | Oyun kütüphanesi |
| Epic Games | `brew install --cask epic-games` | Oyun kütüphanesi |
| Hand Mirror | [App Store](https://apps.apple.com/us/app/hand-mirror/id1502839586?mt=12) | Kamera önizleme |
| One Thing | [App Store](https://apps.apple.com/us/app/one-thing/id1604176982?mt=12) | Hedef odaklanma |

---

## 6. Sistem Temizliği

```sh
brew cleanup
```
