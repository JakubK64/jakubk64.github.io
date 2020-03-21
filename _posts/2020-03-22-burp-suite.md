Burp Suite jest narzędziem typu proxy HTTP. Pozwala on przechwycić żądania HTTP (generowane np. przez przeglądarkę internetową). Przechwycenie takich zapytań jest równoznaczne z możliwością ich modyfikacji


### Przechwytywanie ruchu przez Burp jako proxy

W ustawieniach przeglądarki ustawiamy serwer proxy HTTP na adres localhosta (127.0.0.1) oraz port 8080:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Proxy.png }}\CTF\Notatki\Burp_Suite\Proxy.png)

Dzięki temu Burp będzie przechwytywał żądania HTTP wysyłane przez przeglądarkę i dopiero po zatwierdzeniu będą one trafiały do serwera docelowego. Tj. gdy wpiszemy dowolną stronę w przeglądarkę, będzie ona czekała na odpowiedź serwera, czyli de facto na zatwierdzenie przez nas żądania tak, aby serwer docelowy je otrzymał i mógł na nie odpowiedzieć:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Web1.png }}\CTF\Notatki\Burp_Suite\Web1.png)

Po "wysłaniu" żądania Burp przechwytuje je i wyświetla opcje "zarządzania" tym zapytaniem - z tego miejsca możemy je chociażby modyfikować. Po użyciu przycisku "Forward" zapytanie zostanie przesłane do serwera:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/BurpRequest1.png }}\CTF\Notatki\Burp_Suite\BurpRequest1.png)

Po zatwierdzeniu wysłania żądania do serwera otrzymujemy odpowiedź i nasza przeglądarka wyświetla docelową stronę:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Web2.png }}\CTF\Notatki\Burp_Suite\Web2.png)

### Przelgądanie wysłanych zapytań HTTP

W zakłądce "HTTP History" widzimy historię zapytań HTTP wysłanych przez naszą przeglądarkę. Część z nich to faktyczne, wygenerowane przez nas zapytania, część automatyczne zapytania generowane przez przeglądarkę w celu np. sprawdzenia aktualizacji. Skoncentrujmy się np. na wysłanym przez nas przed chwilą zapytaniu:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/BurpHistory1.png }}\CTF\Notatki\Burp_Suite\BurpHistory1.png)

Poniżej widzimy szczegółowe informacje dotyczące wybranego wyżej zapytania:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/BurpHistory2.png }}\CTF\Notatki\Burp_Suite\BurpHistory2.png)

Widzimy dwie główne zakładki - Request i Response, w których możemy podejrzeć szczegółowe informacje na temat odpowiednio: wysłanego przez nas zapytania oraz odpowiedzi otrzymanej od serwera docelowego.

Poniżej mamy podzakładki, które prezentują zapytania/odpowiedzi HTTP w różnej formie. W zakładce "Request" możemy zobaczyć takie podzakładki jak:
- Raw - widok na zapytanie/odpowiedź w surowej, nieprzetworzonej formie
- Params - zwraca jedynie listę parametrów takich jak parametry przekazywane w adresie URL, w ciele zapytań typu POST czy wartości Cookies
- Headers - prezentuje jedynie nagłówki, które pojawiły się w zapytaniu HTTP
- Hex - pozwala prześledzić zapytanie w systemie szesnastkowym (hex)

W zakładce "Response" oprócz powyższych (jedynie bez podzakładki "Params") możemy zobaczyć takie podzakładki jak:
- HTML - pokazuje tylko ciało odpowiedzi (czyli w większości przypadków kod HTML zwrócony przez serwer) odpowiednio sformatowany, aby zwiększyć jego czytelność
- Render - pozwala na wygenerowanie strony na podstawie kodu HTML zwróconego przez serwer, tak jak zrobiłaby to zwykła przeglądarka (swoją drogą korzysta z silnika przeglądarki Chromium)

### Przykładowa modyfikacja żądania HTTP

Na aktualnej stronie treningowej mamy do czynienia z prostą apikacją. Pozwala na wpisanie adresu email, który zostanie "zarejestrowany" po wciśnięciu przyciski register oraz otrzymamy potwierdzenie, że rejestracja przebiegła poprawnie:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/App1.png }}\CTF\Notatki\Burp_Suite\App1.png)

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/App2.png }}\CTF\Notatki\Burp_Suite\App2.png)

