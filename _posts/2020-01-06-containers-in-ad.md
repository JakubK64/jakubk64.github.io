---
layout: post
title: Jednostki organizacyjne w AD
published: true
tags: organizational unit AD group container
---

### Krótkie wprowadzenie o strukturze AD

AD ma strukturę hierarchiczną. Jest to pewnego rodzaju hierarchiczna baza danych zawierająca informacje o obiektach (konta użytkowników, komputery, grupy użytkowników czy komputerów itp.) zawartych wewnątrz domeny/lasu. 

Głównymi elementami pozwalającymi na pogrupowanie obiektów w AD są kontenery i jednostki organizacyjne. Obydwa są pewnego rodzaju "folderami" w AD, ich struktura również wygląda bardzo podobnie (mamy przecież foldery, podfoldery itd. które tworzą drzewo - tak samo jest z tymi elementami w AD). 

Różnica pomiędzy tymi dwoma elementami polega na tym, że kontenery są domyślnymi "folderami", które zawierają np. wszystkich użytkowników czy wszystkie komputery w domenie/lesie. Bardzo ważną różnicą jest to, że na kontenery nie możemy stosować GPO (Group Policy Objects) czyli zasad stosowanych dla wszystkich obiektów w danej grupie.

Jednostki organizacyjne są ręcznie tworzone przez Administratora w celu pogrupowania i tym samym łatwiejszego zarządzania obiektami w AD (np. zastosowanie tych samych zasad dotyczących bezpieczeństwa dla wszystkich użytkowników z danego działu w firmie, którzy będą znajdowali się w tej samej jednostce organizacyjnej).

### Tworzenie jednostek organizacyjnych

Tworzeniem czy zarządzaniem jednostkami organizacyjnymi możemy się zająć np. przez ADUC - Active Directory Users and Computers.

Na początku w aplikacji Server Manager wchodzimy w "Tools" -> "ADUC"

Po otwarciu klikamy na wybraną przez siebie domenę. Widzimy teraz aktualną strukturę (możemy ją rozwijać po lewej) oraz obiekty znajdujące się w danej lokalizacji w AD. Mamy również opisy każdego elementu (w kolumnie "Type"). Jak widzimy mamy domyślne kontenery zawierające min. użytkowników czy komputery oraz jedną domyślną jednostkę organizacyjną zawierającą kontrolery wybranej domeny.

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Jednostki_Organizacyjne/OU1.PNG }}CTF/Administracja/Windows_Server/Jednostki_Organizacyjne/OU1.PNG)

Następnie przechodzimy do utworzenia nowej jednostki organizacyjnej. Klikamy PPM na wolną przestrzeń, w "New" -> "Organizational Unit"

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Jednostki_Organizacyjne/OU2.PNG }}CTF/Administracja/Windows_Server/Jednostki_Organizacyjne/OU2.PNG)

Wpisujemy nazwę nowej jednostki organizacyjnej. Warto pozostawić zaznaczoną domyślnie opcję ochrony przed przypadkowym usunięciem.

Zacznijmy od jednostki organizacyjnej skupiającej wszystkie elementy znajdujące się w Warszawskim oddziale jakiejś firmy:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Jednostki_Organizacyjne/OU3.PNG }}CTF/Administracja/Windows_Server/Jednostki_Organizacyjne/OU3.PNG)

Następnie na tej samej zasadzie zbudujmy głębiej drzewa naszej domeny ("podfoldery"). Na tej samej zasadzie jak wyżej zróbmy jednostki organizacyjne dla działów IT i HR znajdujących się w Warszawskim oddziale firmy:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Jednostki_Organizacyjne/OU4.PNG }}CTF/Administracja/Windows_Server/Jednostki_Organizacyjne/OU4.PNG)

Na tej zasadzie możemy tworzyć mniej lub bardziej skomplikowane drzewa np. domen, pozwalające pogrupować obiekty AD. Utrzymanie "porządku" obiektów naszej domeny pozwoli min. na łatwiejsze zarządzenie nimi.

### Przenoszenie obiektów AD do jednostek organizacyjnych

Aby przenieść obiekt w AD do wybranej jednostki organizacyjnej jednym z najprostszych sposobów jest kliknięcie PPM na obiekcie, w "Move..." i wskazanie w które miejsce chcemy przenieść dany obiekt:

Powiedzmy, że chcemy umieścić jednego z użytkowników i jeden z komputerów w dziale IT Warszawskiego oddziału firmy:

Znajdujemy konto użytkownika, którego chcemy przenieść:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Jednostki_Organizacyjne/User1.PNG }}CTF/Administracja/Windows_Server/Jednostki_Organizacyjne/User1.PNG)

Nastepnie wskazujemy gdzie chcemy umieścić wybrany obiekt:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Jednostki_Organizacyjne/User2.PNG }}CTF/Administracja/Windows_Server/Jednostki_Organizacyjne/User2.PNG)

Nastepnie postępujemy tak samo dla wybranego komputera. Jak widzimy elementy znalazły się w wybranej przez nas jednostce organizacyjnej:

![useful image]({{ https://github.com/JakubK64/jakubk64.github.io/blob/master/CTF/Administracja/Windows_Server/Jednostki_Organizacyjne/User3.PNG }}CTF/Administracja/Windows_Server/Jednostki_Organizacyjne/User3.PNG)

Na tej zasadzie możemy pogrupować obiekty znajdujące się w AD. Znacząco ułątwi to zarządzanie nimi a przede wszystkim stosowanie odpowiednich polityk (GPO) dla grup użytkowników (co zaoszczędzi sporo czasu względem ustawiania ich każdemu indywidualnie)

