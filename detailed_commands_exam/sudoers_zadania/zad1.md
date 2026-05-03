Skonfiguruj system tak, aby wszyscy członkowie grupy auditors mogli wykonywać komendę /usr/bin/systemctl z uprawnieniami roota.
Użytkownicy z tej grupy nie powinni być pytani o hasło przy wykonywaniu tej konkretnej komendy.
Ważne: Nie edytuj bezpośrednio pliku /etc/sudoers. Stwórz osobny plik konfiguracyjny w katalogu /etc/sudoers.d/ o nazwie auditors.

```bash
sudo visudo -f /etc/sudoers.d/auditors
%auditors ALL=(root) NOPASSWD: /usr/bin/systemctl
```