Utwórz katalog /root/important_backups.
Znajdź w katalogu /etc wszystkie pliki (tylko pliki! -type f), które:
    Mają rozmiar większy niż 50KB.
    Były modyfikowane w ciągu ostatnich 3 dni.
Skopiuj wszystkie te pliki do /root/important_backups.
Uwaga: Nie możesz użyć xargs, musisz to zrobić bezpośrednio wewnątrz komendy find.

```bash
mkdir /root/important_backups
find /etc -type f -size +50k -mtime -3 -exec sudo cp {} /root/important_backups/ \; 
```

lepsza wersja:
```bash
find /etc -type f -size +50k -mtime -3 -exec sudo cp -t /root/important_backups/ {} +
```

Używamy -t, aby wskazać katalog docelowy przed listą plików