# Floorplan для Home Assistant через HACS

Папка внутри Home Assistant должна быть такой:

```text
/config/www/floorplan/
  apartment_floorplan.svg
  apartment_floorplan.css
```

Файл `apartment_floorplan_card.yaml` нужен не в эту папку, а для вставки в карточку Lovelace.

## Быстрая установка

1. Установи `ha-floorplan` через HACS:
   - HACS → Frontend
   - найти `ha-floorplan`
   - Download

2. Проверь ресурс:
   - Settings → Dashboards → Resources
   - должен быть ресурс:
```yaml
url: /hacsfiles/ha-floorplan/floorplan.js
type: module
```

3. Создай папку:
```text
/config/www/floorplan/
```

4. Закинь туда:
```text
apartment_floorplan.svg
apartment_floorplan.css
```

5. Перезапусти Home Assistant, если папки `www` раньше не было.

6. Dashboard → Edit dashboard → Add card → Manual card.

7. Вставь содержимое `apartment_floorplan_card.yaml`.

## Что делает первая версия

- клик по кухне включает/выключает `light.lampochki_na_kukhne`
- клик по спальне включает/выключает `light.smart_ceiling_light`
- клик по коридору включает/выключает `light.hall_strip`
- клик по гостиной включает/выключает `light.audiosistema_outlet`
- иконки открывают more-info или переключают устройство
- удержание Roborock запускает `vacuum.start`
- ванная пока просто визуальная зона

## Если карта не загрузилась

Проверь в браузере:

```text
http://IP_HOME_ASSISTANT:8123/local/floorplan/apartment_floorplan.svg
http://IP_HOME_ASSISTANT:8123/local/floorplan/apartment_floorplan.css
```

Если 404, значит файлы лежат не там.

## Если карточка пишет Custom element doesn't exist

Значит не подгрузился HACS frontend resource:

```yaml
/hacsfiles/ha-floorplan/floorplan.js
```

Проверь Settings → Dashboards → Resources.
