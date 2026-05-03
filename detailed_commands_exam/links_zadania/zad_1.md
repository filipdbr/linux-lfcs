W swoim katalogu domowym (np. /home/bob) utwórz folder o nazwie link_test.
Znajdź plik /etc/hostname i utwórz do niego dowiązanie twarde (hard link) wewnątrz katalogu link_test. Nazwij je hostname_hl.
Znajdź plik /etc/hosts i utwórz do niego dowiązanie symboliczne (soft link) wewnątrz katalogu link_test. Nazwij je hosts_sl.
Wyświetl listę plików w katalogu link_test w taki sposób, aby było widać ich numery inode.

```bash
mkdir /home/ohhimark/link_test
sudo ln -v /etc/hostname /home/ohhimark/link_test/hostname_hl
sudo ln -s /etc/hosts /home/ohhimark/link_test/hosts_sl
ls -li /home/ohhimark/link_test 
```

-v to verbose