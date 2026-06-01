
For ease of use you can call directly from *Python*   

🦀🐍  
No need to install Rust 

Install microbiorust-py using pip:

```pip install microbiorust```

example script for converting from GenBank to protein fasta: 

```
import microbiorust as mb
#to use json.loads:
import json

#convert genbank to GFF3 (writes directly to file)
#include DNA sequence in the GFF3 file dna=True, otherwise dna=False only writes the feature table data
mb.gbk_to_gff("filename.gbk", dna=True)

faa_collection = mb.gbk_to_faa("filename.gbk") 
#print the locus tag and protein fasta as valid protein fasta format
for info in faa_collection.values():
    print(f">{info.locus_tag}\n{info.faa}\n")

#export directly to JSON (zero dependencies, handled by Rust)
json_str = faa_collection.to_json()
print(json_str)

#capture the json in a Python dictionary
data = json.loads(json_str)

#write json to a file, NB: JSON format file is larger than gbk, embl or gff3
with open("output.json", "w") as f:
    f.write(json_str)

#to write directly to a file in a different format
records = mb.parse_gbk("filename.gbk")
records.write_faa("output.faa") # writes protein fasta
records.write_fna("output.fna") # writes DNA sequence contig fasta
```

microbiorust-py contains the following features: 

GenBank input: 

```
parse_gbk # parse GenBank format into a Python dictionary-like object (Collection)
gbk_to_faa # converts gbk format to protein fasta as above 
gbk_to_fna # converts gbk format to DNA sequence contig fasta  
gbk_to_ffn # converts gbk format to DNA gene sequence fasta  
gbk_to_gff # converts gbk format to GFF3  
gbk_to_faa_count # counts numbers of protein fasta converted from gbk   
```

EMBL input as above:

```
parse_embl # parse EMBL format into a Python dictionary
embl_to_faa  
embl_to_fna  
embl_to_gff 
```

BLAST input:  

```  
parse_tabular # streaming parse BLAST input as -outfmt 6  
parse_XML # streaming BLAST parser for -outfmt 5 
```
Alignment input (MSA):  

``` 
subset_msa_alignment # subset the alignment by row and column  
get_consensus # get a consensus sequence for your MSA alignment 
```

Calculate sequence metrics:  

``` 
hydrophobicity  
amino_counts # counts of each amino acid in the provided sequence  
amino_percentage # percentage of each amino acid in the provided sequence
```
