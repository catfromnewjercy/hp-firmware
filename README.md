Устанавливаем сначала BIOS, потом ILO

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
