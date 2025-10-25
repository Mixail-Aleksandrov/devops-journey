Lesson 5.3 —  rights_for_user

**Дата:** 25.10.2025  
**Время:** 5 часов   
**Прогресс:** 100% 

https://www.youtube.com/watch?v=FJJg7xTni48&list=PL8jIzbooWPdU5eGYZSaICE6Ux4qBlZSGq&index=4

## Что я сделал


-  Создал скрипт и разобрал его

#!/bin/bash

set -e

login=$1
homedir=/home/$login
list_name_dir=(secret_user secret_grup secret_other)

ready_dir_list=()

for name in ${list_name_dir[@]};
do
        mkdir -p $homedir/${login}_${name}
        #chown -R $login:$login $homedir

        if [[ $(echo $name | grep other) ]];
        then
                chmod -R 007 $homedir/${login}_${name}
        elif [[ $(echo $name | grep grup) ]];
        then
                chmod -R 070 $homedir/${login}_${name}
        elif [[ $(echo $name | grep user) ]];
        then
                chmod -R 700 $homedir/${login}_${name}
        fi
done

counter=$(ls $homedir/ | wc -l)
while [ $counter -gt 0 ];
do
        counter=$(( $counter - 1 ))
        access_rules=$(ls -ld ${ready_dir_list[$counter]} | cut -d' ' -f 1)
        type_of_rules=$(echo ${ready_dir_list[$counter]} | cut -d'_' -f 4)

        if [[ ${access_rules:1} == "---rwx---" ]] && [[ ${type_of_rules} == "grup" ]]
        then
                echo "RULES FOR" ${type_of_rules}
                echo $access_rules
                echo "GJ"
        elif [[ ${access_rules:1} == "------rwx" ]] && [[ ${type_of_rules} == "other" ]]
        then
                echo "RULES FOR" ${type_of_rules}
                echo $access_rules
                echo "GJ"
        elif [[ ${access_rules:1} == "rwx------" ]] && [[ ${type_of_rules} == "user" ]]
        then
                echo "RULES FOR" ${type_of_rules}
                echo $access_rules
                echo "GJ"
        else
                echo -------------------------
                echo "ОШИБКА" ${type_of_rules}
                echo -------------------------
        fi
done
