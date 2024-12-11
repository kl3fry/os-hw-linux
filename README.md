# GNU/Linux instalācija
##### Autors: Arvīds Bākulis
##### Studenta apliecības nummurs: ab24166

## Ievads

Šajā darbā es instalēšu **Arch Linux** un konfigurēšu to līdzīgi tam, ko lietoju pats.

Markdown avota failus, konfigurācijas failus un visu pārējo varēs atrast darba [Github repozitorijā](https://github.com/kl3fry/os-hw-linux/)

### Kāpēc Arch?

- Arch ir brīnišķīgs **GNU/Linux** distributīvs, ko pats no dienas dienā lietoju visuos savos datoros.
- Instalējot (bez **archinstall**) un lietojot to, nāksies daudz uzzināt par to kā operētājsistēma īsti strādā "zem pārsega".
- Tas māca lietojotāju pašam risināt problēmas, meklēt to risinājumus un visgalvenākais - lasīt dokumentāciju.
- Un pretī visam iepriekšminētajam tas sniedz plašas pielāgošanas iespējas un lietotāja brīvību.

### Sistēma instalācijai:

- QEMU/KVM virtuālā mašīna ar libvirt un VirtManager frontend-u.
    - Virtuālai mašīnai tiks atvēlēta sava fiziskā grafikas karte **RX 6700 XT***.
    - Video kartei ir pieslēgts savs monitors tāpēc tāds būs ari virtuālai mašīnai. 
    - Perefērijas ierīces (pele, klaviatūra) virtuālai mašīnai pieslēgtas kā **evdev** ierīces lai tās viegli varētu pārslēgt starp **host**** un **guest**** sistēmām.

###### * Vairāk par GPU passthrough: https://wiki.archlinux.org/title/PCI_passthrough_via_OVMF https://wiki.archlinux.org/title/QEMU/Guest_graphics_acceleration
###### ** host - uz tīriem, fiziskiem "dzelžiem" instalētā sistēma; guest - virtuālā mašīna

### Par instalāciju:

- Instalēšu salīdzināmi parastu mūsdienu Arch instalāciju:
    - Ar **Hyprland** kā "grafisko vidi", konfigurēsu to.
    - Instalēšu vēl dažas programmas lai pilnviedotu lietotāja saskarni un konfigurēšu arī tās.
    - Instalēšu pārējās uzdevumā prasītās programmas.
    - Instalēšu kādu spēli (kas būs daudz vienkāršāk, jo virtuālai mašīnai ir atvēlēta sava fiziskā karte un nav jāmocās ar grafikas virtualizāciju)

- Lielākā daļa instalācijai nepiecišamo zināšanu un resursu ir atrodama/ņemta no wiki.archlinux.org

###### Sakot godīgi, no sākuma gribēju instalēt ko interesantāku kā **Gentoo** vai vispār **Linux From Scratch**, bet kā parasti - darbam ķēros pēdēja minūtē un laika pat tīri kompilācijai nepietiktu, tādēļ instalēju to, ar ko pats esmu pazīstams 🙃.

## Instalācijas vides iegūšana

- Arch Linux instalācijai videi izmantošu oficiālo `.iso` failu no oficiālās mājaslapas:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/archweb.png)

## Virtuālās mašīnas izveidošana un konfigurēšana

- Ar VirtManager palīdzību izveidoju jaunu QEMU/KVM virtuālo mašīnu ar lejupielādēto `.iso` failu:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/virtman1.png)

- Izveidoju 128 GB lielu virtuālās mašīnas glabātuvi:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/virtman2.png)

- Neaizmirstu nomainīt `Firmware` uz `UEFI`

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/virtman3.png)

- Katram gadījumam uzlieku pareizu CPU topoloģiju:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/virtman4.png)

- Viruālai mašīnai atvēlu 8 GB RAM:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/virtman5.png)

- Pievienoju `evdev` ievadu lai viegli pārslēgt klavitatūru un peli starp **host** un **guest** sistēmām:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/virtman6.png)

- Beidzot spiežu `Begin Installation` un sāku instalāciju.

## Instalācija

- Nedaudz pagaidu un instalācijas vide ir startējusi:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/intro.png)

