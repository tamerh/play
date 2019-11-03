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

############ perform search and mapping queries

'''
Searching identfiers and keywords such as gene name or accessions by passing comma seperated terms.
'''
bb.search('tpi1,vav_human,ENST00000297261')

#search only within dataset
bb.search("tpi1,ENSG00000164690","ensembl")

'''
Mappings  queries are in following format which allow chains mapping among datasets

map(dataset_id).filter(Boolean expression).map(...).filter(...) 
'''
# map proteins to go terms
bb.mapping('at5g3_human,vav_human','map(go)',attrs = "type")

# map proteins to go terms types with filter
bb.mapping('at5g3_human,vav_human','map(go).filter(go.type=="biological_process")',attrs = "type")

# stop local server. server can be start again with existing built data
bb.stop()

```

## Build data
Building data process selected datasets and retrieve indivudual records belonging to these datasets with their attributes and mapping data to the other datasets. Before building data first let's list the datasets and genomes, 

### List datasets and genomes


```python
# view source and target datasets
res=bb.datasetsView


#display as table
import tabulate
from IPython.display import HTML, display

#display first 50
display(HTML(tabulate.tabulate(res[:50], tablefmt='html')))
```


<table>
<tbody>
<tr><td>id                     </td><td>numeric id</td><td>type           </td></tr>
<tr><td>uniprot                </td><td>1         </td><td>source & target</td></tr>
<tr><td>ensembl                </td><td>2         </td><td>source & target</td></tr>
<tr><td>taxonomy               </td><td>3         </td><td>source & target</td></tr>
<tr><td>go                     </td><td>4         </td><td>source & target</td></tr>
<tr><td>hgnc                   </td><td>5         </td><td>source & target</td></tr>
<tr><td>uniparc                </td><td>6         </td><td>source & target</td></tr>
<tr><td>uniref50               </td><td>7         </td><td>source & target</td></tr>
<tr><td>uniref90               </td><td>8         </td><td>source & target</td></tr>
<tr><td>uniref100              </td><td>9         </td><td>source & target</td></tr>
<tr><td>chebi                  </td><td>10        </td><td>source & target</td></tr>
<tr><td>interpro               </td><td>11        </td><td>source & target</td></tr>
<tr><td>literature_mappings    </td><td>12        </td><td>source & target</td></tr>
<tr><td>hmdb                   </td><td>13        </td><td>source & target</td></tr>
<tr><td>my_data                </td><td>14        </td><td>source & target</td></tr>
<tr><td>chembl_document        </td><td>15        </td><td>source & target</td></tr>
<tr><td>chembl_assay           </td><td>16        </td><td>source & target</td></tr>
<tr><td>chembl_activity        </td><td>17        </td><td>source & target</td></tr>
<tr><td>chembl_molecule        </td><td>18        </td><td>source & target</td></tr>
<tr><td>chembl_target_component</td><td>19        </td><td>source & target</td></tr>
<tr><td>chembl_target          </td><td>20        </td><td>source & target</td></tr>
<tr><td>chembl_cell_line       </td><td>21        </td><td>source & target</td></tr>
<tr><td>efo                    </td><td>22        </td><td>source & target</td></tr>
<tr><td>eco                    </td><td>23        </td><td>source & target</td></tr>
<tr><td>cath                   </td><td>30        </td><td>target         </td></tr>
<tr><td>cathgene3d             </td><td>31        </td><td>target         </td></tr>
<tr><td>ccds                   </td><td>32        </td><td>target         </td></tr>
<tr><td>cdd                    </td><td>33        </td><td>target         </td></tr>
<tr><td>doi                    </td><td>35        </td><td>target         </td></tr>
<tr><td>drugbank               </td><td>36        </td><td>target         </td></tr>
<tr><td>ec                     </td><td>37        </td><td>target         </td></tr>
<tr><td>ena                    </td><td>38        </td><td>target         </td></tr>
<tr><td>exon                   </td><td>39        </td><td>target         </td></tr>
<tr><td>ortholog               </td><td>40        </td><td>target         </td></tr>
<tr><td>paralog                </td><td>41        </td><td>target         </td></tr>
<tr><td>transcript             </td><td>42        </td><td>target         </td></tr>
<tr><td>expressionatlas        </td><td>43        </td><td>target         </td></tr>
<tr><td>flybase                </td><td>44        </td><td>target         </td></tr>
<tr><td>entrez                 </td><td>45        </td><td>target         </td></tr>
<tr><td>hamap                  </td><td>46        </td><td>target         </td></tr>
<tr><td>intact                 </td><td>47        </td><td>target         </td></tr>
<tr><td>kegg                   </td><td>48        </td><td>target         </td></tr>
<tr><td>kegg map               </td><td>49        </td><td>target         </td></tr>
<tr><td>ko                     </td><td>50        </td><td>target         </td></tr>
<tr><td>mim                    </td><td>51        </td><td>target         </td></tr>
<tr><td>metacyc                </td><td>52        </td><td>target         </td></tr>
<tr><td>genbank                </td><td>53        </td><td>target         </td></tr>
<tr><td>opentargets            </td><td>54        </td><td>target         </td></tr>
<tr><td>orphanet               </td><td>55        </td><td>target         </td></tr>
<tr><td>panther                </td><td>56        </td><td>target         </td></tr>
</tbody>
</table>



```python
res=bb.listGenomes("ensembl")

