---
layout: post
title: Różne sposoby dodawania komutera do domeny
published: true
tags: domain domena dodawanie adding cmd powershell
---

### Przygotowanie

Pracująć na środowisku "testowym", złożonym z maszyn wirtualnych należe zadbać o kilka podstawowych rzeczy:
- Przynajmniej główny kontroler domeny musi być uruchomiony
- Maszyna, musi być w tej samej sieci (mieć ustawiony ten sam switch) co kontroler domeny (w przypadku środowiska lokalnego, bez dostępu do internetu)
- Należy ustawić adresy IP, aby maszyna wiedziała pod jakim adresem szukać serwera DNS
- Warto zmienić nazwę maszyny na bardziej przyjazną - w celu większej przejrzystości maszyn (nie jest to wymagane)

Na początek przejdźmy do ustawienia adresów IP. Korzystamy z jednej z kilku metod wejścia do ustawień adresów IPv4:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Dodawanie_Do_Domeny/IP1.PNG }}CTF/Administracja/Windows_Server/Dodawanie_Do_Domeny/IP1.PNG)

Potem w "Change adapter options":

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Dodawanie_Do_Domeny\IP2.PNG }}CTF\Administracja\Windows_Server\Dodawanie_Do_Domeny\IP2.PNG)

Następnie prawym przyciskiem myszy na używaną kartę sieciową oraz w "Properties":

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Dodawanie_Do_Domeny\IP3.PNG }}CTF\Administracja\Windows_Server\Dodawanie_Do_Domeny\IP3.PNG)

Natępnie wyłączamy IPv6 - może to powodować konflikty przy DNS szczgólnie w przypadku kontrolerów domeny. W przypadków hostów i tak nie będą one przez nas używane, więc można je dla bezpieczeństwa również wyłączyć.

Następnie przechodzimy do ustawień IPv4:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Dodawanie_Do_Domeny\IP4.PNG }}CTF\Administracja\Windows_Server\Dodawanie_Do_Domeny\IP4.PNG)

Następnie przechodzimy do części właściwej. Adres hosta możemy również ustawić jako statyczny - niech to będzie 10.0.0.2 z maską /24 (255.255.255.0 - maska niech pozostanie taka sama dla wszystkich urządzeń w sieci)

W polu "Preferred DNS server" ustaiwamy adres głównego kontrolera domeny, a w polu "Alternate DNS server" adres zapasowego kontrolera domeny (jeżeli jest, jeżeli nie to zostawiamy puste)

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Dodawanie_Do_Domeny\IP5.PNG }}CTF\Administracja\Windows_Server\Dodawanie_Do_Domeny\IP5.PNG)

Ustawianie adresów IP zostało również omówione w artykule dotyczącym tworzenia kontrolera domeny

Na tak przygotowanej maszynie możemy przejść do dodawania jej do domeny.

*Przedstawię tutaj wszystkie znane mi sposoby dodawania komputera do domeny - kolejność pozostaje przypadkowa*

### 1. Przez właściwości komputera

Pierwszy i najczęściej używany przeze mnie sposób polega na dodaniu komputera do domeny w jego właściwościach. W tym celu otwieramy np. File Explorer i klikamy PPM na This PC -> Properties

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Dodawanie_Do_Domeny\WK1.PNG }}CTF\Administracja\Windows_Server\Dodawanie_Do_Domeny\WK1.PNG)

Przechodzimy do "Advanced system settings":

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Dodawanie_Do_Domeny\WK2.PNG }}CTF\Administracja\Windows_Server\Dodawanie_Do_Domeny\WK2.PNG)

Następnie do "Change..." w zakładce "Computer Name":

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Dodawanie_Do_Domeny\WK3.PNG }}CTF\Administracja\Windows_Server\Dodawanie_Do_Domeny\WK3.PNG)

W polu "Domain:" wpisujemy nazwę naszej domeny

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Dodawanie_Do_Domeny\WK4.PNG }}CTF\Administracja\Windows_Server\Dodawanie_Do_Domeny\WK4.PNG)

Następnie musimy się zalogować na konto w domenie i po niedługim czasie oczekiwanie maszyna zostaje dodana do domeny

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Dodawanie_Do_Domeny\WK5.PNG }}CTF\Administracja\Windows_Server\Dodawanie_Do_Domeny\WK5.PNG)

Po zamknięciu komunikatu wymagany będzie restart komputera. Po restarcie widzimy, że komputer został poprawnie dodany do domeny:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Dodawanie_Do_Domeny\WK6.PNG }}CTF\Administracja\Windows_Server\Dodawanie_Do_Domeny\WK6.PNG)

### 2. Przez ustawienia komputera

Kolejnym sposobem na dodanie komputera do domeny s ustawienia komputera. Ustawienia komputera znajdziemy wchodząc do menu start i klikając w zębatkę po lewej stronie:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Dodawanie_Do_Domeny\UK1.PNG }}CTF\Administracja\Windows_Server\Dodawanie_Do_Domeny\UK1.PNG)

