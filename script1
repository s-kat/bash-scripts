x=-1
grade=1
ans=0
base=$(((10 ** 9) + 7))
for line in $(cat input.txt)
do
  if [[ $x -eq -1 ]]; then
    x=$line
  else
    ans=$((($ans + ($line * $grade)%base)%base))
    grade=$((($grade * $x)%base))
  fi
done
ans=$(($ans % $base))
echo $ans > output.txt
