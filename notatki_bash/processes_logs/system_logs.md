# 📂 Locate and Analyze System Logs

System operacyjny zapisuje niemal każde zdarzenie. Twoim zadaniem jest umiejętne przeszukiwanie tych zapisów, aby szybko naprawiać awarie.

---

## 1. Tradycyjne logi tekstowe (`/var/log`)
Większość usług zapisuje logi w formie plików tekstowych.

| Plik | Zawartość |
| :--- | :--- |
| `/var/log/syslog` | Ogólne logi systemowe (Debian/Ubuntu). |
| `/var/log/messages` | Ogólne logi systemowe (RHEL/CentOS). |
| `/var/log/auth.log` | Logi logowania i bezpieczeństwa (SSH, sudo). |
| `/var/log/kern.log` | Informacje prosto z jądra systemu (Kernel). |
| `/var/log/dmesg` | Logi z momentu startu systemu (bufor pierścieniowy). |



---

## 2. Nowoczesne logi: `journalctl`
W systemach z `systemd`, logi są przechowywane w formie binarnej przez `journald`. Daje to potężne możliwości filtrowania.

### Podstawowe filtrowanie:
* `journalctl` – wyświetla wszystkie logi (od najstarszych).
* `journalctl -r` – (reverse) pokazuje najnowsze wpisy na górze.
* `journalctl -u ssh.service` – logi tylko dla konkretnej usługi.
* `journalctl -xe` – (eXtended, End) pokazuje ostatnie logi z dodatkowym opisem błędów (standard przy awariach).

### Filtrowanie po czasie:
* `journalctl --since today`
* `journalctl --since "2026-03-15 12:00:00" --until "2026-03-15 13:00:00"`
* `journalctl -b` – logi tylko z aktualnego uruchomienia systemu (boot).



---

## 3. Priorytety logów
Możesz filtrować logi według ich ważności (od 0 do 7).
`journalctl -p err` – pokaże tylko błędy (Error) i ważniejsze (Critical, Alert, Emerg).

| Kod | Nazwa | Opis |
| :--- | :--- | :--- |
| **3** | `err` | Błędy wykonania. |
| **4** | `warning` | Ostrzeżenia (coś może pójść nie tak). |
| **5** | `notice` | Normalne, ale istotne zdarzenia. |
| **6** | `info` | Informacje czysto techniczne. |

---

## 4. Narzędzia do analizy w czasie rzeczywistym
Na egzaminie często musisz obserwować logi podczas restartu usługi:

* **`tail -f /var/log/auth.log`**: Śledzi plik na żywo (każda nowa linia pojawia się w terminalu).
* **`journalctl -f`**: To samo, ale dla całego dziennika systemowego.

---

## 5. Rotacja logów (`logrotate`)
Logi nie mogą zajmować całego dysku. Za ich czyszczenie odpowiada `logrotate`.
* Konfiguracja: `/etc/logrotate.conf` oraz katalog `/etc/logrotate.d/`.
* Działanie: Pakuje stare logi w `.gz` i usuwa najstarsze po określonym czasie.

---

## 💡 LFCS Pro-Tips:
1. **Łączenie narzędzi**: Używaj potoków, by znaleźć konkretne IP w logach:
   `grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c`
2. **Persistence**: Domyślnie w niektórych systemach logi `journalctl` znikają po restarcie (są w `/run/log/journal`). Aby były trwałe, musisz stworzyć katalog `/var/log/journal`.
3. **Logi binarne**: Nie próbuj otwierać plików w `/var/log/journal/` edytorem `vi` – do tego służy wyłącznie komenda `journalctl`.