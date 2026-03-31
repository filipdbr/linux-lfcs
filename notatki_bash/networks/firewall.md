# Notatki: Firewall w systemie Linux

## 1. UFW (Uncomplicated Firewall) - Standard Ubuntu
Narzędzie stworzone, aby uprościć zarządzanie regułami bez znajomości skomplikowanej składni iptables.

### Podstawowe komendy:
- `sudo ufw status` – Sprawdzenie stanu (active/inactive).
- `sudo ufw status numbered` – Wyświetlenie reguł z numerami (kluczowe do usuwania).
- `sudo ufw enable` / `disable` – Włączenie lub wyłączenie firewalla.
- `sudo ufw default deny incoming` – Standardowa polityka: blokuj wszystko co wchodzi.
- `sudo ufw default allow outgoing` – Standardowa polityka: pozwól na wszystko co wychodzi.

### Zarządzanie regułami:
- `sudo ufw allow 22/tcp` – Otwarcie portu SSH (tylko TCP).
- `sudo ufw allow http` – Otwarcie portu na podstawie nazwy usługi z `/etc/services`.
- `sudo ufw deny from 192.168.1.50` – Całkowite zablokowanie konkretnego IP.
- `sudo ufw delete [numer]` – Usunięcie reguły o danym numerze z listy `status numbered`.

## 2. Firewalld - Standard RHEL/CentOS
Działa w oparciu o "strefy" (zones), co pozwala na różne poziomy zaufania dla różnych kart sieciowych.

### Podstawowe komendy (firewall-cmd):
- `firewall-cmd --state` – Sprawdzenie czy działa.
- `firewall-cmd --get-active-zones` – Sprawdzenie, która karta jest w której strefie.
- `firewall-cmd --add-port=80/tcp --permanent` – Dodanie portu na stałe.
- `firewall-cmd --reload` – Przeładowanie konfiguracji (wymagane po użyciu `--permanent`).

## 3. Kluczowe pliki i katalogi

- **/etc/default/ufw** – Główna konfiguracja UFW (polityki domyślne, obsługa IPv6).
- **/etc/ufw/** – Katalog z plikami reguł (pliki .rules).
- **/lib/systemd/system/ufw.service** – Plik jednostki systemd zarządzający startem usługi.
- **/var/log/ufw.log** – Miejsce, gdzie firewall zapisuje informacje o zablokowanych pakietach (jeśli logowanie jest włączone).
- **/etc/services** – Plik, z którego firewall bierze nazwy portów (np. że 'ssh' to port 22).

## 4. Koncepcja "Stateful Firewall"
Nowoczesne firewalle w Linuxie pamiętają stan połączenia. Jeśli Ty wyślesz zapytanie do Google (ruch wychodzący), firewall automatycznie wpuści odpowiedź (ruch przychodzący), mimo że masz ustawione `deny incoming`.