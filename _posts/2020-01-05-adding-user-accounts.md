---
layout: post
title: Różne sposoby tworzenia konta użytkownika w domenie
published: true
tags: domain domena dodawanie adding cmd powershell
---

### 1. Poprzez ADUC - Active Directory Users and Computes

Na głównym kontrolerze domeny uruchamiamy Server Maganer. Następnie przechodzimy do "Tools" -> "Active Directory Users and Computers"

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Tworzenie_Kont_Uzytkownika/ADUC1.PNG }}CTF\Administracja\Windows_Server\Tworzenie_Kont_Uzytkownika\ADUC1.PNG)

W okienku, które widzimy pojawia się nam struktura organizacyjna AD naszej domeny oraz odpowiednio pogrupowane zawarte w domenie obiekty. Nas interesują konta użytkowników, więc przechodzimy do zakładki "Users".

Następnie klikamy PPM w wolną przestrzeń, w "New" i "User"

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Tworzenie_Kont_Uzytkownika/ADUC2.PNG }}CTF\Administracja\Windows_Server\Tworzenie_Kont_Uzytkownika\ADUC2.PNG)

Następnie widzimy kreator tworzenia użytkownika. Wpisujemy odpowiednie dane. W przypadku tworzenia loginów użytkowników warto zastosować własną politykę tworzenia loginów dla wszystkich kont użytkowników w domenie (np. login użytkownika to zawsze [imie].[nazwisko] lub [pierwsza litera imienia][nazwisko] itp.). Ułatwia to zarządzanie użytkownikami, gdy pojawia się ich coraz więcej. Mimo potencjalnej podatności stosowania takiej polityki na ataki typu brute-force jest to powszechnie wykorzystywana praktyka.

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Tworzenie_Kont_Uzytkownika/ADUC2.PNG }}CTF\Administracja\Windows_Server\Tworzenie_Kont_Uzytkownika\ADUC3.PNG)

W kolejnym oknie ustawiamy hasło użytkownika oraz związane z nim opcje. Mamy do wyboru kilka podstawowych opcji:
- użytkownik musi zmienić hasło przy następnym logowaniu - podczas tworzenia konta użytkownika możemy nadać mu np. losowo wygenerowane hasło, które będzie mógł zmienić na swoje, według swojego uznania przy kolejnym logowaniu (w tym przypadku pierwszym)
- użytkownik nie może zmienić ustawionego przez nas hasła - opcja narzucająca użytkownikowi ustalone przez nas hasło, często może być bezpieczeniejsza choć, jak to najczęściej w takich przypadkach bywa, mniej wygodna dla użytkownika. Jako administrator możemy wygenerować i ustawić silne hasło i uniknąć częstej sytuacji ustawiania zbyt słabych, łatwych do zapamiętania i przewidzenia haseł, przez użytkowników.
- hasło użytkownika nigdy nie wygasa - w przypadku ustalenia zasad dotyczących haseł typu "hasło musi być zmieniane co 3 miesiące" zaznaczenie takiej opcji pozwoli uniknąć obowiązku zmiany tego hasła
- tworzone przez nas konto będzie nieaktywne

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Tworzenie_Kont_Uzytkownika/ADUC4.PNG }}CTF\Administracja\Windows_Server\Tworzenie_Kont_Uzytkownika\ADUC4.PNG)

Ostatnie okno jest podsumowaniem:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Tworzenie_Kont_Uzytkownika/ADUC5.PNG }}CTF\Administracja\Windows_Server\Tworzenie_Kont_Uzytkownika\ADUC5.PNG)

Jak widzimy konto zostało utworzone poprawnie:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Tworzenie_Kont_Uzytkownika/ADUC6.PNG }}CTF\Administracja\Windows_Server\Tworzenie_Kont_Uzytkownika\ADUC6.PNG)

### 2. Poprzez ADAC - Active Directory Administrative Center

W Server Managerze wchodzimy w "Tools" -> "Active Directory Administrative Center"

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Tworzenie_Kont_Uzytkownika/ADAC1.PNG }}CTF\Administracja\Windows_Server\Tworzenie_Kont_Uzytkownika\ADAC1.PNG)

