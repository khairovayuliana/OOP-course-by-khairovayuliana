# ГЕТТЕРЫ И СЕТТЕРЫ  
Представь, что у тебя есть младший брат лет десяти-двенадцати, который ВЕЧНО лезет в твои вещи! Знакомо ведь? Так вот, ты купил себе новый Wi-Fi роутер и не хочешь, чтобы он:
1. Менял пароль (иначе никто не сможет подключиться).
2. Видел пароль просто так (а то ведь разболтает друзьям, а ты не хочешь чтобы твой интернет поедали дети).  

Так вот геттеры и сеттеры — это что-то типа _правил дома_, которые _защищают данные твоего класса от хаоса_.
```cpp
class MyWiFiRouter {
private:
    string password = "123"; // приватное поле (пароль)
public:
    // Геттер — "узнать пароль, но перед этим убедиться, что человеку можно доверять"
    string getPassword(const string& secretWord) const {
        if (secretWord == "ЭтоЖеЯ") {
            return password;
        }
        return "А тебе доступ запрещён!";
    }

    // Сеттер — "изменить пароль, но только если старый введен верно"
    void setPassword(const string& oldPassword, const string& newPassword) {
        if (oldPassword == password) {
            password = newPassword;
            cout << "Пароль изменён!";
        } else {
            cout << "Неверно введен старый пароль!";
        }
    }
};
```
- _Для получения доступа к приватным данным в классе нужно использовать публичные методы._  
- _Для получения значения полей и их изменения используются методы, называемые геттерами и сеттерами соответственно._

У тебя есть класс `LyceumStudent`:
```cpp
#include<iostream>
#include<vector>
#include<unordered_map>
using namespace std;
class LyceumStudent {
private:
    string name, surname;
    unordered_map<string, vector<int>> marks;
public:
    // Геттеры и сеттеры для name
    string getName() const {
        return name;
    }
    void setName(const string& newName) {
        name = newName;
    }
    
    // Геттеры и сеттеры для surname
    string getSurname(){
        return surname;
    }
    
    void setSurname(const string& newSurname) {
        surname = newSurname;
    }
    
    // Геттер и сеттер для marks
    const unordered_map<string, vector<int>>& getMarks(){
        return marks;
    }
    
    void setMarks(const unordered_map<string, vector<int>>& newMarks) {
        marks = newMarks;
    }
    
};
int main(){
    string name;
    LyceumStudent s;
    s.setName("Juliana");
    cout << s.getName();
}
```
Рассмотрим __геттеры (getters)__ и __сеттеры (setters)__ на примере их использования для поля `name` в классе `LyceumStudent`:
1. __Геттер (getter) — метод для получения значения из непубличного поля класса__  
```cpp
string getName(){
    return name;
}


string getName() const {
    return name;
}
```
- **Тип возвращаемого значения:** `string` (так как `name` имеет тип `string`).

- **Название метода:** Обычно начинается с get + имя поля (`getName`).

- **Модификатор `const` (необязательно):** Он показывает, что метод не изменяет состояние объекта, а только читает поле. 

- **Но без `const` может возникнуть проблема:** такой геттер **нельзя вызвать у константного объекта**

- **Возвращает:** Значение поля `name`.

2. **Сеттер (setter) — метод для изменения значения непубличного поля класса**
```cpp
void setName(const string& newName) {
    name = newName;
}
```
- **Тип возвращаемого значения:** `void` (так как метод только устанавливает значение, ничего не возвращая).

- **Название метода:** Обычно начинается с set + имя поля (`setName`).

- **Параметр:** Принимает новое значение переменной:

    - `const string&` — константная ссылка (чтобы избежать копирования строки).

    - `newName` — новое имя, которое будет записано в поле name.

- Метод присваивает полю name переданное значение (`name = newName`).


## Безопасный сеттер с проверкой входных значений
Кажется, что все хорошо, но на самом деле есть небольшая проблема: а что будет, если пользователь решит присвоить себе несуществующую оценку? Представь, что младшеклассники решили побаловаться и поставить себе оценку 10, хотя в их школе 5-балльная шкала. Или вообще решили поиздеваться над своим другом и поставить ему -5.  
Нам нужно изменить сеттер `setMarks`, добавив в него проверку на то, что оценка находится в нужном нам диапазоне, например, от 1 до 5. 

```cpp
void setMarks(const unordered_map<string, vector<int>>& newMarks) {
    bool isValid = true;

    for (const std::pair<const std::string, std::vector<int>>& subjectEntry : newMarks) {
        const string& subject = subjectEntry.first;
        const vector<int>& grades = subjectEntry.second;

        for (int mark : grades) {
            if (mark < 1 || mark > 5) {
                cout << "Ошибка: оценка " << mark << " по предмету '" 
                     << subject << "' недопустима!\n";
                isValid = false;
                break;
            }
        }
        if (!isValid) break;
    }

    if (isValid) {
        marks = newMarks;
    }
}
```

