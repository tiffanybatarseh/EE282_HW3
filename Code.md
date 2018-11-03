wget ftp://ftp.flybase.net/genomes/Drosophila_melanogaster/current/fasta/dmel-all-chromosome-r6.24.fasta.gz

wget ftp://ftp.flybase.net/genomes/Drosophila_melanogaster/current/fasta/md5sum.txt

md5sum -c md5sum.txt

module load jje/kent
module load jje/jjeutils

faSize dmel-all-chromosome-r6.24.fasta.gz

wget ftp://ftp.flybase.net/genomes/Drosophila_melanogaster/current/gtf/dmel-all-r6.24.gtf.gz

md5sum -c md5sum.txt

awk '{print $3}' dmel-all-r6.24.gtf \
| sort \
| uniq -c \
| sort -rn \
| nl

awk '{print $1,$3}' dmel-all-r6.24.gtf \
| grep '[XY234][LR]\?' \
| grep 'gene' \
| sort \
| uniq -c \
| sort -rn \
| nl
