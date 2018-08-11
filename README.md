

#  PHP-YANDEX-ALISA

Библиотека для работы с Алисой.
Пример блоковой системы находиться в папке blocks.
___

##  Содержание

 1. [TODO](#todo)
 2. [Установка](#%D0%A3%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0)
 3. [Описание констант](#%D0%9E%D0%BF%D0%B8%D1%81%D0%B0%D0%BD%D0%B8%D0%B5-%D0%BA%D0%BE%D0%BD%D1%81%D1%82%D0%B0%D0%BD%D1%82)
 4. [Описание переменных](#%D0%9E%D0%BF%D0%B8%D1%81%D0%B0%D0%BD%D0%B8%D0%B5-%D0%BF%D0%B5%D1%80%D0%B5%D0%BC%D0%B5%D0%BD%D0%BD%D1%8B%D1%85)
 5. [Описание методов](#%D0%9E%D0%BF%D0%B8%D1%81%D0%B0%D0%BD%D0%B8%D0%B5-%D0%BC%D0%B5%D1%82%D0%BE%D0%B4%D0%BE%D0%B2)
 6. [Система JSON-блоков](#Система-json-блоков)
	+ [Что это такое?](#Что-это-такое)
	+ [Файлы обработки](#Файлы-обработки)
	+ [Описание Action](#Описание-action) 
	  + [Простой пример](#Простой-пример)
	  + [Пример с вариацией сообщений](#Пример-с-вариацией-сообщений)
	  + [Пример с добавлением кнопки](#Пример-с-добавлением-кнопки)
	  + [Пример с подготовленные запросы](#Пример-с-подготовленные-запросы)
	  + [Группировка prepare и question](#Группировка-prepare-и-question)
	  + [Пример кода с кнопками и payload](#Пример-кода-с-кнопками-и-payload)
	  + [Пример сложного JSON-блока](#Пример-сложного-json-блока)
	+ [Описание Payload](#Описание-payload)
	  + [Пример запроса](#Пример-запроса)
	  + [Форматирование текста](#Форматирование-текста)
 7. [Консоль Алиса](#Консоль-Алиса)
      + [Получить загруженные файлы](#php-alisa-uploadget)
      + [Загрузить с сервера](#php-alisa-uploadfile-file_name)
      + [Загрузить с сайта](#php-alisa-uploadurl-url)
 9. [index.php](#indexphp)
 10. [setting.yml](#settingyml)
 11. [Локальный Webhook.](#%D0%9B%D0%BE%D0%BA%D0%B0%D0%BB%D1%8C%D0%BD%D1%8B%D0%B9-webhook)

___
## TODO
|Дата|Описание релиза |Состояние 
|:--:|--|--|
| 5.08.2018 |Теперь можно делать вариации вопросов и ответов.  |**done**
| 5.08.2018 |Выполнять payload-функции (callback).  |**done**
| 5.08.2018 |Проверка на орфографию.  |**done**
| 6.08.2018 |Поддержка подготовленных запросов.  |**done**
| 8.08.2018 |Реализация созданий навыков при помощи JSON-блоков.   |**done**
| 11.08.2018 |Отправка сообщений с фотографиями.  |**done 80%**
|---|Оплата при помощи компании Unitpay. |**in plan**

## Установка
  ``` 
composer require danil005/php-yandex-alisa
 ```
## Описание констант
### SKILL_NAME 
Название навыка.
___
### VERSION
Версия Алиса API. По умолчанию: 1.0
## Описание переменных
### private $startMessage = ""
Сообщение, которое будет отображаться при старте навыка.
___
### private $startMessageTTS = ""
Сообщение, которое будет озвучено синтезом речи Yandex.
___
### private $startButton = []
Кнопки, которые будут отображены при запуске навыка
___
### private $version = self::VERSION
Наследует константу VERSION.
___
### private $anyMessage  = "Простите, я вас не понимаю." 
Сообщение, которое будет отправлено пользователю в случае, если команда не будет найдена.
___
### private $caseSensitive = true
Чувствительность к регистру сообщений.
___
### private $speller = false
Проверка на орфографию.
___
### public $vars = []
Переменная для обработки [Prepare-функции](#public-preparestring-getmessage-string-command-bool).
___
### private $request = []
Переменная, которая получает ответ от Алисы.
___
### public $imagesDir = "images"
Директория с изображениями.
___
### public $response = stdObject
Переменная для формирования ответа на полученный запрос от Алисы.
## Описание методов
### public setCaseSensitive(bool $sensitive = true): $this
Метод, который меняет чувствительность к регистру.
`TRUE` - включить чувствительность.
`FALSE` - выключить чувствительность.

|Аргумент|Тип| Описание  |По умолчанию
|:--:|--|--|--
|sensitive|bool|Чувствительно к регистру.  |true 
___
### public setImagesDir(String $path): $this
Устанавливает директорию с изображениями.
|Аргумент|Тип| Описание  |По умолчанию
|:--:|--|--|--
|path|String|Директория с изображениями. |Обязательно
___
### public setAny(String $message): $this
Метод, который устанавливает сообщение по умолчанию, если чат-бот не смог понять, что от него хотят.

|Аргумент|Тип| Описание  |По умолчанию
|:--:|--|--|--
|message|String|Текст сообщения.  |Обязательно 
___
### public setVersion(String $version = self::VERSION): $this
Метод, который устанавливает версию Алиса API. 

|Аргумент|Тип| Описание  |По умолчанию
|:--:|--|--|--
|version|String|Версия Алиса API.  |Данные с константы VERSION 
___
### public setSpellerCorrect(bool $speller = false): $this
Исправление ошибок в тексте.

|Аргумент|Тип| Описание  |По умолчанию
|:--:|--|--|--
|speller|bool|Установить проверку на орфографию и ее исправления в случае нахождении ошибки.  |false
___
### public setEndMessage(): $this
Метод, который завершает сессию и закрывает навык.
___

### public addStartMessage(String $message): $this
Метод, который устанавливает сообщение при старте навыка.

|Аргумент|Тип| Описание  |По умолчанию
|:--:|--|--|--
|message|String|Текст сообщения.  |Обязательно 
___
### public addStartTTS(String $message): $this
Метод, который устанавливает сообщение при старте навыка для синтеза речи.

|Аргумент|Тип| Описание  |По умолчанию
|:--:|--|--|--
|message|String|Текст сообщения.  |Обязательно 
___
### public addStartButton(String $title, bool $hide = false, Array $payload = [], String $url = null): $this
Метод, который устанавливает кнопки при старте навыка.

|Аргумент|Тип| Описание  |По умолчанию
|:--:|--|--|--
|title  |String|Название кнопки.  |Обязательно 
|hide|bool|Спрятать кнопки после нажатия.|false
|payload|Array|Отправить дополнительные данные для обработки.|[]
|url|String|URL-ссылка|null
___
### public addButton(String $title, bool $hide = false, Array $payload = [], String $url = null): $this
Метод, который устанавливает кнопки.


**ВНИМАНИЕ!**
**Обязательно перед этим методом использовать sendMessage();**

|Аргумент|Тип| Описание  |По умолчанию
|:--:|--|--|--
|title  |String|Название кнопки.  |Обязательно 
|hide|bool|Спрятать кнопки после нажатия.|false
|payload|Array|Отправить дополнительные данные для обработки.|[]
|url|String|URL-ссылка|null
___
### public sendMessage(String $message, String $tts = "", bool $speller = false ): $this
Отправить сообщение пользователю. 
Использовать speller в этом методе не так важно, ведь вы сами можете вводить корректный текст, однако если вы не уверены в написании или боитесь, что где-то сделали опечатку, то можете использовать этот аргумент и поставить его на `TRUE`.
Также использование аргумента speller очень хорошо сочетается с методом [prepare](#public-preparestring-getmessage-string-command-bool).

|Аргумент|Тип| Описание  |По умолчанию
|:--:|--|--|--
|message|String|Текст сообщения.  |Обязательно 
|tts|String|Синтез речи, ударения, паузы.|""
|speller|bool|Проверка на орфографию и исправление, если будет найдена ошибка.|false
___
### public sendImage(String $path, String $title = "", String $description = "", Array $options = []): $this
Метод, которые отобразить изображение. Если указать его, то sendMessage вызывать нет смысла.
**Внимание, чтобы отобразить изображение, вам необходимо его загрузить на сервер Yandex. Для этого создана консоль - alisa.**

|Аргумент|Тип| Описание  |По умолчанию
|:--:|--|--|--
|path|String|Название файла.  |Обязательно 
|title|String|Подпись под фотографией|""
|description|String|Описание под фотографией|""
|options|Array|Дополнительные настройки|[]

#### Дополнительные настройки:
|Аргумент|Тип| Описание  
|:--:|--|--|--
|url|String|Web-ссылка.  
|payload|Array|Payload-запрос.
___
### public sendGallery(Array $images, String $headerText = "", String $footerText = "", Array $footerOpt = []): $this
Метод, которые отобразить галерею. Если указать его, то sendMessage вызывать нет смысла.
**Внимание, чтобы отобразить изображение, вам необходимо его загрузить на сервер Yandex. Для этого создана консоль - alisa.**


|Аргумент|Тип| Описание  |По умолчанию
|:--:|--|--|--
|images|Array|Массив с изображениями.  |Обязательно 
|headerText|String|Заголовок галереи.|""
|footerText|String|Текст под изображениями.|""
|footerOpt|Array|Дополнительные настройки|[]

#### Дополнительные настройки:
|Аргумент|Тип| Описание  
|:--:|--|--|--
|url|String|Web-ссылка.  
|payload|Array|Payload-запрос.
|message|String|Сообщение, которое будет отправлено при нажатии на кнопку.
___
### protected optionsQuestions(Array $list, String $command): bool
Вариация реагирования на фразы для выполнения определенного дейсвтия указанного в 
[cmd()](#public-cmdstring-command).

|Аргумент|Тип| Описание  |По умолчанию
|:--:|--|--|--
|list|Array|Лист ключевых слов для вызова действия.  |Обязательно 
|command|String|Команда пришедшая от пользователя на сервер.|Обязательно
___
### protected optionsAnswers(Array $list): mixed
Отправить вариационный ответ пользователю. Например, если вы не хотите отправлять пользователю одно и тоже сообщение, то можно использовать этот метод и расширить диапазон речи.

|Аргумент|Тип| Описание  |По умолчанию
|:--:|--|--|--
|list|array|Лист фраз, которые будет выдавать Алиса-навык.  |Обязательно 
___
### protected optionsCallback(Array $list, Array $callback): $this
Метод, который позволяет делать логическую операцию ИЛИ. В этом случае указывается набор
фраз, которые возможны при возврате payload (Callback). В случае если это слово будет найдено, то выдает `TRUE`. Также можно отправлять несколько Payload для одного действия и тем самым через эту функцию написать их оба.

|Аргумент|Тип| Описание  |По умолчанию
|:--:|--|--|--
|list|Array|Лист ключевых слов.  |Обязательно 
|callback|String|Обязательная переменная, которая передается в функцию для обработки.|Обязательно
___
### public prepare(String $getMessage, String $command): bool
Метод, который позволяет делать подготовленные запросы и в последствии выводить их. Возвращает `true` или `false`, а также записывает результат функции в переменную `$this->vars`. 

Пример:

    if( $this->prepare("Купить {what} за {price}", $command) ) {  
	  $this->sendMessage("Вы уверены, что хотите купить
	   {$this->vars['whats']} за {$this->vars['price']}₽?");  
	  return true;  
	}

Как вы можете заметить, ключи переменной vars отображают подготовленные ключи из аргумента `String $getMessage`.

|Аргумент|Тип| Описание  |По умолчанию
|:--:|--|--|--
|getMessage|String|Сообщение, которое должен принять навык для обработки данных.  |Обязательно 
|command|String|Сообщение которое придет от пользователя.|Обязательно
___
### public listen(): bool|null
Начать прослушивать Webhook. 
**Данный метод обязательно указывать в конце цепочки.**
___
### public cmd(String $command)
Метод в котором необходимо обрабатывать все данные.
**Обязательно указывать** `return true;` **после каждого условия.**

    if( $command == "привет" ) {  
	  $this->sendMessage("Приветик")->addButton("А что ты умеешь?");  
	  return true;  
	}  
	//Или 
	if( $this->optionsQuestions(["привет", "здравствуйте"]) ) {  
	  $this
	  ->sendMessage($this->optionsAnswers(["Добрый день!", "Я рада вас видеть!"]))
	  ->addButton("А что ты умеешь?");  
	  return true;  
	}
	//Или 
	if( $this->prepare("забронируй мне {what} на {time} в {when}", $command) ) {  
	  $this->sendMessage
	  ("Мы забронировали 
		  {$this->vars['what']}
	   на {$this->vars['time']}
	    в {$this->vars['when']}"
	  );  
	  return true;  
	}
  
	return false;

___
### public payload(Array $callback)
Метод в котором необходимо обрабатывать все данные пришедших с payload..
**Обязательно указывать** `return true;` **после каждого условия.**

    if( array_key_exists('help', $callback) ) {  
	  $this->sendMessage('Много чего! А ты?');  
	  return true;  
	}  
	//Или
	if( $this->optionsCallback(["help", "helpme"], $callback) ) {  
	  $this->sendMessage('Много чего! А ты?');  
	  return true;  
	}  
	return false;
___
## Система JSON-блоков
Просто, удобно, быстро.
Чтобы активировать блоковую систему, укажите в index.php (или в другом файле) метод `setBlocksActions(true)`.
___
### Что это такое?
Система JSON-блоков построена таким образом, чтобы можно было использовать SDK не трогая навыки программирования. Вам достаточно прочитать эту документацию и делать уже сейчас свои навыки. Пока эта система находиться на Alpha-стадии.
___
### Файлы обработки
Всего 2 файла обработки:

|Файл|Описание|Местонахождение
|:--:|--|--
|action.json|Главный файл, который берет на себя ответственность выполнять определенны действия.|\dir_project\blocks\action.json
|payload.json|Файл для обработки payload запросов (callback).|\dir_project\blocks\payload.

**Если у вас стоит `setCaseSensitive(false)`, то обязательно писать маленькими буквами, разрешает писать хаотично в send-теле.**
___
###  Описание Action
Главный файл блоковой системы.

#### Простой пример: 
Это маленький пример, который демонстрирует работу навыка уже с готовым json-блоком. Он ограничен действиями, однако может прост в понимании.

    [
	  {
		"question": "привет",
		"send": {
		   "message": "Добрый день!",
		   "tts": "Д+обрый---д+ень!"
		}
	  }
    ]
|Аргумент|Тип данных|Описание|
|--|--|--
|question|String|Сообщение, которое будет писать пользователь.
|send|Array|Ответить на сообщение навыку.
|message|String|Сообщение, которое будет отправлено в ответ.
|tts|String|Синтез речи.

#### Пример с вариацией сообщений:
Иногда хочется, чтобы навык реагировал на несколько фраз, а также отвечал не одним предложением, а каким-нибудь разным.

    [  
	 { 
	  "question": [  
		  "привет", "добрый день", "здравствуйте"  
	  ],  
	  "send": {  
		  "message": ["Добрый день", "С новым годом!"],  
		  "tts": ["Д+обрый---д+ень!", "С н+овым г+одом!"]  
	  }  
	 }
	]
|Аргумент|Тип данных|Описание|
|--|--|--
|question|Array|Сообщение, которое будет писать пользователь.
|send|Array|Ответить на сообщение навыку.
|message|Array|Сообщение, которое будет отправлено в ответ.
|tts|Array|Синтез речи.

**ВНИМАНИЕ**
Рекомендуем указывать вариацию ответов в одинаковом порядке с синтезом речи, дабы не было разногласия: текст - один, а слова речи -другие.

#### Пример с добавлением кнопки:
Кнопки необходимо добавлять внутри тела send, там и прописывать логику.

	...
	},
	"buttons": {  
	  "title": "button1",  
	  "hide": false,  
	  "payload": [],  
	  "url": null
	}
|Аргумент|Тип данных|Описание|
|--|--|--
|buttons|Array|Объявление кнопок в теле send.
|title|String|Название кнопки.
|hide|Bool|Спрятать кнопку после нажатия.
|payload|Array|Аналог callback, отправляет данные на сервер. Дальше будет расписано, как можно будет это использовать. 
|url|String|Кнопка с ссылкой.


Кнопок можно добавить несколько, для этого просто сделайте `buttons` как массив: 


	...
	},
	"buttons": [
		{  
		  "title": "button1",  
		  "hide": false,  
		  "payload": [],  
		  "url": null
		},
		{  
		  "title": "button2",  
		  "hide": false,  
		  "payload": [],  
		  "url": null
		},
		...
	]
#### Пример с подготовленные запросы:
Подготовленные запросы - это сложная конструкция позволяющая вам создавать различные запросы, например:  забронируй гостиницу на пятницу 
в 15:40. Запрос будет выглядеть таким образом: 

`забронировать {what} на {when} в {time}`
Такая конструкция может быть **за место question**.


    [
      {
	   "prepare": "купить {what}",  
		 "send": {  
		   "message": "Вы купили {what}!"
		   "tts": "Вы куп+или {what}!"
		 },
		 ...
	  }
	]
|Аргумент|Тип данных|Описание|
|--|--|--
|prepare|String|Подготовленный запрос. Переменная указывается в фигурных скобках. Она может быть любой, но **должна начинаться с буквы**.

Переменные можно указывать в send-теле для отображения того, что написал пользователь. 
#### Группировка prepare и question:
Вы также можете группировать вместе подготовленные запросы и обычные. Непосредственно в обоих этих методах возможно использовать вариацию. Однако блоковая система станет сложнее:

    [  
	 {  
		 "question": ["купить", "приобрести"],
		 "prepare": ["купить {what}", "приобрести {what}"],  
		    "send": {  
			  "message": "Покупаем..." , 
			  "tts": "Покупаем..."
		 ...	
	 }
	]
	
Также можно разделить ответы и создать их опциональными:

    [  
	 {  
		 "question": ["купить", "приобрести"],
		 "prepare": ["купить {what}", "приобрести {what}"],  
		    "send": {  
			  "message": {
				  "question": "Покупаем...",
				  "prepare": "Покупаем {what}..."
			  }, 
			  "tts": {
			      "question": "Покуп+аем...",
				  "prepare": "Покуп+аем {what}..."
			  }
		 ...	
	 }
	]
	
С кнопками тоже самое, что и с сообщениями:

    [  
	 {  
		 "question": ["купить", "приобрести"],
		 "prepare": ["купить {what}", "приобрести {what}"],  
		    "send": {  
			  "message": {
				  "question": "Покупаем...",
				  "prepare": "Покупаем {what}..."
			  }, 
			  "tts": {
			      "question": "Покуп+аем...",
				  "prepare": "Покуп+аем {what}..."
			  },
			  "buttons": {  
				  "prepare": [  
					 {  
						 "title": "ButtonPrepare1",  
						 "hide": false,  
						 "payload": [],
						 "url": null  
					 },  
					 {  
						 "title": "ButtonPrepare2",  
						 "hide": false,  
						 "payload": [],
					     "url": null
				     }
				 ],
				 "question": {  
					  "title": "ButtonQuestion1",  
					  "hide": false,  
					  "payload": [],
					  "url": null  
				 }
			}
	 }
	]
	
Как видите, помимо того, что можно добавить кнопку под каждый ответ, так их можно создать несколько. Это очень удобно и делает в два счета, да куда быстрее чем писать код.

#### Пример кода с кнопками и payload:
Payload - это аналог Callback на других сервисах.


	...,
    "buttons": {   
		"title": "ButtonPrepare1",  
		"hide": false,  
		"payload": {
			"function": "test",
			"vars": [  
			   {  
				 "function": "test",  
				 "vars": {}  
			   }
			]			
		},
		"url": null  
	},  
	...

Аргументы из payload: 

|Аргумент|Тип данных|Описание|
|--|--|--
|function|String|Функция, которая указана в payload.json. Настройка действия делается в этом файле.
|vars|Array|Дополнительные переменные, которые будут переданы. Если вы использовали подготовленные запросы, то все переменные будут переданы **во все vars** кнопок, которые будут созданы.

**ВНИМАНИЕ**
Если указать несколько payload, то отправка кнопок или сообщений не будет, но вы можете таким образом обрабатывать различную информацию. Например, произвести оплату.

#### Пример сложного JSON-блока:

    [  
	 {  
		 "question": ["купить", "приобрести"],  
	     "prepare": ["купить {what}", "приобрести {what}"],  
	     "send": {  
		   "message": {  
			   "question": [
				    "Производим покупку...",
				    "Покупаем...",
				    "Оплачиваем..."
			   ],  
			   "prepare": [
				   "Покупаем {what}...",
				   "Приобретаем {what}..."
			   ]  
		   },  
		   "tts": {  
			   "question": [
				   "Произв+одим пок+упку...",
				   "Покуп+аем...",
				   "Опл+ачиваем..."
			   ],  
			   "prepare": [
				   "Покуп+аем {what}...",
				   "Приобрет+аем {what}..."
			   ] 
		   },  
		   "buttons": {  
			   "prepare": [  
				  {  
					 "title": "ButtonPrepare1",  
					 "hide": false,  
					 "payload": [  
					    {
						  "function": "test",  
					      "vars": {
						      "admin": "yes",
						      "age": 23
					      }  
						}
					 ],
					 "url": null  
			 	  }, 
				  {
				    "title": "ButtonPrepare2",  
					"hide": false,  
					"payload": [],  
					"url": "https://example.ru/"  
				  }  
			   ],  
			   "question": {  
				  "title": "ButtonQuestion1",  
				  "hide": true,  
				  "payload": [],  
				  "url": "https://example.ru/"  
			  }  
			 }
		 },
		 "end_session": false  
	 }  
	]

Глобальное описание всех переменных: 

|Аргумент|Тип данных|Описание|
|--|--|--
|question|String\|Array|Принимает сообщения для обработки. Если использовать массив, то будет вариация того, что может написать пользователь.
|prepare|String\|Array|Подготовленные запросы, которые можно использовать за место **question**. Если использовать массив, то будет вариация того, что может написать пользователь.
|send|Array|Массив, который обрабатывает все действия.
|message|String\|Array|Аргумент, который отправляет сообщение пользователю. Если это массив, то будет создана вариация ответов. Рекомендуется делать соответствие текстов с TTS, чтобы не было асинхронности речевого синтеза и текста. Также можно указать тип ответа и на какой аргумент prepare или question.
|tts|String\|Array|Аргумент, который создает синтез речи для текста.  Если это массив, то будет создана вариация ответов. Рекомендуется делать соответствие текстов с TTS, чтобы не было асинхронности речевого синтеза и текста.
|buttons|Array|Создание кнопок. Можно указать тип ответа prepare или question, добавить несколько кнопок.
|title|String|Название кнопки.
|hide|Bool|Скрыть кнопку после нажатия.
|payload|Array|Отправить payload-запрос.
|function|String|Название функции в файле payload.json.
|vars|Array|Переменные для передачи с payload-запросом.
|url|String|Отправить по ссылки при нажатии на кнопку.
|end_session|Bool|Завершить сессию.


### Описание Payload
Payload файл необходим для обработки запросов отправленных с кнопок:
#### Пример запроса:
	
	{
		{  
			  "buy": {  
				  "sendMessage": {  
					  "message": [
					     "Вы хотите купить $^what|asd$. Вы уверены?",
					     "Вы приобритаете $what$. Вы уверены?"
					  ],  
					  "tts": [
						  "Вы хотите купить $what$. Вы уверены?",
						  "Вы приобритаете $what$. Вы уверены?"
					  ]
				  },  
				  "sendButtons": [  
					 { 
						 "title": "Да, уверен.",  
						 "hide": false,  
						 "payload": [  
							{  
							  "function": "yes_buy",  
							  "vars": {}
							} 
						 ],
						 "url": null  
				  }  
			 ]
		}
	}
Описание аргументов:

|Аргумент|Тип данных|Описание|
|--|--|--
|buy|String|Название функции, за место buy можно использовать любые другие слова. Это название нужно передавать в button => payload => function.
|sendMessage|Array|Аргумент для отправки сообщения.
|message|String\|Array|Отправить сообщение. Если это массив, то будет создана вариация сообщений. Рекомендуется делать соответствие текстов с TTS, чтобы не было асинхронности речевого синтеза и текста.
|tts|String\|Array|Отправить синтез речи. Если это массив, то будет создана вариация сообщений. Рекомендуется делать соответствие текстов с TTS, чтобы не было асинхронности речевого синтеза и текста.
|sendButton|Array|Отправляет кнопки.
|title|String|Название кнопки.
|hide|Bool|Скрыть кнопку после нажатия.
|payload|Array|Выполнить Payload-запрос.
|function|String|Название функции в payload.json.
|vars|Array|Переменные для передачи с payload-запросом.
|url|String|Отправить по ссылки при нажатии на кнопку.

### Форматирование текста: 
> \$param\$ - получить переменную из переменной vars. Например вы передали: Купить {what}. Значит в payload нужно указать \$what\$

>\$^param\$ - если стоит такой знак, значит это слово будет исправлено в случае допущенной ошибки. Например: Исправить {what}. В переменной what = (здровстуйте). На выходи мы получим исправленное слово => здравствуйте.

>\$param1|param2\$ - условия ИЛИ. Если переменная param1 будет пуста, то выведет param2 и наоборот.
___
## Консоль Алиса
Консоль, которая на данный момент создана для загрузки изображения и просмотра уже загруженных.
#### php alisa upload:get
Возвращает все загруженные изображения.

    Images List
    
    ===========
	ID: 11
	ImageName: f3ccdd27d2000e3f
	ImageFile: 1.jpg
	ImageID: 1030494/88a816aaacf55838ade8
	===========
	
	...

#### php alisa upload:file `<file_name>`
Загружает изображение из папку, которую вы установили методом `setImageDir()`.

#### php alisa upload:url `<url>`
Загружает изображение с указной ссылки.

___
## index.php
Файл для запуска чат-бота.
Вы также можете изменить название файла, однако необходимо указывать то, что приведено к примеру ниже:

    $main = new \yandex\alisa\Alisa();  
	$main->addStartMessage("Добро пожаловать")->setCaseSensitive(false)->listen();
	
___
## setting.yml
Файл для настройки стандартных констант.

    skill-name: АлиВиксед  
	skill-id: 354e2695-98b3-4951-be30-e7e2efff5868  
	skill-token: AQAAAAAW2RNZAAT7o0Uw9ECNE0WFgqvKcoGUppc

|Константа|Описание|
|--|--|
|skill-name|Название навыка.
|skill-id|ID-навыка.
|skill-token|OAuth-токен Dialogs для работы с изображениями.

## Локальный Webhook: 
Чтобы запустить локальный webhook необходимо пройти на [ngrko](https://ngrok.com/) и создать аккаунт.
После скачать программу и кинуть ее в удобно для вас место.
Запустите командную строку и пропишете: 
ngrok http `port`
Если это локальный сайт, то можете написать ngrok http `example.ru:port`
В случае, если вы используете [OpenServer](https://ospanel.io/) , то необходимо еще указать алису в настройках:
![enter image description here](http://dl4.joxi.net/drive/2018/08/02/0024/0050/1622066/66/f83b666c74.png)
При успешном запуске, просто введите этот адрес в Webhook URL:
![enter image description here](http://dl4.joxi.net/drive/2018/08/02/0024/0050/1622066/66/b5e67d7a60.png)

___
Version: 2.1
Danil Sidorenko © MIT 2018
