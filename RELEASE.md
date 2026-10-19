# Викладка релізу ViewSync (public repo)

Репозиторій: https://github.com/pvalerii-kh/ViewSync

Агент читає маніфест:
`https://raw.githubusercontent.com/pvalerii-kh/ViewSync/main/latest.json`

## Структура zip

`ViewSync-<version>.zip` повинен містити в корені (або в одному підкаталозі):

- `ViewSync.exe`
- `ViewSyncUpdater.exe`

## Кроки

1. Зібрати агента: `pyinstaller build.spec`
2. Зібрати updater: `pyinstaller updater.spec`
3. Упакувати:
   ```powershell
   .\scripts\pack_release.ps1 -Version 0.1.3a
   ```
4. Створити GitHub Release з tag `v0.1.3a` і прикріпити zip.
5. Оновити `latest.json` у гілці `main`:

```json
{
  "version": "0.1.3a",
  "min_supported_version": "0.1.0a",
  "mandatory": false,
  "download_url": "https://github.com/pvalerii-kh/ViewSync/releases/download/v0.1.3a/ViewSync-0.1.3a.zip",
  "sha256": "<з pack_release.ps1>",
  "release_notes_ua": "…"
}
```

`sha256` обовʼязковий для автоустановки (B10b). Порожній хеш → агент відкриє сторінку завантаження вручну.

## Mandatory

- `"mandatory": true` — користувач не може «Пізніше», моніторинг на паузі.
- або `APP_VERSION < min_supported_version` — теж вважається обовʼязковим.
