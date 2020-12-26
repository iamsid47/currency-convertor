---
name: 'Currency Converter - Python'
description: 'This is a simple currency convertor made using Python'
author: '@iamsid47'
img: https://github.com/iamsid47/hangman-pics/blob/main/hangman.png
---

Hi everyone! In this workshop, we will be making a **currency coverter** which will convert currency a pair of currency. We will be converting INR to USD in this workshop as the default value and later adding a dropdown to select other currencies as well. This program will completely be in Python. This is a GUI (Graphical User Interface) based project.

You can try out this project on Repl.it by clicking [this]() link.

## Files & Libraries

So, there will be just one main file named as `main.py`. Since this is a GUI based project, we will be required to install `tkinter` as well. ALong with this we are also required to have the `requests` library for (http/https).

## Let's Get Started

First, let's head over to [Repl.it](https://repl.it) and create a *repl*. Choose the language as Python and name the repl.

If you have not installed the  `tkinter` library, head over to the **shell** and type in: `pip install tkinter` and let the library get installed.

Now let's import the required the libraries to our `main.py`.

```python
import requests
from tkinter import *
import tkinter as tk
from tkinter import ttk
```

So, this is going to be a real-time currency convertor and not have a pre-defined value for our currencies. Let's first create a class named `App` and define some functions in it.

The first function can be named as `__init__` which will initialize the python program. Next, we will add some dimensions to our program (example: length, breadth, title, etc.)

```python
class App(tk.Tk):

  def __init__(self, converter):
     tk.Tk.__init__(self)
     self.title = 'Currency Converter'
     self.currency_converter = converter
     self.geometry("500x200")
```

After this, we add in some labels like something to welcome, a converter label (example: Indian Rupee equals = ), and the date.

```python
self.intro_label = Label(self, text = 'Welcome to Real Time Currency Convertor',  fg = 'blue', relief = tk.RAISED, borderwidth = 3)
 self.intro_label.config(font = ('Courier',15,'bold'))

 self.date_label = Label(self, text = f"1 Indian Rupee equals = {self.currency_converter.convert('INR','USD',1)} USD \n Date : {self.currency_converter.data['date']}", relief = tk.GROOVE, borderwidth = 5)
```

Now we define that where do we want to place these labels.

```python
self.intro_label.place(x = 10 , y = 5)
self.date_label.place(x = 160, y= 50)
```
Next, we want to add a box to take in the input from the user and later convert it to USD.

```python
valid = (self.register(self.restrictNumberOnly), '%d', '%P')
        self.amount_field = Entry(self,bd = 3, relief = tk.RIDGE, justify = tk.CENTER,validate='key', validatecommand=valid)
        self.converted_amount_field_label = Label(self, text = '', fg = 'black', bg = 'white', relief = tk.RIDGE, justify = tk.CENTER, width = 17, borderwidth = 3)
```

Here, the input has a borderwidth, width, and the color of the background set to white. This is a simple box with a label inside it to make it something which is interactive and looks a bit better.

Let's add in the *dropdown* menu for our users to switch between different currencies.

```python 
self.from_currency_variable = StringVar(self)
self.from_currency_variable.set("INR") # default value
self.to_currency_variable = StringVar(self)
self.to_currency_variable.set("USD") # default value
```

Here, we have just set the default values. That is the starting conversion will be from **INR TO USD**. Next, we add in the other ones. In the same time, we also define the font of the dropdown and some other customizations. We will also set the state to be **readonly** so that the user can't input anything here and just **select** the pairs.

Note that there will be 2 dropdowns as this is a pair.

```python
font = ("Courier", 12, "bold")
self.option_add('*TCombobox*Listbox.font', font)

self.from_currency_dropdown = ttk.Combobox(self, textvariable=self.from_currency_variable,values=list(self.currency_converter.currencies.keys()), font = font, state = 'readonly', width = 12, justify = tk.CENTER)

self.to_currency_dropdown = ttk.Combobox(self, textvariable=self.to_currency_variable,values=list(self.currency_converter.currencies.keys()), font = font, state = 'readonly', width = 12, justify = tk.CENTER)
```