Następnie przechodzmy do zakładki "Accounts":

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Dodawanie_Do_Domeny\UK2.PNG }}CTF\Administracja\Windows_Server\Dodawanie_Do_Domeny\UK2.PNG)

Potem przechodzimy do zakładki "Access work or school" i klikamy w "Connect":

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Dodawanie_Do_Domeny\UK3.PNG }}CTF\Administracja\Windows_Server\Dodawanie_Do_Domeny\UK3.PNG)

Po załadowaniu się okienka w przypadku środowiska lokalnego klikamy w przycisk na dole "Join this device to a local Active Directory domain":

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Dodawanie_Do_Domeny\UK4.PNG }}CTF\Administracja\Windows_Server\Dodawanie_Do_Domeny\UK4.PNG)

Nastepnie podajemy nazwę naszej domeny:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Dodawanie_Do_Domeny\UK5.PNG }}CTF\Administracja\Windows_Server\Dodawanie_Do_Domeny\UK5.PNG)

Logujemy się na konto administratora domeny:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Dodawanie_Do_Domeny\UK6.PNG }}CTF\Administracja\Windows_Server\Dodawanie_Do_Domeny\UK6.PNG)

Ustalamy nazwę oraz rodzaj konta użytkownika danego komputera. Mozemy wybrać domyślnego, standardowego użytkownika lub konto o randze administratora:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Dodawanie_Do_Domeny\UK7.PNG }}CTF\Administracja\Windows_Server\Dodawanie_Do_Domeny\UK7.PNG)

Na koniec musimy zrestartować nasz komputer. Po restarcie komputer zostanie dołączony do domeny:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Dodawanie_Do_Domeny\UK8.PNG }}CTF\Administracja\Windows_Server\Dodawanie_Do_Domeny\UK8.PNG)

### 3. PowerShell

Uruchamiamy PowerShell jako administrator. Następnie dodajemy komputer do domeny za pomocą polecenia:

*Add-Computer -DomainName [nazwa domeny] -Credential [nazwa domeny]\[nazwa użytkownika]@[nazwa domeny]*

Tak skonstruowana komenda powoduje wyskoczenie okienka, w którym mamy już wypełniony login (zawarty w poleceniu w przełączniku -Credential) i musimy wpisać hasło

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Dodawanie_Do_Domeny\PS1.PNG }}CTF\Administracja\Windows_Server\Dodawanie_Do_Domeny\PS1.PNG)

Po poprawnym wpisaniu hasła, uwierzytelnieniu, dostaniemy komunikat, że zmiany zostaną zastosowane po restarcie komputera. Tym samym komputer po restarcie zostanie automatycznie dodany do domeny:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Dodawanie_Do_Domeny\PS2.PNG }}CTF\Administracja\Windows_Server\Dodawanie_Do_Domeny\PS2.PNG)

### 4. Command Line - djoin

djoin jest poleceniem uruchamianym w CMD, które pozwala na dodanie komputera do domeny w trybie offline. Aby z niego skorzystać najpierw musimy utworzyć za pomocą CMD plik tekstowy na kontrolerze domeny, który będzie zawierał informacje dotyczące dołączanego urządzenia, tworzył obiekt komputera w Active Directory.

Przechodzimy więc do kontrolera domeny i uruchamiamy CMD jako administrator. Następnie wpisujemy nastepującą komendę:

*djoin /provision /domain [nazwa domeny] /machine [nazwa maszyny/komputera/urządzenia] /savefile [nazwa pliku].txt*
																																			
![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Dodawanie_Do_Domeny\DJ1.PNG }}CTF\Administracja\Windows_Server\Dodawanie_Do_Domeny\DJ1.PNG)

Utworzony plik znajdujemy w lokalicacji, w której przebywaliśmy podczas uruchomienia polecenia djoin:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Dodawanie_Do_Domeny\DJ2.PNG }}CTF\Administracja\Windows_Server\Dodawanie_Do_Domeny\DJ2.PNG)

Następnie utworzony plik musimy przenieść na komputer, który chcemy dołączyć do domeny - w przypadku zastosowania tego polecenia offline np. przez Pendrive. Po przeniesieniu pliku na urządzeniu, które chcemy dodać do domeny odpalamy CMD jako administrator i wydajemy polecenie:

*djoin /requestodj /loadfile [ścieżka do pliku] /windowspath %systemroot% /localos*

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Dodawanie_Do_Domeny\DJ3.PNG }}CTF\Administracja\Windows_Server\Dodawanie_Do_Domeny\DJ3.PNG)

Zgodnie z informacją w konsoli, po restarcie komputera zmiany zostaną zastosowane czyli komputer zostanie dołąćzony do domeny