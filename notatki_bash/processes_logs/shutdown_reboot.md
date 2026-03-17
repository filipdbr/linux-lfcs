# 🚀 Boot, Reboot, and Shutdown

Zarządzanie sesją i zasilaniem systemu to krytyczny element pracy administratora. Na egzaminie musisz wiedzieć, jak kontrolować te procesy bez ryzyka utraty danych.

---

## 1. Shutdown (Zamykanie systemu)
Komenda `shutdown` jest preferowana, ponieważ wysyła ostrzeżenie do wszystkich zalogowanych użytkowników i bezpiecznie zamyka procesy.

| Komenda | Działanie |
| :--- | :--- |
| `sudo shutdown now` | Natychmiastowe wyłączenie zasilania. |
| `sudo shutdown +15` | Wyłączenie za 15 minut. |
| `sudo shutdown 20:00` | Wyłączenie o konkretnej godzinie. |
| `sudo shutdown -c` | **Anulowanie** zaplanowanego wyłączenia. |
| `sudo shutdown -k +5 "Test"` | Wysyła tylko ostrzeżenie (fake shutdown), nie wyłącza systemu. |


---

## 2. Reboot (Restartowanie)
Restart jest niezbędny po aktualizacji jądra (Kernel) lub zmianie ważnych ustawień systemowych.

* `sudo reboot` – najprostszy i najszybszy sposób.
* `sudo shutdown -r +5` – restart zaplanowany za 5 minut (flaga `-r` = reboot).
* `systemctl reboot` – nowoczesny sposób zarządzany przez systemd.

---

## 3. Komunikacja z użytkownikami (Wall)
Zanim wyłączysz serwer, na którym pracują inni, powinieneś ich ostrzec:
* `sudo wall "System zostanie wyłączony za 5 minut. Proszę zapisać pracę."`
(Wysyła wiadomość na terminale wszystkich zalogowanych osób).

---

## 4. Zarządzanie z poziomu Systemd (Modern Way)
W LFCS systemd jest standardem. Te komendy działają niemal w każdej nowoczesnej dystrybucji:
* `systemctl poweroff` – wyłączenie systemu.
* `systemctl reboot` – restart.
* `systemctl suspend` – uśpienie (do RAM).
* `systemctl hibernate` – hibernacja (na dysk).


---

## 5. Sprawdzanie aktywności przed wyłączeniem
Zanim wpiszesz `shutdown`, sprawdź kto "oberwie":
* `who` – lista zalogowanych użytkowników.
* `w` – pokazuje użytkowników ORAZ co aktualnie robią (jakie procesy mają otwarte).
* `last -n 5` – pokazuje 5 ostatnio zalogowanych osób i historię restartów (boot).

---

## 💡 LFCS Pro-Tips:
1. **Pamiętaj o `sync`**: Przed restartem warto wpisać `sync`. Wymusza to zapisanie wszystkich danych z buforów RAM na dysk. Choć `shutdown` robi to automatycznie, to dobry nawyk.
2. **Uptime**: Komenda `uptime` powie Ci, jak długo system pracuje od ostatniego boota.
3. **Plik /etc/motd**: Jeśli chcesz na stałe ustawić wiadomość powitalną ostrzegającą o planowanych przerwach technicznych, edytuj plik "Message of the Day".