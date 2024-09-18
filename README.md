# 1 mkdir
```mkdir biosoft
mkdir -p project/rat
cd project/rat
mkdir annotation genome sequence output script
```
# 2 install tools
basic workfolw:
* download
* decompress
* change dir
* config
* make
* export PATH
## 2.1 sra
download and convert data
```cd ~/biosoft
wget https://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/2.11.3/sratoolkit.2.11.3-win64.zip
unzip -d sratoolkit.2.11.3-win64.zip
mv sratoolkit.2.11.3-win64 sratoolkit.2.11.3
cd sratoolkit.2.11.3/bin

#set PATH
export PATH="$(pwd):$PATH"

#or:brew install sratoolkit
```
## 2.2 fastqc
```
wget https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.12.1.zip
unzip fastqc_v0.12.1.zip

#brew install fastqc
```
## 2.3 multiqc
```
pip install multiqc
```
## 2.4 cutadapt
```
pip install cutadapt
```
## 2.5 quality control and trim adapter
* Trim Galore
```
wget https://github.com/FelixKrueger/TrimGalore/archive/0.6.3.tar.gz -O TrimGalore.gz
```
*trimmomatic
```
wget http://www.usadellab.org/cms/uploads/supplementary/Trimmomatic/Trimmomatic-0.38.zip
```
## 2.6 hisat2
```
wget http://www.di.fc.ul.pt/~afalcao/hisat2.1/hisat2.1_Windows.zip
```
## 2.7 sortmerna[optional]

## 2.8 samtools
```
wget -c https://github.com/samtools/samtools/releases/download/1.21/samtools-1.21.tar.bz2
cd samtools-1.21
./configure --prefix=$(pwd)
make -j 4
export PATH="$(pwd):PATH"
```
## 2.9 HTseq
```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple HTseq
```
## 2.10 parallel
```
brew install parallel
```
## 2.11 StringTie[optional]
```
wget http://ccb.jhu.edu/software/stringtie/dl/stringtie-2.2.3.tar.gz
```
## 2.12 Ballgown[optional]
```
wget https://bioconductor.org/packages/release/bioc/bin/windows/contrib/4.4/ballgown_2.36.0.zip
```
# 3 data download
## 3.1 ref data
### 3.1.1 genomes data(.fasta)
* download from Ensembl
```
wget https://ftp.ensembl.org/pub/release-112/fasta/rattus_norvegicus/dna/Rattus_norvegicus.mRatBN7.2.dna.toplevel.fa.gz
```
* decompress
* rename
* check(chr...)
* clear some additional text
```
cat rn6.raw.fa | perl -ne 'if(m/^>(.+?)(?:\s|$)/){ print ">$1\n";}else{print}' > rn6.fa
```
* count chr lengths

### 3.1.2 Genome Index File[optional]
* hisat2
### 3.1.3 annotation(.gff)

## 3.2 Experimental Data(.sra)
* NCBI-GEO >> GEO accession
* SRA Run Selector
* download
```
nohup prefetch SRR2190795 SRR224018{2..7} SRR2240228 -o . &
```
* convert format(.sar > .fastq > .gz)
```
parallel -j 4 "
    fastq-dump --split-3 --gzip {1}
" ::: $(ls *.sra)
rm *.sra
```
# 4 quality control
fastqc [选项][测序文件]