W przypadku wpisania nieprawidłowego formatu adresu email strona nie pozwoli nam przesłać takiego żądania - jest zaimplementowany prosty mechanizm bezpieczeństwa sprawdzający format wpisywanych danych, więc nie możemy umieścić tutaj np. kodu, który miałby się wykonać zamiast wpisania adresu email. Nic jednak nie stoi na przeszkodzie, aby wpisać poprawny adres email i wygenerować zapytanie tak, aby obejść mechanizm zabezpieczający a następnie podmienić zawartość zapytania z Burpie (zakładając, że jest to jedyny mechanizm i walidacja następuje tylko po stronie klienta, nie ma żadnej walidacji po stornie serwera). Wysyłamy więc prawidłowe żądanie:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/App4.png }}\CTF\Notatki\Burp_Suite\App4.png)

Następnie przechodzimy do Burpa i przed wysłaniem żądania do serwera podmieniamy jego zawartość podmieniając wartość parametru "email=test%40test2.pl" (%40 to reprezentacja znaku @, który należy do znaków zarezerwowanych przez co przedstawiany jest w formie tzw. kodowania procentowego. 40 to hexadecymalna reprezentacja znaku @) w ciele na dowolny kod, np. taki do wygenerowania przykładowego alertu:

Prawidłowe zapytanie:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/BurpChange1.png }}\CTF\Notatki\Burp_Suite\BurpChange1.png)

Zapytanie po modyfikacji parametru email:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/BurpChange2.png }}\CTF\Notatki\Burp_Suite\BurpChange2.png)

Wynik zapytania:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/BurpChange3.png }}\CTF\Notatki\Burp_Suite\BurpChange3.png)


Informacje o zmodyfikowanym zapytaniu możemy teraz znaleźć w dodatkowej zakładce "Edited request":

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/BurpHistory3.png }}\CTF\Notatki\Burp_Suite\BurpHistory3.png)

O poprawności "wstrzyknięcia" kodu oprócz aspektów wizualnych jak w tym przypadku możemy się przekonać analizująć odpowiedź serwera a dokładnie kod HTML zwrócony przez serwer w ciele. Jak widzimy, możemy w nim znaleźć "wstrzyknięty" przez nas kod:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/BurpChange4.png }}\CTF\Notatki\Burp_Suite\BurpChange4.png)

### Przechwytywanie odpowiedzi od serwera

Domyślnie Burp przechwytuje tylko zapytania, które mają zostać przesłane do serwera. Aby przechwytywać odpowiedz od serwera (którą możemy np. zmodyfikować) trzeba włączyć taką opcję:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Resopnse1.png }}\CTF\Notatki\Burp_Suite\Response1.png)

Wykorzystajmy więc możliwość wstrzyknięcia kodu na tej stronie i zmodyfikujmy odpowiedź, tak aby otrzymać w alercie zamiast nazwy domeny - cookies. Wpisujemy więc w przeglądarkę dowolny adres email i modyfikujemy wartość parametru w Burpie tak jak poprzednio:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Resopnse2.png }}\CTF\Notatki\Burp_Suite\Response2.png)

Wysyłamy i czekamy na odpowiedź serwera. Tym razem oczywiście trafia ona najpierw do Burpa. Znajdujemy fragment odpowiedzi gdzie znajduje się wstrzyknięty przez nas kod i modyfikujemy zawartość - zamiast kodu "alert(document.domain)" wpisujemy alert(document.cookie)":

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Resopnse3.png }}\CTF\Notatki\Burp_Suite\Response3.png)

Następnie akceptujemy odpowiedź. Otrzymujemy alert z wyświetloną wartością cookie (w tym przypadku wartość PHPSESSID):

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Resopnse4.png }}\CTF\Notatki\Burp_Suite\Response4.png)

### Repeater

Przy konstruowaniu opisywanych wyżej zapytań cały czas wymagana jest interakcja z przeglądarką. Jeżeli chcielibyśmy wysyłać wiele zapytań korzystanie z Burpa w taki sposób mogłoby zająć bardzo dużo czasu. Aby usprawnić ten proces można wykorzystać narzędzie jakim jest repeater. W tym celu bierzemy zmodyfikowane przez nas zapytanie POST i klikamy "Send to repeater" (co przygotuje nam od razu środowisko testowe) po czym przechodzimy do zakładki "Repeater":

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Repeater1.png }}\CTF\Notatki\Burp_Suite\Repeater1.png)

