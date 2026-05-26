# 💱 Currency Converter

Конвертер валют — FastAPI backend + PyQt5 GUI клиент.

## Поддерживаемые валюты

| Код | Название |
|-----|----------|
| USD | Доллар США |
| EUR | Евро |
| RUB | Российский рубль |
| GBP | Британский фунт |
| JPY | Японская иена |
| CNY | Китайский юань |

## Структура проекта

```
currency-converter/
├── server/
│   ├── main.py          # FastAPI-сервер
│   └── requirements.txt
├── client/
│   ├── main.py          # PyQt5-клиент
│   └── requirements.txt
├── requirements.txt     # все зависимости разом
└── README.md
```

## Запуск

### 1. Установка зависимостей

```bash
pip install -r requirements.txt
```

### 2. Запуск сервера

```bash
cd currency-converter
uvicorn server.main:app --reload --host 127.0.0.1 --port 8000
```

Swagger UI доступен по адресу: http://127.0.0.1:8000/docs

### 3. Запуск клиента (в отдельном терминале)

```bash
cd currency-converter
python client/main.py
```

## API-эндпоинты

### `GET /currencies`
Возвращает список поддерживаемых валют и текущие курсы.

### `POST /convert`
Конвертирует сумму. Тело запроса:
```json
{
  "amount": 1000,
  "from_currency": "USD",
  "to_currency": "EUR"
}
```

Ответ:
```json
{
  "from_currency": "USD",
  "to_currency": "EUR",
  "original_amount": 1000.0,
  "converted_amount": 920.0,
  "rate": 0.92,
  "rates_source": "open.er-api.com"
}
```

### Ошибки (HTTP 400)
- Отрицательная сумма → `"Сумма не может быть отрицательной: -10"`
- Неизвестная валюта → `"Неизвестная валюта: «XYZ». Поддерживаются: CNY, EUR, GBP, JPY, RUB, USD"`

### `GET /health`
Проверка доступности сервера.

## Источник курсов

Приложение пытается получить актуальные курсы с **open.er-api.com** (бесплатный публичный API, без ключа).  
Курсы кэшируются на 5 минут. Если API недоступен — используются фиксированные запасные курсы.
