
# STEP 5 - Strings
1. [Char functions](#char) 
2. [C style strings](#cstyle)
3. [C++ strings](#cppstrings)
4. [Zadanie](#zadanie)

---

<a name="char"></a>
### Char functions

`c++` posiada standardową biblioteke `<cctype>` pomocą przy konwertowaniu lub sprawdzaniu pojedyńczych znaków.
W skład `cctype` wchodzą międzyinnymi funkcje:

```c++
char c { 'a' };

isalpha(c); //czy c jest literą
isalnum(c); //czy c jest literą lub liczbą
isdigit(c); //czy c jest liczbą
islower(c); //czy c jest małą literą
isprint(c); //czy c jest drukowalnym znakiem
ispunct(c); //czy c jest interpunkcją
isupper(c); //czy c jest wielką literą
isspace(c); //czy c jest spacją

c = tolower(c); //zwraca c pisane małą literą
c = toupper(c); //zwraca c pisane wielką literą
```

---

<a name="cstyle"></a>
### C style strings

`C style string` to stare podejście do tematu przechowywania tekstu. Jako że w naszych
aplikacjach będziemy używać nowszego podejścia tylko niewielki wstęp o co chodziło.

W języku `C` string był reprezentowany jako zwykła tablica znaków `char` co przysparzało
trochę problemów i było niewygodne. Jak wiemy tablice w `c++` mają stały rozmiar dlatego 
ciężko było zmienić raz utworzony ciąg znaków. W przypadkach kiedy chcieliśmy go zmienić,
coś dodać potrzebne było kopiowanie całego ciągu do nowej tablicy znaków.

`c++` mamy biblioteke pomocniczną `<cstring>` która umożliwia nam operacje na stringach
```c++
#include <cstring>

int main()
{
char text[80]; 

std::strcpy(text, "Hello "); //kopiowanie wartości "Hello " do naszej tablicy znaków
std::strcat(text, "there"); //łączymy nasz text z wartością "there"
std::cout << std::strlen(text); //wypisujemy długość naszego tekstu 
std::strcmp(text, "Compare"); //Wypisze -1 ponieważ podane ciągi różnią się
}
```

---

<a name="cppstring"></a>
### C++ strings

W `c++` typ danych string zawarty jest w standardowej bibliotece `<string>` w namespace `std`.
Aby z niego korzystać trzeba go zaincludować na początku pliku. 

Typ `string` w `c++` charakteryzuje się kilkoma właściwościami:
* ma dynamiczny rozmiar
* jest przechowywany w pamięci
* działa z streamami `input` i `output` (cout <<, cin >>) 
* posiada wiele przydatnych funkcji
* może być używany z operatorami przypisania, artmetycznymi, porównania (++, <=, = itd.)
* jest bezpieczniejszy od klasycznego podejścia w `C`

Zmienne typu string deklarujemy i inicjalizujemy w ten sam sposób jak wszystkie inne typy.
```c++
#include <string>

int main()
{
    std::string text1;
    std::string text2 { "Franek" }; //Franek
    std::string text3 { text2 }; //Franek
    std::string text4 { "Franek", 3};//Fra
    std::string text5 { "Franek", 0, 3 };//Fra
    std::string text6 ("X",3); //XXX
}
```

`Strign` w c++ to tak naprawdę wektor dlatego możemy na nim wykonywać wszystkie te same 
operacje co na wektorach

```c++
string text { "Franek" };
cout << text.at(1);//Wypisze r
``` 

Stringi także możemy do siebie porównywać i dodawać bez konieczności kopiowania ich

```c++
string text { "Franek" };
string text2 { "Franek" };

if (text == text2)
{
    cout << text + " " + text2;
}
//Wypisze Franek Franek
```

Kilka przydatnych metod z biblioteki `string`.

```c++
string text { "Franek" };

//substr to funkcja pobierająca określoną część wartości 
//pierwszy argument to indeks od którego startujemy
//drugi to ilość pobieranych znaków
cout << text.substr(0,4);//Wypisze Fran 

//find wyszukuje w podanym stringu innego. Jeżeli znajdzie zwraca indeks 
//pierwszego znaku, jeżeli nie zwraca string::npos
cout << text.find("ane");//Wypisze 2

//clear czyści cały string
text.clear();

//erase usunie z stringa wskazany zakres znaków
//pierwszy argument to indeks startowy
//drugi to ilość znaków do usunięcia
cout << text.erase(0,3);//Wypisze nek

//lenght zwraca długość naszego stringa
cout << text.lenght();//Wypisze 6
```

Ważną rzeczą jest że jeżeli chcemy przypisać do zmiennej `string` wpisaną wartość
z konsoli, użawamy funkcji `getline()`. Wtedy mamy pewność że cała wprowadzona wartość 
zostanie przypisana do naszej zmiennej.

```c++
string text;

//pierwszy parametr to stream z którego zaczytane zostaną dane 
//druga to zmienna do której zostanie przypisana wartość
getline(cin, text);
```

Ciekawostka.
W `c++` jeżeli zrzutujemy pojedyńczy znak na typ `int` odrzymamy jego reprezentacje w liczbie
całkowitej.

---

<a name="zadanie"></a>
### Zadanie

```c++
#include <iostream>

int main()
{
    /*
     Zadanie
     Przy użuciu biblioteki string napisz prosty program który szyfruje
     wprowadzone przez użytkownika zdanie. Użyj szyfru cezara z ustalonym stałym przesunięciem.]
     1.Odczytaj wprowadzony tekst przez użytkownika
     2.Zaszyfruj tekst oraz wypisz na ekranie
     3.Następnie już zaszyfrowany tekst poddaj deszyfryzacji
     4.Ponownie wypisz na ekranie zdeszyfrowany tekst
     */
}
```