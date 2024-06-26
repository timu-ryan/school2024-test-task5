# Тестовое задание для отбора на Летнюю ИТ-школу КРОК по разработке

## Условие задания
В теории управления проектами существует такое понятие как стейкхолдер или, иначе говоря, заинтересованная сторона (ЗС). Это лицо, которое может иметь влияние на ход проекта и с чьим мнением нужно считаться по совершенно разным причинам.

Тем не менее, очевидно, что каждый из стейкхолдеров по-своему важен для проекта, что является следствием разного уровня показателей интереса и влияния, потому реакция на обратную связь от каждого из стейкхолдеров определяется в соответствии с матрицей заинтересованных лиц, которая позволяет наглядно определить наиболее и наименее весомых из них.

Чаще всего такая матрица представляет собой график с осями X и Y, где по оси X располагается величина влияния, а по оси Y - величина интереса, и так как значения величин - сугубо неотрицательные числа, работа идет в правом верхнем квадранте. В свою очередь такой срез оси координат делится на 4 части в центрах осей.

Рассмотрим на примере. Допустим, у нас есть 5 ЗС. Первым делом построим ось координат XY и нанесем деления по обеим осям от 0 до 5. Далее поделим пополам каждую из осей (это будут точки (0; 2.5) и (2.5; 0)) и построим пунктирную линию для разделения матрицы на 4 квадранта. Таким образом, мы получим будущую матрицу стейкхолдеров, на которой сможем визуально расположить их по степени влияния и интереса.
![Матрица стейкхолдеров](https://github.com/croc-code/school2024-test-task5/blob/master/stakeholders_matrix.png)

Для того, чтобы определить положение стейкхолдера, используется метод попарного сравнения, схожий с рейтинговыми таблицами в спорте. Строятся матрицы (для интереса и влияния отдельно), в строках и столбцах которых располагаются стейкхолдеры в одинаковом порядке следования, а на пересечении строк и столбцов выставляются значения 0, 0.5 и 1 по следующим правилам:
- 1, если стейкхолдер в текущей строке имеет большее влияние/интерес, чем стейкхолдер в текущем столбце;
- 0.5, если стейкхолдеры в текущих строке и столбце имеют одинаковое влияние/интерес;
- 0, если стейкхолдер в текущей строке имеет меньшее влияние/интерес, чем стейкхолдер в текущем столбце.

По диагоналям значения не выставляются, так как сравнивать стейкхолдера с самим собой некорректно.

Далее полученные величины в строках складываются и получается итоговый ранг заинтересованной стороны. То же самое, но наглядно:
![alt text](https://github.com/croc-code/school2024-test-task5/blob/master/pair_compair.png)

Построив такие матрицы для интереса и влияния, мы получаем “координаты” каждого из стейкхолдеров в матрице и можем расположить в соответствующих квадратах.
Самыми важными для проекта ЗС будут те, что располагаются в правом верхнем квадранте, а те, чье мнение не обязательно учитывать здесь и сейчас, - в левом нижнем.

Вам необходимо реализовать программу, которая по полученным данным матриц влияния и интереса будет определять самых важных стейкхолдеров. В качестве входных данных используются два текстовых файла:
- interest.txt (матрица попарного сравнения стейкхолдеров по уровню интереса)
- influence.txt (матрица попарного сравнения стейкхолдеров по уровню влияния)

Каждый из файлов имеет следующий формат:
- в первой строке файла располагаются наименования заинтересованных сторон, разделенные символов “|”, соответствующие наименованию строк и столбцов матрицы в одинаковом порядке следования;
- последующие строки представляют собой матрицу попарного сравнения со значениями 0, 0.5, 1 в соответствии с правилами формирования;
- на главной диагонали (сравнение ЗС самой с собой) вместо значения идет символ “_”.

Пример содержимого одного из входных файлов:
```
Stakeholder 1 | Stakeholder 2 | Stakeholder 3 | Stakeholder 4 | Stakeholder 5
_ 0 1 0 0
1 _ 0.5 1 1
0 0.5 _ 0 0.5
1 0 1 _ 1
1 0 0.5 0 _
```

В результате работы программа должна сформировать файл (result.txt), в котором будут располагаться самые важные стейкхолдеры (в произвольном порядке). Пример содержимого выходного файла:
```
Stakeholder 1
Stakeholder 3
```

## Автор решения
Гафаров Тимур Рустемович
timu.ryanst@gmail.com
tg: @timuryanst

## Описание реализации
1. Функция `read_from_file(file_name)` считывает информацию с файла, название которого передано в аргументы функции. Функция возвращает список стейкхолдеров и двумерную матрицу со значениями сравнений. Вместо `_` сразу подставляется `0`, чтобы можно было суммировать значения строк без влияния. Обрабатываются возможные исключения.
2. Функция `write_to_file(stakeholders_list, file_name)` записывает в столбец самых важных стейкхолдеров в файл, название которого передано вторым аргументом функции. Также обрабатываются возможные исключения.
3. Функция `get_important_stakeholder_by_criteria(stakeholders, score_table)` возвращает список важных стейкхолдеров по одному критерию, принимая стейкхолдеры и матрицу для одного критерия. В функции сравнивается сумма чисел в ряду для одного стейкхолдера с медианой (количество стейкхолдеров пополам), если сумма больше, то стейкхолдер добавляется в массив важных.
4. В функции `main()` стейкхолдеры и таблицы чисел записываются в списки `stakeholders`, `interest_scores` и `influence_scores` с помощью функции `read_from_file`, с файлов `"interest.txt"` и `"influence.txt"`, после отбираются важные стейкхолдеры `interest_stakeholers` и `influence_stakeholders` при помощи функции `get_important_stakeholder_by_criteria`, после два списка сравниваются и выявляются уникальные члены, которые и "влиятельные", и "интересные" при помощи множества set. Результат записывается в файл `"result.txt"` функцией `write_to_file`.

## Инструкция по сборке и запуску решения
1. Клонировать репозиторий командой `git clone`.
2. Перейти в директорию с решением.
Для успешной отработки кода, необходимы файлы `influence.txt` и `interest.txt`.
3. Запустить код (например командой `python solution.py`).
В результаты работы программы, если не было исключений в той же дериктории появится файл `result.txt`, его можно прочитать например командой `cat result.txt`.