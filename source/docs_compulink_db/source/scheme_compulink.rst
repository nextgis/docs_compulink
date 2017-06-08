.. sectionauthor:: Александр Мурый <amuriy@gmail.com>

.. _compulink_db_schema_compulink:

Описание таблиц схемы compulink
===============================

Таблица хранения информации о проектах строительства
----------------------------------------------------

Данная таблица содержит информацию о крупных проектах строительства, таких как УЦН. 
Данная информация необходима для построения графиков и отчетов о реализации проекта Устранение цифрового неравенства, в части выявления принадлежности объектов строительства к данному проекту.

.. _table-project:
.. tabularcolumns:: |p{3.5cm}|p{1.6cm}|p{1.6cm}|p{0.7cm}|p{7.5cm}|
.. csv-table:: Структура таблицы project
   :header: "Поле", "PK\/FK\/UN", "Тип", "Null", "Описание"

    id, PK, serial, нет, Идентификатор
    name,, character varying (300), да, Наименование
    "short_name",, character varying (100), да, Короткое название
    "description",, text, да, Описание
    "root\_resource\_id",, integer, да, "Идентификатор ресурса группового слоя, содержащего все объекты строительства. Родительская таблица public.resource"
    "keyname", UNIQ, character varying (100), да, "Ключевое имя. Используется для отметок проектов, которые учитываются при сборе статистики проекта УЦН"



Таблица хранения информации об объектах строительства
-----------------------------------------------------

Данная таблица содержит информацию по всем объектам строительства. Данные из этой таблицы используются для построения
отчетов, а так же для осуществления выборок в различных фильтрах системы.
Периодически (ежесуточно), при доступности, информация в таблице обновляется из внешней БД.
Связь со структурами слоев объектов строительства осуществляется  через внешний идентифиактор.

.. _table-construct-object:
.. tabularcolumns:: |p{3.5cm}|p{1.6cm}|p{1.6cm}|p{0.7cm}|p{7.5cm}|
.. csv-table:: Структура таблицы construct\_object
   :header: "Поле", "PK\/FK\/UN", "Тип", "Null", "Описание"

   resource\_id              ,PK ,integer                 ,нет ,Идентификатор ресурса группового слоя. Родительская таблица public.resource
   name                      ,   ,character varying (300) ,да  ,Наименование ВОЛС
   external\_id              ,   ,character varying (300) ,да  ,Внешний идентификатор
   district\_id              ,FK ,integer                 ,да  ,Идентификатор района. Родительская таблица :ref:`district<table-district>`
   region\_id                ,FK ,integer                 ,да  ,Идентификатор региона. Родительская таблица :ref:`region<table-region>`
   project\_id               ,FK ,integer                 ,да  ,Идентификатор проекта. Родительская таблица :ref:`project<table-project>`
   subcontr\_name            ,   ,character varying (300) ,да  ,Подрядчик строительства
   start\_build\_date        ,   ,date                    ,да  ,Строительство ВОЛС (начало)
   end\_build\_date          ,   ,date                    ,да  ,Строительство ВОЛС (окончание)
   start\_deliver\_date      ,   ,date                    ,да  ,Cдача заказчику (начало)
   end\_deliver\_date        ,   ,date                    ,да  ,Cдача заказчику (окончание)
   cabling\_plan             ,   ,double precision        ,да  ,Прокладка ОК План (км)
   fosc\_plan                ,   ,integer                 ,да  ,Разварка муфт План (шт)
   cross\_plan               ,   ,integer                 ,да  ,Разварка кроссов План (шт)
   spec\_trans\_plan         ,   ,integer                 ,да  ,Строительство ГНБ переходов План (шт)
   access\_point\_plan       ,   ,integer                 ,да  ,Монтаж точек доступа План (шт)

 
Таблицы хранения информации об административно-территориальном делении
----------------------------------------------------------------------

Таблицы содержат информацию о федеральных округах (далее ФО), регионах и районах РФ. rt\_macro\_division и rt\_branch содержат 
соответственно макрорегиональные и региональные филиалы Ростелекома. Таблица rt\_branch\_region описывает взаимоотношение
регионов к региональным филиалам Ростелекома.

