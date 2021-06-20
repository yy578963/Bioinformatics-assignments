samtools faidx ~/Bioinformatic_ assignments/input/reference/reference.fa 

Picard java -jar picard.jar CreateSequenceDictionary \R=reference.fasta \O=reference.dict 

fastp -i ~/Bioinformatic_ assignments/input/rawdata/rawdata_1.fastq.gz \
-o ~/Bioinformatic_ assignments/input/cleandata/clean_data1.fastq.gz \
-I ~/Bioinformatic_ assignments/rawdata/rawdata_2.fastq.gz \
-O ~/Bioinformatic_ assignments/cleandata/clean_data2.fastq.gz 

bwa mem -t 4 -R '@RG\tID:sample_name\tPL:illumina\tSM:sample_name' \
~/Bioinformatic_ assignments/input/reference/reference.fa \
~/Bioinformatic_ assignments/cleandata/clean_data1.fastq.gz \
~/Bioinformatic_ assignments/cleandata/clean_data2_fastq.gz > ~/Bioinformatic_ assignments/output/sample_name.sam

samtools view -Sb ~/Bioinformatic_ assignments/output/sam/sample_name.sam > ~/Bioinformatic_ assignments/output/bam/sample_name.bam 

samtools sort -@ 4 -m 4G -O bam -o ~/Bioinformatic_ assignments/output/sorted/sample_name.sorted.bam ~/Bioinformatic_ assignments/output/bam/sample_name.bam

sambamba markdup -r ~/Bioinformatic_ assignments/output/sorted/sample_name.sorted.bam ~/Bioinformatic_ assignments/output/marked/sample_name.sorted.markdup.bam

gatk HaplotypeCaller \
-R ~/Bioinformatic_ assignments/input/reference/reference.fa --emit-ref-confidence GVCF \
-I ~/Bioinformatic_ assignments/output/marked/sample_name.sorted.markdup.bam \
-O ~/Bioinformatic_ assignments/output/gvcf/sample_name.g.vcf

find ~/Bioinformatic_ assignments/output/gvcf/ -name "*.g.vcf" > input.list

gatk CombineGVCFs \
-R ~/Bioinformatic_ assignments/input/reference/reference.fa \
--variant input.list \
-O ~/Bioinformatic_ assignments/output/gvcf/combined.g.vcf

gatk GenotypeGVCFs \
-R ~/Bioinformatic_ assignments/input/reference/reference.fa \
-V ~/Bioinformatic_ assignments/output/gvcf/combined.g.vcf \
-O ~/Bioinformatic_ assignments/output/vcf/samples.vcf

bedtools subtract -a Patients_samples.vcf -b Normal_samples.vcf > Positive_samples.vcf 


