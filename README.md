from tkinter import *
import math

def btn_click(item):
    global expression
    try:
        input_field["state"] = "normal"
        expression += item
        input_field.insert(END, item)
        
        if item == "=":
            result = str(eval(expression[:-1]))
            input_field.insert(END, result)
            expression = ""
        input_field["state"] = "readonly"            

    except ZeroDivisionError:
        input_field.delete(0, END)
        input_field.insert(0, "Ошибка (деление на 0)")
        expression = ""
        input_field["state"] = "readonly"
    except SyntaxError:
        input_field.delete(0, END)
        input_field.insert(0, "Ошибка синтаксиса")
        expression = ""
        input_field["state"] = "readonly"

def bt_clear():
    global expression
    expression = ""
    input_field["state"] = "normal"
    input_field.delete(0, END)
    input_field["state"] = "readonly"

root = Tk()
root.title("Калькулятор")
root.geometry("700x600")
root.resizable(0, 0)

# Настройка шрифтов
button_font = ('Arial', 20)
entry_font = ('Arial', 24)

frame_input = Frame(root)
frame_input.grid(row=0, column=0, columnspan=4, sticky="nsew", padx=10, pady=10)
input_field = Entry(frame_input, font=entry_font, width=20, state="readonly", justify=RIGHT)
input_field.pack(fill=BOTH, expand=True, ipady=20)

buttons = (
    ("7", "8", "9", "/"),
    ("4", "5", "6", "*"),
    ("1", "2", "3", "-"),
    ("0", ".", "=", "+")
)

expression = ""

# Кнопка Clear
Button(root, text="C", font=button_font, command=bt_clear, height=2).grid(
    row=1, column=3, sticky="nsew", padx=0, pady=0)

# Настройка веса строк и столбцов
for i in range(5):
    root.grid_rowconfigure(i, weight=1)
for i in range(4):
    root.grid_columnconfigure(i, weight=1)

for row in range(4):
    for col in range(4):
        Button(root, text=buttons[row][col], font=button_font,
               command=lambda row=row, col=col: btn_click(buttons[row][col])).grid(
            row=row + 2, column=col, sticky="nsew", padx=0, pady=0)

root.mainloop()
