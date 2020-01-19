---
layout: post
title: la-cifra-de (picoCTF2019)
published: true
tags: picoCTF2019 pico CTF open-to-admins open to admins 
---

## Opis

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Cryptography/la-cifra-de/zadanie.png }}\CTF\picoCTF2019\Cryptography\la-cifra-de\zadanie.png)

## Rozwiązanie

Na początek łączymy się ze wskazanym serwerem i otrzymujemy zaszyfrowaną wiadomość. Warto zwrócić uwagę na format tekstu przypominający flagę pod koniec 4 akapitu

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Cryptography/la-cifra-de/1.png }}\CTF\picoCTF2019\Cryptography\la-cifra-de\1.png)

Wygląda to na zastosowanie jakiegoś prostego, starego szyfru. Piersze co może przyjść na myśl to szyfr przestawny, ale okazuje się to błędny trop. Okazuje się, że tekst został zaszyfrowany za pomocą szyfru Vigenera z kluczem "flag" (szyfr polegający na dodanie pozycji w alfabecie liter tekstu i klucza, gdzie pozycja A=0, oraz zastosowanie modulo [wielkosć alfabetu, dla angielskiego 26] a następnie z liczby wynikowej powstają litery będące na tej pozycji w alfabecie). Korzystamy więc z decodera znalezionego w internecie:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Cryptography/la-cifra-de/2.png }}\CTF\picoCTF2019\Cryptography\la-cifra-de\2.png)

I otrzymujemy odszyfrowany tekst zawierający flagę:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Cryptography/la-cifra-de/3.png }}\CTF\picoCTF2019\Cryptography\la-cifra-de\3.png)

## Flag: *picoCTF{b311a50_0r_v1ge3r3_c1ph3raac148e7}*