Korzystając teraz z repeatera podmieńmy wartość parametru email np. na "Przykladowy Tekst" i wyślijmy zapytanie (przyciskiem "Send" lub skrótem klawiszowym "Crtl+Spacja"):

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Repeater2.png }}\CTF\Notatki\Burp_Suite\Repeater2.png)

Po prawej stronie wyświetlona zostaje odpowiedź od serwera:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Repeater3.png }}\CTF\Notatki\Burp_Suite\Repeater3.png)

### Intruder - automatyzacja zadań

Intruder jest narzędziem pozwalającym na automatyzację. Pozwala on np. wielokrotnie wysłać dane zapytanie HTTP do serwera, co ułatwia pracę i oszczędza znaczną ilość czasu. Wróćmy od testowej aplikacji. Po przesłaniu poprawnego adresu email aplikacja zwraca adres email oraz adres URL do profilu. Adres URL wygląda w zależności od przypadku mniej więcej tak:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Intruder1.png }}\CTF\Notatki\Burp_Suite\Intruder1.png)

Warto zwrócić uwagę na to, że w URL mamy widoczną wartość parametru User. Warto w tym przypadku sprawdzić czy przy okazji zmiany wartości tego parametru nie uda się w nieautoryzowany sposób zdobyć danych innego użytkownika. Do tego celu można wykorzystać Intrudera. Aby uzyskać odpowiednie dane/środowisko do pracy tego narzędzia wyszukujemy w zakładce "HTTP History" zapytanie GET i przesyłamy do Intrudera:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Intruder2.png }}\CTF\Notatki\Burp_Suite\Intruder2.png)

Następnie przechodzimy do zakładki "Intruder" -> "Positions". Ta zakładka służy do manipulacji zapytaniem. Narzędzie od razu wykrywa możliwe do podmiany parametry zapytania:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Intruder3.png }}\CTF\Notatki\Burp_Suite\Intruder3.png)

Interesuje nas oczywiści parametr "user", nie będziemy modyfikowali ciasteczka. Aby przygotować zapytanie tak jak chcemy klikamy przycick "Clear" po prawej stronie, który pozwoli nam usunąć wszystkie znaczniki wykrytych przez narzędzie parametrów. Następnie zaznaczamy myszką wartość parametru "user", który interesuje nas w tym przypadku, i klikamy przycisk "Add". Narzędzie automatycznie doda znak dolara przed i za wartością argumentu oznaczając w ten sposób parametr, który będzie ulegał zmianie. Odpowiednio przygotowane zapytanie wygląda następująco:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Intruder4.png }}\CTF\Notatki\Burp_Suite\Intruder4.png)

Gdy mamy już zdefiniowane parametry, które mają podlegać zmianie możemy przejść do zakładki "Payloads". Zakładka ta pozwala nam określić dowolne wartości, które narzędzie ma podstawiać jako wartość wybranego parametru. Ponieważ wybrany przez nas paramter przyjmuje tylko różne liczby całkowite najlepszym typem Payloadu będzie typ "Numbers". Wybieramy więc taki typ payloudu i poniżej określamy zakres generowanych liczb - "From" określa od jakiej wartości rozpoczynamy, "To" określa ostatnią wartość, a "Step" o jaką wartość będziemy inkrementować. W naszym przypadku wybieramy wartość "Type" jako "Sqquential" co spowoduje, że przy wybranych parametrach narzędzie będzie podstawiało po kolei liczcby od 1 do 500 co 1 liczbę:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Intruder5.png }}\CTF\Notatki\Burp_Suite\Intruder5.png)

Gdy mamy tak skonfigurowane narzędzie przystępujemy do ataku. Klikamy przycisk "Start attack" dostępny w prawym górnym rogu. Jak widać w darmowej wersji Burpa przeprowadzanie tego typu ataku może zająć sporo czasu. Mimo ograniczeń prędkości w damrowej wersji i tak wykorzystanie tego narzędzia względem ręcznego wpisywania wartości parametru user jest nieporównywalnie wygodniejsze. Zatrzymajmy się więc przy wartości równej 200 i przeanalizujmy wykryte do tej pory wyniki. Aby zatrzymać atak klikamy w zakładkę "Attack" -> "Pause".

