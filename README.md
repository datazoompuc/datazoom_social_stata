
For the English version, click on the Shield below:
<!-- badges: start -->
[![en](https://img.shields.io/badge/lang-en-red.svg)](https://github.com/datazoompuc/datazoom_social_Stata/blob/main/README_en.md)
<!-- badges: end -->

<a href="https://github.com/datazoompuc/datazoom_social_Stata"><img src="https://raw.githubusercontent.com/datazoompuc/datazoom_social_stata/master/logo.jpg" align="left" width="100" hspace="10" vspace="6"></a>

<!-- README.md is generated from README.Rmd. Please edit that file -->

# datazoom_social

<!-- badges: start -->

![Languages](https://img.shields.io/github/languages/count/datazoompuc/datazoom_social_Stata?style=flat)
![Commits](https://img.shields.io/github/commit-activity/y/datazoompuc/datazoom_social_Stata?style=flat)
![Open
Issues](https://img.shields.io/github/issues-raw/datazoompuc/datazoom_social_Stata?style=flat)
![Closed
Issues](https://img.shields.io/github/issues-closed-raw/datazoompuc/datazoom_social_Stata?style=flat)
![Files](https://img.shields.io/github/directory-file-count/datazoompuc/datazoom_social_Stata?style=flat)
![Followers](https://img.shields.io/github/followers/datazoompuc?style=flat)
<!-- badges: end -->

O `datazoom_social` é um pacote que compatibiliza microdados de
pesquisas realizadas pelo IBGE. Com o pacote, é possível fazer a leitura
de todas as pesquisas domiciliares realizadas pelo IBGE, compatibilizar
os Censos Demográficos de diferentes anos, gerar identificação dos
indivíduos da PNAD Contínua, e muito mais.

## Instalação <a name="instalacao"></a>

Digite o código abaixo na linha de comando do Stata para baixar e
instalar a versão mais recente do pacote

    net install datazoom_social, from("https://raw.githubusercontent.com/datazoompuc/datazoom_social_stata/master/") force

## Uso

Todas as funções do pacote podem ser usadas pelas caixas de diálogo.
Para acessá-las, digite o comando

    db datazoom_social

Clique nos botões abaixo para ir à helpfile de cada função. Elas são
melhor visualizadas abrindo no Stata, com `help função`.

|                                                                                                                                                                                            |                                                                                                                                                                  |                                                                                                                                                                |                                                                                                                                                                       |
|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:----------------------------------------------------------------------------------------------------------------------------------------------------------------:|:--------------------------------------------------------------------------------------------------------------------------------------------------------------:|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| <a href = "Censo/datazoom_censo.sthlp"> <kbd> <br>    <font size = 3> Censo </font>    <br><br> </kbd> </a> <br> <br> <small> Censo Demográfico </small> <br> <small> 1970 a 2010 </small> | \[<kbd> <br>    <font size = 3> ECINF </font>    <br><br> </kbd>\]\[ecinf\] <br><br> <small> Economia Informal Urbana </small> <br> <small> 1997 e 2003 </small> | \[<kbd> <br>    <font size = 3> PME </font>    <br><br> </kbd>\]\[pme\] <br><br> <small> Pesquisa Mensal de Emprego </small> <br> <small> 1990 a 2015 </small> |           \[<kbd> <br>    <font size = 3> PNAD </font>    <br><br> </kbd>\]\[pnad\] <br><br> <small> PNAD Antiga </small> <br> <small> 2001 a 2015 </small>           |
|        <a href = "#pnad-contínua"> <kbd> <br> <font size = 3> PNAD Contínua </font> <br><br> </kbd> </a> <br><br> <small> PNAD Contínua </small> <br> <small> 2012 a 2023 </small>         | <a href = "#pnad-covid"> <kbd> <br>   <font size = 3> PNAD Covid </font>   <br><br> </kbd> </a> <br><br> <small> PNAD Covid </small> <br> <small> 2020 </small>  | \[<kbd> <br>    <font size = 3> PNS </font>    <br><br> </kbd>\]\[pns\] <br><br> <small> Pesquisa Nacional de Saúde </small> <br> <small> 2013 e 2019 </small> | \[<kbd> <br>    <font size = 3> POF </font>    <br><br> </kbd>\]\[pof\] <br><br> <small> Pesquisa de Orçamentos Familiares </small> <br> <small> 1995 a 2018 </small> |

<a href = "#créditos">![Static
Badge](https://img.shields.io/badge/Cr%C3%A9ditos%20-%20Departamento%20de%20Economia%20PUC%20Rio%20-%20blue)
</a> <a href = "#créditos"> ![Static
Badge](https://img.shields.io/badge/Cita%C3%A7%C3%A3o%20-%20green) </a>

## Programas Auxiliares (Dicionários)

A maioria dos programas do pacote lidam com dados originais armazenados
em formato *.txt*, que precisam de dicionários – formato *.dct* no Stata
– para serem lidos. A consequência é um volume de dicionários que supera
o limite de 100 arquivos permitido para um pacote do Stata poder ser
instalado. Por isso, os dicionários individuais são compactados em um
arquivo *.dta* único, que é lido dentro de cada programa. Ambas as
funções usadas para isso são definidas no arquivo *read_compdct.ado*.

O primeiro programa definido nesse arquivo é `write_compdct`, que pode
ser usado como a seguir: após rodar o arquivo *.ado* para definir a
função, basta usar o código

    write_compdct, folder("/pasta com os dicionários") saving("/caminho/dict.dta")

A função então lê todos os arquivos *.dct* presentes na pasta, e junta
todos no arquivo *dict.dta*, com cada dicionário identificado por uma
variável com seu nome.

Para transformar esse arquivo compactado novamente do dicionário
original, se usa o programa `read_compdct`:

    read_compdct, compdct("dict.dta") dict_name("dic original") out("dic extraído.dct")

que extrai o *“dic original”* do arquivo *dict.dta* e o salva para *“dic
extraído.dct”*. Como exemplo, veja o uso dessa função no programa
`datazoom_pnadcontinua`

    tempfile dic // Arquivo temporário onde o .dct extraído será salvo

    findfile dict.dta // Acha o arquivo dict.dta salvo pela instalação
                      // do pacote na pasta /ado/, e armazena o caminho
                      // até ele na macro r(fn)

    read_compdct, compdct("`r(fn)'") dict_name("pnadcontinua`lang'") out("`dic'")
      // Lê o dicionário compactado dict.dta, extrai o dicionário pnadcontinua
      // (ou pnadcontinua_en, `lang` é vazio ou "_en"), e salva o arquivo final
      // na tempfile dic, que é usada para ler os dados

Para a nossa organização interna, cada pasta correspondente a um
programa armazena os dicionários na sub-pasta */dct/*. Todos esses
dicionários são também armazenados juntos na pasta */dct/* diretamente,
que é usada para gerar o *dict.dta* através do `write_compdct`. Note que
nenhum arquivo *.dct* é efetivamente listado no arquivo
*datazoom_social.pkg*, e portanto, não são instalados no computador do
usuário. Apenas o arquivo *dict.dta* é enviado.

Periodicamente, na manutenção do pacote, temos que rodar a do-file
`atualizacao_dict.do` e seguir as instruções.

## Créditos

O [Data Zoom](https://www.econ.puc-rio.br/datazoom/) é desenvolvido por
uma equipe do Departamento de Economia da Pontifícia Universidade
Católica do Rio de Janeiro (PUC-Rio)

Para citar o pacote `datazoom_social`, utilize:

> Data Zoom (2023). Data Zoom: Simplifying Access To Brazilian
> Microdata.  
> <https://www.econ.puc-rio.br/datazoom/english/index.html>

Ou, em formato BibTeX:

    @Unpublished{DataZoom2023,
        author = {Data Zoom},
        title = {Data Zoom: Simplifying Access To Brazilian Microdata},
        url = {https://www.econ.puc-rio.br/datazoom/english/index.html},
        year = {2023},
    }
