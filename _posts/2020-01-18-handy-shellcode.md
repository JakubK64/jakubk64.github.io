---
layout: post
title: handy-shellcode (picoCTF2019)
published: true
tags: picoCTF2019 pico CTF 
---

## Opis

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Web%20Exploitation/handy-shellcode/zadanie.PNG }}\CTF\picoCTF2019\Web%20Exploitation\handy-shellcode\zadanie.PNG)

## Rozwiązanie

Po przejściu do wskazanej lokalizacji znajdujemy plik "flag.txt" zawierający nasz flagę, do którego odczytania nie mamy uprawnień. Dodatkowo znajdujemy pewien program oraz jego kod źródłowy w języku C:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Web%20Exploitation/handy-shellcode/1.PNG }}\CTF\picoCTF2019\Web%20Exploitation\handy-shellcode\1.PNG)

Przejdźmy do analizy działania programu. Kod źródłowy programu jest bardzo prosty. Program po uruchomieniu oczekuje na wpisanie pewnych instrukcji używając funkcji vuln (nr.1, wywołanie w 27 linijce) a następnie próbuje wykonać wprowadzoną przez nas instrukcję (nr.2). Po wykonaniu program kończy swoje działanie.

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Web%20Exploitation/handy-shellcode/2.PNG }}\CTF\picoCTF2019\Web%20Exploitation\handy-shellcode\2.PNG)

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Web%20Exploitation/handy-shellcode/4.PNG }}\CTF\picoCTF2019\Web%20Exploitation\handy-shellcode\4.PNG)

Celem zadanie jest spowodowanie, aby program nie zakończył swojego działania po wykonaniu wprowadzonej przez nas instrukcji. W tym celu możemy wykorzystać gotowy shellcode, których wiele dostępnych jest w sieci. Musimy tylko poznać architekturę. W tym celu w konsoli (shellu na serwerze)używamy polecenia:

```unix
file vuln
```

3 interesujące nas parametry to 32 bitowy system, Linux, Intel:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Web%20Exploitation/handy-shellcode/3.PNG }}\CTF\picoCTF2019\Web%20Exploitation\handy-shellcode\3.PNG)

Wpisanie gotowego kodu do programu bądź utworzenie potoku ze znalezionym kodem nie daje pozytywnego rezultatu. Kod jest poprawnie wykonywany, ale program po wykonaniu go kończy swoje działanie:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Web%20Exploitation/handy-shellcode/5.PNG }}\CTF\picoCTF2019\Web%20Exploitation\handy-shellcode\5.PNG)

Rozwiązaniem tego problemu jest zapiwanie polecenia echo z kodem do pliku tekstowego za pomocą polecenia:

```unix
echo -e '[kod]' > [ścieżka pliku]
```

Następnie za pomocą potoku przekierowujemy odczytanie zawartości pliku na wejście programu za pomocą:

```unix
cat [ścieżka pliku] - | ./vuln
```

W ten sposób otrzymujemy oczekiwany rezultat, program nie zamyka się po wykonaniu wpisanego przez nas kodu. Mamy wiec swobodny dostęp do zawartości katalogu, w którym znajduje się program. Dla pewności sprawdzamy zawartość katalogu za pomocą:

```unix
ls -l
```

A następnie korzystając z tego, że program ma uprawnienia do odczytania pliku tekstowego z flagą odczytujemy z niego flagę:

```unix
cat flag.txt
```

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/picoCTF2019/Web%20Exploitation/handy-shellcode/6.PNG }}\CTF\picoCTF2019\Web%20Exploitation\handy-shellcode\6.PNG)


## *picoCTF{h4ndY_d4ndY_sh311c0d3_0b440487}*