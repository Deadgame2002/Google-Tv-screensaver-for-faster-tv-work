# Photo Dream – slideshow screensaver with clock for Android TV / Google TV
# Photo Dream – заставка со слайд-шоу и часами для Android TV / Google TV

**English** | [Русский](#русский)

---

## English

A tiny screensaver (Android "Dream") for Android TV / Google TV.

- Shows your own photos as a **slideshow** with a smooth fade (1 picture = static, 2+ = slideshow)
- Photos fill the whole screen (center-crop)
- **Clock + date** in the bottom corner (left or right). Every minute it fades out and comes back slightly moved, so it never stays in one spot
- **No permissions, no internet**, no ads, no launcher changes
- Pictures are loaded from a folder on the TV, so you can change them any time with ADB

> Set up and used on a **Xiaomi TV A Pro 2025**.
> It should work on other Android TV / Google TV devices (Android 5.0+), but I only tested it on that one TV. TVs without screensaver (Dream) support, and Fire TV, may not work.

### What you need

- Android TV / Google TV with **ADB debugging** turned on
  (Settings → Device Preferences → About → tap *Build* 7 times → Developer options → enable USB / network debugging)
- A PC with [ADB (platform-tools)](https://developer.android.com/tools/releases/platform-tools)
- The file `PhotoDream-slideshow.apk` from this repository
- Your images (jpg, jpeg, png or webp), ideally 1920x1080 or 3840x2160, dark and without bright white areas

On Windows PowerShell write `.\adb` instead of `adb` (and run commands from the platform-tools folder).

### Install

1. Connect to the TV (use your TV's IP address):
   ```
   adb connect 192.168.1.50:5555
   adb devices
   ```
   You should see `device`. If it says `unauthorized`, accept the request on the TV screen.

2. Install:
   ```
   adb install PhotoDream-slideshow.apk
   ```
   To update later: `adb install -r PhotoDream-slideshow.apk`

3. Set it as the screensaver:
   ```
   adb shell settings put secure screensaver_components local.photodream/local.photodream.PhotoDream
   adb shell settings put secure screensaver_enabled 1
   ```
   Or choose *Photo Dream* manually in the TV settings (usually Device Preferences → Screen saver).

### Google's built-in screensaver

You do **not** need to delete or disable it. Choosing *Photo Dream* as the screensaver (step 3, or in TV settings → Screen saver) replaces it automatically. To go back to Google's one, pick it again in the same settings menu.
Please do not remove or disable Google / system apps with ADB – it can break the TV.

### Where to find pictures

- Free photo sites: [Unsplash](https://unsplash.com), [Pexels](https://www.pexels.com), [Pixabay](https://pixabay.com) (check the license of each picture)
- Search words that work well for a night screensaver: `dark landscape`, `night sky`, `milky way`, `dark minimal`, `city night`, `northern lights`, `dark abstract`
- Pick **landscape (horizontal)** pictures, download the 1920x1080 or 3840x2160 size (or larger) and avoid pictures with big bright white areas
- You can also use your own photos

### Add your pictures

Create the folder and push the images:
```
adb shell mkdir -p /sdcard/Android/data/local.photodream/files
adb push image1.jpg /sdcard/Android/data/local.photodream/files/
adb push image2.jpg /sdcard/Android/data/local.photodream/files/
```
Push many files at once (Windows PowerShell):
```
Get-ChildItem image*.jpg | ForEach-Object { .\adb push $_.FullName /sdcard/Android/data/local.photodream/files/ }
```
Check:
```
adb shell ls -l /sdcard/Android/data/local.photodream/files/
```
Images are shown in alphabetical order of file names (name them `01.jpg`, `02.jpg` ... for a fixed order). New files are picked up automatically.

### Test it right away

```
adb shell am start -n com.android.systemui/.Somnambulator
```
If this does not start it on your TV, just wait for the screensaver to start by itself.

### Settings (optional)

Small text files in the same folder:

| File | Content | Meaning |
|------|---------|---------|
| `interval.txt` | a number of seconds (5–86400) | time between pictures, default 60 |
| `clock.txt` | `right`, `left` or `off` | clock position, default `right` |

Example:
```
echo left > clock.txt
adb push clock.txt /sdcard/Android/data/local.photodream/files/
```

### Troubleshooting

- **Only the built-in picture is shown** – check the folder with `ls -l` (see above) and the file extensions.
- **`adb push` is denied** – start the screensaver once (test command above) so the app creates its folder, then push again.
- **Wrong time** – fix the time zone in the TV settings.
- **Remove everything** – `adb uninstall local.photodream`

### Tips

- A one-picture screensaver all night is not ideal – use 5+ dark pictures and a TV sleep timer.
- **Do not disable or uninstall system apps / launchers with `pm disable-user` or `pm uninstall`** to "make room" for custom apps. It can make the TV freeze or look dead. If you did, re-enable them: `adb shell pm list packages -d` shows disabled packages and `adb shell pm enable <package>` brings them back.

### Source

`PhotoDream.java` is the whole app code. The APK is signed with my own key (not Google Play), so if you build it yourself you must uninstall mine before installing yours.
Made with the help of Claude (Anthropic).

---

## Русский

Маленькая заставка (Android "Dream") для Android TV / Google TV.

- Показывает ваши фото в виде **слайд-шоу** с плавной сменой (1 картинка = статичная, 2 и больше = слайд-шоу)
- Картинки заполняют весь экран (с обрезкой по краям)
- **Часы и дата** в нижнем углу (слева или справа). Раз в минуту они плавно гаснут и появляются чуть в другом месте, поэтому не стоят на одном месте
- **Без разрешений и без интернета**, без рекламы, лаунчер не трогает
- Картинки берутся из папки на ТВ, их можно менять в любое время через ADB

> Настроено и используется на **Xiaomi TV A Pro 2025**.
> Должно работать и на других Android TV / Google TV (Android 5.0+), но я проверял только на этом ТВ. На ТВ без поддержки заставок (Dream) и на Fire TV может не работать.

### Что нужно

- Android TV / Google TV с включённой **отладкой ADB**
  (Настройки → Настройки устройства → О системе → 7 раз нажать на *Сборка* → Для разработчиков → включить отладку по USB / сети)
- ПК с [ADB (platform-tools)](https://developer.android.com/tools/releases/platform-tools)
- Файл `PhotoDream-slideshow.apk` из этого репозитория
- Ваши картинки (jpg, jpeg, png или webp), лучше 1920x1080 или 3840x2160, тёмные и без ярких белых участков

В Windows PowerShell пишите `.\adb` вместо `adb` (и запускайте команды из папки platform-tools).

### Установка

1. Подключитесь к ТВ (подставьте IP вашего ТВ):
   ```
   adb connect 192.168.1.50:5555
   adb devices
   ```
   Должно быть `device`. Если `unauthorized`, подтвердите запрос на экране ТВ.

2. Установите:
   ```
   adb install PhotoDream-slideshow.apk
   ```
   Обновить позже: `adb install -r PhotoDream-slideshow.apk`

3. Сделайте её заставкой:
   ```
   adb shell settings put secure screensaver_components local.photodream/local.photodream.PhotoDream
   adb shell settings put secure screensaver_enabled 1
   ```
   Или выберите *Photo Dream* вручную в настройках ТВ (обычно Настройки устройства → Заставка).

### Встроенная заставка Google

Её **не нужно** удалять или отключать. Когда вы выбираете *Photo Dream* заставкой (шаг 3 или в настройках ТВ → Заставка), она автоматически заменяет заставку Google. Чтобы вернуть заставку Google, просто выберите её снова в том же меню.
Пожалуйста, не удаляйте и не отключайте приложения Google и системные приложения через ADB: из-за этого ТВ может сломаться.

### Где взять картинки

- Бесплатные сайты с фото: [Unsplash](https://unsplash.com), [Pexels](https://www.pexels.com), [Pixabay](https://pixabay.com) (проверяйте лицензию каждой картинки)
- Что искать для ночной заставки: `dark landscape`, `night sky`, `milky way`, `dark minimal`, `city night`, `northern lights`, `dark abstract`
- Выбирайте **горизонтальные** картинки, скачивайте размер 1920x1080 или 3840x2160 (или больше) и избегайте картинок с большими яркими белыми участками
- Можно использовать и свои фото

### Добавить картинки

Создайте папку и отправьте картинки:
```
adb shell mkdir -p /sdcard/Android/data/local.photodream/files
adb push image1.jpg /sdcard/Android/data/local.photodream/files/
adb push image2.jpg /sdcard/Android/data/local.photodream/files/
```
Отправить много файлов сразу (Windows PowerShell):
```
Get-ChildItem image*.jpg | ForEach-Object { .\adb push $_.FullName /sdcard/Android/data/local.photodream/files/ }
```
Проверить:
```
adb shell ls -l /sdcard/Android/data/local.photodream/files/
```
Картинки показываются по алфавиту имён файлов (назовите их `01.jpg`, `02.jpg` ... для нужного порядка). Новые файлы подхватываются автоматически.

### Проверить сразу

```
adb shell am start -n com.android.systemui/.Somnambulator
```
Если на вашем ТВ это не запустит заставку, просто подождите, пока она включится сама.

### Настройки (по желанию)

Маленькие текстовые файлы в той же папке:

| Файл | Содержимое | Что делает |
|------|------------|------------|
| `interval.txt` | число секунд (5–86400) | время между картинками, по умолчанию 60 |
| `clock.txt` | `right`, `left` или `off` | положение часов, по умолчанию `right` |

Пример:
```
echo left > clock.txt
adb push clock.txt /sdcard/Android/data/local.photodream/files/
```

### Если что-то не работает

- **Показывается только встроенная картинка** – проверьте папку командой `ls -l` (см. выше) и расширения файлов.
- **`adb push` не разрешён** – один раз запустите заставку (команда проверки выше), чтобы приложение создало свою папку, и отправьте файлы снова.
- **Неверное время** – поправьте часовой пояс в настройках ТВ.
- **Удалить всё** – `adb uninstall local.photodream`

### Советы

- Одна картинка на всю ночь не идеальна: используйте 5+ тёмных картинок и таймер сна на ТВ.
- **Не отключайте и не удаляйте системные приложения и лаунчеры через `pm disable-user` или `pm uninstall`**, чтобы "освободить место" для сторонних приложений. Из-за этого ТВ может зависать или выглядеть "мёртвым". Если вы уже так сделали, верните их: `adb shell pm list packages -d` покажет отключённые пакеты, а `adb shell pm enable <пакет>` включит их обратно.

### Исходный код

`PhotoDream.java` – весь код приложения. APK подписан моим собственным ключом (не из Google Play), поэтому если вы соберёте своё, сначала удалите моё приложение, а потом ставьте своё.
Сделано с помощью Claude (Anthropic).
