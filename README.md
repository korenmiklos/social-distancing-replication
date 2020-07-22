# Template README and Guidance

Data Availability Statements
----------------------------

### SafeGraph data 
> The mobility data used in this paper (SafeGraph, 2020) is proprietary, but may be obtained free of charge for Covid-19-related research from the COVID-19 Consortium. Researchers interested in access to the data can apply at https://www.safegraph.com/covid-19-data-consortium. After signing a Data Agreement, access is granted within a few days. The authors will assist with any reasonable replication attempts for two years following publication.
> 
> Datafiles used: `s3://sg-c19-response/monthly-patterns/patterns_backfill/2020/05/07/12/2020/02/patterns-part[1-4].csv.gz` (data for February 2020, released May 7, 2020) and `s3://sg-c19-response/monthly-patterns/patterns/2020/06/05/06/patterns-part[1-4].csv.gz` (data for February 2020, released June 5, 2020).

> The code and all other data underlying our analysis are licensed for public use and are available on Zenodo (XXXX). 

Dataset list
------------

| Data file | Source | Notes    |Provided |
|-----------|--------|----------|---------|
| `data/raw/bls/industry-employment/ces.txt` | BLS Current Employment Statistics | Public domain | Yes |
| `data/raw/bls/atus/*.dat` | BLS Time Use Survey | Public domain | Yes |
| `data/raw/bls/employment-matrix/matrix.xlsx` | BLS National Employment Matrix | Public domain | Yes |
| `data/raw/bls/crosswalk/matrix.xlsx` | ONET-SOC to Occupational Outlook Handbook Crosswalk | Public domain | Yes |
| `data/raw/onet/*.csv` | ONET Online | Creative Commons 4.0 | Yes |
| `data/raw/census/cbp/*.txt` | County Business Patterns | Public domain | Yes |
| `not-included/safegraph/*.csv`| SafeGraph | Available with Data Agreement with SafeGraph | No |
| `data/derived/visit/visit-change.dta`| SafeGraph | Aggregated to 3-digit NAICS industries | Yes |


Computational requirements
---------------------------

