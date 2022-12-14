# Decoder-PNG-to-PNM
## Декодирование PNG
* Цель работы: 
Изучить особенности работы с двоичными файлами, структурами, директивами препроцессора и сторонними библиотеками в C. Закрепить навык чтения спецификаций.
* Стандарт языка
С11 и новее. Компиляция в проверяющей системе проводится по С17 стандарту.
* Описание:
Необходимо реализовать программу, которая декодирует изображение PNG в PNM. 
должны поддерживаться: серые и цветные изображения (Color Type 0 и 2), 8 бит на канал.
не нужно поддерживать: работу с прозрачностью, interlacing и цветовую коррекцию (гамму и пр.).
Необходимо использовать одну библиотеку из следующих: libdeflate, zlib или isa-l (на ваше усмотрение).
Критерий “Все библиотеки”: должно поддерживаться использование всех трёх сторонних библиотек: zlib, libdeflate и isa-l.
Библиотеки должны использоваться только для разжатия deflate-потока из изображения.


Для вашего репозитория (на github) исходный файл, содержащий функцию main, должен лежать в корне репозитория. Пример: 
/c_2-<github_nickname>
├── main.c /* your src file*/
├── my_header.h /* [optional] your header file */
├── .gitignore /* [optional] */
├── .clang-format
├── …

* Выбор библиотеки должен определяться макросом, который указывается ключом компилятора, создающим define. Гарантируется, что при компиляции будет указан ровно один из трёх макросов.
Библиотека
zlib
libdeflate
isa-l
define
ZLIB
LIBDEFLATE
ISAL

В случае, если при компиляции был указан макрос, определяющий подключение библиотеки, которую ваша программа не поддерживает, то необходимо командой препроцессора #error() выводить сообщение о том, что данная библиотека не поддерживается. Даже если ваша программа умеет работать только с одной из библиотек (например, zlib), то необходимо делать проверку на ZLIB макрос. Подробнее: https://en.cppreference.com/w/c/preprocessor/error 
Примеры:
#if defined(ZLIB)
#error …
#ifndef ZLIB
#error …

Формат аргументов командной строки
Аргументы программе передаются через командную строку:
c2 <имя_входного_PNG_файла> <имя_выходного_PNM_файла>
Входной файл
PNG-изображение. Не гарантируется корректность данных внутри файла (может быть любой файл, в том числе не PNG или файл с ошибками).
Выходной файл
Формат выходных изображений: PNM (P5 или P6). Во всех PNM файлах (pgm, ppm) комментарии отсутствуют. 
Если изображения во входном файле было в оттенках серого (Color Type 0), то выходное изображение должно быть в формате P5. Если входное было Color Type 2, то на выходе - P6.
Формат представлен ниже:
P5 (PGM)
“P5\n<width> <height>\n255\n<Gray_data>”
P6 (PPM)
“P6\n<width> <height>\n255\n<RGB_data>”

* Требования к программе:
должна быть написана на C по заданному стандарту;
должна выполнять поставленную в ТЗ задачу;
не использовать внешние библиотеки, кроме указанных выше;
всегда корректно освобождать память и закрывать файлы;
обрабатывать ошибки: 
файл не открылся; 
не удалось выделить память;
на вход передано неверное число аргументов командной строки;
аргументы некорректны;
формат файла не поддерживается;
входной файл некорректен.
В этих случаях необходимо выдавать сообщение об ошибке и корректно завершаться с ненулевым кодом возврата (см. "return_codes.h");
не писать в консоль ничего лишнего, кроме сообщений об ошибках и по желанию краткой справки по использованию (при запуске с неправильным числом аргументов).
Сообщение об ошибке необходимо выводить в поток вывода ошибок. Если вы будете выводить сообщение об ошибке в стандартный поток вывода, то это будет засчитываться за проваленный тест.
