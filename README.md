[![Build Status](https://travis-ci.com/allnes/pp_2019_autumn.svg?branch=master)](https://travis-ci.com/allnes/pp_2019_autumn)
[![Build status](https://ci.appveyor.com/api/projects/status/dye49wsp7ij15gdo/branch/master?svg=true)](https://ci.appveyor.com/project/allnes/pp-2019-autumn/branch/master)

# Практика по параллельному программированию
В практике рассматриваются следующие технологии параллельного программирования:
* MPI
* OpenMP
* TBB

### Правила работы
1. <b>Не</b> отлаживаемся в репозитории, для этого у вас есть локальные машины и все скрипты (стиль кодирования)
2. <b>Уважаем</b> время других и не задерживаем очередь
3. <b>Тщательно</b> проверяем программу на зависания

## 1. Установка компонент для корректной работы
### Submodules
Проект не будет собираться полностью пока вы не скачаете все модули репозитория командой:
```
git submodule update --init --recursive
```
### MPI
  * <b>Windows (MSVC)</b>:
    Ссылка на установочные файлы [здесь](https://www.microsoft.com/en-us/download/details.aspx?id=57467).
    Обязательно надо установить следующие файлы: `msmpisdk.msi` и `msmpisetup.exe`
  * <b>Linux (gcc и clang)</b>:
  ```
  sudo apt install mpich
  sudo apt install openmpi-bin
  sudo apt install libopenmpi-dev
  ```
  * <b>MacOS (apple clang)</b>:
  ```
  brew install open-mpi
  ```

### OpenMP
  OpenMP встроен в компиляторы `gcc` и `msvc`, но все таки часть компонент нужно установить для некоторых систем:
  * <b>Linux (gcc и clang)</b>:
  ```
  sudo apt install libomp-dev
  ```
  * <b>MacOS (apple clang)</b>: Система сильно нестабильная, пока не рекомендуется использовать ее для OpenMP!
  ```
  brew install libomp
  ```

### TBB
  * <b>Windows (MSVC)</b>: CMake при использовании этого проекта на Windows сам устанвливает TBB.
  * <b>Linux (gcc и clang)</b>:
  ```
  sudo apt-get install libtbb-dev
  ```
  * <b>MacOS (apple clang)</b>:
  ```
  brew install tbb
  ```

## 2. Построение проекта с помощью CMake
Переходим в директорию с исходным кодом.

1) Получаем конфигурационные файлы для сборки: makefile, .sln и т.д.

  ```
  mkdir build
  cd build
  cmake -D USE_MPI=ON -D USE_OMP=ON -D USE_TBB=ON ..
  cd ..
  ```
<i>Комментарий про ключи CMake:</i>
- `-D USE_MPI=ON` отвечает за сборку зависимостей и проектов свзанных с MPI.
- `-D USE_OMP=ON` отвечает за сборку зависимостей и проектов свзанных с OpenMP.
- `-D USE_TBB=ON` отвечает за сборку зависимостей и проектов свзанных с TBB.

<i>Соотвественно, если что-то не потребуется, то флаг можно не указывать.</i>

2) Собираем  проект:
  ```
  cmake --build build --config RELEASE
  ```
3) Находим и запускаем исполняемый файл в директории `<наш проект>/build/bin`

## 3. Инструкция по размещению своих исходных кодов в проекте
* В директории `modules` есть папки с задачами: `task_1`, `task_2`, `task_3`.
Находим директорию соотвествующую вашей задаче и переходим в нее. Создаем папку с названием `<фамилия>_<инициал имени>_<краткое название задачи>`. К примеру: `task1/nesterov_a_vector_sum`.
* Теперь переходим в созданную нами директорию и начинаем работу над задачей. В данной директории должны быть всего 4 файла и все (кроме `CMakeLists.txt`) написанные вами:
  - `main.cpp` - google тесты для вашей задачи, количество тестов должно быть 5 или больше.
  - `vector_sum.h`   - заголовочный файл с прототипами функций вашей задачи, название файла такое же, как и `<краткое название задачи>`.
  - `vector_sum.cpp` - исходные коды реализации функций вашей задачи, название файла такое же, как и `<краткое название задачи>`.
  - `CMakeLists.txt` - конфигурация вашего проекта. Пример для каждой конфигурации находятся в директории `test_tasks`.
* Название pull-request'а выглядит следующим образом:
  ```
  <Фамилия Имя>. Задача <Номер задачи>. <Полное название задачи>.
  Нестеров Александр. Задач 1. Сумма элементов вектора.
  ```
* В описание pull-request'а пишем полную постановку задачи.

  Пример pull-request'а находится в pull-request'ах проекта.

* Работаем со своим fork-репозитроием. Работаем в отдельной ветке и <b>НЕ в `master`!!!</b> Название ветки аналогично названию директории для вашей задачи. К примеру создание ветки:
  ```
  git checkout -b nesterov_a_vector_sum
  ```

## 4. Инстркция по размещению отчетов в проекте

* В директории `reports` размещаем <b>pdf-файл</b> с отчетом.
Отчет должен называться следующим образом - `<фамилия>_<инициал имени>_<краткое название задачи>.pdf`

  ```
  nesterov_a_vector_sum.pdf
  ```
* Название pull-request'а для отчета выглядит следующим образом:
  ```
  <Фамилия Имя>. Отчет. <Полное название задачи>.
  Нестеров Александр. Отчет. Сумма элементов вектора.
  ```

## Использование стиля кодирования
Для проверки стиля кодирования используется Google C++ Style.
* Описание стиля находится [здесь](https://google.github.io/styleguide/cppguide.html).
* Проверить стиль можно с помощью скрипта:
  ```
  python scripts/lint.py
  ```
<i>Невывполнение правил ведет к покраснению сборки проекта.</i>
