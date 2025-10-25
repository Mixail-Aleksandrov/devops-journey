 Lesson 5.2 —  adduser_skript

**Дата:** 22.10.2025  
**Время:** 4 часа 30 минут   
**Прогресс:** 100% 

https://www.youtube.com/watch?v=G_T8Ad7hhqA&list=PL8jIzbooWPdU5eGYZSaICE6Ux4qBlZSGq&index=3

## Что я сделал


-  Создал скрипт для добавления нового пользователя и разобрал его

#!/bin/bash

set -e

read -p "login:" login
read -p "password:" -s password
read -p "path of ssh public key:" -e ssh_pub_path

homedir=/home/$login
ssh_dir=$homedir/.ssh

sudo mkdir -p "$ssh_dir"

sudo cat "$ssh_pub_path" >> "$ssh_dir/authorized_keys"
sudo cp -rT /etc/skel $homedir

sudo useradd -d $homedir -s /bin/bash $login
sudo chown -R $login:$login $homedir

echo "$login:$password" | sudo chpasswd

cat /etc/passwd | grep $login
