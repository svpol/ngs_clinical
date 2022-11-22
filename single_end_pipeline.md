# 1 Quality assessment and improving

To assess and improve the quality of a sample, do the following steps:

1. Obtain the quality info with `fastqc`:

   ```
   fastqc <sample>.fastq
   ```

2. Study the data received with and make a decision how to improve the quality of the sample. E.g., to remove reads with len < 50 and with quality < 20 use this `trimmomatic` command:

   ```
   java -jar /usr/share/java/trimmomatic.jar SE -phred33 <sample>.fastq <sample_trimmed>.fastq TRAILING:20 MINLEN:50
   ```

   For further work use the trimmed file.

# 2 Alignment

The second step is to map the sample with a reference genome:

1. Before the alignment index the reference sequence:

   ```
   bwa index <ref_seq>.fasta
   ```

2. Perform the alignment:

   ```
   bwa mem <ref_seq>.fasta <sample_trimmed>.fastq > <sample>.sam
   ```

3. Convert sam to bam:

   ```
   samtools view <sample>.sam -b -o <sample>.bam
   ```

4. Sort the bam file:

   ```
   samtools sort -T /tmp/ -o <sample_sorted>.bam <sample>.bam
   ```

5. Index the sorted bam:

   ```
   samtools index <sample_sorted>.bam
   ```

6. See stats about the aligned reads:

   ```
   samtools flagstat <sample_sorted>.bam
   ```

# 3 Comparison the sample with the reference

To compare the sample with the reference and find the difference with the clinical meaning follow the steps:

1. Create a bcf file:

   ```
   bcftools mpileup -Ob -o <sample>.bcf -f <ref_seq>.fasta <sample_sorted>.bam
   ```

2. Convert bcf to vcf to obtain a file with SNPs, then unzip it:

   ```
   bcftools call -cv -o <sample>.vcf.gz <sample>.bcf
   gunzip <sample>.vcf.gz
   ```

# 4 VCF annotation

To annotate and obtain the info about clinically associated SNPs follow the step:

1. Convert vcf to annovar-readable file (avinput):

   ```
   perl /<path>/convert2annovar.pl -format vcf4 <sample>.vcf > <sample>.avinput
   ```
2. Assumed that the databases are already downloaded (with the command `perl /<path>/annotate_variation.pl -buildver hg38 -downdb -webfrom annovar <db_name> humandb/`), annotate the avinput using chosen databases:

   ```
   perl /<path>/annotate_variation.pl -filter -dbtype <db_name> -buildver hg38 <sample>.avinput -outfile <annotation> /<path>/annovar/humandb
   ```

Then you will see the SNP and will be able to make a conclusion.
