## Monday, February 2, 2026
- sick day
---

## Tuesday, February 3, 2026
- Meeting at 9 with rubayet from macmaster to discuss 16s rRNA (mohammad hasan)
  - both sanger and ngs has problems
  - what they do:
    - directly sequence sample
    - native barcode
    - v1-v8 regions
  - chanlegges: consistency (flowcell to use, can we reuse flowcell), do they get enough biomass: minimum four barcode to use
  - intepret the data:
    - in one run, if an orgnism stands out in one sample but much less reads in other, then it is positive
    - need knowledge to do the call
    - need review board for NGS
  - nanopore: samples with contamination, with pathogen, sensitivity to need
    
- Filled the sch form for monday's sick day, put the vacation request for feb 17 (phoenix trip) and feb 27-march 4 (mexica trip) 
- add barnap to nf-taxflow and setup matthew's request to classify some of the bacteria
- process brollie data with updated parameters, which were decided in the meeting last friday
  - both side to have primers
  - length cutoff: 500-600, 500-700
---

## Wednesday, Feberary 4, 2026
  - SNAPPI meeting: Mark Unger presentation
    - Began WGS for research in 2015, WGS was added to rutine workflow in 2021, GRDI funded wgs for legionella subtyping 2023
    -  Legionella: transmission from env to human, barely human to human
    -  LD outbreak investigation:
      -  Currently: sequence based typing (SBT) = current gold standard (flaA-pilE-asd-mip-mompS-proA-neuA)
      -  New WGS approaches: cgMST, in silicon SBT for both illumina and nanopore
          <img width="450" alt="image" src="https://github.com/user-attachments/assets/b053a401-74af-463e-9976-5f7ec3caef36" />
    
          <img width="450" alt="image"  src="https://github.com/user-attachments/assets/78b5ddcc-90fd-4002-902e-567b5dcd055c" />
  
      - tools developed: diphtOscan 
    - Rapid barcode is not working, native barcode get single genome assembly
    - clinical metagenomics:
      - sanger 500 bp, cannot handle mixed samples
      
      <img width="450" alt="image"  src="https://github.com/user-attachments/assets/9e63462e-7910-438a-b86e-42e9d18929d3" />
  
      <img width="450" alt="image"  src="https://github.com/user-attachments/assets/cd380db3-d4e9-4f6d-8ff9-01886a552fee" />
  
      <img width="450" alt="image"  src="https://github.com/user-attachments/assets/d0b09495-4866-49a5-9c8c-f3ed99fcc238" />
      
     - MAB114 kit: with rapid chemistry but get full length 16s rRNA
     - targeted: take extracts, pcr with full

- Genomics huddle:
    - 16s rRNA, MAB114 kit
    - waster metagenomics:  kanti, run did not work great, did not pick up covid, 
- Working on ospc data analysis pipeline with modified parameters
## Thursday, February 5, 2026
- Contiune to working on refining ospC gene analysis procedure:
  - [Fitness estimates from experimental infections predict the long-term strain structure of a vector-borne pathogen in the field](https://www.nature.com/articles/s41598-017-01821-1)
    - The ospC gene sequences can be classified into what are called ospC major groups (oMGs). The oMGs have a highly discrete pattern of genetic variation where each oMG is ≥8% different in DNA sequence from all other oMGs
    - Within each oMG, the DNA sequence variation was <2%
    - This finding is important because it shows that the oMG alleles are real biological categories that are relatively robust to errors in sequencing or to changes in the clustering protocol. For example, changing the similarity threshold of our clustering protocol from 93–98% did not affect the number of unique oMGs in our dataset
- modify ospc analysis parameter:
  - both end primers,  minlen=500, maxlen=700, merge_consensus 0.92, reads_consensus=100
  - modify concompra to copy locally identified chimera from temporary dir to results
- Analyzed some tb assembled data for Matt with nf-taxflow
