# Тестирование глубоких моделей

## Тестирование глубоких моделей в синхронном режиме

**Командная строка для решения задачи классификации изображений**
```bash
python inference_sync_mode.py \
    -t classification -i <path_to_image>/<image_name> \
    -m <path_to_model>/<model_name>.xml -w <path_to_weights>/<model_name>.bin \
    --labels <path_to_labels>/image_net_synset.txt
```

Результат выполнения: набор наиболее вероятных классов, которым принадлежит
изображение.

**Командная строка для решения задачи семантической сегментации изображений**
```bash
python inference_sync_mode.py \
    -t segmentation -i <path_to_image>/<image_name> \
    -m <path_to_model>/<model_name>.xml -w <path_to_weights>/<model_name>.bin \
    --color_map <path_to_color_map>/color_map.txt
```

Результат выполнения: изображение, разрешение которого совпадает с разрешением
входного изображения; интенсивность пикселя соответствует классу объектов,
которому принадлежит даннная точка на изображении.

**Командная строка для решения задачи детектирования объектов**
```bash
python inference_sync_mode.py \
    -t detection -i <path_to_image>/<image_name> \
    -m <path_to_model>/<model_name>.xml -w <path_to_weights>/<model_name>.bin \
    -d <device>
```

Результат выполнения: набор окаймляющих прямоугольников, соответствующих
обнаруженным объектам.

## Тестирование глубоких моделей в асинхронном режиме

**Командная строка для решения задачи классификации изображений**
```bash
python inference_async_mode.py \
    -t classification -i <path_to_image>/<image_name> \
    -m <path_to_model>/<model_name>.xml -w <path_to_weights>/<model_name>.bin \
    -r <step> --labels <path_to_labels>/image_net_synset.txt -ni <iteration_number>
```

Результат выполнения: набор наиболее вероятных классов, которым принадлежит
изображение.

**Командная строка для решения задачи детектирования объектов**
```bash
python inference_async_mode.py \
    -t detection -i <path_to_image>/<image_name> \
    -m <path_to_model>/<model_name>.xml -w <path_to_weights>/<model_name>.bin \
    -r <step> -d <device> -ni <iteration_number>
```

Результат выполнения: набор окаймляющих прямоугольников, соответствующих
обнаруженным объектам.

**Командная строка для решения задачи семантической сегментации изображений**
```bash
python inference_async_mode.py \
    -t segmentation -i <path_to_image>/<image_name> \
    -m <path_to_model>/<model_name>.xml -w <path_to_weights>/<model_name>.bin \
    -r step --color_map <path_to_color_map>/color_map.txt -ni <iteration_number>
```

Результат выполнения: изображение, разрешение которого совпадает с разрешением
входного изображения; интенсивность пикселя соответствует классу объектов,
которому принадлежит даннная точка на изображении.

## Параметры скриптов

Обязательные параметры:
- `-t / --model_type` - решаемая задача (`classification`, `detection`, `segmentation`).
- `-i / --input` - путь до изображения или директории с изображениями,
  расширения картинок `.jpg`, `.png`, `.bmp` и т.д.
- `-m / --model` - путь до описания модели (xml).
- `-w / --weights` - путь до бинарного файла, содержащего веса обученной модели.
- `-r / --request` - положительное целое число, определяющее количество
  запросов на одновременную обработку в асинхронном режиме. Возможные значения:
  1 (равносилен синхронному режиму) и 2 (равносилен двухэтапному конвейеру).
- `--labels` - путь до файла с перечнем классов при решении задачи классификации.
- `-d / --device` - устройство, на котором выполняются вычисления
  (`CPU`, `GPU`, `FPGA`, `MYRIAD`).
- `--color_map` - путь до карты цветов при решении задачи семантической сегментации.
- `-ni / --number_iter` - число итераций асинхронного режима.

Необязательные параметры:
- `-b / --batch_size` - размер пачки изображений, которые будут обработаны.
  По умолчанию размер пачки равен количеству входных изображений.
  Размер пачки нужно задавать, только при работе с видео.
- `-l / --cpu_extension` - абсолютный путь к библиотеке 
  с реализацией пользовательских ядер.
- `-nt / --number_top` - число лучших результатов, выводимых
  при решении задачи классификации.
- `--prob_threshold` - порог вероятности для фильтрации результатов и 
  отбрасывания лишних при решении задачи детектирования.
