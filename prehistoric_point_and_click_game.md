**Projet de jeu préhistorique point-and-click - PDF complet**

---

# 1. Sprite Sheet Personnage

**Style**: Préhistorique, vue isométrique, cartoon
**Taille**: 64x64 px
**Couleurs**: neutres, peau et vêtements naturels

**Animations**:
- Idle: 4 frames
- Marche: 6 frames
- Attaque: 4 frames (optionnel)
- Saut: 2 frames (optionnel)

**Exemple de code Pygame pour animer le sprite**:
```python
import pygame, sys
pygame.init()
screen = pygame.display.set_mode((400,400))
clock = pygame.time.Clock()
sprite_sheet = pygame.image.load("assets/sprites/prehistoric.png").convert_alpha()
sprite_width, sprite_height = 64, 64
animations = {
    "idle": [sprite_sheet.subsurface(pygame.Rect(i*sprite_width, 0, sprite_width, sprite_height)) for i in range(4)],
    "walk": [sprite_sheet.subsurface(pygame.Rect(i*sprite_width, 64, sprite_width, sprite_height)) for i in range(6)]
}
current_animation = "idle"
frame_index = 0
animation_speed = 0.2
x, y = 200, 200
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
    frame_index += animation_speed
    if frame_index >= len(animations[current_animation]): frame_index = 0
    current_frame = animations[current_animation][int(frame_index)]
    screen.fill((50,50,50))
    screen.blit(current_frame,(x,y))
    pygame.display.flip()
    clock.tick(60)
```

---

# 2. Point-and-Click Movement

**Exemple de code Pygame**:
```python
import pygame, sys, math
pygame.init()
screen = pygame.display.set_mode((640,480))
clock = pygame.time.Clock()
sprite_sheet = pygame.image.load("assets/sprites/prehistoric.png").convert_alpha()
sprite_width, sprite_height = 64,64
animations = {
    "idle": [sprite_sheet.subsurface(pygame.Rect(i*sprite_width,0,sprite_width,sprite_height)) for i in range(4)],
    "walk": [sprite_sheet.subsurface(pygame.Rect(i*sprite_width,64,sprite_width,sprite_height)) for i in range(6)]
}
current_animation="idle"
frame_index=0
animation_speed=0.2
x,y=320,240
speed=2
destination=(x,y)

def move_towards(pos,dest,speed):
    dx = dest[0]-pos[0]
    dy = dest[1]-pos[1]
    distance = math.hypot(dx,dy)
    if distance<speed: return dest
    return (pos[0]+dx/distance*speed, pos[1]+dy/distance*speed)

while True:
    for event in pygame.event.get():
        if event.type==pygame.QUIT: pygame.quit(); sys.exit()
        elif event.type==pygame.MOUSEBUTTONDOWN and event.button==1: destination=event.pos
    if (x,y)!=destination:
        x,y=move_towards((x,y),destination,speed)
        current_animation="walk"
    else: current_animation="idle"
    frame_index+=animation_speed
    if frame_index>=len(animations[current_animation]): frame_index=0
    current_frame=animations[current_animation][int(frame_index)]
    screen.fill((100,100,100))
    screen.blit(current_frame,(x,y))
    pygame.display.flip()
    clock.tick(60)
```

---

# 3. Bibliothèque sonore

**Organisation des fichiers**:
```txt
sounds/
├─ effects/
│  ├─ walk.wav
│  ├─ click.wav
│  └─ interact.wav
├─ ui/
│  ├─ button_hover.wav
│  └─ button_click.wav
└─ music/
   └─ ambient.ogg
```

**Exemple Pygame**:
```python
walk_sound = pygame.mixer.Sound("sounds/effects/walk.wav")
click_sound = pygame.mixer.Sound("sounds/effects/click.wav")
pygame.mixer.music.load("sounds/music/ambient.ogg")
pygame.mixer.music.play(-1)
walk_sound.play()
click_sound.play()
```

---

# 4. Environnement de développement / Dépôt Git

**Arborescence**:
```txt
prehistoric_game/
├─ assets/
│  ├─ tiles/
│  ├─ sprites/
│  └─ sounds/
├─ main.py
├─ requirements.txt
└─ README.md
```

**requirements.txt**:
```txt
pygame>=2.1.0
```

**README.md**: Instructions d'installation, création d'environnement virtuel, lancement via `python main.py`, et arborescence.

---

# 5. Design de l'environnement (Style préhistorique, isométrique, 64x64 px, couleurs neutres)

**Palette préhistorique neutre**:
- Terrains : herbe #556B2F, terre #8B4513, pierre #A9A9A9
- Décors : arbres tronc #654321, feuillage #2E8B57; buissons #3B5323; rochers #7F7F7F
- Objets interactifs : os #F5F5DC, feu #D2691E, fruits #C1440E
- Arrière-plan : ciel neutre #B0C4DE

**Dimensions standard**:
- Tuiles sol : 64x64 px
- Décor : 32x64 px
- Objets interactifs : 32x32 px
- Arrière-plan : 640x480 px

**Éléments à produire**:
- Sols : grass.png, grass_var1.png, dirt.png, stone.png
- Décors : tree.png, bush.png, rock.png
- Objets interactifs : fire.png, bone.png, fruit.png
- Arrière-plan : bg_forest.png

**Organisation des assets**:
```txt
assets/
├─ tiles/
├─ decor/
├─ objects/
└─ backgrounds/
```

**Exemple de code Pygame pour afficher l'environnement isométrique**:
```python
import pygame, random
pygame.init()
screen = pygame.display.set_mode((640,480))
clock = pygame.time.Clock()

# Charger tuiles
tiles = [pygame.image.load(f"assets/tiles/{name}.png").convert_alpha() for name in ["grass","grass_var1","dirt","stone"]]
# Charger décorations et objets
tree = pygame.image.load("assets/decor/tree.png").convert_alpha()
rock = pygame.image.load("assets/decor/rock.png").convert_alpha()
fire = pygame.image.load("assets/objects/fire.png").convert_alpha()

tile_size = 64
grid_width = 10
grid_height = 8

running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    for y in range(grid_height):
        for x in range(grid_width):
            tile = random.choice(tiles)
            iso_x = (x - y) * tile_size // 2 + 320
            iso_y = (x + y) * tile_size // 4 + 50
            screen.blit(tile, (iso_x, iso_y))

    screen.blit(tree,(200,150))
    screen.blit(rock,(300,250))
    screen.blit(fire,(400,200))

    pygame.display.flip()
    clock.tick(30)

pygame.quit()
```

---

# Fin du PDF

