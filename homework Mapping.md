#### （1） 请阐述bowtie中利用了 BWT 的什么性质提高了运算速度？并通过哪些策略优化了对内存的需求？
**Bowtie利用BWT的压缩特性提高了运算速度：** 
BWT将原始参考基因组序列转换为一种更紧凑的表示形式（BWT字符串），同时保留序列的局部重复模式。这种转换通过将相似字符聚集，提高了后续索引的压缩率。
**Bowtie通过以下策略优化对内存的需求：**
1. 索引压缩技术：基因组中相邻重复字符在BWT转换后会被聚集成连续块，Bowtie利用RLE压缩这些块，将重复字符替换为（字符，长度）对，减少存储空间。
2. 部分后缀数组存储：仅存储稀疏的后缀数组样本（如每隔32或64个位置存储一个样本），而非完整的SA。当需要定位具体匹配位置时，通过BWT的逆向转换逐步回溯至最近的样本点，再通过计算补全中间位置。此方法将SA的内存占用降低至原始的1/32~1/64。
3. 位级数据压缩：使用紧凑的位表示存储BWT和辅助数据结构。例如将基因组字符（A/T/C/G）用2位表示，而非传统8位的ASCII编码。
4. 内存映射与按需加载：将索引文件分割为多个区块，仅将当前比对所需的索引部分加载到内存，而非一次性载入全部数据。通过操作系统内存映射技术实现高效的部分加载。
#### （2）用bowtie将 `THA2.fa` mapping 到 `BowtieIndex/YeastGenome` 上，得到 `THA2.sam`，统计mapping到不同染色体上的reads数量(即统计每条染色体都map上了多少条reads)。
code:
```
bowtie -v 1 -m 10 --best --strata BowtieIndex/YeastGenome -f THA2.fa -S THA2.sam
grep -v '^@' THA2.sam | awk '$3 != "*" {print $3}' | sort | uniq -c | sort -nr
```
result:
```
    187 chrIV
    158 chrXII
    119 chrVII
     97 chrXV
     77 chrXVI
     68 chrX
     64 chrVIII
     61 chrXIII
     57 chrXIV
     53 chrXI
     51 chrII
     31 chrV
     23 chrIX
     17 chrVI
     16 chrI
     14 chrIII
      9 chrmt
```
#### （3）回答以下问题:
#### （3.1）什么是sam/bam文件中的"CIGAR string"? 它包含了什么信息?
CIGAR string，全称Compact Idiosyncratic Gapped Alignment Report，用于描述测序Read与参考基因组的比对细节。其核心是通过“操作符+长度”的组合记录信息
其中，操作符包括M（匹配/错配）、I（插入）、D（删除）、N（参考序列的大段跳过）等
#### （3.2）"soft clip"的含义是什么，在CIGAR string中如何表示？
Soft clip，即软剪切，指Read的某部分未能比对到参考基因组，但这些未比对部分仍保留在Read序列中（未被切除）。在CIGAR中通过S标记，通常出现在Read的开头或结尾。
#### （3.3）什么是reads的mapping quality? 它反映了什么样的信息?
Mapping Quality是衡量Read比对到当前位点正确性的置信度，以Phred分数形式呈现。
公式：$P = 10^{-\frac{MQ} {10.0}}$
其中，P为错误率，MQ为Mapping Quality。可以看出，MQ越大，错误率越低，数据质量越好。
#### （3.4）仅根据sam/bam文件的信息，能否推断出read mapping到的区域对应的参考基因组序列? 
是可行的。通过通过CIGAR和MD标签可推断参考基因组序列。CIGAR提供了比对的结构，而MD标签明确了了参考序列的具体差异，故将二者结合可以推断参考序列。