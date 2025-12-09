

The following directives can be embedded in the format string:

Directive    Meaning
%a     Weekday name.
%A     Full weekday name.
%b     Abbreviated month name.
%B     Full month name.
%c     Appropriate date and time representation.
%d     Day of the month as a decimal number [01,31].
%H     Hour (24-hour clock) as a decimal number [00,23].
%I     Hour (12-hour clock) as a decimal number [01,12].
%j     Day of the year as a decimal number [001,366].
%m     Month as a decimal number [01,12].
%M     Minute as a decimal number [00,59].
%p     Equivalent of either AM or PM.
%S     Second as a decimal number [00,61].
%U     Week number of the year (Sunday as the first day of the week) as a decimal number [00,53]. All days in a new year preceding the first Sunday are considered to be in week 0.
%w     Weekday as a decimal number [0(Sunday),6].
%W     Week number of the year (Monday as the first day of the week) as a decimal number [00,53]. All days in a new year preceding the first Monday are considered to be in week 0.
%x     Appropriate date representation.
%X     Apropriate time representation.
%y     Year without century as a decimal number [00,99].
%Y     Year with century as a decimal number.
%Z     Time zone name (no characters if no time zone exists).
%%     A literal '%' character.






## HOW TO GET CURRENT YEAR

```python
from datetime import datetime
year = datetime.strftime(datetime.today(), '%Y')


# OR


import time
time.strftime("%Y")


# OR



from datetime import datetime
StartDate = "10/10/11"
current_date=datetime.now()

# if u need year 
current_date.year

# if u need day 
current_date.day

# if u need month 
current_date.month
```



### Date manipulation in python:

1- http://learnopenerp.blogspot.com/2018/01/python-date-manipulation.html
2- http://learnopenerp.blogspot.com/2018/02/python-strftime-datetime-formatting.html
3- http://learnopenerp.blogspot.com/2018/02/python-timedelta.html

```python
from datetime import date
from datetime import time
from datetime import datetime   

today = datetime.now()
# Output: 
2020-07-17 08:19:24.123456

today = date.today()
# Output: 
2020-07-17

current_time = datetime.time(datetime.now())
# Output:
08:19:24.123456

today.day
# Output: 
17

today.month
# Output: 
07

today.year
# Output: 
2020

today.weekday()
# Output: 
5
```




```python
# weekday returns number 0 to 6 means (Monday to Sunday)
    wd = date.weekday(today)
    days = ["Monday","Tuesday","Wednesday","Thursday","Friday","Saturday","Sunday"]
    print("Today is day number: ",wd)
    print("The Day is: ",days[wd])   


************************************************************************************************************

from datetime import datetime

def main():
    # Times and dates can be formatted using a set of predefined string    
    now = datetime.now() # get the current date and time
    
    # Date Formatting Controll Codes
    # %y/%Y - Year, %a/%A - weekday, %b/B% - month, %d - day of month
    
    print("Full Date: ",now.strftime("%Y"))
    print("Abbreviated Date: ",now.strftime("%y"))
    
    print(now.strftime("%a, %d %B, %y"))  # will print out Sat, 17 February, 18
    
    # %c - locale's date and time, %x - locale's date, %X - locale's time
    print(now.strftime("%c"))
    print(now.strftime("%x"))
    print(now.strftime("%X"))
    
    # Formatting Time
    # %I%H - 12/24 Hour, %M - minute, %S - second, %p - locale's AM/PM
    print(now.strftime("%I:%M:%S:%p")) # 12-Hour:Minute:Second:AM
    print(now.strftime("%H:%M")) #24-Hour:Minute
    

************************************************************************************************************

```







```python
import datetime

string = '2018-05-17 10:44:50'
dt = datetime.datetime.strptime(string,'%Y-%m-%d %H:%M:%S')
print dt.time()
print dt.time().hour
print dt.time().minute

# Output:
10:44:50
10
44
```





dt = datetime.datetime.strptime(string,'%Y-%m-%d %H:%M:%S')

time = dt.time()

date = dt.date()




```python
from datetime import datetime
timeObj = datetime.now()
hour = timeObj.time().hour
minute = timeObj.time().minute
float('%s.%s' % (hour, minute))
```



```python

# To find the date of the day in odoo 10



# Yenthe Van Ginneken (Mainframe Monkey)
# 13 februari 2018
# Hi Silviaa,

# This question is rather about Python than just Odoo but to answer your question: 
#     you should use the Python packages date, datetime or calendar in order to get these values out. 
#     There are quite some ways to do this, but here is an example with the calendar package:

import calendar

c = calendar.Calendar(firstweekday=calendar.MONDAY)
year = 2018; month = 2
monthcal = c.monthdatescalendar(year,month)
mondays = [day for week in monthcal for day in week if \
                day.weekday() == calendar.MONDAY and \
                day.month == month]
for monday in mondays:
    print('monday: ' + str(monday))



# Or an example with datetime:

import datetime

today = datetime.date.today()
day = datetime.date(today.year, today.month, 1)
single_day = datetime.timedelta(days=1)
mondays = 0
dates = []
while day.month == today.month:
    if day.weekday() == 0:
        mondays += 1
        dates.append(day)
    day += single_day
print('Number of mondays this month:'+ str(mondays))
for date in dates:
    print('date: ' + str(date))

# Regards,
# Yenthe
```