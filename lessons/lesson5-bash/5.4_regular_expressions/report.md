Lesson 5.4 —  regular_expressions

**Дата:** 25.10.2025  
**Время:** 2 часа 30 минут
**Прогресс:** 100% 

https://www.youtube.com/watch?v=pCh2ww4KeVs&list=PL8jIzbooWPdU5eGYZSaICE6Ux4qBlZSGq&index=5

## Что я сделал


- Оформил уроки в VScode
-  Создал скрипт и разобрал его
   
#!/bin/bash

set -e

login=$1
year_month_day=$2

if [ -z "$login" ] || [ -z "$year_month_day" ]; 
then
    echo "Usage: $0 <login> <year_month_day>"
    exit 1
fi

comm=$(journalctl -S "$year_month_day" | grep -E "sudo|${login}" | grep -oE "COMMAND=([^ ]+)" | cut -d"=" -f 2 | sort -u)
file_dir="/home/cuba/${login}_${year_month_day}.log"

{
    echo "---------------"
    echo "LOGIN: $login"
    echo "DATE: $year_month_day"
    
    
    while IFS= read -r line; 
    do
        command_count=$(journalctl -S "$year_month_day" | grep -E "sudo|${login}" | grep -F "COMMAND=$line" | wc -l)
        echo "COMMAND: ${line}:${command_count}"
    done <<< "$comm"
    
    echo "---------------"
} > "$file_dir"  