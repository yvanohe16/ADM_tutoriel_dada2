R Notebook
================

``` r
refdb_folder <- here::here("data", "refdb")
refdb_folder
```

    ## [1] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/refdb"

``` r
if (!dir.exists(refdb_folder)) dir.create(refdb_folder, recursive = TRUE)
```

``` r
file.copy(from = "course-material-main/data/raw", to = "data", recursive = TRUE)
```

    ## [1] TRUE

``` r
# R stop downloading after timeout which is
# 60 seconds by default
getOption("timeout")
```

    ## [1] 60

``` r
# so we change timeout to be 20 minutes
options(timeout = 1200)

# we save in variable the path to the refdb
# in the working space
silva_train_set <- file.path(refdb_folder,
                             "silva_nr99_v138.1_train_set.fa.gz")

silva_species_assignment <- file.path(refdb_folder,
                                      "silva_species_assignment_v138.1.fa.gz")

# then we download the files if they don't already exist

if (!file.exists(silva_train_set)) {
  download.file(
    "https://zenodo.org/record/4587955/files/silva_nr99_v138.1_train_set.fa.gz",
    silva_train_set,
    quiet = TRUE
  )
}

if (!file.exists(silva_species_assignment)) {
  download.file(
    "https://zenodo.org/record/4587955/files/silva_species_assignment_v138.1.fa.gz",
    silva_species_assignment,
    quiet = TRUE
  )
}
```

``` r
devtools::load_all("/Users/yvanohemartinier/ADM_tutoriel_dada2/course-material-main/R")
```

    ## ℹ Loading ANF_metaB

``` r
path_to_fastqs <- here::here("data", "raw")
```

``` r
fnFs <- sort(list.files(path_to_fastqs,
                        pattern = "_R1.fastq.gz",
                        full.names = TRUE))
print(fnFs)
```

    ##  [1] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S11B_R1.fastq.gz"
    ##  [2] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S1B_R1.fastq.gz" 
    ##  [3] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S2B_R1.fastq.gz" 
    ##  [4] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S2S_R1.fastq.gz" 
    ##  [5] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S3B_R1.fastq.gz" 
    ##  [6] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S3S_R1.fastq.gz" 
    ##  [7] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S4B_R1.fastq.gz" 
    ##  [8] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S4S_R1.fastq.gz" 
    ##  [9] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S5B_R1.fastq.gz" 
    ## [10] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S5S_R1.fastq.gz" 
    ## [11] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S6B_R1.fastq.gz" 
    ## [12] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S6S_R1.fastq.gz" 
    ## [13] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S7B_R1.fastq.gz" 
    ## [14] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S7S_R1.fastq.gz" 
    ## [15] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S8B_R1.fastq.gz" 
    ## [16] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S8S_R1.fastq.gz" 
    ## [17] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S9B_R1.fastq.gz" 
    ## [18] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S9S_R1.fastq.gz"

``` r
fnRs <- sort(list.files(path_to_fastqs,
                        pattern = "_R2.fastq.gz",
                        full.names = TRUE))
print(fnRs)
```

    ##  [1] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S11B_R2.fastq.gz"
    ##  [2] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S1B_R2.fastq.gz" 
    ##  [3] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S2B_R2.fastq.gz" 
    ##  [4] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S2S_R2.fastq.gz" 
    ##  [5] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S3B_R2.fastq.gz" 
    ##  [6] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S3S_R2.fastq.gz" 
    ##  [7] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S4B_R2.fastq.gz" 
    ##  [8] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S4S_R2.fastq.gz" 
    ##  [9] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S5B_R2.fastq.gz" 
    ## [10] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S5S_R2.fastq.gz" 
    ## [11] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S6B_R2.fastq.gz" 
    ## [12] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S6S_R2.fastq.gz" 
    ## [13] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S7B_R2.fastq.gz" 
    ## [14] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S7S_R2.fastq.gz" 
    ## [15] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S8B_R2.fastq.gz" 
    ## [16] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S8S_R2.fastq.gz" 
    ## [17] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S9B_R2.fastq.gz" 
    ## [18] "/Users/yvanohemartinier/ADM_tutoriel_dada2/data/raw/S9S_R2.fastq.gz"

``` r
sample_names <- basename(fnFs) |>
  strsplit(split = "_") |>
  sapply(head, 1)
```

``` r
basename(fnFs) |>
  head()
```

    ## [1] "S11B_R1.fastq.gz" "S1B_R1.fastq.gz"  "S2B_R1.fastq.gz"  "S2S_R1.fastq.gz" 
    ## [5] "S3B_R1.fastq.gz"  "S3S_R1.fastq.gz"

``` r
basename(fnFs) |>
  strsplit(split = "_") |>
  head()
```

    ## [[1]]
    ## [1] "S11B"        "R1.fastq.gz"
    ## 
    ## [[2]]
    ## [1] "S1B"         "R1.fastq.gz"
    ## 
    ## [[3]]
    ## [1] "S2B"         "R1.fastq.gz"
    ## 
    ## [[4]]
    ## [1] "S2S"         "R1.fastq.gz"
    ## 
    ## [[5]]
    ## [1] "S3B"         "R1.fastq.gz"
    ## 
    ## [[6]]
    ## [1] "S3S"         "R1.fastq.gz"