Następnie wchodzimy w naszą domenę, "Users", klikamy PPM w pustej przestrzeni, w "New" -> "User"

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Tworzenie_Kont_Uzytkownika/ADAC2.PNG }}CTF\Administracja\Windows_Server\Tworzenie_Kont_Uzytkownika\ADAC2.PNG)

W kreatorze od razu widzimy dużo więcej dostępnych opcji niż w przypadku ADUC (te same opcje można znaleźć we właściwościach utworzonego już konta jeżeli korzystamy z ADUC_

Musimy wypełnić podstawowe pola jak login, hasło użytkownika i pole "Full Name" reszta pół jest opcjonalna. W niewymaganych opcjach możemy ustawić wiele rzeczy jak np. opcje dotyczące hasła, szczegołowe informacje o użytkowniku, jego dane osobowe, dodać go do grupy czy jednostki organizacyjnej w domenie i wiele więcej.

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Tworzenie_Kont_Uzytkownika/ADAC3.PNG }}CTF\Administracja\Windows_Server\Tworzenie_Kont_Uzytkownika\ADAC3.PNG)

Jak widzimy, konto użytkownika zostało utworzone poprawnie:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Tworzenie_Kont_Uzytkownika/ADAC4.PNG }}CTF\Administracja\Windows_Server\Tworzenie_Kont_Uzytkownika\ADAC4.PNG)

### 3. Poprzez PowerShell

W PowerShell użytkownika możemy utworzyć za pomocą komendy New-ADUser (New Active Directory User) z odpowiednimi przęłącznikami.

Wpisujemy więc podaną komendę a następnie precyzujemy dane dotyczące konta za pomocą następujących przęłączników:
- "-Name" po którym wpisujemy Imię
- "-Surname" po którym wpisujemy Nazwisko
- "-SamAccountName" po którym wpisujemy login uzytkownika
- "-Path" po którym określamy ścieżkę DN (dystinguished name) określającą dokładne położenie obiektu w drzewie AD DS. Składa się ona z 3 elementów: DC (domain component), OU (Organizational Unit), CN (common name). Przykładowo ścieżką DN dla użytkownika Ponury Bazyl który znajdowałby się w jednostce organizacyjnej IT a niej w "podjednostce" Warszawa wyglądała by następująco: "CN=Jan Nowak,OU=IT,OU=Warszawa,DC=Ocean,DC=Local" (taką ścieżkę zawsze budujemy "od tyłu", od elementu położonego "najgłębiej" w drzewie).
- "-AccountPassword", który można ustawić np. w formie "Read-Host -AsSecureString "Password"" - czyli przed utworzeniem konta zostaniemy zapytani o hasło wiadomością zawartą w cudzysłowiu, a następnie takie hasło zostanie zaszyfrowane w odpowiedni sposób
- "-Enabled" przęłącznik, który decyduje o tym, czy utworzone konto będzie aktywne czy nie. W tym przypadku chcemy żeby było aktywne, więc ustawiamy wartość "$true"

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Tworzenie_Kont_Uzytkownika/PS1.PNG }}CTF\Administracja\Windows_Server\Tworzenie_Kont_Uzytkownika\PS1.PNG)

Aby sprawdzić, czy konto zostało utworzone możemy użyć komendy "Get-ADUser [login]". Jak widzimy, zostało ono utworzone poprawnie:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Tworzenie_Kont_Uzytkownika/PS2.PNG }}CTF\Administracja\Windows_Server\Tworzenie_Kont_Uzytkownika\PS2.PNG)

### 4. Poprzez CMD

Odpalamy CMD jako administrator i używamy następującej komendy:
*dsadd user [ścieżka DN] -pwd *

W ścieżce zawieramy informacje o użytkowniku i położeniu tego obiektu w drzewie ADDS

Następnie wpisujemy hasło dla użytkownika i potwierdzamy je wpisując je kolejny raz

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Tworzenie_Kont_Uzytkownika/CMD1.PNG }}CTF\Administracja\Windows_Server\Tworzenie_Kont_Uzytkownika\CMD1.PNG)

