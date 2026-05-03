Jako root utwórz plik /root/secret_report.txt i wpisz do niego dowolny tekst.
Sprawdź domyślne uprawnienia tego pliku (zobaczysz, że zwykły użytkownik nie ma do niego dostępu).
Nadaj użytkownikowi ohhimark uprawnienia do odczytu i zapisu (rw) do tego konkretnego pliku, używając mechanizmu ACL.
Upewnij się, że inne uprawnienia (właściciel: root, grupa: root) pozostały bez zmian.
Wyświetl rozszerzone uprawnienia pliku, aby potwierdzić, że ohhimark widnieje na liście.

```bash
# sudo tee zebym mógł od razu przekierować do pliku bez sudo su -
# > /dev/null to to samo co 1>/dev/null. Robię to dlatego, bo tee wyświetla stdout w print, więc przekierowuje stdout do /dev/null.
echo "my text" | sudo tee /root/secret_report.txt > /dev/null
sudo getfacl /root/secret_report.txt
sudo setfacl --modify user:ohhimark:rw /root/secret_report.txt
sudo getfacl /root/secret_report.txt
```

wazne komendy ACL:
```bash
# to dla usera np
setfacl --modify user:filip:rw path_to_file

# dla grupy
setfacl --modify group:admins:rwx path_to_file

# usuwanie
setfacl --remove user:filip path_to_file

# zobacz parawa
getfacl path_to_file
```

zwykle wszystko na sudo

# 🛡️ Zaawansowane zarządzanie dostępem (ACL)

Standardowe uprawnienia (Owner/Group/Others) są niewystarczające, gdy chcemy nadać dostęp "trzeciej osobie".

- **Nadawanie uprawnień:** `sudo setfacl -m u:nazwa_użytkownika:rwx /ścieżka/plik`
- **Usuwanie uprawnień ACL dla usera:** `sudo setfacl -x u:nazwa_użytkownika /ścieżka/plik`
- **Usuwanie wszystkich wpisów ACL:** `sudo setfacl -b /ścieżka/plik`
- **Podgląd:** `getfacl /ścieżka/plik`

# 👤 Wykonywanie zadań jako inny użytkownik

- **Jako root (najczęstsze):** `sudo [komenda]`
- **Jako inny user (np. bob):** `sudo -u bob [komenda]`
- **Sprawdzenie kim aktualnie jestem:** `whoami` lub `id`