.. _table-federal-district:
.. tabularcolumns:: |p{3.5cm}|p{1.6cm}|p{1.6cm}|p{0.7cm}|p{7.5cm}|
.. csv-table:: Структура таблицы federal\_district
   :header: "Поле", "PK\/FK\/UN", "Тип", "Null", "Описание"

   id                        ,PK ,serial                  ,нет ,Идентификатор
   name                      ,   ,character varying (300) ,да  ,Наименование
   short_name                ,   ,character varying (100) ,да  ,Короткое название

.. _table-region:
.. tabularcolumns:: |p{3.5cm}|p{1.6cm}|p{1.6cm}|p{0.7cm}|p{7.5cm}|
.. csv-table:: Структура таблицы region
   :header: "Поле", "PK\/FK\/UN", "Тип", "Null", "Описание"

   id                        ,PK ,serial                  ,нет ,Идентификатор
   name                      ,   ,character varying (300) ,да  ,Наименование
   short_name                ,   ,character varying (100) ,да  ,Короткое название
   region\_code              ,   ,integer                 ,да  ,Код региона
   federal\_dist\_id         ,   ,integer                 ,да  ,Идентификатор родительского ФО. Родительская таблица :ref:`federal_district <table-federal-district>`

.. _table-district:
.. tabularcolumns:: |p{3.5cm}|p{1.6cm}|p{1.6cm}|p{0.7cm}|p{7.5cm}|
.. csv-table:: Структура таблицы district
   :header: "Поле", "PK\/FK\/UN", "Тип", "Null", "Описание"

   id                        ,PK ,serial                  ,нет ,Идентификатор
   name                      ,   ,character varying (300) ,да  ,Наименование
   short_name                ,   ,character varying (100) ,да  ,Короткое название
   region\_id                ,FK ,integer                 ,да  ,Идентификатор родительского региона. Родительская таблица :ref:`region <table-region>`


.. _table-rt-macro-division:
.. tabularcolumns:: |p{3.5cm}|p{1.6cm}|p{1.6cm}|p{0.7cm}|p{7.5cm}|
.. csv-table:: Структура таблицы rt\_macro\_division
   :header: "Поле", "PK\/FK\/UN", "Тип", "Null", "Описание"

   id                        ,PK ,serial                  ,нет ,Идентификатор
   name                      ,   ,character varying (300) ,да  ,Наименование Макрорегионального филиал Ростелекома
   short_name                ,   ,character varying (100) ,да  ,Короткое название


.. _table-rt-branch:
.. tabularcolumns:: |p{3.5cm}|p{1.6cm}|p{1.6cm}|p{0.7cm}|p{7.5cm}|
.. csv-table:: Структура таблицы rt\_branch
   :header: "Поле", "PK\/FK\/UN", "Тип", "Null", "Описание"

   id                        ,PK ,serial                  ,нет ,Идентификатор
   name                      ,   ,character varying (300) ,да  ,Наименование Регионального филиала Ростелекома
   short_name                ,   ,character varying (100) ,да  ,Короткое название
   macro_division\_id        ,FK ,integer                 ,да  ,Идентификатор макрорегионального филиала Ростелекома. Родительская таблица :ref:`rt_macro_division<table-rt-macro-division>`


.. _table-rt-branch-region:
.. tabularcolumns:: |p{3.5cm}|p{1.6cm}|p{1.6cm}|p{0.7cm}|p{7.5cm}|
.. csv-table:: Структура таблицы rt\_branch\_region
   :header: "Поле", "PK\/FK\/UN", "Тип", "Null", "Описание"

   region\_id              ,PK/FK ,integer               ,нет ,Идентификатор субъекта РФ. Родительская таблица :ref:`region<table-region>`
   rt\_branch\_id          ,FK    ,integer               ,нет ,Идентификатор регионального филиала Ростелекома. Родительская таблица :ref:`rt_branch<table-rt-branch>`



Таблицы хранения информации о ежедневном ходе строительства
-----------------------------------------------------------

Таблицы содержат данные по результатам ежедневного подсчета выполненных работ по каждому объекту строительства. 
Для каждого элемента строительства информацию хранится в разрезе основных характеристик.
Справочники, используемые в данных таблицах описаны в разделе Справочники.

