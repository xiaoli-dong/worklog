# Day-to-Day Work Log

# June 1, 2026, Monday, rainy day
- Lab meeting: 
- Continue to work on monkeypox data analysis for Jamil's paper
  - Primarily concerns tecovirimat, the leading antiviral medication
    - Tecovirimat selectively targets the extracellular enveloped virus formation protein, F13L (also known as gp045) encode VP37, a conserved protein across all orthopoxviruses (OPXVs) involved in secondary envelope formation. [ref] F13L gene is strongly conserved across all known members of the orthopoxvirus genus(https://doi.org/10.1093/infdis/jiaf471) 
    - It targets the F13 protein, blocking the virus from leaving infected cells. Resistance mutations in the F13 gene prevent the drug from binding.
    - mutations in the viral target protein (the F13 gene) can emerge, particaularly during prolonged use in  severely immunocompromised indeviduals. 
- Cidofovir & Brincidofovir
  -  Alternative drugs that inhibit viral DNA replication but carry higher risks of toxicity.
---
## June 2, 2026 (Tuesday, rainy day)

- Mpox sequencing follow-up with Kanti: Runs done with COVID samples, so barcode detection requires both ends, are we throwing away too many sequences because the amplicon size is around 2500bps
  - Why both-end barcodes
    - Reduce barcode misclassification
    - Detect barcode hopping/chimeras
    - Improve consensus accuracy
    - Trade-off: some reads are lost due to:
      - missing barcode at one end
      - truncated reads
      - low-quality ends
  - R10 run summary
    - 240822_S_N_132-barcode73: 308,189 total / 12,052 failed (3.91%)
    - 240822_S_N_132-barcode74: 441,071 total / 16,767 failed (3.80%)
    - 240822_S_N_132-barcode73 QC note:
      - avg read length ~615 bp → likely pre-barcoding fragmentation, which suggests read loss from dual-end requirement is limited
  - Conclusion
    - For clinical samples, keeping barcode detection at both ends is still reasonable.
    - This is important given variable viral loads and the higher sensitivity of low-coverage samples to contamination.
- Run nf-taxflow on bac_16S\260525_N_I_052\analysis_apl\results

---
## June 3, 2026 (Wednesday, sunny day)
- transfer the nf-taxflow to bac_16s for matthew
- look into the arbovirus probe pane sequence results: \APL_Genomics\nextseq\260508_S_I_025, the following is the workflow used in Andrew Cameron group in univerisy of regina. (from Daniel.Kos@uregina.ca, Daniel Kos)
  - Part 1: I'll usually start with fastp, then filter out my host reads (my typical patients go moo) with bowtie2 where I keep the reads that don't map. Even if this doesn't get all the reads, it at least reduces downstream processing times. then into Megahit with a minimum contig length of 500 bp. Usually already the top few contigs will be significant. As you've already done, I would also run kraken2 on a custom relevant database from the filtered reads with a minimum ID of 80. 
  
    ```
    bowtie2 -x ${REF}/Bos_taurus -1 $RRP/${SRR}_R1.fastq.gz -2 $RRP/${SRR}_R2.fastq.gz \
      --un-conc-gz Post_Host_Removal/${SRR}_NotBT.fastq.gz \
      --met-file Post_Host_Removal/Metrics_${SRR}.txt \
      --no-unal --no-hd --no-sq -p 16 --mm > /dev/null 
    ```
  - Part 2. (iterative): This is where it can get a little bit more manual. Based on your kraken2 results you'll want to put together a reference database to map to. If you got any complete genomes in your MAGS, definitely put those in too! My personal tool of choice for this is bbsplit for getting a count and bbmap for viewing overall coverage.

     ```
     bbsplit.sh ref=$input_dir_G/MLST_CDS.fasta \
       in=$input_dir_F/${MP}.fastq.1.gz \
       in2=$input_dir_F/${MP}.fastq.2.gz \
       minid=0.95 \
       local=t \
       scafstats=$output_dir/${MP}_MLST_Mapped_Scafstats.tsv

     bbmap.sh ref=$input_dir_G/MLST_CDS.fasta \
       in=$input_dir_F/${MP}.fastq.1.gz \
       in2=$input_dir_F/${MP}.fastq.2.gz \
       kfilter=25 subfilter=20 maxindel=3 \
       outm=$output_dir/${MP}_MLST_Mapped.sam \
       insfilter=0 delfilter=0\
       ambiguous=all\
       minid=0.95 \
       local=t \
       secondary=t\
       pairedonly=t\
       refstats=$output_dir/${MP}_MLST_Mapped_RefStats.tsv \
       scafstats=$output_dir/${MP}_MLST_Mapped_Scafstats.tsv
     ```
  - You may go through all that and see that the Kraken2 results aren't being fully covered by your reference database.  What I'll usually do is map again with a lower minid of ~80% on a reference genome of what it says it should be mapping to and try to figure out where those reads are going to, and what if existent would be better on NCBI. Then I would add that to the database for BBMap and BBSplit and run again. Obviously the process is greatly simplified if you pull the BBsplit numbers into R to build an overview of where reads are going to for each sample.

## Thursday
- Arbovirus proble meeting with Daniel kos,Ashlyn Kirk, Andrew Cameron
  - update with the run
    - workflow from Ashlyn:  illumina rna capture,  not total nuclitide acid
    - regina create their own virus databases (source is ncbi high quality data such as refseq)

---

## Monday, June 8, Sunny
- lab meeting: need slrum, linux competency form
- working on background reading of mpox

## Tuesday
- request mpox final report format
- got linux, slurm competency sign-off form from anita
- drafted linux, slurm sign off forms and sent it off for feedback

## Wednesday
- Got metadata from Jamil and integrated it with the mpxv_master.tsv
- the mpox run tracker has some key and run id consistency issues, fixed it.
- Linux sequencer all failed in the same way becuase the nanopore certificate failed. update the key 
## Thursday
- CPHLN meeting
  - measle (hiebert joanne presentation): measeq pipeline, atcc reference genome, N450 submit to MeanNS, n450 tree, wgs submitted to pathoplexus.org
  - Sequencer upgrade: suspend for now but work with IT to get our system properly backed up. Met with chandra and rehan bari to discuss the possibility. The answer is yes and I will create a ticket tomorrow morning with chandra to move this forward.

## Friday
- work with chandra to create ticket for backup sequencers to IT
- generated slurm tutorial
- 
