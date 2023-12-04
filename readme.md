# Домашнее задание к лекции «Открытие и чтение файла, запись в файл»
from pprint import pprint
import os

# ----------------------------------------------------------
# Задача №1
# ----------------------------------------------------------
cook_book = {}
def get_dishes_from_file(dishes):
    with open('recipes.txt', 'r') as f:
        for idx, line in enumerate(f):
            if type(dishes) == list or type(dishes) == tuple:
                for dish in dishes:
                    if dish == line.strip():
                        cook_book.setdefault(dish, [])
                        for i in range(int(f.readline().strip())):
                            lines = f.readline().strip().split(' | ')
                            cook_book[dish].append({'ingredient_name': lines[0], 'quantity': int(lines[1]), 'measure': lines[2]})
            elif type(dishes) == str:
                if dishes == line.strip():
                    cook_book.setdefault(dishes, [])
                    for i in range(int(f.readline().strip())):
                        lines = f.readline().strip().split(' | ')
                        cook_book[dishes].append({'ingredient_name': lines[0], 'quantity': int(lines[1]), 'measure': lines[2]})

    return cook_book

pprint(get_dishes_from_file(['Омлет','Утка по-пекински', 'Запеченный картофель', 'Фахитос']))
print()


# ----------------------------------------------------------
# Задача №2
# ----------------------------------------------------------

def get_shop_list_by_dishes(dishes, person_count):
    new_dict = {}
    if type(dishes) == list or type(dishes) == tuple:
        for i in range(len(dishes)):
            for dish in cook_book:
                if dish == dishes[i]:
                    for ingredients in cook_book[dish]:
                        for ingredient in ingredients.keys():
                            if ingredients[ingredient] not in new_dict:
                                new_dict.setdefault(ingredients['ingredient_name'], {'measure': ingredients['measure'], 'quantity': ingredients['quantity'] * person_count})
                            else:
                                new_dict[ingredients['ingredient_name']]['quantity'] += (ingredients['quantity'] * person_count)
    elif type(dishes) == str:
        for dish in cook_book:
            if dish == dishes:
                for ingredients in cook_book[dish]:
                    for ingredient in ingredients.keys():
                        new_dict.setdefault(ingredients['ingredient_name'], {'measure': ingredients['measure'], 'quantity': ingredients['quantity'] * person_count})

    sorted_new_dict = dict(sorted(new_dict.items(), key=lambda x: x[0]))

    return sorted_new_dict

pprint(get_shop_list_by_dishes(['Омлет', 'Утка по-пекински'],10))
print()

# ----------------------------------------------------------
# Задача 3
# ----------------------------------------------------------

def files_merger(existing_files, new_file):

    file_values = {}
    file_text = {}

    file1 = existing_files[0]
    file2 = existing_files[1]
    file3 = existing_files[2]

    file_path = os.path.join(os.getcwd(), new_file)

    with open(file1, 'r') as f1, open(file2, 'r') as f2, open(file3, 'r') as f3:
        file_values.setdefault(file1, len(f1.readlines()))
        file_values.setdefault(file2, len(f2.readlines()))
        file_values.setdefault(file3, len(f3.readlines()))

    file_values = dict(sorted(file_values.items(), key=lambda x: x[1]))

    with open(file1, 'r') as f1, open(file2, 'r') as f2, open(file3, 'r') as f3:
        file_text.setdefault(file1, f1.readlines())
        file_text.setdefault(file2, f2.readlines())
        file_text.setdefault(file3, f3.readlines())

    with open(file_path, 'w') as f4:
        for key, value in file_values.items():
            f4.write(f'Имя файла: {key}\nКоличество строк: {value}\n')
            for idx, values in enumerate(file_text[key]):
                f4.write(f'Строка номер {idx+1} файла {key}: {values}')
            f4.write('\n')
    return new_file

print(files_merger(['1.txt', '2.txt', '3.txt'], 'total.txt'))