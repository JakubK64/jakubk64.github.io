---
layout: post
title: Tworzenie zapasowego kontrolera domeny - Windows Serwer 2019
published: true
tags: DC Domain Kontroler Controler Domeny zapasowy replikacja
---

### Ogólnie o zapasowym kontrolerze domeny

Zapasowy kontroler domeny replikuje dane od głównego kontrolera domeny. Oznacza to, że jest on w zasadzie dokładną kopią głównego kontrolera, który cały czas działa i jest gotowy, w razie konieczności, przejąć jego zadania.
Wyobraźmy sobie sytuację, że w firmie z jakiegoś powodu następuje awaria i główny kontroler domeny przestaje działać. Gdyby był on jedynym kontrolerem w domenie działalność firmy zostaje w zasadzie zatrzymana, ponieważ podstawowe funkcje domeny jak np. możliwość logowania się użytkowników na swoje konta, przestają działać. W przypadku, gdy mamy poprawnie skonfigurowany i działający zapasowy kontroler domeny cały ruch zostanie przekierowany na niego i tym samym stanie się on tymczasowo głównym kontrolerem. Aby wszystko zadziałało poprawnie zapasowy kontroler musi być oczywiście dokładną kopią podstawowego kontrolera. Po to właśnie potrzebna jest replikacja danych - zarówno podczas promowania zapasowego kontrolera domeny, aby "otrzymał" on wszystkie niezbędne informacje o domenie (np. obiekty AD) , ale także trwająca cały czas synchronizacja, aby po dodaniu nowego użytkownika na głównym kontrolerze, kontroler zapasowy również miał taki obiekt w swojej bazie.

### Ustawienia IP

Na początek ponownie należy skonfigurować statyczny adres IP naszego kontrolera.
(cały proces doładniej opisany w artykule o Tworzeniu Kontrolera Domeny)

Na początek wyłączamy IPv6 i wchodzimy w ustawienia IPv4:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Zapasowy_Kontroler\IP1.PNG }}CTF\Administracja\Windows_Server\Zapasowy_Kontroler\IP1.PNG)

Następnie ustawiamy adres IP oraz maskę. W moim przypadku adresem będzie 10.0.0.16, maska /24 (255.255.255.0)
Ustawiamy także adresy DNS - adresem preferowanym będzie adres głównego kontrolera domeny, adresem alternatywnym 127.0.0.1 wskazujące na serwer, na którym się znajdujemy

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Zapasowy_Kontroler\IP2.PNG }}CTF\Administracja\Windows_Server\Zapasowy_Kontroler\IP2.PNG)

Następnie należy ustawić adres zapasowego kontrolera domeny jako alternatywny adres DNS na głównym kontrolerze domeny:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Zapasowy_Kontroler\IP3.PNG }}CTF\Administracja\Windows_Server\Zapasowy_Kontroler\IP3.PNG)

Na koniec warto sprawdzić czy wszystko działa poprawnie i kontrolery widzą się wzajemnie w sieci. Możemy to zrobić np. przez użycie polecenia ping w Windows Command Line:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Zapasowy_Kontroler\IP4.PNG }}CTF\Administracja\Windows_Server\Zapasowy_Kontroler\IP4.PNG)

### Promowanie zapasowego kontrolera domeny

(Przechodząc do promowania zapasowego kontrolera domeny zakładam, że na maszynie z Windows Serwer zainstalowana jest już rola ADDS)

Na początek przechodzimy więc do promowania kontrolera domeny:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Zapasowy_Kontroler\DC1.PNG }}CTF\Administracja\Windows_Server\Zapasowy_Kontroler\DC1.PNG)

Następnie w kreatorze wybieramy opcję dołączenia kontrolera do istniejącej domeny. Aby wybrać, do której domeny chcemy dołączyć kontroler wybieramy najpierw zaznaczony przycisk:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Zapasowy_Kontroler\DC2.PNG }}CTF\Administracja\Windows_Server\Zapasowy_Kontroler\DC2.PNG)

W okienku logujemy się za pomocą kontra administratora w domenie wg schematu [nazwa_użytkownika]@[nazwa_domeny] i klikamy ok. W podsumowaniu zobaczymy nazwę domeny oraz kontro administratora:
Jeżeli w tym miejscu coś nie zadziałało poprawnie najczęstsze przyczyny to:
- Główny kontroler domeny jest wyłączony i należy go włączyć
- Maszyna nie jest w tej samej sieci (w przypadku Hyper-V switchu) co główny kontroler domeny
- Źle wpisaliśmy nazwę domeny, użytkownika lub hasło

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Zapasowy_Kontroler\DC4.PNG }}CTF\Administracja\Windows_Server\Zapasowy_Kontroler\DC4.PNG)

