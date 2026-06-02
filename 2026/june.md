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
