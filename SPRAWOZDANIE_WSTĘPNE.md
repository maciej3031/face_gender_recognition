### SPRAWOZDANIE WSTĘPNE - ROZPOZNAWANIE PŁCI NA PODSTAWIE ZDJĘĆ TWARZY

### 1. Opis zadania

Zadanie polega na stworzeniu trzech klasyfikatorów:
- Perceptronu wielowarstwowego (MLP)
- Maszyny wektorów nośnych (SVM)
- Głębokiej sieci splotowej (CNN)

Następnie wytrenowaniu każdego z nich w taki sposób, aby jak najlepiej radził sobie
z postawionym zadaniem i porównaniu wyników końcowych z wyciągnięciem wniosków dotyczących każego z 
zastosowanych podejść. Ponadto wnioski obejmować będą również użyte oprogramowanie pod kątem jego użyteczności,
dostępności i przyjazności użytkownikowi.

### 2. Opis zbioru danych

Jako zbiór danych wykorzystano zbiór *"Labeled Faces in the Wild"*:

http://vis-www.cs.umass.edu/lfw/

Jest to zbiór ponad 13 000 zdjęć twarzy zebranych z internetu pod kątem zagadnień związanych z rozpoznawanie i
klasyfikacją twarzy. Każda twarz w zbiorze jest oznaczona m.in. imeniem, nazwiskiem i płcią. Wszystkie zdjęcia mają
rozmiar 250x250 pixeli. Położenie twarzy zostało dostosowane w taki sposób, aby leżała ona w centralnej części zdjęcia,
była nie obrócona i o podobnej wielkości na każdym zdjęciu.

### 3. Spsób rozwiązania  
####  3.1. MLP    
####  3.2. SVM  
####  3.3. CNN  
####  3.3.1 Zbiór danych 
Do uczenia wybrano ze zbioru losowo po 3000 zdjęć kobiet i mężczyzn, tak aby zbiór uczący był zbalansowany
(Taka ilość wynika z faktu, iż w zbiorze 13000 zdjęć, jedynie około 3000 to zdjęcia kobiet).
Zdjęcia zostały podzielone na 3 zbiory: treningowy, walidacyjny i testowy w stosunku: 0,2 : 0,2 : 0,6. 
Następnie zbiór testowy został odseparowany i pozostawiony dopiero do końcowej oceny modelu.
Trenowanie sieci odbywało się z użyciem zbioru treningowego, natomiast dobór hiperparametrów został zrobiony, przy pomocy
zbioru walidacyjnego. 
  
####  3.3.2 Architektura sieci

Do rozwiązania zadania została użyta sieć o następującej architekturze:
  - Pierwsza warstwa splotowa
  - Warstwa typu: *"max pooling"*
  - Druga Warstwa splotowa
  - Warstwa typu: *"max pooling"*
  - Warstwa spłaszczająca otrzymane dwu wyiarowe mapy cech do wektora jednowymiarowego
  - Warstwa gęsta z *"dropoutem"*
  - Warstwa wyjściowa z funkcją aktywacji typu *sigmoid* jako klasyfikator binarny.
  
Dokładna architektura sieci z hiperparametrami zostanie przedstawiona w raporcie końcowym.
  
#### 3.3.3 Dobór hiperparametrów i proces uczenia  

Sieć zostanie wytrenowana na wielu różnych kombinacjach hiperparametrów takich jak:
 - Liczba epok
 - Współczynnik uczenia *learning rate*
 - Współczynnik regularyzacji
 - Rozmiar wejściowy obrazu
 - Rozmiar i liczba filtrów w warstwach głębokich
 - Liczba neuronów w warstwie gęstej

#### 3.3.4 Miara jakości i wyniki
  
Jako, że wybrany do uczenia zbiór jest dobrze zbalansowany, jako miarę jakości postanowiono przyjąć parametr
*acuraccy* opisujący stosunek dobrze sklasyfikowanych obrazów do liczby wszystkich klasyfikowanych obrazów.
Ponadto do wyniku końcowego dołączone zostaną, także wykresy uczenia sieci.