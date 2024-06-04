# Análisis de Mutaciones Somáticas de CHEK1 y BRDA4

## Descripción del Proyecto

Este repositorio contiene los scripts y datos para el análisis de mutaciones somáticas en los genes CHEK1 y BRDA4 a partir de las bases de datos COSMIC, ICGC y TCGA. 


## Objetivos

1. Extraer mutaciones somáticas de los genes CHEK1 y BRDA4 de las bases de datos COSMIC, ICGC y TCGA.
2. Analizar la concurrencia de mutaciones somáticas entre CHEK1 y BRDA4.

## Archivos y Directorios

```plaintext
TFM
├── COSMIC
│   ├── actionability
│   │   ├── Actionability_AllData_Tsv_v11_GRCh38.tar
│   │   ├── Actionability_AllData_v11_GRCh38.tsv
│   │   └── README_Actionability_AllData_v11_GRCh38.pdf
│   ├── Autenticacion_Cosmic.txt
│   ├── cancer_mutation_census
│   │   ├── brd4_CancerMutationCensus_AllData_v100_GRCh37.tsv
│   │   ├── CancerMutationCensus_AllData_Tsv_v100_GRCh37.tar
│   │   ├── CancerMutationCensus_AllData_v100_GRCh37.tsv
│   │   ├── chek1_CancerMutationCensus_AllData_v100_GRCh37.tsv
│   │   ├── prueba
│   │   │   └── chek1_variants_filtered.tsv
│   │   ├── README_CancerMutationCensus_AllData_v100_GRCh37.txt
│   │   └── README.md
│   ├── Cosmic_MutantCensus_Tsv_v100_GRCh38.tar
│   ├── Cosmic_MutantCensus_v100_GRCh38.tsv
│   └── README_Cosmic_MutantCensus_v100_GRCh38.txt
├── ICGC
│   ├── ALL-US
│   │   └── simple_somatic_mutation.open.ALL-US.tsv
│   ├── brd4_script.sh
│   ├── brd4_variants.vcf
│   ├── chek1_script.sh
│   ├── chek1_variants.tsv
│   ├── chek1_variants.vcf
│   ├── filtered_chek1.vcf
│   ├── filtered_chr11.vcf
│   ├── header.vcf
│   ├── README.txt
│   ├── simple_somatic_mutation.aggregated.vcf.gz
│   └── vcf2tsv.py
├── README.md
├── TCGA
│   ├── frequent-mutations.2024-05-26_BRD4.tsv
│   ├── frequent-mutations.2024-05-26_CHEK1.tsv
│   └── README.txt
└── tree_structure.txt
`

# Requisitos

- Python 3
- Bibliotecas: pandas

## Instrucciones para la obtención de variantes

1. Descarga de ficheros:
    Para este primer paso he utilizado las siguientes bases de datos:
	- Cosmic: obtengo los datos de la release v100, actualizada a 21 de Mayo de 2024. (https://cancer.sanger.ac.uk/cosmic/download/cancer-mutation-census/v100/alldata-cmc)
	  Descargo del v100 Cancer Mutation Census el archivo CancerMutationCensus_AllData_Tsv_v100_GRCh37.tar correspondiente al GRCH37. En el encontramos los siguientes ficheros:
		- CancerMutationCensus_AllData_v100_GRCh37.tsv
		- README_CancerMutationCensus_AllData_v100_GRCh37.txt
	- ICGC: obtengo los datos correspondientes a la release 28, ya que es la última y más actualizada. (https://dcc.icgc.org/releases/release_28/Summary) 
	  En ella utilizo el siguiente fichero de la carpeta summary:
		- simple_somatic_mutation.aggregated.vcf.gz
		En dicho archivo se encuentra la información de las mutacciones somáticas.
	- TCAG: Aqui utilizamos los datos correspodientes al programa TCGA dentro del GDC. Ahi descargamos directamente los ficheros filtrados para nuestros dos genes de interés (https://portal.gdc.cancer.gov/analysis_page?app=CohortBuilder&tab=general):
		- frequent-mutations.2024-05-26_BRD4.tsv (https://portal.gdc.cancer.gov/genes/ENSG00000149554)
		- frequent-mutations.2024-05-26_CHEK1.tsv (https://portal.gdc.cancer.gov/genes/ENSG00000141867)
		
 
2. Filtrar los archivos generales de Cosmic e ICGC:
	- ICGC: Realizamos un script para filtrar las variantes de CHEC1 y BRD4
		- bcftools view -i 'INFO/CONSEQUENCE ~ "BRD4"' simple_somatic_mutation.aggregated.vcf.gz > brd4_variants.vcf
	- COSMIC: Realizamos el siguiente comando para filtrar para CHEK1 y BRD4
		- grep -E '^CHEK1|GENE_NAME' CancerMutationCensus_AllData_v100_GRCh37.tsv > prueba/chek1_variants_filtered.tsv
