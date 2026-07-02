# Day-to-Day Work Log

## Monday, June 1, 2026 (Rainy)

- Lab meeting
- Continue work on monkeypox data analysis for Jamil's paper

### Tecovirimat (Leading Antiviral Medication)

- Selectively targets the extracellular enveloped virus formation protein, F13L (also known as gp045), which encodes VP37—a conserved protein across all orthopoxviruses (OPXVs) involved in secondary envelope formation
- The F13L gene is strongly conserved across all known members of the orthopoxvirus genus
- It targets the F13 protein, blocking the virus from leaving infected cells
- Resistance mutations in the F13 gene prevent the drug from binding
- Mutations in the viral target protein (the F13 gene) can emerge, particularly during prolonged use in severely immunocompromised individuals

### Cidofovir & Brincidofovir

- Alternative drugs that inhibit viral DNA replication but carry higher risks of toxicity

---

## Tuesday, June 2, 2026 (Rainy)

### Mpox Sequencing Follow-up with Kanti

- Runs completed with COVID samples
- Barcode detection requires both ends
- Question: Are we discarding too many sequences? Amplicon size: ~2500 bp

#### Rationale for Dual-End Barcodes

- Reduce barcode misclassification
- Detect barcode hopping/chimeras
- Improve consensus accuracy
- Trade-off: Some reads are lost due to missing barcode at one end, truncated reads, or low-quality ends

#### R10 Run Summary

| Sample | Total Reads | Failed | Failed % | Notes |
|--------|-------------|--------|----------|-------|
| 240822_S_N_132-barcode73 | 308,189 | 12,052 | 3.91% | Avg read length ~615 bp, suggesting pre-barcoding fragmentation |
| 240822_S_N_132-barcode74 | 441,071 | 16,767 | 3.80% | |

**QC Note:** Average read length ~615 bp suggests pre-barcoding fragmentation. This indicates that read loss from dual-end barcode requirement is limited.

#### Conclusion

For clinical samples, keeping barcode detection at both ends remains reasonable. This is important given variable viral loads and the higher sensitivity of low-coverage samples to contamination.

#### Actions

- Run nf-taxflow on `bac_16S\260525_N_I_052\analysis_apl\results`

---

## Wednesday, June 3, 2026 (Sunny)

- Transfer nf-taxflow to bac_16s for Matthew
- Investigate arbovirus probe panel sequence results: `\APL_Genomics\nextseq\260508_S_I_025`

### Workflow from Andrew Cameron Group (University of Regina)
*Provided by Daniel Kos*

#### Part 1: Host Removal and Assembly

Start with fastp, then filter host reads using bowtie2 (keeping reads that do not map). Reduce downstream processing times. Run Megahit with minimum contig length of 500 bp. The top contigs are typically significant. Also run Kraken2 on a custom database with minimum identity threshold of 80%.

```bash
bowtie2 -x ${REF}/Bos_taurus -1 $RRP/${SRR}_R1.fastq.gz -2 $RRP/${SRR}_R2.fastq.gz \
  --un-conc-gz Post_Host_Removal/${SRR}_NotBT.fastq.gz \
  --met-file Post_Host_Removal/Metrics_${SRR}.txt \
  --no-unal --no-hd --no-sq -p 16 --mm > /dev/null
```

#### Part 2: Iterative Mapping and Refinement

Based on Kraken2 results, assemble a reference database for mapping. Include complete genomes from MAGs if available. Recommended tools: bbsplit (for count) and bbmap (for coverage visualization).

```bash
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
  insfilter=0 delfilter=0 \
  ambiguous=all \
  minid=0.95 \
  local=t \
  secondary=t \
  pairedonly=t \
  refstats=$output_dir/${MP}_MLST_Mapped_RefStats.tsv \
  scafstats=$output_dir/${MP}_MLST_Mapped_Scafstats.tsv
```

#### Validation and Refinement Process

If Kraken2 results are not fully covered by the reference database:
1. Map again with a lower identity threshold (~80%) to determine where those reads belong
2. Check NCBI for better reference sequences and add them to the bbmap database
3. Repeat analysis
4. Pull bbsplit numbers into R for visualization of read mapping by sample

