# **STAR: A Brief Introduction to Its Algorithm and Usage**
### Name: Yang Han, Zhaogu Sun, Simona Wu (Team 21)
## **Background and Introduction:** 
RNA sequencing (RNA-Seq) is prevalently used for analyzing gene expression and transcriptomic profiles. It involves several key computational steps to process raw sequencing data, with read alignment to a reference genome being a critical step. STAR, which stands for "Spliced Transcripts Alignment to a Reference", is a commonly used tool for this RNA-seq. STAR is known for its ability to handle large RNA-Seq datasets.

STAR operates on Linux via the command line, aligning trimmed .fastq files to an indexed reference genome. The tool produces output files in .sam or .bam formats, which contain the aligned reads necessary for downstream analyses, such as gene quantification and splice junction identification.

The RNA-Seq workflow begins with biological sample preparation and sequencing, followed by quality control steps like adapter trimming and sequence evaluation using tools such as FASTQC. STAR facilitates the alignment process, accurately mapping reads to the genome while accounting for splicing events. Once the reads are aligned, they are quantified to associate them with genes, enabling statistical analysis to identify differentially expressed genes.

STAR’s alignment capability supports essential steps in RNA-Seq workflows, ensuring the accurate representation of transcriptomic data for downstream interpretation.
![]()

## **STAR Alignment Overview**
### **Seed-and-Extension Strategy in STAR Alignment**
STAR employs a "seed-and-extension" strategy to efficiently map sequencing reads to a reference genome. This approach balances computational efficiency with alignment accuracy, making it powerful for RNA-Seq data analysis.

### **Seed Phase**
The first phase of the seed-and-extension strategy involves identifying short sequences, called "seeds," within the sequencing reads. These seeds are chosen because they exactly match subsequences in the reference genome. By focusing on exact matches, STAR can utilize efficient string search algorithms such as the Burrows-Wheeler Transform (BWT) or Suffix Arrays to rapidly locate potential regions of alignment. This step significantly reduces the search space, narrowing the possible locations where the full read might align.

#### **Advantages of Seeds:**
- Efficiency: Exact matching enables the use of precomputed indices like the Suffix Array or BWT, speeding up the search process.

- Scalability: This approach allows the alignment algorithm to handle large genomes and datasets.

#### **Disadvantages of Seeds:**
- Memory Intensity: Building and storing indices for the reference genome can require substantial computational resources.

- High Computational Cost: Despite the efficiency of exact match algorithms, the sheer volume of reads in high-throughput sequencing datasets can make this step computationally expensive.

### **Extension Phase**
Once potential seed locations are identified, the algorithm extends these matches to align the entire read. This involves dealing with mismatches, insertions, deletions, and splicing events that may occur within the read. STAR is especially good at this phase by:

- Allowing for gaps in alignments to accommodate introns, which is particularly important for RNA-Seq data.

- Using scoring systems to evaluate alignment quality and select the best match for each read.

## **STAR Algorithm Illustration**
### Step1: Finding the longest prefixes in reads that exactly matches the reference genome
![]()
STAR starts by finding the longest prefix in reads that exactly match some (>= 1) locations in the reference genome. The current longest prefix in a read is called a Maximal Mapping Prefix. As stated previously, STAR uses uncompressed suffix arrays to conduct string search in the reference genome. This reduces the runtime of aligning a sequence from O(n^2) to at most O(n*log2(n)) and greatly reduces the memory usage from O(n^2) to O(n). By statistical probability, longer sequences tend to map to fewer locations in the reference genome, which offers less genome coordinates to look at at later steps.  Additionally, by doing this, STAR minimizes the unmapped regions in reads, which saves some effort for later alignment tasks.

### Step 2: Mapping the the unmapped regions to the reference genome
#### Scenario 1: Able to find exact match in the reference genome for the unmapped portion: 
![]()
If STAR is able to find some exact match for the entire unmapped portion at some genome locations after the first MMP, then we are done. However, in many cases, we are not so lucky.

#### Scenario 2: Minor mismatch between unmapped region and the reference genome




