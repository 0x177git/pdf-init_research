bem, como jah comentei, o *body* eh composto de _objects_ e como explicar os object??

> "eles são os blocos de construção sobre os quais o PDF se sustenta”
 
os objects sao estruturas de elementos com os quais podemos criar as propiedades do pdf, alem de escrever o conteudo (logical), existem 8 tipos de **dados elementais** contido no tipo _object_

`Boolean, integer, real, name, string, array, dictionary, and stream`


cada um deles compartilha suas funçoes e propiedades, mas aqui vamos focar especificamente em dois tipos bem especiais


# dictionary

### geralmente eh o tipo de elemento dentro de um object que mais vamos ver presentes numa analise do corpo de um pdf, este tipo de elemento consiste em uma chave e seu valor (os quais podem invocar classes internas e tratamentos de multiplos outros tipos),
pode ser recursivo e invocar outros tipos de dados, vou explicar melhor

![structure](https://didierstevens.files.wordpress.com/2008/04/pdf-physical.png)

aqui vemos o corpo esturutural e o conteudo de um pdf o qual mostra uma pagina em branco com um hello world (criado como com um tipo `stream`) em helvetica e contendo 1 pagina

se vc observou com calma esse code, conseguiu enxergar tdo o que temos comentado ateh agr; a parte em azul eh o body e nele existem essas chaves definindo o
conteudo do corpo, isso sao o tipo de elemento `dictionary`, um _dado associativo_ com uma chave invocada por `/xxxx` e sua funçao ou tipo de dado, lembrando que sempre precissamos implementar <sub><<</sub> ao começo

outra parte interessante eh observar o numero dos objects ( 1..n) ao lado do seu <sub>id</sub> e observar que tdo _object_ eh declarado por `obj` e terminado por `endobj`
pra aqueles que jah desenvolveram code em c ou python podem encontrar uma sintaxe bem familiar

existem multiplos tipos de definiçoes de chave dentro do `dictionary`, alem de ser recursivo, ou seja pode definir outros `dictionary` dentro de si


# streams
### esse tipo de elemento eh o mais expressivo (ou seja, qndo vemos blocos de bytes que nao estao dentro das codificaçoes utf8 ou unicode, geralmente sao estes) dentro de um arquivo, podem ser dados comprimidos, imagens, graficos e ateh outros _objects_ dentro de si
fazem que o pdf seja mais potente e pequeno
elementos streams permite carregar *_javascript_* dentro do pdf, alem de carregar grandes dados dentro de si, que podem ser representados de varias maneiras
por poderem ser codificadas e comprimidas de diferentes tipos de codigos arbitrarios **_(xml, js, imagens, anotaçoes especiais)_** outros _objects_ e elementos especiais invisiveis ao usuario final 
bem, nem precisso explicar o potencial disso neh

# stream (filters)
### certo, jah sabemos que stream podem ser blocos de bytes de grande tamanho, entao a pergunta que fica eh, como comprimimos e descomprimimos _(lembrando que essa compressao em multiplos casos eh definida como uma interpretaçao, ou seja, digamos que queremos "comprimir" um tipo de fonte ou imagem de grande tamanho (ou seja, um pedaço de code ou um pedaço de bytes que representam uma imagem ou grafico), o mecanismo em si pode ser chamado de codificaçao ou interpretaçao, mas vamos usar compressao pq geralmente eh o termo que mais veremos na documentaçao original)_ 
e eh ai que entra a necessidade dos _filters_ (do tipo `dictionary`). 

entao este mecanismo podemos dizer que define as propiedades de compressao do **_stream_**, podendo conter code de outro tipo que nao seja os que vimos anteriormente

logo ha varias formas de chamar e definir os atributos de um tipo **_stream_**. existe um tipo de **object** chamado de _external object_, geralmente eh um tipo de **_stream_** grafico auto-contido, ou seja, ele eh definido dentro do **object** que invoca o elemento _stream_, porem fora do bloco de bytes stream, um exemplo facil eh qndo vc define alguns atributos antes de compilar alguma coisa com gcc, ou seja, um _external object_ eh uma diretiva pra invocar um elemento stream onde seu tipo e compressao eh atribuido dentro do propio **object** e no modulo _external-object_

mais a frente irei me aprofundar nesses modulos contidos, mas agr, vou explicar alguns _filters_ e seus tipos de compressao pra entendermos com claridade esse assunto lah na frente
alguns filters que geralmente vamos ver em diversos documentos

>
> **lateDecode**
>
> geralmente vamos ver pra comprimir imagens e fontes, uma propiedade interessante eh que esse filter utiliza o mesmo mecanismo de compressao que zip
>
> **DCTDecode/DPXDecode**
>
> geralmente veremos em compressao de imagens compativeis com jpeg/jfif


![um object onde definimos propiedades do pedaço de bytes (usamos `dictionarys type` pra isso) contido em um _stream_](https://github.com/exoForce01/pdf-init_research/blob/main/filter_example.jpeg?raw=true)

entao, como podemos ver, temos um object com o <sub>id</sub> *4* onde temos os argumentos pra criaçao de um bloco de bytes (identificaçao do _stream_) onde definimos o tipo de compressao, o tipo de _external-object_ e outras especificidades (tdo feito com elementos `dictionary`) pra dps carregarmos o conteudo de _stream_




### vejamos agr uma revisao grafica que pode acabar esclarecendo este assunto

![layout of pdf](https://gendignoux.com/blog/images/pdf-basics/simplefile-by-nc-sa.png)

aqui podemos ver o corpo de um pdf e dentro podemos observar alguns `dictionary` dentro de alguns **_objects_** que constituem o **body** (verde)

![stream body](https://gendignoux.com/blog/images/pdf-basics/objstm-by-nc-sa.png)

e aqui temo um **body** constituido de **_object_** do tipo _stream_, no qual temos uma invocaçao dos atributos que existem em _stream_ (do tipo `dictionary`), assim como seus filters pra comprimirmos e identificar o bloco de bytes dentro do _stream_ (pode ser um codigo javascript, um grafico, um xml ou qlqr outro dos tipos que sao admitidos)
(lembrando que filters podem cooperar dentro do stream pra comprimir de forma alterada ou simbioticamente)


neste caso podemos conter **objects** (sejam _external-object_ pra elementos graficos ou de outro tipo aceito pelo ISO, ou seja _indirect-objects_) dentro de um _stream_, como jah falei, mas logo surge a pergunta, pq algumas vezes temos **object** contidos em blocos de bytes do tipo _stream_??

>
> eu to mencionando isto pq se respirarmos um pouco e observamos com delicadeza, podemos ver que este tipo de stream parece uma oportunidade bastante conveniente pra carregar um code ou object o qual nao iriamos
>querer que seja mapeado de forma direta, sendo apenas carregado pra memoria durante o processamento de um pedaço especifico do documento, capicci?? ::mage_man: :pinched_fingers:



## indirect-object 

um detalhe importante eh que varias  vezes veremos um tipo de **object** chamado de _indirect-object_, eh facil se pensarmos nele como um simbolo ou um link de um **object** jah existente, o qual podemos querer utilizar varias vezes durante nosso corpo de pdf, isto eh uma propiedade do _polimorfismo_, como mencionei no primeiro post

> 'alguns atributos e objetos podem ser utilizados em objetos distintos, porém,com implementações lógicas diferentes.’
>

deixo aqui uma explicaçao que fiz no post original, mas caso vc jah conheça o polimorfismo, talvez nem seja mto necessario, porem soh pra fins de esclarecimento

digamos que temos o **object** *5* que define uma pagina ou eh uma folha de uma arvore de paginas (lembre-se que os **object** sao tidos como *arvores binarias*), entao esse outro **object** chama o **object** *4*, porem, lah na frente iremos precissar do **object** *4* qndo estivermos utilizando o **object** *8*, logo podemos definir o **object** *4* como um _indirect object_ e como invocamos o **object** *4* varias vezes ao longo do body??  
bem, tdos os _indirect object_ possuim um <sub>id</sub> unico, que pode ser invocado dentro de outros **objects** e _streams_ e eh alocado dentro do *xref* (lembre qndo comentei sobre subobject)

