# 8. Оператор HAVING

## Вопрос 1

**Формулировка:**
Найти города, в которых проживают более двух преподавателей.

**SQL-ответ:**
```sql
SELECT city, (SELECT COUNT(DISTINCT lecturer_id) FROM lecturer l2 WHERE l1.city IS l2.city)
FROM lecturer l1
GROUP BY city
HAVING (SELECT COUNT(DISTINCT lecturer_id) FROM lecturer l2 WHERE l1.city IS l2.city) > 2
```

**Тест `-- test`:**
```text
CITY           LECTURER_COUNT
-------------  --------------
               11
Медвежьегорск  3
Радужный       3
Торопец        3
```

**Тест `-- test1`:**
```text
CITY  LECTURER_COUNT
----  --------------
```

---

## Вопрос 2

**Формулировка:**
Найти города, где есть преподаватели, чьи `LECTURER_ID` содержат цифру `2`, и таких преподавателей в городе больше одного.

**SQL-ответ:**
```sql
SELECT city, (SELECT COUNT(DISTINCT lecturer_id) FROM lecturer l2 WHERE l1.city IS l2.city AND lecturer_id LIKE '%2%')
FROM lecturer l1
GROUP BY city
HAVING (SELECT COUNT(DISTINCT lecturer_id) FROM lecturer l2 WHERE l1.city IS l2.city AND lecturer_id LIKE '%2%') > 1
```

**Тест `-- test`:**
```text
CITY        COUNT(LECTURER_ID)
----------  ------------------
            3
Жиздра      2
Новотроицк  2
Норильск    2
Яровое      2
```

**Тест `-- test1`:**
```text
CITY  COUNT(LECTURER_ID)
----  ------------------
```

---

## Вопрос 3

**Формулировка:**
Найти города, в которых проживает более 10 студентов, и отсортировать их по убыванию численности.

**SQL-ответ:**
```sql
SELECT city, (SELECT COUNT(DISTINCT student_id) FROM student s2 WHERE s1.city IS s2.city) as count
FROM student s1
GROUP BY city
HAVING (SELECT COUNT(DISTINCT student_id) FROM student s2 WHERE s1.city IS s2.city) > 10
ORDER BY count DESC;
```

**Тест `-- test`:**
```text
CITY            STUDENT_COUNT
--------------  -------------
Горнозаводск    16
Волжск          14
Суровикино      13
Павловск        13
Зеленогорск     13
Слободской      12
Нерехта         12
Краснознаменск  12
Железногорск    12
Фокино          11
Спас-Клепики    11
Отрадное        11
Озерск          11
Киров           11
Заречный        11
Воронеж         11
Березовский     11
```

**Тест `-- test1`:**
```text
CITY  STUDENT_COUNT
----  -------------
```

---

## Вопрос 4

**Формулировка:**
Напишите запрос, выбирающий названия предметов обучения, для которых средняя оценка превышает среднюю оценку по всем предметам. Упорядочить выводимые названия предметов по алфавиту.

**SQL-ответ:**
```sql
SELECT subj_name
FROM subject s
GROUP BY subj_name
HAVING (SELECT AVG(mark) FROM exam_marks em WHERE s.subj_id = em.subj_id) > (SELECT AVG(mark) FROM exam_marks)
ORDER BY subj_name;
```

**Тест `-- test`:**
```text
SUBJ_NAME
--------------
Биология
География
История России
Обществознание
Собеседование
Сочинение
```

**Тест `-- test1`:**
```text
SUBJ_NAME
---------
```

---

## Вопрос 5

**Формулировка:**
Напишите запрос, выполняющий вывод `STUDENT_ID`, количества предметов, по которым экзаменовался каждый студент, сдававший более 6 предметов.

**SQL-ответ:**
```sql
SELECT student_id, (SELECT COUNT(exam_id) FROM exam_marks em WHERE em.student_id IS s.student_id)
FROM student s
GROUP BY student_id
HAVING (SELECT COUNT(exam_id) FROM exam_marks em WHERE em.student_id IS s.student_id) > 6;
```

**Тест `-- test`:**
```text
STUDENT_ID  COUNT(SUBJ_ID)
----------  --------------
4           7
5           7
7           7
10          8
14          8
15          8
16          8
31          8
32          8
33          7
34          7
38          7
50          7
51          7
53          7
59          7
61          8
62          7
70          7
71          7
73          7
78          7
79          7
80          7
85          8
98          8
102         8
105         8
```

**Тест `-- test1`:**
```text
STUDENT_ID  COUNT(SUBJ_ID)
----------  --------------
```

---

## Вопрос 6

**Формулировка:**
Найти университеты (`UNIV_ID`), в которых есть 2 и более студентов с одинаковой фамилией.

**SQL-ответ:**
```sql
SELECT DISTINCT univ_id, surname, COUNT(student_id) as cnt
FROM student
GROUP BY univ_id, surname
HAVING cnt >= 2;
```

