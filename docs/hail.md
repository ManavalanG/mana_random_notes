# Hail

## Get value counts for a column

```py
ht.aggregate(hl.agg.counter(ht.variant_type))
```

## Filter by column value

```py
filt_ht = ht.filter((ht.is_canonical_transcript=="true") & (ht.gene_symbol=="BRCA2"))
```

## Write matrixtable rows as pandas compatible tsv file

```py
mt.rows().flatten().export("out.tsv")
```
