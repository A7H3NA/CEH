# Injection(Web) - Writeup

## description

web/digits-of-pi  
cppio

There's more to this [spreadsheet](https://docs.google.com/spreadsheets/d/1y7AxYvBwJ1DeapnhV401w0T5HzQNIfrN1WeQFbnwbIE/edit) than meets the eye.


## answer

When you access the specified URL, the Google Sheets screen will appear.

![001-1](https://user-images.githubusercontent.com/45488828/122660709-4239ae80-d1be-11eb-9660-f120c7156a4d.jpg)

The source sheet exists as a hidden sheet.

![002-1](https://user-images.githubusercontent.com/45488828/122660714-4960bc80-d1be-11eb-855e-569ac47e81d1.jpg)

Searching for the flag string will give you access to the flag listed in the hidden source sheet.

![003-1](https://user-images.githubusercontent.com/45488828/122660716-4f569d80-d1be-11eb-8dfc-2c0d77ae65d9.jpg)

###### flag{hidden_sheets_are_not_actually_hidden}
