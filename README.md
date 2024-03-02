# Отказоустойчивые вычислительные системы

### Справка
Блок if __name__ == "__main__": позволяет изолировать код, который должен выполняться только при запуске скрипта напрямую из командной строки

Если файл скрипта запускается напрямую, то переменная __name__ равна __main__

Если же скрипт импортирован как модуль в другой скрипт, значение __name__ будет установлено равным имени модуля

### Конфигурация сервера
Файл конфигурации сервера содержит ip-адрес и порт, который прослушивает сервер после запуска, а также адреса и  порты резервных серверов, между которыми выполняется синхронизация данных (состояния игры)

Расширение файла конфигурации `json`. Структура файла:

{\
  "host": "< ip >",\
  "port": "< port >",\
  "reserve": ["< ip:port >", "< ip:port >..."]\
}

### Запуск

| Действие       | Команда                                       | Описание                                                                      |
|----------------|-----------------------------------------------|-------------------------------------------------------------------------------|
| Запуск сервера | `py .\test\server.py --config config/c1.json` | В качестве параметра `--config` указывается путь к файлу конфигурации сервера |
| Запуск клиента | `py .\test\client.py`                                              |                                                                               |

### Описание

Сервер выполняет следующие функции:

- Хранение состояния (прогресса) игры
- Рассылка состояния на резервные серверы, указанные в конфигурации - синхронизация данных
- Обработка запросов пользователей (клиентов) - получение запроса, валидация, отправка ответа

Клиент выполняет следующие функции:

- Управление ходом игры (подключение к серверу, отправка данных для изменения состояния (ход), ожидание хода)
- Переподключение. В случае возникновения ошибки подключения при выполнении запроса, клиенты переподключаются к первому доступному серверу из списка (полученного из конфигурации клиента)