- Pirmais ko izdaru - pārbaudu vai ir interneta savienojums:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/ping.png)

- Viss ir kārtībā un varu turpināt.

- Nākamais ko darīšu - sadalīšu un sagatavošu viruālo disku instalācijai. Tam izmantošu `cfdisk`:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/part.png)

- Izveidoju tādas sadaļas:
    - 1 GB boot
    - 8 GB swap
    - pārējais - pārējam

- Saglabāju, un izeju:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/yes.png)

- Tālāk formatēju visas sadaļās:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/format.png)

- Tagad pievienoju(mount) visas izveidotās sadaļas instalācijas vides `/mnt` direktorijā un "ieslēdzu" swap sadaļu (ne bez kļūdām 🥲):

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/mount.png)

- Ar `lsblk` pārliecinos ka viss ir pareizi:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/lsblk.png)

- Tagad ir laiks instalēt nepieciešamās pakotnes janajā failu sistēmā ar `pacstrap` komandu:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/pacstrap3.png)

- Īsi par dažām no tām (Daudzi pēc archlinux.org/packages aprkasta):
    - `base` - Minimālā pakotne, lai definētu pamata Arch Linux instalāciju
    - `linux` - pats Linux kodols
    - `linux-firmware` - Programmaparatūras faili priekš Linux
    - `base-devel` - Pamata rīki Arch Linux pakotņu izveidei
    - `git` - Versiju kontroles sistēma (būs vajadzīgs lai dabūtu failus no git repozitorijām)
    - `neovim` - failu redaktors
    - `networkmanager` - Tīkla savienojumu pārvaldnieks
    - `grub` - GNU GRand Unified Bootloader
    - `efibootmgr` - Lietotāja telpas programma, lai modificētu EFI sāknēšanas pārvaldnieku (vajadzīgs GRUB instalācijai)

- Pēc nelielas gaidīšanas pakotnes ir instalētas un es varu turpināt:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/pacstrapfin.png)

- Pirms darboties ar jauno sistēmu ir jāģenerē `fstab` fails, kas ir lietots lai noteiktu, kā diska nodalījumi jāpievieno failu sistēmā:

- Tam lietošu `genfstab -U /mnt >> /mnt/etc/fstab` komandu kur:
    - `genfstab -U /mnt` - ģenerē `fstab` failu `/mnt` direktorijai
    - `>> /mnt/etc/fstab` - ieraksta pirmās komadas rezultātu sekojošajā direktorijā

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/fstab.png)

- Tagad ir jānomaina saknes direktorija uz jaunizveidoto sistēmu (`chroot`) ar `arch-chroot` komandu:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/chroot.png)

- Jaunajā sistēmā ir janorāda laika zona ar komandu `ln -sf /usr/zoneinfo/Europe/Riga /etc/localtime` kur:
    - `ln -s` veido simbolisko saitni no:
        - `/usr/zoneinfo/Europe/Riga` (mana laika zona)
        - uz `/etc/localtime`
- Un pārliecinos ka viss ir pareizi ar `date`:
- Kā arī izpildu `hwclock --systohc`

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/date.png)

- Tālāk instalēju lokalizācijas, atveru `/etc/locale.gen` ar `neovim` ur noņemu `#` no rindas ar `en_US.UTF-8 UTF-8`:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/locale.png)

- Un izmantoju `locale-gen`

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/locale-gen.png)

- Tagad sistēmai ir jādod vārds (hostname) kas šajā gadījumā vienkārši būs "archbox". Tas būs jāieraksta `/etc/hostname` failā ar sekojošo komandu:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/hostname.png)

- Pirms turpināsim - uzstādīsu root lietotājam parolu un izveidošu parasto lietotāju un tam arī iedošu paroli (ļoti grūtu no 3 cipariem):

- Izveidoju savu lietotāju ar `useradd -G wheel -m -s /bin/bash arvids_bakulis -U` kur:
    - `-G wheel` pievieno jauno lietotāju `wheel` grupai kas kontrolē piekļuvi administratīvām privilēģijām.
    - `-m` - izveido "mājas" direktoriju lietotājam
    - `-s /bin/bash` - nosaka noklusējuma "čaulu" lietotājam (šajā gadījumā Bash)
    - `arvids_bakulis` - nosaka lietotāja nosaukumu (tas esmu es 😇)
    - `-U` - izveido lietotāja grupu ar tādu pašu vārdu kā lietotājam (ir noderīgs)

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/useradd.png)

