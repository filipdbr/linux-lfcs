# 🔍 Notatki: Inspekcja i Diagnostyka Montowań (LFCS)

Zrozumienie, co i jak jest zamontowane, to podstawa rozwiązywania problemów z uprawnieniami, wydajnością i brakiem miejsca na dysku.

## 1. Najważniejsze komendy inspekcyjne

### A. findmnt (Rekomendowane)
Najnowocześniejsze narzędzie, które domyślnie pokazuje strukturę drzewiastą (hierarchię).
- `findmnt` – Wyświetla wszystkie punkty montowania.
- `findmnt --verify` – **Kluczowe dla LFCS!** Sprawdza `/etc/fstab` pod kątem błędów (brakujące foldery, błędne UUID).
- `findmnt /test` – Pokazuje szczegóły konkretnego punktu montowania.

### B. mount (Zestawienie techniczne)
- `mount` – Bez argumentów wyświetla wszystkie zamontowane systemy wraz z pełną listą flag (np. `rw,relatime,errors=remount-ro`).
- `mount | grep /dev/vde` – Filtruje wynik, by zobaczyć tylko konkretne urządzenie.



### C. df -h (Zajętość miejsca)
- `df -h` – Wyświetla systemy plików, ich rozmiar, zużycie i punkt montowania w formacie czytelnym (GB/MB).
- `df -T` – Dodatkowo wyświetla typ systemu plików (np. ext4, xfs).

## 2. Rozwiązywanie problemów (Troubleshooting)

### Problem: System plików "Read-only"
Jeśli nie możesz utworzyć pliku, sprawdź czy dysk nie jest zamontowany w trybie `ro`.
- **Komenda:** `mount | grep /test`
- **Wynik:** Szukaj `(ro,...)` lub `(rw,...)` na początku nawiasu.

### Szybka naprawa (Remount)
Jeśli dysk jest w trybie `ro`, a chcesz zmienić go na `rw` bez odmontowywania (np. żeby poprawić błąd):
```bash
sudo mount -o remount,rw /test