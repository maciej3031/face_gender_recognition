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
####  3.1 Ekstracja cech za pomocą sieci CNN.
Do wykonania powyższego zadania wykorzystamy środowisko MATLAB. 
Estrakcję cech wykonamy na wyżej wymienionym zbiorze danych oraz użyjemy sieci splotowej AlexNet. Jest to sieć przetrenowana i składa się z 25 warstw:
	 1   'data'     Image Input                   227x227x3 images with 'zerocenter' normalization
     2   'conv1'    Convolution                   96 11x11x3 convolutions with stride [4  4] and padding [0  0]
     3   'relu1'    ReLU                          ReLU
     4   'norm1'    Cross Channel Normalization   cross channel normalization with 5 channels per element
     5   'pool1'    Max Pooling                   3x3 max pooling with stride [2  2] and padding [0  0]
     6   'conv2'    Convolution                   256 5x5x48 convolutions with stride [1  1] and padding [2  2]
     7   'relu2'    ReLU                          ReLU
     8   'norm2'    Cross Channel Normalization   cross channel normalization with 5 channels per element
     9   'pool2'    Max Pooling                   3x3 max pooling with stride [2  2] and padding [0  0]
    10   'conv3'    Convolution                   384 3x3x256 convolutions with stride [1  1] and padding [1  1]
    11   'relu3'    ReLU                          ReLU
    12   'conv4'    Convolution                   384 3x3x192 convolutions with stride [1  1] and padding [1  1]
    13   'relu4'    ReLU                          ReLU
    14   'conv5'    Convolution                   256 3x3x192 convolutions with stride [1  1] and padding [1  1]
    15   'relu5'    ReLU                          ReLU
    16   'pool5'    Max Pooling                   3x3 max pooling with stride [2  2] and padding [0  0]
    17   'fc6'      Fully Connected               4096 fully connected layer
    18   'relu6'    ReLU                          ReLU
    19   'drop6'    Dropout                       50% dropout
    20   'fc7'      Fully Connected               4096 fully connected layer
    21   'relu7'    ReLU                          ReLU
    22   'drop7'    Dropout                       50% dropout
    23   'fc8'      Fully Connected               1000 fully connected layer
    24   'prob'     Softmax                       softmax
    25   'output'   Classification Output         crossentropyex with 'tench', 'goldfish', and 998 other classes
	
Uzyskane cechy pobierzemy z ostatniej warstwy przed klasyfikacją. W tym wypadku jest to warstwa 'fc7'.

####  3.2. MLP
####  3.2.1 Sposób ekstrakcji cech
Cechy użyte jako wejścia perceptronu wielowarstwowego uzyskane zostaną za pomocą metody Eingefaces zaimplementowanej w programie matlab. Jest to popularna metoda wywodząca się z teorii informacji korzystająca z analizy składowych głównych PCA w celu zmniejszenia wymiarowości reprezentacji obrazów. W metodzie tej dążymy to przekształcenia dużego wektora danych skorelowanych, w zbiór parametrów (przy czym będzie to zbiór stosunkowo niewielki) o zróżnicowanej wariancji, dobrze opisujący dany obraz. Jest to metoda  z powodzeniem wykorzystywana do rozpoznawania twarzy.
####  3.2.2 Dobór parametrów sieci
W ramach projektu przebadany zostanie wpływ poszczególnych parametrów sieci na jakość klasyfikacji. Wśród badanych przez nas parametrów znajdują się:
- liczba neuronów w poszczególnych warstwach,
- liczba warstw ukrytych,
- rodzaj funkcji aktywacji.
Badania przeprowadzone zostaną z wykorzystaniem sieci neuronowych zaimplementowanych w Neural Network Toolbox programu Matlab.
####  3.2.3 Ocena jakości
Kryterium oceny sieci będzie procent poprawnie zaklasyfikowanych obrazów.

###  3.3. SVM (Support Vector Machine)
#### 3.3.1 Ogólny opis algorytmu

Algorytm SVM jako metoda uczenia nadzorowanego wymaga wytrenowania modelu za pomocą zaklasyfikowanych danych.
Dzieli ona wielowymiarową przestrzeń na dwie klasy używając tak zwanych wektorów nośnych, które odnoszą się do wzajemnych
odległości poszczególnych obserwacji. Celem jest by znaleziona hiperpłaszczyzna rozdzielała przestrzeń (która może zostać
przekształcona za pomocą tzw. funkcji jądra) na dwie, jak najbardziej odseparowane od siebie, klasy.