#displays first 10
display(HTML(tabulate.tabulate(res[:10], tablefmt='html',headers=["genomes"])))
```


<table>
<thead>
<tr><th>genomes                         </th></tr>
</thead>
<tbody>
<tr><td>acanthochromis_polyacanthus     </td></tr>
<tr><td>ailuropoda_melanoleuca          </td></tr>
<tr><td>amphilophus_citrinellus         </td></tr>
<tr><td>amphiprion_ocellaris            </td></tr>
<tr><td>amphiprion_percula              </td></tr>
<tr><td>anabas_testudineus              </td></tr>
<tr><td>anas_platyrhynchos_platyrhynchos</td></tr>
<tr><td>anolis_carolinensis             </td></tr>
<tr><td>anser_brachyrhynchus            </td></tr>
<tr><td>aotus_nancymaae                 </td></tr>
</tbody>
</table>



```python
res=bb.listGenomes("ensembl_bacteria")

#displays first 10
display(HTML(tabulate.tabulate(res[:10], tablefmt='html',headers=["genomes"])))
```


<table>
<thead>
<tr><th>genomes                                            </th></tr>
</thead>
<tbody>
<tr><td>_bacillus_aminovorans                              </td></tr>
<tr><td>_bacillus_aminovorans_gca_001643235                </td></tr>
<tr><td>_bacillus_selenitireducens_mls10                   </td></tr>
<tr><td>_bacillus_thuringiensis_serovar_konkukian_str_97_27</td></tr>
<tr><td>_bacteroides_pectinophilus_atcc_43243              </td></tr>
<tr><td>_brevibacterium_flavum                             </td></tr>
<tr><td>_brevibacterium_halotolerans                       </td></tr>
<tr><td>_butyribacterium_methylotrophicum                  </td></tr>
<tr><td>_chrysanthemum_coronarium_phytoplasma              </td></tr>
<tr><td>_clostridium_acidurici_9a                          </td></tr>
</tbody>
</table>



```python
res=bb.listGenomes("ensembl_fungi")

#displays first 10
display(HTML(tabulate.tabulate(res[:10], tablefmt='html',headers=["genomes"])))
```


<table>
<thead>
<tr><th>genomes                                              </th></tr>
</thead>
<tbody>
<tr><td>_candida_arabinofermentans_nrrl_yb_2248_gca_001661425</td></tr>
<tr><td>_candida_auris_gca_001189475                         </td></tr>
<tr><td>_candida_auris_gca_002759435                         </td></tr>
<tr><td>_candida_auris_gca_002775015                         </td></tr>
<tr><td>_candida_auris_gca_003013715                         </td></tr>
<tr><td>_candida_auris_gca_003014415                         </td></tr>
<tr><td>_candida_duobushaemulonis_gca_002926085              </td></tr>
<tr><td>_candida_glabrata_gca_000002545                      </td></tr>
<tr><td>_candida_glabrata_gca_001466525                      </td></tr>
<tr><td>_candida_glabrata_gca_001466535                      </td></tr>
</tbody>
</table>



```python
res=bb.listGenomes("ensembl_metazoa")

