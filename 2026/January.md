
# Day-to-Day Lab Log

## January 5
- Lab meeting: continued testing and validation of the **rsv-analyzer** pipeline.
- Met with **Kanti** to discuss and align on the work plan for the coming period.
- Re-familiarized myself with the RSV pipeline after the holiday.
- Set up **four RSV analysis jobs**.

## January 6
- Reviewed **rsv-analyzer** analysis outputs.
  - Identified missing fields in the summary report.
  - Debugged the pipeline and fixed the issue causing incomplete summaries.
- Walked **Anita** through visualization of **influenza segment BAM files** in **IGV**.
- Noticed an issue in **influenza segment 4** for some positive control samples:
  - High number of ambiguous bases observed in the first ~60 bp.
  - Affected samples include:
    - `251211_S_I_084-S85`
    - `251204_S_I_083-S76`
    - `251204_S_I_083-S79`

## Issues & To-Do
- Investigate the cause of ambiguous bases at the 5′ end of influenza segment 4 in positive controls.
  - Check alignment quality, primer/amplicon design, and reference sequence.
- Follow up with Anita and the team after further investigation.


 