Каждая запись в таблице built\_fosc содержит количество муфт типа fosc\_type разваренных в течении дня build\_date для объекта строительства resource\_id.
Агрегация записей по полю resource\_id позволяет получить общее количество разваренных муфт (всех типов) для объекта строительства.
Агрегация записей по полям resource\_id и build\_date позволяет получить общее количество разваренных муфт (всех типов) для объекта строительства на каждую дату строительства.
Агрегация записей по полям resource\_id и fosc\_type позволяет получить общее количество разваренных муфт каждого типа для объекта строительства.

.. tabularcolumns:: |p{3.5cm}|p{1.6cm}|p{1.6cm}|p{0.7cm}|p{7.5cm}|
.. csv-table:: Структура таблицы built\_fosc
   :header: "Поле", "PK\/FK\/UN", "Тип", "Null", "Описание"

   id                        ,PK ,serial                  ,нет ,Идентификатор
   resource\_id              ,   ,integer                 ,нет ,Идентификатор ресурса группового слоя (ид объекта строительства). Родительская таблица public.resource
   build\_date\_id           ,FK ,integer                 ,нет ,Дата строительства. Родительская таблица :ref:`calendar<table-calendar>`
   fosc\_count               ,   ,integer                 ,нет ,Кол-во развараренных муфт (шт)
   fosc\_type\_id            ,FK ,integer                 ,да  ,Тип муфты. Родительская таблица :ref:`fosc_type<table-fosc-type>`



Каждая запись в таблице built\_cable содержит протяженность кабеля проложенного способом laying\_method в течении дня build\_date для объекта строительства resource\_id.
Агрегация записей по полю resource\_id позволяет получить общую протяженность кабеля, проложенного всеми способами, для объекта строительства.
Агрегация записей по полям resource\_id и build\_date позволяет получить общую протяженность кабеля, проложенного всеми способами, для объекта строительства на каждую дату строительства.
Агрегация записей по полям resource\_id и laying\_method позволяет получить общую протяженность кабеля, проложенного каждым способом прокладки, для объекта строительства.

.. tabularcolumns:: |p{3.5cm}|p{1.6cm}|p{1.6cm}|p{0.7cm}|p{7.5cm}|
.. csv-table:: Структура таблицы built\_cable
   :header: "Поле", "PK\/FK\/UN", "Тип", "Null", "Описание"

   id                        ,PK ,serial                  ,нет ,Идентификатор
   resource\_id              ,   ,integer                 ,нет ,Идентификатор ресурса группового слоя (ид объекта строительства). Родительская таблица public.resource
   build\_date\_id           ,FK ,integer                 ,нет ,Дата строительства. Родительская таблица :ref:`calendar<table-calendar>`
   cable\_length             ,   ,double precision        ,нет ,Протяженность проложенного кабеля (км)
   laying\_method_id         ,FK ,integer                 ,да  ,Способ прокладки. Родительская таблица :ref:`cable_laying_method<table-cable-laying-method>`


Каждая запись в таблице built\_optical\_cross содержит количество оптических кроссов типа optical\_cross\_type разваренных в течении дня build\_date для объекта строительства resource\_id.
Агрегация записей по полю resource\_id позволяет получить общее количество разваренных оптических кроссов (всех типов) для объекта строительства.
Агрегация записей по полям resource\_id и build\_date позволяет получить общее количество разваренных оптических кроссов (всех типов) для объекта строительства на каждую дату строительства.
Агрегация записей по полям resource\_id и optical\_cross\_type позволяет получить общее количество разваренных оптических кроссов каждого типа для объекта строительства.

.. tabularcolumns:: |p{3.6cm}|p{1.6cm}|p{1.6cm}|p{0.7cm}|p{7.4cm}|
.. csv-table:: Структура таблицы built\_optical\_cross
   :header: "Поле", "PK\/FK\/UN", "Тип", "Null", "Описание"

   id                        ,PK ,serial                  ,нет ,Идентификатор
   resource\_id              ,   ,integer                 ,нет ,Идентификатор ресурса группового слоя (ид объекта строительства). Родительская таблица public.resource
   build\_date\_id           ,FK ,integer                 ,нет ,Дата строительства. Родительская таблица :ref:`calendar<table-calendar>`
   optical\_cross\_count     ,   ,integer                 ,нет ,Кол-во разваренных кроссов (шт)
   optical\_cross\_type_id   ,FK ,integer                 ,да ,Тип кросса. Родительская таблица :ref:`optical_cross_type<table-optical-cross-type>`



