#!/bin/bash
> output.txt
sed -i 's/google/yandex/g' input.txt

for line in $(cat input.txt)
do
  if [[ $line =~ ^https?:// ]]
    then
        scheme=$(echo $line | grep -E -o "^https?:\/\/")
        line="$(echo ${line/$scheme/})"
        echo "Scheme:" $scheme >> output.txt
  fi

  host1=$(echo $line | grep -P -o '^.*?(:|/|$)')

  if [[ $host1 =~ .*(:|/) ]]
    then
      if [[ $host1 =~ .*: ]]
        then
          host1=$(echo $host1 | rev | cut -c 2- | rev)
          line=$(echo $line | sed -E "s/^${host1}//")
          port=$(echo $line | grep -P -o -m1 '^:[0-9]+' | head -1 | cut -c 2-)
          line="$(echo ${line/:$port/})"
          echo "Host:" $host1 >> output.txt
          echo "Port:" $port >> output.txt
        else
          host1=$(echo $host1 | rev | cut -c 2- | rev)
          line="$(echo ${line/$host1/})"
          echo "Host:" $host1 >> output.txt
      fi
    else
      line=$(echo $line | sed -E "s/^${host1}//")
      echo "Host:" $host1 >> output.txt
  fi

  if [[ $line =~ ^/?.*=.* ]]
    then
      echo "Args:" >> output.txt
      line=$( echo $line | sed -e "s/\/?//")
      while [[ $line =~ .*=.* ]]
        do
          first=$(echo $line | grep -P -o -m1 '^.*?=' | head -1 | rev | cut -c 2- | rev)
          second=$(echo $line | grep -P -o -m1 '=[^(&|$)]*' | head -1 | cut -c 2-)
          line=$(echo ${line/$first=$second/})
          line=$( echo $line | sed -e "s/&//") 
          echo "  Key: ${first}; Value: ${second}" >> output.txt
        done
  fi
  echo >> output.txt
done
                               
