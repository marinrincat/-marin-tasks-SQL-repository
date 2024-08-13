# marin-tasks-SQL-repository
	Практика.
	PostgreSQL 16 + pgAdmin 4.
	для отправки запроса, нужно перейти в консоль.
	
	СОЗДАНИЕ ТАБЛИЦ
	CREATE для создания любых сущностей(таблиц, индексов, последовательностей)
	CREATE TABLE (создание таблицы) users (имя таблицы) (
    	id (имя первой колонки) BIGINT(целое число) NOT NULL(значение никогда не пустое) PRIMARY KEY(уникальный идентификатор каждой строчки),
   	 first_name(имя второй колонки) VARCHAR(текстовые символы)+(64)(максимальная длинна строки), NOT NULL(значение никогда не пустое),
	)
	
	!!! PRIMARY KEY(уникальный идентификатор каждой строчки) - очень важен для удобства взаимодействия !!!
	
	CREATE TABLE users (
   	 id BIGINT NOT NULL PRIMARY KEY,
   	 first_name VARCHAR(64) NOT NULL,
   	 last_name VARCHAR(64) NOT NULL,
   	 email VARCHAR(120) NOT NULL
	);
	
	ДОБАВЛЕНИЕ ДАННЫХ В ТАБЛИЦЫ
	INSERT - команда, которая позволяет вставлять данные в любую таблицу в виде новой строчки в ней
	VALUES - команда, которая указывает, какие именно значения будут использованы

	INSERT INTO users (id, first_name, last_name, email)
	VALUES ('1', 'Marin', 'Rincat', 'marinrincat@gmail.com');

	ОБНОВЛЕНИЕ ДАННЫХ В ТАБЛИЦАХ
	UPDATE - команда, для обновления определённых значений определённых колонок в конкретных строках данной таблицы
	WHERE - команда, которая определяет условие выборки конкретных строк для выполнения того или иного действия
	(чтения, обновления и т.д.)

	UPDATE users SET
	email = 'marinrinrinrincat@gmail.com'
	WHERE id = 1

	УДАЛЕНИЕ ДАННЫХ В ТАБЛИЦАХ
	DELETE - удаление строк из данной таблицы, выбранных по какому-либо определённому условию

	DELETE FROM users
	WHERE id = 2 OR id = 3

	ПОЛУЧИТЬ ДАННЫЕ ИЗ БД
	SELECT - для получения данных из конкретной строки и колонки в данной таблице, которые необходимо выбрать(прочитать) из БД

	SELECT id, first_name, last_name, email FROM users
	WHERE id = 1
   	 AND first_name = 'Marin'
   	 AND last_name = 'Rincat"

	ИЛИ ЕСЛИ НУЖНЫ ВСЕ КОЛОНКИ В КОНКРЕТНОЙ ТАБЛИЦЕ, ТО:
	SELECT * FROM users

	ВНЕШНИЙ КЛЮЧ ТАБЛИЦ ДЛЯ СОЗДАНИЯ ОТНОШЕНИЙ ТАБЛИЦ ИЛИ FOREIGN KEY
	CONSTRAINT - команда, определяющая новое правило организации данных в БД(связи, уникальность и т.д.)
	REFERENCES - команда, определяющая на какую таблицу будет ссылка

	CREATE TABLE spendings (
		id BIGINT NOT NULL PRIMARY KEY,
		price INT NOT NULL,
		created_at TIMESTAMP DEFAULT now(),
		user_id BIGINT NOT NULL,

		CONSTRAINT user_id_fk FOREIGN KEY (user_id) REFERENCES users (id)
	);

	JOIN ПРИСОЕДИНЕНИЕ ДРУГ К ДРУГУ КОЛОНОК ИЗ ДВУХ РАЗНЫХ ТАБЛИЦ
	ON - используется для определения условия присоединения

	SELECT * FROM spendings
	JOIN users ON users.id = spendings.user_id
	- объединение двух таблиц со всеми колонками

	SELECT spendings.*, users.first_name FROM spendings
	JOIN users ON users.id = spendings.user_id
	- достаём все колонки только из spendings + имя из users
	!!! INNER JOIN это JOIN с условиями к которому нашлась пара, 
	а OUTER JOIN добавляет в выборку те строки, для которых не нашлось пары!!!

	SELECT * FROM spendings
	LEFT OUTER JOIN users ON users.id = spendings.user_id
	- для передачи данных из левой таблицы(та, которая идёт первой в запросе)
	SELECT * FROM spendings
	RIGHT OUTER JOIN users ON users.id = spendings.user_id
	- для передачи данных из правой таблицы(та, которая идёт второй в запросе)
	SELECT * FROM spendings
	FULL OUTER JOIN users ON users.id = spendings.user_id
	- для передачи данных и из левой и из правой таблиц

	АГРЕГАТНЫЕ ФУНКЦИИ
	инструменты, для произведения математических операций над числовыми данными(сумма, максимум, минимум, среднее значение)
	SUM() - суммирует все значения в переданной колонке данной таблицы в одно общее значение

	SELECT SUM(price) FROM spendings

	GROUP BY - команда, определяющая колонки, по которым будет проводится группировка результатов агрегатных функций

	SELECT SUM(price) FROM spendings
	GROUP BY user_id

	!!! внутри WHERE нельзя писать агрегатные функции !!!
	HAVING - заменяет WHERE, внутри можно писать агрегатные функции, ТОЛЬКО ПОСЛЕ GROUP BY

	ИЗМЕНЕНИЕ ТАБЛИЦ БЕЗ УТРАТЫ ДАННЫХ
	ALTER TABLE 

	ALTER TABLE spendings ADD COLUMN category_id BIGINT;
	ALTER TABLE spendings ADD CONSTRAINT category_fk FOREIGN KEY (category_id) REFERENCES categories (id);
	(´｡• ω •｡`)
