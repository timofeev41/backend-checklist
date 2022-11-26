## Random Picker Bot

Бывали ли у вас случаи, когда нужно сделать выбор из нескольких вариантов?
Конечно бывали, поэтому предлагаю написать бота который упростит вам жизнь, делая выбор за вас!


Бот должен уметь (в MVP):

- Добавлять варианты пользователя (команда /add <вариант>)
- Удалять варианты (команда /remove <номер варианта>)
- Выбирать из вариантов случайный и выводить его (команда /choice)
- Вести статистику (/stats), те запоминать как часто выпадает какой либо вариант

На первом этапе хранить значения можно в обычном списке

```python
vars: list[str] = []

@bot
def add_variant(message):
    vars.append(message.text)


@bot
def remove_variant(message)
    vars.pop(int(message.text))


@bot
def show_variants(message):
    send_message(message.sender_id)


@bot
def pick_variant(message):
    # Для ведения статистики можно предусмотреть отдельный словарь!
    send_message(random.choice(vars))
```
