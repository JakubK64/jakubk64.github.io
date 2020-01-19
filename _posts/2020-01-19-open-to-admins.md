---
layout: post
title: Open-to-admins (picoCTF2019)
published: true
tags: picoCTF2019 pico CTF open-to-admins open to admins 
---

## Opis

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Web%20Exploitation/Open-to-admins/zadanie.png }}\CTF\picoCTF2019\Web%20Exploitation\Open-to-admins\zadanie.png)

## Rozwiązanie

Na początek widzimy prostą stronę z przyciskiem "Flag". Gdy spróbujemy kliknąć w podany przycisk otrzymujemy komunikat, że nie jesteśmy administartorem lub czas nie jest odpowiedni:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Web%20Exploitation/Open-to-admins/1.png }}\CTF\picoCTF2019\Web%20Exploitation\Open-to-admins\1.png)

W treści zadania możemy znaleźć informację, że aby uzyskać flagę musimy być administartorem oraz czas musi być równy dokładnie 1400. W tym celu wykorzystamy to, że walidacja następuje po stronie klienta a nie serwera. Pozwala to nam na tworzenie i usuwanie ciasteczek dla naszej sesji. Wystarczy więc odpalić konsolę i wydać dwa krótkie polecenia:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Web%20Exploitation/Open-to-admins/2.png }}\CTF\picoCTF2019\Web%20Exploitation\Open-to-admins\2.png)

Dzięki temu mamy status administratora a czas na stronie jest równy 1400 czyli zgodnie z wymaganiami. Teraz gdy klikniemy w przycisk "Flag" na stronie pokaże się nam nasza flaga:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Web%20Exploitation/Open-to-admins/3.png }}\CTF\picoCTF2019\Web%20Exploitation\Open-to-admins\3.png)

## Flag: *picoCTF{0p3n_t0_adm1n5_dcb566bb}*