- Tagad jāveic dažas redakcijas lietotāju "noteikumos" `/etc/sudoers` failā, tam lietošu komandu `EDITOR=nvim visudo` kur `EDITOR=nvim` nosaka vides mainīgo `EDITOR` kā neovim redaktoru:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/visudo.png)

- Noņemu komentāru no `%wheel ALL=(ALL:ALL) ALL` rindas un faila beigās pielieku `Defaults rootpw`. Tādā veidā es nosaku ka mans lietotājs varēs izmantot jebkuru komandu un izmantojot `sudo` komandu sistēma vienmēr prasīs `root` lietotāja paroli:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/visudoedit.png)

- Saglabāju un izeju.

### Svarīga daļa - Bootloader instalācija:

- Lai instalētu GRUB `/boot` sadaļa ko agrāk izveidoju un izveidot tā konfigurācijas failu izmantošu sekojošās komandas:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/grub.png)

- Pirms pārstartēšanas ir vērts ieslēgt NetworkManager systemd servisu ar komandu `systemctl enable NetworkManager.service`:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/netman.png)

- Tagad var iet ārā no chroot vides ar `exit` komandu un pārstartēt virtuālo mašīnu jauninstalētajā Arch Linux sistēmā ar `reboot` komandu.

- Pēc pārstartēšanas es redzu savu jauno un svaigo sistēmu 🎉🎊🥳:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/login.png)

- Palika tikai ievadīt savu lietotājvārdu un paroli: 

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/loginin.png)

- Sistēma ir instalēta bet pagaidām viss ko varam redzēt ir melns ekrāns ar tekstu:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/neofetch1.png)

- Lai dabūtu ierasto personālā datora vidi būs jāinstalē vēl dažas pakotnes ko darīšū nākamajā sadaļā.

## Sistēmas konfigurēšana un nepieciešamo programmu instalēšana

- Linux(un ne tikai) sistēmām ir piejamas daudz dažadas gatavas vizūālās lietotājvides (Desktop Enviroment). Piemēram: GNOME, KDE Plasma, Cinnamon, MATE, Xfce un citi.

*Bet mani tādi prasti risinājumi neinteresē* 😎.

- Es izmantošu savrupu logu pārvaldnieku (Window Manager) un kustēšos no tā punkta instalējot visu nepieciešamo pats.

- Mūsdienās ir piejami Logu Pārvaldnieki uz divu dažādu displeja servera protokolu "bāzēm":
    - **Xorg** - Arhaisks, novecojis un vienkārši slikts 🙃. Bet vēljoprojām ļoti populārs un plaši atbalstīts.

    - **Wayland** - Jauns, mūsdienīgs un kopumā labāks un strauji aug popularitātē, bet sava jaunuma dēļ dažos momentos vēl nav tik plaši atbalstīts.
        - Wayland adoptēšanas progresu var skatīt šajā mājas lapā: https://arewewaylandyet.com/

*Es izvēlos **Wayland***.

- Mūsdienās ir piejami daudz dažādi Wayland kompositori. Piemēram: Sway, River, dwl, labwc, Qtile un pēdējais, bet ne mazāk svarīgs - Hyprland.

*Es izmantoju **Hyprland***.

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/hyprland.jpg)

- Lai instalētu Hyprland savā svaigajā sistēmā man pietiks ar vienu komandu `sudo pacman -S hyprland`: 

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/hyprlandinstal.png)

- Nedaudz pagaidu un viss ir gatavs:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/hyprlandinstal1.png)

- Pirmas turpināšu instalēšu termināļa emulātoru **Kitty** lai būtu ko lietot iekšā **Hyprland** vidē:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/kittyinstall.png)

- Var mēģināt startēt Hyprland izpildot `Hyprland` komandu:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/hypr1.png)

