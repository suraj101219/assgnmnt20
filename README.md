# assgnmnt20


Q. Set the variable test1 to the string 'This is a test of the emergency text system,' and save test1 to a file named test.txt.


test1 = 'This is a test of the emergency text system,'
filee = open('test.txt','w')
filee.write(test1)
     
 output: 44
Q. RRead the contents of the file test.txt into the variable test2. Is there a difference between test 1

and test 2?


file2 = open('test.txt','r')
test2 = file2.readline()
test2
     
 output: 'This is a test of the emergency text system,'

if test1==test2:
    print('Both are same')
     
 output:Both are same
Q. Create a CSV file called books.csv by using these lines:

title,author,year The Weirdstone of Brisingamen,Alan Garner,1960 Perdido Street Station,China Miéville,2000 Thud!,Terry Pratchett,2005 The Spellman Files,Lisa Lutz,2007 Small Gods,Terry Pratchett,1992


import csv
rows =[ ['title','author','year'],
    ['The Weirdstone of Brisingamen','Alan Garner',1960],
    ['Perdido Street Station','China Miéville',2000],
    ['Thud!','Terry Pratchett',2005],
    ['The Spellman Files','Lisa Lutz',2007],
    ['Small Gods','Terry Pratchett',1992]]
with open('books.csv','w',newline='') as file:
    writer = csv.writer(file)
    writer.writerows(rows)
     
Q.Use the sqlite3 module to create a SQLite database called books.db, and a table called books with these fields: title (text), author (text), and year (integer).


import sqlite3
conn = sqlite3.connect('books.db')
c = conn.cursor()

c.execute('create table books(title varchar(20),author varchar(20), year int)')
conn.commit()
     
Q. Read books.csv and insert its data into the book table.


import pandas as pd

read_books = pd.read_csv('books.csv',encoding='unicode_escape')
read_books.to_sql('books', conn, if_exists='append', index = False)
     
Q. Select and print the title column from the book table in alphabetical order.


c.execute('select title from books order by title asc')
print(c.fetchall())
     
[('Perdido Street Station',), ('Small Gods',), ('The Spellman Files',), ('The Weirdstone of Brisingamen',), ('Thud!',)]
Q. From the book table, select and print all columns in the order of publication.


c.execute('select title, author,year from books order by year')


df = pd.DataFrame(c.fetchall(), columns=['title','author','year'])
df
     
 output: title	author	year
0	The Weirdstone of Brisingamen	Alan Garner	1960
1	Small Gods	Terry Pratchett	1992
2	Perdido Street Station	China MiÃ©ville	2000
3	Thud!	Terry Pratchett	2005
4	The Spellman Files	Lisa Lutz	2007
Q. Use the sqlalchemy module to connect to the sqlite3 database books.db that you just made in exercise 6.


import sqlalchemy
engine = sqlalchemy.create_engine("sqlite:///books.db")
rows = engine.execute('select * from books')
for i in rows:
    print(i)
     
('The Weirdstone of Brisingamen', 'Alan Garner', 1960)
('Perdido Street Station', 'China MiÃ©ville', 2000)
('Thud!', 'Terry Pratchett', 2005)
('The Spellman Files', 'Lisa Lutz', 2007)
('Small Gods', 'Terry Pratchett', 1992)
Q. Install the Redis server and the Python redis library (pip install redis) on your computer. Create a Redis hash called test with the fields count (1) and name ('Fester Bestertester'). Print all the fields for test.


!pip install redis
     
Collecting redis
  Downloading redis-3.5.3-py2.py3-none-any.whl (72 kB)
     |████████████████████████████████| 72 kB 440 kB/s 
Installing collected packages: redis
Successfully installed redis-3.5.3

import redis
conn = redis.Redis()
conn.delete('test')
conn.hmset('test', {'count': 1, 'name': 'Fester Bestertester'})
conn.hgetall('test')
     
Q. Increment the count field of test and print it.


conn.hincrby('test','count', 3)
     
