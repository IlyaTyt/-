```py
from tkinter import *
from tkinter import ttk
from tkinter import messagebox
from datetime import datetime
from tkinter.messagebox import showinfo, askyesno
import requests
#Переменный
a = '' #Переменная для Доп-Информ
i = 0 #Кол-во людей
uio = i
b = i
op = i
Korpis = ""
Name = ""
group = ""
Cgick = i
Kol_vo = b
Kol_vo1 = 0
FIO = []
Covid = []
Progyl = []
Ybajul = []
e = datetime.now()
Do = []
data = datetime.now()

def checke():
    global i, b, uio, Korpis, Name, group, Cgick, Kol_vo, Kol_vo1, Kol_vo2, Covid, FIOCovid, Progyl, Ybajul, FIOYbajul, e, Do, FIO, op
    if entry_username.get().lower() == "123" and entry_username1.get().lower() == "123":
        languages = ["Кунык", "Лиенко", "Осипов", "Агапов", "Анохин", "Балыкин", "Водин", "Волков", "Гришина", "Карелин"]
        combobox.config(values=languages)
        lab2.config(text="Группа - 1Ис")
        i = 34  # Ков-ло учащийся
        uio = i
        b = i  # Для кол-во присутствующих
        op = i
        Korpis = "Шенкурсский"
        Name = "Чалов"
        group = "1Ис"
        Cgick = i
        Kol_vo = b
        Kol_vo1 = 0
        FIO = []
        Covid = []
        Progyl = []
        Ybajul = []
        e = datetime.now()
        Do = []
        switch_form()
    elif (entry_username.get().lower() == "321" and entry_username1.get().lower() == "321"):
        languages = ["Богомолова","Бондарев","Губанова","Ильин","Катшани","Кощиц","Кузьмин","Комаеда","Левин","Лысенко"]
        combobox.config(values=languages)
        lab2.config(text="Группа - 2Ис")
        i = 30# Ков-ло учащийся
        uio = i
        b = i # Для кол-во присутствующих
        op = i
        Cgick = i
        Kol_vo = b
        Korpis = "Шенкурсский"
        Name = "Олейник"
        group = "2Ис"
        switch_form()
    elif (entry_username.get().lower() == "231" and entry_username1.get().lower() == "231"):
        languages = ["Бoрада", "Власенков", "Мамаджонов", "Жаров", "Котов", "Кувшинов", "Ли", "Недорезов", "Апарина", "Носкова"]
        combobox.config(values=languages)
        lab2.config(text="Группа - 3Ис")
        i = 19 # Ков-ло учащийся
        uio = i
        b = i # Для кол-во присутствующих
        op = i
        Cgick = i
        Kol_vo = b
        Korpis = "Шенкурсский"
        Name = "Никитин"
        group = "3Ис"
        switch_form()
    else:
        messagebox.showerror("Ошибка", "Неверное имя пользователя")


def switch_form():#Функция для закрытие 1 формы и открытие 2 формы
    form1.pack_forget()
    form2.pack()


def sel(event):#Функция для выбора студента и причине его отсутствие в виде строки
    selection = combobox.get()
    selection1 = combobox1.get()
    label1["text"] = f"Студент - {selection} отсутствует по причине - {selection1}"


def zanoc(): #Добавление данных в tree
    global b, Kol_vo,Covid
    selection = combobox.get()
    selection1 = combobox1.get()
    selection2 = datetime.now().strftime('%d %b %Y %H:%M:%S')
    tree.insert("", "end", values=(selection, selection1, selection2))
    Kol_vo = Kol_vo - 1
    if (selection1 == "Болеет"):
        FIO.append(selection)
    elif (selection1 == "Болеет-COVID"):
        Covid.append(selection)
    elif (selection1 == "Прогул"):
        Progyl.append(selection)
    elif (selection1 == "Заявление"):
        Ybajul.append(selection)
    elif (selection1 == "Дистант"):
        Do.append(selection)
    print(Kol_vo)


def clear():#Очистка выбранного строки в таблице
    global FIO, Covid, Progyl, Ybajul, Do,zamena,op,Kol_vo
    row_id = str(tree.focus())  # Получаем идентификатор выбранной строки
    if row_id:  # Проверка, что строка выбрана
        item = tree.item(row_id, 'values')  # Получаем значения выбранной строки
        name,reason,_= item  # Распаковываем значения
        tree.delete(row_id)  # Удаляем выбранную строку из виджета Treeview

        # Удаление выбранных элементов из массивов
        if reason == "Прогул":
            if name in FIO:
                FIO.remove(name)
            if name in zamena:
                zamena.remove(name)
            if name in Progyl:
                Progyl.remove(name)
        elif reason == "Болеет":
            if name in FIO:
                FIO.remove(name)
            if name in zamena:
                zamena.remove(name)
            if name in Covid:
                Covid.remove(name)
        elif reason == "Заявление":
            if name in Ybajul:
                Ybajul.remove(name)
        elif reason == "ДО":
            if name in Do:
                Do.remove(name)
            if name in zamena:
                zamena.remove(name)
    Kol_vo = Kol_vo + 1

def show_password():#Для показа пароля
    state = entry_username1.config()['show'][-1]
    if state == '*':
        entry_username1.configure(show='')
    else:
        entry_username1.configure(show='*')

def submit_form():#Функция,которая заносит данные в таблицу
    global Kol_vo, Kol_vo1, op,a
    GoogleURL = "https://docs.google.com/forms/d/e/1FAIpQLSdzAfoxhCJmHnti72jfIg-nrO-SRygzvf99NKUc0pEihcJLjg"
    urlResponse = GoogleURL + '/formResponse'
    urlReferer = GoogleURL + '/viewform'
    result = askyesno(title="Отправить", message="Отправить отчет?")
    a = entry.get()
    if result:
        if (enab.get() == 1):
            op = Kol_vo
            Kol_vo, Kol_vo1 = Kol_vo1, Kol_vo
        else:
            op = Kol_vo
        form_data = {'entry.234736211': Korpis,
                     'entry.999293437': Name,
                     'entry.411943097': group,  #
                     'entry.102528162': Cgick,  #
                     'entry.366570961': Kol_vo,
                     'entry.1577207787': Kol_vo1,  #
                     'entry.534036855': len(FIO) + len(Covid),  #
                     'entry.1693314686': FIO,
                     'entry.1800471545': len(Covid),
                     'entry.791770035': Covid,
                     'entry.823927112': len(Progyl),
                     'entry.1937679818': len(Ybajul),
                     'entry.1733660016': Ybajul,  #
                     'entry.945833765': len(Do),
                     'entry.1370681129': Do,
                     'entry.1737924855_year': data.year,
                     'entry.1737924855_month': data.month,
                     'entry.1737924855_day': data.day,
                     'entry.375010821': Progyl,
                     'entry.609793696': a
                     }
        user_agent = {'Referer': urlReferer,
                      'User-Agent': "Mozilla/5.0 (X11; Linux i686) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/28.0.1500.52 Safari/537.36"}
        r = requests.post(urlResponse, data=form_data, headers=user_agent)
        showinfo("Результат", f"Отчет отправлен\n{op} - Человек присутствует")


root = Tk()
root.title("Окно")
root.geometry("800x600")
#root.iconbitmap(default = "ico12.ico")

form1 = ttk.Frame(root)#Первая форма
form1.pack(anchor=CENTER, expand=True)

label_username = ttk.Label(form1, text="Логин:", font="Courier 14")
label_username.pack(pady=5)

entry_username = ttk.Entry(form1)
entry_username.pack(pady=5)

label_username1 = ttk.Label(form1, text="Пароль:", font="Courier 14")
label_username1.pack(pady=5)

entry_username1 = ttk.Entry(form1,show = "*")
entry_username1.pack(pady=5)

button_next3 = ttk.Button(form1, text="Зайти", command=checke)
button_next3.pack(pady=5)

butPok = Button(form1,text="Показать пароль",command=show_password)
butPok.pack()


form2 = ttk.Frame(root)#2 Форма
form2.pack(anchor = CENTER)

lab1 = ttk.Label(form2, text="ГБПОУ Политехнический колледж им.П.А.Овчинникова")
lab1.pack(pady=5)

combobox = ttk.Combobox(form2)
combobox.pack(anchor=CENTER, fill=X, padx=6, pady=6)
combobox.bind("<<ComboboxSelected>>", sel)

languages1 = ["Прогул", "Болеет", "Заявление", "Болеет-COVID", "Дистант"]
combobox1 = ttk.Combobox(form2, values=languages1, state="readonly")
combobox1.pack(anchor=CENTER, fill=X, padx=6, pady=6)
combobox1.bind("<<ComboboxSelected>>", sel)
enab = IntVar()
label1 = ttk.Label(form2)
label1.pack(anchor=CENTER, padx=5, pady=5)

button3 = ttk.Button(form2, text="Добавить", command=zanoc)
button3.pack(pady=5)

button4 = ttk.Button(form2, text="Очистить", command=clear)
button4.pack(pady=5, anchor=S)

lab1 = ttk.Label(form2, text="")
lab1.pack(pady=5)

lab2 = ttk.Label(form2, text="")
lab2.pack(pady=1, anchor=S)

ena = ttk.Checkbutton(form2, text="Группа на практике", variable=enab)
ena.pack(padx=5, pady=5)

LabelInfo = ttk.Label(form2,text="Доп.Информация")
LabelInfo.pack()

entry = ttk.Entry(form2,width=50)
entry.pack()
# Определяем столбцы

columns = ("name", "prichina", "data")
tree = ttk.Treeview(form2, columns=columns, show="headings")

tree.heading("name", text="ФИО")
tree.heading("prichina", text="Причина")
tree.heading("data", text="Дата")
tree.pack(fill=BOTH, expand=1)

btn21 = ttk.Button(form2, text="Итог", command=submit_form)  # "Итог"
btn21.pack()

zamena = []
def on_select(event): #Для выбора строки в таблице
    global zamena
    zamena = []
    if not tree.selection():
        return

    # Получаем id первого выделенного элемента
    selected_item = tree.selection()[0]

    # Получаем значения в выделенной строке
    values = tree.item(selected_item, option="values")
    zamena.append(values[0])
    zamena.append(values[1])
    print(zamena)

tree.bind('<<TreeviewSelect>>', on_select)

form2.pack_forget()
root.mainloop()#```
