# Классификация товара (Соревнование от Казань-Экспресс)
Проект завершён в марте 2023

## В ходе проекта:
*Данный проект оказался достаточно требовательным к вычислительным ресурсам и оперативной памяти. Поэтому изначально пришлось отказаться от использования сложных моделей глубокого обучения и сосредоточиться на традиционном ML и методах оптимизации. В частности было применено несколько технических решений, показавших свою эффективность*
- *Например, метод поэтапной иерархической классификации, разработанный и реализованный с нуля, уменьшил время обучения всего до 4 минут, при потреблении памяти не более 3 Гб, хотя на "плоской" классификации падало ядро даже в Google Colab*
- *Другой пример – для лемматизации текстовых описаний товаров был применён подход с созданием словаря лемм для каждого уникального слова в датфреймах – это сократило время лемматизации в 10 раз*
- *Также в проекте активно применялись пайплайны, было написано множество функций для автоматизации повторяющихся процессов, для удобного форматированного вывода необходимой информации, для подсчёта времени выполнения процессов и другие*

<br>

## Описание
На маркетплейсе KazanExpress ежедневно появляются сотни новых товаров. Однако, проверить правильность заполнения информации обо всех товарах сразу невозможно. Неверно определенная категория зачастую приводит к потенциально упущенной прибыли как со стороны продавца, так и со стороны маркетплейса. Необходимо научиться предсказывать категорию на основе описания, картинки и других параметров товаров

В распоряжении следующие файлы:
- `train.parquet` – датафрейм pandas в формате parquet с информацией о товарах на маркетплейсе и их категориях
- `test.parquet` – файл, идентичный `train.parquet`, но без категорий, именно их и предстоит предсказать
- images – папка с двумя подпапками: train и test для картинок товаров из, соответственно, обучающей и тестовой выборки

## Цели
- Построить модель, предсказывающую категорию товара
- Модель должна иметь наилучший взвешенный F1 score, посчитанный на данных из `test.parquet`
- Также критерием оценки решения является чистота кода, оформление, понятность и последовательность исследования

## Результаты и выводы
- В рамках нашего исследования мы загрузили, изучили и обработали исходные данные
- Был проведён глубокий исследовательский анализ с подробным изучением каждого столбца на предмет использования в качестве признака для моделей ML. Дополнительно изучен целевой признак для определения стратегии построения моделей
- На основании проведённого исследования, учитывая количество категорий, размеры датасета, иерархическую структуру целевого признака и допустимые параметры ветвления, было принято решение использовать в нашей работе метод поэтапной (иерархической) классификации, что позволит разработать максимально лёгкое и нетребовательное к вычислительным ресурсам решение
- В качестве основных признаков для моделей были выбраны столбцы text_fields, shop_title, shop_id и rating. Влияние остальных признаков было решено проверить экспериментально
- Выбранный метод поэтапной классификации показал свою эффективность и быстродействие. Все модели в рамках данного исследования обучались не более ~10 минут
- Наибольший вклад в качество модели дают текстовые данные, влияние остальных признаков – product_id и sale – оказалось несущественным
- Метод стекинга в данном конкретном случае не принес значительного улучшения, но, тем не менее, лучшая модель в этом проекте использует именно его

Лучшей моделью в рамках данного исследования была признана модель стекинга на текстовой (TF-IDF + LogisticRegression) и табличной (DecisionTreeClassifier) моделях. Оценочная метрика на валидационной выборке – взвешенный f1 score – составил 0.82

За недостатком вычислительных ресурсов и времени не были рассмотрены и опробованы более технологичные, но более ресурсоемкие решения, такие как продвинутые языковые модели-трансформеры типа Bert и нейронные сети. Также никак не были задействованы изображения товаров

## Стек
python, pandas, numpy, matplotlib, seaborn, nltk, pymorphy2, sklearn, catboost

<br><br>

Код проекта: [kazan_express.ipynb](https://github.com/petrochenkovp/kazan_express/blob/main/kazan_express.ipynb)

Другие мои проекты: [Портфолио](https://github.com/petrochenkovp/portfolio/)

<br><br>
