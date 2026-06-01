# Work Log - May 2026

## Monday, May 4, 2026
### varify influenza coinfection problems: 

As discussed on Monday, here is the flu run I was talking about. Sample 35 is a FluB pos, sample 44 is H1N1 and sample 50 is the H3N2 pos control

- fluB: 260410_S_I_019-S35, 260410_S_I_019-S35_hit2
  ```
  (base) [xiaolidong@uxaplgnmxapp06 260410_S_I_019-S35]$ grep "|A" mapping/260410_S_I_019-S35.screen 
    0.976046        601/1000        2       0       PV343094        Human|4|HA|H3N2|na|A/Colorado/ISC-1136/2024|na|na|na len=1737|1701
    0.958654        412/1000        7       0       PP664072        Environment|1|PB2|H3N2|United_Kingdom|A/environment/Northern_Ireland/4/2022|na|na|na len=2341|2280
    0.988838        790/1000        2       0       OQ842009        Human|6|NA|H3N2|USA|A/USA/PV63214/2022|na|na|na len=1469|1413
    0.988719        788/1000        3       0       OR201735        Human|5|NP|H3N2|USA|A/New_Jersey/29/2023|na|na|na len=1541|1497
    0.987521        739/962 15      0       OP547305        Human|7|M|H3N2|France|A/Marseille/IHU-000624/2022|na|na|na len=1027|982
    0.989013        651/821 12      0       MG279234        Human|8|NS|H3N2|Singapore|A/Singapore/NHRC_RGN0210/2017|na|na|na len=841|841
    (base) [xiaolidong@uxaplgnmxapp06 260410_S_I_019-S35]$ 
    (base) [xiaolidong@uxaplgnmxapp06 260410_S_I_019-S35]$ grep "|B" mapping/260410_S_I_019-S35.screen 
    0.990709        822/1000        1943    0       PV349045        Human|3|PA|Victoria|na|B/Colorado/01/2025|na|na|na len=2275|2181
    0.993336        869/1000        2892    0       PV349071        Human|5|NP|Victoria|na|B/Connecticut/01/2025|na|na|na len=1815|1683
    0.999377        987/1000        4253    0       PV349170        Human|8|NS|Victoria|na|B/Idaho/03/2025|na|na|na len=1069|1027
    0.995693        664/727 722     0       PV349601        Human|7|M|Victoria|na|B/New_Hampshire/04/2025|na|na|na len=1154|747
    0.994146        884/1000        4594    0       PV349654        Human|4|HA|Victoria|na|B/Oregon/01/2025|na|na|na len=1847|1749
    0.997209        943/1000        4295    0       PQ078353        Human|6|NA|Victoria|na|B/Arizona/18/2024|na|na|na len=1532|1404
  ```
- fluA: 
 
- 260410_S_I_019-S44: two hits,

####
python otu_table_filter.py otu_table.csv noglobal_nolocalchim.consensus.sequences.fas chimeric_consensus.sequences.fas localchim_consensus.sequences.fas 
[INFO] Good consensus: 29
[INFO] Global chimera: 6
[INFO] Local chimera: 21
[INFO] DONE
[INFO] otu_table.filtered.csv
[INFO] removed_global_chimera.csv
[INFO] removed_local_chimera.csv
[INFO] removed_not_in_consensus.csv
  - this looks like true coinfections of H3N2 and H1N1

260410_S_I_019-S45
260410_S_I_019-S46
260410_S_I_019-S47
260410_S_I_019-S48
260410_S_I_019-S49
260410_S_I_019-S50

- looks like coinfrection: 260410_S_I_019-S50_hit2, 260410_S_I_019-S51

### May 6, 2026, Wednesday
SNAPPI meeting:
- Troubleshooting the assembly canadian helicobactor genome
  - gram-negative, small genome 1.67 mbp
  - infect gastric epithelium
  - Q15, lenght > 2kb help to get better assembly
  - quast can be used to map reads back to genome

