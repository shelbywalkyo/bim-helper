# bim-helper — канал обновлений

Служебный репозиторий. Здесь **нет исходного кода** — только то, что нужно
плагину bim-helper для автообновления:

- `version.json` — последняя доступная версия и ссылка на пакет;
- релизы (вкладка **Releases**) — сами пакеты `update.zip`.

Исходный код плагина — в приватном репозитории `bim-helper`.

## Как это работает

Плагин внутри Revit раз в час запрашивает `version.json` по прямой ссылке:

```
https://raw.githubusercontent.com/shelbywalkyo/bim-helper-updates/main/version.json
```

Если поле `Version` больше установленной версии, плагин качает `DownloadUrl`
(ассет релиза — публичная ссылка, токен не нужен), распаковывает и при
закрытии Revit подменяет файлы.

## Формат version.json

```json
{
  "Version": "1.1.25.0",
  "DownloadUrl": "https://github.com/shelbywalkyo/bim-helper-updates/releases/download/v1.1.25.0/update.zip",
  "ReleaseNotes": "Короткое описание, показывается пользователю в уведомлении"
}
```

`Version` — четыре числа, как у сборок (сравнивается как `System.Version`).

## Формат update.zip

```
update.zip
├── net48/             содержимое bin\Release\net48           (Revit 2022–2024)
├── net8.0-windows/    содержимое bin\Release\net8.0-windows  (Revit 2025–2026)
└── bim-helper.addin   манифест (один на все версии)
```

## Как выпустить обновление

Из приватного репозитория плагина:

```
Installer\publish-update.ps1 -Notes "Что нового в этой версии"
```

Скрипт собирает обе ветки в Release, упаковывает `update.zip`, создаёт релиз
в этом репозитории и обновляет `version.json`. Номер версии берётся из
`SharedAssemblyVersion.cs`, вручную его здесь править не нужно.