####  3.3.2 Sposób ekstrakcji cech
Do ekstrakcji cech z zaproponowanego zbioru danych wykorzystane 2 metody:
- konwolucyjna sieć neuronowa zaproponowana w punkcie 3.1
- metoda Eingefaces opisana w punkcie 3.2.1

#### 3.3.3 Sposób wykorzystania zbioru danych
Zbiór danych zostanie podzielony na zbiór treningowy oraz testujący. Zostaną wykonane eksperymenty dotyczące czasu trenowania
w zależności od wielkości zbioru trenującego. Przeprowadzony zostanie również eksperyment na zbiorze trenującym
identycznym do  tego wykorzystanego w MLP i CNN, mający na celu porównanie metod.

Zaproponowany zbiór danych trenujących jest podzielony na dwie klasy (płci: kobieta/mężczyzna). Każdy z obrazów
ze zbioru zostanie przetworzony narzędziem służącym do ekstrakcji cech a następnie podany do trenowania metodą SVM.

#### 3.3.4 Parametry klasyfikatora

##### Funkcja jądra
Wskazane metody ekstrakcji danych umożliwiają uzyskanie różnej ilości cech. Zostanie przetestowane kilka
funkcji jądra w celu znalezienia najlepiej klasyfikującej dany zbiór cech. Dobór cech zostanie dobrany tak
aby nie było overfittingu.

##### Parametry gamma i współczynnik kary i inne
Zostanie przetestowany również wpływ tych współczynników na klasyfikację.

#### 3.3.5
Wykorzystane narzędzia:
Do esktrakcji cech zostaną wykorzystane przedstawione wcześniej metody (ewentualnie im tożsame biblioteki w Pythone).
SVM będzie trenowany oraz testowany prze użyciu pakietu scikit-learn Pythona.

####  3.4. CNN  
####  3.4.1 Zbiór danych 
Do uczenia wybrano ze zbioru losowo po 3000 zdjęć kobiet i mężczyzn, tak aby zbiór uczący był zbalansowany
(Taka ilość wynika z faktu, iż w zbiorze 13000 zdjęć, jedynie około 3000 to zdjęcia kobiet).
Zdjęcia zostały podzielone na 3 zbiory: treningowy, walidacyjny i testowy w stosunku: 0,2 : 0,2 : 0,6. 
Następnie zbiór testowy został odseparowany i pozostawiony dopiero do końcowej oceny modelu.
Trenowanie sieci odbywało się z użyciem zbioru treningowego, natomiast dobór hiperparametrów został zrobiony, przy pomocy
zbioru walidacyjnego. 

####  3.4.2 Architektura sieci

Do rozwiązania zadania została użyta sieć o następującej architekturze:
  - Pierwsza warstwa splotowa
  - Warstwa typu: *"max pooling"*
  - Druga Warstwa splotowa
  - Warstwa typu: *"max pooling"*
  - Warstwa spłaszczająca otrzymane dwu wyiarowe mapy cech do wektora jednowymiarowego
  - Warstwa gęsta z *"dropoutem"*
  - Warstwa wyjściowa z funkcją aktywacji typu *sigmoid* jako klasyfikator binarny.
  
Dokładna architektura sieci z hiperparametrami zostanie przedstawiona w raporcie końcowym.
  
#### 3.4.3 Dobór hiperparametrów i proces uczenia  

Sieć zostanie wytrenowana na wielu różnych kombinacjach hiperparametrów takich jak:
 - Liczba epok
 - Współczynnik uczenia *learning rate*
 - Współczynnik regularyzacji
 - Rozmiar wejściowy obrazu
 - Rozmiar i liczba filtrów w warstwach głębokich
 - Liczba neuronów w warstwie gęstej

#### 3.4.4 Miara jakości i wyniki
  
Jako, że wybrany do uczenia zbiór jest dobrze zbalansowany, jako miarę jakości postanowiono przyjąć parametr
*acuraccy* opisujący stosunek dobrze sklasyfikowanych obrazów do liczby wszystkich klasyfikowanych obrazów.
Ponadto do wyniku końcowego dołączone zostaną, także wykresy uczenia sieci.

  
