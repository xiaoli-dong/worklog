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
  
  primers
  ```
  The PathAmp primers:
    >pathamp_Reverse_Transcription_Primer
    CTGGATACGCCAGCRAAAGCAGG
    >pathamp_cDNA_amplification_primer
    GACCTGATGCGGAGTAGAAACAAGG
    
  IRVC primers:
    >Opti-F1
    ACGCGTGATCAGCAAAAGCAGG
    >Opti-F2
    ACGCGTGATCAGCGAAAGCAGG
    >MBTuni-13-R
    ACGCGTGATCAGTAGAAACAAGG
  ```

  Alberta strains

  ```
  A/Alberta/01/2020 H1N2v : GISAID EPI_ISL_683998
  A/Alberta/001/2014 H5N1 : GISAID EPI_ISL_154130
  A/Anhui/1/2013 H7N9 : GISAID EPI_ISL_17264823
  A/Canada/504/2004 : GISAID EPI_ISL_5873
  ```
  
  | Run | Sample ID | Sample Name | Subtype |
  |---|---|---|---|
  | fluA_PathAmp_20260622 | 260624_S_I_032_11_S11 | A/Alberta/001/2014 10^-1 | H5N1 |
  | fluA_PathAmp_20260622 | 260624_S_I_032_12_S12 | A/Alberta/01/2020 10^-3 | H1N2v |
  | fluA_PathAmp_20260622 | 260624_S_I_032_13_S13 | A/Anhui/1/13 10^-1 | H7N9 |
  | fluA_PathAmp_20260622 | 260624_S_I_032_14_S14 | A/Canada/504/04 10^-2 | H7N3 |
  | fluA_PathAmp_20260622 | 260624_S_I_032_15_S15 | Extr_FluA_H3N2_Pos_MM | H3N2 |
  | fluA_PathAmp_20260622 | 260624_S_I_032_16_S16 | Extr_UTM_Neg_Ctrl_MM | neg |
  | fluA_PathAmp_20260622 | 260624_S_I_032_17_S17 | NTC_Neg_Ctrl | neg |
  | fluA_IRVC_NML_20260622 | 260624_S_I_032_18_S18 | A/Alberta/001/2014 10^-1 | H5N1 |
  | fluA_IRVC_NML_20260622 | 260624_S_I_032_19_S19 | A/Alberta/01/2020 10^-3 | H1N2v |
  | fluA_IRVC_NML_20260622 | 260624_S_I_032_20_S20 | A/Anhui/1/13 10^-1 | H7N9 |
  | fluA_IRVC_NML_20260622 | 260624_S_I_032_21_S21 | A/Canada/504/04 10^-2 | H7N3 |
  | fluA_IRVC_NML_20260622 | 260624_S_I_032_22_S22 | Extr_FluA_H3N2_Pos_MM | H3N2 |
  | fluA_IRVC_NML_20260622 | 260624_S_I_032_23_S23 | Extr_UTM_Neg_Ctrl_MM | neg |
  | fluA_IRVC_NML_20260622 | 260624_S_I_032_24_S24 | NTC_Neg_Ctrl | neg |
  
