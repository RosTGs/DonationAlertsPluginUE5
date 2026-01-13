![GitHub](https://img.shields.io/github/license/ufna/DonationAlerts)

# DonationAlerts UE5.4

DonationAlerts integration plugin for Unreal Engine 5.

Minimum supported version: UE5.4. The plugin is intended for UE5.4 projects and should be compatible with newer UE5 releases unless noted otherwise.

This plugin allows stream viewers to make changes to the gameplay on the go: switch weapons, alter character behavior or environmental factors. It’s up to the developer to choose the elements that can be made interactive. DonationAlerts will be providing developers with all the necessary information: donation value, message text, sender name, currency, date and time of donation. With this data at hand, the game could choose one of the scenarios to run.

Documentation: https://www.donationalerts.com/apidoc#plugins_and_libraries

## Инструкция по использованию (RU)

1. Установите плагин:
   - Скопируйте папку `DonationAlerts` в каталог `Plugins` вашего проекта Unreal Engine.
   - Убедитесь, что в файле `DonationAlerts.uplugin` указана версия, совместимая с вашей версией UE (минимум 5.4).
2. Включите плагин в проекте:
   - Откройте Unreal Editor → **Edit** → **Plugins**.
   - Найдите **DonationAlerts** в списке и включите его.
   - Перезапустите редактор, если потребуется.
3. Подготовьте ключи доступа DonationAlerts:
   - Войдите в кабинет DonationAlerts и получите необходимые ключи/токены для интеграции.
   - Сохраните их в безопасном месте.
4. Настройте плагин в проекте:
   - Откройте **Project Settings** и найдите раздел, связанный с DonationAlerts.
   - Укажите полученные ключи/токены и сохраните настройки.
5. Подключите обработку событий донатов:
   - В вашей логике игры (Blueprint или C++) подпишитесь на события донатов.
   - Используйте полученные данные (сумма, сообщение, имя отправителя, валюта, дата/время), чтобы запускать нужные игровые сценарии.
6. Проверьте работу:
   - Запустите проект и отправьте тестовый донат из кабинета DonationAlerts.
   - Убедитесь, что событие корректно обрабатывается в игре.