Jak znaleźć to co nas interesuje w danych wygenerowanych przez narzędzie ? Jednym z najprostszych sposbów w tym przypadku będzie posegregowanie zapytań/odpowiedzi po ich długości. W tym celu klikamy na kolumnę "Length" i sortujemy wyniki. Widzimy, że kilka zapytań a konkretnie ich długość znacząco odstaje od pozostałych co oznacza, że znajduje się w nich więcej danych. Więcej danych niż w większości innych przypadków daje nam spore prawdopodobieństwo, że trafiliśmy na zapisane tam dane innego użytkownika:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Intruder6.png }}\CTF\Notatki\Burp_Suite\Intruder6.png)

Po sprawdzeniu okazuje się, że zapytania o wartości parametru użytkownika < 10 to nic innego jak utworzone przy okazji testowania poprzednich narzędzi/elementów Burpa konta:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Intruder7.png }}\CTF\Notatki\Burp_Suite\Intruder7.png)

Sprawdźmy co kryje się jednak pod wartościami "58" oraz "77". Jednym ze sposobó będzie wybranie odpowiedniego zapytania, następnie przejście do zakładki "Response" -> "Render" i wyrenderowanie strony z otrzymanego kodu HTML (oczywiście można to odczytać także filtrując kod HTML, ale w tym przypadku renderujemy stronę, aby było to czytelniejsze do pokazania):

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Intruder8.png }}\CTF\Notatki\Burp_Suite\Intruder8.png)

Widzimy, że udało się znaleźć sesję innego użytkownika. Możemy podejrzeć w przypadku tej aplikacji jego adres email:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Intruder9.png }}\CTF\Notatki\Burp_Suite\Intruder9.png)

Znając format zwracanej odpowiedzi z adresem email sprawdźmy co kryje się pod wartością 77 filtrując kod HTML. Na wyrenderowanej stronie adres email zawsze występuje po słowach "Welcome back (...)". Wyszukajmy więc ten tekst w zakładce "Response" -> "HTML". W ten sposób znajdujemy email kolejnego użytkownika:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Intruder10.png }}\CTF\Notatki\Burp_Suite\Intruder10.png)

### Wykorzystanie Intrudera do rekonesansu infrastruktury

Jak można wykorzystać Intrudera jeżeli w adresie nie ma żadnego parametru, którego wartość możemy podmieniać? Np. do wykonania rekonesansu. Wykorzystamy Intrudera jakos skaner zasobów (katalogów) znajdujących się na atakowanym serwerze. 

Na początek konfigurujemy zakładkę "Positions". Zaznaczamy fragment zapytania, który ma ulec zmianie. Będzie to oczywiście fragment zawierający ścieżkę z nagłówku zapytania HTTP. Niezwykle istotne, aby zostawić znak "/" niezaznaczony do zmiany -> w przypadku korzystania ze słownika danych (o czym niżej) wartości to po prostu popularne nazwy katalogów. Aby zapytanie było poprawne w nagłówku musi się znajdować ścieżka a nie nazwa. Pozostawienia znaku "/" niezmiennego pozwoli dla każdego zapytania generować właśnie taką ścieżkę:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/IntruderRep1.png }}\CTF\Notatki\Burp_Suite\IntruderRep1.png)

Jako Payload tym razem konfigurujemy typ "Simple List". Skąd wziąć dane do sprawdzenia ? Możemy wykorzystać różnego rodzaju słowniki najpopularniejszych domen - np. dirbuster udostępniony przez Kali.org. Ze względu na ograniczenia prędkości działania darmowej wersji podłączamy mały słownik:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/IntruderRep2.png }}\CTF\Notatki\Burp_Suite\IntruderRep2.png)

Przystępujemy do ataku i czekamy na wyniki. Mały słownik zawiera prawie 90tys pozycji, więc ponownie ze względu na ograniczenia prędkości darmowej wersji Burpa przerwiemy atak po około 200 próbach. Tym razem, ponieważ dostajemy różne rodzaje odpowiedzi o serwera wykorzystamy dostępne w narzędziu filrty. Zaznaczymy tylko sukcesy czyli kody odpowiedzi 2XX (czyli kody z przedziału 200-299, najczęściej oczywiście zwykły kod "OK 200"). W tym celu kilamy na pole "Filter" i zaznaczamy odpowiednie opcje:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/IntruderRep3.png }}\CTF\Notatki\Burp_Suite\IntruderRep3.png)

