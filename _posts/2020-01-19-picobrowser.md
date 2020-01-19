---
layout: post
title: picobrowser (picoCTF2019)
published: true
tags: picoCTF2019 pico CTF pico browser picobrowser 
---

## Opis

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Web%20Exploitation/picobrowser/zadanie.png }}\CTF\picoCTF2019\Web%20Exploitation\picobrowser\zadanie.png)

## Rozwiązanie

Pod linkiem znajdujemy prostą stronę z przyciskiem "Flag", która nie wyświetla nam flagi, ponieważ używamy innego rodzaju przeglądarki niż "picobrowser":

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Web%20Exploitation/picobrowser/1.png }}\CTF\picoCTF2019\Web%20Exploitation\picobrowser\1.png)

Aby to zmienić klikamy F12 i przechodzimy do opcji emulowania innego urządzenia:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Web%20Exploitation/picobrowser/2.png }}\CTF\picoCTF2019\Web%20Exploitation\picobrowser\2.png)

Następnie dodajemy nowe urządzenie:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Web%20Exploitation/picobrowser/3.png }}\CTF\picoCTF2019\Web%20Exploitation\picobrowser\3.png)

Nazywamy je zgodnie z wytycznymi "picobrowser" oraz w stringu wpisujemy również "picobrwser", aby wartość parametru "User-agent" przyjęła wartość "picobrowser" i strona prawidłowo rozpoznała naszą przelgądarkę.

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Web%20Exploitation/picobrowser/4.png }}\CTF\picoCTF2019\Web%20Exploitation\picobrowser\4.png)

Następnie przełączamy widok na nowo utworzoną "przeglądarkę":

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Web%20Exploitation/picobrowser/5.png }}\CTF\picoCTF2019\Web%20Exploitation\picobrowser\5.png)

I klikamy przycisk "Flag" dzięki czemu po odświeżeniu strony widzimy szukaną flagę

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Web%20Exploitation/picobrowser/6.png }}\CTF\picoCTF2019\Web%20Exploitation\picobrowser\6.png)

## Flag: *picoCTF{p1c0_s3cr3t_ag3nt_ee951878}*