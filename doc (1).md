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

display(HTML(tabulate.tabulate(res, tablefmt='html')))
```


<table>
<tbody>
<tr><td>id                              </td><td>numeric id</td><td>type           </td></tr>
<tr><td>uniprot                         </td><td>1         </td><td>source & target</td></tr>
<tr><td>ensembl                         </td><td>2         </td><td>source & target</td></tr>
<tr><td>taxonomy                        </td><td>3         </td><td>source & target</td></tr>
<tr><td>go                              </td><td>4         </td><td>source & target</td></tr>
<tr><td>hgnc                            </td><td>5         </td><td>source & target</td></tr>
<tr><td>uniparc                         </td><td>6         </td><td>source & target</td></tr>
<tr><td>uniref50                        </td><td>7         </td><td>source & target</td></tr>
<tr><td>uniref90                        </td><td>8         </td><td>source & target</td></tr>
<tr><td>uniref100                       </td><td>9         </td><td>source & target</td></tr>
<tr><td>chebi                           </td><td>10        </td><td>source & target</td></tr>
<tr><td>interpro                        </td><td>11        </td><td>source & target</td></tr>
<tr><td>literature_mappings             </td><td>12        </td><td>source & target</td></tr>
<tr><td>hmdb                            </td><td>13        </td><td>source & target</td></tr>
<tr><td>my_data                         </td><td>14        </td><td>source & target</td></tr>
<tr><td>chembl_document                 </td><td>15        </td><td>source & target</td></tr>
<tr><td>chembl_assay                    </td><td>16        </td><td>source & target</td></tr>
<tr><td>chembl_activity                 </td><td>17        </td><td>source & target</td></tr>
<tr><td>chembl_molecule                 </td><td>18        </td><td>source & target</td></tr>
<tr><td>chembl_target_component         </td><td>19        </td><td>source & target</td></tr>
<tr><td>chembl_target                   </td><td>20        </td><td>source & target</td></tr>
<tr><td>chembl_cell_line                </td><td>21        </td><td>source & target</td></tr>
<tr><td>efo                             </td><td>22        </td><td>source & target</td></tr>
<tr><td>eco                             </td><td>23        </td><td>source & target</td></tr>
<tr><td>cath                            </td><td>30        </td><td>target         </td></tr>
<tr><td>cathgene3d                      </td><td>31        </td><td>target         </td></tr>
<tr><td>ccds                            </td><td>32        </td><td>target         </td></tr>
<tr><td>cdd                             </td><td>33        </td><td>target         </td></tr>
<tr><td>doi                             </td><td>35        </td><td>target         </td></tr>
<tr><td>drugbank                        </td><td>36        </td><td>target         </td></tr>
<tr><td>ec                              </td><td>37        </td><td>target         </td></tr>
<tr><td>ena                             </td><td>38        </td><td>target         </td></tr>
<tr><td>exon                            </td><td>39        </td><td>target         </td></tr>
<tr><td>ortholog                        </td><td>40        </td><td>target         </td></tr>
<tr><td>paralog                         </td><td>41        </td><td>target         </td></tr>
<tr><td>transcript                      </td><td>42        </td><td>target         </td></tr>
<tr><td>expressionatlas                 </td><td>43        </td><td>target         </td></tr>
<tr><td>flybase                         </td><td>44        </td><td>target         </td></tr>
<tr><td>entrez                          </td><td>45        </td><td>target         </td></tr>
<tr><td>hamap                           </td><td>46        </td><td>target         </td></tr>
<tr><td>intact                          </td><td>47        </td><td>target         </td></tr>
<tr><td>kegg                            </td><td>48        </td><td>target         </td></tr>
<tr><td>kegg map                        </td><td>49        </td><td>target         </td></tr>
<tr><td>ko                              </td><td>50        </td><td>target         </td></tr>
<tr><td>mim                             </td><td>51        </td><td>target         </td></tr>
<tr><td>metacyc                         </td><td>52        </td><td>target         </td></tr>
<tr><td>genbank                         </td><td>53        </td><td>target         </td></tr>
<tr><td>opentargets                     </td><td>54        </td><td>target         </td></tr>
<tr><td>orphanet                        </td><td>55        </td><td>target         </td></tr>
<tr><td>panther                         </td><td>56        </td><td>target         </td></tr>
<tr><td>patric                          </td><td>57        </td><td>target         </td></tr>
<tr><td>pdb                             </td><td>58        </td><td>target         </td></tr>
<tr><td>pdbechem                        </td><td>59        </td><td>target         </td></tr>
<tr><td>pfam                            </td><td>61        </td><td>target         </td></tr>
<tr><td>proteomes                       </td><td>62        </td><td>target         </td></tr>
<tr><td>pubmed                          </td><td>63        </td><td>target         </td></tr>
<tr><td>rnacentral                      </td><td>64        </td><td>target         </td></tr>
<tr><td>reactome                        </td><td>65        </td><td>target         </td></tr>
<tr><td>refseq                          </td><td>66        </td><td>target         </td></tr>
<tr><td>string                          </td><td>67        </td><td>target         </td></tr>
<tr><td>supfam                          </td><td>68        </td><td>target         </td></tr>
<tr><td>vgnc                            </td><td>69        </td><td>target         </td></tr>
<tr><td>wbparasite                      </td><td>70        </td><td>target         </td></tr>
<tr><td>ufeature                        </td><td>72        </td><td>target         </td></tr>
<tr><td>variantid                       </td><td>73        </td><td>target         </td></tr>
<tr><td>taxchild                        </td><td>74        </td><td>target         </td></tr>
<tr><td>taxparent                       </td><td>75        </td><td>target         </td></tr>
<tr><td>affy_bovine                     </td><td>76        </td><td>target         </td></tr>
<tr><td>affy_c_elegans                  </td><td>77        </td><td>target         </td></tr>
<tr><td>affy_canine_2                   </td><td>78        </td><td>target         </td></tr>
<tr><td>affy_chicken                    </td><td>79        </td><td>target         </td></tr>
<tr><td>affy_chogene_2_0_st_v1          </td><td>80        </td><td>target         </td></tr>
<tr><td>affy_chogene_2_1_st_v1          </td><td>81        </td><td>target         </td></tr>
<tr><td>affy_cint06a520380f             </td><td>82        </td><td>target         </td></tr>
<tr><td>affy_cyngene_1_0_st_v1          </td><td>83        </td><td>target         </td></tr>
<tr><td>affy_cyrgene_1_0_st_v1          </td><td>84        </td><td>target         </td></tr>
<tr><td>affy_drosgenome1                </td><td>85        </td><td>target         </td></tr>
<tr><td>affy_drosophila_2               </td><td>86        </td><td>target         </td></tr>
<tr><td>affy_equgene_1_0_st_v1          </td><td>87        </td><td>target         </td></tr>
<tr><td>affy_equgene_1_1_st_v1          </td><td>88        </td><td>target         </td></tr>
<tr><td>affy_felgene_1_0_st_v1          </td><td>89        </td><td>target         </td></tr>
<tr><td>affy_felgene_1_1_st_v1          </td><td>90        </td><td>target         </td></tr>
<tr><td>affy_gpl19230                   </td><td>91        </td><td>target         </td></tr>
<tr><td>affy_hc_g110                    </td><td>92        </td><td>target         </td></tr>
<tr><td>affy_hg_focus                   </td><td>93        </td><td>target         </td></tr>
<tr><td>affy_hg_u133_plus_2             </td><td>94        </td><td>target         </td></tr>
<tr><td>affy_hg_u133a                   </td><td>95        </td><td>target         </td></tr>
<tr><td>affy_hg_u133a_2                 </td><td>96        </td><td>target         </td></tr>
<tr><td>affy_hg_u133b                   </td><td>97        </td><td>target         </td></tr>
<tr><td>affy_hg_u95a                    </td><td>98        </td><td>target         </td></tr>
<tr><td>affy_hg_u95av2                  </td><td>99        </td><td>target         </td></tr>
<tr><td>affy_hg_u95b                    </td><td>100       </td><td>target         </td></tr>
<tr><td>affy_hg_u95c                    </td><td>101       </td><td>target         </td></tr>
<tr><td>affy_hg_u95d                    </td><td>102       </td><td>target         </td></tr>
<tr><td>affy_hg_u95e                    </td><td>103       </td><td>target         </td></tr>
<tr><td>affy_hta_2_0                    </td><td>104       </td><td>target         </td></tr>
<tr><td>affy_huex_1_0_st_v2             </td><td>105       </td><td>target         </td></tr>
<tr><td>affy_hugene_1_0_st_v1           </td><td>106       </td><td>target         </td></tr>
<tr><td>affy_hugene_2_0_st_v1           </td><td>107       </td><td>target         </td></tr>
<tr><td>affy_hugenefl                   </td><td>108       </td><td>target         </td></tr>
<tr><td>affy_mg_u74a                    </td><td>109       </td><td>target         </td></tr>
<tr><td>affy_mg_u74av2                  </td><td>110       </td><td>target         </td></tr>
<tr><td>affy_mg_u74b                    </td><td>111       </td><td>target         </td></tr>
<tr><td>affy_mg_u74bv2                  </td><td>112       </td><td>target         </td></tr>
<tr><td>affy_mg_u74c                    </td><td>113       </td><td>target         </td></tr>
<tr><td>affy_mg_u74cv2                  </td><td>114       </td><td>target         </td></tr>
<tr><td>affy_moe430a                    </td><td>115       </td><td>target         </td></tr>
<tr><td>affy_moe430b                    </td><td>116       </td><td>target         </td></tr>
<tr><td>affy_moex_1_0_st_v1             </td><td>117       </td><td>target         </td></tr>
<tr><td>affy_mogene_1_0_st_v1           </td><td>118       </td><td>target         </td></tr>
<tr><td>affy_mogene_2_1_st_v1           </td><td>119       </td><td>target         </td></tr>
<tr><td>affy_mouse430_2                 </td><td>120       </td><td>target         </td></tr>
<tr><td>affy_mouse430a_2                </td><td>121       </td><td>target         </td></tr>
<tr><td>affy_mu11ksuba                  </td><td>122       </td><td>target         </td></tr>
<tr><td>affy_mu11ksubb                  </td><td>123       </td><td>target         </td></tr>
<tr><td>affy_platypus_exon              </td><td>124       </td><td>target         </td></tr>
<tr><td>affy_porcine                    </td><td>125       </td><td>target         </td></tr>
<tr><td>affy_primeview                  </td><td>126       </td><td>target         </td></tr>
<tr><td>affy_rae230a                    </td><td>127       </td><td>target         </td></tr>
<tr><td>affy_rae230b                    </td><td>128       </td><td>target         </td></tr>
<tr><td>affy_raex_1_0_st_v1             </td><td>129       </td><td>target         </td></tr>
<tr><td>affy_ragene_1_0_st_v1           </td><td>130       </td><td>target         </td></tr>
<tr><td>affy_ragene_2_1_st_v1           </td><td>131       </td><td>target         </td></tr>
<tr><td>affy_rat230_2                   </td><td>132       </td><td>target         </td></tr>
<tr><td>affy_rg_u34a                    </td><td>133       </td><td>target         </td></tr>
<tr><td>affy_rg_u34b                    </td><td>134       </td><td>target         </td></tr>
<tr><td>affy_rg_u34c                    </td><td>135       </td><td>target         </td></tr>
<tr><td>affy_rhegene_1_0_st_v1          </td><td>136       </td><td>target         </td></tr>
<tr><td>affy_rhegene_1_1_st_v1          </td><td>137       </td><td>target         </td></tr>
<tr><td>affy_rhesus                     </td><td>138       </td><td>target         </td></tr>
<tr><td>affy_rn_u34                     </td><td>139       </td><td>target         </td></tr>
<tr><td>affy_rt_u34                     </td><td>140       </td><td>target         </td></tr>
<tr><td>affy_u133_x3p                   </td><td>141       </td><td>target         </td></tr>
<tr><td>affy_x_tropicalis               </td><td>142       </td><td>target         </td></tr>
<tr><td>affy_yeast_2                    </td><td>143       </td><td>target         </td></tr>
<tr><td>affy_yg_s98                     </td><td>144       </td><td>target         </td></tr>
<tr><td>affy_zebgene_1_0_st_v1          </td><td>145       </td><td>target         </td></tr>
<tr><td>affy_zebgene_1_1_st_v1          </td><td>146       </td><td>target         </td></tr>
<tr><td>affy_zebrafish                  </td><td>147       </td><td>target         </td></tr>
<tr><td>agilent_012795                  </td><td>148       </td><td>target         </td></tr>
<tr><td>agilent_015061                  </td><td>149       </td><td>target         </td></tr>
<tr><td>agilent_020186                  </td><td>150       </td><td>target         </td></tr>
<tr><td>agilent_037725_hamarrayv        </td><td>151       </td><td>target         </td></tr>
<tr><td>agilent_059389_chicken_ge_8x60k </td><td>152       </td><td>target         </td></tr>
<tr><td>agilent_agilent_8x15k           </td><td>153       </td><td>target         </td></tr>
<tr><td>agilent_arraystar               </td><td>154       </td><td>target         </td></tr>
<tr><td>agilent_cgh_44b                 </td><td>155       </td><td>target         </td></tr>
<tr><td>agilent_cho2agl44v1             </td><td>156       </td><td>target         </td></tr>
<tr><td>agilent_g2518a                  </td><td>157       </td><td>target         </td></tr>
<tr><td>agilent_g2519f                  </td><td>158       </td><td>target         </td></tr>
<tr><td>agilent_gpl10157                </td><td>159       </td><td>target         </td></tr>
<tr><td>agilent_gpl10158                </td><td>160       </td><td>target         </td></tr>
<tr><td>agilent_gpl10248                </td><td>161       </td><td>target         </td></tr>
<tr><td>agilent_gpl13394                </td><td>162       </td><td>target         </td></tr>
<tr><td>agilent_gpl13522                </td><td>163       </td><td>target         </td></tr>
<tr><td>agilent_gpl13723                </td><td>164       </td><td>target         </td></tr>
<tr><td>agilent_gpl13795                </td><td>165       </td><td>target         </td></tr>
<tr><td>agilent_gpl13914                </td><td>166       </td><td>target         </td></tr>
<tr><td>agilent_gpl14144                </td><td>167       </td><td>target         </td></tr>
<tr><td>agilent_gpl14629                </td><td>168       </td><td>target         </td></tr>
<tr><td>agilent_gpl14664                </td><td>169       </td><td>target         </td></tr>
<tr><td>agilent_gpl15189                </td><td>170       </td><td>target         </td></tr>
<tr><td>agilent_gpl15190                </td><td>171       </td><td>target         </td></tr>
<tr><td>agilent_gpl15204                </td><td>172       </td><td>target         </td></tr>
<tr><td>agilent_gpl15217                </td><td>173       </td><td>target         </td></tr>
<tr><td>agilent_gpl15450                </td><td>174       </td><td>target         </td></tr>
<tr><td>agilent_gpl15747                </td><td>175       </td><td>target         </td></tr>
<tr><td>agilent_gpl15799                </td><td>176       </td><td>target         </td></tr>
<tr><td>agilent_gpl16032                </td><td>177       </td><td>target         </td></tr>
<tr><td>agilent_gpl16776                </td><td>178       </td><td>target         </td></tr>
<tr><td>agilent_gpl17465                </td><td>179       </td><td>target         </td></tr>
<tr><td>agilent_gpl17670                </td><td>180       </td><td>target         </td></tr>
<tr><td>agilent_gpl17686                </td><td>181       </td><td>target         </td></tr>
<tr><td>agilent_gpl17689                </td><td>182       </td><td>target         </td></tr>
<tr><td>agilent_gpl18520                </td><td>183       </td><td>target         </td></tr>
<tr><td>agilent_gpl18606                </td><td>184       </td><td>target         </td></tr>
<tr><td>agilent_gpl19384                </td><td>185       </td><td>target         </td></tr>
<tr><td>agilent_gpl19516                </td><td>186       </td><td>target         </td></tr>
<tr><td>agilent_gpl19564                </td><td>187       </td><td>target         </td></tr>
<tr><td>agilent_gpl19666                </td><td>188       </td><td>target         </td></tr>
<tr><td>agilent_gpl20686                </td><td>189       </td><td>target         </td></tr>
<tr><td>agilent_gpl20834                </td><td>190       </td><td>target         </td></tr>
<tr><td>agilent_gpl20900                </td><td>191       </td><td>target         </td></tr>
<tr><td>agilent_gpl20908                </td><td>192       </td><td>target         </td></tr>
<tr><td>agilent_gpl21244                </td><td>193       </td><td>target         </td></tr>
<tr><td>agilent_gpl21361                </td><td>194       </td><td>target         </td></tr>
<tr><td>agilent_gpl21860                </td><td>195       </td><td>target         </td></tr>
<tr><td>agilent_gpl22083                </td><td>196       </td><td>target         </td></tr>
<tr><td>agilent_gpl23036                </td><td>197       </td><td>target         </td></tr>
<tr><td>agilent_gpl23307                </td><td>198       </td><td>target         </td></tr>
<tr><td>agilent_gpl23389                </td><td>199       </td><td>target         </td></tr>
<tr><td>agilent_gpl6848                 </td><td>200       </td><td>target         </td></tr>
<tr><td>agilent_gpl7244                 </td><td>201       </td><td>target         </td></tr>
<tr><td>agilent_gpl7301                 </td><td>202       </td><td>target         </td></tr>
<tr><td>agilent_gpl7302                 </td><td>203       </td><td>target         </td></tr>
<tr><td>agilent_gpl7735                 </td><td>204       </td><td>target         </td></tr>
<tr><td>agilent_gpl7801                 </td><td>205       </td><td>target         </td></tr>
<tr><td>agilent_gpl8304                 </td><td>206       </td><td>target         </td></tr>
<tr><td>agilent_gpl9060                 </td><td>207       </td><td>target         </td></tr>
<tr><td>agilent_gpl9074                 </td><td>208       </td><td>target         </td></tr>
<tr><td>agilent_sureprint_g3_ge_8x60k   </td><td>209       </td><td>target         </td></tr>
<tr><td>agilent_sureprint_g3_ge_8x60k_v2</td><td>210       </td><td>target         </td></tr>
<tr><td>agilent_sureprint_gpl7083_4x44k </td><td>211       </td><td>target         </td></tr>
<tr><td>agilent_sureprnt_gpl16709_4x44k </td><td>212       </td><td>target         </td></tr>
<tr><td>agilent_wholegenome             </td><td>213       </td><td>target         </td></tr>
<tr><td>agilent_wholegenome_4x44k       </td><td>214       </td><td>target         </td></tr>
<tr><td>agilent_wholegenome_4x44k_v1    </td><td>215       </td><td>target         </td></tr>
<tr><td>agilent_wholegenome_4x44k_v2    </td><td>216       </td><td>target         </td></tr>
<tr><td>agilent_wholegenome_4x44k_v3    </td><td>217       </td><td>target         </td></tr>
<tr><td>codelink_codelink               </td><td>218       </td><td>target         </td></tr>
<tr><td>illumina_humanht_12_v3          </td><td>219       </td><td>target         </td></tr>
<tr><td>illumina_humanht_12_v4          </td><td>220       </td><td>target         </td></tr>
<tr><td>illumina_humanref_8_v3          </td><td>221       </td><td>target         </td></tr>
<tr><td>illumina_humanwg_6_v1           </td><td>222       </td><td>target         </td></tr>
<tr><td>illumina_humanwg_6_v2           </td><td>223       </td><td>target         </td></tr>
<tr><td>illumina_humanwg_6_v3           </td><td>224       </td><td>target         </td></tr>
<tr><td>illumina_mouseref_8             </td><td>225       </td><td>target         </td></tr>
<tr><td>illumina_mousewg_6_v1           </td><td>226       </td><td>target         </td></tr>
<tr><td>illumina_mousewg_6_v2           </td><td>227       </td><td>target         </td></tr>
<tr><td>illumina_ratref_12_v1           </td><td>228       </td><td>target         </td></tr>
<tr><td>leiden_leiden2                  </td><td>229       </td><td>target         </td></tr>
<tr><td>leiden_leiden3                  </td><td>230       </td><td>target         </td></tr>
<tr><td>nimblegen_gpl10076              </td><td>231       </td><td>target         </td></tr>
<tr><td>nimblegen_gpl10392              </td><td>232       </td><td>target         </td></tr>
<tr><td>nimblegen_gpl13318              </td><td>233       </td><td>target         </td></tr>
<tr><td>nimblegen_gpl13762              </td><td>234       </td><td>target         </td></tr>
<tr><td>nimblegen_gpl13784              </td><td>235       </td><td>target         </td></tr>
<tr><td>nimblegen_gpl14375              </td><td>236       </td><td>target         </td></tr>
<tr><td>nimblegen_gpl14562              </td><td>237       </td><td>target         </td></tr>
<tr><td>nimblegen_gpl14607              </td><td>238       </td><td>target         </td></tr>
<tr><td>nimblegen_gpl14994              </td><td>239       </td><td>target         </td></tr>
<tr><td>nimblegen_gpl17210              </td><td>240       </td><td>target         </td></tr>
<tr><td>nimblegen_gpl21301              </td><td>241       </td><td>target         </td></tr>
<tr><td>nimblegen_gpl21560              </td><td>242       </td><td>target         </td></tr>
<tr><td>nimblegen_gpl22527              </td><td>243       </td><td>target         </td></tr>
<tr><td>nimblegen_gpl7338               </td><td>244       </td><td>target         </td></tr>
<tr><td>nimblegen_gpl8673               </td><td>245       </td><td>target         </td></tr>
<tr><td>nimblegen_nimblegen_13k         </td><td>246       </td><td>target         </td></tr>
<tr><td>phalanx_onearray                </td><td>247       </td><td>target         </td></tr>
<tr><td>slri_gpl3518                    </td><td>248       </td><td>target         </td></tr>
<tr><td>ucsf_gpl9450                    </td><td>249       </td><td>target         </td></tr>
<tr><td>wustl_wustl_c_elegans           </td><td>250       </td><td>target         </td></tr>
<tr><td>affy_aragene                    </td><td>251       </td><td>target         </td></tr>
<tr><td>affy_ath1_121501                </td><td>252       </td><td>target         </td></tr>
<tr><td>affy_barley1                    </td><td>253       </td><td>target         </td></tr>
<tr><td>affy_cotton                     </td><td>254       </td><td>target         </td></tr>
<tr><td>affy_gpl16720                   </td><td>255       </td><td>target         </td></tr>
<tr><td>affy_maize                      </td><td>256       </td><td>target         </td></tr>
<tr><td>affy_medgene                    </td><td>257       </td><td>target         </td></tr>
<tr><td>affy_medicago                   </td><td>258       </td><td>target         </td></tr>
<tr><td>affy_poplar                     </td><td>259       </td><td>target         </td></tr>
<tr><td>affy_rcngene                    </td><td>260       </td><td>target         </td></tr>
<tr><td>affy_rice                       </td><td>261       </td><td>target         </td></tr>
<tr><td>affy_rjpgene                    </td><td>262       </td><td>target         </td></tr>
<tr><td>affy_soybean                    </td><td>263       </td><td>target         </td></tr>
<tr><td>affy_soygene                    </td><td>264       </td><td>target         </td></tr>
<tr><td>affy_tomato                     </td><td>265       </td><td>target         </td></tr>
<tr><td>affy_tomgene                    </td><td>266       </td><td>target         </td></tr>
<tr><td>affy_vitis_vinifera             </td><td>267       </td><td>target         </td></tr>
<tr><td>affy_wheat                      </td><td>268       </td><td>target         </td></tr>
<tr><td>agilent_g2519f_015058           </td><td>269       </td><td>target         </td></tr>
<tr><td>agilent_g2519f_015059           </td><td>270       </td><td>target         </td></tr>
<tr><td>agilent_g2519f_015241           </td><td>271       </td><td>target         </td></tr>
<tr><td>agilent_g2519f_016047           </td><td>272       </td><td>target         </td></tr>
<tr><td>agilent_g2519f_016772           </td><td>273       </td><td>target         </td></tr>
<tr><td>agilent_g2519f_021113           </td><td>274       </td><td>target         </td></tr>
<tr><td>agilent_g2519f_021169           </td><td>275       </td><td>target         </td></tr>
<tr><td>agilent_g2519f_021623           </td><td>276       </td><td>target         </td></tr>
<tr><td>agilent_g2519f_022270           </td><td>277       </td><td>target         </td></tr>
<tr><td>agilent_g2519f_022297           </td><td>278       </td><td>target         </td></tr>
<tr><td>agilent_g2519f_022520           </td><td>279       </td><td>target         </td></tr>
<tr><td>agilent_g2519f_022523           </td><td>280       </td><td>target         </td></tr>
<tr><td>agilent_g2519f_022524           </td><td>281       </td><td>target         </td></tr>
<tr><td>agilent_g4136a_011839           </td><td>282       </td><td>target         </td></tr>
<tr><td>agilent_g4136b_013324           </td><td>283       </td><td>target         </td></tr>
<tr><td>agilent_g4138a_012106           </td><td>284       </td><td>target         </td></tr>
<tr><td>agilent_g4142a_012600           </td><td>285       </td><td>target         </td></tr>
<tr><td>catma_catma_v5                  </td><td>286       </td><td>target         </td></tr>
<tr><td>nsf_bgiyale                     </td><td>287       </td><td>target         </td></tr>
<tr><td>nsf_nsf20k                      </td><td>288       </td><td>target         </td></tr>
<tr><td>nsf_nsf45k                      </td><td>289       </td><td>target         </td></tr>
<tr><td>gochild                         </td><td>290       </td><td>target         </td></tr>
<tr><td>goparent                        </td><td>291       </td><td>target         </td></tr>
<tr><td>efochild                        </td><td>292       </td><td>target         </td></tr>
<tr><td>efoparent                       </td><td>293       </td><td>target         </td></tr>
<tr><td>eprotein                        </td><td>294       </td><td>target         </td></tr>
<tr><td>affy_unibasa_gossypiisysyng001a </td><td>295       </td><td>target         </td></tr>
<tr><td>ecochild                        </td><td>296       </td><td>target         </td></tr>
<tr><td>ecoparent                       </td><td>297       </td><td>target         </td></tr>
<tr><td>allergome                       </td><td>500       </td><td>target         </td></tr>
<tr><td>arachnoserver                   </td><td>501       </td><td>target         </td></tr>
<tr><td>araport                         </td><td>502       </td><td>target         </td></tr>
<tr><td>beilstein                       </td><td>503       </td><td>target         </td></tr>
<tr><td>bgee                            </td><td>504       </td><td>target         </td></tr>
<tr><td>bindingdb                       </td><td>505       </td><td>target         </td></tr>
<tr><td>biocyc                          </td><td>506       </td><td>target         </td></tr>
<tr><td>biogrid                         </td><td>507       </td><td>target         </td></tr>
<tr><td>biomuta                         </td><td>508       </td><td>target         </td></tr>
<tr><td>brenda                          </td><td>509       </td><td>target         </td></tr>
<tr><td>carbonyldb                      </td><td>510       </td><td>target         </td></tr>
<tr><td>cas                             </td><td>511       </td><td>target         </td></tr>
<tr><td>cazy                            </td><td>512       </td><td>target         </td></tr>
<tr><td>cgd                             </td><td>513       </td><td>target         </td></tr>
<tr><td>chemidplus                      </td><td>514       </td><td>target         </td></tr>
<tr><td>chemspider                      </td><td>515       </td><td>target         </td></tr>
<tr><td>chitars                         </td><td>516       </td><td>target         </td></tr>
<tr><td>cleanex                         </td><td>517       </td><td>target         </td></tr>
<tr><td>collectf                        </td><td>518       </td><td>target         </td></tr>
<tr><td>come                            </td><td>519       </td><td>target         </td></tr>
<tr><td>complexportal                   </td><td>520       </td><td>target         </td></tr>
<tr><td>compluyeast-2dpage              </td><td>521       </td><td>target         </td></tr>
<tr><td>conoserver                      </td><td>522       </td><td>target         </td></tr>
<tr><td>corum                           </td><td>523       </td><td>target         </td></tr>
<tr><td>cosmic                          </td><td>524       </td><td>target         </td></tr>
<tr><td>ctd                             </td><td>525       </td><td>target         </td></tr>
<tr><td>depod                           </td><td>526       </td><td>target         </td></tr>
<tr><td>dictybase                       </td><td>527       </td><td>target         </td></tr>
<tr><td>dip                             </td><td>528       </td><td>target         </td></tr>
<tr><td>disgenet                        </td><td>529       </td><td>target         </td></tr>
<tr><td>disprot                         </td><td>530       </td><td>target         </td></tr>
<tr><td>dmdm                            </td><td>531       </td><td>target         </td></tr>
<tr><td>dnasu                           </td><td>532       </td><td>target         </td></tr>
<tr><td>drugcentral                     </td><td>533       </td><td>target         </td></tr>
<tr><td>echobase                        </td><td>534       </td><td>target         </td></tr>
<tr><td>ecmdb                           </td><td>535       </td><td>target         </td></tr>
<tr><td>ecogene                         </td><td>536       </td><td>target         </td></tr>
<tr><td>eggnog                          </td><td>537       </td><td>target         </td></tr>
<tr><td>elm                             </td><td>538       </td><td>target         </td></tr>
<tr><td>epd                             </td><td>539       </td><td>target         </td></tr>
<tr><td>epo                             </td><td>540       </td><td>target         </td></tr>
<tr><td>esther                          </td><td>541       </td><td>target         </td></tr>
<tr><td>euhcvdb                         </td><td>542       </td><td>target         </td></tr>
<tr><td>eupathdb                        </td><td>543       </td><td>target         </td></tr>
<tr><td>evolutionarytrace               </td><td>544       </td><td>target         </td></tr>
<tr><td>foodb                           </td><td>546       </td><td>target         </td></tr>
<tr><td>genecards                       </td><td>547       </td><td>target         </td></tr>
<tr><td>genedb                          </td><td>548       </td><td>target         </td></tr>
<tr><td>genereviews                     </td><td>549       </td><td>target         </td></tr>
<tr><td>genetree                        </td><td>550       </td><td>target         </td></tr>
<tr><td>genevisible                     </td><td>551       </td><td>target         </td></tr>
<tr><td>genomernai                      </td><td>552       </td><td>target         </td></tr>
<tr><td>glyconnect                      </td><td>553       </td><td>target         </td></tr>
<tr><td>glytoucan                       </td><td>554       </td><td>target         </td></tr>
<tr><td>gramene                         </td><td>555       </td><td>target         </td></tr>
<tr><td>guidetopharmacology             </td><td>556       </td><td>target         </td></tr>
<tr><td>h-invdb                         </td><td>557       </td><td>target         </td></tr>
<tr><td>hogenom                         </td><td>558       </td><td>target         </td></tr>
<tr><td>hovergen                        </td><td>559       </td><td>target         </td></tr>
<tr><td>hpa                             </td><td>560       </td><td>target         </td></tr>
<tr><td>imgt_gene-db                    </td><td>561       </td><td>target         </td></tr>
<tr><td>inparanoid                      </td><td>562       </td><td>target         </td></tr>
<tr><td>iptmnet                         </td><td>563       </td><td>target         </td></tr>
<tr><td>iuphar                          </td><td>564       </td><td>target         </td></tr>
<tr><td>jpo                             </td><td>565       </td><td>target         </td></tr>
<tr><td>kipo                            </td><td>566       </td><td>target         </td></tr>
<tr><td>knapsack                        </td><td>567       </td><td>target         </td></tr>
<tr><td>legiolist                       </td><td>568       </td><td>target         </td></tr>
<tr><td>leproma                         </td><td>569       </td><td>target         </td></tr>
<tr><td>lincs                           </td><td>570       </td><td>target         </td></tr>
<tr><td>lipidmapsclass                  </td><td>571       </td><td>target         </td></tr>
<tr><td>lipidmapsinstance               </td><td>572       </td><td>target         </td></tr>
<tr><td>maizegdb                        </td><td>573       </td><td>target         </td></tr>
<tr><td>malacards                       </td><td>574       </td><td>target         </td></tr>
<tr><td>maxqb                           </td><td>575       </td><td>target         </td></tr>
<tr><td>merops                          </td><td>576       </td><td>target         </td></tr>
<tr><td>mgi                             </td><td>577       </td><td>target         </td></tr>
<tr><td>mint                            </td><td>578       </td><td>target         </td></tr>
<tr><td>moondb                          </td><td>579       </td><td>target         </td></tr>
<tr><td>mycoclap                        </td><td>580       </td><td>target         </td></tr>
<tr><td>nextprot                        </td><td>581       </td><td>target         </td></tr>
<tr><td>ogp                             </td><td>582       </td><td>target         </td></tr>
<tr><td>oma                             </td><td>583       </td><td>target         </td></tr>
<tr><td>orthodb                         </td><td>584       </td><td>target         </td></tr>
<tr><td>patent                          </td><td>585       </td><td>target         </td></tr>
<tr><td>paxdb                           </td><td>586       </td><td>target         </td></tr>
<tr><td>peptideatlas                    </td><td>587       </td><td>target         </td></tr>
<tr><td>peroxibase                      </td><td>588       </td><td>target         </td></tr>
<tr><td>phosphositeplus                 </td><td>589       </td><td>target         </td></tr>
<tr><td>phylomedb                       </td><td>590       </td><td>target         </td></tr>
<tr><td>pir                             </td><td>591       </td><td>target         </td></tr>
<tr><td>pirsf                           </td><td>592       </td><td>target         </td></tr>
<tr><td>pirsr                           </td><td>593       </td><td>target         </td></tr>
<tr><td>pmap-cutdb                      </td><td>594       </td><td>target         </td></tr>
<tr><td>pombase                         </td><td>595       </td><td>target         </td></tr>
<tr><td>ppdb                            </td><td>596       </td><td>target         </td></tr>
<tr><td>prf                             </td><td>597       </td><td>target         </td></tr>
<tr><td>priam                           </td><td>598       </td><td>target         </td></tr>
<tr><td>prints                          </td><td>599       </td><td>target         </td></tr>
<tr><td>pro                             </td><td>600       </td><td>target         </td></tr>
<tr><td>prodom                          </td><td>601       </td><td>target         </td></tr>
<tr><td>profile                         </td><td>602       </td><td>target         </td></tr>
<tr><td>promex                          </td><td>603       </td><td>target         </td></tr>
<tr><td>proteinmodelportal              </td><td>604       </td><td>target         </td></tr>
<tr><td>proteomicsdb                    </td><td>605       </td><td>target         </td></tr>
<tr><td>pseudocap                       </td><td>606       </td><td>target         </td></tr>
<tr><td>reax                            </td><td>607       </td><td>target         </td></tr>
<tr><td>rebase                          </td><td>608       </td><td>target         </td></tr>
<tr><td>reproduction-2dpage             </td><td>609       </td><td>target         </td></tr>
<tr><td>resid                           </td><td>610       </td><td>target         </td></tr>
<tr><td>rgd                             </td><td>611       </td><td>target         </td></tr>
<tr><td>rulebase                        </td><td>612       </td><td>target         </td></tr>
<tr><td>saas                            </td><td>613       </td><td>target         </td></tr>
<tr><td>sabio-rk                        </td><td>614       </td><td>target         </td></tr>
<tr><td>sam                             </td><td>615       </td><td>target         </td></tr>
<tr><td>scop                            </td><td>616       </td><td>target         </td></tr>
<tr><td>seed                            </td><td>617       </td><td>target         </td></tr>
<tr><td>sfld                            </td><td>618       </td><td>target         </td></tr>
<tr><td>sgd                             </td><td>619       </td><td>target         </td></tr>
<tr><td>signalink                       </td><td>620       </td><td>target         </td></tr>
<tr><td>signor                          </td><td>621       </td><td>target         </td></tr>
<tr><td>smart                           </td><td>622       </td><td>target         </td></tr>
<tr><td>smid                            </td><td>623       </td><td>target         </td></tr>
<tr><td>smr                             </td><td>624       </td><td>target         </td></tr>
<tr><td>ssf                             </td><td>625       </td><td>target         </td></tr>
<tr><td>swiss-2dpage                    </td><td>627       </td><td>target         </td></tr>
<tr><td>swisslipids                     </td><td>628       </td><td>target         </td></tr>
<tr><td>swisspalm                       </td><td>629       </td><td>target         </td></tr>
<tr><td>tair                            </td><td>630       </td><td>target         </td></tr>
<tr><td>tcdb                            </td><td>631       </td><td>target         </td></tr>
<tr><td>tigrfams                        </td><td>632       </td><td>target         </td></tr>
<tr><td>topdownproteomics               </td><td>633       </td><td>target         </td></tr>
<tr><td>treefam                         </td><td>634       </td><td>target         </td></tr>
<tr><td>trome                           </td><td>635       </td><td>target         </td></tr>
<tr><td>tuberculist                     </td><td>636       </td><td>target         </td></tr>
<tr><td>ucd-2dpage                      </td><td>637       </td><td>target         </td></tr>
<tr><td>ucsc                            </td><td>638       </td><td>target         </td></tr>
<tr><td>unicarbkb                       </td><td>639       </td><td>target         </td></tr>
<tr><td>unilectin                       </td><td>640       </td><td>target         </td></tr>
<tr><td>unimes                          </td><td>641       </td><td>target         </td></tr>
<tr><td>unipathway                      </td><td>642       </td><td>target         </td></tr>
<tr><td>uspto                           </td><td>643       </td><td>target         </td></tr>
<tr><td>vectorbase                      </td><td>644       </td><td>target         </td></tr>
<tr><td>vega                            </td><td>645       </td><td>target         </td></tr>
<tr><td>vsdb                            </td><td>646       </td><td>target         </td></tr>
<tr><td>world-2dpage                    </td><td>647       </td><td>target         </td></tr>
<tr><td>wormbase                        </td><td>648       </td><td>target         </td></tr>
<tr><td>xenbase                         </td><td>649       </td><td>target         </td></tr>
<tr><td>ymdb                            </td><td>650       </td><td>target         </td></tr>
<tr><td>zfin                            </td><td>651       </td><td>target         </td></tr>
</tbody>
</table>



```python
res=bb.listGenomes("ensembl")

