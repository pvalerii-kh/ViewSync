# Выкладка релиза ViewSync (public repo)

Репозиторий: https://github.com/pvalerii-kh/ViewSync

Агент и Setup читают манифест:
`https://raw.githubusercontent.com/pvalerii-kh/ViewSync/main/latest.json`

## Ссылка для пользователей (вечная)

Онлайн-установщик (не менять при каждом релизе версии):

`https://github.com/pvalerii-kh/ViewSync/releases/download/installer/ViewSyncSetup.exe`

Кнопка в боте → эта URL. Setup сам тянет актуальный zip из `latest.json`.

Пересобирать Setup только при багах установщика / смене путей.

Выкладка Setup:

```powershell
pyinstaller --noconfirm setup.spec
gh release upload installer .\dist\ViewSyncSetup.exe --repo pvalerii-kh/ViewSync --clobber
```

(если release `installer` ещё нет — создать один раз с `--notes "Installer channel"`)

## Структура zip (версии)

`ViewSync-<version>.zip` в корне (или в одном подкаталоге):

- `ViewSync.exe`
- `ViewSyncUpdater.exe`

Папка установки у пользователя: `%LOCALAPPDATA%\ViewSync\`

## Шаги релиза версии

1. Собрать агента: `pyinstaller build.spec`
2. Собрать updater: `pyinstaller updater.spec`
3. Упаковать:
   ```powershell
   .\scripts\pack_release.ps1 -Version 0.1.3a
   ```
4. GitHub Release tag `v0.1.3a` + zip.
5. Обновить `latest.json` в `main` (version, download_url, **sha256**, notes).

`sha256` обязателен. Пустой → автоустановка не пройдёт.

## Mandatory

- `"mandatory": true` — нельзя «Пізніше», мониторинг на паузе.
- или `APP_VERSION < min_supported_version` — тоже обязательное.