Каждая запись в таблице built\_spec\_transition содержит протяженность и количество спецпереходов, построенных способом spec\_laying\_method, в течении дня build\_date для объекта строительства resource\_id.
Агрегация записей по полю resource\_id позволяет получить общую протяженность и количество спецпереходов, построенных всеми способами, для объекта строительства.
Агрегация записей по полям resource\_id и build\_date позволяет получить общую протяженность и количество спецпереходов, построенных всеми способами, для объекта строительства на каждую дату строительства.
Агрегация записей по полям resource\_id и spec\_laying\_method позволяет получить общую протяженность и количество спецпереходов, построенных каждым способом прокладки, для объекта строительства.

.. tabularcolumns:: |p{3.9cm}|p{1.6cm}|p{1.6cm}|p{0.7cm}|p{7.1cm}|
.. csv-table:: Структура таблицы built\_spec\_transition
   :header: "Поле", "PK\/FK\/UN", "Тип", "Null", "Описание"

   id                        ,PK ,serial                  ,нет ,Идентификатор
   resource\_id              ,   ,integer                 ,нет ,Идентификатор ресурса группового слоя (ид объекта строительства). Родительская таблица public.resource
   build\_date\_id           ,FK ,integer                 ,нет ,Дата строительства. Родительская таблица :ref:`calendar<table-calendar>`
   spec\_trans\_length       ,   ,integer                 ,нет ,Протяженность построенных спецпереходов (км)
   spec\_laying\_method\_id  ,FK ,integer                 ,да  ,Способ прокладки кабеля. Родительская таблица :ref:`spec_laying_method<table-spec-laying-method>`
   spec\_trans\_count        ,   ,integer                 ,нет ,Количество построенных спецпереходов



Каждая запись в таблице built\_access\_point содержит количество точек доступа типа access\_point\_type установленных в течении дня build\_date для объекта строительства resource\_id.
Агрегация записей по полю resource\_id позволяет получить общее количество установленных точек доступа (всех типов) для объекта строительства.
Агрегация записей по полям resource\_id и build\_date позволяет получить общее количество установленных точек доступа (всех типов) для объекта строительства на каждую дату строительства.
Агрегация записей по полям resource\_id и access\_point\_type  позволяет получить общее количество установленных точек доступа каждого типа для объекта строительства.

.. tabularcolumns:: |p{3.9cm}|p{1.6cm}|p{1.6cm}|p{0.7cm}|p{7.1cm}|
.. csv-table:: Структура таблицы built\_access\_point
   :header: "Поле", "PK\/FK\/UN", "Тип", "Null", "Описание"

   id                        ,PK ,serial                  ,нет ,Идентификатор
   resource\_id              ,   ,integer                 ,нет ,Идентификатор ресурса группового слоя (ид объекта строительства). Родительская таблица public.resource
   build\_date\_id           ,FK ,integer                 ,нет ,Дата строительства. Родительская таблица :ref:`calendar<table-calendar>`
   access\_point\_count      ,   ,integer                 ,нет ,Кол-во установленных точек доступа (шт)
   access\_point\_type\_id   ,FK ,integer                 ,да  ,Тип точки доступа. Родительская таблица :ref:`access_point_type<table-access-point-type>`



Таблицы хранения справочников
-----------------------------
Таблицы, структура которых приведена ниже, используются для хранения справочных типов, используемых в других таблицах.

Каждая запись в таблице calendar содержит денормализованную информацию на каждую календарную дату в период с 2014 по 2024 года.
Данная таблица позволяет быстро производить выборки по различным произвольным периодам времени.
Идентификатор записи представляет из себя целое число, поразрядные значения которого соответствуют шаблону  'YYYYMMDD'.

