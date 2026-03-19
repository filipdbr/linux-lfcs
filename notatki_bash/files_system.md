# Linux Filesystem Hierarchy & Sudo Guide

W Linuxie wszystko jest plikiem, a całe drzewo zaczyna się od korzenia: `/` (root).

## 1. Kluczowe katalogi systemowe

| Katalog | Nazwa/Znaczenie | Co tam znajdziesz? |
| :--- | :--- | :--- |
| `/bin` | Binaries | Podstawowe komendy dla wszystkich użytkowników (np. `ls`, `cp`). |
| `/sbin` | System Binaries | Komendy administracyjne (np. `fdisk`, `reboot`). Zazwyczaj wymagają `sudo`. |
| `/etc` | Et cetera | **Serce systemu.** Pliki konfiguracyjne (np. `/etc/passwd`, `/etc/ssh/`). |
| `/var` | Variable | Dane, które często się zmieniają. Logi (`/var/log`), bazy danych, poczta. |
| `/tmp` | Temporary | Pliki tymczasowe. Są **czyszczone przy restarcie** systemu! |
| `/home` | Home | Katalogi domowe użytkowników (np. `/home/filip`). Twoje dokumenty i ustawienia. |
| `/root` | Root Home | Katalog domowy superużytkownika (nie mylić z `/`). |
| `/run` | Runtime | Dane o działających procesach (np. pliki `.pid`). Znika po restarcie (RAM). |
| `/mnt` / `/media` | Mount/Media | Punkty montowania dla dysków zewnętrznych, pendrive'ów lub NFS. |
| `/usr` | User Resources | Programy użytkownika, dokumentacja, biblioteki (`/usr/bin`, `/usr/lib`). |
| `/proc` | Processes | Wirtualny system plików. Informacje o jądrze i procesach (widziane jako pliki). |



---

## 2. Gdzie i kiedy używać SUDO?

`sudo` (Substitute User Do) pozwala wykonać komendę z uprawnieniami innego użytkownika (domyślnie `root`).

### ✅ Używaj SUDO, gdy:
- **Zarządzasz pakietami:** `sudo apt install ...` lub `sudo dnf install ...`
- **Edytujesz konfigurację systemową:** `sudo nano /etc/ssh/sshd_config`
- **Zarządzasz serwisami:** `sudo systemctl restart cron`
- **Przeglądasz logi systemowe:** `sudo journalctl` (choć nie zawsze wymagane, daje pełny widok).
- **Zarządzasz użytkownikami i uprawnieniami:** `sudo useradd`, `sudo chmod` (na plikach systemowych).
- **Montujesz dyski:** `sudo mount /dev/sdb1 /mnt`

### ❌ NIE używaj SUDO, gdy:
- **Pracujesz w swoim `/home`:** Nie rób `sudo touch ~/test.txt` – plik będzie należał do roota i sam go potem nie usuniesz bez sudo!
- **Kompilujesz kod:** `make` powinno działać jako zwykły user, dopiero `sudo make install` wymaga uprawnień.
- **Przeglądasz internet/używasz przeglądarki:** To ogromne ryzyko bezpieczeństwa.

---

## 💡 LFCS Pro-Tip: Lokalizacja logów i konfów
Jeśli na egzaminie każą Ci "zmienić ustawienia sieci" lub "sprawdzić błędy autoryzacji", Twoje pierwsze kroki to:
1. Konfiguracja? Szukaj w `/etc/...`
2. Błędy/Logi? Szukaj w `/var/log/...` lub przez `journalctl`.