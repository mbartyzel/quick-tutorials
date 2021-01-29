# O tym dokumencie
  * Ten dokument zawiera podpowiedzi, w jaki sposób programiści systemów wbudowanych, piszący w języku C na procesory z rdzeniem ARM, mogą wykorzystywać techniki #CleanCode, #TDD, #DI, #SOLID do dbania o czytelność swojego kodu.
  * Dokument utrzymuje: [Michał Bartyzel](http://blog.gettingthingsprogrammed.pl)
  * Treści pochodzą warsztatu [Techniki #CleanCode dla programistów embedded C](http://www.bnsit.pl/szkolenie,techniki-cleancode-dla-programistow-embedded-c)
  * Dokument udostępniany jest na licencji [Creative Commons: BY-NC](https://en.wikipedia.org/wiki/Creative_Commons_license)

# W czym problem?
Programiści systemów wbudowanych (ang. _embedded_) spotykają się z ogranicznieniami nieznanymi w świecie komputerów PC. 

Najpowszechniejsze z nich to:

* Ograniczone zasoby, np:
  * 4kB RAM
  * 60kb Flash
  * 8MHz CPU
* Kieczność częstego debugowania i pracy z kodem assemblera
* Mikrokontrolery czasem działają wadliwie
* Na urządzenie nie wolno wgrywać zbędnego kodu; rozwiązania typu [feature toggles](https://martinfowler.com/articles/feature-toggles.html) nie wchodzą w grę
* Konieczność ręcznego dbania o alkokację i zwalnianie pamięci; nie ma gc()
* Nierzadko wogóle nie wolno dynamicznie alokować pamięci w trakcie działania programu
* Należy brać pod uwagę w jaki sposób kod wynikowy będzie "układany" w pamięci RAM oraz rejestrach procesora
* Zasoby sprzętowe nie są tanie; zastosowanie lepszego mikrokontrolera najczęściej sprawi, że produkcja danego urządzania stanie się nieopłacalna
* Znakomita większość literatury, wpisów na blogu oraz wystąpień konferencyjnych związanych traktujacych o #CleanCode skupia się na "nowszych" językach programowania: Java, C#

---

# Upraszczaj warunki logiczne
W świecie systemów wbudowanych nie ma wyjątków, metody zwracają kody błędów i wszędzie wszystko należy sprawdzać, aby być pewnym czy można wykonać daną funkcję, czy nie. W związku z tym często spotykamy następujące konstrukcje:

```c
int read(const char* src, char* dest, int mode)
{
  char* _buff ...

  if ((NULL == src) || (NULL == dest) || (NULL == _buff))
  {
    return ERR_NOT_READY;
  }
  
  //wykonaj przetwarzanie
}
```

Możesz nieco poprawić czytelność tego kodu definiując makro `IS_NULL`

```c
#define IS_NULL(value) (NULL == (value))
```

Jak to wygląda po zmianie?

```c
int read(char* src, char* dest, int mode)
{
  char* _buff ...

  if (IS_NULL(src) ||  IS_NULL(dest) || IS_NULL(_buff))
  {
    return ERR_NOT_READY;
  }
  
  //wykonaj przetwarzanie
}
```

Inną techniką na poprawę czytelności jest _explaining macro_

```c
int read(char* src, char* dest, int mode)
{
  #define WRONG_INIT (src == NULL && NULL == dest) 
  #define IS_READING_LOCKED (NULL == _buff)
  
  if (WRONG_INIT && IS_READING_LOCKED)
  {
    return ERR_NOT_READY;
  }
  
  //wykonaj przetwarzanie
  
  //ew. #undef makr
}
```

# Wielokrotne wyjścia z metody
Porównaj dwa poniższe fragmenty fragmenty kodu

```c
int calculate(void)
{
  int returnValue = 0;
  
  if (warunek_1)
  {
    //Oblicz returnValue  
  } else if (warunek_2)
  {
    //Oblicz inaczej returnValue
    if (warunek_3) {
        //Oblicz inaczej returnValue
    } else
    {
        //Oblicz jeszcze inaczej returnValue
    }
  } else 
  {
    //Oblicz zupełnie inaczej returnValue
  }
  
  return returnValue;
}
```

```c
void int calcultate(void)
{
  if (warunek_1)
  {
    //Oblicz returnValue  
    return returnValue;
  } 
  
  if (warunek_2 && warunek_3)
  {
    //Oblicz inaczej returnValue
    return returnValue;
  } 
  
  if (warunek_2 && (FALSE == warunek_3))
  {
    //Oblicz inaczej jeszcze returnValue
    return returnValue;
  } 
  
  //Oblicz zupełnie inaczej returnValue
  
  return returnValue;
}
```

Plusy wielokrotnego wyjścia z metod:
  * czytelniejszy kod (dla większości programistów)
  * mniej zagnieżdżeń
  
Minusy wielokrotnego wyjścia z metod:
  * Nieco trudniej się debaguje
  * Trudniej zidentyfikować miejsca, w których należy zwolnić zaalowaną pamięć

Stosuj, gdy defensywnie sprawdzasz warunki uruchomienia funkcji

```c
int void send_char(const char c)
{
  if (FALSE == warunek_paczatkowy_1)
  {
    return;
  }
  
  if (FALSE == warunek_paczatkowy_2)
  {
    return;
  }
  
  //Skoro warunki spełnione, to funkcja może się wykonać
}
```

Stosuj, gdy kod w if-ie jest względnie krótki. Unikaj, gdy wyjście z funkcji miałoby nastąpić wewnątrz zagnieżdżonych bloków

```c
int send_char(const char c)
{
  for(unsigned int i; i <= buflen; ++i)
  {
    for(unsigned int j; j <= i; ++j)
    {
      if (warunek_1)
      {
        //Obliczenia
      } else if (warunek_2)
      {
        if(warunek_3)
        {
          //Obliczenia
          
          return 7; //tak nie rób; raczej przemyśl algorytm
        }
      }
    }
  }
  
  //Skoro warunki spełnione, to funkcja może się wykonać
}
```

# Modularyzuj swój kod
Traktuj plik *.c jako zamknięte, funkcjonalne części Twojego kodu. Funkcje, które udostępniasz na zewnątrz prefiksuj nazwą modułu.

[*] por. paragraf "Zadbaj o lepsze IDE".

```c
//com.h file
int COM_send(char);
int COM_send(char*);

//com.c file
int COM_send(char c);
{
  //implementacja
}

int COM_send(char* sequence);
{
  //implementacja
}
```

# Enkapsuluj dane i definuj API
Zmaiast bezpośrednio odwoływać się do składowych struktury, definiuj funkcje dostępowe. Jest to implemenacja zasady #TellDontAsk.

[*] por. paragraf "Zadbaj o lepsze IDE".

```c 
typedef struct Point
{
  int x,
  int y
};

void Point_move(Point* p, int dx, int dy);
void Point_set(Point* p, int x, int y);

//...


```

# Projektuj interfejsy do modułów
 * Pliki *.h traktuj jak interfejsy, a pliki *.c jako implementację Twoich modułów.
 * Projektuj API modułu dostępne dla innych modułów
 * Wewnętrzne funkcje modułów deklaruj jako `static`
 * Możesz użyć _syntactic sugar_ w postaci:
    - `#define _public`
    - `#define _private static`

```c
//plik *.h
#define _public
#define _private static

_public const char* getMessage_Key_1();

//plik *c
_public const char* getMessage_Key_1()
{
  //Implementacja
  module_memember_function();
}

_private int module_memember_function()
{
  //Implementacja
}


```

# Dekopmonuj funkcje
Zastosowanie wzorca [Composed Method](http://c2.com/ppr/wiki/WikiPagesAboutRefactoring/ComposedMethod.html) albo refaktoringu [Extract Method](https://refactoring.com/catalog/extractMethod.html) może być problematyczne ponieważ:
  * w kodzie wynikowym pojawią się skoki do funkcji
  * trudniej będzie Ci przewidzieć zajętność stosu
  * możesz spoktać kompilator, który dopuszcza ograniczoną liczbę zanieżdżonych wywołań funkcji
  
### Wtedy:
  * wydzielone funkcje deklaruj jako `inline`
  * wynikowy kod będzie nieco większy, lecz unikniesz skoków pomiędzy funkcjami oraz kopiowania paramterów

### Dodatkowo pmiętaj o tym, że:
  * funkcje `inline` powinny być proste i krótkie
  * `inline` to sugestia, a ostateczny wynik zależy od kompilatora

```c
void longMethod(void)
{
  part_1();
  part_2();
}

inline void part_1()
{
  //Implmentacja
}

inline void part_2()
{
  //Implementacja
}
```

#  Policy-based design zamiast preprocesora
Preprocesor sam w sobie nie jest problemem. Problemy sprawia używanie go wewnątrz funkcji.
Używaj preprocesora do wyboru zaenkapsulowanego fragmentu algorytmu.

```c
//HEADER FILE
#define DEVICE_0 1000
#define DEVICE_5 1005

void device_5_SpecificPart();
void (*  deviceSpecificPart)();

int handleRequest(char* name);
```

```c
//C FILE

#define DEVICE DEVICE_5

#if DEVICE == DEVICE_5
	void device5_SpecificPart()
	{
		//Implementacja
	}
#endif 

int handleRequest(char* name)
{
  //Implementacja
	deviceSpecificPart();

	return 0;
}


int main(int argc, char const *argv[])
{	
#if DEVICE == DEVICE_5 	
	deviceSpecificPart = device5_SpecificPart;
#endif

	handleRequest("New request");
	return 0;
}
```

# Zewnętrzne szablonowanie zamiast preprocesora
Urządzenie, które oprogramowujesz zazwyczaj współpracuje z innymi urządzeniami. Wdrażane jest w różnych konfiguracjach sprzętowych. Dodatkowo klienci posiadają swoje specyficzne oczekiwania odnośnie funkcjonalności. To wszystko sprawia, że Twój kod musi być przygotowany na kilka równoległych źródeł zmienności. Powiedzmy sobie szczerze, że taka sytuacja poważnie nadwyręża architekturę kodu. Dodatkowo nie chcemy pisać zbyt rozbudowanych rozwiązań, nie chcemy również, aby plik wynikowy był jak najmniejszy (nie powinno być tam nieużywanego kodu).

Pod presją czasu często stosowanym rozwiązaniem są dyrektywy preprocesora

```c
int recalculate(int tmode)
{
  //obliczenia
  
#ifdef CUSTOMER_1
    //kod specyficzny dla danego klienta
#endif
  
  //kolejne obliczenia
  
  for(unsigned int i; i < buff_len; ++i )
  {
    //obliczenia
    
#ifdef CFG_451
      //kod specyficzny dla danej konfiguracji sprzętowej
#endif
    
    //i dalsze obliczenia
  }
  
  return returnValue;
}
```

Opisane wcześniej _policy-based design_ nie zawsze jest tu dobrym rozwiązaniem, gdyż nieptrzebnie rozdrobniło by kod na niezliczoną ilosć niewielkich funkcji. 

Zamiast sterować kompilacją za pomocą dyrektyw `#if #endif` wykorzytaj zewnętrzy mechanizm szablonujący. W poniższym przykłądzie wykorzystano język [Python](https://www.python.org) oraz bibliotekę [Jinja2](http://jinja.pocoo.org).

Plik _keyboard-template.c_ zawiera w sobie znaczniki [Jinja2](http://jinja.pocoo.org) w postaci `{{zmienna}}`.

```c
//keyboard-template.c

//...

const char* getMessage_Key_0(int key)
{
	static int couter = 0;
	const int OPTIONS_NUM = {{key_0_options_num}}; 
	const char* MESSAGES[OPTIONS_NUM] = { {{key_0_options}} }; 

	if (couter == OPTIONS_NUM)
	{
		couter = 0;	
	} 

	return concat("0: ", MESSAGES[couter++]);
}

//...

```

Plik _keyboard-config.py_ generuje różne wersje plik `*.c` w zależności od potrzeb danej komplilacji.

```python
#keyboard-config.py

from jinja2 import *

def template():
	env = Environment(loader = FileSystemLoader("."), trim_blocks = True, lstrip_blocks = True)
	return env.get_template('keyboard-template.c')

def to_cfile(filename, content):
	file = open(filename,"w+")
	file.write(content)
	file.close()

if __name__ == '__main__':

#....

	key_0_options_num = 2
	key_0_options = '"SEG_2_APT", "TX_RC"'
	
	output = template().render(key_0_options_num, key_0_options)

  to_cfile('keyboard.c', output)
	  
#...
```

Wynikiem działania skryptu będzie plik `keyboard.c`..

```c
//keyboard.c

//...

const char* getMessage_Key_0(int key)
{
	static int couter = 0;
	const int OPTIONS_NUM = 2; 
	const char* MESSAGES[OPTIONS_NUM] = { "SEG_2_APT", "TX_RC" }; 

	if (couter == OPTIONS_NUM)
	{
		couter = 0;	
	} 

	return concat("0: ", MESSAGES[couter++]);
}

//...


```

# Idempotentność funcji
Idempotentność funcji oznacza, że f(x) = f(f(x)). Czyli wielokrotnie wywoływanie tej samej funkcji daje ten sam wynik.

W systemach budowancyh funkcje często współpracują ze sprzętem np. odczytując wyniki pomiarów. Najczęściej nie są one idempotentne, ponieważ dwukrotne wywoływanie tej samej funkcji może zaskutkować odczytaniem kolejnego pomiaru z czujnika. W takim wypadku nie możliwe jest użycie technik czystego kodu wynikających ze stosowania algebry Boole'a, na przykład:

```c
if (read_dev())
{
  if (IS_LOCKED)
  {
    push();
  }
} else
{
  send_signal();
}
```

można by zamienić na:

```c
if (read_dev() && IS_LOCKED) 
{
  push();
  return;
}

if (FALSE == read_dev()) //i tu może być zonk!
{
  return;
}
```

Jeśli jednak funkcja `read_dev()` nie jest idempotenta, to jej drugie wywołanie może dać odmienny rezultat niż pierwsze. 

# Efekty uboczne a Command-Query Separation 

TBD
# Współpraca między modułami
TBD
# Współpraca z obcym kodem
TBD
# Test-Driven Development i testy jednostkowe
TBD
# Programowanie poprzez interfejsy
TBD
# SOLID w kontekście kodu embedded C
TBD
# Co warto refaktoryzować?
TBD
# Naturalny Porządek Refaktoryzacji
TBD

# Ile pamięci zjada mój kod?
Jeśli nie jesteś pewien, czy wydajnie zaprojektowałeś swój kod, możesz posłużyć się prostym profilowaniem. Wykonaj następujące kroki:
1. Na początku metody `main()` wypełnij całą dostępną pamięć jakąś znaną wartością np. `0xAA55`
2. Załóż _break point_ tuż przed zakończeniem programu
3. Skompiluj i uruchom swój kod w trybie debugowania
3. Wykonaj wszystkie scenariusze swojego kodu w tym warunki brzegowa oraz te, co do których przewidujesz, że pochłaniją dużo pamięci
4. Podejrzyj pamięć i sprawdź ile pamięci wciąż posiada wartość `0xAA55`

Roboczo możesz przyjąć, że jeśli połowa pamięci nie została zagospodarowana przez dane programu, to jeszcze nie musisz się martwić.

# Zadbaj o lepsze IDE
Techniki #CleanCode należy rozpatrywać w kontekście danej technologii. Przyczyną zabiegów takich jak: prefiksowanie nazw funkcji, a czasem zmiennych, czy unikanie dekomponowania metod jest najczęściej ubogie IDE.

Wśród programistów C/C++ istnieje kulktura dużej dowolności stosowanego IDE nawet wewnątrz jednego zespołu. 

Narzędzia, których używają programiści emebedded C to chociażby: vim, Emacs, Notepad++, Sublime Text 3, Visual Studio Code, Eclipse C/C++, CrossWorks for ARM i inne narzędzia dostarczane przez producentów mikrokontrolerów (które to narzędzia zazwyczaj mają bardzo ubogie wsparcie do refaktoringu).

Zachęcam zespoły do ujednolicenia używanych IDE, a każdego programistę do oczekiwania od swojego pracodawcy zakupu IDE, które:

  * wspierają automatyczny refaktoring, przynajmniej:
    - zmiana nazwy
    - zmiana sygnatury
    - zmiana nazw plików
    - wyodrębnienie zmiennych, funkcji
    - ww. z wyegzekwowaniem zmian w plikach *.h i *c wraz odwołaniami `#ifndef header_h`
    - outline modułu
    - kolorowanie składni,
    - wyszukiwanie par nawiasów
    - wyszukiwanie tekstowe oraz po modułach i funkcjach
    - wyszukiwanie odwołań i wywołań
    - definiowanie task tagów (np. TODO) i ich outline
    - podpowiadanie składni, funkcji bibliotecznych i własnych inkludowanych
    - intelli sense
    - skróty klawiszowe
    - dark mode :)
  * mają debugger, który nie boi się skoków pomiędzy funkcjami

# Chcesz więcej?

# [Michał Bartyzel](http://blog.gettingthingsprogrammed.pl)
Zajmuję się przede wszystkim ułatwianiem współpracy na linii IT – biznes, uzwinnianiem organizacji, refaktoryzacją i efektywnością pracy.

Technikami, które opracowuję dzielę się w artykułach w magazynie [Programista](https://szukaj.programistamag.pl/search?q=bartyzel), portalu [InfoQ](https://www.infoq.com/profile/Michał-Bartyzel), [blogu](http://blog.gettingthingsprogrammed.pl)), [konferencjach](http://www.bnsit.pl/konferencje). Jestem autorem książek: [_Oprogramowanie szyte na miarę. Jak rozmawiać z klientem, który nie wie, czego chce?_](https://helion.pl/ksiazki/oprogramowanie-szyte-na-miare-jak-rozmawiac-z-klientem-ktory-nie-wie-czego-chce-wydanie-ii-rozsz-michal-bartyzel,opszm2.htm#format/e), [_Getting Things Programmed_](https://helion.pl/ksiazki/getting-things-programmed-droga-do-efektywnosci-michal-bartyzel,droppp_ebook.htm), [_Conversation Patterns for Software Professionals_](https://www.infoq.com/minibooks/conversation-patterns) oraz [_Eseje o efektywności programistów_](http://www.bnsit.pl/ksiazka,eseje-o-efektywnosci-programistow).

Moja praca w liczbach to:

* 600+ dni poprowadzonych szkoleń
* 9 projektów doradczych związanych ze zwinną transformacją
* 40+ artykułów w prasie branżowej
* 4 napisane książki
* 80+ klientów

[blog](http://blog.gettingthingsprogrammed.pl) | [linkedin](https://www.linkedin.com/in/mbartyzel) | [twitter](https://twitter.com/MichalBartyzel) | [facebook](https://www.facebook.com/gettingthingsprogrammed)