.. _table-calendar:
.. tabularcolumns:: |p{4.0cm}|p{1.6cm}|p{1.6cm}|p{0.7cm}|p{7.0cm}|
.. csv-table:: Структура таблицы calendar
   :header: "Поле", "PK\/FK\/UN", "Тип", "Null", "Описание"

   id                        ,PK ,integer                 ,нет ,Идентификатор
   full\_date                ,   ,date                    ,да  ,Полная дата
   year\_number              ,   ,smallint                ,да  ,Год
   semester\_number          ,   ,smallint                ,да  ,Номер полугодия
   semester\_name            ,   ,character varying (15)  ,да  ,Название полугодия
   quarter\_number           ,   ,smallint                ,да  ,Номер квартала
   quarter\_name             ,   ,character varying (15)  ,да  ,Название квартала
   month\_number             ,   ,smallint                ,да  ,Номер месяца
   month\_name               ,   ,character varying (15)  ,да  ,Название месяца
   year\_week\_number        ,   ,smallint                ,да  ,Номер недели в году
   month\_week\_number       ,   ,smallint                ,да  ,Номер недели в месяце
   month\_decade\_number     ,   ,smallint                ,да  ,Номер недели в декаде
   year\_day\_number         ,   ,smallint                ,да  ,Номер дня в году
   month\_day\_number        ,   ,smallint                ,да  ,Номер дня в месяце
   week\_day\_number         ,   ,smallint                ,да  ,Номер дня в недели
   week\_day\_name           ,   ,character varying (11)  ,да  ,Название дня недели
   week\_day\_short\_name    ,   ,character varying (2)   ,да  ,Короткое название дня недели
   weekend                   ,   ,boolean                 ,да  ,Выходной



Таблица access\_point\_type содержит описание типов точек доступа. Поле description содержит текстовое описание конкретного типа.

.. _table-access-point-type:
.. tabularcolumns:: |p{3.5cm}|p{1.6cm}|p{1.6cm}|p{0.7cm}|p{7.5cm}|
.. csv-table:: Структура таблицы access\_point\_type
   :header: "Поле", "PK\/FK\/UN", "Тип", "Null", "Описание"

   id                        ,PK ,serial                  ,нет ,Идентификатор
   description               ,   ,text                    ,да  ,Описание
   type                      ,   ,character varying (100) ,нет ,Тип точки доступа

Таблица cable\_laying\_method содержит описание методов прокладки кабеля. Поле description содержит текстовое описание конкретного метода.

.. _table-cable-laying-method:
.. tabularcolumns:: |p{3.5cm}|p{1.6cm}|p{1.6cm}|p{0.7cm}|p{7.5cm}|
.. csv-table:: Структура таблицы cable\_laying\_method
   :header: "Поле", "PK\/FK\/UN", "Тип", "Null", "Описание"

   id                        ,PK ,serial                  ,нет ,Идентификатор
   description               ,   ,text                    ,да  ,Описание
   method                    ,   ,character varying (100) ,нет ,Метод прокладки кабеля

Таблица fosc\_type содержит описание типов оптических муфт. Поле description содержит текстовое описание конкретного типа.

.. _table-fosc-type:
.. tabularcolumns:: |p{3.5cm}|p{1.6cm}|p{1.6cm}|p{0.7cm}|p{7.5cm}|
.. csv-table:: Структура таблицы fosc\_type
   :header: "Поле", "PK\/FK\/UN", "Тип", "Null", "Описание"

   id                        ,PK ,serial                  ,нет ,Идентификатор
   description               ,   ,text                    ,да  ,Описание
   type                      ,   ,character varying (100) ,нет ,Тип оптической муфты



Таблица optical\_cross\_type содержит описание типов оптических кроссов. Поле description содержит текстовое описание конкретного типа.

.. _table-optical-cross-type:
.. tabularcolumns:: |p{3.5cm}|p{1.6cm}|p{1.6cm}|p{0.7cm}|p{7.5cm}|
.. csv-table:: Структура таблицы optical\_cross\_type
   :header: "Поле", "PK\/FK\/UN", "Тип", "Null", "Описание"

   id                        ,PK ,serial                  ,нет ,Идентификатор
   description               ,   ,text                    ,да  ,Описание
   type                      ,   ,character varying (100) ,нет ,Тип оптического кросса



Таблица spec\_laying\_method содержит описание типов спецпереходов. Поле description содержит текстовое описание конкретного типа.

.. _table-spec-laying-method:
.. tabularcolumns:: |p{3.5cm}|p{1.6cm}|p{1.6cm}|p{0.7cm}|p{7.5cm}|
.. csv-table:: Структура таблицы spec\_laying\_method
   :header: "Поле", "PK\/FK\/UN", "Тип", "Null", "Описание"

   id                        ,PK ,serial                  ,нет ,Идентификатор
   description               ,   ,text                    ,да  ,Описание
   method                    ,   ,character varying (100) ,нет ,Тип спецперехода



