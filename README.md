# HW7 – Користувачі, Моніторинг Диску та Monit для Nginx

## Завдання 1: Керування користувачами та групами

1. **Створення користувачів:**
   - Створено користувачів `dev1`, `dev2`, `dev3`, `backupdev`.

2. **Створення груп:**
   - Створено групи `розробники` та `веб-майстри`.

3. **Додавання користувачів до груп:**
   - `dev1` та `dev2` додані до групи `розробники`.
   - `dev3` доданий до групи `веб-майстри`.

4. **Встановлення групи за замовчуванням:**
   - Для `dev1`, `dev2`, `dev3` встановлено групу за замовчуванням `розробники` (демонстраційно).

5. **Створення домашніх каталогів:**
   - Створено каталоги `/home/dev1`, `/home/dev2`, `/home/dev3`, `/home/backupdev`.

6. **Клонування каталогу:**
   - Клоновано `/home/dev1` у `/home/backupdev`.

7. **Каталог `web_project`:**
   - Створено `/home/web_project` з правами групового запису (chmod 2775).

8. **Файл журналу `my.log`:**
   - Створено `/home/my.log`, з дозволами тільки на додавання (chattr +a).

---

## Завдання 2: Моніторинг використання диску

- Створено скрипт `/usr/local/bin/disk_check.sh`, який перевіряє використання кореневого розділу `/`.
- Якщо використання >75%, записується попередження у `/var/log/disk.log`.
- Налаштовано запуск скрипта кожні 5 хвилин через crontab.

---

## Завдання 3: Конфігурація Monit для моніторингу Nginx

1. Встановлено та запущено `nginx`.
2. Встановлено та налаштовано `monit`.
3. Створено конфігурацію `/etc/monit/conf-available/nginx`:

```text
check process nginx with pidfile /run/nginx.pid
  start program = "/bin/systemctl start nginx"
  stop program  = "/bin/systemctl stop nginx"
  if failed port 80 protocol http then restart
  if 7 restarts within 7 cycles then timeout
```

4. Активовано веб-інтерфейс Monit на порту 2812:

```text
set httpd port 2812 and
    use address localhost
    allow localhost
```

5. Перезапущено Monit та активовано моніторинг nginx:
   - `sudo monit reload`
   - `sudo monit start nginx`

6. Перевірка:

```bash
sudo monit summary
```

✅ Статус nginx: OK, моніторинг працює!

---

# Скриншоти для підтвердження (зробити окрему папку screenshots/):

- Користувачі (`grep dev /etc/passwd`)
- Групи (`getent group розробники`, `getent group веб-майстри`)
- Права на домашні каталоги (`ls -l /home`)
- Атрибути файлу `my.log` (`lsattr /home/my.log`)
- Вміст скрипта `disk_check.sh`
- Crontab (`sudo crontab -l`)
- Логи `/var/log/disk.log` (`tail /var/log/disk.log`)
- Конфіг monit для nginx (`cat /etc/monit/conf-enabled/nginx`)
- Статус Monit (`sudo monit summary`)

---

# Висновок

Всі частини домашнього завдання виконані успішно:
- Користувачі та групи налаштовані.
- Скрипт моніторингу диску працює через crontab.
- Monit контролює процес nginx і реагує на помилки.

