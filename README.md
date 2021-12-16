# tinkproject

The program for portfolio analysis via Tinkoff API

Hello! This is my first project, please, don't judge me. This project was developed for Tinkoff Investments' clients, like me, to gather accurate and full information about portfolio into Excel table.

Здравствуйте! Это мой первый проект, пожалуйста не судите строго. Этот проект был разработан для клиентов Тинькофф Инвестиций, чтобы собирать подробную информацию об инвестиционном портфеле в таблицу Excel.

Статья на Хабре: [Что недоговаривают Тинькофф Инвестиции. Вытаскиваем все данные по портфелю через API в большую таблицу Excel](https://habr.com/en/post/555884/)

Для автоматического сбора информации используется Tinkoff API. Для использования вам нужно получить *токен для OpenAPI* в личном кабинете Тинькофф.

## Параметры доступа

Далее вам нужно СОЗДАТЬ файл `my_account.txt` со следующим содержимым построчно:

```text
Ваш Токен для OpenAPI
Временная зона, например: Asia/Yekaterinburg
Год начала инвестирования
Месяц начала инвестирования
День начала инвестирования
```

Подробнее о файле настроек скрипта можно почитать в разедел [Файл настроек](docs/configuration.md).

Программа будет собирать данные за указанный вами период по всем брокерским счетам (включая ИИС). Для работы программы все файлы программы: `main.py`, `excel_builder.py`, `data_parser.py`, `my_account.txt` - должны находиться в одной папке. В этой же папке появятся файлы Excel с результатом (один на каждый счёт).

## Запуск

Для работы программы на вашем компьютере должен быть установлен Python 3.9

Запускается программа командой `python main.py` или через `docker`:
```
docker build -t tinkproject .
docker run --rm -it -v $(pwd):/app tinkproject
```

Чтобы не печатать логи, при запуске через командную строку добавьте опцию `-q` или `--quiet`.

Наоборот, для отладки добавьте `-d` или `--debug`.

Внимание: бумаги, полученные в подарок, например за приведённого друга, могут не выдаваться через API и в отчёте они тоже будут отсутствовать. Таким образом, если есть подаренные бумаги, итоговый баланс портфеля будет отличаться от того, который в приложении Тинькофф.

Бумаги, полученные в результате дробления или, возможно, каких-то других корпоративных событий, в отчёте могут частично отображаться нулевыми значениями.

## Результат

Полученный отчёт включает следующие разделы:

### Portfolio - текущий портфель

#### <рыночные цены>

**Market today rates**: курсы валют по бирже

**name** - название актива

**ticker** - тикер

**balance** - количество бумаг в портфеле

**currency** - валюта, в которой бумага торгуется

**ave.price** - средняя цена покупки бумаги

**exp.yield** - ваша потенциальная выручка при полной продаже данного актива

**market price** - текущая рыночная стоимость одной бумаги данного актива

**%change** - процент изменения стоимости по сравнению со средней стоимостью покупки

**market value** - текущая рыночная стоимость всего актива

**market value RUB** - текущая рыночная стоимость всего актива в рублях по рыночному курсу соответсвующей валюты

**ave.%** - средний процент изменения стоимости всех бумаг

**total value** - общая рыночная стоимость всего портфеля в рублях

#### <перерасчёт по курсу ЦБ>

**CB value RUB** - стоимость актива в рублях по сегодняшнему курсу ЦБ

**ave.buy in RUB** - средняя цена покупки одной бумаги в рублях по курсу ЦБ на дату покупки

**sum.buy in RUB** - общая стоимость преобретения актива в пересчёте на рубли по курсу ЦБ на дату покупки

**tax base** - налоговая база - разница, облагаемая налогом

**expected tax** - предполагаемая сумма удерживаемого налога по ставке 13%, если актив будет продан сегодня

#### Небольшой анализ

**Investing period** - анализируемый период инвестирования, начниая с даты, которую вы указывали в текстовом файле

**PayIn-PayOut** - сумма всех внесённых на счёт средств (в рублях по курсу ЦБ на дату внесения) минус сумма всех выводов средств.

**Commissions payed** - сумма всех комиссий, удержанных брокером

**Taxes payed** - сумма всех налогов, удержанных брокером

**Clean portfolio** - рыночная стоимость всего портфеля за вычетом предполагаемой суммы налога, которая будет удержана (комиссия брокера не учитывается!)

**Profit** - это Clean portfolio минус PayIn-PayOut

**XIRR** - так называется формула Excel для расчёта эффективности инвестиций с учётом всех пополнений и выводов (irregular internal rate of return). Считается в процентах годовых.

### Operations - списки всех операций

Здесь всё стандартно, сами переведёте, если что

### Coupons and Dividends

Список выплаченных купонов и дивидендов. Так же отображаются налоги, удержанные при выплате.

Для счетов ИИС - табличка расчета объема налогового вычета по годам.

### Parts

Структура портфеля по валютам и типам активов. Представлено в подробном и сокращенном вариантах, а так же в графическом представлении.

## Ограничения, связанные с особенностями API Тинькофф банка

* Выводятся только RUB, USD, EUR. Остальные валюты получить на данный момент невозможно. (https://github.com/TinkoffCreditSystems/invest-openapi/issues/478)
* Только биржевые бумаги. Внебиржевые активы не выводятся. (https://github.com/TinkoffCreditSystems/invest-openapi/issues/331)
