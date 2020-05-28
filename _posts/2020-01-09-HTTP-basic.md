---
layout: post
title: Protokuł HTTP (notatki)
published: true
tags: ksiazka bezpieczenstwo aplikacji webowych http protokol
---

### Ogólnie o HTTP/1.1 
Najczęściej spotykamy się z komunikacją HTTP z wykorzystaniem TCP (ewentualnie UDP)

Komunikacja HTTP realizowana jest przez wysłanie *request* z hosta do serwera i wygenerowanie *response* przez server

Pierwsza linijka rządania HTTP zawiera zawsze 3 elementy oddzielone spacjami: 
- metodę (GET,POST,HEAD itp.)
- adres URL (adres na stronie, który może być względny lub bezwzględny, np. "http://przyklad.pl/admin/" lub po prostu "/admin", zdecydowanie częściej stosowany jest zapis względny)
- wersję protokołu (np. HTTP/1.1)

Ciąg "0d0a" w zapisie szesnastkowym, w "normalnym CRLF to separator linii w protokole HTTP. Jest o tyle istotny, że żądanie zawierające musi zostać wysłane z 2 pustymi liniami a w zasadzie 2 znakami kończącymi linie (inaczej serwer nie odpowie):

!!!jeżeli dana metoda jest zabroniona na serwerze warto spróbować zapisać ją np. małymi literami. Np. jeżeli "PUT" jest zablokowana, to można spróbować "pUt" itp.

Nagłówek "Host" jest jedynym wymaganym nagłówkiem w przypadki HTTP/1.1

Parametry można przekazywać zarówno poprzez linijkę żądania i "body" w ramach tego samego żądania. 

Zmiana nagłówka "Content-Length" na mniejszą wartość nie spowoduje wysłania określonej części żądania. Zostanie ono wysłane całe, a dopiero web server odpowiednio przetworzy całość (można tym np. ominąć firewall).

### Wysyłanie żądania HTTP
Poprawny request z 2 pustymi liniami - 2 linie ze względu na specyfikę programu (2 znakami CRLF kończącymi linię)

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/HTTP/Request1.PNG }}CTF/Notatki/HTTP/Request1.PNG)

Poprawna odpowiedź serwera:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/HTTP/Response1.PNG }}CTF/Notatki/HTTP/Response1.PNG)

Przykład błędnego requesta z tylko 1 pustą linią:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/HTTP/Request2.PNG }}CTF/Notatki/HTTP/Request2.PNG)

Odpowiedź serwera (a w zasadzie brak odpowiedzi, komunikat o przekroczeniu czasu żądania):

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/HTTP/Response2.PNG }}CTF/Notatki/HTTP/Response2.png)

### Przeszukiwanie zawartości serwera metodą HEAD

W celu przeszukiwania zawartości serwera możemy wykorzystać metodę HEAD, która zwraca odpowiedź serwera bez ciałą tj. sam nagłówek odpowiedzi. Przykład:

Nagłówek konstruujemy w taki sposób, że zamieniamy 2 element 1 linii żądania (adres URL) na różne ścieżki np. plików, które chcemy przetestować (czy są na serwerze)

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/HTTP/HEAD1.PNG }}CTF/Notatki/HTTP/HEAD1.png)

To, czy dany element jest na serwerze określamy na podstawie kodu opdowiedzi z 1 linii odpowiedzi serwera. Podstawowe kody odpowiedzi:

- 200 - OK, ścieżka (czy bardziej URL) podany w żądaniu istnieje:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/HTTP/HEAD2.PNG }}CTF/Notatki/HTTP/HEAD2.png)

- 301 - Moved Permanently, zasób został przeniesiony/znajduje się w innej lokalizacji (która jest podana w odpowiedzi w nagłówku "Location":

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/HTTP/HEAD3.PNG }}CTF/Notatki/HTTP/HEAD3.PNG)

- 404 - Not Found, podana ścieżka (URL) nie jest poprawna, taki zasób nie istnieje

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/HTTP/HEAD4.PNG }}CTF/Notatki/HTTP/HEAD4.PNG)

