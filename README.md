# postfix
## To install this script you need some permissions first:
## Do the following ya sadiki : chmod +x ./postfix.sh
# enjoy!! 




On sauvegarde d’abord les mails lus de 2019–2020 :
find /home/f.touil/Maildir/cur/ -type f -newermt 2019-01-01 ! -newermt 2021-01-01 -print0 | \
tar --null -czvf /bneder/home/mails_f.touil_2019_2020.tar.gz --files-from=-

Vérifications
tar -tzf /bneder/home/mails_f.touil_2019_2020.tar.gz | head

Efface les origineaux
find /home/f.touil/Maildir/cur/ -type f -newermt 2019-01-01 ! -newermt 2021-01-01 -delete

Restauration
tar -xzvf /bneder/home/mails_f.touil_2019_2020.tar.gz -C /home/f.touil/Maildir/cur/

Vérifications
ls -lh /home/f.touil/Maildir/cur/ | head



find /home/f.touil/Maildir/cur/ -type f -newermt 2021-01-01 ! -newermt 2022-01-01 -exec mv {} /home/f.touil/Maildir/.Trash/cur/ \;
find /home/f.touil/Maildir/.Trash/ -type f -newermt 2021-01-01 ! -newermt 2022-01-01 -delete

du -sh /home/*/Maildir/.Trash 2>/dev/null | awk '
function toKB(size, unit) {
    if (unit=="K") return size;
    if (unit=="M") return size*1024;
    if (unit=="G") return size*1024*1024;
}
{
    n=$1; u=substr(n, length(n));
    gsub(",",".",n); # si locale utilise virgule pour décimales
    if (u ~ /[KMG]/) {
        val=substr(n,1,length(n)-1);
        total+=toKB(val,u);
    }
}
END { printf "Total Trash = %.2f GB\n", total/1024/1024 }'


rm -rf /home/*/Maildir/.Trash/* /home/*/Maildir/.Trash/.[!.]* /home/*/Maildir/.Trash/..?*
