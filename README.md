## Тестовые фу-ии для класса *BooksCollector*

### Создание объекта класса вынесено фикстурой в conftest

1. Тест с параметризацией проверяет добавление книг 
```
    @pytest.mark.parametrize('book', ['Падение дома Ашеров', 'Граф Монте-Кристо'])
    def test_add_new_book_adding_book(self, books_collector, book):
        books_collector.add_new_book(book)
        assert books_collector.books_genre[book] == ''
```
2. Тест проверки 40+ символов без добавления в список книг
```
    @pytest.mark.parametrize('book', ['Очень много было в жизни 1111111111111111111111111111111 но такого еще никогда не было'])
    def test_add_new_book_more_plus40_simbol_sizes(self, books_collector, book):
        assert not books_collector.add_new_book(book)
```
---
3. Тест добавленнных книг, что они словарь.
Циклом добавляем книги, сравниваем рандомное значение из списка.
```
        def test_get_books_genre_dict(self, books_collector):
            books = ['А как на Руси жить хорошо', '1984', 'Лед и пламя']
            for name in books:
                books_collector.add_new_book(name)
            rand_book = random.choice(books)
            assert rand_book in books_collector.get_books_genre() and type(books_collector.get_books_genre()) == dict
```
---
4. Тест проверяет неправильный жанр. 
Некорректный размер, и неправильный жанр.
```
    @pytest.mark.parametrize('name, genre', [
        ['Название 1111111111111111111111111111111111111111111111111111111111111111111111111111', 'Комедии'],
        ['Название 2', 'Тест жанр 2']
    ])
    def test_set_book_genre_fail_right_write_info(self, books_collector, name, genre):
        books_collector.add_new_book(name)
        assert not books_collector.set_book_genre(name, genre)
```
---
5. Тест на получение фильмов по неправильному жанру (список пуст или неправильный). 
В парамеретризации получаем список и получаем по неправильному жанру.
```
    @pytest.mark.parametrize('name, genre', [['', 'Какой то жанр'], ['ахахахаахахах', 'Комедии']])
    def test_get_books_with_specific_genre_empty_list_books_false_genre(self, books_collector, name, genre):
        books_collector.add_new_book(name)
        assert not books_collector.get_books_with_specific_genre('Какой то жанр 2')
```
---
6. Проверка получения жанра, добавляем книгу, фиксируем жанр (корректный)
Проверяем, что по названию возвращается жанр.
```
    def test_get_book_genre_return_valid_name_genre(self, books_collector):
        books_collector.add_new_book('x')
        books_collector.set_book_genre('x', 'Фантастика')
        assert 'Фантастика' == books_collector.get_book_genre('x')
```
---
7. Тест проверяет рейтинг жанров 18+. 
Циклом добавляем книги и задаем им жанры 18+.
```
    def test_get_books_for_children_18plus_rating(self, books_collector):
        books = ['Детская 1', 'Детская 2']
        x = 0
        for name in books:
            books_collector.add_new_book(name)
            books_collector.set_book_genre(name, books_collector.genre_age_rating[x])
            x += 1
        assert not books_collector.get_books_for_children()
```
---
8. Тест проверяет, что книга добавленная в избранное, есть в избранном
```
    def test_add_book_in_favorites_when_books_in_list(self, books_collector):
        books_collector.add_new_book('Высшая математика')
        books_collector.add_book_in_favorites('Высшая математика')
        assert 'Высшая математика' in books_collector.favorites
```
---
9. Тест проверяет удаление книги из избранного.
```
    def test_delete_book_from_favorites_positive_deleted_book(self, books_collector):
        books_collector.add_new_book('Теоритическая физика')
        books_collector.add_book_in_favorites('Теоритическая физика')
        books_collector.delete_book_from_favorites('Теоритическая физика')
        assert 'Теоритическая физика' not in books_collector.favorites
```
---
10. Тест проверяет получение списка избранных книг. 
Циклом добавляем книги, добавляем избранное, assert списока, что он не пуст.
```
    def test_get_list_of_favorites_books_not_empty(self, books_collector):
        books = ['Бесприданница', 'Любовницы 2', 'Анигиляторная пушка']
        for name in books:
            books_collector.add_new_book(name)
            books_collector.add_book_in_favorites(name)
        assert books_collector.get_list_of_favorites_books()
```
---