- Hyprland atveras veiksmīgi bet teksta vietā ir redzami tikai tukši kvadrāti. Lai to izlabot - instalēju `nerd-fonts` pakotni un mēģinu vēlreiz.
- Bet pirms tam es pārstartēju virtuālo mašīnu un pieslēdzu tai GPU. Kā arī instalēju `grim` un `slurp` pakotnes lai es varētu ņem ekrānattēlus, lai kombinētu šos divus rīkus izmantošu savu nelielo Bash programmu kam nepiecišama arī `jq` un `wl-copy`.
- Kā arī instalēju `ranger` kā failu menedžeri.

- Pēc pārstartēšanas redzu, ka tagad fonti strādā kā vajag:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/hypr2.png)

- Laiks nedaudz parediģēt Hyprland konfigurācijas failu kas atrodas `~/.config/hypr/hyprland.conf` :
    - Nomainu ekrāna mērogu labākai redzamībai
    - Nomainu `autogenerated` uz `0` lai nebūtu brīdinājumu

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/hyprconf.png)

- Manā personīgā konfigurācijā ir daudz vairāk sīkāku izmaiņu un nianšu ko es tagad neskaršu, jo tas aizņemtu pārāk daudz laika un būtu nesaprātīgi, par tiem īsi pastāstīšu vēlāk šajā darbā. Tāpēc pagaidām vienkārši nokopēju to.

- Arī nokopēju **Kitty** konfigurācijas failus.

- Pēc konfigurācijas failu kopēšanas un jauka fona uzstādīšanas (kam bīja vajadzīgs instalēt `swaybg` un pievienot Hyprland konfigurācijas failā `exec-onece = swaybg -o bg.jpg`) es redzu daudz jaukāku rezultātu:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/rice1.png)

- Laiks konfigurēt klaviatūras valodas:

    - `input` sadaļā nomainu `kb_layout` uz `us, lv, ru` lai man būtu pieejamas Latviešu, Angļu un Krievu klaviatūra.

    - Kā arī pierakstu `kb_options = grp:rctl_toggle` lai noteiktu labo `CTRL` taustiņu valodas maiņai.

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/lang.png)

- Tagad var izmēģināt rakstīšanu dažādās valodās 🤓 :

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/lignux.png)

- Vai kaut-kas ko pats uzrakstīju:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/lignux2.png)

- Tagad var instalēt dažas noderīgas programmas:

    - **LibreOffice** - biroja programmatūras komplekts
    - **Chromium** - interneta pārlūks 
    - **Thunderbird** - e-pasta klients
    - **Discord** - komunikācijas programma **Zoom** vietā
    - **mpv** - atskaņotājs (instalēju vēlāk)
    - **Krita** - attēlu manipulācijas programma (instalēju vēlāk)

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/install1.png)

- Kā otru pārlūk programmu instalēšu **Librewolf**. Bet pirms tam būs jāinstalē `yay` - ***AUR** pakotņu pārvaldnieku.
    ###### *AUR - Arch User Repository: https://aur.archlinux.org/

- Lai to izdarītu ir jāizpilda komandas no oficiālās github lapas:
```bash 
sudo pacman -S --needed git base-devel
git clone https://aur.archlinux.org/yay-bin.git
cd yay-bin
makepkg -si
```
`git` un `base-devel` jau instalēju tāpēc pirmā komanda nav obligāta.

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/yay1.png)

- Sekojot Yay github lapas instrukcijai ir jāveic pirmās reizes iestatīšana: 

```bash 
yay -Y --gendb
yay -Syu --devel
yay -Y --devel --save
```

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/yay2.png)

- Tagad beidzot varu izmantot `yay -s librewolf-bin` lai instalēto otro interneta pārlūku (`-bin` lai nekompilētu visu pārlūku pašam):

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/wolfinstall.png)

- Gribēju atvērt jauninstalētās programma un pārliecināties ka tās strādā bet nav ar ko atvērt!
- Būs jāinstalē lietotņu palaidējs. Es izmantoju **Tofi**, tas ir piejams tika AUR repozitorijā tāpēc izmantoju Yay:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/tofiinstall.png)

- Tas instalējās veiksmīgi, tomēr man nepatīk tā izsakts tāpēc atkal pārkopēšu savus konfigurācijas failus:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/tofiold.png)

