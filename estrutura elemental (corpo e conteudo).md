pdfs são arquivos _polimorficos_, ou seja, constituidos de duas esturuturas primarias (physical and logical)
alterar a estutura física, não implica em alterar necessariamente a estutura logica, ou seja, aqueles sub-contidos do pdf

a estrutura física é composta por quatro elementos fundamentais e a lógica eh o conteúdo dentro destas estruturas (e suas conveçoes), logo alterando a ordem entre sub objetos do body (parte fisica) e ou conteudo alterara apenas o referente a estes e seus relativos, nao necessariamente outras partes do pdf, se nao for comprometido durante o processamento (trailer, cross-reference)

# header 
 eh a definiçao do arquivo (pdf) e a versao, alem de poder conter metadados, informaçoes do usuario criador e outras informaçoes referentes as tags do arquivo
 > %pdf1.x

# body
eh a parte mais interessante em si, ja que consiste em grupos de objects (estes objects sao conjuntos de dados que criam os atributos, definem as propiedades e o tipo de dados, podendo ter multiplos tipos de dados, sendo uma das partes mais diversas do pdf e que constituem o corpo bruto) que constroem o conteudo bruto do arquivos, eses object podem ser representados como **arvores binarias**
eles seguem uma ordem inicial de (1..n), nao sendo necessariamente sequencial

<sub>vamos ver mais sobre lah na frente</sub>

# cross-reference

eh composto por um tipo de tabela que aponta a cada inicio de object, assim como seu identificador (1..n), seu tamanho e eh apontado por seçoes de bytes (offset)


# Trailer
aponta a cross-reference, inclui o tamanho elementos, indica o cross-reference por espaço de byte e o ponto inicial do body (1..n %1)pu seja o primeiro object do body
cabe dizer que este espaço eh o primeiro a ser posto na fila de processamento, ou seja, a ser processado durante processamento por isso contem os **apontadores fundamentais** do arquivo, que eh a **_cross-reference_**, onde podemos localizar os objects por espaços de bytes(offset) ou seja
> xref
> 
> 0 8
>
>                         _ (quantidade de segmentos no arquivo) ou objects dentro do body, sempre vai ter **n+1** (com n sendo a qntidade total), ja que referencia o header com 1 _
> 
> offset   id
> 
> 0x000   65535  f
> 
>                            _o primeiro na tabela costuma representar o header, por isso possui  esse id absurdo, em conjunto com o f nao corre risco de incidir com uma id real_
>
> 0x0xx   0000  n
>
> ….
>

esses dois ultimos elementos são importantissimos pra o processamento do pdf, já que definem uma sintaxe rapida de localizaçao do conteudo (*body*)
jah que o _trailer_, aponta a quantidades de objects esperada pelo reader (independente do sistema ou arquitetura) e a tabela de referencia (apartir daqui chamarei de *xref*) que permite acceder o conteudo do pdf como parte do pre-processamento, ou seja, o reader jah sabe qual eh o primeiro item a acceder e seus relativos
alem disso tmb permite que o arquivo seja processado sem alocar imensos espaços de memoria com o arquivo inteiro nela, apenas processando o pedaço de **body** contido nos _objects_ necessarios (lembrando que uma pagina pode conter diversos _objects_ de diversos tipos de dados entrelaçados pelo _xref_), lendo esse pedaço de bytes (pra saber mais sobre a natureza e comportamento dos objects, veja objects dentro do repositorio)

`carregando apenas os `_objects_ `necessarios pra a posiçao do usuario no pdf, mais` _objects_ `podem ser carregados pela interaçao do usuario guiando-se do` *xref* `(atraves dos offset escritos)
eh interessante lembrar que se um` _object_` possui` _sub-objects_` dentro de si, seu` <sub>id</sub> `vai ser a quantidade dentro dele`

>0x000 65535 f (header)
>
>0x000 0000   n
>
>0x000 0000  n
>
>0x000 0000  n
>
>0x888 0000 n
>
>4 8
>
>..

ou seja, o object 4 possui 8 subobject e ele se inicia dentro do `offset 0x888`

embrando tmb que existem formas mais complexas de encontrar as xref, por exemplo a partir do pdf1.7 que implementou as **_incrementally ups_**, os *xref* possuim subtipos chamado **_xref streams_**, essa implementaçao permitiu pdf com mais de 10 gb e que tivesse diversas formas de _objects_ 

alem dos _linearized_ (este exemplo)

outra curiosidade bem interessante, eh que caso o pdf esteja encriptado ele iria conter dois elementos dentro

>
> `/encrypt`  atribui o tipo de diccionario pra o processo de cifra

> `/info`  contem o offset que aloca o diccionario
>

porem, isto eh soh pra criar uma ideia fudamental do corpo estutural e seus comportamento; tem bastante conteudo por ai caso algm se interesse em dev estes tipos de arquivos, porem espero que tenha ficado claro as bases que permitem que esse tipo de arquivo seja tao flexivel e tao amplamente utilizado, basta um leitor minimamente bem trabalhado (ou nem tanto) que sua implementaçao sera bem incorporada por qlqr sistema e arch

caso tenha interesse em saber mais, recomendo a leitura da documentaçao do *pymudf* , uma api bem interessante e com bastante docs, alem da _iso 32000_
tem tmb disponivel uma documentaçao aqui 

([https://www.oreilly.com/library/view/developing-with-pdf/9781449327903/ch01.html])

e uma explicaçao do que jah vimos aqui, que foi uma das principais referencias

([https://blog.didierstevens.com/2008/04/09/quickpost-about-the-physical-and-logical-structure-of-pdf-files/])

