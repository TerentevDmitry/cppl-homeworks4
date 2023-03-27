# Домашнее задание к занятию «Тестирование»

Выполнив это задание, вы научитесь создавать unit-тесты и поработаете с библиотекой Catch2.

### Цель задания

1. Научиться работать с библиотекой Catch2.
2. Научиться покрывать свой код тестами.

### Подготовка к выполнению домашнего задания

1. Для выполнения задания и прохождения курса нужен компьютер с операционной системой Windows или macOS и установленной на нём Microsoft Visual Studio 2022, готовой для разработки консольных программ на C++.
2. Аккаунт на [GitHub](https://github.com/). [Инструкция по регистрации на GitHub](https://github.com/netology-code/cppm-homeworks/tree/main/common/sign%20up).
3. Система контроля версий [Git](https://git-scm.com/), установленная локально. [Инструкция по установке Git](https://github.com/netology-code/cppm-homeworks/tree/main/common/download).

### Инструкция по выполнению домашнего задания

[Инструкция дана по ссылке](https://github.com/netology-code/cppm-homeworks/blob/main/common/readme.md).

------

### Задание 1

[Проверка базовых функций двусвзяного списка](01).
<details>
# Задача 1. Проверка базовых функций двусвзяного списка

### Описание
В этом задании вам нужно написать тесты для некоторых функций двусвзяного списка.
Сначала добавьте Catch2 в ваш проект. Инструкция дана [по ссылке](https://github.com/catchorg/Catch2/blob/devel/docs/cmake-integration.md).

Нужно проверить работу функций и написать unit-тесты для них:
1. Empty.
2. Size.
3. Clear.

Код двусвязного списка:

``` C++
#include <iostream>

struct ListNode
{
public:
    ListNode(int value, ListNode* prev = nullptr, ListNode* next = nullptr)
        : value(value), prev(prev), next(next)
    {
        if (prev != nullptr) prev->next = this;
        if (next != nullptr) next->prev = this;
    }

public:
    int value;
    ListNode* prev;
    ListNode* next;
};


class List
{
public:
    List()
        : m_head(new ListNode(static_cast<int>(0))), m_size(0),
        m_tail(new ListNode(0, m_head))
    {       
    }

    virtual ~List()
    {
        Clear();
        delete m_head;
        delete m_tail;
    }

    bool Empty() { return m_size == 0; }

    unsigned long Size() { return m_size; }

    void PushFront(int value)
    {
        new ListNode(value, m_head, m_head->next);
        ++m_size;
    }

    void PushBack(int value)
    {
        new ListNode(value, m_tail->prev, m_tail);
        ++m_size;
    }

    int PopFront()
    {
        if (Empty()) throw std::runtime_error("list is empty");
        auto node = extractPrev(m_head->next->next);
        int ret = node->value;
        delete node;
        return ret;
    }

    int PopBack()
    {
        if (Empty()) throw std::runtime_error("list is empty");
        auto node = extractPrev(m_tail);
        int ret = node->value;
        delete node;
        return ret;
    }

    void Clear()
    {
        auto current = m_head->next;
        while (current != m_tail)
        {
            current = current->next;
            delete extractPrev(current);
        }
    }

private:
    ListNode* extractPrev(ListNode* node)
    {
        auto target = node->prev;
        target->prev->next = target->next;
        target->next->prev = target->prev;
        --m_size;
        return target;
    }

private:
    ListNode* m_head;
    ListNode* m_tail;
    unsigned long m_size;
};
```


#### Подсказки

> Не читайте этот раздел сразу. Попытайтесь сначала решить задачу самостоятельно :)

<details>

<summary>Что использовать для решения.</summary>

1. Проверьте, что функция на свежесозданном списке возвращает `true`.
2. Добавьте несколько элементов в список. Проверьте правильность количества элементов в списке.
3. Проверьте, что количество элементов после очистки равно 0.

</details>
</details>

### Задание 2

[Проверка функций добавления и получения элементов из двусвязного списка](02).
<details>
# Задача 2. Проверка функций добавления и получения элементов из двусвязного списка

### Описание
В этом задании вам нужно написать тесты для некоторых функций двусвзяного списка.
Сначала добавьте Catch2 в ваш проект. Инструкция дана [по ссылке](https://github.com/catchorg/Catch2/blob/devel/docs/cmake-integration.md).

Нужно проверить работу функций:
1. PushBack.
2. PushFront.
3. PopBack. Проверьте правильность работы на пустом списке.
4. PopFront. Проверьте правильность работы на пустом списке.

Кроме простых тестов напишите сценарий использования списка сложнее: несколько вызовов функций 1-4.

Код двусвязного списка:
``` C++
#include <iostream>

struct ListNode
{
public:
    ListNode(int value, ListNode* prev = nullptr, ListNode* next = nullptr)
        : value(value), prev(prev), next(next)
    {
        if (prev != nullptr) prev->next = this;
        if (next != nullptr) next->prev = this;
    }

public:
    int value;
    ListNode* prev;
    ListNode* next;
};


class List
{
public:
    List()
        : m_head(new ListNode(static_cast<int>(0))), m_size(0),
        m_tail(new ListNode(0, m_head))
    {       
    }

    virtual ~List()
    {
        Clear();
        delete m_head;
        delete m_tail;
    }

    bool Empty() { return m_size == 0; }

    unsigned long Size() { return m_size; }

    void PushFront(int value)
    {
        new ListNode(value, m_head, m_head->next);
        ++m_size;
    }

    void PushBack(int value)
    {
        new ListNode(value, m_tail->prev, m_tail);
        ++m_size;
    }

    int PopFront()
    {
        if (Empty()) throw std::runtime_error("list is empty");
        auto node = extractPrev(m_head->next->next);
        int ret = node->value;
        delete node;
        return ret;
    }

    int PopBack()
    {
        if (Empty()) throw std::runtime_error("list is empty");
        auto node = extractPrev(m_tail);
        int ret = node->value;
        delete node;
        return ret;
    }

    void Clear()
    {
        auto current = m_head->next;
        while (current != m_tail)
        {
            current = current->next;
            delete extractPrev(current);
        }
    }

private:
    ListNode* extractPrev(ListNode* node)
    {
        auto target = node->prev;
        target->prev->next = target->next;
        target->next->prev = target->prev;
        --m_size;
        return target;
    }

private:
    ListNode* m_head;
    ListNode* m_tail;
    unsigned long m_size;
};
```

</details>

------

### Правила приёма домашней работы

Чтобы сдать домашнее задание, прикрепите в личном кабинете ссылку на ваш репозиторий.

### Критерии оценки домашней работы

1. В личном кабинете прикреплена ссылка на репозиторий с кодом для заданий 1 и 2.
2. В ссылке содержится код, который при запуске выполняет описанный в задании алгоритм.