#displays first 10
display(HTML(tabulate.tabulate(res[:10], tablefmt='html',headers=["genomes"])))
```


<table>
<thead>
<tr><th>genomes                 </th></tr>
</thead>
<tbody>
<tr><td>acyrthosiphon_pisum     </td></tr>
<tr><td>adineta_vaga            </td></tr>
<tr><td>aedes_aegypti           </td></tr>
<tr><td>amphimedon_queenslandica</td></tr>
<tr><td>anopheles_darlingi      </td></tr>
<tr><td>anopheles_gambiae       </td></tr>
<tr><td>anoplophora_glabripennis</td></tr>
<tr><td>apis_mellifera          </td></tr>
<tr><td>atta_cephalotes         </td></tr>
<tr><td>belgica_antarctica      </td></tr>
</tbody>
</table>



```python
res=bb.listGenomes("ensembl_plants")

#displays first 10
display(HTML(tabulate.tabulate(res[:10], tablefmt='html',headers=["genomes"])))
```


<table>
<thead>
<tr><th>genomes                </th></tr>
</thead>
<tbody>
<tr><td>actinidia_chinensis    </td></tr>
<tr><td>aegilops_tauschii      </td></tr>
<tr><td>amborella_trichopoda   </td></tr>
<tr><td>arabidopsis_halleri    </td></tr>
<tr><td>arabidopsis_lyrata     </td></tr>
<tr><td>arabidopsis_thaliana   </td></tr>
<tr><td>beta_vulgaris          </td></tr>
<tr><td>brachypodium_distachyon</td></tr>
<tr><td>brassica_napus         </td></tr>
<tr><td>brassica_oleracea      </td></tr>
</tbody>
</table>



```python
res=bb.listGenomes("ensembl_protists")

#displays first 10
display(HTML(tabulate.tabulate(res[:10], tablefmt='html',headers=["genomes"])))
```


<table>
<thead>
<tr><th>genomes                                        </th></tr>
</thead>
<tbody>
<tr><td>acanthamoeba_castellanii_str_neff_gca_000313135</td></tr>
<tr><td>achlya_hypogyna_gca_002081595                  </td></tr>
<tr><td>albugo_laibachii                               </td></tr>
<tr><td>angomonas_deanei_gca_000442575                 </td></tr>
<tr><td>aphanomyces_astaci_gca_000520075               </td></tr>
<tr><td>aphanomyces_astaci_gca_002197585               </td></tr>
<tr><td>aphanomyces_astaci_gca_003546545               </td></tr>
<tr><td>aphanomyces_astaci_gca_003546565               </td></tr>
<tr><td>aphanomyces_astaci_gca_003546585               </td></tr>
<tr><td>aphanomyces_astaci_gca_003546605               </td></tr>
</tbody>
</table>


### Build data examples 


```python
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

## Run local server
Running local server allows performing queries and provide web interface for data explorations. Server can start and stop with existing built data.


```python
import bbpy as b

# build data with default dataset and start server
bb=b.bbpy('bb')
bb.stop()
bb.start()
```

    biobtreePy is starting





    'biobtreePy started'



## Example use cases


```python
import bbpy as b

# build data with default dataset and start server
bb=b.bbpy('outFolder')
#lbb.buildData()
bb.start()
```

### Gene centric use cases
Ensembl, Ensembl Genomes and HGNC datasets are used for gene related data. One of the most common gene related dataset identfiers are `ensembl`,`hgnc`,`transcript`,`exon`. Let's start with listing their attiributes,


```python
bb.listAttrs('ensembl')
```


```python
bb.listAttrs('hgnc')
```


```python
bb.listAttrs('transcript')
```


```python
bb.listAttrs('exon')
```

Note that as shown with `bb.datasetsView` there are several other gene related datasets without attributes and can be used in mapping queries such as probesets, genebank and entrez etc.


```python
# Map gene names to Ensembl transcript identifier
bb.mapping('ATP5MC3,TP53','map(transcript)')
```


```python
# Map gene names to exon identifiers and retrieve the region
bb.mapping('ATP5MC3,TP53','map(transcript).map(exon)',attrs = "seq_region_name")
```


