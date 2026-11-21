# rcs-ads-mcp — Яндекс Директ, VK Реклама и Roistat в Claude

[Русский](#русский) · [English](#english) · [rcs.kz/product/rcs-ads-mcp](https://rcs.kz/product/rcs-ads-mcp) · [Релизы](https://github.com/RCS-kz/yandex-direct-vk-roistat-mcp/releases)

> Публичная витрина коммерческого продукта: README, релизы, журнал изменений. Исходный код закрыт.

## Русский

**Что это.** rcs-ads-mcp сводит Яндекс Директ и VK Рекламу с аналитикой Roistat и считает по каждой кампании деньги — ROMI, CPL, CPO и прибыль, а не клики.

### Установка в 3 шага (Claude Desktop)

1. Оставьте [заявку](https://rcs.kz/product/rcs-ads-mcp) — пришлём ключ на пробный период; пакет — `rcs-ads-mcp-<версия>.mcpb` из [релизов](https://github.com/RCS-kz/yandex-direct-vk-roistat-mcp/releases).
2. Откройте `.mcpb` в Claude Desktop.
3. В настройках расширения заполните только нужные площадки: OAuth-токен Яндекс Директа, client_id и секрет приложения VK Рекламы, ключ API и номер проекта Roistat, лицензионный ключ RCS.

Площадки подключаются по отдельности; без ключа сервер запускается и прямо пишет, какого поля не хватает.

**Облако.** Если у вас облачный Битрикс24, реклама работает в том же [облачном коннекторе](https://rcs.kz/product/oblachnyj-mcp): в браузере, на телефоне и в Claude Desktop, ничего не ставя. Ключи кабинетов администратор вставляет один раз по одноразовой ссылке, на сервере они хранятся зашифрованными; рекламу видят администраторы портала и отмеченные ими сотрудники. Без Битрикс24 — локальная версия.

### Что можно поручить

| Кто | Запрос |
|---|---|
| Владелец бизнеса | «Какие кампании за прошлую неделю сожгли деньги без продаж? Сколько сэкономит пауза?» |
| Маркетолог | «Сравни ROMI и стоимость заявки в Директе и VK за октябрь по кампаниям» |
| Маркетолог | «Подготовь недельный бриф: что выключить, где поднять ставки — в рублях эффекта» |
| Маркетолог | «Предложи перенос бюджета между Директом и VK с расчётом» — исполнение только по команде |
| Специалист по рекламе | «Добавь минус-слова из поисковых запросов с нулевыми продажами в кампанию …» — сначала план, потом подтверждение |
| Специалист по рекламе | «Откати вчерашнее изменение ставок» |

### Что внутри — 75 инструментов

| Группа | Инструментов | Что закрывает |
|---|---|---|
| Яндекс Директ | 31 | кампании, группы, объявления, фразы, ставки, минус-слова, отчёты, поисковые запросы, создание кампании целиком |
| VK Реклама | 22 | планы, группы, баннеры, статистика с событиями лид-форм, баланс, квота |
| Roistat | 7 | аналитика, лиды, каналы, расходы (включая выгрузку), паспорт проекта |
| Деньги | 10 | сводка, кампании, слитый бюджет, недельный бриф, сопоставление, ребалансинг |
| Журнал | 3 | список, карточка, откат изменения |
| Служебные | 2 | что настроено и статус лицензии, проверка площадок |

### Ни одной записи в кабинет молча

1. Первый вызов инструмента записи возвращает план: что изменится, у каких объектов, с каких значений на какие. Ничего не выполняется.
2. Вы подтверждаете.
3. Сервер делает снимок «было», выполняет запрос, делает снимок «стало» и пишет запись в журнал.
4. Любую запись можно откатить. Удалять объекты сервер не умеет вообще — максимум остановка кампании.

Локально журнал лежит у вас (`~/.rcs-ads-mcp/ads.db`) и никуда не отправляется. Токены не попадают в ответы и логи; названия кампаний и тексты объявлений помечаются для модели как данные, а не команды.

### Чем отличаемся (честно)

| | **rcs-ads-mcp (RCS)** | [@theyahia/roistat-mcp](https://github.com/theyahia/roistat-mcp) | [askads/mcp-yandex-direct](https://github.com/askads/mcp-yandex-direct) |
|---|---|---|---|
| Площадки | Директ + VK Реклама + Roistat в одном сервере | Roistat | Яндекс Директ |
| Инструментов | 75 | 6 (чтение: аналитика, визиты, лиды, каналы, расходы, интеграции) | 46 |
| Связь расхода с выручкой по кампании | да, слой «Деньги» | аналитика Roistat | нет |
| Запись в кабинет | план → подтверждение → журнал → откат | — | есть; подтверждение зависит от клиента, `raw_request` требует `confirmWrite=true` |
| Подключение | `.mcpb` в Claude Desktop; облако — вместе с Битрикс24 | npx (Claude Desktop, Claude Code), Streamable HTTP | удалённый сервер по URL (только чтение, вход через Яндекс) или локально через npx |
| Код и цена | закрытый, 14 900 ₸/мес | открытый (MIT), бесплатно | открытый (MIT), бесплатно; по умолчанию шлёт анонимную техническую телеметрию |

Кому что: нужен только Директ и быстро — askads (есть удалённый вход без токена); нужны только отчёты Roistat — theyahia/roistat-mcp; нужно видеть деньги по всем площадкам сразу и менять кампании с журналом и откатом — rcs-ads-mcp.

### Цены

| | Казахстан, с НДС 16 % | РФ, по инвойсу |
|---|---|---|
| **Маркетинг → деньги** (Директ + VK + Roistat) | 14 900 ₸/мес | 3 490 ₽/мес |
| **Всё вместе** (Битрикс24 + реклама + 1С) | 49 900 ₸/мес | 16 900 ₽/мес |

Один ключ на три площадки. Бесплатного тарифа нет, пробный период — по заявке, на ваших кабинетах. В облаке — та же цена, только при облачном Битрикс24. Для юрлица РФ покупатель — налоговый агент по НДС 22 %.

### Поддержка
go@rcs.kz · [rcs.kz/product/rcs-ads-mcp](https://rcs.kz/product/rcs-ads-mcp). RCS (ИП КВАНТ, Астана, Казахстан).

---

## English

**What it is.** rcs-ads-mcp joins Yandex Direct and VK Ads with Roistat analytics and shows money per campaign — ROMI, CPL, CPO and profit, not clicks.

### Install in 3 steps (Claude Desktop)

1. Request a trial key at [rcs.kz/product/rcs-ads-mcp](https://rcs.kz/product/rcs-ads-mcp); download `rcs-ads-mcp-<version>.mcpb` from [Releases](https://github.com/RCS-kz/yandex-direct-vk-roistat-mcp/releases).
2. Open the `.mcpb` in Claude Desktop.
3. Fill in only the platforms you use: Yandex Direct OAuth token, VK Ads app client_id/secret, Roistat API key and project number, RCS license key.

Hosted option: if you use Bitrix24 Cloud, the same tools run inside our [hosted connector](https://rcs.kz/product/oblachnyj-mcp) (browser, mobile, Claude Desktop).

### What you can ask

- "Which campaigns burned money with zero sales last week, and how much would pausing them save?"
- "Compare ROMI and cost per lead for Direct vs VK in October, by campaign."
- "Draft the weekly brief: what to pause and where to raise bids, ranked by money."
- "Propose a budget shift between Direct and VK with the expected effect." — runs only on your command.
- "Roll back yesterday's bid change."

75 tools: Yandex Direct 31, VK Ads 22, Roistat 7, money layer 10, journal 3, service 2. Every write returns a plan first, then runs after confirmation with before/after snapshots and a rollback-able journal; the server never deletes objects.

### How it compares

- **@theyahia/roistat-mcp** (MIT, free) — 6 read tools over the Roistat API.
- **askads/mcp-yandex-direct** (MIT, free) — 46 Yandex Direct tools; hosted read-only URL with Yandex sign-in, or full access locally via npx.
- **rcs-ads-mcp** (commercial) — three platforms in one server, revenue and profit per campaign from Roistat, cross-platform budget proposals, journal with rollback.

### Pricing

14 900 KZT/month incl. 16% VAT (3 490 RUB by invoice), one key for all three platforms. No free tier; trial on request. Support: go@rcs.kz.
