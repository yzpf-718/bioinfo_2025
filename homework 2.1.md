wc -l test_command.gtf
wc -c test_command.gtf
grep '^chr_' test_command.gtf | grep '"YDL248W"'
sed 's/chr_/chromosome_/g' test_command.gtf | cut -f 1,3,4,5
awk '{ tmp = $2; $2 = $3; $3 = tmp; print}' test_command.gtf | sort -k 4n -k 5n > result.gtf
ls -hl
chmod 774 test_command.gtf
ls -hl