Next, we define the place where we want these dropdowns to be. For the same, let's define their placing.

```python
self.from_currency_dropdown.place(x = 30, y= 120)
        self.amount_field.place(x = 36, y = 150)
        self.to_currency_dropdown.place(x = 340, y= 120)
        #self.converted_amount_field.place(x = 346, y = 150)
        self.converted_amount_field_label.place(x = 346, y = 150)
```

We also want a **Convert** button so that the user can press it and the converted value is displayed inside the other currency's box.

```python
self.convert_button = Button(self, text = "Convert", fg = "black", command = self.perform) 
        self.convert_button.config(font=('Courier', 10, 'bold'))
        self.convert_button.place(x = 225, y = 135)
```

Next, we define a function in which we create 4 variables. `amount`, `from_curr`, `to_curr`, and `converted_amount`. The `amount` variable will get the input from the user and convert it to *float* whilst the `from_curr` and `to_curr` will define the *from currency* and the *to currency*.

The `converted_amount` will be the variable which stores the converted value and will be used to later display it into the box which we used earlier. Also, when we convert currencies, there is a chance that the value will be more that 3-4 decimals. Thus, to keep it simple, we will round off this currency and make the max decimal place to be **2**.

```python
def perform(self):
        amount = float(self.amount_field.get())
        from_curr = self.from_currency_variable.get()
        to_curr = self.to_currency_variable.get()

        converted_amount = self.currency_converter.convert(from_curr,to_curr,amount)
        converted_amount = round(converted_amount, 2)
```
Next, we convert this this amount given by `converted_amount` to a string.

```python
self.converted_amount_field_label.config(text = str(converted_amount))
```
Now there is a possibility that the user enters a *text* input instead of an integer. To restrict text being converted and throwing an error, we create a function called `restrictNumberOnly`. Inside we put in `regex` to form a certain pattern which will start and end only with an integer. Then we take this, create a variable named `result`, and then not let the conversion take place. 

```python
def restrictNumberOnly(self, action, string):
 regex = re.compile(r"[0-9,]*?(\.)?[0-9,]*$")
 result = regex.match(string)
 return (string == "" or (string.count('.') <= 1 and result is not None))
```

We then type the initialization code and add in an URL and create a class called `RealTimeCurrencyConverter` which will be equal to `convert`.

```python
if __name__ == '__main__':
    url = 'https://api.exchangerate-api.com/v4/latest/USD'
    converter = RealTimeCurrencyConverter(url)
```

Now let's create the class `RealTimeCurrencyConverter`. This class will contain 2 functions namely `__init__` and `convert`. Inside `__init__` we will enter that the requests from the url (above) shall be stored in a variable named `self.data` and **rates** from this variable will be fetched by another variable named `self.currencies`.

Next, the `convert` function will do things according to it's name and that is to convert the currencies. First, the initial amount shall be equal to `amount` and then we will be adding a conditional to it that if the `from_currency` is equal to **USD** then the output will be equal to `from_currency` only.

```python
class RealTimeCurrencyConverter():
def __init__(self,url):
   self.data = requests.get(url).json()
   self.currencies = self.data['rates']

def convert(self, from_currency, to_currency, amount): 
  initial_amount = amount 
  if from_currency != 'USD' : 
            amount = amount / self.currencies[from_currency] 
```
Lastly, we want to limit the precision of `amount` to 4 so that the output doesn't get a lot more congested. Thus;

```python
amount = round(amount * self.currencies[to_currency], 4) 
return amount
```

## Voila! You Did It :)

You just made your very own GUI based currency convertor.

## Hack It

You can surely host this convertor and turn it into a web app!

## Demos
 
 1.
 2.
 3.
