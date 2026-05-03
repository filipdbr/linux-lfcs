Utwórz grupę o nazwie developers i nadaj jej konkretny numer GID: 2000.
Dodaj swojego aktualnego użytkownika (ohhimark) do grupy developers.
Uwaga: Zrób to tak, aby użytkownik nadal należał do wszystkich swoich dotychczasowych grup (nie nadpisz ich!).
Wyświetl identyfikatory (UID i GID) swojego użytkownika, aby potwierdzić zmianę.
Znajdź w systemie wszystkie pliki, które należą do grupy o GID 2000.

```bash
sudo groupadd -g 2000 developers
sudo gpasswd -a ohhimark developers

# mozna tez tak, tutaj mozna wpisac kilka grup po przecinku
sudo usermod -aG developers ohhimark

# Sprawdzenie ID
id ohhimark

# Znalezienie plików po GID
sudo find / -type f -gid 2000 2>/dev/null
```