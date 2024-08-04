# Podstawy korzystania z silnika do gier Godot
Spis treści:

- [Sceny](#sceny)
- [Łaczenie komponentów](#łaczenie-komponentów-z-scen-aby-z-sobą-reagowały)
- [Poprawienie pixeli](#poprawienie-pixeli)
- [Tworzenie gracza](#tworzenie-gracza)
- [TileMap](#tilemap)
- [Tworzenie Background](#tworzenie-background)
- [Tworzenie Przeciwnika](#tworzenie-przeciwnika)
- [Globalny Skrypt](#globalny-skrypt)
- [Drabina](#tworzenie-drabiny)

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
6. Zapętlenie animacji wykonujemy po kliknięciu ikonki `Zapętlenia` czyli dwóch strzałek po prawej stronie okna animacji u dołu ekranu ![](/img/zapetlenie.png)
7. Animacja podstawowa `Idle` animacja podstawowa ma to do siebie, że powinna być odgrywana w momencie gdy postać jest dodana do sceny dlatego aby tego dokonać należy zaznaczyć jaka animacja powinna być odgrywana jako podstawowa i kliknąć ikonę `Play z znakiem A` ![](/img/default-play.png)
 
### Dodanie skryptu
Po wszystkich czynnościach możemy przejść do dodania skryptu dla naszego gracza dokonujemy tego kliknięciu w `root` drzewa komponentów i wybieramy `Add Script`. Jeżeli `rootem` jest `CharacterBody2D` to przy dodawaniu skryptu zaznaczamy 'szablon' o nazwie `CharacterBody2D: Basic Movement`, dzięki czemu będziemy mieć skonfigurowane podstawowe funkcje dla naszej postaci.

Tworzenie prostego skryptu obsługującego HP gracza

```
var health = 10

<!-- Usunięcie gracza i przerzucenie do menu głównego gdy jest martwy -->
if health <= 0:
   queue_free()
   get_tree().change_scene_to_file("res://mainMenu/main_menu.tscn")
```

Wyświetlanie HP w poziomie - najlepiej wykorzystać komponent `Label` w scenie poziomu z wbudowanym skryptem który będzie wyświetlał zaktualizowaną wartość HP gracza
```
func _process(delta):
	text = "HP: " + str(get_node("../../Player/Player").health)
   
```

Oczywiście do aktualizowania wartości HP gracza należy posiadać przeciwnika który będzie zadawał obrażenia graczowi, tworzenie przeciwnika można przeczytać w tej sekcji [Tworzenie przeciwnika](#tworzenie-przeciwnika)


## TileMap
Komponent TileMap służy nam do tworzenia poziomów z wykorzystaniem sprite'ów które po skonfigurowaniu TileMapy możemy bez trosko malować na Backgroundzie.
Aby skonfigurować TileMap należy:
1. Dodać komponent `TileMap` do sceny
   - po prawej stronie w sekcji `TileMap` -> `Tileset` dodajemy nowy tileset
   - aby skonfigurować kolizję dodajemy w `TileMap` -> `TileSet` -> `Physics Layers` -> `Add element`
   - przechodzimy do sekcji `TileSet` na dole, wybieramy sekcję `Paint` -> `Phusics` -> `Layer of physics` aby zastosować nową kolizję na wskazanym elemencie należy w seksji na dole `TileSet` po prostu kliknąć na wskazany sprite i dostosować kolizję do sprite'a
2. Tworzenie poziomu za pomocą `TileMap` musimy na dole edytora zaznaczyć sekcję `TileMap` i po prtostu wybrać pole w poziomie aby je zamalować nowym tilem

## Tworzenie BackGround
Do tworzenia backgroundu służy komponent `ParallaxBackground`.
1. Dodajemy komponent `ParallaxBackground` do sceny
2. Dodajemy do sceny komponent `ParallaxLayer`
3. Dodajemy do sceny komponent `Sprite`

Aby ustawić nieskończone przesuwanie się tła należy każdy `ParallaxLayer` ustawić `Mirroring` w punkcie kończenia się grafiki. Czyli jeżeli grafika się kończy w punkcie 1100 to mysimy wpisać w `Mirroring` do pola `x` wartość 1100. `x` i `y` to osie x jest odpowiedzialne za oś horyzontalną a y za oś wertykalną.
Możemy również spowolnić przewijanie się tła ustawiając w `ParallaxLayer` -> `Scale` dla wartości horyzontalnej wybieramy `x` a dla wartości wertykalnej `y` zmniejszanie wartości zmniejsza prędkość przesuwania się tła.

## Tworzenie przeciwnika
Proces tworzenia przeciwnika przechodzi dosyć w podobny sposób jak proces tworzenia gracza z wyjatkiem, że przy przeciwniku nie wykorzystujemy `Camera2D` oraz `AnimationPlayer`.

Najważniejszą różnicą przeciwnika jest to, że przeciwnik nie jest sterowany przez gracza i musimy rozwiązać problem poznania lokalizacji gdzie przebywa gracz, aby tego dokonać wykorzystujemy zazwyczaj komponent `Area2D`

1. Dodajemy komponent `Area2D` konfigurujemy jego kolizję za pomocą komponentu `CollisionShape2D`
2. Do komponentu `CharacterBody2D` dodajemy skrypt może być to szablon wykorzystywany w graczu.
3. Z komponentu `Area2D` wysyłamy sygnał `body_entered` do komponentu `CharacterBody2D`
   - Jeżeli zmienna `body` jest równa nawie ciała jakie posiada gracz (ustalona nazwa node **CharacterBody2D** w scenie z graczem) możemy przejść do konfiguracji akcji jakie mają zajść przy kolizji gracza z kolizją strefy przeciwnika.
4. Stworzenie funkcji śledzącej pozycję gracza
```
@onready var player = get_node("../../Player/Player")
@onready var animSprite = get_node("AnimatedSprite2D")
var chase = false

func findPlayer(): 
	var direction = (player.position - self.position).normalized()
	if direction.x > 0:
		animSprite.flip_h = true
	else:
		animSprite.flip_h = false
	velocity.x = direction.x * SPEED
	animSprite.play("Jump")
```

## Globalny skrypt
Skrypty globalne możemy dodać do gry po prostu za pomocą dodania nowego sktyptu
1. Tworzymy skrypt na przykład `Global.gd`
2. Ustawienia `Projekt` -> `Autoładowanie` -> `dodanie ścieżki do skryptu` -> `Zaznaczenie na włączone`
3. Skrypt będzie się automatycznie ładował po uruchomieniu gry

## Tworzenie drabiny
Tworzenie drabiny jest trochę skomplikowane pod warunkiem jeżeli wykorzystamy do tego sposób aby nie dodawać oddzielnego sprite'a oraz oddzielnej detekcji kolizji

1. Tworzymy `TileMap` wraz z `TileSet` z jednym sprite'em który będzie naszą drabiną - nazwa `TileMap` powinna zostać zmieniona na `Ladder`
2. Po dodaniu `TileSet` ustawiamy po prawej stronie w inspektorze `Physic Layers` -> `Collision Layer` -> `dodajemy nową warstwę z nazwą Ladder`
w tym samym miejscu wyłączamy `Collision Mask` aby nic nie mogło reagować na drabinę, oczywiście prócz nas
3. Dodajemy do gracza komponent `Area2D` który będzie sprawdzać detekcję czy gracz wszedł w tile który jest drabiną
   - Ustawiamy `Area2D` w inspektorze `Collision` -> `Layer` na zero czyli odznaczamy wszystkie zaznaczone warstwy
   - Ustawiamy `Area2D` w inspektorze `Collision` -> `Mask` na warstwę tą która jest wykorzystywana przez `TileMap` o nazwie `Ladder`
4. Po dodaniu `Area2D` musimy z nią połączyć sygnał robimy to poprzez wybranie po prawej stronie w inspektorze zakładki `Węzeł` i wybranie dwóch sygnałów:
   - `body_entered`
   - `body_exited`
Jeżeli `body_entered` zostanie zarejestrowane to zmieniamy zmienną w skrypcie gracza `on_ladder` na `true`
Jeżeli `body_exited` zostanie zarejestrowane to zmieniamy zmienną w skrypcie gracza `on_ladder` na `false`

Gdy zmienna `on_ladder` jest równa true wyłączamy działanie grawitacji na gracza i możemy uruchomić funkcję odpowiedzialną za poruszanie się po drabinie czyli tak naprawdę po kliknięciu strzałki w górę postać powinna się poruszać w górę i na odwrót jak zostanie kliknięta strzałka w dół.