<div align="center">
    <h2>Uniwersytet Rzeszowski</h2>
    <h3>Wydział Nauk Ścisłych i Technicznych</h3>
    <br>
    <img src="https://www.ur.edu.pl/storage/file/core_files/2021/4/23/91fb7de042976b9dfa3fc70cdd6abf63/Logo%20UR%20wersja%20podstawowa.png" alt="Logo UR" width="250">
    <br><br>
    <h1>System zarządzania zadaniami HIKIXMORI Kanban</h1>
    <h3>Moduł Administratora i Integracje z zewnętrzną chmurą GitHub API</h3>
</div>

<br>

**Autor:** Bartosz Bieszczak  
**Numer indeksu:** .......................  
**Przedmiot:** Aplikacje Internetowe  
**Kierunek, rok:** Informatyka i Ekonometria, II rok  
**Grupa laboratoryjna:** .......................  
**Data:** 11.06.2026  
**Rok akademicki:** 2025/2026  

---

# Podział pracy w zespole (Praca Indywidualna)
Aby sprawiedliwie podzielić projekt między 3 programistów, odrzuciliśmy płaski podział na front/back-end. W zamian zastosowaliśmy profesjonalny podział według funkcjonalności (Feature-based). Zapewnia to wykorzystanie pełnego stacku technologicznego (PostgreSQL, PHP Laravel, Tailwind CSS) przez każdego z nas.
Moja odpowiedzialność to **Osoba 3: Moduł Administratora i Integracje**. Stworzyłem ukrytą sekcję zarządzania systemem od "góry": potężny panel statystyk, interfejs do awansowania użytkowników, logikę filtrowania tabel danych oraz innowacyjny silnik API pozwalający przesyłać ułamkiem sekundy prace z firmy prosto do zewnętrznego portalu GitHub. (Części dotyczące Lidera oraz zwykłego pracownika udokumentowali w swoich aktach koledzy Justyn i Oliwer).

# Temat projektu
Oprogramowanie HIKIXMORI to interaktywny kombajn zarządzający czasem grafików, pisarzy i koderów tworzących wspólny projekt (np. grę). Aplikacja zapobiega utracie załączników oraz eliminuje techniczny ból głowy pracowników związany z trudnym komenderowaniem plików po rozmaitych chmurach zewnętrznych, oddelegowując wysyłki do jednego magicznego przycisku podłączonego przez API.

# Instrukcja uruchomienia projektu

### Dla programisty (Developer)
Zestaw instrukcji do "podniesienia" serwera roboczego w domu:
1. Zainstaluj oprogramowanie wymagane: Node v22+, PHP w wersji nowszej niż 8.2 (konieczne włączenie modułu OpenSSL do komunikacji z serwerem API) oraz bazę danych PostgreSQL v17.
2. Pobierz z sieci projekt z użyciem wiersza komend: `git clone <adres>`. Przejdź poleceniem `cd kanban-app`.
3. Narzędziem Composer ściągnij tyły aplikacji wpisując `composer install`.
4. Środowiskiem NPM skompiluj barwy okien: `npm install && npm run build`.
5. Przekopiuj pusty plik certyfikatów `cp .env.example .env` i wygeneruj własne bezpieczeństwo aplikacyjne wpisując `php artisan key:generate`.
6. W owym ukrytym pliku `.env` wypełnij swoje namiary na domową bazę postgres wpisując pożądane hasła do połączenia. Pamiętaj by uprzednio stworzyć nową bazę `kanban_hikixmori`.
7. Otwórz komunikację publiczną załączników komendą: `php artisan storage:link`.
8. Zbuduj wielką bazę z przykładowymi osobami i Administratorami do weryfikacji funkcjonalności, uruchamiając komendę startową "Seeding": `php artisan migrate --seed`.
9. Odpal silnik serwera: `php artisan serve`. (Wejdź przeglądarką pod testowy `http://localhost:8000`).

### Dla użytkownika (User)
Rozwiązanie funkcjonuje w najpopularniejszym standardzie chmury (Web App). Pracownik lub administrator operujący firmą z kawiarenki czy na delegacji, jest zobligowany do wpisania jedynie w pełni funkcjonalnego linku wygenerowanego przy wdrożeniu produkcji (np. `https://kanban.nasza-firma.pl`). Oprogramowanie z powodu działania w pamięci serwerowej nie zajmuje dysków C: użytkownika, wymaga tylko trwałego internetu do przesyłu grafiki pod rygor najnowszych przeglądarek internetowych.

# Podręcznik użytkownika
Perspektywa najwyższej instancji, Administratora (Szefa):
1. **Dostęp globalny:** Widzisz nie tylko tablicę Kanban zespołu, ale specjalnie dedykowany i ukryty przed pracownikami, pożądany panel Dashboardu ze słupkami ilości zrealizowanych poleceń i odrzutów z wszystkich obszarów działalności.
2. **Sekcja "Zarządzaj":** Na lewej belce nawigacyjnej zyskujesz magiczny przybornik. W zakładkach tych manipulujesz posadami, wyrzucając leniwych uczestników lub budując im całkiem nowe projekty od podstaw wraz z precyzyjnym podpinaniem firmy w chmurze GitHub przy zastosowaniu swojego Tokena Personalnego. 