Таблица хранения информации об отклонениях при строительстве
------------------------------------------------------------

Таблица содержит сводную информацию об отклонениях, допущенных при строительстве объектов. 
Данные из этой таблицы используются для построения отчета об отклонениях.

.. tabularcolumns:: |p{3.5cm}|p{1.6cm}|p{1.7cm}|p{0.7cm}|p{7.4cm}|
.. csv-table:: Структура таблицы construct\_deviation
   :header: "Поле", "PK\/FK\/UN", "Тип", "Null", "Описание"

   id                        ,PK ,serial                  ,нет ,Идентификатор
   focl\_res\_id             ,   ,integer                 ,нет ,Идентификатор ресурса. Родительская таблица public.resource
   focl\_proj                ,   ,character varying       ,нет ,Название проекта строительства
   focl\_name                ,   ,character varying       ,нет ,Название объекта строительства
   object\_type              ,   ,character varying       ,нет ,Тип объекта с отклонением
   object\_num               ,   ,integer                 ,нет ,Номер объекта с отклонением
   deviation\_distance       ,   ,integer                 ,нет ,Расстояние отклонения (м)
   deviation\_approved       ,   ,boolean                 ,нет ,Отклонение утверждено
   approval\_comment         ,   ,character varying       ,да  ,Комментарий к утверждению отклонения
   approval\_author          ,   ,character varying       ,да  ,"Пользователь, утвердивший отклонение"
   approval\_timestamp       ,   ,timestamp wtz           ,да  ,Дата и время утверждения отклонения


Таблица хранения информации о статусе строительства
---------------------------------------------------

Таблица содержит сводную информацию о всех объектах строительства и текущем состоянии этих объектов. 
Данные из этой таблицы используются для построения отчета о статусе строительства.

.. tabularcolumns:: |p{3.5cm}|p{1.6cm}|p{1.7cm}|p{0.7cm}|p{7.4cm}|
.. csv-table:: Структура таблицы status\_report
   :header: "Поле", "PK\/FK\/UN", "Тип", "Null", "Описание"

   id                        ,PK ,serial                  ,нет ,Идентификатор
   focl\_res\_id             ,   ,integer                 ,да  ,Идентификатор ресурса. Родительская таблица public.resource
   focl\_name                ,   ,character varying       ,да  ,Название объекта строительства
   region                    ,   ,integer                 ,да  ,Регион. Родительская таблица :ref:`region<table-region>`
   district                  ,   ,integer                 ,да  ,Район. Родительская таблица :ref:`disrtict<table-district>`
   status                    ,   ,character varying       ,да  ,Статус строительства
   subcontr\_name            ,   ,character varying       ,да  ,Подрядчик строительства
   start\_build\_time        ,   ,timestamp wtz           ,да  ,Строительство ВОЛС (начало)
   end\_build\_time          ,   ,timestamp wtz           ,да  ,Строительство ВОЛС (окончание)
   start\_deliver\_time      ,   ,timestamp wtz           ,да  ,Cдача заказчику (начало)
   end\_deliver\_time        ,   ,timestamp wtz           ,да  ,Cдача заказчику (окончание)
   cabling\_plan             ,   ,double precision        ,да  ,Прокладка ОК. План (км)
   cabling\_fact             ,   ,double precision        ,да  ,Прокладка ОК. Факт (км)
   cabling\_percent          ,   ,integer                 ,да  ,Прокладка ОК. Процент выполнения (%)
   fosc\_plan                ,   ,integer                 ,да  ,Разварка муфт. План (шт)
   fosc\_fact                ,   ,integer                 ,да  ,Разварка муфт. Факт (шт)
   fosc\_percent             ,   ,integer                 ,да  ,Разварка муфт. Процент выполнения (%)
   cross\_plan               ,   ,integer                 ,да  ,Разварка кроссов. План (шт)
   cross\_fact               ,   ,integer                 ,да  ,Разварка кроссов. Факт (шт)
   cross\_percent            ,   ,integer                 ,да  ,Разварка кроссов. Процент выполнения (%)
   spec\_trans\_plan         ,   ,integer                 ,да  ,Строительство ГНБ переходов. План (шт)
   spec\_trans\_fact         ,   ,integer                 ,да  ,Строительство ГНБ переходов. Факт (шт)
   spec\_trans\_percent      ,   ,integer                 ,да  ,Строительство ГНБ переходов. Процент выполнения (%)
   ap\_plan                  ,   ,integer                 ,да  ,Монтаж точек доступа. План (шт)
   ap\_fact                  ,   ,integer                 ,да  ,Монтаж точек доступа. Факт (шт)
   ap\_percent               ,   ,integer                 ,да  ,Монтаж точек доступа. Процент выполнения (%)
   is\_overdue               ,   ,boolean                 ,да  ,Просрочена дата сдачи
   is\_month\_overdue        ,   ,boolean                 ,да  ,Просрочена дата сдачи более чем на месяц


