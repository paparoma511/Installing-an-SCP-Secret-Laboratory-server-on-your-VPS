🧪 SCP: Secret Laboratory Dedicated Server + Pterodactyl

Краткий и понятный гайд по установке SCP: Secret Laboratory Dedicated Server на VPS с использованием Pterodactyl Panel + Wings.

«⚠️ Этот гайд протестирован на Debian 13.
На других операционных системах процесс установки может отличаться.»

---

1. Подготовка VPS

Подключитесь к VPS по SSH:

ssh root@ВАШ_IP

Обновите систему и установите необходимые утилиты:

apt update && apt upgrade -y
apt install -y curl wget sudo

---

2. Установка Pterodactyl

Запустите установщик Pterodactyl:

bash <(curl -s https://pterodactyl-installer.se)

Выберите:

2) Install both Panel and Wings

Во время установки укажите:

Пароль базы данных — ваш пароль
Часовой пояс — Europe/Moscow
Email — ваш email
Логин/пароль администратора — данные администратора
FQDN — ваш домен или IP-адрес
SSL — y (если используется домен)
Firewall — y

После завершения установки откройте панель:

https://ВАШ_ДОМЕН

---

3. Создание Node

В панели Pterodactyl откройте:

Admin Panel → Locations → Create New

Создайте Location, например:

Short Code: local
Description: Local server

Затем откройте:

Admin Panel → Nodes → Create New

Пример конфигурации:

Name: Node-01
Location: local
FQDN: ВАШ_ДОМЕН_ИЛИ_IP
Daemon Port: 8080
SFTP Port: 2022

Укажите значения RAM и Disk в соответствии с характеристиками вашего VPS.

После создания Node откройте вкладку Configuration и скопируйте конфигурацию.

Откройте конфигурационный файл Wings:

nano /etc/pterodactyl/config.yml

Вставьте конфигурацию и сохраните файл.

Перезапустите Wings:

systemctl restart wings
systemctl enable wings

Проверьте статус:

systemctl status wings

Должно отображаться:

Active: active (running)

---

4. Allocation

Откройте:

Admin Panel → Nodes → Node-01 → Allocations

Добавьте Allocation:

IP: 0.0.0.0
Port: 7777

Для дополнительных SCP:SL серверов можно использовать отдельные порты:

7778
7779
7780

---

5. SCP:SL Egg

Создайте новый Nest:

Admin Panel → Nests → Create New

Например:

Name: SCP:SL

Затем импортируйте подготовленный JSON Egg через:

Import Egg

«📦 Используйте свой подготовленный файл ".json".»

Убедитесь, что настройки Egg соответствуют используемой версии SCP: Secret Laboratory.

---

6. Создание сервера

Откройте:

Servers → Create New

Выберите:

Nest: SCP:SL
Egg: SCP:SL
Memory: 4096 MB
Disk: 10 GB+
Allocation: 7777

Создайте сервер и нажмите:

Start

Pterodactyl автоматически установит сервер через SteamCMD.

«⏳ Первая установка может занять некоторое время.»

---

7. LabAPI

После установки SCP:SL используйте соответствующую структуру LabAPI для вашей версии.

Плагины LabAPI обычно размещаются в:

LabAPI/
└── plugins/
    └── global/
        ├── Plugin1.dll
        ├── Plugin2.dll
        └── Plugin3.dll

Загрузите ".dll"-файлы плагинов в:

LabAPI/plugins/global/

После загрузки плагинов перезапустите сервер.

«⚠️ Плагин должен быть совместим как с вашей версией SCP:SL, так и с вашей версией LabAPI.»

---

8. EXILED

Если вы используете EXILED, установите версию EXILED, совместимую с вашей версией SCP:SL.

Плагины EXILED размещаются в:

EXILED/
└── Plugins/
    ├── Plugin1.dll
    └── Plugin2.dll

Конфигурационные файлы находятся в:

EXILED/
└── Configs/

«⚠️ Проверяйте совместимость плагинов перед установкой. LabAPI и EXILED — разные моддинг-экосистемы, поэтому плагины, предназначенные для одного фреймворка, не следует автоматически считать совместимыми с другим.»

---

9. Открытие портов

Если вы используете UFW, откройте необходимые порты:

ufw allow 7777/tcp
ufw allow 7777/udp
ufw allow 8080/tcp
ufw allow 2022/tcp

Проверьте состояние Firewall:

ufw status

«💡 Если вы используете другой порт SCP:SL, замените "7777" на выбранный вами порт.»

---

10. Готово! 🎉

После успешной установки структура будет выглядеть примерно так:

VPS
├── Pterodactyl Panel
├── Wings
└── SCP: Secret Laboratory
    ├── LabAPI
    │   └── Plugins
    └── EXILED
        ├── Plugins
        └── Configs

Теперь сервером можно управлять через Pterodactyl:

- ▶️ Запуск / остановка
- 🔄 Перезапуск
- 🖥️ Консоль
- 📁 Управление файлами
- 🔌 Управление плагинами
- 🌐 Несколько SCP:SL серверов на одном VPS

---

⚠️ Важно

«SCP:SL, LabAPI и EXILED постоянно обновляются.

Всегда проверяйте совместимость версий перед установкой сервера, фреймворка или плагинов.»

Убедитесь, что следующие компоненты совместимы:

SCP:SL
   ↓
LabAPI / EXILED
   ↓
Plugins

«⚠️ Версии игры, API и плагинов должны быть совместимы между собой.»

---

⭐ Поддержать проект

Если этот гайд оказался полезен, поставьте ⭐ этому репозиторию.

Сделано для сообщества SCP: Secret Laboratory 🧪