- Daudz labāk!

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/tofinew2.png)

- Pie viena instalēšu statusa joslu **Waybar** ar `sudo pacman -S waybar` un pievienošu `exec-once = waybar` savā **Hyprland** konfigurācijā kā arī pārkopēšu savus konfigurācijas failus arī tam:

- "No kastes" **Waybar** konfigurācija un izskats:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/waybarfresh.png)

- Mana konfigurācija:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/waybarnew.png)

- Kā arī instalēju **GCC** kompilātorus un **Python** interpretētāju ar `sudo pacman -S gcc python`, kuri protams jau bīja instalēti:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/proginstall.png)

### Tagad var pārbaudīt instalētās programmas:

- **Thunderbird** un e-pasta saņemšana:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/bird.png)

- **LibreOffice** un nedaudz teksta:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/office1.png)
![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/office2.png)

- **Chromium** pārlūks

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/chromium.png)

- **Librewolf** pārlūks:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/wolf.png)

- **mpv** atskaņotājs un froša varde:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/frogmpv.png)

- **Discord** komunikācijai:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/discord.png)

- **Krita** darbam ar attēliem:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/krita.png)

- Kā koda rediģēšanas programmu izmantoju **Neovim**, ko jau esmu instalējis. "no kastes" tā ir diezgan minimāla tāpēc es izmantoju tai daudzus plugin-us tāpēc kā arī citiem - pārkopēju savus konfigurācijas failus:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/nvim1.png)

- Uzrakstu divas parastas programmas abās valodās:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/cpp.png)
![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/python.png)

- Nomainu krāsu shēmu no **jellybeans** uz **zenburned** ar `:colorscheme zenburned` komandu:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/nvimtheme.png)

- Katram gadījumam atjauninu visu sistēmu ar `yay` (var arī lietot `sudo pacman -Syu` bet tas strādās tikai ar pacman instalētām pakotnēm):

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/yayupd.png)

## Spēles instalācija un demonstrācija:

- Es instalēšu kādu spēli no savas **Steam** bibliotēkas.

- Pirmais ko vajadzēs izdarīt - instalēt pašu **Steam**, to var izdarīt instalējot `steam` pakotni ar `sudo pacman -S steam` komandu:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/steamfail.png)

- Mēģinu to izdarīt, bet man nekas nesanāk, kas liecina par to, ka es aizmirsu ieslēgt **multilib** repozitoriju atbalstu priekš **pacman**, tāpēc to izdarīšu tagad.

- **multilib** satur 32 bitu programmatūru un bibliotēkas, ko var izmantot, lai palaistu un izveidotu 32 bitu lietojumprogrammas 64 bitu instalācijās (piem., Wine, Steam utt.).

- Lai ieslēgtu **multilib** atbalstu ir jāatver `/etc/pacman.conf` fails redaktorā ar `root` privilēģijām un jānoņem `#` no sekojošām rindām:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/multilib.png)

- Pēc tam ir vienkārši jāizpilda `sudo pacman -Syu`:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/multilibsyu.png)

- Šo segmentu rakstīju vēlāk nekā pārējo darbu, tāpēc ir uzkrājušies atjauninājumi kurus arī pie viena instalēju.

- Mēģinu instalēt vēlreiz un tagad viss sanāk!
- Kā **Vulkan** draiveri izvēlos `vulkan-radeon` jo tam ir labāka saderība:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/steamsucc.png)

- Tagad varu mēģināt vert vaļā **Steam**.
- Nedaudz pagaidu un tas ir vaļā:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/steamsign.png)

- Ienāku savā kontā un tagad varēšu lejupielādēt kādu spēli:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/steamin.png)

- Es izvēlos **"Half-Life"**, bet pirms instalēšanas ir iestatījumos jāieslēdz **Steam Play** lai varētu spēlēt Windows spēles:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/steamproton.png)

- Pārstartēju **Steam** un tagad var instalēt:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/steamhl.png)

- Nedaudz pagaidu un spēle ir instalēta, varu vērt vaļā:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/hl1.png)

- Izvēlos **"New Game"**:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/hl2.png)

- Izvēlos **"Training Room"** lai ātri parādīt, ka spēle strādā:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/hl3.png)

