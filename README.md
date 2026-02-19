# Cloudflare Admin Session Fix

Готовое решение для устранения проблемы «вылета» из админ-панели сайта (Joomla, OpenCart, самописные системы) после подключения Cloudflare.

## Проблема
При использовании Cloudflare IP-адрес пользователя постоянно меняется на IP серверов Cloudflare. Из-за этого система безопасности сайта обрывает сессию администратора, требуя повторного входа каждые несколько секунд.

## Решение (2 этапа)

### 1. Настройка Cloudflare (Page Rules)
Чтобы Cloudflare не вмешивался в работу админки, нужно создать правило:
* Перейдите в раздел **Rules** -> **Page Rules**.
* URL (пример): `example.com/administrator/*`
* Параметры:
  * **Cache Level**: `Bypass` (отключает кэширование сессий).
  * **Rocket Loader**: `Off` (предотвращает конфликты JS-скриптов).

### 2. Исправление в коде PHP
Добавьте этот блок кода в самый верх файла конфигурации сайта (напр. `configuration.php` или `cfg.system.php`) сразу после тега `<?php`:

```php
/** FIX CLOUDFLARE REAL IP **/
if (isset($_SERVER['HTTP_CF_CONNECTING_IP'])) {
    $_SERVER['REMOTE_ADDR'] = $_SERVER['HTTP_CF_CONNECTING_IP'];
}
/** END FIX **/
