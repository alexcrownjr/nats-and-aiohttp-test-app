Нужно сделать сервис, получающий сводную информацию о рекламных кампаниях.
У сервиса есть API с одним методом `/info/{account}`
Задача сервиса запросить из трёх других сервисов (сервис статистики, сервис рекламных кампаний, сервис тегов) данные и объединить их в одну структуру.
Предполагается, что запросов к сервису достаточно много и обрабатывать их он должен быстро.
У сервисов статистики, кампаний и тегов есть ограничение на количество запросов. 1 запрос в 1 секунду.
При достижении лимита, нужно взять данные из кеша.

1) Сервис кампаний
У сервиса есть API с одним методом /campaigns/{account}
Возвращает массив `[{"id": 123, "title": "Campaign 1}, {"id": 124, "title": "Campaign 2},
...]`

2) Сервис статистики
У сервиса есть API с одним методом /get_stat/{account}
Возвращает массив `[{"campaign_id": 123, "date": "2018-05-24", "shows": 500, "clicks":
100, "costs": 26}, {"campaign_id": 124, "date": "2018-05-24", "shows": 400, "clicks": 50,
"costs": 13}, ...]`

3) Сервис тегов
У сервиса есть API с одним методом /tags/{account}
Возвращает массив [{"campaign_id": 123, "tags": `["tag 1", "tag 2"]}, {"campaign_id":
124, "tags": ["tag 3", "tag 4"]}, ...]`

Сервис должен возвращать структуру вида
`[
{“id”: 123, “title”: “Campaign 1”, “get_stat”: [{“date”: “2018-05-24”, “shows”: 500, “clicks”: 100,
“costs”: 26}, …], “tags”: [“tag 1”, “tag 2”]}`