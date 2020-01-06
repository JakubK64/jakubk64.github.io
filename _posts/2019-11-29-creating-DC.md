---
layout: post
title: Tworzenie kontrolera domeny - Windows Serwer 2019
published: true
tags: Hyper-V VM maszyna windows server 2019 2016 domain controller DC 
---

### Czym jest kontroler domeny?

Kontroler domeny jest komputerem w domenie, który zarządza realizacją zadań związanych z bezpieczeństwem pomiędzy użytkownikiem a Domeną Windows. Kontroluje on w jaki sposób użytkownicy uzyskują dostęp i konfigurują i korzystają z zasobów w obrębie domeny.
Pełni on także rolę administratora w domenie.

Gdzie i dlaczego stosuje się kontroler domeny?
Weźmy dla przykładu małą firmę. Ma ona 10 komputerów i 10 użytkowników. Aby móc zarządzać każdym komputerem, dać możliwość pracownikom uzyskiwania dostępu do ich stanowisk roboczych itp. możemy dołączyć wszystkie komputery do domeny. Na kontrolerze domeny możemy skonfigurować konta użytkowników (pracowników firmy), aby umożliwić im dostęp do odpowiednich zasobów i możliwość autoryzacji na każdym komputerze w firmie. Nie wymaga to od nich używania tylko 1 komputera, do którego mają przypisane konto. Podobnie, jeżeli chcą uzyskać dostęp do zasobów firmowych z prywatnych komputerów. Można wtedy dołączyć taki komputer do domeny w trybie JOIN i umożliwić im dostęp do tych zasobów.
Dzięki kontrolerowi domeny można skonfigurować dużo zasad, polityk dotyczących min. bezpieczeństwa od tego, że możemy pozwolić na logowanie użytkownikom tylko w ich godzinach pracy używając GPO (Group Policy Object), możemy tworzyć jednostki organizacyjne w celu pogrupowania ludzie z podobnymi uprawnieniami np. pracowników działu HR, co ułatwia zarządzanie dostępem konkretnych użytkowników do przydzielonych im zasobów, po zmapowanie dysków sieciowych konkretnych użytkowników, zablokowanie możliwości wejścia do Panelu Sterowania czy wymuszenie tapety firmowej na komputerze. Możliwości i funkcji jest bardzo dużo.
Skracając i podsumowując: Kontroler domeny ułatwia zarządzanie obiektami (użytkownikami, komputerami itp.) w domenie.

Do utworzenia Kontrolera domeny (w tym przypadku) musimy mieć maszynę wirtualną z zainstalowanym systemem Windows Server

### Ustawianie statycznego adresu IP kontrolera domeny

Na początek na maszynie ustawimy statyczny adres IP. Będzie nam to potrzebne, aby również statycznie ustawić adres preferowanego serwera DNS, pod którym hosty w domenie będą szukać kontrolera domeny. Pozostawienie dynamicznego adresu IP może spowodować błędy, ponieważ hosty przy zmianie IP servera nie będą mogły odnaleźć go w sieci. 

Jest kilka sposobów na zmianę adresu IP maszyny, jednym z nich jest wykorzystanie Panelu Sterowania:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Instalacja_Kontrolera_Domeny\Panel1.PNG }}CTF\Administracja\Windows_Server\Instalacja_Kontrolera_Domeny\Panel1.PNG)

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Instalacja_Kontrolera_Domeny\Panel2.PNG }}CTF\Administracja\Windows_Server\Instalacja_Kontrolera_Domeny\Panel2.PNG)

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Instalacja_Kontrolera_Domeny\Panel3.PNG }}CTF\Administracja\Windows_Server\Instalacja_Kontrolera_Domeny\Panel3.PNG)

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Instalacja_Kontrolera_Domeny\Panel4.PNG }}CTF\Administracja\Windows_Server\Instalacja_Kontrolera_Domeny\Panel4.PNG)

W tym miejscu dobrze jest wyłączyć IPv6, o ile oczywiście nie planujemy z niego korzystać (może to powodować problemy przy okazji konfigurowania DNS-ów). Statyczny adres ustawiamy w wersji IPv4:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Instalacja_Kontrolera_Domeny\Panel5.PNG }}CTF\Administracja\Windows_Server\Instalacja_Kontrolera_Domeny\Panel5.PNG)