Otrzymujemy wyniki kilku znalezionych istniejących zasobów. Zasób o wartości "Request" 0 to podstawowa strona z aplikację (zaznaczyłem ją na czerwono - kolory wyników w celu uzyskania lepszej przejrzystości można zmieniać klikając na pole "Request" w wybranym wierszu):

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/IntruderRep4.png }}\CTF\Notatki\Burp_Suite\IntruderRep4.png)

Mając taką wiedzę możemy sprawdzić zawartość znalezionych zasobów korzystając np. z zakładki "Render" lub wpisując adres URL w przeglądarkę - być może uda się znaleźć coś ciekawego ?

### Comparer - porównywanie otrzymanych wyników

Kolejnym narzędziem wbudowanym w Burpa jest Comparer. Pozwala on zestawić ze sobą wyniki np. przeprowadzanych przed chwilą ataków i porównanie np. otrzymanych od serwera odpowiedzi. W tym celu wybierzmy 2 znalezione w poprzednim punkcie zasoby i prześlijmy odpowiedzi otrzymane od serwera do tego narzędzia:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Comparer1.png }}\CTF\Notatki\Burp_Suite\Comparer1.png)

W narzędziu wybieramy z listy 2 elementy do porównania - pierwszy w oknie "Select item 1" i drugi w oknie "Select item 2":

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Comparer2.png }}\CTF\Notatki\Burp_Suite\Comparer2.png)

A następnie uruchamiamy narzędzie porównania zawartości wybranych odpowiedzi od serwera. Możemy analizować dane w dwóch typach - zwykłym, z użyciem liter oraz po bajtach w zapiscie heksadecymalnym (warto pamiętać o opcji "Sync views" pozwalającej na zsynchronizowanie scrollowania w obu plikach):

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Comparer3.png }}\CTF\Notatki\Burp_Suite\Comparer3.png)

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Comparer4.png }}\CTF\Notatki\Burp_Suite\Comparer4.png)

### Decoder - konwertowanie danych pomiędzy różnymi formatami

Narzędzie Decoder służy do różnego rodzaju konwersji pomiędzy danymi. Możemy go używać w przypadku np. dekodowania zaszyfrowanego hasła do jakiejś usługi. Aby pokazać jego praktyczną stronę weźmy kolejną przykładową aplikację - panel logowania do jakiejś usługi. Spróbujmy się zalogować przykładowymi danymi:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Decoder1.png }}\CTF\Notatki\Burp_Suite\Decoder1.png)

Tak wygląda zapytanie do serwera zawierające wpisany przez nas login i hasło:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Decoder2.png }}\CTF\Notatki\Burp_Suite\Decoder2.png)

Załóżmy że to nie my wysłaliśmy żądanie a udało się nam przechwycić takie żądanie z zaszyfrowaną wartością parametru "passw" czyli hasła. Na pierwszy rzut oka widać algorytm użyty do zaszyfrowania tekstu - dwa znaki "=" na końcu sugerują Base64. Aby zdeszyfrować hasło użyjemy właśnie Decodera. Zaznaczamy hasło, klikamy PPM i wybieramy opcję "Send to Decoder". Spośród opcji dostępnych po prawej stronie wybieramy "Decode as ..." -> "Base64". W ten szybki i prosty sposób udało się nam zdeszyfrować hasło:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Decoder3.png }}\CTF\Notatki\Burp_Suite\Decoder3.png)

### Sequencer - narzędzie analizy 

Sequencer jest narzędziem pomagającym nam w analizie. Weźmy na warsztat przykładowe Cookie. W testowenej aplikacji wartość Cookie - PHPSESSID czyli identyfikator sesji użytkownika jest dość skomplikowany, składa się z 26 znaków, małych i dużych liter oraz cyfr, więc jest dość dobrze zabezpieczone na próby odgadnięcia go metodą brute-force:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Sequencer1.png }}\CTF\Notatki\Burp_Suite\Sequencer1.png)

