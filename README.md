# Техническое задание ООО "ЛЕОТЕРРА" на должность "Python-разработчик"

[Ссылка](https://hh.ru/vacancy/108419998) на вакансию.

--------

Ниже приведены задания, которые предлагаем вам решить.

Мы не ограничиваем вас в выборе средства или метода (алгоритма) решения, если иное не оговорено в задании, так что, пожалуйста, реализуйте решение так, как бы вы хотели его сделать или привыкли делать.

> (!) Несомненным плюсом будет наличие объяснения/обоснования почему был выбран тот или иной метод/способ решения задания.



## Задание 1 - Создание простого проекта

1. **Предложите свою логику API авторизации**
1. Реализуете её с использованием flask (реализация базы данных - по желанию);

1. Также реализуете две "ручки" в веб-приложении: 
   - /login (которая возвращает общую страницу авторизации, как разметку, в которую будет подгружаться форма авторизации);
   - /get/login/content (которая вернёт исключительно форму авторизации, для указанной страницы выше. Вызвав этот метод на странице /login, необходимо получить и отрисовать форму авторизации).

1. Дополнительно реализуйте функцию-пустышку (название и путь данной "ручки" придумайте сами), которая возвращает авторизованному пользователю собранный вами json-объект, допустим, подготовленный список объектов для отображения на сайте.



## Задание 2

Это задание решите любым удобным вам способом.

Дано описание (структура) схемы базы данных (таблицы):
```postgresql
CREATE SCHEMA counterparties;
COMMENT ON SCHEMA counterparties IS 'Вся информация о заказчиках';

CREATE TABLE counterparties.type_person(
    value varchar(16) PRIMARY KEY
);

COMMENT ON TABLE counterparties.type_person IS 'Варианты значений для выбора типа лица - контрагента';
COMMENT ON COLUMN counterparties.type_person.value IS 'Само значение';

INSERT INTO counterparties.type_person(value) VALUES
('Юр. лицо'),
('Физ. лицо');


CREATE TABLE counterparties.counterparties(
    id              smallserial     PRIMARY KEY                 ,
    internal_name   varchar(64)     NOT NULL UNIQUE             ,
    person          varchar(16)     NOT NULL REFERENCES counterparties.type_person(value) DEFAULT 'Юр. лицо' ,
    INN             varchar(12)     NOT NULL UNIQUE             ,
    KPP             varchar(9)                                  ,
    OGRN            varchar(15)     UNIQUE                      ,
    heads           jsonb           NOT NULL DEFAULT '{"Фамилия Имя Отчество": {"position": "Должность", "surname": "Фамилия", "first_name": "Имя", "middle_name": "Отчество", "sex": "Пол", "act_upon": "Устава", "from_date_of": "2000-01-01", "up_to_date_of": ""}}'                   ,
    banks           jsonb           NOT NULL DEFAULT '{"Название банка": {"account": "00000000000000000000", "name": "Наименование банка", "corr_account": "00000000000000000000", "bik": "000000000"}}'                   ,
    full_name       varchar(512)    NOT NULL                    ,
    active          boolean         NOT NULL DEFAULT true
);

COMMENT ON TABLE counterparties.counterparties IS 'Таблица с перечислением всех заказчиков, которые оформляли хотя бы один заказ';
COMMENT ON COLUMN counterparties.counterparties.id IS 'Уникальный идентификационный номер';
COMMENT ON COLUMN counterparties.counterparties.internal_name IS 'Локальное имя (используется в рамках отображения пользователям в формах)';
COMMENT ON COLUMN counterparties.counterparties.person IS 'Тип лица (юридическое или физическое)';
COMMENT ON COLUMN counterparties.counterparties.INN IS 'ИНН (Идентификационный номер налогоплательщика)';
COMMENT ON COLUMN counterparties.counterparties.KPP IS 'КПП (Код причины постановки на учёт)';
COMMENT ON COLUMN counterparties.counterparties.OGRN IS 'ОГРН (Основной государственный регистрационный номер)';
COMMENT ON COLUMN counterparties.counterparties.heads IS 'Руководители (данные о руководителях: должность, фамилия, имя, отчество, пол, на основании чего действуют, дата начала деятельности, дата окончания деятельности)';
COMMENT ON COLUMN counterparties.counterparties.banks IS 'Данные о банковских счетах в разных банках (при необходимости) (расчетный счет, наименование банка, корреспондентский счет, БИК)';
COMMENT ON COLUMN counterparties.counterparties.full_name IS 'Полное наименование (используется в документах)';
COMMENT ON COLUMN counterparties.counterparties.active IS 'Выполняет ли заказчик ещё свою деятельность';


INSERT INTO counterparties.counterparties(internal_name, INN, KPP, OGRN, heads, banks, full_name) VALUES
('Компания 1', '5073530173', '504120546', '5644193644208', '{"Петров Евгений Васильевич": {"position": "Генеральный директор", "surname": "Петров", "first_name": "Евгений", "middle_name": "Васильевич", "sex": "male", "act_upon": "Устава", "from_date_of": "2009-11-16", "up_to_date_of": ""}}', '{"Банк ВТБ": {"account": "46235285108075356324", "name": "Банк ВТБ", "corr_account": "82456846202400480848", "bik": "715493251"}}', 'Общество с ограниченной ответственностью "Компания Первых"'),
('Компания Егора', '5091475412', '506193519', '2883519172040', '{"Поддубный Егор Дмитрович": {"position": "Директор", "surname": "Поддубный", "first_name": "Егор", "middle_name": "Дмитрович", "sex": "male", "act_upon": "Устава", "from_date_of": "2012-10-01", "up_to_date_of": ""}}', '{"Банк СБЕР": {"account": "91423421155207912522", "name": "Банк СБЕР", "corr_account": "03610058264387258146", "bik": "520474627"}, "Альфа Банк": {"account": "63482356472656477742", "name": "Альфа Банк", "corr_account": "95878563565550043241", "bik": 6210064525"}}', 'Общество с ограниченной ответственностью "ПЕДСтрой"'),
('ООО Вымпелком', '5018246125', '505239156', '7256372514834', '{"Разумовский Виктор Игоревич": {"position": "Исполнительный директор", "surname": "Разумовский", "first_name": "Виктор", "middle_name": "Игоревич", "sex": "male", "act_upon": "Устава", "from_date_of": "2021-04-09", "up_to_date_of": ""}, "Никифоров Арсений Витальевич": {"position": "Генеральный директор", "surname": "Никифоров", "first_name": "Арсений", "middle_name": "Витальевич", "sex": "male", "act_upon": "Устава", "from_date_of": "2016-04-03", "up_to_date_of": "2021-04-09"}}', '{"Банк ГАЗПРОМ": {"account": "84629341521383256729", "name": "Банк ГАЗПРОМ", "corr_account": "12493746237492990023", "bik": "718544306"}}', 'Общество с ограниченной ответственностью "Вымпелком"'),
('Рога и копыта', '5084764622', '507477325', '6407886451211', '{"Курпина Евгения Анатольевна": {"position": "Генеральный директор", "surname": "Курпина", "first_name": "Евгения", "middle_name": "Анатольевна", "sex": "female", "act_upon": "Устава", "from_date_of": "2013-06-21", "up_to_date_of": ""}}', '{"Банк Тинькоф": {"account": "94854348840008631648", "name": "Банк Тинькоф", "corr_account": "74746662829900034332", "bik": "932113118"}}', 'Общество с ограниченной ответственностью "Рога и Копыта"');
```

1. Вам необходимо вывести/вернуть список всех заказчиков (id, локальное имя, тип, актуального руководителя) отсортировав результаты по возрастанию по полям: 
   - типу лица;
   - в алфавитном порядке по локальным именам;
   - только рабочих заказчиков.

1. Необходимо вывести/вернуть список банков определенного заказчика (по его уникальному полю)



## Задание 3 - Упаковать проект с Flask в docker/docker-compose

Упаковать вами написанный проект в docker/docker-compose, при необходимости добавить окружение.



## Задание 4 - Написать менеджеру в hh.ru

Не забудьте **обязательно предоставить** ссылку на github с решением тестового задания.



## Задание 5 - Ожидать результатов 

Запаситесь терпением, с вами обязательно свяжутся и сообщат решение.


-------

# Успехов!
