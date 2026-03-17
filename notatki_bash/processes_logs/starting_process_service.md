# ⚙️ Manage Startup Process and Services

Zrozumienie jak Linux wstaje i jak zarządzać usługami w `systemd` to absolutna podstawa pracy administratora.

---

## 1. Proces Bootowania (Krok po Kroku)
Zanim zobaczysz terminal, dzieje się kilka kluczowych rzeczy:
1.  **BIOS/UEFI**: Sprawdza sprzęt i szuka sektora rozruchowego.
2.  **GRUB2**: Menu wyboru systemu (Bootloader). Tu możesz np. wejść w tryb ratunkowy.
3.  **Kernel (Jądro)**: Ładuje sterowniki i montuje główny system plików (`/`).
4.  **systemd**: Pierwszy proces (PID 1), który uruchamia wszystkie inne usługi.

---

## 2. Zarządzanie Usługami (systemctl)
W `systemd` wszystko jest "unitem". Najważniejsze są usługi (`.service`).

| Komenda | Opis |
| :--- | :--- |
| `systemctl start [usługa]` | Uruchamia usługę natychmiast. |
| `systemctl stop [usługa]` | Zatrzymuje usługę natychmiast. |
| `systemctl restart [usługa]` | Stop + Start (przerywa połączenia). |
| `systemctl reload [usługa]` | Odświeża konfigurację bez przerywania działania. |
| `systemctl status [usługa]` | Sprawdza, czy działa, pokazuje PID i ostatnie logi. |



---

## 3. Automatyczny Start (Enable vs Disable)
To najczęstsze zadanie egzaminacyjne. Pamiętaj: `start` to "teraz", `enable` to "po restarcie".

* **`systemctl enable [usługa]`**: Sprawia, że usługa włączy się sama przy starcie systemu (tworzy dowiązanie symboliczne).
* **`systemctl disable [usługa]`**: Usuwa automatyczny start.
* **`systemctl is-enabled [usługa]`**: Szybkie sprawdzenie statusu automatycznego startu.

---

## 4. Targets (Poziomy uruchomienia)
W systemd zamiast "runleveli" mamy **Targets**. To zestawy usług, które mają być uruchomione.

| Target | Odpowiednik Runlevel | Opis |
| :--- | :--- | :--- |
| `graphical.target` | 5 | Tryb graficzny (GUI), sieć, wielu użytkowników. |
| `multi-user.target` | 3 | Tryb tekstowy (CLI), sieć, wielu użytkowników (Standard serwerowy). |
| `rescue.target` | 1 | Tryb ratunkowy, tylko root, brak sieci. |

**Komendy dla Targetów:**
* `systemctl get-default` – sprawdza, w jakim trybie system wstaje domyślnie.
* `systemctl set-default multi-user.target` – ustawia serwer tak, by wstawał bez grafiki.
* `systemctl isolate graphical.target` – przełącza system w tryb graficzny "na żywo".



---

## 5. Analiza problemów ze startem (Troubleshooting)
Jeśli usługa nie wstaje lub system startuje zbyt wolno:

* **`systemctl list-units --type=service --state=failed`**: Pokazuje tylko te usługi, które się wywaliły.
* **`systemd-analyze`**: Pokazuje, ile czasu zajęło bootowanie.
* **`systemd-analyze blame`**: Wyświetla listę usług od tej, która startowała najdłużej (idealne do optymalizacji).

---

## 6. Maskowanie Usług (Mask)
Gdy usługa jest tak "zła", że nie chcesz, by cokolwiek (nawet inna usługa) ją uruchomiło:
* `systemctl mask apache2` – "zabija" usługę, linkując ją do `/dev/null`.
* `systemctl unmask apache2` – przywraca do życia.

---

## 💡 LFCS Pro-Tips:
1.  **Tab Completing**: Na egzaminie zawsze używaj Tab. Jeśli `systemctl start ss[Tab]` nie dopełnia nazwy, to usługa może się inaczej nazywać.
2.  **daemon-reload**: Jeśli zmienisz cokolwiek w pliku `.service` (w `/etc/systemd/system/`), **MUSISZ** wpisać `systemctl daemon-reload`, inaczej systemd będzie używać starej wersji z pamięci.
3.  **Active vs Loaded**: Jeśli status pokazuje `Loaded: loaded` ale `Active: inactive`, oznacza to, że systemd zna usługę, ale nie jest ona teraz uruchomiona.