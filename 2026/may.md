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