```python
# Map gene to its ortholog identifiers
bb.mapping('shh','map(ortholog)')
```


```python
# Map gene to its paralog
bb.mapping('fry','map(paralog)')
```


```python
# Map ensembl identifier or gene name to the entrez identifier
bb.mapping('ENSG00000073910,shh' ,'map(entrez)')
```


```python
# Map refseq identifiers to hgnc identifiers
bb.mapping('NM_005359,NM_000546','map(hgnc)',attrs = 'symbols')
```


```python
# Get all Ensembl human identifiers and gene names on chromosome Y with lncRNA type
bb.mapping('homo_sapiens','map(ensembl).filter(ensembl.seq_region_name=="Y" && ensembl.biotype=="lncRNA")',attrs = 'name')
```


```python
''' 
Get all Ensembl human identifiers and gene names within or overlapping range [114129278-114129328]
In this example as a first parameter taxonomy identifier is used instead of specifying as homo sapiens like in the previous example. 
Both of these usage are equivalent and produce same output as homo sapiens refer to taxonomy identifer 9606.
'''
bb.mapping('9606','map(ensembl).filter((114129278>ensembl.start && 114129278<ensembl.end) || (114129328>ensembl.start && 114129328<ensembl.end))',attrs = 'name')
```


```python
# Map Affymetrix identifiers to Ensembl identifiers and gene names
res=bb.mapping("202763_at,213596_at,209310_s_at",source ="affy_hg_u133_plus_2" ,'map(transcript).map(ensembl)',attrs = "name")
```

Note that all mappings can be done with opposite way, for instance from gene name to Affymetrix identifiers mapping is performed following way


```python
res=bb.mapping("CASP3,CASP4",'map(transcript).map(affy_hg_u133_plus_2)')
```


```python
# Retrieve all the human gene names which contains TTY
res=bb.mapping("homo sapiens",'map(ensembl).filter(ensembl.name.contains("TTY"))',attrs = "name")
```

### Protein centric use cases
Uniprot is used for protein related dataset such as protein identifiers, accession, sequence, features, variants, and mapping information to other datasets. Let's list some protein related datasets attributes and then execute example queries similary with gene centric examples,


```python
bb.listAttrs('uniprot')
```


```python
bb.listAttrs('ufeature')
```


```python
bb.listAttrs('pdb')
```


```python
bb.listAttrs('interpro')
```


```python
# Map gene names to reviewed uniprot identifiers
bb.mapping("msh6,stk11,bmpr1a,smad4,brca2","map(uniprot)",source ="hgnc")
```


```python
# Filter proteins by sequence mass and retrieve protein sequences
bb.mapping("clock_human,shh_human,p53_human","filter(uniprot.sequence.mass > 45000)" ,attrs = "sequence$mass,sequence$seq")
```


```python
# Helix type feature locations of a protein
bb.mapping("shh_human",'map(ufeature).filter(ufeature.type=="helix")' ,attrs = "location$begin,location$end")
```


```python
# Get all variation identifiers from a gene with given condition
bb.mapping("tp53",'map(uniprot).map(ufeature).filter(ufeature.original=="I" && ufeature.variation=="S").map(variantid)',source = "hgnc")
```

### Chemisty centric use cases
ChEMBL is used as a source for chemical related datasets. Chembl does not processed in the default dataset so first we index it


```python
bb=b.bbpy('bbChem')
#bb.buildData(datasets="chembl,uniprot,hgnc,taxonomy,go,efo,eco")
bb.start()
```

Listing chembl related dataset attiributes


```python
bb.listAttrs('chembl_document')
```


```python
bb.listAttrs('chembl_assay')
```


```python
bb.listAttrs('chembl_activity')
```


```python
bb.listAttrs('chembl_molecule')
```


```python
bb.listAttrs('chembl_target')
```


```python
bb.listAttrs('chembl_target_component')
```


```python
bb.listAttrs('chembl_cell_line')
```

Search chembl identifiers


```python
# search molecules by names,smile and inchi key
bb.search('GSK2606414,Cn1cc(c2ccc3N(CCc3c2)C(=O)Cc4cccc(c4)C(F)(F)F)c5c(N)ncnc15,SIXVRXARNAVBTC-UHFFFAOYSA-N')
```
