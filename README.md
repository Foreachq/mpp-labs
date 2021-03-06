# MPP-labs
Modern Programming Platforms labs

#### Lab 1 - Tracer
Необходимо реализовать измеритель времени выполнения методов.

Tracer должен собирать следующую информацию об измеряемом методе:
- имя метода;
- имя класса с измеряемым методом;
- время выполнения метода.

Также должно подсчитываться общее время выполнения анализируемых методов в одном потоке (для этого достаточно подсчитать сумму времен "корневых" методов, вызванных из потока).

Результаты трассировки вложенных методов должны быть представлены в соответствующем месте в дереве результатов.

Результат измерений должен быть представлен в двух форматах: JSON и XML (для классов, реализующих сериализацию в данные форматы, необходимо разработать общий интерфейс).

Готовый результат (полученный JSON и XML) должен выводиться в консоль и записываться в файл. Для данных классов необходимо разработать общий интерфейс, допустимо создать один переиспользуемый класс, не зависящий от того, куда должен выводиться результат.

#### Lab 2 - Faker
Необходимо реализовать генератор объектов со случайными тестовыми данными.

При создании объекта следует использовать конструктор, а также заполнять публичные поля и свойства с публичными сеттерами, которые не были заполнены в конструкторе. Следует учитывать сценарии, когда у класса только приватный конструктор, несколько конструкторов, конструктор с параметрами и публичные поля/свойства. 

При наличии нескольких конструкторов следует отдавать предпочтение конструктору с большим числом параметров, однако если при попытке его использования возникло исключение, следует пытаться использовать остальные. 

Заполнение должно быть рекурсивным (если полем является другой объект, то он также должен быть создан с помощью Faker).

Реализовать генераторы случайных значений для базовых типов-значений (int, long, double, float, etc), строк, одного любого системного класса для представления определенного типа данных на выбор (дата/время, url, etc), коллекций объектов всех типов, которые могут быть сгенерированы Faker (поддержка разновидностей IEnumerable<T>, List<T>, IList<T>, ICollection<T>, T[] на усмотрение автора, минимум один вариант из приведенных).

Создание коллекций должно выполняться аналогично созданию других типов, для которых есть генераторы. Внутри кода Faker не должно быть проверок if/switch на коллекцию и какой-то особой обработки. Допустим и любой другой способ, согласующийся с требованием об отсутствии специальных проверок для коллекций.

Выделить как минимум 2 предопределенных генератора в отдельные подключаемые модули (плагины), которые будут загружаться на старте приложения.

Предусмотреть учет циклических зависимостей.

#### Lab 3 - Assembly Browser
Необходимо реализовать графическую утилиту с использованием WPF для просмотра информации о составе произвольной .NET сборки. 

Содержимое загруженной сборки должно быть представлено в иерархическом виде (элемент управления TreeView):
- пространства имен; 
  - типы данных; 
    - поля, свойства и методы (информация о методах помимо имени должна включать сигнатуру, о полях и свойствах - тип).

Обязательным является применение MVVM, а также использование INotifyPropertyChanged и ICommand.

#### Lab 4 - Tests Generator
Необходимо реализовать многопоточный генератор шаблонного кода тестовых классов для одной из библиотек для тестирования (NUnit, xUnit, MSTest) по тестируемым классам.

Входные данные: 
- список файлов, для классов из которых необходимо сгенерировать тестовые классы;
- путь к папке для записи созданных файлов;
- ограничения на секции конвейера  (см. далее).

Выходные данные:
- файлы с тестовыми классами (по одному тестовому классу на файл, вне зависимости от того, как были расположены тестируемые классы в исходных файлах);
- все сгенерированные тестовые классы должны компилироваться при включении в отдельный проект, в котором имеется ссылка на проект с тестируемыми классами;
- все сгенерированные тесты должны завершаться с ошибкой.

Генерация должна выполняться в конвейерном режиме "производитель-потребитель" и состоять из трех этапов: 
- параллельная загрузка исходных текстов в память (с ограничением количества файлов, загружаемых за раз);
- генерация тестовых классов в многопоточном режиме (с ограничением максимального количества одновременно обрабатываемых задач); 
- параллельная запись результатов на диск (с ограничением количества одновременно записываемых файлов).

При реализации использовать async/await и асинхронный API. Для реализации конвейера можно использовать Dataflow API.

Необходимо сгенерировать по одному пустому тесту на каждый публичный метод тестируемого класса.

Для синтаксического разбора и генерации исходного кода следует использовать Roslyn.

#### Lab 5 - Dependency Injection Container
Необходимо реализовать простой Dependency Injection контейнер.

Контейнер должен позволять регистрировать зависимости в формате: Тип интерфейса (TDependency) -> Тип реализации (TImplementation), где TDependency — любой ссылочный тип данных, а TImplementation — не абстрактный класс, совместимый с TDependency, объект которого может быть создан.

Внедрение зависимостей должно осуществляться через конструктор. Создание зависимостей должно выполняться рекурсивно, то есть если TImplementation имеет свои зависимости, а каждая из его зависимостей — свои (и т. д.), то контейнер должен создать их все.
Контейнер должен быть отделен от своей конфигурации: сначала выполняется создание конфигурации и регистрация в нее зависимостей, а затем создание на ее основе контейнера.

Необходимо реализовать два варианта времени жизни зависимостей (задается при регистрации зависимости): 
- instance per dependency — каждый новый запрос зависимости из контейнера приводит к созданию нового объекта;
- singleton — на все запросы зависимостей возвращается один экземпляр объекта (следует учитывать параллельные запросы в многопоточной среде).

Необходимо учитывать ситуацию наличия нескольких реализаций для одной зависимости и предусмотреть способ получения сразу всех реализаций.
