# Uspacy import from file

Цей скрипт оновлює сутності в Uspacy на основі CSV/XLSX файлу. Скрипт працює послідовно по одному рядку, що підходить для файлів 20–30 тисяч записів.

## Вимоги

```bash
pip install -r requirements.txt
```

## Покроково для новачка (приклад для Linux/macOS)

1. Відкрийте термінал.
2. Перейдіть у папку, де хочете зберігати скрипт:

```bash
cd ~/i.sydorenko/codex/uspacy_import
```

> Якщо папки ще немає — створіть її:

```bash
mkdir -p ~/i.sydorenko/codex/uspacy_import
cd ~/i.sydorenko/codex/uspacy_import
```

3. Покладіть в цю папку:
   - файл `uspacy_import.py`
   - файл `requirements.txt`
   - ваш файл імпорту, наприклад `data.xlsx` або `data.csv`
4. Встановіть залежності:

```bash
pip install -r requirements.txt
```

5. Запустіть скрипт:

```bash
python uspacy_import.py \
  --base-url https://{domain}.uspacy.ua \
  --entity companies \
  --file data.xlsx \
  --search-field cod_1c \
  --webhook-token "YOUR_WEBHOOK_TOKEN"
```

## Формат файлу

- Перший рядок містить **id полів** в Uspacy.
- Перша колонка містить поле, по якому шукаємо сутність (наприклад `cod_1c`).
- Подальші колонки — поля для оновлення.

Приклад CSV:

```csv
cod_1c,oblast,region
0001,Вінницька,Центр
```

## Запуск

```bash
python uspacy_import.py \
  --base-url https://{domain}.uspacy.ua \
  --entity companies \
  --file data.xlsx \
  --search-field cod_1c \
  --webhook-token "YOUR_WEBHOOK_TOKEN"
```

Також можна передати токен через змінну середовища:

```bash
export USPACY_WEBHOOK_TOKEN="YOUR_WEBHOOK_TOKEN"
python uspacy_import.py --base-url https://{domain}.uspacy.ua --entity companies --file data.csv
```

## Як обробляються list-поля

Скрипт робить запит:

```
GET /crm/v1/entities/{entity}/fields
```

Якщо поле типу `list`, значення з файлу співставляються за `title`, а в запит `PATCH` передається відповідний `value`.

## Dry-run

```bash
python uspacy_import.py --base-url https://{domain}.uspacy.ua --entity companies --file data.csv --dry-run
```

Dry-run показує, що буде оновлено, без фактичного `PATCH`.