W przypadku innej, słabo zabezpieczonej aplikacji te wartości mogą być inne, może to być mniej lub więcej znaków, mogą występować różne wielkości liter i cyfra a może tylko same cyfry. W określeniu wskaźnika losowości czy podatności takiego ciągu znaków na odgadnięcie może pomóc właśnie Squencer. Aby użyć tego narzędzia weźmy dowolne zapytanie, gdzie w zakładce "Response" czyli odpowiedzi serwera otrzymujemy nagłówek "Cookie", klikamy PPM, a następnie "Send to Sequencer":

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Sequencer2.png }}\CTF\Notatki\Burp_Suite\Sequencer2.png)

Narzędzie automatycznie wykryje wartość w nagłówku "Cookie", w zasadzie od zaraz możemy uruchomić narzędzie klikając "Start live capture":

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Sequencer3.png }}\CTF\Notatki\Burp_Suite\Sequencer3.png)

W tym momencie rozpoczyna się proces zbierania danych. Jak to w statystyce bywa, im więcej próbek tym dokładniejsze otrzymamy wyniki. W każdej chwili możemy zakończyć wysyłanie kolejnych zapytań i zbieranie danych za pomocą przycisku "Pause" (jeżeli oczywiście chcemy wznowić) lub "Stop". Możemy również podejrzeć zebrane dane używając opcji "Save tokens", która pozwala zrzucić wszystkie dane np. do pliku tekstowego. Jeżeli nie jesteśmy pewni ile danych jest nam potrzebnych do analizy możemy obserwować wyniki na bieżąco (a dokładniej w pewnych punktach) uruchamiając "Auto analyze". Poczekajmy aż zbierzemy powiedzmy 7.000 próbek i przerwijmy zbieranie danych:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Sequencer4.png }}\CTF\Notatki\Burp_Suite\Sequencer4.png)

Mając odpowiednią ilość danych możemy przejść do analizy klikając "Analyze now". A analizy możemy się dowiedzieć wiele ciekawych i użytecznych informacji na temat zebranych danych, jak dla przykładu entropia (czyli losowość), która dla generowanych identyfikatorów sesji wynosi 87 bitów (OWASP zaleca 64 bit, więc jest to bardzo dobry wynik - raczej nie ma co próbować losować identyfikatorów, "łamać" ich metodą siłową). Jeżeli nie jesteśmy pewni co do wiarygodności zebranych danych możemy skorzystać z podpowiedzi w sekcji "Reliabilty", która określiła wiarygodność wyników dla wygenerowanych 7tys próbek jako dobrą:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Sequencer5.png }}\CTF\Notatki\Burp_Suite\Sequencer5.png)
![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Sequencer6.png }}\CTF\Notatki\Burp_Suite\Sequencer6.png)

### HTTPS - jak testować aplikacje używające HTTPS

Obecnie większość aplikacji korzysta z protokołu HTTPS. Na domyślnych ustawieniach nie jesteśmy w stanie testować takich aplikacji. Aby to zmienić musimy dodać w naszej przeglądarce nowy organ cerfykikacji - na podstawie certyfikatu z naszego localhosta, ale po kolei. Wchodzimy w przeglądarce na adres 127.0.0.1:8080 czyli adres lokalny pod którym nasłuchuje proxy i klimay w przycisk "CA Certificate":

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/HTTPS.png }}\CTF\Notatki\Burp_Suite\HTTPS.png)

Zostanie pobrany plik "cert.der" który zawiera nasz certyfikat. Następnie w ustawieniach przeglądarki przechodzimy do opcji "Wyświetl Certyfikaty -> Organy Certyfikacji a następnie wybieramy opcję "Importuj" i importujemy pobrany przed chwilą plik z certyfikatem i zaznaczamy opcję "Zaufaj temu CA przy identyfikacji witryn internetowych":

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/HTTPS2.png }}\CTF\Notatki\Burp_Suite\HTTPS2.png)

Od tego momentu możemy testować również aplikacje wykorzystujące protokuł HTTPS

### Opcja "Match and Replace" - automatyzacja stałych zmian w zapytaniach

Opcja "Match and Replace" dostępna w zakładce "Proxy" -> "Options" pozwala na modyfikację określonych fragmentów zapytania na stałe. Można takie elementy w prawdzie za każdym razem modyfikować ręcznie wysyłając zapytanie (lub odbierając odpowiedź w przypadku modyfikacji odpowiedzi od serwera), jednak na dłuższą metę nie jest to wygodna metoda.  

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Match1.png }}\CTF\Notatki\Burp_Suite\Match1.png)

