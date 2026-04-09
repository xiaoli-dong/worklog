# Work Log - April 2026


## April 2, 2026

- mpox-analyzer developing
- meeting tarah to discuss mpox analysis for jamil's paper
- proble capture meeting with Ashlyn kirk and andrew cameon
  - expired reagents can be used
  - kits used in plsa: nextera, Illumina DNA Prep
  - regina using 300x sequencing , reagents: hybridization, universal blocking
  - what type of the samples: 8 samples in one pool capture, trying 12x
  - vsp panel from illumina , not working well for complex samples
  - sensentivity is viral dependent,
  - illumina has a nice ribosome kitto depelete robsom rRNA, upstream to get rid of is not working well, universal also binding to human ribosome and bind to adapter
- 
## Monday, April 6, 2026
- Group meeting: cap, quality 
- work on mpox

### Tuesday, April 7, 2026, Sunny
- mpxv-analyzer testing
- mpxv-analyzer SOP draft

### Wednesday, April 8, 2026
- sick day
### Thursday, April 9, 2027
- clean up home directories across all the nodes on the server and redirected some of the dot files to path/project/xdong
- WGU readship
  - tuition discount: full time employee, membership
  - apply for scholarships
  - accept enrollment by April 15 and finiance deadline April 22
- rsv pipeline failed because the pipeline failed to get includeConfig 'https://raw.githubusercontent.com/nf-core/configs/master/nfcore_custom.config', the ahs is blocking it. I added the following optons to the sbatch script to run the nextflow pipeline offline:
  ```
    # ======================================
    # Environment setup
    # ======================================
    # ahs is blocking download nfcore_custom.config at the run time
    # https://raw.githubusercontent.com/nf-core/configs/master/nfcore_custom.config
    #so run the pipeline in offline mode
    export NXF_OFFLINE='true'
  ```
- rsv pipeline bug fixing:
  - when making the summary report, the nextclade path for rsvB was wrong, put in "nextclade/nextclade/nextcalde.tsv" instead of "nextclade/nextclade.tsv".
  - updating the results for testing dataset under "testdata" dir
