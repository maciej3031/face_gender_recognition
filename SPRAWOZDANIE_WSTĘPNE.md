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
####  3.3.2 Ekstracja cech za pomocą sieci CNN.
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