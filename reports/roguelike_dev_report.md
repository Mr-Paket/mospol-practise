# Отчёт о разработке Rogue-Like игры на Unity с использованием RogueSharp

## Введение

Данный отчёт описывает процесс разработки прототипа Rogue-Like игры в фэнтезийном сеттинге. Основной целью проекта было создание игры с процедурной генерацией подземелий, сбором ресурсов, боем с монстрами и исследованием многоэтажных локаций. Ключевой особенностью реализации стал режим реального времени для передвижения игрока и противников. В проекте использовались:

- **Unity** (2022.3.x) как игровой движок

- **RogueSharp** (v5.0.0) для процедурной генерации карт

- **C#** для реализации игровой логики

## Архитектура проекта

### Генерация игрового мира

Основу генерации составлял скрипт `MapGenerator.cs`:

```csharp

public class MapGenerator : MonoBehaviour

{

private DungeonMap dungeonMap;

public void GenerateMap()

{

dungeonMap = new DungeonMap(width, height);

// Логика генерации карты с использованием RogueSharp

VisualizeMap();

}

private void VisualizeMap()

{

for (int x = 0; x < width; x++)

{

for (int y = 0; y < height; y++)

{

Cell cell = dungeonMap.GetCell(x, y);

Instantiate(GetTilePrefab(cell), new Vector3(x, y, 0), Quaternion.identity);

}

}

}

}

```

### Система спавна объектов

**Механика спавна сундуков:**

```csharp

int chestCount = Random.Range(0, 4);

for (int i = 0; i < chestCount; i++)

{

Room randomRoom = dungeonMap.Rooms.GetRandom();

Point spawnPoint = dungeonMap.GetRandomWalkableLocationInRoom(randomRoom);

if (!IsOverlappingWithHatch(spawnPoint))

{

Instantiate(chestPrefab, new Vector3(spawnPoint.X, spawnPoint.Y, 0), Quaternion.identity);

}

}

```

**Механика спавна люков:**

```csharp

Room hatchRoom = dungeonMap.Rooms.GetRandom();

Point hatchPoint = dungeonMap.GetRandomWalkableLocationInRoom(hatchRoom);

Instantiate(hatchPrefab, new Vector3(hatchPoint.X, hatchPoint.Y, 0), Quaternion.identity);

```

## Ключевые механики

### Режим реального времени

В отличие от классических Rogue-Like игр:

- Движение игрока и врагов реализовано через `Rigidbody2D` и `Transform.Translate`

- Коллизии обрабатываются системой физики Unity

- Время игры не привязано к ходам

### Система сундуков

- При взаимодействии с сундуком вызывается анимация открытия

- Реализация всплывающего текста:

```csharp

IEnumerator ShowGoldText(int amount)

{

GameObject textObj = Instantiate(goldTextPrefab, transform.position, Quaternion.identity);

TextMeshPro text = textObj.GetComponent<TextMeshPro>();

text.text = $"+{amount}G";

Animator anim = textObj.GetComponent<Animator>();

anim.Play("GoldTextPopup");

yield return new WaitForSeconds(1f);

Destroy(textObj);

}

```

### Система перехода между этажами

- При взаимодействии с люком:

1. Активируется экран загрузки

2. Сохраняется состояние игрока через `DontDestroyOnLoad`

3. Загружается новая сцена с новым сгенерированным уровнем

4. Восстанавливается состояние игрока

```csharp

public void LoadNextLevel()

{

StartCoroutine(LoadLevelAsync());

}

IEnumerator LoadLevelAsync()

{

loadingScreen.SetActive(true);

AsyncOperation asyncLoad = SceneManager.LoadSceneAsync(SceneManager.GetActiveScene().buildIndex);

while (!asyncLoad.isDone)

{

float progress = Mathf.Clamp01(asyncLoad.progress / 0.9f);

loadingBar.fillAmount = progress;

yield return null;

}

loadingScreen.SetActive(false);

}

```

## Технические особенности

### Сохранение состояния

При переходе между уровнями сохраняются:

- Здоровье игрока

- Собранное золото

- Уровень персонажа

- Инвентарь

```csharp

public class PlayerState : MonoBehaviour

{

public static PlayerState Instance;

public int Health;

public int Gold;

public int CurrentLevel = 1;

void Awake()

{

if (Instance == null)

{

Instance = this;

DontDestroyOnLoad(gameObject);

}

else

{

Destroy(gameObject);

}

}

}

```

### Оптимизация

- Пулиннг объектов (сундуки, враги, эффекты)

- Кэширование ссылок на компоненты

- Оптимизированные алгоритмы генерации карт

- Ограниченное количество одновременно активных NPC

## Заключение

В ходе разработки был создан полнофункциональный прототип Rogue-Like игры с уникальными особенностями:

1. Гибкая система процедурной генерации на базе RogueSharp

2. Динамичный геймплей в реальном времени

3. Система интерактивных объектов (сундуки, люки)

4. Многоуровневая структура подземелий

5. Система сохранения состояния между уровнями

Основные достижения:

- Успешная интеграция RogueSharp с Unity

- Создание эффективного конвейера генерации уровней

- Реализация плавных переходов между сценами

- Разработка визуально понятной системы обратной связи

Дальнейшие направления развития:

1. Добавление системы инвентаря

2. Реализация ветки навыков персонажа

3. Расширение вариативности процедурной генерации

4. Добавление разных типов врагов и боссов

5. Внедрение системы случайных событий

## Список литературы

1. RogueSharp Official Documentation. URL: https://roguesharp.wordpress.com/

2. Unity Manual: Scene Management. URL: https://docs.unity3d.com/Manual/SceneManagement.html

3. Procedural Generation in Game Design / T. Short, T. Adams. - CRC Press, 2017. - 320 p.

4. Game Development Patterns with Unity 2021 / A. Boduch. - Packt Publishing, 2021. - 420 p.

5. Unity in Action / J. Hocking. - Manning Publications, 2022. - 480 p.