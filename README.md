# TgProxyGuard — зашифрованная конфигурация сервера

Публичный репозиторий содержит **только** зашифрованные учётные данные SSH/прокси. Ключ расшифровки встроен в APK (обфусцирован) и **не** публикуется здесь.

## Файлы

| Файл | Описание |
|------|----------|
| `config/server-config.enc` | AES-256-GCM, base64 (nonce + ciphertext + tag) |

## Формат plaintext (до шифрования)

```json
{"host":"...","user":"...","password":"...","port":443,"secret":"..."}
```

## Ротация пароля / секрета

1. Смените пароль на сервере (`passwd`) или обновите секрет прокси.
2. На машине разработчика:
   ```powershell
   cd C:\CompanyCall\User\TgProxyGuardAndroid
   python tools\encrypt_server_config.py --password "НОВЫЙ_ПАРОЛЬ"
   ```
3. Скрипт перезапишет `tgproxyguard-config/config/server-config.enc` и выведет новый ключ (если генерировался заново).
4. Обновите `config.decrypt.key.hex` в `local.properties` (если ключ новый).
5. Пересоберите APK и опубликуйте обновлённый `.enc`:
   ```powershell
   .\tools\push_config_repo.ps1
   ```

> **Важно:** это обфускация, а не военный уровень защиты. Любой, кто декомпилирует APK, может извлечь ключ. Не храните в репозитории открытый пароль.

## Raw URL для приложения

```
https://raw.githubusercontent.com/upl74/tgproxyguard/main/config/server-config.enc
```
