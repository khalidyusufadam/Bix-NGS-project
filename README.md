# Bix-NGS-project
This project was developed after the completion of the 'Next Generation Sequencing (NGS) Informatics' module at Cranfield University, UK

## Introduction
This is a report of the transcriptomic analysis of the impact of climate change on mycotoxin production in Fusarium langsethiae. Two simulated climate change conditions and a normal condition experimental data were analysed using RNA-seq analysis tools and software. The analysis targeted the differential expression of trichothecene genes, which are involved in the biosynthesis of trichothecene mycotoxin in Fusarium (Zuo et al., 2022, Yli-Mattila et al., 2011) across the normal and the climate conditions. The analysis found two differentially expressed genes (tri3 and tri10) related to the biosynthetic pathway. of the trichothecene mycotoxin in Fusarium langsethiae. A breakdown of the analysis pipeline is given below.

## Question 1 – Quality Control
The provided fastq files were analysed using the FastQC module. The quality scores of all the raw reads were observed to be more than 20% which is a good score for further analysis. Some of the sample files were observed to have even higher quality scores of more than 30% as shown below. Thus, fastqc trimmer was not applied.

 ![image](https://github.com/user-attachments/assets/167c8733-98cd-4463-8dd1-a9a13630acf3)

Figure1: Showing the quality score of B009951 sample file.

 ![image](https://github.com/user-attachments/assets/497b006a-f57a-4097-9d66-9d22e626f04a)

Figure2: Showing the quality score of B50983 sample file.

However, Trimmomatic was used to trim adaptor sequences and unpaired reads. The script rna-seq_Trimmomatic_fus.sub attached to this report was used for this purpose. Below is the number of reads before and after trimming of the sample’s files:

<img width="438" alt="image" src="https://github.com/user-attachments/assets/8244c089-7367-4586-ac31-9cf93d631e3c" />


## Question 2: Read Alignment
 Alignment was conducted using STAR and BWA (rna-seq_star.sub and rna-seq_bwa.sub scripts respectively). Although a very good score of >97% primary mapped and >94% properly mapped was observed across all samples using BWA aligner, no percentage of uniquely mapped was provided by the aligner. In the other hand, a score of >74% uniquely mapped was provided by the STAR aligner, which is the most important score for RNA-Sequencing analysis, and therefore STAR alignment results were used for the rest of the analysis. Read counts was performed using HTSeq (rna-seq_star_HTSeq.sub) which converted the generated bam files into read count text files.

 ![image](https://github.com/user-attachments/assets/cbd7e234-42b1-4216-ba75-55389eff1ab0)

Figure3: A screenshot of the STAR alignment logout file of one the samples


## Question 3: Differential Gene Expression and Downstream analysis in R
The read count text files were exported to R and the following analysis were conducted: for this part of the analysis, the following libraries were used: library(DESeq2), library("RColorBrewer"), library("gplots"), library("RColorBrewer"), library(pheatmap), library(ggplot2), library(VennDiagram)

a.	A multivariate analysis of the samples was conducted, and the following plots were generated:

 ![image](https://github.com/user-attachments/assets/6a87c646-8cc0-4230-b02a-471c01a27b47)

Figure4: MA plot of normalized counts

![image](https://github.com/user-attachments/assets/36cdda72-56dc-452d-bf5a-4440a7e30040)
 
Figure5: Hierarchical cluster analysis (HCA) plot of the normal and climate conditions of the experiment.

 ![image](https://github.com/user-attachments/assets/14553686-62a0-4309-b1cf-bac7b04d9220)

Figure6: Principal component analysis (PCA) plot of the normal and climate conditions of the experiment.

Comment: from the above plots, we can observe an overall good clustering of the replicates across the two experimental conditions in both the HCA and PCA plots. 20-0.995-n represent the normal condition of 20oC temperature and 0.995aw water activity, while 25-0.98-c and 25-0.995-c represents the two climate conditions 25oC temperatures and 0.98aw and 0.995aw respectively.

b.	The total number of over- and under-expressed genes for each contrast was generated using a padj value of 0.05 and log2FoldChange of 0 as shown below. The other p-values (0.01 and 0.1) were also tested but just like the 0.05, only two genes related to the trichothecene mycotoxin synthesis were found which are tri3 and tri10, discussed in the later part of this report. Since 0.05 p-value is mostly used as a standard and because not more or less genes of interest to the analysis were found in the other adjusted p-values, 0.05 was used for further analysis of the experimental data.

 ![image](https://github.com/user-attachments/assets/d8df6e29-21b3-4804-b32d-73ec29a8a68e)

Figure7: Shows the total number of up- and down-regulated genes between normal (20-0.995-n) and climate (25-0.995-c) conditions.

 ![image](https://github.com/user-attachments/assets/9c1d5f1f-31b5-4ddd-becb-73a9ca26d990)

Figure8: Shows the total number of up- and down-regulated genes between normal (20-0.995) and climate (25-0.98-c) conditions.

 ![image](https://github.com/user-attachments/assets/f1be36e7-fe4b-404e-9c1b-cba407ac4dea)

Figure9: Shows the total number of up- and down-regulated genes between climate (25-0.995-c) and climate (25-0.995-c) conditions.

c.	An intersection of the differentially expressed genes across all the contrasts showing the number of common and unique differentially expressed genes between the three conditions was performed.

 ![image](https://github.com/user-attachments/assets/017e2f86-1150-4b0d-a5da-130f5f798dcb)

Figure10: An intersection, showing the number differentially expressed genes across the conditions. NB: normal = 20oC aw0.995, climateA = 25 oC aw0.995 and climateB = 25 oC aw0.98.

Comment – from the venn diagram, 1230 genes are only differentially expressed in the normal_vs_climateA contrast, 623 for climateA_vs_climateB, 697 for normal_vs_climateB, 1819 of the genes are differentially expressed in the normal_vs_climateA and climateA_vs_climateB contrasts, 879 genes for climateA_vs_climateB and normal_vs_climateB contrasts, 1925 for normal_vs_climateB and normal_vs_climateA and 1041 genes across the contrasts. 

d.	Treatment impact on trichothecene genes expression: Only two genes related to the synthesis of trichothecene mycotoxin were observed to be differentially expressed in the dataset and they include tri3 (g3785.t1) and tri10 (g3245.t1).

Tri3 (g3785.t1) – is number 3833 in the list of differentially expressed genes of the experimental data at the adjusted p-value of 0.05. The below is a heatmap plot of the differentially expressed genes between 3830 and 3840.

 ![image](https://github.com/user-attachments/assets/ccf098ab-4d60-44bd-9b8e-b4e3908519f6)

Figure11: Showing the differential expression level of tri3 (g3245.t1) gene across the experimental groups. NB: blue = down-regulation, red = up-regulation.

Comment: from the plot, we can observe an up-regulation of the tri3 gene in the normal (20-0.995) group and a down-regulation in the simulated climate conditions. A further down-regulation can be observed in 25-0.995 climate condition compared to 25-0.98 climate condition.

Biological Interpretation – Trichothecene are toxic fungal mycotoxins that inhibit protein translation in eukaryotes which increases the pathogenicity of some Fusarium species.
tri3 gene encode the enzyme acetyl transferase which catalyse one of the 10 reactions in the biosynthesis of trichothecene mycotoxin. Particularly it converts 15-decalonectrin to calonectrin which is a crucial step in the synthesis of T-2 mycotoxin as shown in the below pathway (Villafana et al., 2019, Garvey et al., 2009). 

 ![image](https://github.com/user-attachments/assets/76b4959d-da3e-4c36-9c9f-47f12d8cdefc)

https://www.ncbi.nlm.nih.gov/corecgi/tileshop/tileshop.fcgi?p=PMC3&id=704433&s=84&r=2&c=2,

Figure12: Showing a portion of the T-2 toxin biosynthesis catalysed by tri3 gene.

Thus, a down-regulation of the tri3 gene in the two climate conditions will signifies reduced infection because it will negatively impact the synthesis of the] acetyltransferase enzyme which plays an important role in the T-2 toxin biosynthesis and vice versa in the control (Fig11.). Hence, we can infer that both the simulated climate conditions negatively impacted the ability of the Fusarium langsethiae to cause infection in the conducted experiment with more of the reduced effect been in the 25oC aw0.995 condition.
 
tri10 (g3245.t1) – this gene is number 1506 in the list of differentially expressed (DE) genes of the analysed experimental data at the adjusted p-value of 0.05. tri10 is a gene encoding a transcription factor (Tag et al., 2001). Below is a heatmap of the DE genes between 1500 and 1510.

 ![image](https://github.com/user-attachments/assets/d5456e92-af55-49d2-a56b-c0a87fc29440)

Figure13: Showing the expression level of tri10 (g3785.t1) gene across the experimental groups.

Comment – from the above plot, we can observe an up-regulation of the tri10 gene in the normal condition and a down-regulation in the climate conditions. Just like in case of tri3 gene, we can also observe more down-regulation in the 25-0.995 climate condition compared to the 25-0.98 and this confirms an additional effect of the water activity between the two climate conditions.

Biological Interpretation – Trichothecene genes like other mycotoxin genes are expressed as a biosynthetic gene cluster (BGC) and exist in three loci in the Fusarium genome. Apart from tri16, all the other tri-genes that are of interest in this analysis (tri6, tri10, tri15, tri13, tri9, tri13, tri14) are in the two-gene locus including tri3 and tri10 (Villafana et al., 2019) that were found to be differentially expressed in this analysis. Since tri10 gene is a transcription factor and exist within the biosynthetic gene cluster, down-regulation of the gene can significantly affect the expression of the other tri genes. Hence, down-regulation of tri3 gene observed earlier may be associated with down-regulation of this gene. 
However, tri10 gene is not the only transcription factor in the BGC of Fusarium; tri6 is also a transcription factor in the cluster as shown in fig14. Therefore, our inability to observe a differential expression of the other tri-genes in the cluster may be associated with the other transcription factor not differentially expressed. As such we may conclude that the simulated climate conditions of this experiment only influenced the differential expression of some of the genes involved in the synthesis of trichothecene mycotoxin in the Fusarium langsethiae.

 ![image](https://github.com/user-attachments/assets/912b7b6b-6a28-4616-8f8f-44f81c4b58ed)

https://www.ncbi.nlm.nih.gov/corecgi/tileshop/tileshop.fcgi?p=PMC3&id=704428&s=84&r=1&c=3

Figure14: Showing biosynthetic gene clustering of tri-genes in some Fusarium species.

Comment: we can observe a different arrangement of the biosynthetic gene cluster of the tri-genes in different species of Fusarium. The configuration pattern and the location of the transcription factors in a specific Fusarium species may impact the expression of the other genes in the cluster.

4. RNA-Seq variant calling was performed using the BCFtools module (variantcalling.sub script), duplicates were marked using the MarkDuplicates function of the picard module and the resulting marked.bam files were converted to vcf files. The vcf files were then used to call the variants using the bcftools call function.

References
Tag, A. G., Garifullina, G. F., Peplow, A. W., Charles Ake, J., Phillips, T. D., Hohn, T. M., & Beremand, M. N. (2001). A Novel Regulatory Gene, Tri10, Controls Trichothecene Toxin Production and Gene Expression. Applied and Environmental Microbiology, 67(11), 5294-5302. https://doi.org/10.1128/AEM.67.11.5294-5302.2001

Villafana, R. T., Ramdass, A. C., & Rampersad, S. N. (2019). Selection of Fusarium Trichothecene Toxin Genes for Molecular Detection Depends on TRI Gene Cluster Organization and Gene Function. Toxins, 11(1). https://doi.org/10.3390/toxins11010036

Yli-Mattila, T., Ward, T. J., O'Donnell, K., Proctor, R. H., Burkin, A. A., Kononenko, G. P., Gavrilova, O. P., Aoki, T., McCormick, S. P., & Gagkaeva, T. Y. (2011). Fusarium sibiricum sp. nov, a novel type A trichothecene-producing Fusarium from northern Asia closely related to F. sporotrichioides and F. langsethiae. International journal of food microbiology, 147(1), 58–68. https://doi.org/10.1016/j.ijfoodmicro.2011.03.007

Zuo, Y., Verheecke-Vaessen, C., Molitor, C., Medina, A., Magan, N., & Mohareb, F. (2022). De novo genome assembly and functional annotation for Fusarium langsethiae. BMC genomics, 23(1), 158. https://doi.org/10.1186/s12864-022-08368-0

