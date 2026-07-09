# Day-to-Day Work Log

## Thursday, July 2, 2026 (Sunny)

- Follow up with andrew about the pipeline launcher training material issues and he will work on it today
- Added setup section for [navigation practice of linux tutorial](https://github.com/provlab-bioinfo/training-hub/blob/main/linux_basics/exercises/beginner/01-navigation-exercises.md)


---

## Friday, July 3, 2026 (Rainy)
- work on the bioinfo training and competency check list
- Andrew fixed pipeline launcher and need to be tested

---

## Monday, July 6, (Sunny)
- Test pipeline launcher
- ask for a meeting to training and competency check list

## Tuesday, July 7 (Sunny)

- draft bioinformaitcs training checklist based on the discusstion on monday with Kara and kanti
  - bioinformatics check list including linux, slurm, and pipeline launcher
  - linux and slurm have quizs, which will be locked in the supervisor folder
- influenza data analysis problem investication
  - H5, H3, H7, H1N2v are consistently lower than the others. need to figure out what happened:
- Analysis 

  primers
  ```
  The PathAmp primers:
  >pathamp_Reverse_Transcription_Primer
  CTGGATACGCCAGCRAAAGCAGG
  >pathamp_cDNA_amplification_primer
  GACCTGATGCGGAGTAGAAACAAGG

  Reverse compliments
  >pathamp_Reverse_Transcription_Primer-rc
  CCTGCTTTYGCTGGCGTATCCAG    
  >pathamp_cDNA_amplification_primer-rc
  CCTTGTTTCTACTCCGCATCAGGTC    
 
  IRVC primers:
  >Opti-F1
  ACGCGTGATCAGCAAAAGCAGG
  >Opti-F2
  ACGCGTGATCAGCGAAAGCAGG
  >MBTuni-13-R
  ACGCGTGATCAGTAGAAACAAGG

  Reverse compliments
  >Opti-F1-rc
  CCTGCTTTTGCTGATCACGCGT  
  >Opti-F2-rc
  CCTGCTTTCGCTGATCACGCGT  
  >MBTuni-13-R-rc
  CCTTGTTTCTACTGATCACGCGT  

  ```

  Alberta strains

  ```
  A/Alberta/01/2020 H1N2v : GISAID EPI_ISL_683998
  A/Alberta/001/2014 H5N1 : GISAID EPI_ISL_154130
  A/Anhui/1/2013 H7N9 : GISAID EPI_ISL_17264823
  A/Canada/504/2004 : GISAID EPI_ISL_5873
  ```
  Problematic samples:
   
  | Run | Sample ID | Sample Name | Subtype | seg4 | seg6 |
  |---|---|---|---|---|---|
  | fluA_PathAmp_20260622 | 260624_S_I_032_11_S11 | A/Alberta/001/2014 10^-1 | H5N1 | | |
  | fluA_PathAmp_20260622 | 260624_S_I_032_12_S12 | A/Alberta/01/2020 10^-3 | H1N2v | yes | yes|
  | fluA_PathAmp_20260622 | 260624_S_I_032_13_S13 | A/Anhui/1/13 10^-1 | H7N9 || yes |
  | fluA_PathAmp_20260622 | 260624_S_I_032_14_S14 | A/Canada/504/04 10^-2 | H7N3 || |
  | fluA_PathAmp_20260622 | 260624_S_I_032_15_S15 | Extr_FluA_H3N2_Pos_MM | H3N2 | yes | yes |
  | fluA_PathAmp_20260622 | 260624_S_I_032_16_S16 | Extr_UTM_Neg_Ctrl_MM | neg || |
  | fluA_PathAmp_20260622 | 260624_S_I_032_17_S17 | NTC_Neg_Ctrl | neg |
  | fluA_IRVC_NML_20260622 | 260624_S_I_032_18_S18 | A/Alberta/001/2014 10^-1 | H5N1 ||
  | fluA_IRVC_NML_20260622 | 260624_S_I_032_19_S19 | A/Alberta/01/2020 10^-3 | H1N2v | yes | yes|
  | fluA_IRVC_NML_20260622 | 260624_S_I_032_20_S20 | A/Anhui/1/13 10^-1 | H7N9 ||
  | fluA_IRVC_NML_20260622 | 260624_S_I_032_21_S21 | A/Canada/504/04 10^-2 | H7N3 ||
  | fluA_IRVC_NML_20260622 | 260624_S_I_032_22_S22 | Extr_FluA_H3N2_Pos_MM | H3N2 | yes | yes|
  | fluA_IRVC_NML_20260622 | 260624_S_I_032_23_S23 | Extr_UTM_Neg_Ctrl_MM | neg ||
  | fluA_IRVC_NML_20260622 | 260624_S_I_032_24_S24 | NTC_Neg_Ctrl | neg ||

  Results:

  - Expected amplicon orientation should be RT primer — insert — pathamp_cDNA_amplification_primer-rc. After generating these full-length products, physical or enzymatic fragmentation is applied to break the DNA down into sequenceable lengths. In a perfect library, middle fragments will contain pure viral insert with no primers, while the outer tip fragments will preserve a single primer sequence at one end.
  - However, searching for the full-length RT primer against the raw data yielded 54,074 hits in R1 and 58,200 hits in R2, with a high frequency of individual reads containing multiple internal primers stacked back-to-back. In a normal, non-chimeric library, the intact forward primer can only be situated at the absolute 5' start of a fragment, meaning it should appear exclusively in R1. Seeing full-length forward primers symmetrically distributed at the start of tens of thousands of reads in both R1 and R2 definitively proves the presence of a classic molecular artifact known as Symmetrical Primer-Dimer Concatemers.
  - During library preparation, an excess of unreacted, free-floating RT primers linked together head-to-tail to form long, double-stranded synthetic chains. When the Illumina library prep kit attached the sequencing adapters, it treated these repeating primer complexes exactly like true viral inserts. Because these synthetic molecules harbor Illumina sequencing adapters on both flanks, they bound randomly to the flow cell in both possible orientations: half of the molecules bound facing one way, causing R1 to read the forward RT primer sequence first, while the remaining half bound facing the opposite direction, causing R2 to read the forward RT primer sequence first. This random flipping mathematically accounts for the nearly identical counts (~54k vs ~58k) observed across the paired-end files.
  -  Crucially, because fragmentation occurs at completely random coordinates across the molecule, the cleavage site often falls directly within the 23-base primer binding regions themselves. This random cutting shears the primers into variable lengths, generating thousands of terminal fragments that begin or end with truncated, partial primer remnants rather than intact, full-length sequences.
  -  why this only happen to H5N1 not other type like H1N1:
      -  This selective failure occurs because seasonal human strains like H1N1 perfectly match the PathAmp universal primer sequences, while highly pathogenic avian H5N1 strains often harbor subtle genetic mutations (mismatches) at the primer-binding boundaries.
      -  5N1 samples frequently originate from complex field surveillance environments—such as wild bird fecal droplets, environmental water pools, or veterinary tissues. These samples commonly suffer from environmental degradation, freeze-thaw cycles, or the presence of high-concentration host cellular inhibitors. If the long 2.3 kb RNA segments are fractured, the enzyme drops off prematurely, causing only the short MP and NS segments to succeed.
    -  Universal Influenza A primer sets, including those in the PathAmp kit, were historically engineered and optimized using seasonal human lineages like H1N1 and H3N2. Because the sequence matches perfectly, the molecular efficiency in the wet-lab is completely different.
      
  mapping S11 to alberta strains
  
  | Segment | Length (bp) | Num Reads | Coverage (%) | Mean Depth (×) | What This Means                         |
  | ------- | ----------: | --------: | -----------: | -------------: | --------------------------------------- |
  | MP      |         982 |   177,062 |      100.00% |       24,360.8 | Completely sequenced coding region.     |
  | NS      |         823 |    44,972 |      100.00% |       7,739.32 | Completely sequenced coding region.     |
  | HA      |       1,704 |    13,824 |       39.84% |         862.76 | Only ~680 bp of the flanks are visible. |
  | NP      |       1,497 |    22,193 |       22.11% |       1,901.97 | Only ~330 bp of the flanks are visible. |
  | NA      |       1,350 |    28,606 |       15.77% |       2,476.13 | Only ~213 bp of the flanks are visible. |
  | PB1     |       2,274 |    40,799 |       15.56% |       2,379.72 | Only ~354 bp of the flanks are visible. |
  | PB2     |       2,280 |    15,434 |       10.39% |         852.85 | Only ~237 bp of the flanks are visible. |

  - Justin Jia presentation: IMicroSeq Data protal
    - Brinkman lab , simon fraser university
    -   
