# 🏗️ Zarządzanie Wolumenami LVM (Logical Volume Manager)

LVM to warstwa abstrakcji między fizycznymi dyskami a systemem plików. Pozwala na elastyczne zarządzanie miejscem bez konieczności restartowania systemu czy odmontowywania partycji.

## 1. Architektura LVM (Od dołu do góry)

1.  **PV (Physical Volume) – Fizyczny Wolumen**: 
    - Surowy dysk lub partycja (np. `/dev/sdb`, `/dev/nvme0n1p2`).
    - Musi zostać zainicjalizowany przez LVM, aby stać się "materiałem budowlanym".
    - [Image of LVM Physical Volume initialization on raw disk]

2.  **VG (Volume Group) – Grupa Wolumenów**:
    - Wirtualna pula miejsca stworzona z jednego lub wielu PV.
    - Działa jak "wirtualny dysk twardy", z którego wycinamy mniejsze kawałki.
    - [Image of LVM Volume Group pooling multiple Physical Volumes]

3.  **PE (Physical Extents) – Fizyczne Jednostki**:
    - Najmniejsze "cegiełki" (domyślnie 4MB), z których składa się VG.
    - Rozmiar LV jest zawsze wielokrotnością rozmiaru PE.

4.  **LV (Logical Volume) – Wolumen Logiczny**:
    - Wirtualna partycja wycięta z VG.
    - To tutaj tworzymy system plików (`mkfs`) i to ten element montujemy w systemie.
    - [Image of LVM Logical Volume carved out of Volume Group]

---

## 2. Komendy Diagnostyczne (Ściąga)

| Warstwa | Komenda krótka | Komenda szczegółowa | Co pokazuje? |
| :--- | :--- | :--- | :--- |
| **PV** | `pvs` | `pvdisplay` | Ścieżkę do dysku, przynależność do VG. |
| **VG** | `vgs` | `vgdisplay` | Całkowity rozmiar puli, wolne miejsce. |
| **LV** | `lvs` | `lvdisplay` | Rozmiar wolumenu, ścieżkę `/dev/mapper/...`. |

---

## 3. Tworzenie LVM krok po kroku

1. **Przygotowanie fizycznych nośników:**
   `pvcreate /dev/sdb /dev/sdc`
   
2. **Stworzenie grupy wolumenów (puli):**
   `vgcreate nazwa_grupy /dev/sdb /dev/sdc`

3. **Utworzenie wolumenu logicznego:**
   - Rozmiar konkretny (np. 10GB): `lvcreate -L 10G -n nazwa_lv nazwa_grupy`
   - Całe wolne miejsce: `lvcreate -l 100%FREE -n nazwa_lv nazwa_grupy`

4. **Nałożenie systemu plików i montowanie:**
   `mkfs.ext4 /dev/nazwa_grupy/nazwa_lv`
   `mount /dev/nazwa_grupy/nazwa_lv /mnt/data`

---

## 4. Rozszerzanie Wolumenów (Kluczowe na egzaminie!)

Jedną z największych zalet LVM jest możliwość powiększenia dysku "w locie".

### Scenariusz: Powiększamy LV o 5GB i od razu rozciągamy system plików
```bash
lvextend -L +5G -r /dev/nazwa_grupy/nazwa_lv