W tym miejscu należy przełączyć opcję automatycznego przydzielania adresu IP na statyczną. Adres IP w zasadzie zależy tylko od nas, najlepiej ustawić go tak, aby był łatwy do zapamiętania, gdyż będzie potrzebny do podania wspomnianych preferowanych adresów serwera DNS na hostach w domenie. Ustalmy więc np. adres 10.0.0.15

Niezwykle istotna w tym miejscu jest maska. Musi być ona koniecznie taka sama dla wszystkich urządzeń znajdujących się w naszym środowisku (można oczywiście dodawać inne podsieci, routery, konfigurować tabele routingu itd. ale to nie tekst od konfigurowaniu środowiska sieciowego). Ustawienie tej samej maski ułatwi zadanie (najlepiej, żeby była to standardowa sieć klasy C, czyli maska /24 (255.255.255.0))

Default gateway ustawimy, gdy będziemy chcieli łączyć się z innymi podsieciami, internetem itp. W tym przypadku nie jest to konieczne.

Jeżeli chodzi o preferowany adres DNS głównym kontrolerem domeny będzie ten aktualnie przez nas tworzony. Tak więc możemy ustawić tak samo jak na każdej innej maszynie w domenie jego adres IP (10.0.0.15) lub w tym przypadku adres lokalny, wskazujący na maszynę na której aktualnie się znajdujemy (wskazanie "sam na siebie") - 127.0.0.1. W obu przypadkach zadziałą to poprawnie.

Jeżeli chodzi o adres alternatywny to ustawia się go np. gdy mamy już działający zapasowy kontroler domeny. Wtedy ustawiamy adres takiego kontrolera domeny (który replikuje dane od podstawowego kontrolera domeny). Opcja szczególnie ważna na hostach, aby w przypadku awarii głównego serwera DNS wszystkie usługi nie stanęły, tylko tymczasowo "przeniosły się" na zapasowy kontroler domeny.

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Instalacja_Kontrolera_Domeny\Panel6.PNG }}CTF\Administracja\Windows_Server\Instalacja_Kontrolera_Domeny\Panel6.PNG)

### Instalacja ADDS - Active Directory Domain Services

Aby wypromować komputer do roli kontrolera domeny najpierw musimy zainstalować takie funkcje. W tym celu wchodzimy w aplikację Server Manager (odpala się automatycznie przy starcie Windows Server) i wchodzimy w opcję "Add roles and features":

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Instalacja_Kontrolera_Domeny\ADDS1.PNG }}CTF\Administracja\Windows_Server\Instalacja_Kontrolera_Domeny\ADDS1.PNG)

Otwiera się nam kreator dodawania ról i funkcji do serwera. Pierwszą stronę, informacyjną, możemy śmiało pominąć.

Na kolejnej stronie wybieramy typ instalacji. W tym miejscu zostawiamy zaznaczoną defaultowo opcję Role-based or feature-based installation.

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Instalacja_Kontrolera_Domeny\ADDS2.PNG }}CTF\Administracja\Windows_Server\Instalacja_Kontrolera_Domeny\ADDS2.PNG)

Nastnie wybieramy Serwer na którym chcemy, żeby dane role czy funkcje zostały zainstalowane. W tym przypadku wybieramy serwer na którym się aktualnie znajdujemy.

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Instalacja_Kontrolera_Domeny\ADDS3.PNG }}CTF\Administracja\Windows_Server\Instalacja_Kontrolera_Domeny\ADDS3.PNG)

Przechodzimy do właściwej części w kreatorze. W tym miejscu możemy wybrać z listy role, które chcemy zainstalować na serwerze. W naszym przypadku będzie do Active Directory Domain Services. Można także zainstalować funkcję DNS Server, ale nawet jeżeli nie zaznaczymy tej opcji zostanie ona automatycznie zainstalowana razem z ADDS

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Instalacja_Kontrolera_Domeny\ADDS4.PNG }}CTF\Administracja\Windows_Server\Instalacja_Kontrolera_Domeny\ADDS4.PNG)

Kreator sam zaznaczy do instalacji opcje, które są wymagane do doinstalowania, aby rola ADDS mogła się zainstalować i działać poprawnie

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Instalacja_Kontrolera_Domeny\ADDS5.PNG }}CTF\Administracja\Windows_Server\Instalacja_Kontrolera_Domeny\ADDS5.PNG)

Nie instalujemy żadnych dodatkowych funkcji niż te, które dodał nam kreator. Przechodzimy dalej

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Instalacja_Kontrolera_Domeny\ADDS6.PNG }}CTF\Administracja\Windows_Server\Instalacja_Kontrolera_Domeny\ADDS6.PNG)