# Udokumentowany CRUD (Zadania i Projekty)
Jako zarządca struktury bazy, odpowiadałem za zaprojektowanie dogłębnych, zaawansowanych systemów CRUD dla **Projektów**, **Użytkowników** oraz potężnych encji zależnych typu **Zadania (Tasks)** (wymagających rygorystycznych relacji od ról pracowników i projektów biznesowych).

### CREATE
*Formularz z pełną walidacją pól po stronie serwera i klienta.*
Konstruując mechanizm kreacji nowego Zadania odrzuciłem archaiczne dodawanie tekstu z pamięci. 
- Formularze operują na rozbudowanych listach wyboru (select) w oparciu o prawdziwe relacje strukturalne encji (Szef wskazuje palcem nazwisko z systemu). 
- Moduł chroni się twardo wytyczoną zaporą logiki zapobiegającą błędom ludzkim (Validation Rules). Wymuszane jest logiczne zachowanie – szef nie jest np. w stanie nadać pracownikowi terminu zadania widniejącego wstecz w stosunku do wczorajszej daty, bez podania racjonalnego opisu na conajmniej kilkanaście znaków. Serwer błędu wyświeci na ekran precyzyjne, czerwone podpowiedzi o pominiętej wadliwej wartości chroniąc integralność bazy.

### READ
*Lista zasobów z możliwością filtrowania i sortowania.*
Prosty "Select * z Bazy" byłby nieużyteczny przy setkach projektów w firmie. Stworzyłem tabelę z potężnym podwoziem analitycznym. 
Administrator zyskuje specjalne okienka u samej góry list: wyszukiwarkę słowną, rozwijane kategorie priorytetów i sortowanie statystyczne po wczesnej dacie. Algorytmy SQL w sekundę odfiltrowują gąszcz starych zrobionych zadań i wyrzucają zaledwie 3 rekordy, precyzyjnie pasujące do wytycznych "Bieżące prorytety grafika z napisem LOGO". Ten potężny szkielet w połączeniu z limitowaną Paginacją do kilkunastu wierszy redukuje zamrożenia interfejsu (UX) do zupełnego zera.

### UPDATE
*Formularz z pełną walidacją pól po stronie serwera i klienta.*
Mechanizm w trybie edycji, poza oczywistą zmianą nazwy z pełną ochroną logiki, zabezpiecza aplikację na poziomie inteligentnego oczyszczania (Sanityzacji). 
Jeśli w procesie aktualizacji ustawień firmy na serwerze API (GitHuba), użytkownik pośpieszy się i wrzuci wadliwy długi adres hiperłącza zaczynający się brudnym śmieciem protokołu (np. `https://github.com/firma/projekt.git`), funkcja ukrytego skryptu użyje wyrafinowanych cięć znakowych (Regex) w ułamku sekundy korygując ignorancję admina na w pełni sterylny identyfikator: `firma/projekt`. Chroni to proces zapisu by błędna wartość nie powielała trwałego zacięcia błędu 404 w przyszłych odwołaniach rynkowych.

### DELETE
*Formularz z pełną walidacją pól po stronie serwera i klienta.*
Nierozważne stąpnięcie na klawisz nie wykasuje w tej sekcji milionów cennego budżetu firmy. 
Logika jest podpięta pod walidację intencji ludzkich i zaporę logiczną bazy – uniemożliwiając Adminowi "zamordowanie" istnienia wielkiego projektu jeśli dookoła egzystuje tysiące niezakonczonych z pracownikami na umowach ról i komentarzy. Formularze potwierdzające chronią system blokując usterki korporacyjne.

# Udokumentowanie dostępu do zasobu przez użytkownika niezalogowanego
Bezkompromisowość biznesowa nakazuje chronić wszystkie dane i relacje współpracownicze. Dlatego użytkownik z poza firmy (Użytkownik Niezalogowany lub niezaproszony ze świata wirtualnego Internetu) nie zrealizuje na stronie ani odczytów, ani operacji edycji czy usuwania, nie mając prawa nawet wejść na wizualny system. Architektura kontrolerów odbija takie ataki nakazem blokowania tras dostępu (middleware) i cofa intruza rzucając go z powrotem do zera (strony wejściowej wymuszającej legalne poświadczenia).

