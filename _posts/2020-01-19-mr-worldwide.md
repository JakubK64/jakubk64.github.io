---
layout: post
title: Mr-Worldwide (picoCTF2019)
published: true
tags: picoCTF2019 pico CTF Mr-Worldwide music cypher cryptography
---

### Opis

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Cryptography/Mr-Worldwide/zadanie.png }}\CTF\picoCTF2019\Cryptography\Mr-Worldwide\zadanie.png)

### Rozwiązanie

Otrzymujemy jakiś zaszyfrowany tekst w pliku tekstowym (format zapisanej wiadomości wskazuje na to, iż jest to zaszyfrowana flaga). Po ułożeniu nawiasów w kolumnie wygląda to nastepująco:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Cryptography/Mr-Worldwide/1.png }}\CTF\picoCTF2019\Cryptography\Mr-Worldwide\1.png)

Widzimy ujemne i dodtnie liczby z zakresu mniej więcej (-150,150) co od razu sugeruje, że mogą to być koordynaty. Po sprawdzeniu kilku koordynatów (używając Google Maps, w angielskiej wersji) okazuje się, iż prawdopodobnie wszystkie wskazują na jakieś miasto na świecie. Próbuję więc ułożyć flagę z pierwszych liter miast, na które wskazują koordynaty. Okazuje się, że jest to dobry trop.

#### Flag: *picoCTF{KODIAK_ALASKA}*