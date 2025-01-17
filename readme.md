# Sprut-db-json

Инструмент для быстрого подключения базы данных с возможностью поиска, добавления и удаления одного или коллекции элементов(строк таблиц). Удобно при прототипировании. В планах разнообразить доступные запросы к таблицам.

Важно осторожное использование, с написанием адаптера к основному интерфейсу(важно его наличие), дабы перепрыг на нормальную базу был безболезненным.

## Инициализация пространства:

```js
const { DataSpace } = require('sprut-db-json');
const db = new DateSpace('./', 'db', {
    saveTimeout: 1000
});
```

## Аргументы конструктора ```DataSpace```:

```path: string``` - директория расположения файла данных, может быть относительным путём (относительно директории запущенного процесса)

```fileName: string``` - имя файла данных (будет создан файл ```${fileName}.json``` или прочитан, если он уже существует)

```config``` - конфига создаваемого пространства

## Поля конфига:

```js
{
    saveTimeout: number;
}
```

```saveTimeout``` - настройка, при положительном значении которой данные будут сохраняться в файл, если они были обновлениы с помощью метода ```.change()``` экземпляра ```DataSpace```

## Интерфейс класса DataSpace, методы и свойства:

```createTable: (table: string, config: TableConfig) => void``` - Создание таблицы под именем ```table``` для дальнейшего обращения к ней с помощью этого имени

```save: (isForce: boolean) => void``` - Сохранение изменений в файл, сохранение произойдёт, если были внесены изменения, или форсированно, если ```isForce == true```

```add: (table: string, data: any) => any``` - Добавление нового элемента ```data``` в таблицу ```table```, возвращает ```any```, который является изменённой копией ```data``` (добавляется поле ```id```, если для таблицы был указан ```idMethod``` конфига при её создании)

```getOne: (table: string, fieldValues: any, optionalFieldValues: any) => any | null``` - Возвращает первый попавшийся элемент в таблице ```table```, отбираемого посредством обязательного сходства с полями ```fieldValues```, и опионального сходства с полями ```optionalFeildValues```, либо возвращает ```null```

```getItems: (table: string, fieldValues: any, optionalFieldValues: any) => any[]``` - Возвращает массив элементов из таблицы ```table```, отбираемых посредством обязательного сходства с полями ```fieldValues```, и опионального сходства с полями ```optionalFeildValues```

```removeOne: (table: string, fieldValues: any) => void``` - Удаление первого попавшегося элемента из таблицы ```table```, отбираемого посредством сходства с полями ```fieldValues```, как при поиске

```removeItems: (table: string, fieldValues: any) => void``` - Удаление всех элементов из таблицы ```table```, отбираемых посредством сходства с полями ```fieldValues```, как при поиске

```change: (table: string, fieldValues: any, newValues: any, isFullChange: boolean) => OperationStatus``` - Изменяет единственный попавшийся элемент, отбираемый посредством сходства с полями ```fieldValues```, ```newValues``` не должен содержать поле ```id```, если для талицы указан ```idMethod``` при её создании, добавляет значения полей ```newValues``` (если ```isFullChange``` !== true) или полностью заменяет элемент (если ```isFullChange``` === true), возвращает ```OperationStatus``` с ```success === true```, если найден единственный элемент, иначе ```OperationStatus``` с ```success === false```

```getTable: (table: string) => any[]``` - Возвращает полную копию таблицы ```table```

```removeTable: (table: string) => void``` - Удаление таблицы ```table``` из пространства

## Интерфейс объекта TableConfig:

```idMethod: string``` - Необязательный параметр, также как и конфиг, из доступных вариантов пока только значение ```increament``` (то есть последовательный прирост для поля ```id``` добавляемых элементов)

## Интерфейс класса OperationStatus:

```success: boolean``` - Успешность операции

```error: string | null``` - Строка ошибки, если та обнаружена

```code: number``` - Код завершения операции