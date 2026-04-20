# Git Синхронизация

## Описание

Функционал Git синхронизации позволяет автоматически сохранять и восстанавливать конфигурацию SingBox и базу данных в Git репозиториях (GitHub, GitLab, Gitea).

## Поддерживаемые провайдеры

- **GitHub** - https://github.com
- **GitLab** - https://gitlab.com или self-hosted
- **Gitea** - self-hosted

## Настройка

### 1. Создание токена доступа

#### GitHub
1. Перейдите в Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Создайте новый токен с правами `repo` (Full control of private repositories)
3. Скопируйте токен

#### GitLab
1. Перейдите в Settings → Access Tokens
2. Создайте токен с правами `api`, `read_repository`, `write_repository`
3. Скопируйте токен

#### Gitea
1. Перейдите в Settings → Applications → Generate New Token
2. Выберите права `repo` (Full control of repositories)
3. Скопируйте токен

### 2. Создание репозитория

Создайте новый приватный репозиторий для хранения конфигураций и бэкапов.

### 3. Настройка в S-UI

#### API Endpoints

**Получить конфигурацию:**
```
GET /app/api/gitSyncConfig
```

**Сохранить конфигурацию:**
```
POST /app/api/gitSyncConfig
Content-Type: application/json

{
  "enable": true,
  "provider": "github",
  "repoUrl": "https://github.com/username/repo",
  "branch": "main",
  "token": "your_token_here",
  "autoSync": true,
  "syncInterval": 3600,
  "syncConfig": true,
  "syncDb": true
}
```

**Параметры:**
- `enable` - включить/выключить синхронизацию
- `provider` - провайдер: `github`, `gitlab`, `gitea`
- `repoUrl` - URL репозитория
- `branch` - ветка (по умолчанию `main`)
- `token` - токен доступа
- `autoSync` - автоматическая синхронизация
- `syncInterval` - интервал синхронизации в секундах (по умолчанию 3600 = 1 час)
- `syncConfig` - синхронизировать конфигурацию SingBox
- `syncDb` - синхронизировать базу данных

**Отправить данные в Git:**
```
POST /app/api/gitSyncPush
```

**Получить данные из Git:**
```
POST /app/api/gitSyncPull
```

**Проверить подключение:**
```
POST /app/api/gitSyncTest
```

## Использование

### Ручная синхронизация

1. Настройте параметры Git синхронизации через API
2. Вызовите `/app/api/gitSyncPush` для отправки данных в Git
3. Вызовите `/app/api/gitSyncPull` для получения данных из Git

### Автоматическая синхронизация

При включении `autoSync` система будет автоматически отправлять изменения в Git с интервалом, указанным в `syncInterval`.

## Файлы в репозитории

После синхронизации в репозитории появятся следующие файлы:

- `singbox-config.json` - конфигурация SingBox в "сыром" виде
- `s-ui-backup.db` - резервная копия базы данных (без статистики и истории изменений)

## Восстановление

### Восстановление конфигурации SingBox

Конфигурация автоматически применяется при pull из Git.

### Восстановление базы данных

1. Получите файл `s-ui-backup.db` из репозитория
2. Используйте существующий API endpoint для импорта:
```
POST /app/api/importdb
Content-Type: multipart/form-data

db: <файл базы данных>
```

## Безопасность

⚠️ **Важно:**
- Используйте **приватные** репозитории
- Храните токены в безопасности
- Не публикуйте токены в открытом доступе
- Регулярно обновляйте токены доступа
- Используйте токены с минимально необходимыми правами

## Примеры URL репозиториев

**GitHub:**
```
https://github.com/username/repo
https://github.com/username/repo.git
```

**GitLab:**
```
https://gitlab.com/username/repo
https://gitlab.example.com/username/repo
```

**Gitea:**
```
https://gitea.example.com/username/repo
```

## Устранение неполадок

### Ошибка аутентификации
- Проверьте правильность токена
- Убедитесь, что токен имеет необходимые права
- Проверьте срок действия токена

### Ошибка доступа к репозиторию
- Убедитесь, что репозиторий существует
- Проверьте правильность URL
- Убедитесь, что у токена есть доступ к репозиторию

### Файлы не появляются в репозитории
- Проверьте правильность ветки
- Убедитесь, что синхронизация включена
- Проверьте логи приложения

## Логирование

Все операции синхронизации логируются. Проверьте логи для диагностики проблем:
```
GET /app/api/logs?c=100&l=debug
```
"