**Тест `-- test`:**
```text
UNIV_ID  SURNAME       CNT
-------  ------------  ---
5        Иванченко     2
8        Свиридова     2
9        Воротнюк      2
11       Гусева        2
14       Коломыцев     2
15       Пономарев     2
16       Грушецкий     2
17       Кузнецова     2
18       Иванова       2
18       Масленников   2
19       Сергеева      2
21       Татаринцев    2
21       Яковлева      2
23       Бураков       2
23       Колодяжная    2
23       Пархоменко    2
24       Попова        2
29       Бондаренко    2
29       Петрова       2
29       Толстикова    2
30       Комаров       2
31       Матвиенко     2
32       Чеснокова     2
35       Попова        2
37       Гусева        2
37       Нестерова     2
39       Сергеев       2
40       Лукина        2
```

**Тест `-- test1`:**
```text
UNIV_ID  SURNAME  CNT
-------  -------  ---
```

---

## Вопрос 7

**Формулировка:**
Найти города, в которых студенты носят более 10 различных имён.

**SQL-ответ:**
```sql
SELECT city, COUNT(DISTINCT name) as cnt
FROM student
WHERE name IS NOT NULL
GROUP BY city
HAVING cnt > 10;
```

**Тест `-- test`:**
```text
CITY            UNIQUE_NAMES
--------------  ------------
Березовский     11
Волжск          13
Горнозаводск    16
Краснознаменск  11
Нерехта         12
Павловск        12
Спас-Клепики    11
```

**Тест `-- test1`:**
```text
CITY  UNIQUE_NAMES
----  ------------
```

---

## Вопрос 8

**Формулировка:**
Выявите самые трудоемкие курсы в учебном плане.
Выведите названия предметов, суммарная количество часов по которым (во всех семестрах) превышает 100 часов.
Отсортировать по убыванию часов.

**SQL-ответ:**
```sql
SELECT subj_name, SUM(hour) AS sum
FROM subject
GROUP BY subj_name
HAVING sum > 100
ORDER BY sum DESC;
```

**Тест `-- test`:**
```text
SUBJ_NAME             TOTAL_HOURS
--------------------  -----------
Математика            210
Биология              119
Русский и литература  111
Изложение             107
Литература            104
Собеседование         102
```

**Тест `-- test1`:**
```text
SUBJ_NAME  TOTAL_HOURS
---------  -----------
```

---

## Вопрос 9

**Формулировка:**
Найти дисциплины, которые изучаются в течение 2-х и более семестров.

**SQL-ответ:**
```sql
SELECT subj_name, COUNT(semester) SEMESTER_COUNT
FROM subject
GROUP BY subj_name
HAVING SEMESTER_COUNT >= 2;
```

**Тест `-- test`:**
```text
SUBJ_NAME   SEMESTER_COUNT
----------  --------------
Математика  2
```

**Тест `-- test1`:**
```text
SUBJ_NAME  SEMESTER_COUNT
---------  --------------
```

---

## Вопрос 10

**Формулировка:**
Найти дисциплины, которые преподаются в весенних семестрах (чётные номера: 2, 4, 6, 8), и при этом их суммарная нагрузка во **ВСЕХ** семестрах (чётных и нечётных) превышает 70 часов.

**SQL-ответ:**
```sql
SELECT subj_name, SUM(hour) sum
FROM subject
WHERE subj_name IN (
    SELECT subj_name
    FROM subject
    WHERE semester % 2 = 0
)
GROUP BY subj_name
HAVING sum > 70;
```

**Тест `-- test`:**
```text
SUBJ_NAME             TOTAL_SPRING_HOURS
--------------------  ------------------
География             74
Изложение             107
Математика            210
Обществознание        79
Русский и литература  111
Собеседование         102
Сочинение             95
```

**Тест `-- test1`:**
```text
SUBJ_NAME  TOTAL_SPRING_HOURS
---------  ------------------
```

**Тест `--test 2`:**
```text
SUBJ_NAME                                        TOTAL_SPRING_HOURS
-----------------------------------------------  ------------------
Информатика                                      120
Математика                                       75
Практика                                         250
Физика                                           80
Экономика                                        85
```

---

## Вопрос 11

**Формулировка:**
Найти дисциплины, которые преподаются в осенних семестрах (нечётные номера семестров — 1, 3, 5, 7), и при этом их суммарная нагрузка во **ВСЕХ** (чётных и нечётных) семестрах не превышает 120 часов.
Упорядочить по убыванию часов.

**SQL-ответ:**
```sql
SELECT subj_name, SUM(hour) sum
FROM subject
WHERE subj_name IN (
    SELECT subj_name
    FROM subject
    WHERE semester % 2 = 1
)
GROUP BY subj_name
HAVING sum <= 120
```

**Тест `-- test`:**
```text
SUBJ_NAME       TOTAL_SPRING_HOURS
--------------  ------------------
Биология        119
История России  73
Литература      104
Физика          69
```

**Тест `-- test1`:**
```text
SUBJ_NAME  TOTAL_SPRING_HOURS
---------  ------------------
```

**Тест `--test3`:**
```text
SUBJ_NAME             SUM(HOUR)
--------------------  ---------
Информатика           32
Логика                54
Макроэкономика        120
Маркетинг             110
Управление проектами  120
```