- Nedaudz paskraidu apkārt:

![](https://raw.githubusercontent.com/kl3fry/os-hw-linux/refs/heads/main/img/vm/hl4.png)

## Nobeigumā - īsi par maniem konfigurācijas failiem:
Kā jebkuram sevi cienošam Arch lietotājam - es pavadu vairāk laika savu sistēmu konfigurējot nekā lietojot tāpēc mani konfigurācijas faili atrodas pastāvīgā modifikācijas stāvoklī un šeit ir aprakstītas tā patreizējās iterācijas ko lietoju šajā darbā.

Šeit mēģināsu īsi pastātīt par svarīgākajiem momentiem.

Bet ja interesē plašaka informācija - laipni lūgti attiecīgos repozitorijos un dokumentācijās:

Pilno failu saites:

### Hyprland
Nav pilns fails, tikai interesantās un svarīgās lietas. 

[Pilno failu varēs apskatīt šeit](https://github.com/kl3fry/os-hw-linux/blob/main/dots/hypr/hyprland.conf)

- Monitoru konfigurācija. Nokomentēti, jo vajadzīgi tikai dažreiz.

```
#monitor = eDP-1, 1920x1080@120, 0x1440, 1
#monitor = eDP-1, 2880x1800@120, 0x1440, 2/
#monitor = , 2560x1440@60, 0x0, 1

#env = AQ_DRM_DEVICES,/dev/dri/card0 # Nosaka videokarti ko lietot
#monitor = HDMI-A-1, 2560x1440, 0x0, 1
```

- Šeit nosaku mainīgos ar programmām ko lietoju un izsaucu ar Hyprland

```
$terminal = kitty
$fileManager = kitty ranger
$menu = tofi-drun --drun-launch=true
```

- Šeit tiek noteikas programmas kas izpildās atverot Hyprland

```
exec-once = waybar & librewolf & swaybg -i bg.jpg # Atveru statusa joslu, pārlūku un fona bildi
exec-once = echo 40 >> /sys/class/backlight/amdgpu_bl1/brightness # Šeit pārslēdzu spilgtumu sava portatīvā datora ekrānam
```

- Vides mainīgie:

```
env = XCURSOR_SIZE,24
env = HYPRCURSOR_SIZE,32

env = HYPRCURSOR_THEME, rose-pine-hypercursor # Nosaka kursoru kā Rose-Pine
```

- Nedaudz izmainu noklusējuma izskatu:

```
general { 
    gaps_in = 0 # Pēc noklusējuma izslēdzu tukšās malas
    gaps_out = 0

    border_size = 3 # Nosaku apmales resnumu

    col.active_border = rgb(b65d54) # Aktīvā loga apmales ir sarkanas
    col.inactive_border = rgba(00000050) # Neaktīvo - caurspīdīgs melns

    resize_on_border = false 

    allow_tearing = false

    layout = dwindle
}

decoration {
    rounding = 0 # Man nepatīk logu kaktu noapaļošana. Izslēdzu to.

    active_opacity = 1.0
    inactive_opacity = 1.0

    #enabled = false # Izslēdzu logu ēnu 
    #shadow_range = 4
    #shadow_render_power = 3
    #col.shadow = rgba(1a1a1aee)

    dim_special = 0.4 # Speciālo darbavirsmu fona aptumšošna

    blur {
        enabled = true
        size = 7
        passes = 1
        
        vibrancy = 0.1696
        special = false
    }
}

# Ja uz darbavirsmas ir vairāk par 1 logu - tikai tad ieslēdz logu apmales
workspace = w[1], border:false, gapsin:0, gapsout:0
workspace = s[true], gapsout:35, gapsin:10
```

- Animācijas ir ieslēgtas, stāv pēc nokluējuma, bet es tās padarīju nedaudz ātrākas

```
animations {
    enabled = true 

    bezier = myBezier, 0.05, 0.9, 0.1, 1.05

    animation = windows, 1, 2, default
    animation = windowsOut, 1, 2, default, popin 80%
    animation = border, 1, 10, default
    animation = borderangle, 1, 8, default
    animation = fade, 1, 2, default
    animation = workspaces, 1, 2, default
}
```

- Ievada konfigurācija, par to jau stāstiju:

```
input {
    kb_layout = us, lv, ru
    kb_options = grp:rctrl_toggle

    # Nosaku taustiņu atkārtošanas ātrumu un biežumu kā ātrāku:
    repeat_rate = 35
    repeat_delay = 170

    follow_mouse = 1

    sensitivity = 0 # -1.0 - 1.0, 0 means no modification.

    touchpad {
        natural_scroll = true
        scroll_factor = 0.5
    }
}
```

- Taustiņu kombināciju konfigurācija:

```
$mainMod = SUPER # Nosaku "Windows" pogu kā "mainMod"


# Nosaku paroļu pārvaldniekam savu speciālo darbavirsmu:
bind = $mainMod, K, togglespecialworkspace, 󰌆
bind = $mainMod SHIFT, k, movetoworkspace, special:󰌆
workspace = special:󰌆, on-created-empty: keepassxc

# Pārslēdzu valodas ar z x a taustiņiem:
$KEYB = at-translated-set-2-keyboard
bind = $mainMod, z, exec, hyprctl switchxkblayout $KEYB 1 #en
bind = $mainMod, x, exec, hyprctl switchxkblayout $KEYB 2 #ru
bind = $mainMod, a, exec, hyprctl switchxkblayout $KEYB 3 #lv

# Dažu jau minēto programmu pārstartēšanas kombinācijas:
bind = $mainMod, b, exec, killall waybar && waybar
bind = $mainMod, n, exec, killall swaybg && swaybg -i bg.jpg

# Ekrānattēlu uzņemšanas taustiņi:
bind = $mainMod, s, exec, glorbscreen -p
bind = $mainMod SHIFT, s, exec, glorbscreen -pf
```

### Neovim konfigurācija:

Mana Neovim konfigurācija ir ļoti haotiska un neglīta tāpēc es tikai norādīšu spraudņu sarakstu, ko lietoju:

- Kā plugin pārvaldnieku izmantoju **Vim-Plug**.
    - To var instālēt priekš **Neovim** ar sekojošo komandu: 
        - `sh -c 'curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'`

- Pastāstīšu par dažiem no tiem (plašāk par tiem var atrast attiecīgās github.com repozitorijās):
```
local vim = vim
local Plug = vim.fn['plug#']

vim.call('plug#begin')
Plug 'romgrk/barbar.nvim' -- Cilņu josla (augšējā)
Plug 'nvim-lualine/lualine.nvim' -- Status josla (apakšējā)
Plug 'nvim-tree/nvim-tree.lua' -- Failu pārvaldnieks
Plug 'nvim-tree/nvim-web-devicons' 
Plug 'lukas-reineke/indent-blankline.nvim'
Plug 'RRethy/vim-illuminate'
Plug 'nvim-treesitter/nvim-treesitter'
Plug 'nvim-telescope/telescope.nvim'
Plug 'nvim-lua/plenary.nvim'
Plug 'nvim-telescope/telescope-fzf-native.nvim'
-- Krāsu shēmas --
Plug 'morhetz/gruvbox'
Plug 'nanotech/jellybeans.vim'
Plug 'rebelot/kanagawa.nvim'
Plug 'rafi/awesome-vim-colorschemes'
Plug "zenbones-theme/zenbones.nvim"
Plug "rktjmp/lush.nvim"
Plug "ntk148v/komau.vim"
Plug "ronisbr/nano-theme.nvim"
Plug "keitanakamura/neodark.vim"
Plug "vim-scripts/spacegray.vim"
Plug "teloe/bland.vim"
Plug "rayes0/blossom.vim"
-- Spraudņi LSP konfigurācijai un koda auto-pabeigšanai --
Plug 'neovim/nvim-lspconfig'
Plug 'hrsh7th/cmp-nvim-lsp'
Plug 'hrsh7th/cmp-buffer'
Plug 'hrsh7th/cmp-path'
Plug 'hrsh7th/cmp-cmdline'
Plug 'hrsh7th/nvim-cmp'
Plug 'hrsh7th/cmp-vsnip'
Plug 'hrsh7th/vim-vsnip'
Plug "williamboman/mason.nvim"
Plug "williamboman/mason-lspconfig.nvim"
Plug "elkowar/yuck.vim"
-- Markdown priekšskatījums --
Plug "toppair/peek.nvim"

vim.call('plug#end')
```

### Kitty termināļa konfiurācija:

[Pilni faili](https://github.com/kl3fry/os-hw-linux/tree/main/dots/kitty)

- Fonts un tā izmērs:
```
font_size 16.0
font_family JetBrainsMonoNL Nerd Font
```

- Pielāgoju loga izmērus un paradumus:
```
remember_window_size no 
initial_window_width 800
initial_window_height 600
window_padding_width 10
window_border_width 0pt
confirm_os_window_close 0
```

- Nosaku fona caurspīgīgumu un importēju krāsu shēmas:
```
background_opacity 0.95
include /home/arvids_bakulis/.config/kitty/Solarized_Darcula.conf
```

- Izslēdzās kaitinošas skaņas:
```
enable_audio_bell no
```

- Taustiņi burtu izmērā regulācijai:

```
map ctrl+minus change_font_size all -2.0
map ctrl+equal change_font_size all +2.0
```

### Tofi konfigurācija:

Mana konfigurācija ir balstītā no šīs: https://github.com/philj56/tofi/blob/master/themes/dmenu

[Pilns fails](https://github.com/kl3fry/os-hw-linux/blob/main/dots/tofi/theme)

- Norāda loga atrasšanās vietu un izmēru:
```
anchor = top
width = 100%
height = 20
horizontal = true
```

- Nosaka testu, fontu un tā izmēru:

```
prompt-text = " run: "
font-size = 8
font = JetBrainsMono Nerd Font Bold
```

- Nosaka krāsas:

```
background-color = #000000
selection-color = #8a9a65
text-color = #f9fbff
```

- Nosaka elementu izmērus iekšā logā:

```
min-input-width = 120
result-spacing = 15
```

- Noņem loga atstarpes un apmales:

```
padding-top = 0
padding-bottom = 0
padding-left = 0
padding-right = 0
outline-width = 0
border-width = 0
```

### Waybar konfigurācija:

Waybar tiek konfigurēts `.jsonc` un `.css` failos, kas arī ir diezgan gari, tāpēc atkal pastātīšu tikai par pašu svarīgāko.
[Pilnus failus varēs apskatīties šeit](https://github.com/kl3fry/os-hw-linux/tree/main/dots/waybar)

- Šeit tiek notiekti kādi moduļi tiks rādīti:

```
"modules-left": [
    "hyprland/workspaces",  /* Hyprland darbavirsmu modulis */
    "hyprland/special",  /* Hyprland speciālo darbavirsmu modulis */
    "hyprland/window"  /* Hyprland modulis kas rāda aktīvo logu */
],

"modules-center": [  /* Pavidu nekā nav */
],

"modules-right": [
    "privacy",  /* Modulis kas rāda kad tiek izmantoti mikrofons vai kamera */
    "battery",  /* Modulis kas rāda akumulatora uzlādi */
    "network",  /* Modulis kas rāda tīkla savienojuma statusu */
    "wireplumber",  /* Modulis kas rāda skaļumu un dod to regulēt */
    "cpu",  /* Modulis kas rāda CPU utilizāciju procentos */
    "memory",  /* Modulis kas rāda RAM utilizāciju procentos */
    "temperature",  /* Modulis kas rāda CPU temperatūru */
    "keyboard-state",
    "hyprland/language",  /* Modulis kas rāda izvēlēto valodu */
    "clock"  /* Modulis kas rāda laiku un datumu */
],
```

## Kursa vērtējums:

**A**. Cik daudz stāstīt par OS Windows?
***Vajadzēja mazāk***

**B**. Cik daudz stāstīt par OS Linux?
***Vajadzēja vairāk***

**C**. Cik daudz stāstīt par citām tēmām?
***Vajadzēja mazāk***

**D**. Cik daudz mājas darbu vajag semestra laikā?
***Pietiekami***

**E**. Ko vēl vajadzētu pastāstīt šinī kursā?
***Par GNU projektu Linux kontekstā***

**F**. Citi ieteikumi pasniedzējam:
***Īsti nav***
