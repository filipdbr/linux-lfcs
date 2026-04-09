# 💾 Notatki: Zarządzanie Wolumenami i Pamięcią (LFCS)

Na egzaminie każda operacja na dysku musi być trwała (przetrwać restart). Proces zawsze wygląda tak: **Partycja -> System plików -> Montowanie -> /etc/fstab**.

## 1. Tworzenie i Zarządzanie Partycjami
Do manipulacji tablicą partycji używamy `fdisk` (tekstowy) lub `cfdisk` (graficzny/interaktywny).

- `fdisk -l` – Wyświetla wszystkie dostępne dyski i partycje.
- `fdisk /dev/sdb` – Rozpoczyna edycję dysku `sdb`.
    - `n` – nowa partycja.
    - `d` – usuwanie partycji.
    - `t` – zmiana typu (np. na `82` dla Swap lub `8e` dla LVM).
    - `w` – **Zapisanie zmian** i wyjście.
- `partprobe` – Informuje jądro o zmianach w tablicy partycji bez restartu (kluczowe!).

## 2. Tworzenie Systemów Plików (Formatowanie)
Sama partycja to tylko "pudełko". Musisz nadać jej format, żeby system mógł pisać dane.

- `mkfs.ext4 /dev/sdb1` – Tworzy standardowy system plików ext4.
- `mkfs.xfs /dev/sdb2` – Tworzy system plików XFS (często spotykany w RHEL/CentOS).
- `mkswap /dev/sdb3` – Przygotowuje partycję pod pamięć wymiany (Swap).



## 3. Montowanie (Mounting)
Podpinanie sformatowanej partycji do konkretnego folderu w systemie.

- `mount /dev/sdb1 /mnt/data` – Montuje partycję tymczasowo.
- `umount /mnt/data` – Odmontowuje partycję.
- `df -h` – Sprawdza zajętość zamontowanych systemów plików (w formacie czytelnym dla człowieka).
- `lsblk -f` – Wyświetla drzewo blokowe wraz z systemami plików i ich UUID.

## 4. Konfiguracja Trwała: /etc/fstab
Aby partycje montowały się same po restarcie, musisz dodać wpis w `/etc/fstab`.

**Format wpisu:**
`[Urządzenie/UUID] [Punkt_montowania] [Typ] [Opcje] [Dump] [Pass]`

**Przykład:**
`UUID=b5e... /data ext4 defaults 0 2`

> **Wskazówka:** Zawsze używaj **UUID** zamiast nazw typu `/dev/sdb1`, bo nazwy urządzeń mogą się zmienić po restarcie. UUID sprawdzisz komendą `blkid`.



## 5. Powiększanie Wolumenów (Resize)
Jeśli zwiększyłeś partycję w `fdisk`/`cfdisk`, musisz jeszcze rozciągnąć system plików.

- **Dla ext4:** `sudo resize2fs /dev/sdb1`
- **Dla XFS:** `sudo xfs_growfs /mnt/data` (Uwaga: XFS musi być zamontowany, aby go powiększyć!).

## 6. Swap (Pamięć wymiany)
- `swapon /dev/sdb3` – Aktywuje swap.
- `swapoff /dev/sdb3` – Wyłącza swap.
- `free -h` – Sprawdza status pamięci RAM i Swap.