Po pominięciu strony informacyjnej odnośnie ADDS przechodzimy do strony z potwierdzeniem i jednocześnie podsumowaniem instalacji. Instalacja roli ADDS może wymagać restartu maszyny, dlatego wygodnie jest od razu zaznaczyć dostępną na górze opcję, aby zezwolić na automatyczny restart komputera.
Po odhaczeniu zezwolenia na automatyczny restart klikami Install:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Instalacja_Kontrolera_Domeny\ADDS7.PNG }}CTF\Administracja\Windows_Server\Instalacja_Kontrolera_Domeny\ADDS7.PNG)

Instalacja rozpoczęła się. Po jej zakończeniu można jeszcze dla pewności zrestartować maszynę i można przejść do promowania serwera do roli kontrolera domeny

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Instalacja_Kontrolera_Domeny\ADDS8.PNG }}CTF\Administracja\Windows_Server\Instalacja_Kontrolera_Domeny\ADDS8.PNG)

### (OPCJONALNIE) Zmiana nazwy maszyny

W celu łatwiejszej orientacji, z którą maszyną mamy do czynienia czy to w przypadku ustawiania usług DNS, wybierania odpowiedniego serwera czy np. przy dodawaniu i zarządzaniu maszyną w Active Directory warto zmienić jej nazwę na bardziej przyjazną. Podobnie jak w przypadku ustawiania statycznego adresu IP można to zrobić na kilka różnych sposobów, ja przykładowo wykorzystam moim zdaniem najprostszy z nich. W tym celu przechodzimy do Komputera, klikamy prawym przyciskiem myszy i wchodzimy we właściwości:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Instalacja_Kontrolera_Domeny\NAME1.PNG }}CTF\Administracja\Windows_Server\Instalacja_Kontrolera_Domeny\NAME1.PNG)

Następnie klikamy w "Advanced system settings"

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Instalacja_Kontrolera_Domeny\NAME2.PNG }}CTF\Administracja\Windows_Server\Instalacja_Kontrolera_Domeny\NAME2.PNG)

W zakładce "Computer Name" klikame w "Change", wpisujemy swoją nazwę komputera i zatwierdzamy. Zmiana nazwy będzie wymagała restartu maszyny

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Instalacja_Kontrolera_Domeny\NAME3.PNG }}CTF\Administracja\Windows_Server\Instalacja_Kontrolera_Domeny\NAME3.PNG)

### Promowanie serwera do roli kontrolera domeny

Aby wypromować serwer do kontrolera domeny najprościej jest skorzystać z powieadomień w aplikacji Server Manager i kliknąć w "Promote this server to a domain controller":

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Instalacja_Kontrolera_Domeny\DC1.PNG }}CTF\Administracja\Windows_Server\Instalacja_Kontrolera_Domeny\DC1.PNG)

W pierwszym oknie kreatora mamy do wyboru kilka istotnych opcji:
- "Add a domain controller to an existing domain" pozwala jak sama nazwa wskazuje na dodanie kontrolera do istniejącej już domeny. Kliknięcie w którykolwiek z guzików będzie wymagał od nas zalogowania się na konto administratora tej domeny. Po poprawnym zalogowaniu nazwa domeny zostanie dodana w polu "Domain", a maszyna zostanie dodana do domeny.
- "Add a new domain to an existing forest" pozwala na utworzenie nowej domeny i dołączenie jej do istniejącego już lasu domen. Las jest pewnego rodzaju zbiorem domen. Nie może być domeny, która nie należy do żadnego lasu oraz pustego lasu, bez żadnej domeny, więc nie możemy stworzyć nowej domeny nienależącej do żadnego lasu. Las zostanie automatycznie utworzony wraz z domeną i domyślnie będzie miał taką samą nazwę jak ta domena. Las służy przede wszystkim do łączenia domen np. w sytuacji gdy firma kupuje inną firmę, domena kupionej firmy zostaje dołączona do lasu domen firmy kupującej. Tworzy się wtedy relacja zaufania pomiędzy domenami dzięki czemu np. użytkownicy z 1 firmy mogą się logować na komputerach podłączonych do 2 domeny.
- "Add a new forest" tworzy nowy las domen i jednocześnie nową domenę oraz drzewo domen (gdzie tworzona domena będzie korzeniem)

