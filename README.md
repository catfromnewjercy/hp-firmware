*   Для установки обновлений Bios/iLO, необходимо перейти на адрес iLO (https://x.x.x.x/, где x.x.x.x - ip адресс)
*   После авторизации переходим во вкладку Firmware & OS Software

![Alt text](/instruction/ilo_firmware_screen1.png?raw=true "iLO Screen")

*   Скачиваем архив из репозитория и распаковываем
*   Нажимаем на Update Firmware
*   Выбираем Local file и в меню "Выберите файл" указываем путь сначала до файла обновления BIOS (U46_*.flash)

![Alt text](/instruction/ilo_firmware_upload.png?raw=true "Upload Screen")

* Обязательно устанавливаем галочку "Also store in iLO Repository

*	Подключаем монитор и клавитуру к сервер, включаем питание.
*	При виде экрана инициализации, необходимо вызвать меню “System Utilities” клавишей F9

![Alt text](/instruction/bios_initial_screen.png?raw=true "Bios Screen")

*	Переходим в System Configuration
*	Переходим в Bios/Platform Configuration (RBSU)
*	Workload Profile – выбираем Virtualization Max Perfomance

![Alt text](/instruction/rbsu_screen.png?raw=true "RBSU Screen")

*	Переходим в System Options
*	Далее Server Availability
Wake on Lan – Enable
Post F1 Prompt – 2 sec
Automatic Power-On – Always Power On
*	Date and Time
Выставляем часовой пояс на +5
*	Сохраняем настройки и перезагружаемся.
*	На экране инициализации нажимаем F11 (Boot Menu)
*	Запускаемся с ISO
