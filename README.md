# RoboHead CLI

Командная утилита для управления роботом RoboHead на базе ROS (Robot Operating System).

## Описание

RoboHead CLI предоставляет удобный интерфейс для:

- Управления системными сервисами робота (старт, стоп, рестарт)
- Настройки аудио и громкости
- Управления словарём голосового распознавания
- Создания и управления действиями (actions) робота
- Выключения и перезагрузки системы

## Структура проекта

```
robohead-cli/
├── source/
│   └── robohead          # Основной Python-скрипт CLI
└── install.sh            # Скрипт автоматической установки
```

## Установка

### Автоматическая установка

```bash
cd robohead-cli
sudo ./install.sh
```

### Ручная установка

```bash
sudo mkdir -p /opt/robohead-cli
sudo cp source/robohead /opt/robohead-cli/robohead
sudo chmod +x /opt/robohead-cli/robohead
sudo ln -s /opt/robohead-cli/robohead /usr/local/bin/robohead
```

## Использование

После установки команда `robohead` доступна из любой директории:

```bash
robohead [модуль] [команда] [опции]
```

### Основные модули

#### core — Управление системой и сервисами

```bash
# Запуск сервиса
robohead core start

# Остановка сервиса
robohead core stop

# Перезапуск сервиса
robohead core restart

# Перезапуск в debug-режиме
robohead core restart --debug

# Выключение системы
robohead core shutdown
robohead core shutdown --no-sync

# Перезагрузка системы
robohead core reboot
robohead core reboot --no-sync
```

#### config — Настройки системы

**Динамики и аудио:**

```bash
# Установить громкость (0-100)
robohead config speakers set_volume 75

# Установить громкость для текущего сеанса
robohead config speakers set_volume 80 --now

# Установить стандартную громкость (50%)
robohead config speakers set_volume --default

# Сохранить громкость для автозапуска
robohead config speakers set_volume 60 --standart
```

**Голосовое распознавание:**

```bash
# Добавить активационное слово
robohead config words set_activation_word привет
robohead config words set_activation_word робот --act_volume 40

# Пересобрать фонетический словарь
robohead config words --rebuild

# Загрузить словарь из файла
robohead config words write_from_file /path/to/dictionary.txt
```

**Аудио файлы:**

```bash
# Заменить аудио файл в модуле
robohead config voice set_audio_to_module \
  --from-file ./new_sound.mp3 \
  --to-module robohead_controller_actions.greeting.action
```

#### action — Управление действиями

```bash
# Список всех действий
robohead action list

# Создать новое действие
robohead action create smile улыбнись
robohead action create dance танцуй --update-dictionary

# Создать действие без слова активации
robohead action create custom_action --no-word

# Получить информацию о действии
robohead action info smile
robohead action info smile --path
robohead action info smile --data
robohead action info smile --in-file

# Удалить действие
robohead action remove smile
robohead action remove smile --dont-change-dictionary
```

## Примеры использования

### Создание нового действия

```bash
# 1. Создайте действие
robohead action create greeting привет --update-dictionary

# 2. Отредактируйте файл действия
nano ~/robohead_ws/src/robohead/robohead_controller/scripts/robohead_controller_actions/greeting/action.py

# 3. Пересоберите словарь
robohead config words --rebuild

# 4. Перезапустите сервис
robohead core restart
```

### Настройка громкости

```bash
# Установить громкость 75% сейчас и сохранить для автозапуска
robohead config speakers set_volume 75 --now --standart
```

### Debug режим

```bash
# Запуск в debug режиме для отладки
robohead core restart --debug
```

## Справка

Для получения помощи по конкретной команде используйте флаг `-h` или `--help`:

```bash
robohead --help
robohead core --help
robohead config speakers set_volume --help
robohead action create --help
```