Jak pamiętamy nasza testowa aplikacja ma wprowadzony mechanizm walidacji po stronie użytkownika. Jest to domyślny mechanizm walidacji przeglądarki wynikający z określenia typu pola do którego wpisujemy dane jako "email" zamiast np. "text" w kodzie HTML strony:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Match2.png }}\CTF\Notatki\Burp_Suite\Match2.png)

Jeżeli chcemy przesyłać do tej aplikacji dane, które nie są zgodne z formatem adresu email możemy zastosować regułę, która za każdym razem będzie zmieniała wartość typu pola z "email" na "text". W kreatorze określamy typ czyli w jakiego typu zapytaniach automat ma poszukiwać szukanego elementu, następnie "Match" czyli jakiego ciągu znaków ma szukać oraz "Replace" jak ciąg znaków, który ma być podstawiony zamiast znalezionego ciągu znaków:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Match3.png }}\CTF\Notatki\Burp_Suite\Match3.png)

Po wprowadzeniu reguły oraz odświeżeniu strony spróbujmy wpisać dowolny tekst nieprzypominający adresu email - wcześniej było to oczywiście niemożliwe. Dzięki nowo dodanej regule nie widzimy żadnego komunikatu o błędzie, a korzystając z tego, że jak wspomnieliśmy nie ma walidacji wysyłanych danych po stronie serwera, tekst przechodzi jako prawidłowy - odpowiedź została odpowiednio zmodyfikowana przez regułę:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/MAtch4.png }}\CTF\Notatki\Burp_Suite\MAtch4.png)

Udało się więc zaimplementować regułę dla Burpa, która automatycznie usuwa walidację adresu email zaimplementowanego po stronie przeglądarki.

Spróbujmy także na innym przykładzie - ukryjmy wartość nagłówka "User-Agent" określającą informacje dla serwera o używanej przez nas przeglądarce, systemie operacyjnym itd. Zaimplementujmy mechanizm podmieniający treść zapytania tak, aby serwer otrzymywał informację, iż łączymy się używając przegląarki Internet Explorer. W tym celu kopiujemy naszą aktualną wartość nagłówka "User-Agent":

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Match5.png }}\CTF\Notatki\Burp_Suite\Match5.png)

A następnie przygotowujemy regułę zamieniającą ją na wartość User-Agent dla Internet Explorer:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Match6.png }}\CTF\Notatki\Burp_Suite\Match6.png)

Od teraz w zapytaniach wysyłanych w moim przypadku z Mozilli wartość User-Agent będzie wskazywała na to, że używana jest przeglądarka Internet Explorer

### Hostname Resolution - lokalne wpisy DNS bez uprawnień do modyfikowania wpisów prawdziwego DNS

W zakładce "Project options" -> "Connections" znajdziemy sekcję "Hostname Resolution". Opcja ta pozwoli tłumaczyć nazwy na adresy IP tak jak robi to usługa DNS. Tą opcję możemy wykorzystać, gdy nie mamy dostępu/uprawnień do edycji rekordów na prawdziwym serwerze DNS. Dodajmy tutaj przykładowy wpis tłumaczący adres tej strony na przykładowy adres IPv4:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/DNS1.png }}\CTF\Notatki\Burp_Suite\DNS1.png)

Od teraz gdy wpiszemy w przeglądarkę adres tej strony ruch zostanie przekierowany na wybrany adres IP:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/DNS2.png }}\CTF\Notatki\Burp_Suite\DNS2.png)

### Skróty klawiszowe - czyli jak przyśpieszyć sobie pracę

Krótko o skrótach klawiszowych: używając Burpa w zdecydowanej większości przypadków będziemy powtarzali wiele czynności dziesiątki czy setki razy. Aby przyśpieszyć ten proces i zwiększyć swoją efektywność warto stosować skróty klawiszowe. Burp ma oczywiście ustawione domyślne skróty klawiszowe, lecz my jako użytkownicy możemy je dowolnie modyfikować "pod siebie" a także dodawać nowe skóty klawiszowe do wybranych funkcji. Zrobić to możemy przechodząc do zakładki "User options" -> "Misc" i odnajdując sekcję "Hotkeys":

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Notatki/Burp_Suite/Hotkeys.png }}\CTF\Notatki\Burp_Suite\Hotkeys.png)
