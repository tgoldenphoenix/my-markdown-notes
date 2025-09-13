# Date mnemonic

## Month code

January: 1 => It's the first month.

February: 4 => The Fab Four, the Beatles.

March: 4 => An army marching forward (four) ngày 30/4 năm 75.

April: 0 => Tháng 4 là lời nói dối của em. Mà lời nói dối nên anh không được cái gì hết.

May: 2 => I may thing of May, or I may not. It's a **twofold** choice.

June: 5 => Stan Laurel (born in June 1890) playing with a beehive

July: 0 => Kim Phượng không thèm nhắn tin với mình.

August: 3 => Augustus arrest people with handcuffs

September: 6 => Quốc khánh 2/9, bã chó đọc tuyên ngôn ở Ba Đình, kế bên có con voi bản đôn

October: 1 => An octopus carrying a candle under water.

November: 4 => Aang (tháng 11) with a sailboat on the serene sea with Appa flying alongside.

December: 6 => An Hào có cái "vòi voi" cực khủng

## Day code

Sunday is `1`, Monday is `2`, Tuesday `3`

- Wed 4
- Thursday 5
- Fri 6
- Saturday 7

until Saturday.

## The calculation

Mod 7

Saturday 7 => 0

7th of the month => 0

22nd of the month => 22mod7=1

## Year code

You must memorize the year code from `00XX` -> `99XX` (the format is `YYXX`)

All 100 year codes split up into 7 areas. A journey of your house divided into 7 stages each stage has multiple numbers.

### How to calculate the year code

To calculate the year code `(YY + (YY div 4)) mod 7`.

YY is the last two digits of the year. For the year 1997, it’s 97.

First, divide YY by 4 and discard the remainder: 97 div 4 = 24.

Then add 24 back into the YY number, which is 97 in this case, resulting in 121.

The next step is: 121 mod 7. So we’ve removed all the sevens from 121 until we are left with a remainder of 2. That is the Year Code for 1997.

## References

[reddit](https://www.reddit.com/r/LearnUselessTalents/comments/avb5bi/how_do_i_learn_to_calculate_the_day_of_the_week/) how to calculate year code

[different methods](https://forum.artofmemory.com/t/calendar-calculation-year-code-techniques/63056) to remember year codes

[what happen](https://forum.artofmemory.com/t/dominic-obriens-calender-method/28544) with leap year like 2000