# Udokumentowanie ról użytkowników
W celu zachowania stabilności łańcucha dowodzenia wyróżniono następujące pozycje od najwyższej:
- **Administrator Firmy:** Zarządca. Tworzy projekty korporacyjne na szerszym schemacie i ma absolutne, wszechmogące moce rzutowania ról czy całkowitego dezaktywowania ludzi stwarzających problemy prawne w przedsiębiorstwie, mając do pomocy tylko wykresy statystyk dla zarządu.
- **Lider Zespołu:** Menadżer oddziału. Rola nadrzędna wobec zwykłych wykonawców o silnym nakazie podejmowania decyzji o losie wgranych przez rzemieślników plików. 
- **Zwykły Użytkownik:** Wyrobnik systemu z zadaniem informowania o procesach komentarzami. Z najniższą odpowiedzialnością za pliki. Wymagający stałego zalogowania w celu wylistowania pracy przydzielonej konkretnie do jego konta, w ramach ścisłych reguł nie wkraczania kompetencyjnego w inne gałęzie profesji kolegów pracowniczych z boku biurka.
- **Gość:** Blokada wejściowa. 

# Udokumentowanie uprawnień użytkowników
Przejrzystość uprawnień jest strzeżona na twardym jądrze Polices uformowanych w architekturę Laravel. 
Pracownik czy Lider wykonują ograniczone manewry podglądu z uwagi na nieprzydzielone im obszary na poziomie bazy danych. Władzę dyktatora ma natomiast uwzględniony administrator – nie ocenia, co namalował grafik (nie wgrywa też plików samodzielnie do recenzji nie pełniąc tej roli wykonawczej), ale za to odblokowany i uprawniony jest całkowicie do zniszczenia każdego przypisania tego Lidera na inny projekt (co wykracza poza uwarunkowania kompetencji niższych ludzi w drabinie).

# Udokumentowanie zarządzania zasobami przez użytkowników
Aby platforma była skuteczna, Zwykli Użytkownicy (w swoich modułach opisanych oddzielnie przez ich realizatorów np. p.Oliwer i p.Justyn) mogą swobodnie władać podesłanymi swoimi zasobami np. skasować swój autorski komentarz napisany z omyłką bądź wyrzucić omyłkowo wgraną grafikę zanim nastąpi kłódka akceptacyjna. Chroni to zasoby serwera przed kumulowaniem cyfrowych, źle wrzuconych pomyłek na dnie chmury serwera.

# Udokumentowanie zarządzania profilami użytkowników przez administratora
Administrator jest centrum dowodzenia personalnego. Posiada całkowicie osobny panel "Użytkownicy", skąd nadzoruje strukturę firmy. Nie tylko tworzy nowego członka zatrudniając go przez formularz z bezpiecznym kodowaniem haseł przez Hashowanie i automatycznym przesyłaniem go na jego poświadczony firmowy email, ale z jednego wielkiego poziomu panelowego może przydzielić mu nową gałąź zawodu lub co najważniejsze – zablokować na amen wycofując mu flagę "is_active" odbierając natychmiastowe fizyczne prawa przepustowości do zasobu tablicy do momentu wyjasnienia problemu służbowego. 

# Udokumentowanie dodatkowej, nietrywialnej logiki biznesowej
Do najpotężniejszych zjawisk systemu należy bezapelacyjnie Integracja z gigantyczną chmurą zewnętrzną (GitHub API). Wyrzuca to trywialne pojęcia ręcznego przerzucania pikseli do historii. Admin uzbrajając zaledwie jeden ukryty formularz uwierzytelniania na platformie pozycją z Personalnym Tokenem API buduje niewidzialny most między aplikacją a kontynentem amerykańskim. Moduł PushToGithub sprytnie kodując dane plikowe z serwera na czystą linię znaków Base64 w tle w ułamek momentu, nie zakłócając pracy firmy przesuwa gigabajty załaczników prosto na zewnętrze serwery z niesamowitym uwzględnieniem dynamicznego przechwytywania niesłusznych Certyfikatów Zabezpieczeń lokalnych maszyn (`withoutVerifying()`), redukując straty prądowe systemu na serwerach głównej firmy o setki procent poprzez optymalizacyjne przesterowania na pobrania z wbudowanego linku `302 ZipBall URL` zamiast ładowania RAM-u.

# Udokumentowanie funkcjonalności dedykowanych dla użytkowników końcowych wykraczających poza proste CRUD
Platforma poza zabetonowanym systemem wysyłki ułatwia odnajdowanie się w firmie po wdrożeniu profesjonalnych wizualnych statystyk na widokach z Dashboardu (Użytkownicy końcowi na roli Szefa bez żargonu programistycznego widzą czytelny, kolorowy, potężny diagram wyliczony i wyrysowany graficznie w locie pokazujący procent wgranych poprawek czy zatwierdzeń w sektorze grafików). Dodatkowo zablokowanie responsywne zjawisk bocznego znikającego panelu pomaga zarządcy czytać powiadomienia, przefiltrowywać tabelki, zwalniać, edytować tablice ról w dowolnej scenerii życia biznesowego podczas picia rano domowej kawy przy smartfonie.
