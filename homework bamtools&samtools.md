### bedtools&samtools作业
#### （1）我们提供的bam文件`COAD.ACTB.bam`是单端测序分析的结果还是双端测序分析的结果？为什么？
该文件是单端测序分析的结果。原因如下：
在终端中输入如下命令：
```
samtools flagstat COAD.ACTB.bam
```
结果为：
```
185650 + 0 in total (QC-passed reads + QC-failed reads)
4923 + 0 secondary
0 + 0 supplementary
0 + 0 duplicates
185650 + 0 mapped (100.00% : N/A)
0 + 0 paired in sequencing
0 + 0 read1
0 + 0 read2
0 + 0 properly paired (N/A : N/A)
0 + 0 with itself and mate mapped
0 + 0 singletons (N/A : N/A)
0 + 0 with mate mapped to a different chr
0 + 0 with mate mapped to a different chr (mapQ>=5)
```
可以看到，总reads数为185650，而"paired in sequencing"数为0。如果是双端测序，每一段DNA片段会测序得到两个reads，并在文件中会被标记为同一个pair。而单端测序则不会被标记为pair。因此文件`COAD.ACTB.bam`是属于单端测序的结果文件。
#### （2）查阅资料回答什么叫做"secondary alignment"？并统计提供的bam文件中，有多少条记录属于"secondary alignment?" （提示：可以使用samtools view -f 获得对应secondary alignment的records进行统计）
secondary alignment，即次要比对，指的是一个read在参考基因组上有多个比对位点时，在主要比对（primary alignment）之外的其他比对结果。与主要比对相比，次要比对的比对质量较低，可能是由于重复序列、测序错误或同源序列导致的假阳性比对。
在SAM/BAM文件中，次要比对通过FLAG字段的 0x100（十进制256） 位标识（即该位为1时表示次要比对）。
使用命令：
```
samtools view -f 0x100 COAD.ACTB.bam | wc -l
```
输出结果：
```
4923
```
则`COAD.ACTB.bam`文件中存在4923条次要比对记录。
#### （3）请根据hg38.ACTB.gff计算出在ACTB基因的每一条转录本中都被注释成intron的区域，以bed格式输出。并提取COAD.ACTB.bam中比对到ACTB基因intron区域的bam信息，后将bam转换为fastq文件。
编写脚本如下：
```
#/bin/bash
awk 'BEGIN{OFS="\t"} $3 == "gene" {print $1, $4-1, $5, "ACTB_gene", ".", $7}' hg38.ACTB.gff | sort -k1,1 -k2,2n > gene.bed
awk 'BEGIN{OFS="\t"} $3 == "exon" {print $1, $4-1, $5, "exon", ".", $7}' hg38.ACTB.gff | sort -k1,1 -k2,2n > exon.bed
bedtools subtract -a gene.bed -b exon.bed > intron.bed
samtools view -b -L intron.bed COAD.ACTB.bam > COAD.ACTB.intron.bam
samtools fastq COAD.ACTB.intron.bam > COAD.ACTB.intron.fastq
exit 0
```
#### （4）利用COAD.ACTB.bam计算出reads在ACTB基因对应的genomic interval上的coverage，以bedgraph格式输出。 
编写脚本如下：
```
#/bin/bash
samtools sort COAD.ACTB.bam > COAD.ACTB.sorted.bam
samtools index COAD.ACTB.sorted.bam
bedtools genomecov -split -ibam COAD.ACTB.sorted.bam -bg > COAD.ACTB.coverage.bedgraph
exit 0
```
### 课件后作业
#### 1.人类基因组的大小以及基本组成是哪些？
人类基因组大小约为3.1Gb。（数据来源：NCBI，物种Homo Sapiens，参考基因组GRCh38.p14，时间为2022年2月3日。） 
如果根据功能来划分，人类基因组可以分为编码片段和非编码片段。非编码片段又可细分为非编码RNA基因、调控序列（增强子等）、转座子等。
#### 2.2.基因中的非编码RNA的最新注释是多少个了？请详细列一下其中的非编码RNA的细分类型的数目，并对主要的非编码RNA是做什么的用1-2句话解释一下。
目前，人类基因组中的非编码RNA的总数为59291。以下是部分非编码RNA具体细分各类型的数目。
lncRNA	34914
miRNA	1879
Mt_rRNA	2
Mt_tRNA	22
rRNA	47	
scaRNA	49	
snoRNA	942
snRNA	1901	
sRNA	5
vault_RNA	4	
（数据来源：GENCODE。Release 47，参考基因组GRCh38.p14，发布时间2024年10月）
主要的非编码RNA的作用：
lncRNA（长非编码RNA）：长度大于200个核苷酸，参与表观遗传调控、细胞周期调控等。
miRNA（微小RNA）：长度约为20个核苷酸，结合mRNA抑制翻译或促进降解，调控基因表达。
snRNA（小核RNA）：长度约为150个核苷酸，在细胞核中加工pre-mRNA。
snoRNA（小核仁RNA）：在细胞核中指导rRNA，tRNA，snRNA等的化学修饰