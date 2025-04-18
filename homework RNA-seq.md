### （1）请阐述 RNA-seq 中归一化基因表达值的几种基本计算方法。
RNA-seq归一化基因表达值有以下几种方法：
**1.CPM/RPM(Counts/Reads Per Million mapped reads)** 
$CPM/RPM=\frac{比对到目的基因上的Reads数}{总比对Reads数}\times10^6$
该方法仅矫正测序深度，未矫正测序长度。因此适用于基因长度相近或已单独校正长度的情况，如small RNA-seq。
**2.RPKM(Reads Per Kilobase per Million mapped reads)**
$RPKM=\frac{比对到目的基因上的Reads数}{总比对Reads数\times基因长度(kbp)}\times10^6$
该方法矫正了测序深度和测序长度。适用于单端测序的分析。
**3.FPKM(Reads Per Kilobase per Million mapped reads)**
$FPKM=\frac{比对到目的基因上的Fragments数}{总比对Fragments数\times基因长度(kbp)}\times10^6$
其中，fragments指一对reads。因此，FPKM与RPKM特征类似，但适用于双端测序的分析。
**4.TPM(Transcripts Per Million)**
$TPM=\frac{比对到目的基因上的Reads数/基因长度(kbp)}{\sum (比对到某个基因的Reads数/该基因的长度(kbp))}\times10^6$
该方法先对测序长度进行归一化，再对测序深度进行归一化。且每个样本的TPM总和为$10^6$，适合跨样本比较。