### Software Requirements
- Stata (code was last run with version 16)
  - `estout` (from http://www.stata-journal.com/software/sj14-2/)
  - running `make install` from the root of the folder will install `estout` locally, and should be run once.

Portions of the code use bash scripting (`make`, `wget`), which may require Linux or Mac OS X.

### Description of programs
> INSTRUCTIONS: Give a high-level overview of the program files and their purpose. Remove redundant/ obsolete files from the Replication archive.

- Programs in `programs/01_dataprep` will extract and reformat all datasets referenced above. The file `programs/01_dataprep/master.do` will run them all.
- Programs in `programs/02_analysis` generate all tables and figures in the main body of the article. The program `programs/02_analysis/master.do` will run them all. Each program called from `master.do` identifies the table or figure it creates (e.g., `05_table5.do`).  Output files are called appropriate names (`table5.tex`, `figure12.png`) and should be easy to correlate with the manuscript.
- Programs in `programs/03_appendix` will generate all tables and figures  in the online appendix. The program `programs/03_appendix/master-appendix.do` will run them all. 
- Ado files have been stored in `programs/ado` and the `master.do` files set the ADO directories appropriately. 
- The program `programs/00_setup.do` will populate the `programs/ado` directory with updated ado packages, but for purposes of exact reproduction, this is not needed. The file `programs/00_setup.log` identifies the versions as they were last updated.
- The program `programs/config.do` contains parameters used by all programs, including a random seed. Note that the random seed is set once for each of the two sequences (in `02_analysis` and `03_appendix`). If running in any order other than the one outlined below, your results may differ.


### Memory and Runtime Requirements
> INSTRUCTIONS: Memory and compute-time requirements may also be relevant or even critical. Some example text follows.

The code was last run on a **4-core Intel-based laptop with MacOS version 10.14.4**. 

Portions of the code were last run on a **32-core Intel server with 1024 GB of RAM, 12 TB of fast local storage**. Computation took 734 hours. 

Portions of the code were last run on a **12-node AWS R3 cluster, consuming 20,000 core-hours**.  

Instructions
------------
> INSTRUCTIONS: The first two sections ensure that the data and software necessary to conduct the replication have been collected. This section then describes a human-readable instruction to conduct the replication. This may be simple, or may involve many complicated steps. It should be a simple list, no excess prose. Strict linear sequence. If more than 4-5 manual steps, please wrap a master program/Makefile around them, in logical sequences. Examples follow.

- Edit `programs/config.do` to adjust the default path
- Run `programs/00_setup.do` once on a new system to set up the working environment. 
- Download the data files referenced above. Each should be stored in the prepared subdirectories of `data/`, in the format that you download them in. Do not unzip. Scripts are provided in each directory to download the public-use files. Confidential data files requested as part of your FSRDC project will appear in the `/data` folder. No further action is needed on the replicator's part.
- Run `programs/01_master.do` to run all steps in sequence.

### Details

- `programs/00_setup.do`: will create all output directories, install needed ado packages. 
   - If wishing to update the ado packages used by this archive, change the parameter `update_ado` to `yes`. However, this is not needed to successfully reproduce the manuscript tables. 
- `programs/01_dataprep`:  
   - These programs were last run at various times in 2018. 
   - Order does not matter, all programs can be run in parallel, if needed. 
   - A `programs/01_dataprep/master.do` will run them all in sequence, which should take about 2 hours.
- `programs/02_analysis/master.do`.
   - If running programs individually, note that ORDER IS IMPORTANT. 
   - The programs were last run top to bottom on July 4, 2019.
- `programs/03_appendix/master-appendix.do`. The programs were last run top to bottom on July 4, 2019.

List of tables and programs
---------------------------

> INSTRUCTIONS: Your programs should clearly identify the tables and figures as they appear in the manuscript, by number. Sometimes, this may be obvious, e.g. a program called "`table1.do`" generates a file called `table1.png`. Sometimes, mnemonics are used, and a mapping is necessary. In all circumstances, provide a list of tables and figures, identifying the program (and possibly the line number) where a figure is created.

| Figure/Table #    | Program                  | Line Number | Output file                      | Note                            |
|-------------------|--------------------------|-------------|----------------------------------|---------------------------------|
| Table 1           | 02_analysis/table1.do    |             | summarystats.csv                 ||
| Table 2           | 02_analysis/table2and3.do| 15          | table2.csv                       ||
| Table 3           | 02_analysis/table2and3.do| 145         | table3.csv                       ||
| Figure 1          | n.a. (no data)           |             |                                  | Source: Herodus (2011)          |
| Figure 2          | 02_analysis/fig2.do      |             | figure2.png                      ||
| Figure 3          | 02_analysis/fig3.do      |             | figure-robustness.png            | Requires confidential data      |

## References

> INSTRUCTIONS: As in any scientific manuscript, you should have proper references. For instance, in this sample README, we cited "Ruggles et al, 2019" and "DESE, 2019" in a Data Availability Statement. The reference should thus be listed here, in the style of your journal:

Steven Ruggles, Steven M. Manson, Tracy A. Kugler, David A. Haynes II, David C. Van Riper, and Maryia Bakhtsiyarava. 2018. "IPUMS Terra: Integrated Data on Population and Environment: Version 2 [dataset]." Minneapolis, MN: *Minnesota Population Center, IPUMS*. https://doi.org/10.18128/D090.V2

Department of Elementary and Secondary Education (DESE), 2019. "Student outcomes database [dataset]" *Massachusetts Department of Elementary and Secondary Education (DESE)*. Accessed January 15, 2019.

<hr>

## Acknowledgements

Some content on this page was copied from [Hindawi](https://www.hindawi.com/research.data/#statement.templates). Other content was adapted  from [Fort (2016)](https://doi.org/10.1093/restud/rdw057), Supplementary data, with the author's permission.