W naszym przypadku nie mamy jeszcze utworzonej ani domeny ani lasu, więc wybierzemy opcję "Add a new forest" oraz podamy nazwę domeny. W moim przypadku będzie to "ocean.local."

Kropka dodana na końcu nazwy domeny nie jest przypadkowa. Wręcz przeciwnie! Jest niezwykle istotna, a w 99% przypadków jest pomijana przez administratorów. Wpływa ona znacząco na wiele procesów wymagających sprawdzania i wyszukiwania różnych wartości w domenie min. na szybkość procesu logowania użytkowników.

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Instalacja_Kontrolera_Domeny\DC2.PNG }}CTF\Administracja\Windows_Server\Instalacja_Kontrolera_Domeny\DC2.PNG)

Na następnej stronie ponownie widzimy kilka istotnych ustawień. 

Na początek widzimy ustawienia dotyczące poziomów funkcjonalności lasu i domeny. Poziomy funkcjonalności określają dostępne możliwości domeny czy lasu w ADDS. Poziom funkcjonalności lasu i domeny zawsze powinien być ustawiony na najwyższy możliwy, a za określenie jaki poziom jest aktualnie najwyższy możliwy odpowiada kilka warunków:
- po pierwsze poziom funkcjonalności musi być dostosowany do najniższej wersji systemu na dowolnym kontrolerze domeny - np. gdy mamy poziom funkcjonalności domeny ustawiony w tym przypadku na "Windows Server 2016" to żaden z kontrolerów domeny nie może mieć wersji systemu starszej niż Windows Serwer 2016 czyli np. 2012. Może natomiast mieć nowszą np. 2019. Patrząc w drugą stronę jeżeli mamy kilka kontrolerów domeny z systemem Windows Serwer 2016 i jeden kontroler z Windows Serwer 2012 poziom funkcjonalności domeny musi być ustawiony na Windows Serwer 2012, aby wszystkie funkcje działały poprawnie.
- po drugie (co znacząco wiąże się z pierwszym) poziom funkcjonalności domeny lub lasu musi być najwyższy możliwy w tym sensie, że jeżeli mamy wszystkie kontrolery domeny z systemem Windows Serwer 2019 to nie ustawiamy poziomu funkcjonalności na Windows Serwer 2016 ("dla bezpieczeństwa, na wszelki wypadek, a jakby pojawił się w przyszłości starszy" itp.). Poziom funkcjonalności w tym przypadku musi mieć wartość Windows Serwer 2019
- po trzecie poziom funkcjonalności domeny może być wyższy niż lasu, ale nie może być niższy. Las jest zbiorem domen a jego poziom funkcjonalności określa minimalny poziom funkcjonalności każdej domeny. Jeżeli las obsługuje wszystkie dostępne możliwości Windows Serwer 2016 to domena z Windows Serwer 2019 również je obsłuży, natomiast domeny z poziomem funkcjonalności Windows Serwer 2012 może nie obsłużyć części z funkcjonalności wprowadzonych w 2016. Z tego powodu poziom lasu musi wyznaczą pewne standardy dla wszystkich domen na tej samej zasadzie jak domena wyznacza je dla każdego jej kontrolera.

Niżej widzimy opcje ustawienia pewnych możliwości kontrolera domeny:
- DNS server - po prostu kontroler domeny będzie jednocześnie serwerem DNS. Opcję tę powinno się zostawić włączoną - w maym środowisku przynajmniej jeden kontroler domeny powinien być serwerem DNS. Serwer DNS pozwala na tłumaczenie adresów IP na nazwy i odwrotnie. Przez DNS tłumaczone są np. wpisywane w przeglądarkę nazwy stron internetowych na adresy IP, żeby przeglądarka wiedziała gdzie się łączyć, aby "wejść" na daną stronę internetową. Na takiej samej zasadzie możemy skonfigurować DNS w naszym środowisku np. przypisując pewne adresy IP do konkretnych nazw, pod którymi będą rozpoznawane przez inne urządzenia.
- Global Catalog - jest to usługa, która przechowuje repliki wszystkich obiektów z niewielką liczbą najczęściej wykorzystywanych atrybutów np. podczas wyszukiwania jak np. login użytkownika, jego imię, nazwisko. Pozwala on na szybkie zlokalizowanie (wyszukanie) obiektóww drzewie domen Active Directory bez wiedzy o tym, w której domenie się znajdują, bez konieczności ciągłego rozszerzania przestrzeni nazw itp. 
- RODC (Read Only Domain Controler) - czyli jak sama nazwa wskazuje "tylko odczyt". Opcja ta stosowana jest np. na kontrolerze domeny, którą chcemy przyłączyć do naszej, a mamy obawy co do bezpieczeństwa takiej domeny lub umiejętności lokalnego IT. Wtedy z kontrolera domeny w tym trybie nie ma dostępu do głównego kontrolera.

