# macOS Development Environment Setup

Clean macOS setup guide for embedded systems and IoT development.
Part of the [linux-embedded](.) foundation layer.

---


## MacOs Clean Setup
**Açıklama:** Yazılım geliştirme ilgilenmek ugraşmak ve hatalarla ugraşmak istemiyorsanız. İlk önce işletim sisteminizi ingilizce kurun. 
<img src="https://developer.apple.com/macos/images/lockup-hero-large_2x.png" >

### Route Steps : Mac Settings 

- [OS Settings](#os-settings)
  - [Basic](#basic-settings)
  - [Dock Speed Up](#dock-speed-up-settings)
  - [Dock](#dock)
  - [Desktop](#desktop)
  - [Finder](#finder)
  - [Home Brew PATH Settings](#home-brew-path-settings)
  - [Homebrew Update](#homebrew-update)
  - [Git Setup](#git-setup)
    - [Github SSH Setup](#github-ssh-setup)
    - [Git Configuration](#git-configuration)
  - [App List](#app-list)
  - [Iterm 2 Details](#iterm-2-details)
  - [Oh MY Zsh Setup](#oh-my-zsh-setup)
  - [Powerlevel10k Theme Setup](#powerlevel10k-theme-setup)
  - [ZSH Eklentileri](#zsh-eklentileri)


## OS Settings

These are my preferred settings for `Desktop`, `Finder` and the `Dock`.

### Basic Settings

* Accessibility
  * Display
    * Reduce Transparrency -> Yes
    
* Trackpad
  * Scrool & Zoom -> Natural Scrolling -> No 

### Dock Speed Up Settings

- Speed Up
```sh
defaults write com.apple.dock autohide-delay -float 0
killall Dock
```

- Revert Default Value
```sh
defaults delete com.apple.dock autohide-delay
killall Dock
```
### Dock

I don't use the Dock at all. It takes up screen space, and I can use RayCast to launch apps and AltTab to switch between apps. I make the dock as small as possible and auto hide it.

* System Preferences
  * Desktop & Dock
    * Size -> Small as possible
    * Position on screen -> Left
    * Automatically hide and show the Dock -> Yes
    * Animate opening applications -> No
    * Show suggested and recent apps in the Dock -> No

### Desktop

I don't like the new Desktop, Stage Manager or Widget features in Sonoma, so I disable them.

* System Preferences
  * Desktop & Dock
    * Desktop & Stage Manager
      * Show Items
        * On Desktop -> check
        * In Stage Manager -> check
      * Click wallpaper to reveal desktop -> Only in Stage Manager
      * Stage Manager -> uncheck
      * Widgets
        * On Desktop -> uncheck
        * In Stage Manager -> uncheck

### Finder

* Finder -> Preferences
  * General -> Show these on the desktop -> Select None
      * I try to keep my desktop completely clean.
  * General -> New Finder windows show -> Home Folder
      * I prefer to see my home folder in each new finder window instead of recent documents
  * Advanced -> Show all filename extensions -> Yes
  * Advanced -> Show warning before changing an extension -> No
  * Advanced -> When performing a search -> Search the current folder
* View
  * Show Status Bar
  * Show Path Bar
  * Show Tab Bar

### Homebrew Setup
<img src="https://brew.sh/assets/img/homebrew.svg" align="left" height="120" width="120">


Öncelikle Homebrew'u kurmanız gerekiyor. Terminal'i açın ve aşağıdaki komutu girin:

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
<br>

### Home Brew PATH Settings
Homebrew kurulumundan sonra, PATH değişkeninizi güncellemeniz gerekebilir. Terminal ekrarınında da aynı kod bulunacaktır. Öncelik onu yazınız. 
Bulamazsanız eğer aşağıdaki komutu girin:

```sh
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

### Homebrew Update
Homebrew'u güncelleyin ve hazır hale getirin:

```sh
brew update
brew upgrade
```

#### Details Git Setup

<img src="https://avatars.githubusercontent.com/u/18133?s=200&v=4" align="left" height="120" width="120">
<br>

```sh
brew install git
```
<br>

##### Github SSH Setup
* Follow [this guide](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) to setup an ssh key for github
* Follow [this guide](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account) to add the ssh key to your github account

##### Git Configuration
GitHub hesabınızla Git'i yapılandırmak için aşağıdaki komutları kullanın, `<your-name>` ve `<your-email>` alanlarını kendi bilgilerinizle değiştirin:

```sh
git config --global user.name "your-name"
git config --global user.email "your-email"
```

##### SSH Key Creating
Eğer GitHub hesabınızda SSH anahtarı kullanmak istiyorsanız, aşağıdaki komutu kullanarak yeni bir SSH anahtarı oluşturun:

```sh
ssh-keygen -t ed25519 -C "your-email"
```

Komut size birkaç soru soracaktır; hepsini varsayılan olarak bırakabilirsiniz.

##### Adding SSH Key In GitHub Account

Oluşturulan SSH anahtarını GitHub hesabınıza eklemek için aşağıdaki adımları izleyin:

- Terminal'de aşağıdaki komutu çalıştırarak SSH anahtarınızı kopyalayın:
  ```sh
  pbcopy < ~/.ssh/id_ed25519.pub
  ```
- GitHub'da [SSH ve GPG anahtarları](https://github.com/settings/keys) sayfasına gidin.
- "New SSH key" butonuna tıklayın.
- Anahtarınızı "Title" kutusuna yapıştırın ve ardından "Add SSH key" butonuna tıklayın.

##### SSH Connetion Test
SSH bağlantısının doğru yapılandırıldığını test etmek için aşağıdaki komutu çalıştırın:

```sh
ssh -T git@github.com
```

Eğer her şey doğru yapılandırılmışsa, bir hoş geldiniz mesajı alırsınız.
Eğer git push origin main kısmında hata alırsanız. 

- Github -> Settings -> Personel Token (Classic)-> Add name and Expiration Date -> Check All -> Generate Token
  Terminal in push folder. Write this code. 
  - git remote set-url origin https://Your-token-number-paste-it@github.com/username/reponame

##### System Repair And Clean
Son olarak, sistemdeki gereksiz dosyaları temizlemek için Homebrew'un temizlik komutunu çalıştırabilirsiniz:

```sh
brew cleanup
```

### App List
List make based on homebrew, others apps linked. 


|          APP NAME           |    INSTALL COMMAND      |          DESCIRIPTION                        |
|-----------------------------|-------------------------|----------------------------------------------|
|Source Tree Setup|brew install --cask sourcetree| Git arayüz kullanımı için ek program|
|Python Setup|brew install python|Python programa dilinin kurulumu.|
||python3 --version pip3 --version|Python ve pip’in yüklü olduğunu doğrulamak için aşağıdaki komutları çalıştırın:|
||python3 -m venv venv|Proje dizinine gidin ve aşağıdaki komutu çalıştırarak sanal bir ortam oluşturun |
||source venv/bin/activate|Sanal ortam etkinleştirildiğinde, komut satırında (venv) etiketi görünecektir|
|Flask Setup|pip install Flask|Flask ve diğer gerekli paketleri yükleyin|
|Node.js Setup|brew install node|Node js kurulumu.|
|R Setup|brew install r|R Programlama dilinin kurulumu.|
|Julia Setup|brew install --cask julia|Julia Programlama dilinin kurulumu|
|Visual S Code Setup|brew install --cask visual-studio-code|VSC IDE kurulumu.|
||Terminal.integrated.font -> 'FNTNME'|VS üzerindeki terminalde ohmyzhs eklentilerini ve görsellerini çalıştırma|
|Docker Setup|brew install --cask docker|Konteyner yoneticisi|
|Java Setup|brew install openjdk|JavaKurulum da terminal arasında 2 adet path isteyen kod var|
|Brave Browser Setup|brew install --cask brave-browser|Brave Web Tarayıcı kurulumu|
|Rain Drop IO Setup|brew install --cask raindropio|Bookmark ve not alma uygulaması.|
|Spotify Setup|brew install --cask spotify|Müzik çalar kurulumu|
|WhatsApp  Setup|brew install --cask whatsapp|Telefonla masaüstü mesajlaşma yardımcısı kurulumu.|
|Zoom Setup|brew install --cask zoom|Zoom desktop kurulumu.|
|Slack Setup|brew install --cask slack|Slack mesajlaşma uygulaması kurulumu.|
|Discord Setup|brew install --cask discord|Discord mesajlaşma uygulaması kurulumu.|
|Xcode Setup|brew install --cask xcodes|Xcode farklı versiyonları indirme aralarında geçiş yapmayı sağlayan. |
|SF-Symbols Setup|brew install --cask sf-symbols|Xcode kullanılmak için kullanılan bir sembol kütüphanesi. |
|FreeDownload  Setup|brew install --cask free-download-manager|Dosya indirme yöneticisi, torrent dosyalarını da indirmeyi destekliyor. |
|Alt Tab Setup|brew install --cask alt-tab|Windows tarzı uygulamalar arası geçisi destekleyen bir eklenti. |
|Mac Mouse Fix|brew install --cask mac-mouse-fix|Mac Mause ayarlarını değiştirebildiğimiz eklenti  yardımcı program.|
|Al Dente Setup|brew install --cask aldente|Laptoplar için bir batarya yöneticisi programı. |
|Obs Setup|brew install --cask obs|Canlı yayın veya kayıt yapılabilecek bir kayıt programı.|
|KDE Live Setup|brew install --cask kdenlive|Video düzenleme programı. |
|Audacity Setup|brew install --cask audacity|Ses düzenleme programı. |
|Libre Office Setup|brew install --cask libreoffice|Ofis programları kurulumu.|
|Android Trasfer Setup|brew install --cask android-file-transfer|Android  mac üzerine bağlamayı sağlayan bir eklenti programı. |
|Keeping Awake Setup|brew install --cask keepingyouawake|Bilgisayarın uykuya geçmesini engelleyen bir eklenti. |
|Rectangle Setup|brew install --cask rectangle|Pencere yöneticisi. |
|VLC Player Setup|brew install --cask vlc|Video izleme programı .|
|Keka Setup|brew install --cask keka|Zip programı dosyaları sıkıştırabilir ve açabilir. |
|Kap Screen Recor |brew install --cask kap|Masa üstü ekran kayıt programı.|
|VMwareFusion Setup|brew install --cask vmware-fusion|Sanal işletim sistemi arayüz programı. |
|Bitwarden Setup|brew install bitwarden|Şifre yöneticisi. |
|Figma Setup|brew install --cask figma|Tasaramı ve protatip oluşturma aracı. |
|Steam Setup|brew install --cask steam|Oyun kütüphanesi. |
|EpicGames Setup|brew install --cask epic-games|Oyun kütüphanesi. |
|Key Castr Setup|brew install --cask keycastr|Klavyeden basılan tuşu gösteren program. |
|Bettir Display Setup|brew install --cask betterdisplay|Harici ekranların kontrolü için program. |
|Hand Mirror Setup|https://apps.apple.com/us/app/hand-mirror/id1502839586?mt=12|Toplantı öncesi kamera görselini kontrol etme eklenti aracı. |
|One Thing|https://apps.apple.com/us/app/one-thing/id1604176982?mt=12|Hedef belirleme konsantirasyon eklentisi.|
|iTerm2 Terminal Setup|brew install --cask iterm2|Mac terminale alternatif|



### Iterm 2 Details 

I prefer [iTerm2](https://iterm2.com/) because:
* Lots of customization options
* Clickable links
* Native OS notifications

Once installed, launch it and customize the settings / preferences to your liking. These are my preferred settings:


* [Fonts, What you wish.](https://www.nerdfonts.com/font-downloads)
* Appearance
  * Theme
    * Minimal
    * Tabbar Location -> Bottom
    * Status Bar Location -> Bottom
* Profiles
  * Default
      * General -> Working Directory -> Reuse previous session's directory
      * Color Presets -> High Contrast
      * Text -> Font -> Your Choise From NerdFonts for icons.
      * Keys -> Key Mappings -> Presets -> Natural Text Editing
      * Window -> Style -> Full Height Rigth of Screen
* Keys 
  * Hotkey
      * Show / Hide all windows hotkey -> Checked
      * Setup Hotkey


#### Oh My Zsh Setup
<img src="https://upload.wikimedia.org/wikipedia/commons/1/1e/Oh_My_Zsh_logo.png" align="left" height="120" width="200">

Oh My Zsh'ı kurmak için aşağıdaki komutu çalıştırın:

```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
<br>

- iTerm2 ile Zsh ve Oh My Zsh Kullanımı
iTerm2'yi açın ve Zsh'in varsayılan kabuk olarak kullanıldığını doğrulayın. Eğer doğru şekilde kurulduysa, Oh My Zsh temasını ve eklentilerini kullanmaya başlayabilirsiniz.

#### Powerlevel10k Theme Setup
**1. Powerlevel10k'yı Klonlayın:**

```sh
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

**2. Oh My Zsh Temasını Değiştirin:**

`~/.zshrc` dosyasını açın ve `ZSH_THEME` satırını aşağıdaki gibi değiştirin:

```sh
ZSH_THEME="powerlevel10k/powerlevel10k"
```

**3. Değişiklikleri Uygulama**

Zsh kabuğunu yeniden yükleyin:

```sh
source ~/.zshrc
```
- Powerlevel10k Yapılandırma Sihirbazını Çalıştırma : Terminalinizi yeniden başlattığınızda veya Zsh'ı yeniden yüklediğinizde, Powerlevel10k yapılandırma sihirbazı otomatik olarak başlayacaktır. Bu sihirbaz, temayı özelleştirmenize ve tercihlerinizi ayarlamanıza yardımcı olacaktır. Sihirbazı adım adım takip ederek kişisel tercihlerinizi belirleyin.

#### ZSH Eklentileri

- Eklenti Dizinine Göz Atma : Öncelikle, Oh My Zsh'ın varsayılan olarak hangi eklentileri sağladığını görmek için eklenti dizinine göz atabilirsiniz. Bu dizin genellikle `~/.oh-my-zsh/plugins` konumunda bulunur.

```sh
ls ~/.oh-my-zsh/plugins
```

- Eklenti Eklemek : Eklentileri etkinleştirmek için `~/.zshrc` dosyasını düzenlemeniz gerekiyor. Terminalde aşağıdaki komutu kullanarak `~/.zshrc` dosyasını açın:

Open VS code in ~/.zshrc location. 

or 

```sh
nano ~/.zshrc
```

Eklentileri etkinleştirmek için `plugins` dizisini bulun. Örneğin, `git` ve `z` eklentilerini etkinleştirmek için `plugins` dizisini aşağıdaki gibi düzenleyin:

```sh
plugins=(git z)
```

- Yeni Eklentiler Kurma : Varsayılan olarak sağlanmayan bir eklentiyi kurmak için, ilgili eklentiyi `~/.oh-my-zsh/custom/plugins` dizinine klonlamanız gerekir.

Örneğin, `zsh-syntax-highlighting` eklentisini kurmak için:

```sh
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ~/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting
```

Kurulumdan sonra, `~/.zshrc` dosyasını açın ve yeni eklentiyi `plugins` dizisine ekleyin:

```sh
plugins=(git zsh-syntax-highlighting)
```

- Değişiklikleri Uygulama : `~/.zshrc` dosyasındaki değişiklikleri uygulamak için Zsh kabuğunu yeniden yükleyin:

```sh
source ~/.zshrc
```

### 1. zsh-autosuggestions
**Açıklama:** Önceki komutlarınızdan otomatik tamamlama önerileri sunar.
**Kurulum:**
```sh
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```
**~/.zshrc dosyasına ekleyin:**
```sh
plugins=(zsh-autosuggestions)
```

### 2. zsh-syntax-highlighting
**Açıklama:** Komutları yazarken sözdizimi vurgulaması sağlar.
**Kurulum:**
```sh
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```
**~/.zshrc dosyasına ekleyin:**
```sh
plugins=(zsh-syntax-highlighting)
```

### 3. zsh-completions
**Açıklama:** Zsh için ek tamamlama komutları sağlar.
**Kurulum:**
```sh
git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-completions
```
**~/.zshrc dosyasına ekleyin:**
```sh
plugins=(zsh-completions)
```

### 4. z
**Açıklama:** Sık kullanılan dizinlere hızlıca gitmeyi sağlar.
**Kurulum:**
```sh
brew install z
```
**~/.zshrc dosyasına ekleyin:**
```sh
plugins=(z)
```

### 5. git
**Açıklama:** Git komutları için yardımcı fonksiyonlar ve kısayollar sağlar.
**~/.zshrc dosyasına ekleyin:**
```sh
plugins=(git)
```

### 6. zsh-history-substring-search
**Açıklama:** Komut geçmişinde alt dize araması yapmanızı sağlar.
**Kurulum:**
```sh
git clone https://github.com/zsh-users/zsh-history-substring-search ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-history-substring-search
```
**~/.zshrc dosyasına ekleyin:**
```sh
plugins=(zsh-history-substring-search)
```

### 7. Colorls
**Açıklama:** Listeleri klasörlü simgelerle göstermeye yarar. 
**Kurulum:**
```sh
sudo gem install colorls
```
**~/.zshrc dosyasınnın sonuna ekleyin:**
```sh
alias ls='colorls'
```

### 8. zsh-interactive-cd
**Açıklama:** Etkileşimli dizin değiştirme işlemlerini kolaylaştırır.
**Kurulum:**
```sh
git clone https://github.com/changyuheng/zsh-interactive-cd ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-interactive-cd
```
**~/.zshrc dosyasına ekleyin:**
```sh
plugins=(zsh-interactive-cd)
```

### 9. zsh-docker-aliases
**Açıklama:** Docker komutları için kısayollar sağlar.
**Kurulum:**
```sh
git clone https://github.com/felixr/docker-zsh ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/docker
```
**~/.zshrc dosyasına ekleyin:**
```sh
plugins=(docker)
```

### 10. autojump
**Açıklama:** Sık kullanılan dizinlere hızlıca gitmeyi sağlar.
**Kurulum:**
```sh
brew install autojump
```
**~/.zshrc dosyasına ekleyin:**
```sh
plugins=(autojump)
```

#### System Repair And Clean
Son olarak, sistemdeki gereksiz dosyaları temizlemek için Homebrew'un temizlik komutunu çalıştırabilirsiniz:

```sh
brew cleanup
```