#print(res)
display(HTML(tabulate.tabulate(res, tablefmt='html',headers=["genomes"])))
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
<tr><td>apteryx_haastii                 </td></tr>
<tr><td>apteryx_owenii                  </td></tr>
<tr><td>apteryx_rowi                    </td></tr>
<tr><td>astatotilapia_calliptera        </td></tr>
<tr><td>astyanax_mexicanus              </td></tr>
<tr><td>astyanax_mexicanus_pachon       </td></tr>
<tr><td>betta_splendens                 </td></tr>
<tr><td>bison_bison_bison               </td></tr>
<tr><td>bos_indicus_hybrid              </td></tr>
<tr><td>bos_mutus                       </td></tr>
<tr><td>bos_taurus                      </td></tr>
<tr><td>bos_taurus_hybrid               </td></tr>
<tr><td>caenorhabditis_elegans          </td></tr>
<tr><td>calidris_pugnax                 </td></tr>
<tr><td>calidris_pygmaea                </td></tr>
<tr><td>callithrix_jacchus              </td></tr>
<tr><td>callorhinchus_milii             </td></tr>
<tr><td>canis_familiaris                </td></tr>
<tr><td>canis_lupus_dingo               </td></tr>
<tr><td>capra_hircus                    </td></tr>
<tr><td>carlito_syrichta                </td></tr>
<tr><td>castor_canadensis               </td></tr>
<tr><td>cavia_aperea                    </td></tr>
<tr><td>cavia_porcellus                 </td></tr>
<tr><td>cebus_capucinus                 </td></tr>
<tr><td>cercocebus_atys                 </td></tr>
<tr><td>chelonoidis_abingdonii          </td></tr>
<tr><td>chinchilla_lanigera             </td></tr>
<tr><td>chlorocebus_sabaeus             </td></tr>
<tr><td>choloepus_hoffmanni             </td></tr>
<tr><td>chrysemys_picta_bellii          </td></tr>
<tr><td>ciona_intestinalis              </td></tr>
<tr><td>ciona_savignyi                  </td></tr>
<tr><td>clupea_harengus                 </td></tr>
<tr><td>colobus_angolensis_palliatus    </td></tr>
<tr><td>cottoperca_gobio                </td></tr>
<tr><td>coturnix_japonica               </td></tr>
<tr><td>cricetulus_griseus_chok1gshd    </td></tr>
<tr><td>cricetulus_griseus_crigri       </td></tr>
<tr><td>cricetulus_griseus_picr         </td></tr>
<tr><td>crocodylus_porosus              </td></tr>
<tr><td>cyanistes_caeruleus             </td></tr>
<tr><td>cynoglossus_semilaevis          </td></tr>
<tr><td>cyprinodon_variegatus           </td></tr>
<tr><td>danio_rerio                     </td></tr>
<tr><td>dasypus_novemcinctus            </td></tr>
<tr><td>denticeps_clupeoides            </td></tr>
<tr><td>dipodomys_ordii                 </td></tr>
<tr><td>dromaius_novaehollandiae        </td></tr>
<tr><td>drosophila_melanogaster         </td></tr>
<tr><td>echinops_telfairi               </td></tr>
<tr><td>electrophorus_electricus        </td></tr>
<tr><td>eptatretus_burgeri              </td></tr>
<tr><td>equus_asinus_asinus             </td></tr>
<tr><td>equus_caballus                  </td></tr>
<tr><td>erinaceus_europaeus             </td></tr>
<tr><td>erpetoichthys_calabaricus       </td></tr>
<tr><td>esox_lucius                     </td></tr>
<tr><td>felis_catus                     </td></tr>
<tr><td>ficedula_albicollis             </td></tr>
<tr><td>fukomys_damarensis              </td></tr>
<tr><td>fundulus_heteroclitus           </td></tr>
<tr><td>gadus_morhua                    </td></tr>
<tr><td>gallus_gallus                   </td></tr>
<tr><td>gambusia_affinis                </td></tr>
<tr><td>gasterosteus_aculeatus          </td></tr>
<tr><td>gopherus_agassizii              </td></tr>
<tr><td>gorilla_gorilla                 </td></tr>
<tr><td>gouania_willdenowi              </td></tr>
<tr><td>haplochromis_burtoni            </td></tr>
<tr><td>heterocephalus_glaber_female    </td></tr>
<tr><td>heterocephalus_glaber_male      </td></tr>
<tr><td>hippocampus_comes               </td></tr>
<tr><td>homo_sapiens                    </td></tr>
<tr><td>hucho_hucho                     </td></tr>
<tr><td>ictalurus_punctatus             </td></tr>
<tr><td>ictidomys_tridecemlineatus      </td></tr>
<tr><td>jaculus_jaculus                 </td></tr>
<tr><td>junco_hyemalis                  </td></tr>
<tr><td>kryptolebias_marmoratus         </td></tr>
<tr><td>labrus_bergylta                 </td></tr>
<tr><td>larimichthys_crocea             </td></tr>
<tr><td>lates_calcarifer                </td></tr>
<tr><td>latimeria_chalumnae             </td></tr>
<tr><td>lepidothrix_coronata            </td></tr>
<tr><td>lepisosteus_oculatus            </td></tr>
<tr><td>lonchura_striata_domestica      </td></tr>
<tr><td>loxodonta_africana              </td></tr>
<tr><td>macaca_fascicularis             </td></tr>
<tr><td>macaca_mulatta                  </td></tr>
<tr><td>macaca_nemestrina               </td></tr>
<tr><td>manacus_vitellinus              </td></tr>
<tr><td>mandrillus_leucophaeus          </td></tr>
<tr><td>marmota_marmota_marmota         </td></tr>
<tr><td>mastacembelus_armatus           </td></tr>
<tr><td>maylandia_zebra                 </td></tr>
<tr><td>meleagris_gallopavo             </td></tr>
<tr><td>melopsittacus_undulatus         </td></tr>
<tr><td>meriones_unguiculatus           </td></tr>
<tr><td>mesocricetus_auratus            </td></tr>
<tr><td>microcebus_murinus              </td></tr>
<tr><td>microtus_ochrogaster            </td></tr>
<tr><td>mola_mola                       </td></tr>
<tr><td>monodelphis_domestica           </td></tr>
<tr><td>monopterus_albus                </td></tr>
<tr><td>mus_caroli                      </td></tr>
<tr><td>mus_musculus                    </td></tr>
<tr><td>mus_musculus_129s1svimj         </td></tr>
<tr><td>mus_musculus_aj                 </td></tr>
<tr><td>mus_musculus_akrj               </td></tr>
<tr><td>mus_musculus_balbcj             </td></tr>
<tr><td>mus_musculus_c3hhej             </td></tr>
<tr><td>mus_musculus_c57bl6nj           </td></tr>
<tr><td>mus_musculus_casteij            </td></tr>
<tr><td>mus_musculus_cbaj               </td></tr>
<tr><td>mus_musculus_dba2j              </td></tr>
<tr><td>mus_musculus_fvbnj              </td></tr>
<tr><td>mus_musculus_lpj                </td></tr>
<tr><td>mus_musculus_nodshiltj          </td></tr>
<tr><td>mus_musculus_nzohlltj           </td></tr>
<tr><td>mus_musculus_pwkphj             </td></tr>
<tr><td>mus_musculus_wsbeij             </td></tr>
<tr><td>mus_pahari                      </td></tr>
<tr><td>mus_spicilegus                  </td></tr>
<tr><td>mus_spretus                     </td></tr>
<tr><td>mustela_putorius_furo           </td></tr>
<tr><td>myotis_lucifugus                </td></tr>
<tr><td>nannospalax_galili              </td></tr>
<tr><td>neolamprologus_brichardi        </td></tr>
<tr><td>neovison_vison                  </td></tr>
<tr><td>nomascus_leucogenys             </td></tr>
<tr><td>notamacropus_eugenii            </td></tr>
<tr><td>notechis_scutatus               </td></tr>
<tr><td>nothoprocta_perdicaria          </td></tr>
<tr><td>numida_meleagris                </td></tr>
<tr><td>ochotona_princeps               </td></tr>
<tr><td>octodon_degus                   </td></tr>
<tr><td>oreochromis_niloticus           </td></tr>
<tr><td>ornithorhynchus_anatinus        </td></tr>
<tr><td>oryctolagus_cuniculus           </td></tr>
<tr><td>oryzias_latipes                 </td></tr>
<tr><td>oryzias_latipes_hni             </td></tr>
<tr><td>oryzias_latipes_hsok            </td></tr>
<tr><td>oryzias_melastigma              </td></tr>
<tr><td>otolemur_garnettii              </td></tr>
<tr><td>ovis_aries                      </td></tr>
<tr><td>pan_paniscus                    </td></tr>
<tr><td>pan_troglodytes                 </td></tr>
<tr><td>panthera_pardus                 </td></tr>
<tr><td>panthera_tigris_altaica         </td></tr>
<tr><td>papio_anubis                    </td></tr>
<tr><td>parambassis_ranga               </td></tr>
<tr><td>paramormyrops_kingsleyae        </td></tr>
<tr><td>parus_major                     </td></tr>
<tr><td>pelodiscus_sinensis             </td></tr>
<tr><td>periophthalmus_magnuspinnatus   </td></tr>
<tr><td>peromyscus_maniculatus_bairdii  </td></tr>
<tr><td>petromyzon_marinus              </td></tr>
<tr><td>phascolarctos_cinereus          </td></tr>
<tr><td>piliocolobus_tephrosceles       </td></tr>
<tr><td>poecilia_formosa                </td></tr>
<tr><td>poecilia_latipinna              </td></tr>
<tr><td>poecilia_mexicana               </td></tr>
<tr><td>poecilia_reticulata             </td></tr>
<tr><td>pogona_vitticeps                </td></tr>
<tr><td>pongo_abelii                    </td></tr>
<tr><td>procavia_capensis               </td></tr>
<tr><td>prolemur_simus                  </td></tr>
<tr><td>propithecus_coquereli           </td></tr>
<tr><td>pteropus_vampyrus               </td></tr>
<tr><td>pundamilia_nyererei             </td></tr>
<tr><td>pygocentrus_nattereri           </td></tr>
<tr><td>rattus_norvegicus               </td></tr>
<tr><td>rhinopithecus_bieti             </td></tr>
<tr><td>rhinopithecus_roxellana         </td></tr>
<tr><td>saccharomyces_cerevisiae        </td></tr>
<tr><td>saimiri_boliviensis_boliviensis </td></tr>
<tr><td>salvator_merianae               </td></tr>
<tr><td>sarcophilus_harrisii            </td></tr>
<tr><td>scleropages_formosus            </td></tr>
<tr><td>scophthalmus_maximus            </td></tr>
<tr><td>serinus_canaria                 </td></tr>
<tr><td>seriola_dumerili                </td></tr>
<tr><td>seriola_lalandi_dorsalis        </td></tr>
<tr><td>sorex_araneus                   </td></tr>
<tr><td>spermophilus_dauricus           </td></tr>
<tr><td>sphenodon_punctatus             </td></tr>
<tr><td>stegastes_partitus              </td></tr>
<tr><td>sus_scrofa                      </td></tr>
<tr><td>sus_scrofa_bamei                </td></tr>
<tr><td>sus_scrofa_berkshire            </td></tr>
<tr><td>sus_scrofa_hampshire            </td></tr>
<tr><td>sus_scrofa_jinhua               </td></tr>
<tr><td>sus_scrofa_landrace             </td></tr>
<tr><td>sus_scrofa_largewhite           </td></tr>
<tr><td>sus_scrofa_meishan              </td></tr>
<tr><td>sus_scrofa_pietrain             </td></tr>
<tr><td>sus_scrofa_rongchang            </td></tr>
<tr><td>sus_scrofa_tibetan              </td></tr>
<tr><td>sus_scrofa_usmarc               </td></tr>
<tr><td>sus_scrofa_wuzhishan            </td></tr>
<tr><td>taeniopygia_guttata             </td></tr>
<tr><td>takifugu_rubripes               </td></tr>
<tr><td>tetraodon_nigroviridis          </td></tr>
<tr><td>theropithecus_gelada            </td></tr>
<tr><td>tupaia_belangeri                </td></tr>
<tr><td>tursiops_truncatus              </td></tr>
<tr><td>urocitellus_parryii             </td></tr>
<tr><td>ursus_americanus                </td></tr>
<tr><td>ursus_maritimus                 </td></tr>
<tr><td>vicugna_pacos                   </td></tr>
<tr><td>vombatus_ursinus                </td></tr>
<tr><td>vulpes_vulpes                   </td></tr>
<tr><td>xenopus_tropicalis              </td></tr>
<tr><td>xiphophorus_couchianus          </td></tr>
<tr><td>xiphophorus_maculatus           </td></tr>
<tr><td>zonotrichia_albicollis          </td></tr>
</tbody>
</table>



```python
bb.listGenomes("ensembl_bacteria")
```


```python
bb.listGenomes("ensembl_fungi")
```


```python
bb.listGenomes("ensembl_metazoa")
```


```python
bb.listGenomes("ensembl_plants")
```


```python
bb.listGenomes("ensembl_protists")
```

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