Таблица хранения информации о задачах формирования видео файлов
---------------------------------------------------------------

Таблица содержит данные, необходимые для постановки задач формирования видео файлов процесса строительства.
Так же, эта таблица содержит результаты процесса обработки запросов и ссылки на сформированные файлы.

.. tabularcolumns:: |p{3.5cm}|p{1.6cm}|p{1.7cm}|p{0.7cm}|p{7.4cm}|
.. csv-table:: Структура таблицы video\_produce\_task
   :header: "Поле", "PK\/FK\/UN", "Тип", "Null", "Описание"

   id                        ,PK ,serial                  ,нет  ,Идентификатор
   name                      ,   ,character varying       ,нет  ,Название объекта строительства
   user\_id                  ,   ,integer                 ,нет  ,"Идентификатор пользователя, сформировавшего задачу"
   state                     ,   ,integer                 ,нет  ,Состояние задачи
   creation\_dt              ,   ,timestamp wtz           ,нет  ,Дата и время создания задачи
   update\_dt                ,   ,timestamp wtz           ,нет  ,Дата и время последнего обновления статуса задачи
   error\_text               ,   ,character varying       ,да   ,Текст ошибки
   resource\_id              ,   ,integer                 ,нет  ,Идентификатор ресурса. Родительская таблица public.resource
   lat\_center               ,   ,double precision        ,нет  ,Долгота центральной точки для записи видео
   lon\_center               ,   ,double precision        ,нет  ,Широта центральной точки для записи видео
   zoom                      ,   ,integer                 ,нет  ,Уровень масштаба карты для записи видео
   sound\_enabled            ,   ,boolean                 ,нет  ,Воспроизведение звука при записи видео
   photo\_enabled            ,   ,boolean                 ,нет  ,Отображение фото при записи видео
   units                     ,   ,character varying       ,нет  ,Единица измерения скорости воспроизведения процесса строительства
   units\_count              ,   ,integer                 ,нет  ,Скорость воспроизведения процесса строительства
   basemap                   ,   ,character varying       ,нет  ,Тип базовой карты для записи видео
   fileobj\_id               ,   ,integer                 ,да   ,Идентификатор сформированного файла
   file\_name                ,   ,character varying       ,да   ,Название сформированного файла
   file\_size                ,   ,bigint                  ,да   ,Размер сформированного файла
   file\_mime\_type          ,   ,character varying       ,да   ,MIME Type сформированного файла
   screen\_width             ,   ,integer                 ,нет  ,Ширина виртуального экрана для записи видео
   screen\_height            ,   ,integer                 ,нет  ,Высота виртуального экрана для записи видео



Таблица хранения информации о файлах музыкального сопровождения для записи видео
--------------------------------------------------------------------------------

Таблица содержит данные о файлах, загрженных администратором системы для звукового сопровождения видео файлов процесса строительства.

.. tabularcolumns:: |p{3.5cm}|p{1.6cm}|p{1.7cm}|p{0.7cm}|p{7.4cm}|
.. csv-table:: Структура таблицы video\_bg\_audio\_file
   :header: "Поле", "PK\/FK\/UN", "Тип", "Null", "Описание"

   id                        ,PK ,serial                  ,нет  ,Идентификатор
   fileobj\_id               ,   ,integer                 ,нет  ,Идентификатор файлового объекта
   file\_name                ,UN ,character varying       ,нет  ,Название звукового файла
   file\_size                ,   ,bigint                  ,да   ,Размер звукового файла
   file\_mime\_type          ,   ,character varying       ,да   ,MIME Type звукового файла