- 403 - Forbidden, zasób istnieje, URL jest poprawny, ale nie mamy uprawnień do "wyświetlenia tego pliku/katalogu"

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/HTTP/HEAD5.PNG }}CTF/Notatki/HTTP/HEAD5.PNG)

- 400 - Bad Request, w przypadku błędnie utworzonego żądania

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/HTTP/HEAD6.PNG }}CTF/Notatki/HTTP/HEAD6.PNG)


### Zastosowanie metody OPTIONS

Jeżeli naszym celem jest poznanie obsługiwanych przez dany serwer metod, możemy wysłać żądanie z metodą OPTIONS. Wygląda to następująco:

Proste żądanie:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/HTTP/OPTIONS1.PNG }}CTF/Notatki/HTTP/OPTIONS1.png)

W odpowiedzi zwracamy uwagę na nagłówek "Allow", która wyświetli wszystkie obsługiwane przez serwer metody (przynajmniej w teorii, bo informacja zwracana przez serwer czasami może być nieprawdziwa):

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/HTTP/OPTIONS2.PNG }}CTF/Notatki/HTTP/OPTIONS2.PNG)

### Metoda PUT

Metoda PUT jest szczególnie niebezpieczna, ponieważ pozwala na umieszczenie plików na serwerze. Takie pliki mogą zawierać złośliwy kod, który zostanie wykonany na serwerze. Przykład żądania z metodą PUT:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/HTTP/PUT1.PNG }}CTF/Notatki/HTTP/PUT1.PNG)

Serwer może zwrócić odpowiedzi o różnych kodach, np.:

- 405 - Methon Not Allowed, czyli serwer nie obsługuje metody PUT i nie ma możliwości umieszczenia pliku na serwerze:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/HTTP/PUT2.PNG }}CTF/Notatki/HTTP/PUT2.PNG)

- 201 - Created, plik z kodem został pomyślnie utworzony na serwerze (brak screena, brak serwera)

### Inne istotne nagłówki

- Cookie (w żądaniu) - wysyła ciasteczko lub ciasteczka do serwera (Zawsze Cookie, nie Cookies)
- Set-Cookie (w odpowiedzi) - ustawia ciasteczko klientowi
- Server (w odpowiedzi) - czasem zdradza typ/wersję wykorzystywanego serwera HTTP
- Location (w odpowiedzi) - realizuje przekierowanie klienta na inny adres
- Referer (w żądaniu) - jego wartością jest adres URL strony poprzednio odwiedzanej przez użytkownika. Czyli np. jeżeli na stronie test1.pl jest link do strony test2.pl to referer ma wartość test1.pl i wyśle tą wartość do serwera pod adresem test2.pl jeżeli klikniemy w link. Na skutek tego mogą wyciekać pewne wrażliwe dane (typu wartość zmiennej SID odpowiedzialnej za identyfikowanie sesji). Umożliwia to wyciek danych nawet w przypadku używania HTTPS, czyli zaszyfrowania ścieżki w URL, o ile obie strony używają HTTPS.

### Przekazywanie wartości do aplikacji protokołem HTTP

Parametry mogą być przekazywane w nagłówkach żądania HTTP i tam występowały błędy bezpieczeństwa (SQL Injection w "User-Agent" oraz Path Traversal w "Accept" we frameworku Ruby on Rails).
Przekazywane parametry najczęściej widzimy w URL przy zapytaniach metodą "GET" na takiej zasadzie:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/HTTP/GET1.PNG }}CTF/Notatki/HTTP/GET1.png)

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/HTTP/GET2.PNG }}CTF/Notatki/HTTP/GET2.png)

Parametry i ich wartości można również przekazywać używając kodowania procentowego tj.%[kod ASCII zapisany szesnastkowo]. Spacja jest reprezentowana przez "+", "+" trzeba zapisać w kodzie procentowym %2b

Zamiast metody GET można również wykorzystać metodę POST. Wtedy żądanie wygląda następująco (parametry i ich wartości są przekazywane w tym przypadku w "body" żądania):

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/HTTP/POST1.PNG }}CTF/Notatki/HTTP/POST1.png)

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/HTTP/POST2.PNG }}CTF/Notatki/HTTP/POST2.png)