---

## Thursday, June 6, 2026

### Arbovirus Probe Meeting

**Participants:** Daniel Kos, Ashlyn Kirk, and Andrew Cameron

- Workflow from Ashlyn: Illumina RNA capture (not total nucleotide acid)
- Regina group creates their own virus databases using high-quality NCBI data such as RefSeq

---

## Monday, June 8, 2026 (Sunny)

- Lab meeting: Obtain SLURM and Linux competency form
- Working on background reading of monkeypox

---

## Tuesday, June 10, 2026

- Request monkeypox final report format
- Obtain Linux and SLURM competency sign-off form from Anita
- Draft Linux and SLURM sign-off forms; send for feedback

---

## Wednesday, June 11, 2026

- Obtain metadata from Jamil; integrate with `mpxv_master.tsv`
- Monkeypox run tracker: Fix key and run ID consistency issues
- Linux sequencer: All failed with the same error (nanopore certificate failure). Update the key.

---

## Thursday, June 12, 2026

### CPHLN Meeting

#### Measles (Presentation by Joanne Hiebert)

- MeaSeq pipeline
- ATCC reference genome
- N450 submit to MEAN
- N450 tree
- WGS submitted to Pathoplexus.org

#### Sequencer Upgrade

- Suspend for now but work with IT to properly back up our system
- Met with Chandra and Rehan Bari to discuss the possibility
- Answer is yes; will create a ticket tomorrow morning with Chandra to move forward

---

## Friday, June 13, 2026

- Work with Chandra to create ticket for backup sequencers to IT
- Generate SLURM tutorial

---

## Monday, June 15, 2026 (Sunny)

### Research Meeting

#### Reading MeaSeq Pipeline

Currently using Virarecon to process measles data.

##### Virarecon Parameters

Variant calling performed with iVar variants using options: `-t 0.25 -q 20 -m 10`

- **-q:** Minimum quality score threshold to count base
- **-t:** Minimum frequency threshold to call variants
- **-m:** Minimum read depth to call variants

*Note: No clade or lineage assignment. In the default parameter config, Nextclade is disabled and Pangolin is enabled but does not produce lineage information.*

#### MeaSeq Pipeline

Candidate variant calling performed using Freebayes with final variants and ambiguous positions determined using filtering parameters:

| Parameter | Value | Details |
|-----------|-------|---------|
| Minimum quality score | 20 | Per Freebayes documentation |
| Minimum depth | 10 reads | |
| Allele frequency 0.30–0.75 | Ambiguous | Due to discordance in sequencing data, possibly from viral evolution or sequencing artifacts |
| Allele frequency > 0.75 | Pass filter (SNP) | |
| Indel variants | Passing frequency 0.60 | Lower threshold to account for alignment difficulties in low-complexity regions of the MeV genome |

##### Validation

MeaSeq pipeline validated against ATCC reference sequences using NGS read data from various sequencing approaches. Minimal, low-impact differences observed between 472 MeaSeq consensus sequences and reference sequences.

**Note:** The rule-of-six divisibility check is built into MeaSeq.

---

## Tuesday, June 16, 2026

### nf-fluab Bug Fixing

In the nf-fluab v1.0.4, nanopore mappings were not filtered before generating depth. Supplementary alignment and count profiles were inflating the count. Deployed v1.0.5 to fix this issue.

---

## Wednesday (Sick Day)

---

## Thursday, June 18, 2026 (Sunny)

- Sent email to Colin about path issues associated with R and VSCode
- Responded to Kara's email to add IRVC NML primers to the nf-fluab primer file
  - Note: Found that those sequences were already in the primer file, though named differently

---

## June 22–26, 2026

- Working on OSPC gene analysis
- Got miniconda backup (June 10th, 2026)
- Pandas issues resolved—everything appears to be fine

---

## TODO List

- [ ] pcrValidator
- [ ] measeq testing
- [ ] nf-fluab revision needed
- [ ] Create database updating procedures for regular database updates
- [ ] OSPC data analysis revisit
- [ ] Prob capture
