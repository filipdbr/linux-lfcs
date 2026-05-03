# 📝 Zadanie LFCS: Przetwarzanie strumieni i pułapka uprawnień

## 📋 Treść zadania
Wyszukaj w pliku `/etc/services` wszystkie linie zawierające frazę **http** (małymi literami). Z wyników wyodrębnij tylko **pierwszą kolumnę** (nazwę usługi). Wynik musi być **posortowany alfabetycznie**, nie może zawierać **duplikatów** i ma zostać zapisany do pliku `/root/http_services.txt`.

## 💻 Rozwiązanie (One-liner)
```bash
grep http /etc/services | awk '{print $1}' | sort -u | sudo tee /root/http_services.txt > /dev/null
```