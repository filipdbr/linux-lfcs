# Notatki: Sieci w systemie Linux (iproute2)

## 1. Zarządzanie Interfejsami (Warstwa 2)
- `ip link show` - Wyświetla wszystkie interfejsy i ich adresy MAC.
- `sudo ip link set eth0 up` - Aktywuje interfejs (podnosi kartę).
- `sudo ip link set eth0 down` - Deaktywuje interfejs.
- `ip -s link show eth0` - Wyświetla statystyki karty (ile pakietów odebrano/wysłano, błędy).

## 2. Adresacja IP (Warstwa 3)
- `ip a` - Skrót od `ip address show`. Wyświetla adresy IP przypisane do kart.
- `sudo ip addr add 10.0.0.5/24 dev eth0` - Przypisuje adres statyczny.
- `sudo ip addr del 10.0.0.5/24 dev eth0` - Usuwa konkretny adres IP.
- `sudo ip addr flush dev eth0` - Usuwa wszystkie adresy IP z danego interfejsu.

## 3. Routing (Trasy)
- `ip route show` - Wyświetla tablicę routingu (gdzie płyną pakiety).
- `sudo ip route add default via 192.168.1.1` - Ustawia bramę domyślną (Gateway).
- `sudo ip route add 10.1.0.0/24 via 192.168.1.254` - Dodaje trasę do konkretnej podsieci.

## 4. Diagnostyka i Narzędzia
- `ping -c 4 8.8.8.8` - Sprawdza łączność (wysyła dokładnie 4 pakiety).
- `ss -tulpn` - Wyświetla otwarte porty i procesy, które z nich korzystają (następca `netstat`).
- `dig google.com` - Diagnozuje zapytania DNS (wymaga pakietu dnsutils).
- `ip neighbor` - Wyświetla tablicę ARP (powiązania IP z adresami MAC w sieci lokalnej).

## 5. Konfiguracja Trwała (Netplan - Ubuntu/Debian)
Pliki znajdują się w `/etc/netplan/*.yaml`.
- `sudo netplan generate` - Generuje konfigurację z plików YAML.
- `sudo netplan apply` - Nakłada nową konfigurację sieciową.
- `sudo netplan try` - Nakłada konfigurację z opcją automatycznego cofnięcia (bezpieczne przy pracy zdalnej).

## 6. DNS i Rozwiązywanie nazw
- `/etc/hosts` - Lokalna mapa IP -> nazwa.
- `/etc/resolv.conf` - Konfiguracja serwerów DNS (często zarządzana przez systemd-resolved).
- `resolvectl status` - Sprawdza, jakie DNS-y są aktualnie używane przez system.

# 📂 Kluczowe Pliki Konfiguracyjne Sieci w Linux

## 1. /etc/hosts (Lokalna tablica nazw)
- **Rola:** Statyczne mapowanie adresów IP na nazwy hostów.
- **Działanie:** System sprawdza ten plik ZANIM zapyta serwer DNS.
- **Format:** `[IP] [nazwa_hosta] [alias]`
- **Przykład:** `192.168.1.50  serwer-projektu  srv1`
- **Zastosowanie:** Przyspieszanie dostępu do lokalnych maszyn lub blokowanie domen (kierowanie ich na 127.0.0.1).

## 2. /etc/resolv.conf (Konfiguracja DNS)
- **Rola:** Definiuje adresy IP serwerów nazw (DNS), do których system wysyła zapytania.
- **Kluczowe słowa:**
    - `nameserver 8.8.8.8` – adres serwera DNS.
    - `search moja-domena.local` – domena wyszukiwania (dodawana automatycznie, gdy wpiszesz samą nazwę hosta).
- **Uwaga:** W nowoczesnych systemach (Ubuntu/systemd) ten plik jest często linkiem symbolicznym zarządzanym przez `systemd-resolved`. Ręczna edycja może zostać nadpisana.

## 3. /etc/nsswitch.conf (Kolejność rozwiązywania nazw)
- **Rola:** Definiuje priorytety źródeł informacji (nie tylko sieciowych).
- **Linia `hosts:`**: Określa kolejność sprawdzania nazw.
    - Przykład: `hosts: files dns` oznacza: "najpierw sprawdź /etc/hosts, potem zapytaj DNS".

## 4. /etc/hostname (Nazwa maszyny)
- **Rola:** Przechowuje unikalną nazwę Twojego systemu (np. `ubuntu-server`).
- **Zmiana:** Można edytować ręcznie lub komendą `hostnamectl set-hostname [nowa_nazwa]`.

## 5. /etc/netplan/*.yaml (Konfiguracja interfejsów - Ubuntu/Debian)
- **Rola:** Główny katalog konfiguracji sieciowej dla narzędzia Netplan.
- **Zastosowanie:** Tu definiujesz statyczne IP, bramy domyślne (gateways) i konfigurację mostów (bridges).
- **Format:** YAML (wymaga rygorystycznych wcięć spacjami).

## 6. /etc/services (Mapa portów i usług)
- **Rola:** Tekstowa baza danych mapująca numery portów na nazwy usług.
- **Zastosowanie:** Pozwala programom (np. `ss` lub `nmap`) wyświetlać "http" zamiast "80" lub "ssh" zamiast "22".

## 7. /etc/protocols (Mapa protokołów)
- **Rola:** Lista numerów identyfikacyjnych protokołów (np. TCP=6, UDP=17, ICMP=1).