Na kolejnej stronie musimy zachować włączoną opcję DNS serwer. Global Catalog może zostać, ale nie jest on niezbędny. Oprócz tego ustawiamy hasło odzyskiwania DSRM:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Zapasowy_Kontroler\DC5.PNG }}CTF\Administracja\Windows_Server\Zapasowy_Kontroler\DC5.PNG)

Na następnej stronie widzimy delegacje DNS. Nie ustawialiśmy jej na głównym kontrolerze i w tym przypadku również możemy ją pominąć

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Zapasowy_Kontroler\DC6.PNG }}CTF\Administracja\Windows_Server\Zapasowy_Kontroler\DC6.PNG)

Następne okienko jest szczególnie istotne, ponieważ tutaj znajdują się opcję całego sedna działania zapasowego kontrolera domeny.
Mamy dostępne 2 opcje - IFM i replikację przez sieć.
Gdy zostawimy tylko replikację przez sieć i wskażemy z listy główny kontroler domeny, zapasowy kontroler zacznie synchronizować wszystkie elementy AD z głównego kontrolera. W naszym środowisku "testowym", które jest praktycznie puste zadziała to bez zarzutu. W przypadku, gdy nasze AD jest bardzo rozbudowane ustawienie replikacji przez sieć podczas promowania serwera wygeneruje niepotrzebny ruch sieciowy, który może w dużej mierze zapchać naszą sieć, spowodować opóźnienia w przesyłaniu plików czy generalnie używaniu sieci pracownikom itp.
Zdecydowanie lepszą opcją na początek jest IFM (Install From Media) a dopiero później replikacja przez sieć. IFM polega z grubsza na utworzeniu nośnika ze wszystkimi danymi z głównego kontrolera domeny i przeniesienie tego z nośnika na zapasowy kontroler domeny.
(użycie IFM zostało opisane niżej, przechodzimy dalej wskazująć główny kontroler domeny do replikacji)

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Zapasowy_Kontroler\DC7.PNG }}CTF\Administracja\Windows_Server\Zapasowy_Kontroler\DC7.PNG)

Na następnej strony ustawiamy (lub zostawiamy) ścieżki do lokalizacji, gdzie mają być zapisywane wymienione elementy:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Zapasowy_Kontroler\DC8.PNG }}CTF\Administracja\Windows_Server\Zapasowy_Kontroler\DC8.PNG)

Natępnie widzimy podsumowanie:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Zapasowy_Kontroler\DC9.PNG }}CTF\Administracja\Windows_Server\Zapasowy_Kontroler\DC9.PNG)

Na kolejnej stronie kreator sprawdzi czy zostały spełnione wszystkie wymagania do instalacji. Jeżeli nie znajdzie żadnego błędu, możemy przejść do instalacji.

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Zapasowy_Kontroler\DC10.PNG }}CTF\Administracja\Windows_Server\Zapasowy_Kontroler\DC10.PNG)

Po zakończeniu instalacji maszyna powinna się zrestartować. Po restarcie zapasowy kontroler domeny powinien działać poprawnie

### Replikacja przez IFM

Aby użyć medium IFM trzeba go wcześniej przygotować ma głównym kontrolerze domeny. W tym celu odpalamy Windows Command Line i uruchamiamy narzędzie "ntdsutil"
Utworzenie medium IFM jest dość proste, robimy to w następujący sposób: (po uruchomieniu narzędzia ifm w CMD wpisujemy create (aby utworzyć) full (skopiować całość) oraz ścieżkę, gdzie chcemy, aby folder z plikami został utworzony)

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Zapasowy_Kontroler\IFM1.PNG }}CTF\Administracja\Windows_Server\Zapasowy_Kontroler\IFM1.PNG)

Po zakończeniu dostajemy 2 pliki umieszczone w odpowiednim miejscu w folderze - insteresuje nas pierwszy z nich. Jest to wyeksportowana baza danych Active Directory Domain Services:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Zapasowy_Kontroler\IFM3.PNG }}CTF\Administracja\Windows_Server\Zapasowy_Kontroler\IFM3.PNG)

Tworzenie zapasowego kontrolera przez IFM przebiega podobnie jak jest pokazane wyżej, z tą różnicą, że przygotowany wcześniej plik ntds.dit trzeba przenieść na maszynę, na której tworzymy zapasowy kontroler domeny i wskazać ten plik przy promowaniu kontrolera domeny. Dane odnośnie Active Directory zostaną "pobrane" ze wskazanego pliku, a dopiero nowe elementy Active DIrectory będą replikowane przez sieć