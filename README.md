## Описание

Проект Sprint_2. Второй этап создания прототипа чата. Данный проект является учебным проектом YP.

Демонстрация работы данного прототипа: https://yp-7k-ivan-sprint-2.netlify.app/

Использованные инструменты:
Шаблонизатор: Handlebars.
Сборщик: Parcel.
Линт файлов: ESLint, StyleLint.

## Команды

- `npm run build` — сборка проекта,
- `npm run start` — запуск проекта на порту 3000,

## Описание структуры проекта

В корне находится пустой index.html, к которому подключен index.ts.

В соответствии с ТЗ, все элементы html страниц формируются из блоков, на основе класса Block. 
Введены три уровня организации блоков: страницы, панели, компоненты (соответствуют папкам pages, panels, components).

Для описания стилей выбран SCSS. Итоговый файл index.scss находится в папке static и является сборщиков всех scss-файлов
элементов проекта, а также выделенного для общих элементов стилей файла global.scss.

В папке common находятся общие для все блоков элементы, а именно:
- Классы Block и EventBus (block.ts, event_bus.ts) - базовые классы всех элементов.
- Утилиты (utils.ts) - общие функции, используемые различными элементами.
- Константы (constants.ts) - общие константы, используемые различными элементами.
- Класс для работы с запросами (xhr.ts).

Особенности реализации
- В index.ts я создал примитивный роутинг на основании пути входной ссылки.
  Роутинг не являлся частью этого задания, он сделан только для возможности демонстрации.
  Для работы на netlify добавлен редирект всех статически страниц обратно на index.html c параметром шаблона.

- Для реализации интерактивности каждой компоненты при сборке страниц, включающих компоненты меньшего уровня 
  я создал специальную функцию ConstructDomTree (utils.ts). Внутри шаблона я делаю псевдо-узлы "node" с id, 
  которые соответствуют компонентам. Handlebars ничего об этом не знает, поэтому просто их оставляет как есть.
  После того как он возвращает строку я перевожу ее в DOM дерево, в котором ищу мои псевдо-узлы и заменяю 
  их на компоненты. Соответственно render возвращает обратно в block не строку, а DOM структуру.
   
- Для реализации всех страниц, собирающих или демонстрирующих поля с информацией (вход в систему, регистрация, 
  параметры пользователя, изменение данных, изменение пароля) создан один тип страницы - form. Этот тип принимает
  набор полей и кнопок как параметры, после чего формирует страницу. В рамках этой страницы реализован единый механизм
  валидации данных, на основе констант регулярных выражений (constants.ts)

- В ТЗ не было указано, что именно нужно проверять при валидации всех форм. Мною были выбраны тестовые параметры 
  их можно легко изменить, изменив соответствующие регулярные выражения и пояснения к ним (constants.ts):

- В ТЗ не было указано, с какой целью необходимо встроить в проект класс для работы с запросами (xhr.ts). В целях
  демонстрации его работы в index.ts спустя 1 секунду выполняется запрос к статическому содержанию текущей страницы, 
  ответ которого помещается во временный div класса proxy_test.

Особенности контроля корректности кода файлов:
- ESlint. "semi": ["error", "always"], "dot-notation": "off" - это вопрос привычки, наставник сказал, что я могу 
  настраивать это так, как мне привычно.
  
- StyleLit. "at-rule-no-unknown": null - Lint не понимает импортируемых из других файлов фрагментов. Например:
        @import "../../../static/global";
        
        %input_view {
            @extend %text_view;
        
            padding-left: 10px;
            text-align: left;
        }
  В global есть фрагмент %text_view, однако линт не видит этого и выдает ошибки. Пришлось отключить правило.
