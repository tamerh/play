## biobtreePy
The [biobtreePy](https://github.com/tamerh/biobtreePy) package provides a Python interface to [biobtree](https://github.com/tamerh/biobtree) tool which provides search and chain mappings functionalities for identifiers, accessions and special keywords for genomic research pipelines.

## Installation


```python
pip install bbpy
```

## Quick introduction


```python
import bbpy as b
import os

# first create or used existing folder for the tool files
os.mkdir('outFolder')

# create the package class instance with new or existing folder which data built before.
bb=b.bbpy('outFolder')

# build data locally. Sample data contains only few records.
bb.buildData(datasets='sample_data')

#start local server which allows performing queries and provide web interface
bb.start()


#Searching identfiers and keywords such as gene name or accessions by passing comma seperated terms.
bb.search('tpi1,vav_human,ENST00000297261')

#search only within dataset
bb.search("tpi1,ENSG00000164690","ensembl")


#Mappings queries are in following format which allow chains mapping among datasets
#map(dataset_id).filter(Boolean expression).map(...).filter(...) 

# map proteins to go terms
bb.mapping('at5g3_human,vav_human','map(go)',attrs = "type")

# map proteins to go terms types with filter
bb.mapping('at5g3_human,vav_human','map(go).filter(go.type=="biological_process")',attrs = "type")

# stop local server. server can be start again with existing built data
bb.stop()

```

## Build data
Building data process selected datasets and retrieve indivudual records belonging to these datasets with their attributes and mapping data to the other datasets. Before building data first let's list the datasets and genomes, 


```python
# List datasets and genomes
bb.datasetsView
bb.listGenomes("ensembl")
bb.listGenomes("ensembl_bacteria")
bb.listGenomes("ensembl_fungi")
bb.listGenomes("ensembl_metazoa")
bb.listGenomes("ensembl_plants")
bb.listGenomes("ensembl_protists")
```


```python
# Build data examples 

#specific datasets
bb.buildData(datasets = "hgnc,go,taxonomy")

# build default datasets ensembl(homo_sapiens) uniprot(reviewed) hgnc taxonomy go eco efo interpro chebi
bb.buildData() 

#only specific datasets with specific mappings to speed up the build process
bb.buildData(datasets = "hgnc,uniprot,go,taxonomy",targetDatasets = "hgnc,uniprot,taxonomy,ufeature,go,pdb,taxchild,taxparent") 

# build both mouse and human genomes in ensembl insted of default human
bb.buildData(genome="homo_sapiens,mus_musculus")

# build genomes from ensembl genomes fungi + means in addition to default dataset
bb.buildData(datasets="+ensembl_fungi",genome="saccharomyces_cerevisiae")

# multiple species genomes from ensembl genomes plants and protists
bb.buildData(datasets="+ensembl_plants,ensembl_protists",genome="arabidopsis_thaliana,phytophthora_parasitica")

# multiple bacteria genomes with pattern which means any genomes contains given names seperated by comma
bb.buildData(datasets="+ensembl_bacteria",genomePattern="serovar_infantis,serovar_virchow")
```

## Example use cases


```python
# build data with default dataset and start server
bb=b.bbpy('outFolder')
bb.buildData()
bb.stop()
bb.start()
```

### Gene centric use cases
Ensembl, Ensembl Genomes and HGNC datasets are used for gene related data. One of the most common gene related dataset identfiers are `ensembl`,`hgnc`,`transcript`,`exon`. Note that there are several other gene related datasets without attributes and can be used in mapping queries such as probesets, genebank and entrez etc.


```python
# list gene related dataset attiributes
bb.listAttrs('ensembl')
bb.listAttrs('hgnc')
bb.listAttrs('transcript')
bb.listAttrs('exon')
```


```python
# Map gene names to Ensembl transcript identifier
bb.mapping('ATP5MC3,TP53','map(transcript)')

# Map gene names to exon identifiers and retrieve the region
bb.mapping('ATP5MC3,TP53','map(transcript).map(exon)',attrs = "seq_region_name")

# Map gene to its ortholog identifiers
bb.mapping('shh','map(ortholog)')

# Map gene to its paralog
bb.mapping('fry','map(paralog)')

# Map ensembl identifier or gene name to the entrez identifier
bb.mapping('ENSG00000073910,shh' ,'map(entrez)')

# Map refseq identifiers to hgnc identifiers
bb.mapping('NM_005359,NM_000546','map(hgnc)',attrs = 'symbols')

# Get all Ensembl human identifiers and gene names on chromosome Y with lncRNA type
bb.mapping('homo_sapiens','map(ensembl).filter(ensembl.seq_region_name=="Y" && ensembl.biotype=="lncRNA")',attrs = 'name')

 
# Get all Ensembl human identifiers and gene names within or overlapping range [114129278-114129328]
# In this example as a first parameter taxonomy identifier is used instead of specifying as homo sapiens like in the previous example. 
# Both of these usage are equivalent and produce same output as homo sapiens refer to taxonomy identifer 9606.
bb.mapping('9606','map(ensembl).filter((114129278>ensembl.start && 114129278<ensembl.end) || (114129328>ensembl.start && 114129328<ensembl.end))',attrs = 'name')

# Map Affymetrix identifiers to Ensembl identifiers and gene names
res=bb.mapping("202763_at,213596_at,209310_s_at",source ="affy_hg_u133_plus_2" ,'map(transcript).map(ensembl)',attrs = "name")

# all mappings can be done with opposite way, for instance from gene name to Affymetrix identifiers mapping is performed following way
res=bb.mapping("CASP3,CASP4",'map(transcript).map(affy_hg_u133_plus_2)')

# Retrieve all the human gene names which contains TTY
res=bb.mapping("homo sapiens",'map(ensembl).filter(ensembl.name.contains("TTY"))',attrs = "name")
```

### Protein centric use cases
Uniprot is used for protein related dataset such as protein identifiers, accession, sequence, features, variants, and mapping information to other datasets. Let's list some protein related datasets attributes and then execute example queries similary with gene centric examples,


```python
# list some protein related dataset attiributes
bb.listAttrs('uniprot')
bb.listAttrs('ufeature')
bb.listAttrs('pdb')
bb.listAttrs('interpro')
```


```python
# Map gene names to reviewed uniprot identifiers
bb.mapping("msh6,stk11,bmpr1a,smad4,brca2","map(uniprot)",source ="hgnc")

# Filter proteins by sequence mass and retrieve protein sequences
bb.mapping("clock_human,shh_human,p53_human","filter(uniprot.sequence.mass > 45000)" ,attrs = "sequence$mass,sequence$seq")

# Helix type feature locations of a protein
bb.mapping("shh_human",'map(ufeature).filter(ufeature.type=="helix")' ,attrs = "location$begin,location$end")

# Get all variation identifiers from a gene with given condition
bb.mapping("tp53",'map(uniprot).map(ufeature).filter(ufeature.original=="I" && ufeature.variation=="S").map(variantid)',source = "hgnc")
```

### Chemisty centric use cases
ChEMBL is used as a source for chemical related datasets.


```python
# Chembl is not in the default dataset so first built the data
bb=b.bbpy('bbChem')
bb.buildData(datasets="chembl,uniprot,hgnc,taxonomy,go,efo,eco")
bb.start()
```


```python
#Listing chembl related dataset attiributes
bb.listAttrs('chembl_document')
bb.listAttrs('chembl_assay')
bb.listAttrs('chembl_activity')
bb.listAttrs('chembl_molecule')
bb.listAttrs('chembl_target')
bb.listAttrs('chembl_target_component')
bb.listAttrs('chembl_cell_line')
```


```python
# search document, molecules by names,smile and inchi key
bb.search('GSK2606414,Cn1cc(c2ccc3N(CCc3c2)C(=O)Cc4cccc(c4)C(F)(F)F)c5c(N)ncnc15,SIXVRXARNAVBTC-UHFFFAOYSA-N,CHEMBL3421631')

# molecule activities
bb.mapping('GSK2606414','map(chembl_activity)')
           
# molecule activities filter
bb.mapping('GSK2606414','map(chembl_activity).filter(chembl.activity.value > 10.0)')
           
# molecule targets
bb.mapping('GSK2606414','map(chembl_activity).map(chembl_document).map(chembl_assay).map(chembl_target)')
           
# target single protein to uniprot
bb.mapping('CHEMBL2789','map(chembl_target_component).map(uniprot)'')

# cancer related genes to targets
bb.mapping('PMS2,MLH1,MSH2,MSH6,STK11,BMPR1A,SMAD4,BRCA1,BRCA2,TP53,PTEN,PALB2,TSC1,TSC2,FLCN,MET,CDKN2A,RB1' \
           'map(uniprot).map(chembl_target_component).map(chembl_target)',source ="hgnc")

# cancer related genes to targets with type
bb.mapping('PMS2,MLH1,MSH2,MSH6,STK11,BMPR1A,SMAD4,BRCA1,BRCA2,TP53,PTEN,PALB2,TSC1,TSC2,FLCN,MET,CDKN2A,RB1' \
           'map(uniprot).map(chembl_target_component).map(chembl_target).filter(chembl.target.type=="protein-protein_interaction")',source ="hgnc")
           
# document activities
bb.mapping('CHEMBL1121978','map(chembl_activity)')

# document assay
bb.mapping('CHEMBL3421631','map(chembl_assay)')

# document assay filter
bb.mapping('CHEMBL3421631','map(chembl_assay).filter(chembl.assay.type=="Functional" || chembl.assay.type=="Binding")')
           
# document cell line
bb.mapping('CHEMBL3421631','map(chembl_assay).map(chembl_cell_line)')

```