### May 8, 2026, Friday
- timesheet and scf done
- updated nf-qcflow for bug fix, tool version change, database upgrade, cleaned nextflow.config file
- update nf-taxflow for bug fix, tool version change, database upgrade, cleaned nextflow.config file
- updated readme files and added workflow diagram

### May 11, 2026, Monday
- TODO LIST
  - working on cap audit preparation 
  - mpxv data analysis
  - pathogenseq
  - flu pipeline database update?
  - OSPC analysis continue
- ospc analysis
  - want to download all the borrelia dna sequences. it should include all the borrelia speices which could cause lyme disease
  - the term used will be "Borrelia burgdorferi sensu lato", In scientific literature, "sensu lato" means "in the broad sense". It is used to refer to a species complex that includes all the individual species (or "genospecies") capable of causing Lyme disease.
  - https://www.ncbi.nlm.nih.gov/datasets/taxonomy/64895/
### May 12, 2026
- meet vince to prepare for cap audit
  - focus on documentation: sops, problems trouble shooting, software maintaiance, questions about trainning, record how qulify for to run pipeline, training record, auditor detail question: pathogenseq, component, qc matrix, different tools for differet data type,
  - run program and log files, walk her through a typical analysis, to see, what happened downstreams, step by step after get the data, output, qc matrix, anything does not meeting, qc threashold, want to understand the whole process,
  - where the data stored, phyical locations, backup schedulres about the data,
  - what we do after we update pipeline, what happened, sent out communication, if the linage updated, do we reanalyze our data. all about clinc, only focus on the results go out,
  - ebs database a little bit, what get stored,  log, ashwind maintain a big log files ( a little bit),  how do you know when you need to answer tools or databases.

### May 14, 2026
- ospc analysis
  - setup compara analysis at 100% identity
  - clean the barbour reference sequeces
    - t_coffee alignmet, then only keep regions between forward primer and nested primers not including primers
    -   
- cap aduit
  - create bioinformatic training tracker
  -  
- CPHLN meeting:
  -RUiming presentation
    - flu mutation watch list, machine leaning, measure distance
    - IRVC influenza database
      <img width="1211" height="490" alt="image" src="https://github.com/user-attachments/assets/561b5180-a667-492d-84c2-1b6b9bc4adc2" />

### May 19, 2026, Sum
#### pipeline launcher competency:

For Illumina sequencing, the analysis pipeline starts automatically shortly after the run is set up on the sequencer and the Pipeline Launcher is initiated.For Nanopore sequencing, the workflow is different. After the sequencing run is completed, the output data must first be manually transferred from the sequencer to APLGenomics. The Pipeline Launcher is then started to split the data and begin the downstream analysis.

### May 21
MPXV
For Clade II, the reference used is NC_063383 and for Clade I, we use NC_003310. This means that all coordinates within an alignment will be relative to these references. A benefit of this is that within a clade, alignment files and be combined without having to recalculate the alignment. Note however that insertions relative to the reference sequence will not be included in the alignment.

### May 29
- mpxv
  -  To date, no antiviral treatment has been definitively demonstrated to be effective against MPXV.
  -  WHO and several national guidelines recommended tecovirimat as the first-line antiviral drug for mpox, However, recent studies reported that, during the mpox outbreak in the United States, severely immunocompromised patients who received multiple courses of tecovirimat exhibited poor outcomes (35.3%; 18 out of 51 patients)
  -  Resistance-associated mutations, such as F13L gene mutation, may lead to treatment failure and human-to-human transmission,
  -  Cidofovir exhibits broad-spectrum activity against DNA viruses, such as MPXV.
  -  
Squirrel by default creates a single alignment fasta file. Using the genbank coordinates for NC_063383 it also has the ability to extract the aligned coding sequences either as separate records or as a concatenated alignment. This can facilitate codon-aware phylogenetic or sequence analysis.
- meeting with tarah
- <img width="1065" height="309" alt="image" src="https://github.com/user-attachments/assets/7c5d9b30-b6f8-4005-934c-352ac678f235" />

