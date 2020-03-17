---
layout: post
title: Rsa-pop-quiz (picoCTF2019)
published: true
tags: picoCTF2019 pico CTF RSA rsa-pop-quiz
---

## Opis

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Cryptography/RSA-pop-quiz/Task0.png }}\CTF\picoCTF2019\Cryptography\RSA-pop-quiz\Task0.png)

## Rozwiązanie

Mamy do czynienia z zadaniem, które sprawdza znajomość algorytmu RSA. Jest to zestaw zadań z danymi i pytaniami, czy jest możliwe, aby znaleźć szukaną wartość. Jeżeli tak, musimy obliczyć i podać poprawny wynik.

- Zadanie 1

Zadanie:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Cryptography/RSA-pop-quiz/Task1.png }}\CTF\picoCTF2019\Cryptography\RSA-pop-quiz\Task1.png)

Rozwiązanie:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Cryptography/RSA-pop-quiz/Solve1.png }}\CTF\picoCTF2019\Cryptography\RSA-pop-quiz\Solve1.png)

- Zadanie 2

Zadanie:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Cryptography/RSA-pop-quiz/Task2.png }}\CTF\picoCTF2019\Cryptography\RSA-pop-quiz\Task2.png)

Rozwiązanie:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Cryptography/RSA-pop-quiz/Solve2.png }}\CTF\picoCTF2019\Cryptography\RSA-pop-quiz\Solve2.png)

- Zadanie 3

Zadanie:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Cryptography/RSA-pop-quiz/Task3.png }}\CTF\picoCTF2019\Cryptography\RSA-pop-quiz\Task3.png)

Rozwiązanie:

Brak. Aby policzyć p lub q trzeba znać wartość jednej z nich. Nie da się odtworzyć p i q znając jedynie wartość ich iloczynu (n) oraz e

- Zadanie 4

Zadanie:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Cryptography/RSA-pop-quiz/Task4.png }}\CTF\picoCTF2019\Cryptography\RSA-pop-quiz\Task4.png)

Rozwiązanie:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Cryptography/RSA-pop-quiz/Solve3.png }}\CTF\picoCTF2019\Cryptography\RSA-pop-quiz\Solve3.png)

- Zadanie 5

Zadanie:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Cryptography/RSA-pop-quiz/Task5.png }}\CTF\picoCTF2019\Cryptography\RSA-pop-quiz\Task5.png)

Rozwiązanie:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Cryptography/RSA-pop-quiz/Solve4.png }}\CTF\picoCTF2019\Cryptography\RSA-pop-quiz\Solve4.png)

- Zadanie 6

Zadanie:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Cryptography/RSA-pop-quiz/Task6.png }}\CTF\picoCTF2019\Cryptography\RSA-pop-quiz\Task6.png)

Rozwiązanie:

Brak. Nie da się odszyfrować wiadomości mając tylko te dane. Potrzebujemy jeszcze wartości klucza prywatnego, do którego obliczenia niezbędna jest znajomość p i q (a dokładnie ich totientu). Potrzeba więc przynajmniej 1 wartości - p lub q aby odszyfrować tą wiadomość.

- Zadanie 7

Zadanie:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Cryptography/RSA-pop-quiz/Task7.png }}\CTF\picoCTF2019\Cryptography\RSA-pop-quiz\Task7.png)

Rozwiązanie:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Cryptography/RSA-pop-quiz/Sovle5.png }}\CTF\picoCTF2019\Cryptography\RSA-pop-quiz\Solve5.png)

* do policzenia 2 musimy najpierw policzyć totient. Dodatkowo do użycia inwersji w pythonie niezbędne może być doinstalowanie odpowienich bibliotek kryptograficznych (lub w przypadku posiadania zaimportowanie funkcji "inverse" znajdującej się w "Crypto.Util.number")

- Zadanie 8

Zadanie:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Cryptography/RSA-pop-quiz/Task8.png }}\CTF\picoCTF2019\Cryptography\RSA-pop-quiz\Task8.png)

Rozwiązanie:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Cryptography/RSA-pop-quiz/Solve6.png }}\CTF\picoCTF2019\Cryptography\RSA-pop-quiz\Solve6.png)

* Zadanie analogiczne do Zadania 6, tym razem jednak dostajemy wartość p i wyliczamy po kolei wartości pozwalające na zdeszyfrowanie tekstu

Po odpowiednich konwersjach otrzymujemy flagę:

## Flag: *picoCTF{wA8_th4t#_ill3aGal..ob7f0bd39}*