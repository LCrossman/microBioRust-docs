**File types and Parsing behaviour**

**Fasta (.aln) & Clustal**


- we also provide the ability to convert fasta to clustal


Each fasta file contains a dataset that have been aligned using sequence alignment software

Appropriate software includes mafft, muscle, clustal

A fasta sequence alignment may be DNA or protein but all of the entries must be an identical length
If the sequence is shorter it must be padded with a '-' gap character at the specific gap position.


Within align.rs we provide the capability of storing whether the Alignment is DNA or Protein,
alongside the sequence data, rows and column numbers as well as the ids.  The ids are also indexed to a HashMap.

There is the possibility of iterating row-wise or column-wise, as well as deleting columns (by index) or rows (either by index or ID)

Statistics can be collected to provide a frequency map of residues at given column index/indices, check if a column is "gap-heavy"
and purge gappy columns if required.  In addition, the alignment can be written as a Clustal format or Fasta format, and can be
subsetted if required.  The alignment can be written as a subset with start and end ranges provided.

*DNA*

DNA alignments have available statistics to be calculated which are gc_content_at_col and to determine the most_frequent_base at position,
also identifying degenerate sites (is_degenerate_site) and calculating a consensus (consensus_sequence).

*Protein*

Protein alignments can have the shannon entropy calculated column-wise as a list.  Rare residues at a site can be determined
(rare_residues_at_site)

The AlignmentKind is stored as an enum

```
pub enum AlignmentKind {
    DNA(Alignment<DNA>),
    Protein(Alignment<Protein>),
}
```

The Alignment itself is stored as a struct

```
pub struct Alignment<Kind> {
    pub sequence_data: Vec<u8>,
    pub rows: usize,
    pub cols: usize,
    pub ids: Vec<String>,
    pub id_to_index: HashMap<String, usize>,
    pub _marker: std::marker::PhantomData<Kind>,
}
```

to create a new dummy DNA alignment:

```
fn setup_test_dna_msa() -> Alignment<DNA> {
        let seq1 = b"ATGC--ATGCATGCATGC".to_vec(); // 3 rows of 6 columns
        let seq2 = b"ATGC-AATGCTTGCATGC".to_vec();
        let seq3 = b"TTGC-AATCCATGCAAGC".to_vec();
        let data : Vec<u8> = vec![seq1, seq2, seq3].into_iter().flatten().collect();
        let ids = vec!["seq1".to_string(), "seq2".to_string(), "seq3".to_string()];
        Alignment::<DNA>::new(data, 3, ids)
}
```

to check for a gap heavy column:
```
fn gap_heavy_detection() {
   let msa = setup_test_dna_msa();
   let result = msa.is_gap_heavy(5, 0.3);
   println!("result is {}", &result);
}
```

to subset the alignment:
```
fn provide_subsetted_alignment(msa: Alignment<DNA>, rows: Range<usize>, cols: Range<usize>) {
    let msa = setup_test_dna_msa();
    let column_length = msa.cols as usize;
    let starts: usize = 0;
    let starte: usize = 3;
    let stops: usize = 6;
    let subsetted = msa.subset(0..3, 6..column_length);
    let _ = subsetted.display_interleaved(None, None, 60);
    println!("these are the subsetted sequence ids {:?}", &subsetted.ids);
}
```

Please see examples for further detail!