# Event Storming dla Biznesu
* __Rozwijane przez__: [Michał Bartyzel](https://www.michalbartyzel.pl/michal-bartyzel)
* __Prawa autorskie i majątkowe__: [Michał Bartyzel](https://www.michalbartyzel.pl/michal-bartyzel)
* __Wersja__: 1.0
* Możesz używać i rozpowszechniać ten dokument w dowolny sposób o ile:
    - pozostawisz go w takiej formie, w jakiej go otrzymałeś - w całości
    - zawsze dodasz ww. informacje o autorstwie i prawach

---

# Wprowadzenie
Ten dokument jest częścią prowadzonego przeze mnie warsztatu [Event Storming dla Biznesu](https://www.michalbartyzel.pl/2019/02/event-storming-dla-biznesu-beanalyst.html)

Znajdziesz tu następujące informacje:

* Jak się przygotować do warsztatu?
* Jak krok po kroku przeprowadzić warsztat? W szczególności:
    - Jak określić zakres prac deweloperskich?
    - Jak uzyskać wstępne estymaty?
    - Jak zainicjować backlog?

Niniejsza instrukcja jest pisana z założeniem, że uczestniczyłeś w ww. warsztacie. W przeciwnym razie może być niezrozumiała.

Więcej na temat metody Event Storming dowiesz się:

* Z forka na moim profilu [GitHub](https://github.com/mbartyzel/awesome-eventstorming)
* Z ebooka [Autora](https://www.linkedin.com/in/brando/) metody: [Introducing to Event Storming](https://leanpub.com/introducing_eventstorming)

## Dla kogo jest Event Storming
Metoda Event Storming powstała w kontekście zespołów programistycznych. Jednak może być z powodzeniem wykorzystywana wszędzie, gdzie występuje relacja klient-wykonawca. Metoda pomaga obu stronom w skutecznym wykonaniu pracy.

## Kogo zaprosić?
Klucz wyboru uczestników jest następujący: __pełnia wiedzy i pełnia kompetencji__. Zaproś zatem:
* zespół wykonawczy
* eksperci biznesowi w zakresie tematyki wykonywanej pracy
* właściciele biznesowi

Wg mojej oceny warsztat na 15 osób to maksymalna liczba uczestników na jednego moderatora.

## Materiały

Materiały ogólne

* 2 [bloczki malusich karteczek](https://eofficemedia.pl/karteczki-stickn-38-51mm-zote-pastel-12-100-o_3471700.html)
* 3 [taśmy malarskiej](http://narzedzia-merkury.pl/tasma-maskujaca-3m-101e-maskowanie-powierzchni-przed-malowaniem-i-lakierowaniem.html)
* 2 [rolki papieru](https://ploter-media.pl/papier-do-plotera-100g-914mm-x-50m-p-363.html) lub paczka kartek flichart do przyklejenia na ścianę
* 1/2 ryzy papieru do drukarki
* ściana 6-10 m, do której można przytwierdzić papier
* brak stołów na sali

Materiały dla 1 uczestnika warsztatu

* 1 [Marker czarny](https://eofficemedia.pl/szukaj-o_s_0_1.html?keyword=sharpie&search=simple)
* 1 [bloczek pomarańczowy](https://eofficemedia.pl/karteczki-office-products-76-76mm-pomaraczowe-100-o_3068141.html)
* 1/4 [bloczek niebieski](https://eofficemedia.pl/karteczki-office-products-76-76mm-niebieske-100-o_3068140.html)
* 1/4 [bloczek fioletowy (kolor X00951)](https://eofficemedia.pl/karteczki-stickn-76-76mm-neonowe-kolory-100-o_4391816.html)
* 1/8 [bloczek żółty duży](https://eofficemedia.pl/bloczek-donau-eco-127-x-76-mm-zoty-eco-100-kartek-samoprzylepny-hc-o_3431017.html)
* 1/8 [bloczek różowawy](https://eofficemedia.pl/karteczki-stickn-76-127mm-rozowe-neon-100-o_4301023.html)
* 1/4 [bloczek żółty](https://eofficemedia.pl/bloczek-post-it-zoty-76-x-76-mm-100-kartek-samoprzylepny-o_3471532.html)
* 1/4 [bloczka karteczek w linię](https://eofficemedia.pl/notatki-karteczki-samoprzylepne-format-100-mm-i-wiksze-karteczki-donau-101-150mm-w-linie-zote-100-o_3091109.html)
* 1/4 [paczki kolorowych etykiet](https://eofficemedia.pl/zakadki-donau-pp-strzaki-12-x-45-mm-5-kolorow-po-25-kartek-o_3279855.html)


## Etap 1 - Generowanie zdarzeń i hot-spots

* Zdarzenie: kolor pomarańczowy
* Hot-spot: kolor różowy, karteczka przekręcona o 90 stopni

* __Zdarzenie__ to fakt w procesie biznesowym, który zaistniał
* W związku z powyższym zdarzenie:
    - Ma przyporządkowany konkretny moment w czasie, kiedy zaistniało
    - Ma formę czasu przeszłego dokonanego - "coś się stało", np.: "paczka wysłana", "klient poinformowany"
* Skrupulatnie pilnuj, aby zdarzenie miało ww. sformułowanie 

1. Nazwij proces biznesowy, który będziecie analizować
2. Opowiedz uczestnikom historię ilustrującą czym jest zdarzenie. Możesz użyć mojej historii o pieczeniu bułek w piekarniku :)
3. Uczestnicy wypisują zdarzenia w omawianym procesie
4. Uczestnicy przyklejają zdarzenia na ścianę
5. Jeśli uczestnicy mają jakieś pytania albo wątpliwości, poproś aby umieścili na ścianie hot-spot

Wyjaśnienie dlaczego zdarzenia są aż tak ważne, znajdziesz w moim artykule **[Dlaczego zdarzenia są ważne?](https://www.michalbartyzel.pl/2019/05/event-storming-dlaczego-zdarzenia-sa-wazne.html)** ;)

## Etap 2 - Pierwsze porządkowanie

* Element słownika: żółta kartka w linie

1. Uczestnicy uzgadniają wspólną wersję procesu
2. Również na tym etapie jest miejsce na dodawanie hot-spots
3. Zwracaj szczególną uwagę, aby uczestnicy byli zgodni co do nazw zdarzeń
4. Tu często zdarzają się rozmowy nt. pojęć używanych w procesie biznesowym. Dbaj, aby uzgodniono wspólne nazewnictwo. W osobnym miejscu na ścianie umieszczaj elementy słownika wraz z ich definicją

## Etap 3 - Analiza wstecz 

Uczestnicy analizują proces od końca. Dla każdego zdarzenia po kolei od końcowego począwszy, zadają sobie pytanie - "Jakie zdarzenie je poprzedza".

Cały czas doprecyzowujemy zdarzenia, ich kolejność, dodajemy hot-spots oraz elementy słownika.

## Etap 4 - Podział na podprocesy

1. W ułożonym procesie można najczęściej wyróżnić mniejsze zwarte części, czyli pod procesy. Pomóż uczestnikom je nazwać
2. Oddzielcie je wizualnie od siebie i oznaczcie ich nazwami wypisanymi na kawałkach rozklejonej na ścianie taśmy malarskiej
3. W tym momencie najczęściej doprecyzowaniu ulegają krańce podprocesów. Dochodzą nowe darzenia, istniejące zmieniają kolejność bądź przemieszczają się między podprocesami

## Etap 5 - Big Picture

To, co aktualnie znajduje się na ścianie to __Big Picture__. Możesz spodziewać następujących efektów:

* Uczestnicy w końcu dowiedzą się, jak pracują...
* Zostaną uzgodnione używane pojęcia biznesowe
* Transfer wiedzy jest głownie Biznes -> IT
* Zostaną zdefiniowane blokery w postaci hot-spots

To również dobry moment, aby hot-spots stały się action points. Najczęściej przyczyny hot-spots są trzy: 
    * brak odpowiednich informacji 
    * brak koniecznych decyzji
    * brak odpowiednich osób na warsztacie

### Co dalej?

Z tego miejsca można poprowadzić proces w co najmniej czterech różnych kierunkach:

* Analiza wartości
* Wstępne estymaty
* Zakładanie backloga
* Analiza rozwiązania (np. architektury) - tym nie będziemy się zajmować

## Etap 4a -  Analiza wartości

* Odpływ wartości - mała czerwona strzałka
* Przychód wartości - mała czerwona strzałka

Pytanie do uczestników: w których miejscach procesu przychodzi, a w których odpływa wartość? Gdzie szczególnie zyskujemy, a gdzie szczególnie tracimy pieniądze, użytkowników, zaangażowanie klientów, itd.?

Ten etap pomaga identyfikować trudności i potencjały w procesie.

## Etap 4b - Wstępne esytmaty

* Zadania zidentyfikowane przez wykonawców np. zespół deweloperski - żółta, kwadratowa kartka

Na tym etapie informacje przepływają głownie w kierunku IT -> Biznes

1. Wykonawcy wklejają pod odpowiednimi krokami procesu następujące informacje:
    * Jakie zmiany w istniejących funkcjonalnościach są potrzebne, aby w pełni wesprzeć ten proces?
    * Jakie nowe funkcjonalności należy stworzyć, aby w pełni wesprzeć ten proces?
2. W tym miejscu wykonawcy zadają dużo pytań. Upewnij się, że wszystkie otrzymają odpowiedź bądź hot-spot
3. Może się zdarzyć, że wykonawcy nie będą w stanie dookreślić zadania z powodu zbyt dużej ilości zmiennych. Poproś ich wtedy to dopisanie założeń w ramach których mogą jednoznacznie zidentyfikować zadania
4. Również w tym miejscu następuje aktualizacja procesu, zdarzeń oraz słownika pojęć
5. Poproś wykonawców o ocenę ilość pracy przy poszczególnych zadaniach w skali: S, M, L, XL. Jeśli mają trudność z oceną, pomóż im sformułować dodatkowe założenia, na bazie których odbędzie się oszacowanie
6. Poproś wykonawców o podanie przykłady dotychczasowych przedsięwzięć o podobnych rozmiarach. W jakim budżecie były w przeszłości realizowane podobne tematy?

Otrzymane przybliżenia budżetów są wstępnym oszacowaniem kosztu prac przy przyjętych założeniach.

## Etap 4c - Zakładanie backloga i szybki refinement

Na tym etapie dysponujesz już wstępnie zidentyfikowanymi zadaniami dla zespołu deweloperskiego. Niektóre z nich da się zrobić w jednym sprincie, na inne trzeba będzie poświęcić więcej czasu. 

Trudności, które najczęściej występują na tym etapie to:
* zbyt duże i mało konkretne zadania
* zbyt mała ilość informacji / zbyt duża ilość zmiennych, aby podzielić duże zadania na mniejsze i je oszacować
* brak jasności, o co właściwie chodzi w danym zadaniu

Aby poradzić sobie z wymienionymi trudnościami tak poprowadź warsztat, aby dla każdego z/dania określić:
* kto? co? po co? / dlaczego?
* scenariusz podstawowy, tzw. "happy day scenario"
* założenia

## kto? co? po co? / dlaczego?
Część __kto?__ definiuje _callera_ funkcjonalności. Może to być użytkownik-człowiek lub inny system korzystający z możliwości oprogramowania, nad którym pracujesz.

Część __co?__ to funkcjonalność. Innymi słowy __co?__ system robi dla użytkownika. Użytecznie jest myśleć o funkcjonalności jako o pewnym zadaniu, które użytkownik może wykonać w systemie; zadanie to ma początek, przebieg oraz koniec, np. "zaksięguj fakturę", "opłać zamówienie".

Część __po co / dlaczego?__ określa potrzebę, czyli biznesowy powód, który składania klienta do zapłaty za stworzenie nowej funkcjonalności w oprogramowaniu.

Szczegółową metodę definiowania potrzeb biznesowych znajdziesz:

* w rozdziale "Odkrywanie potrzeb" w mojej książce [Oprogramowanie szyte na miarę. Jak rozmawiać z klientem, który nie wie, czego chce?](https://www.michalbartyzel.pl/michal-bartyzel-ksiazki).
* LUB w darmowym ebooku [Conversation Patterns for Software Professionals](https://www.michalbartyzel.pl/michal-bartyzel-ksiazki#cp).


## scenariusz podstawowy, tzw. "happy day scenario"

W przeważającej większości przypadków możliwe jest napisanie scenariusza do zadania, to - nawet jeśli jest czasochłonne i złożone - jest ono na tyle konkretne, że nadaje się do dalszego procedowanie przez zespół np. do podzielenia na mniejsze części.

O szczegółach dzielenia na scenariusze przeczytasz w skrypcie "Język Gherking dla Procuct Ownerów". W praktyce możesz je przetrenować w trakcie warsztatu [Tworzenie specyfikacji w języku Gherkin dla Product Ownerów i Analityków Biznesowych](http://www.bnsit.pl/szkolenie,tworzenie-specyfikacji-w-jezyku-gherkin).

## założenia

Za pomocą założeń możesz poprawić oszacowania zadań. Na tak ogólnym poziomie, na którym obecnie operujemy, wiele istotnych dla dewloperów szczegółów pozostaje nieznanych bądź niedoprecyzowanych. Z tego powodu, za każdym razem, gdy deweloperzy zwrócą uwagę na zbyt dużą ilość możliwości albo brak precyzji, pomóż im spisać założenia, na których będą opierać swoje oszacowania oraz podział funkcjonalności na mniejsze części.

Jeśli w danym momencie nie możesz podjąć decyzji odnośnie do pytań deweloperów, poproś ich o przygotowanie dwóch, trzech lub więcej wersji zadania, jego oszacowania i podziału na części. Każda z alternatywnych wersji będzie opierać się na innym zestawie założeń. W ten sposób przeanalizujesz potencjalne możliwości oraz ryzyka czekające Was w trakcie pracy na oprogramowaniem.

---
# Event Storming dla Biznesu
* __Rozwijane przez__: [Michał Bartyzel](https://www.michalbartyzel.pl/michal-bartyzel)
* __Prawa autorskie i majątkowe__: [Michał Bartyzel](https://www.michalbartyzel.pl/michal-bartyzel)
* __Wersja__: 1.0
* Możesz używać i rozpowszechniać ten dokument w dowolny sposób o ile:
    - pozostawisz go w takiej formie, w jakiej go otrzymałeś - w całości
    - zawsze dodasz ww. informacje o autorstwie i prawach