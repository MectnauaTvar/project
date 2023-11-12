import sqlite3

# Подключение к базе данных
conn = sqlite3.connect('employees.db')
cursor = conn.cursor()

# Создание таблицы, если она не существует
cursor.execute('''CREATE TABLE IF NOT EXISTS employees
                  (id INTEGER PRIMARY KEY AUTOINCREMENT,
                   full_name TEXT,
                   phone_number TEXT,
                   email TEXT,
                   salary REAL)''')

# Функция для добавления нового сотрудника
def add_employee(full_name, phone_number, email, salary):
    cursor.execute('''INSERT INTO employees (full_name, phone_number, email, salary)
                      VALUES (?, ?, ?, ?)''', (full_name, phone_number, email, salary))
    conn.commit()
    print("Сотрудник успешно добавлен")

# Функция для изменения информации о сотруднике
def update_employee(employee_id, full_name, phone_number, email, salary):
    cursor.execute('''UPDATE employees
                      SET full_name = ?, phone_number = ?, email = ?, salary = ?
                      WHERE id = ?''', (full_name, phone_number, email, salary, employee_id))
    conn.commit()
    print("Информация о сотруднике успешно обновлена")

# Функция для удаления сотрудника
def delete_employee(employee_id):
    cursor.execute('''DELETE FROM employees WHERE id = ?''', (employee_id,))
    conn.commit()
    print("Сотрудник успешно удален")

# Функция для поиска сотрудника по ФИО
def search_employee_by_name(full_name):
    cursor.execute('''SELECT * FROM employees WHERE full_name LIKE ?''', ('%' + full_name + '%',))
    employees = cursor.fetchall()
    if len(employees) > 0:
        for employee in employees:
            print(employee)
    else:
        print("Сотрудник не найден")

# Пример использования функций
add_employee("Иванов Иван Иванович", "1234567890", "ivanov@gmail.com", 1000.0)
update_employee(1, "Иванов Иван Петрович", "0987654321", "ivanov@mail.com", 1500.0)
delete_employee(1)
search_employee_by_name("Иван")
