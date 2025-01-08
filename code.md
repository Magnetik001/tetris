import random
import os

anticheat = False
score = 0
time = 0
attempts = 0

figuresPos = [
    [(0, 0), (0, 1)], [(0, 0), (0, 1), (0, 2)], [(0, 0), (1, 0)], [(0, 0), (1, 0), (2, 0)],
    [(0, 0), (1, 0), (2, 0), (2, 1)], [(0, 0), (1, 0), (2, 0), (2, -1)], [(0, 0), (0, 1), (0, 2), (1, 2)],
    [(-1, 2), (0, 0), (0, 1), (0, 2)]
    ]

weight = int(input("Введите размер игрового поля: "))

table = [[" " for _ in range(weight)] for _ in range(weight)]

while attempts < 3:

    os.system("cls")if not anticheat else print(end="")

    print("Рекомендуется ввести команду '! help', если вы играете в первый раз") if time == 0 else print(end="")
    for i in table:
        print(i)
    print()

    time += 1 if not anticheat else 0
    figure = random.choice(figuresPos) if not anticheat else figure
    watchTable = [[" " for _ in range(5)] for _ in range(5)]
    for i in figure:
        watchTable[i[1] + 2][i[0] + 2] = "0"

    for i in watchTable:
        print(i)

    print(f"Ход: {time}")
    print(f"Счет: {score}")

    # Ввод координат или команд
    x, y = input("Введите координаты фигуры 'x y' ").split()
    if x == "!" and y == "end":
        print("Спасибо за игру!")
        break
    if x == "!"and y == "help":
        print('''1 - Ввод размера игрового поля
2 - Показ игрового поля
3 - Показ поля с фигурой (центр - начальные позиции фигуры)
4 - Ход игры
5 - Счет игры
              
! end - закончить игру
! help - помощь
              
Правила:
Ставить фигуры на поле и набирать очки,
если 3 раза фигура не оказалась на поле вы проиграли''')
        
        input()
        continue

    newPos = []
    for i in figure:
        posX, posY = int(x) - 1 + i[0], weight - int(y) + i[1]
        if posX > weight - 1 or posX < 0 or posY > weight - 1 or posY < 0:
            print("Вы вышли за диапозог доступных значений!")
            print("Пожалуйста выберите новые координаты")
            anticheat = True
            attempts += 1
            break
        elif table[posY][posX] == "0":
            print("В этом месте уже стоит фигура!")
            print("Пожалуйста выберите новые координаты")
            anticheat = True
            attempts += 1
            break
        newPos.append((posY, posX))
    else:
        anticheat = False
        attempts = 0
        for i in newPos:
            table[i[0]][i[1]] = "0"

    for i in range(len(table)):
        if table[i] == ["0" for _ in range(weight)]:
            table[i] = [" " for _ in range(weight)]
            score += 100

    for i in range(len(table)):
        lst = []
        for j in table:
            lst.append(j[i])
        if lst == ["0" for _ in range(weight)]:
            for n in range(len(table)):
                table[n][i] = " "
            score += 100
else:
    print("Вы проиграли!")
    print(f"Счет: {score}, ход: {time}")
