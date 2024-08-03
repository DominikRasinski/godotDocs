# Podstawy korzystania z silnika do gier Godot
Spis treści:

- [Sceny](#sceny)
- [Łaczenie komponentów](#łaczenie-komponentów-z-scen-aby-z-sobą-reagowały)
- [Poprawienie pixeli](#poprawienie-pixeli)
- [Tworzenie gracza](#tworzenie-gracza)

## Sceny
Sceny to główny element silnika które są wykorzystywane do opisu poszczególnych elementów gry takich jak:
- Poziomy
- Postacie
- Gracz
- Przeciwnicy
itd...

Sceny mogą być zagnieżdżone w innej scenie, podobny sposób do budowy jak z wykorzystaniem komponentów.

## Łaczenie komponentów z scen aby z sobą reagowały
Możemy przenosić interesujące nas komponenty od razu do kodu przeciągając je z drzewa komponentów do kodu podczas tej operacji musimy trzymać CTRL

CTRL + przeciągniecie komponentu - od razu nam stworzy zmienną w kodzie która bedzie się odwoływać do elementu.
Taka zmienna stworzona przez godot posiada od razu zdefiniowaną metodę `@onready` która zapewnia nas, że element na którym pracujemy będzie dostępny do użycia.

### Kolejnym sposobem na łaczenie elementów jest wywoływanie sygnałów
Przykładem na wykonanie sygnału może być zwykły przycisk po kliknięciu na niego ukaże nam się sekcja po prawej stronie o nazwie `inspektor` a obok niej jest sekcja `Węzeł` która pokazuje nam wszystkie dostępne sygnały dla danego węzła - sygnały są dziedziczone od węzłów rodziców.

Aby wykorzystać któryś z dostępnych sygnałów jak na przykład sygnał `pressed` należy na niego kliknąć i połączyć go z wezłem który posiada już dołączony do niego skrypt. Dzieki takiemu połączeniu a skrypcie ukaże się nam funkcja `_on_Button_pressed()` która jest sygnałem tego przycisku który edytowaliśmy do wykonania określonej akcji.

## Poprawienie pixeli
Godot w domyślnych ustawieniach projektu ma ustawione pixele na `linear` co rozmazuje dosyć sprity jakie będziemy wykorzystywać w projekcie aby to zmienić i nie musieć tego zmieniać dla każdego sprite'a który zostanie dodany do projektu należy:
1. W menu `Project` wybrać opcję `Project Settings`
2. W sekcji `Rendering` -> `Textures` zmienić opcję `Texture Filter` na `Nearest`

## Tworzenie gracza
Aby stworzyć gracza najlepiej zacząć to od stworzenia nowej sceny, która powinna mieć nazwę Player w scenie należy dodać odpowiednie elementy:
1. Dodajemy do sceny komponent `CharacterBody2D` - główny węzeł
2. Dodajemy do sceny komponent `AnimatedSprite` - jeżeli chcemy aby nasza postać posiadała animacje
   - Jeżeli chcemy aby postać **NIE** była animowana możemy skorzystać z komponentu `Sprite`
3. Dodajemy do sceny komponent `CollisionShape2D` - węzeł odpowiedzialny za kolizję postaci
4. Dodajemy do sceny komponent `Camera2D` - opcjonalny węzeł jeżeli chcemy aby kamera podążała za naszą postacią
5. Dodajemy do sceny węzeł `AnimationPlayer` lub `AnimationTree` - oba węzły są odpowiedzialne za odtwarzanie animacji postaci. Ten sposób jest bardziej 'czysty' niż hardkodowanie animacji w skrypcie dołączonym do głównego węzła.

### Opis konfiguracji poszczególnych węzłów aby stworzyć gracza
#### CharacterBody2D
Dodanie komponentu `CharacterBody2D` do sceny ten komponent musi być dodany jako `root` dla sceny
### AnimatedSprite
Dodanie komponentu w zależności od tego czy postać mam być animowana czy nie to dodajemy `AnimatedSprite` lub `Sprite`
   1. po dodaniu komponentu przeznaczonego dla sprite'u należy dodać do niego sprite lub spritesheet dla animowanego spite'a wszystko robimy w inspektorze po prawej stronie
      - konfiguracja `AnimatedSprite` należy dodać w sekcji `Animation` -> `Sprite Frames` -> `Nowy SpriteFrames`
      - na dole pojawi nam się okno `SpriteFrames` w którym należy dodać nasze sprite'y może to być spitesheet lub oddzielnie wykonane pojedyncze sprity animacji. Ale z racji, że to jest animacja to naj lepiej wykorzystywać spitesheet's.
### CollisionShape2D
Dodanie komponentu `CollisionShape2D` do sceny
   1. dodanie do `CollisionShape2D` kolizji po prawej stronie w inspektorze wybieramy interesujący nas kształt kolizji, który będzie pokrywać nasz sprite
### Camera2D
Dodanie komponentu `Camera2D` do sceny spowoduje, że kamera będzie podążać za postacią
    - `Camera2D` daje nam kilka przydatnych opcji takich jak możliwość przybliżenia kamery, ustawić to możemy w sekcji `Camera2D` -> `Zoom`
### AnimationPlayer
Dodanie komponentu `AnimationPlayer` lub `AnimationTree` do sceny komponenty stricte odpowiedzialne za animowanie postaci
1. Aby stworzyć animację musimy posiadać w drzewie komponent o nazwie `AnimatedSprite`
2. Przechodzimy do niego i na dole odnajdujemy sekcję o nazwie `Animation` po kliknięciu otworzy nam się nad tą sekcją okno animacji. Dodawanie nowej animacji możemy dokonać za pomocą kliknięcia przyciski `Animation` -> `New...` uzupełniamy nazwę animacji (musi być dokładnie taka sama jak nazwa animacji w sekcji `SpriteFrames`)
3. Po prawej stronie w sekcji inspektora znajdujemy sekcję `Animation` -> `Animation` w której wybieramy nas interesująca animację klikamy na ikonę `klucza` obok nazwy animacji i dodajemy nową właściwość do lini czasu animacji. Odznaczamy aby nie tworzył nam ścieżki `RESET`
4. Po utworzeniu nowej animacji w linii czasu należy dodać nowe klatki animacji. Robimy to wybierają pojedynczo klatkę animacji w sekcji `Animation` -> `Frame` i klikając ikonę `klucza` obok
   - Linię czasu animacji musimy przybliżyć aby klatki animacji nie były oddzielone od siebie w sekundach aby tego dokonać klikamy na dolne okno z otwartą linią czasu, trzymając `CTRL` + `Kółko myszy` możemy dostosować linię czasu.
5. Po dodaniu wszystkich klatek animacji należy dostosować długość animacji zmieniając wartość `1` na wartość najbliższej pełnej klatki po klatce animacji.
6. Zapętlenie animacji wykonujemy po kliknięciu ikonki `Zapętlenia` czyli dwóch strzałek po prawej stronie okna animacji u dołu ekranu
7. Animacja podstawowa `Idle` animacja podstawowa ma to do siebie, że powinna być odgrywana w momencie gdy postać jest dodana do sceny dlatego aby tego dokonać należy zaznaczyć jaka animacja powinna być odgrywana jako podstawowa i kliknąć ikonę `Play z znakiem A`
 
### Dodanie skryptu
Po wszystkich czynnościach możemy przejść do dodania skryptu dla naszego gracza dokonujemy tego kliknięciu w `root` drzewa komponentów i wybieramy `Add Script`. Jeżeli `rootem` jest `CharacterBody2D` to przy dodawaniu skryptu zaznaczamy 'szablon' o nazwie `CharacterBody2D: Basic Movement`, dzięki czemu będziemy mieć skonfigurowane podstawowe funkcje dla naszej postaci.