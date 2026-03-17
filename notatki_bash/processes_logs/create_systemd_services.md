# 🛠️ Create systemd Services

Tworzenie własnych jednostek (units) pozwala systemowi zarządzać Twoimi aplikacjami tak samo, jak profesjonalnymi usługami systemowymi.

---

## 1. Lokalizacja plików
Gdzie trzymać własne pliki serwisów?
* **`/etc/systemd/system/`** – Tu administrator (Ty) tworzy i edytuje własne serwisy. Pliki w tym katalogu mają najwyższy priorytet.
* Plik musi mieć rozszerzenie `.service` (np. `backup.service`).

---

## 2. Struktura pliku .service
Plik podzielony jest na trzy główne sekcje. Każda z nich jest obowiązkowa w zadaniach egzaminacyjnych.

### [Unit]
Opisuje usługę i jej zależności.
* `Description=` – Krótki opis widoczny w `systemctl status`.
* `After=` – Kolejność. Mówi systemowi: "Najpierw uruchom sieć/bazę, potem mnie". Przykład: `After=network.target`.
* `Requires=` – Silna zależność. Jeśli wskazana usługa padnie, nasz serwis też zostanie wyłączony.

### [Service]
Serce konfiguracji – co i jak uruchomić.
* `Type=` – Typ procesu (najczęściej `simple` dla zwykłych skryptów lub `forking` dla daemonów).
* `ExecStart=` – **Pełna ścieżka** do skryptu lub binarki (np. `/usr/local/bin/myscript.sh`).
* `ExecStop=` – Opcjonalna komenda do bezpiecznego zatrzymania usługi.
* `User=` – Pod jakim użytkownikiem ma działać serwis (domyślnie root).
* `Restart=` – Kiedy system ma sam podnieść usługę? Opcje: `always`, `on-failure`.
* `RestartSec=` – Ile sekund czekać przed restartem.

### [Install]
Mówi systemowi, jak usługa ma być włączona (enable).
* `WantedBy=` – Określa "target", do którego usługa jest przypisana. Prawie zawsze używamy `multi-user.target`.



---

## 3. Przykładowy plik (Gotowiec na egzamin)
Załóżmy, że masz skrypt `/opt/monitor.py`, który musi działać po uruchomieniu sieci:

```ini
[Unit]
Description=My Custom Monitor Service
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/python3 /opt/monitor.py
Restart=always
User=bob

[Install]
WantedBy=multi-user.target
```