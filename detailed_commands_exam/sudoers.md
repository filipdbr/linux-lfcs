# 🛡️ Bezpieczna konfiguracja Sudoers

- **Złota zasada:** Nigdy nie edytuj `/etc/sudoers` zwykłym edytorem (vi/nano). Zawsze używaj `visudo`.
- **Katalog .d:** Zamiast zaśmiecać główny plik, twórz pliki w `/etc/sudoers.d/`.
- **Komenda edycji:** `sudo visudo -f /etc/sudoers.d/nazwa_pliku`.
- **Składnia:**
  `%grupa   HOST=(USER)   [NOPASSWD:] /sciezka/do/binarki`

**Przykład:** `%auditors ALL=(root) NOPASSWD: /usr/bin/systemctl` 
(Pozwala grupie audytorów na restart usług bez podawania hasła).