Ostatnim elementem jest hasło DSRM (Directory Services Restore Mode) - czyli hasło do trybu bezpiecznego bootowania systemu Windows Serwer. W przypadku awarii kontrolera domeny DSRM pozwala administratorowi na naprawę, przywrócenie lub odzyskanie bazy danych Active Directory.

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Instalacja_Kontrolera_Domeny\DC3.PNG }}CTF\Administracja\Windows_Server\Instalacja_Kontrolera_Domeny\DC3.PNG)

Następna strona suży do ustawiania opcji delegacji DNS. Opcja ta w naszym przypadku nie jest szczególnie istotna. 

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Instalacja_Kontrolera_Domeny\DC4.PNG }}CTF\Administracja\Windows_Server\Instalacja_Kontrolera_Domeny\DC4.PNG)

Następnie musimy okreslić nazwę NetBIOS-ową. NetBIOS (Network Basic Input/Output System) jest ogólnie rzecz ujmując protokołem sieciowym, który zapewnia aplikacją komunikację pomiędzy komputerami znajdującymi się w tej samej sieci lokalnej. Działa on na bardzo podobnej zasadzie jak DNS, tłumaczy adresy komputerów na ich nazwy i odwrotnie. Różnica między NetBIOS-em a DNS-em polega na tym, że NetBIOS został zaprojektowany do identyfikowania komputerów w sieci lokalnej, a DNS do tłumaczenia nazw w internecie. NetBIOS jest także podstawą działania sieci dla systemów Windows. 

Nazwa NetBIOS-owa domeny pozwoli na identyfikowanie jej po tej nazwie zamiast po pełnym adresie. Najczęściej można ją zostawić jako domyślną, zaproponowaną przez kreator.

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Instalacja_Kontrolera_Domeny\DC5.PNG }}CTF\Administracja\Windows_Server\Instalacja_Kontrolera_Domeny\DC5.PNG)

Na kolejnej stronie wskazujemy miejsce zapisywania rzeczy typu logi itp. Jeżeli nie mamy jasno określonej koncepcji, gdzie chcemy trzymać tego typu rzeczy można pozostawić domyślne ścieżki

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Instalacja_Kontrolera_Domeny\DC6.PNG }}CTF\Administracja\Windows_Server\Instalacja_Kontrolera_Domeny\DC6.PNG)

Następnie przechodzimy do posumowania:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Instalacja_Kontrolera_Domeny\DC7.PNG }}CTF\Administracja\Windows_Server\Instalacja_Kontrolera_Domeny\DC7.PNG)

W kolejnym oknie kreator sprawdzi, czy spełnione zostały wszystkie wymagania, czy konfiguracja jest poprawna itd. Podczas sprawdzenia możemy zauważyć ostrzeżenia np. odnośnie braku możliwości utworzenia delegacji DNS. Przede wszystkim należy zwrócić uwagę na błędy (czerwony trójkąt), które koniecznie trzeba przeczytać, naprawić ponieważ nie pozwolą one na rozpoczęcie instalacji. Jeżeli chodzi o ostrzeżenia (żółte trójkąty) należy je oczywiście przeczytać i rozważyć, natomiast jeżeli ostrzeżeniem jest np. wspomniany już brak możliwości utworzenia delegacji DNS, gdzie podczas konfiguracji instalacji świadomie nie włączaliśmy tej opcji, to nie należy się takimi ostrzeżeniami zbytnio przejmować.

Jeżeli wszystko zostało skonfigurowane poprawnie na koniec powinniśmy zobaczyć zielone kółko z informacją, że wszystkie sprawdzenia zakończyły się powodzeniem i można przejść do instalacji.

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Instalacja_Kontrolera_Domeny\DC8.PNG }}CTF\Administracja\Windows_Server\Instalacja_Kontrolera_Domeny\DC8.PNG)

Po zakończeniu instalacji maszyna automatycznie się zrestartuje. Po restarcie kontroler domeny jest zainstalowany i działa poprawnie.



