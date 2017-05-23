Управление яркостью экрана

При установке линукс на ноутбук с 2 видеокартами возникает проблема регулирования яркости с помощью клавиш Fn+F5 и Fn+F6.
Для решения проблемы необходимо изменить настройки загрузчика GRUB.
Открываем терминал.
Пишем команду
sudo gedit /etc/default/grub

Вместо gedit можно использовать любой текстовый редактор
Находим строку "GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
Дополняем GRUB_CMDLINE_LINUX_DEFAULT="quiet splash acpi_backlight=native"
Можно попробовать различные варианты
acpi_backlight=native
acpi_backlight=video
acpi_backlight=vendor

Находим строку GRUB_CMDLINE_LINUX=""
Меняем на GRUB_CMDLINE_LINUX="acpi_osi="
В документации говорится следущее

acpi_osi=       [HW,ACPI] Modify list of supported OS interface strings
                        acpi_osi="string1"      # add string1
                        acpi_osi="!string2"     # remove string2
                        acpi_osi=!*             # remove all strings
                        acpi_osi=!              # disable all built-in OS vendor
                                                  strings
                        acpi_osi=               # disable all strings

Сохраняем.
Пишем команду 
sudo update-grub
Для efi
grub2-mkconfig -o /boot/efi/EFI/fedora/grub.cfg

Перезагружаем систему.
Если регулировка яркости не заработала будем писать скрипт для перехвата события нажатия Fn+F5 и Fn+F6
 
Необходимо файл сделать исполняемым 
sudo chmod +x backlight_control.sh

Сохраним файл backlight_control.sh в /etc/acpi
Пишем команду
sudo cp backlight_control.sh /etc/acpi/backlight_control.sh

Добавим реакцию на нажатие Fn+F5(Уменьшить яркость)
Сохраним файл backlight_video_down из папки events в /etc/acpi/events/
sudo cp backlight_video_down /etc/acpi/events/backlight_video_down

Добавим реакцию на нажатие Fn+F6(Увеличить яркость)
Сохраним файл backlight_video_up из папки events в /etc/acpi/events/
sudo cp backlight_video_up /etc/acpi/events/backlight_video_up

Перезапустим службу acpid
sudo service acpid restart 



