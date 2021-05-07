# tinkproject
The program for portfolio analysis via Tinkoff API

Hello! This is my first project, please, don't judge me. This project was developed for Tinkoff Investments' clients, like me, to gether accurate and full information about portfolio into Excel table.

Здравствуйте! Это мой первый проект, пожалуйста не судите строго. Этот проект был разработан для клиентов Тинькофф Инвестиций, чтобы собирать подробную информацию об инвестиционном портфеле в таблицу Excel.

Статья на Хабре: https://habr.com/en/post/555884/

Для автоматического сбора информации используется Tinkoff API. Для этого вам нужно получить его в личном кабинете Тинькофф. Далее вам нужно отредактировать файлик my_account.txt следующим образом:

Ваш API ключ
Год начала инвестирования
Месяц начала инвестирования
День начала инвестирования
Программа будет собирать данные за указанный вами период. Для работы программы все файлики программы: main.py, excel_builder.py, data_parser.py, my_account.txt - должны находиться в одной папке. В этой же папке появится файлик Excel с результатом.

Также, для работы программы на вашем компьютере должен быть установлен Python 3.9.1

Запускается программа файликом main.py или через `docker`:  
```
docker build -t tinkproject .
docker run --rm -it -v $(pwd):/app tinkproject
```

Программа может формировать отчёт достаточно долго, в зависимости от объёма портфеля и количества совершённых операций.

Полученный отчёт включает слудующие разделы:

Текущий портфель
<рыночные цены>

Market today rates: курсы валют по бирже

name - название актива

ticker - тикер

balance - количество бумаг в портфеле

currency - валюта, в которой бумага торгуется

ave.price - средняя цена покупки бумаги

exp.yield - ваша потенциальная выручка при полной продаже данного актива

market price - текущая рыночная стоимость одной бумаги данного актива

%change - процент изменения стоимости по сравнению со средней стоимостью покупки

market value - текущая рыночная стоимость всего актива

market value RUB - текущая рыночная стоимость всего актива в рублях по рыночному курсу соответсвующей валюты

ave.% - средний процент изменения стоимости всех бумаг

total value - общая рыночная стоимость всего портфеля в рублях

<перерасчёт по курсу ЦБ>

CB value RUB - стоимость актива в рублях по сегодняшнему курсу ЦБ

ave.buy in RUB - средняя цена покупки одной бумаги в рублях по курсу ЦБ на дату покупки

sum.buy in RUB - общая стоимость преобретения актива в пересчёте на рубли по курсу ЦБ на дату покупки

tax base - разница, облагаемая налогом

expected tax - предполагаемая сумма удерживаемого налога по ставке 13%, если актив будет продан сегодня

Списки всех операций
Здесь всё стандартно, сами переведёте, если что
Небольшой анализ
Investing period - анализируемый период инвестирования, начниая с даты, которую вы указывали в текстовом файле
PayIn-PayOut - сумма всех внесённых на счёт средств (в рублях по курсу ЦБ на дату внесения) минус сумма всех выводов средств.
Commissions payed - сумма всех комиссий, удержанных брокером
Taxes payed- сумма всех налогов, удержанных брокером
Clean portfolio - рыночная стоимость всего портфеля за вычетом предполагаемой суммы налога, которая будет удержана (комиссия брокера не учетывается!)
Profit - это Clean portfolio минус PayIn-PayOut

Ошибки:
- если в портфеле есть бумага, недавно приобретённая на IPO
