# Библиотека для часов реального времени, RTC (Trema-модуль)

http://iarduino.ru/shop/Expansion-payments/chasy-realnogo-vremeni-rtc-trema-modul.html

###ОПИСАНИЯ ПАРАМЕТРОВ ФУНКЦИЙ:

в библиотеке реализованы 4 функции: begin, settime, gettime и period.
функция begin вызывается первой и предназначена для инициализации модуля, выбора шины, и выводов arduino
количество и порядок вызовов функций settime, gettime и period, значения не имеет.

####инициализация модуля
```С

time.begin(название, RST или SS, CLK, DAT) если модуль работает на шине I2C или SPI, то достаточно указать только название модуля, например: time.begin(RTC_DS3231);
если модуль работает на шине SPI, а аппаратный вывод SS занят, то номер назначенного вывода SS для модуля указывается вторым параметром, например: time.begin(RTC_DS1305,22);
если модуль работает на трехпроводной шине, то указываются номера всех выводов, например: time.begin(RTC_DS1302, 1, 2, 3); // RST, CLK, DAT
запись даты и времени

time.settime(сек,мин,час,д,м,г,дн)       должен присутствовать хотябы 1 параметр
часы указываются в 24-часовом формате, год указывается от 0 до 99, день недели указывается числом: 1-ПН, 2-ВТ, ... 6-СБ, 0-ВС.
если предыдущий(ие) параметр(ы) надо оставить без изменений, то указывается отрицательное значение
чтение даты и времени

time.gettime("строка с параметрами")       функция получает и выводит строку заменяя описанные ниже символы на текущее время
указанные символы эдентичны символам для функции date() в PHP

s  секунды                       от      00    до       59  (два знака)
i  минуты                        от      00    до       59  (два знака)
h  часы в 12-часовом формате     от      01    до       12  (два знака)
H  часы в 24-часовом формате     от      00    до       23  (два знака)
d  день месяца                   от      01    до       31  (два знака)
w  день недели                   от       0    до        6  (один знак: 0-воскресенье, 6-суббота)
D  день недели наименование      от     Mon    до      Sun  (три знака: Mon Tue Wed Thu Fri Sat Sun)
m  месяц                         от      01    до       12  (два знака)
M  месяц наименование            от     Jan    до      Dec  (три знака: Jan Feb Mar Apr May Jun Jul Aug Sep Oct Nov Dec)
Y  год                           от    2000    до     2099  (четыре знака)
y  год                           от      00    до       99  (два знака)
a  полдень                               am   или       pm  (два знака, в нижнем регистре)
A  полдень                               AM   или       PM  (два знака, в верхнем регистре)
 строка не должна превышать 50 символов
 пример: gettime("d-m-Y, H:i:s, D"); ответит строкой "01-10-2015, 14:00:05, Thu"
   пример: gettime("s");               ответит строкой "05"
    чтение даты и времени в виде цифр
    time.gettime()   без параметра
   результат читается из переменных:
time.seconds  секунды     0-59
time.minutes  минуты      0-59
time.hours    часы        1-12
time.Hours    часы        0-23
time.midday   полдень     0-1 (0-am, 1-pm)
time.day      день месяца 1-31
time.weekday  день недели 0-6 (1-понедельник, 6-суббота, 0-воскресенье)
time.month    месяц       1-12
time.year     год         0-99
    не обязательная функция
    time.period(минуты)   указывает минимальный период чтения времени из модуля в минутах. (от 1 до 255 минут)
   пример:
   если после вызова функции time.period(5); в течении 5 минут несколько раз была вызвана функция gettime,
   то запрос времени к модулю пройдёт только в первый раз, а ответом на все остальные вызовы функции gettime
   будет результат последнего полученного от модуля времени + время прошедшее с этого запроса
   уменьшение числа запросов уменьшает время обработки запросов, разгружает шину, и уменьшает потребляемый модулем ток
   при отсутствии вызова функции time.period(минуты), ответ на запрос даты и времени будет всегда читаться из модуля

```
