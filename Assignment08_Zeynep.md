# ------------------------------------------------------------------------ #
# Title: Assignment 08 - Final
# Description: Working with classes

# ChangeLog
# Zeynep , 8/23/23 , Modified code to complete assignment 8
# ------------------------------------------------------------------------ #

import pickle

# CLASSES -------------------------------------------------------------------- #

# Object  -------------------------------------------- #
class Product:
    def __init__(self, name, price):
        self.product_name = name
        self.product_price = price

    @property
    def product_name(self):
        return str(self.__product_name).title()

    @product_name.setter
    def product_name(self, value):
        self.__product_name = value

    @property
    def product_price(self):
        return str(self.__product_price).title()

    @product_price.setter
    def product_price(self, value):
        self.__product_price = value

# Processing  ------------------------------------------------------------- #
class FileProcessor:
    @staticmethod
    def read_data_from_file(file_name):
        try:
            with open(file_name, 'rb') as file:
                data = pickle.load(file)
                return data
        except FileNotFoundError:
            data = []
            return data
        except pickle.UnpicklingError:
            print("Error: Invalid data format.")
            return data

    @staticmethod
    def add_product_to_list(name, price, list_of_rows):
        try:
            product = Product(name, price)
            list_of_rows.append(product)
            print("Product added to your list")
        except ValueError as e:
            print(f"Error: {e}")
        return list_of_rows

    @staticmethod
    def remove_data_from_list(name, list_of_rows):
        removed = False
        name = name.lower()
        for row in list_of_rows:
            if row.product_name.lower() == name:
                value = row.product_price
                list_of_rows.remove(row)
                print(f"Product {name} with value {value} has been removed.")
                removed = True
                break
        if not removed:
            print("Product not found")
        return list_of_rows

    @staticmethod
    def save_data_to_file(file_name, list_of_rows):
        try:
            with open(file_name, 'wb') as file:
                pickle.dump(list_of_rows, file)
            print("File saved")
        except Exception as e:
            print("An error occurred:", str(e))

# Presentation (Input/Output)  -------------------------------------------- #
class IO:
    @staticmethod
    def print_menu_items():
        print('''
        Menu of Options
        1) Show current list
        2) Add a new product
        3) Remove an existing product
        4) Save Data to File
        5) Exit Program
        ''')

    @staticmethod
    def input_menu_choice():
        choice = input("Which option would you like to perform? [1 to 5] - ").strip()
        while choice not in ('1', '2', '3', '4', '5'):
            choice = input("Please enter a valid option [1-5]: ").strip()
        return choice

    @staticmethod
    def print_current_list_items(list_of_rows):
        for row in list_of_rows:
            print(f"Name: {row.product_name}, Price: {row.product_price}")

    @staticmethod
    def input_product_name():
        while True:
            name = input("Enter a product name: ").strip()
            if not name:
                print("Name cannot be empty. Please enter a valid product name.")
                continue
            if name.isnumeric():
                print("Error: Names cannot be numbers. Please enter a valid product name.")
                continue
            else:
                return name

    @staticmethod
    def input_product_price():
        while True:
            price = input("Enter a product price: ").strip()
            if not price:
                print("Price cannot be empty. Please enter a valid product price.")
                continue
            elif not price.isnumeric():
                print("Error: Prices must be numbers. Please enter a valid product price.")
                continue
            else:
                return price

# Data -------------------------------------------------------------------- #
strFileName = 'products.txt'
lstOfProductObjects = FileProcessor.read_data_from_file(strFileName)

# Main Body of Script  ---------------------------------------------------- #
while True:
    IO.print_menu_items()
    choice = IO.input_menu_choice()

    if choice.strip() == "1":
        if len(lstOfProductObjects) == 0:
            print("List is yet empty. You should add some products")
        else:
            IO.print_current_list_items(lstOfProductObjects)
        continue

    elif choice.strip() == "2":
        name = IO.input_product_name()
        if name:
            price = IO.input_product_price()
            if price:
                lstOfProductObjects = FileProcessor.add_product_to_list(name, price, lstOfProductObjects)
        continue

    elif choice.strip() == "3":
        name2remove = input("Enter the name of the product to remove: ")
        lstOfProductObjects = FileProcessor.remove_data_from_list(name2remove, lstOfProductObjects)
        continue

    elif choice.strip() == "4":
        FileProcessor.save_data_to_file(strFileName, lstOfProductObjects)
        continue

    elif choice.strip() == '5':
        print("Goodbye!")
        break
