# 3d-plot-kmer

Small Python scripts to prepare k-mer frequency data for plotting: split a multi-FASTA file into one FASTA per sequence, and merge per-sequence k-mer relative-frequency CSVs into a single table (one row per sequence, one column per k-mer).

## Scripts

### `multi2simple.py`

Splits a multi-FASTA file into single-record FASTA files. Each record is written to `<description>.fasta` in the output folder, with spaces in the description replaced by `_`.

```bash
python multi2simple.py -i <inputfile> -o <outputfolder>
```

| Option | Description |
| --- | --- |
| `-i`, `--inputfile` | Multi-FASTA file to split |
| `-o`, `--outputfolder` | Folder for the output FASTA files (default: the script's folder) |
| `-h` | Print usage |

### `indexes2table.py`

Reads every file in an input folder as a CSV with (at least) the columns `kmer` and `rel_freq`, and builds one table:

- the column headers are `FILE` followed by the `kmer` values of the first file in the folder (all files are expected to list the same k-mers in the same order);
- each row holds the file name and that file's `rel_freq` values, rounded to 4 decimals.

```bash
python indexes2table.py -i <inputfolder> -o <outputfolder>
```

| Option | Description |
| --- | --- |
| `-i`, `--inputfolder` | Folder containing only the per-sequence k-mer CSV files |
| `-o`, `--outputfolder` | Folder for the output table (default: the script's folder) |
| `-h` | Print usage |

Outputs, written to the output folder:

- `tabla_freq_rel.csv` (with an `index` column)
- `tabla_freq_rel.xlsx`

## Example output

`out/tabla_freq_rel.csv` / `out/tabla_freq_rel.xlsx` contain an example table with a single row (`HOMSA.Chr.1_5.csv`, human chromosome 1). Its columns are `FILE`, `LENGTH` (sequence length, carried as an extra row of the input CSV) and the relative frequencies of all 1024 5-mers (`AAAAA` ... `TTTTT`).

## Requirements

- Python 3
- `numpy`, `pandas` (below 2.0: the script uses `DataFrame.append`, which pandas 2.0 removed), `openpyxl` (for the `.xlsx` export), `matplotlib`
- `biopython` (for `multi2simple.py`)

## Status

Unmaintained student project (University of Málaga, 2020). The 3D surface plot the repository is named after is not implemented: the plotting code in `indexes2table.py` is left inside an unused string block at the end of the file, and the scripts only produce the table.

## Related

- [kmers-tynsa](https://github.com/UMA-Bioinformatics/kmers-tynsa): Python methods for working with nucleotide sequences using k-mer hashes.
