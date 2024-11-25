# Система управління транзакціями

## Опис програми
Програма представляє собою систему управління транзакціями з підтримкою фіатних та криптовалютних операцій. Реалізовано можливість створення, видалення та перегляду транзакцій через графічний інтерфейс. Система підтримує автоматичний розрахунок балансів та валідацію операцій.

## Стек технологій
- **Python 3.x**
- **tkinter** - для створення GUI
- **csv** - для роботи з файлами даних
- **datetime** - для роботи з датами
- **random, string** - для генерації унікальних ідентифікаторів

## Основні файли та класи

### 1. **gui.py**
- **TransactionApp**  
  - Головний клас GUI програми.  
  - Створює основне вікно програми.  
  - Управляє відображенням транзакцій.  
  - Обробляє взаємодію з користувачем.  

- **TransactionWindow**  
  - Клас для створення нових транзакцій.  
  - Забезпечує форму введення даних.  
  - Реалізує валідацію введених даних.  

### 2. **transaction_manager.py**
- **BookSingleton**  
  - Реалізує патерн Singleton для книг транзакцій.  
  - Управляє створенням та доступом до файлів транзакцій.  
  - Забезпечує єдину точку доступу до даних.  

- **TransactionManager**  
  - Основний клас управління транзакціями.  
  - Реалізує функції додавання/видалення записів, валідацію операцій.  
  - Взаємодіє з файловою системою.  

- **BalanceManager**  
  - Управляє балансами.  
  - Розраховує баланси.  
  - Перевіряє достатність коштів.  

- **Overview**  
  - Аналізує транзакції.  
  - Пошук останніх транзакцій.  
  - Отримання історії операцій.  

### 3. **dialogs.py**
- Містить допоміжні функції для створення діалогових вікон.  
- Обробляє взаємодію з користувачем при помилках та введенні даних.  

## Файли даних
- **fiat_book.csv** - зберігає фіатні транзакції.  
- **crypto_book.csv** - зберігає криптовалютні транзакції.  

## UML-діаграма класів
```mermaid
classDiagram
    class BookSingleton {
        -_instance_fiat_book
        -_instance_crypto_book
        -file_name: str
        +create_file_if_not_exists()
        +get_fiat_book()
        +get_crypto_book()
    }

    class TransactionManager {
        -book: BookSingleton
        -balance_manager: BalanceManager
        -transaction_validator: TransactionValidator
        -transaction_formatter: TransactionFormatter
        +create_transaction_manager(book_type)
        +add_transaction(transaction_data)
        +delete_transaction_by_id(transaction_id)
        -_validate_currency(book_type, currency)
        -_write_transaction(transaction_data)
    }

    class BalanceManager {
        -book: BookSingleton
        -overview: Overview
        +calculate_balances(amount, account_from, account_to, currency)
        -_get_last_balance(card, currency)
    }

    class TransactionValidator {
        -book: BookSingleton
        +generate_transaction_id()
        -_transaction_id_exists(transaction_id)
    }

    class TransactionFormatter {
        +configure_transactions(data, balance_to)
        +get_current_time()
    }

    class Overview {
        +find_last_transaction(filename, card_number, currency)
    }

    class TransactionApp {
        -book_singleton: BookSingleton
        -book_var: StringVar
        -transaction_tree: Treeview
        +run()
        -_create_widgets()
        -_show_transactions(book_type)
    }

    class TransactionWindow {
        -parent: TransactionApp
        -book_type: StringVar
        -currency_combobox: Combobox
        +_submit_transaction()
        -_update_currencies()
    }

    TransactionManager --> BookSingleton
    TransactionManager --> BalanceManager
    TransactionManager --> TransactionValidator
    TransactionManager --> TransactionFormatter
    BalanceManager --> Overview
    TransactionApp --> BookSingleton
    TransactionWindow <-- TransactionApp
    TransactionWindow --> TransactionManager
```
