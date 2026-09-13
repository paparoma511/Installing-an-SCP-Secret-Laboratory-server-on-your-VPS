SCP: Secret Laboratory Dedicated Server + Pterodactyl

Краткий гайд по установке SCP: Secret Laboratory Dedicated Server на VPS с Pterodactyl Panel + Wings.

«⚠️ Гайд проверен на Debian 13. На других ОС установка может отличаться.»

1. Подготовка VPS

Подключаемся по SSH:

ssh root@ВАШ_IP

Обновляем систему:

apt update && apt upgrade -y
apt install -y curl wget sudo

2. Установка Pterodactyl

Запускаем установщик:

bash <(curl -s https://pterodactyl-installer.se)

Выбираем:

2) Install both Panel and Wings

В процессе установки указываем:

- Database password — свой пароль
- Timezone — "Europe/Moscow"
- Email — свой email
- Admin login/password — данные администратора
- FQDN — домен или IP
- SSL — "y", если используется домен
- Firewall — "y"

После установки открываем панель:

https://ВАШ_ДОМЕН

3. Создание Node

В панели:

Admin Panel → Locations → Create New

Создаём Location, например:

Short Code: local
Description: Local server

Затем:

Admin Panel → Nodes → Create New

Пример:

Name: Node-01
Location: local
FQDN: ВАШ_ДОМЕН_ИЛИ_IP
Daemon Port: 8080
SFTP Port: 2022

RAM и Disk указываем согласно характеристикам VPS.

После создания Node открываем Configuration, копируем конфигурацию и вставляем её:

nano /etc/pterodactyl/config.yml

Перезапускаем Wings:

systemctl restart wings
systemctl enable wings

Проверяем:

systemctl status wings

Должно быть:

active (running)

4. Allocation

Открываем:

Admin Panel → Nodes → Node-01 → Allocations

Добавляем:

IP: 0.0.0.0
Port: 7777

Для дополнительных серверов можно использовать:

7778
7779
7780

5. SCP:SL Egg

Создаём:

Admin Panel → Nests → Create New

Например:

Name: SCP:SL

После этого импортируем свой JSON Egg через:

Import Egg

«Используйте свой подготовленный ".json". Настройки Egg должны соответствовать используемой версии SCP:SL.»

6. Создание сервера

Servers → Create New

Выбираем:

Nest: SCP:SL
Egg: SCP:SL
Memory: 4096 MB
Disk: 10 GB+
Allocation: 7777

Создаём сервер и нажимаем Start.

Pterodactyl автоматически установит сервер через SteamCMD.

7. LabAPI

После установки SCP:SL используем соответствующую структуру LabAPI.

Плагины LabAPI обычно загружаются в:

LabAPI/plugins/global/

После загрузки ".dll" перезапускаем сервер.

«⚠️ Плагин должен соответствовать версии SCP:SL и LabAPI.»

8. EXILED

Если вместо LabAPI используется EXILED, устанавливается соответствующая версия EXILED для вашей версии SCP:SL.

После установки плагины EXILED размещаются в:

EXILED/Plugins/

Конфигурации:

EXILED/Configs/

«⚠️ Не устанавливайте плагины LabAPI и EXILED без проверки их совместимости. Это разные моддинг-экосистемы.»

9. Открытие портов

Если используется UFW:

ufw allow 7777/tcp
ufw allow 7777/udp
ufw allow 8080/tcp
ufw allow 2022/tcp

Проверить:

ufw status

10. Готово

После запуска:

VPS
 ├── Pterodactyl Panel
 ├── Wings
 └── SCP: Secret Laboratory
      ├── LabAPI
      └── Plugins

Теперь сервером можно управлять полностью через Pterodactyl:

- запуск / остановка;
- перезапуск;
- консоль;
- файлы;
- плагины;
- несколько SCP:SL серверов на одном VPS.

«⚠️ SCP:SL, LabAPI и EXILED постоянно обновляются. Перед установкой проверяйте совместимость версий.»