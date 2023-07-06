bem, como jah comentei, o *body* eh composto de _objects_ e como explicar os object??

> "eles são os blocos de construção sobre os quais o PDF se sustenta”
 
os objects sao elementos com os quais podemos criar as propiedades do pdf, alem de escrever o conteudo (logical), existem 8 tipos de **dados elementais** do tipo _object_

`Boolean, integer, real, name, string, array, dictionary, and stream`


cada um deles compartilha suas funçoes e propiedades, mas aqui vamos focar especificamente em dois tipos bem especiais


# dictionary

### geralmente eh o tipo de object que mais vamos ver presentes numa analise do corpo de um pdf, este tipo de elemento consiste em uma chave e seu valor (os quais podem invocar classes internas e tratamentos de multiplos outros tipos),
pode ser recursivo e invocar outros tipos de dados, vou explicar melhor

![structure](https://didierstevens.files.wordpress.com/2008/04/pdf-physical.png)

aqui vemos o corpo esturutural e o conteudo de um pdf o qual mostra uma pagina em branco com um hello world (criado como com um tipo `stream`) em helvetica e contendo 1 pagina

se vc observou com calma esse code, conseguiu enxergar tdo o que temos comentado ateh agr; a parte em azul eh o body e nele existem essas chaves definindo o
conteudo do corpo, isso sao o tipo de elemento `dictionary`, um _dado associativo_ com uma chave invocada por `/xxxx` e sua funçao ou tipo de dado, lembrando que sempre precissamos implementar <sub><<</sub> ao começo

outra parte interessante eh observar o numero dos objects ( 1..n) ao lado do seu <sub>id</sub> e observar que tdo _object_ eh declarado por `obj` e terminado por `endobj`
pra aqueles que jah desenvolveram code em c ou python podem encontrar uma sintaxe bem familiar

existem multiplos tipos de definiçoes de chave dentro do `dictionary`, alem de ser recursivo, ou seja pode definir outros `dictionary` dentro de si


# streams
### esse tipo de elemento eh o mais expressivo dentro de um arquivo, podem ser dados comprimidos, imagens, graficos e ateh outros _objects_ dentro de si
fazem que o pdf seja mais potente e pequeno
elementos streams permite carregar *_javascript_* dentro do pdf, alem de carregar grandes dados dentro de si, que podem ser representados de varias maneiras
por poderem ser codificadas e comprimidas de diferentes tipos de codigos arbitrarios **_(xml, js, imagens, anotaçoes especiais)_** outros objects e elementos especiais invisiveis ao usuario final 
bem, nem precisso explicar o potencial disso neh

# stream (filters)
### certo, jah sabemos que stream podem ser blocos de bytes de grande tamanho, entao a pergunta que fica eh, como comprimimos e descomprimimos _(lembrando que essa compressao em multiplos casos eh definida como uma interpretaçao, ou seja, digamos que queremos "comprimir" um tipo de fonte ou imagem de grande tamanho, o mecanismo em si pode ser chamado de codificaçao ou interpretaçao, mas vamos usar compressao pq geralmente eh o termo que mais veremos na documentaçao original)_ 
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


vejamos agr uma revisao grafica que pode acabar esclarecendo este assunto

![layout of pdf](https://gendignoux.com/blog/images/pdf-basics/simplefile-by-nc-sa.png)

aqui podemos ver o corpo de um pdf e dentro podemos observar alguns `dictionary` dentro de **_object_** que constituem o **body** (verde)

![stream body](https://gendignoux.com/blog/images/pdf-basics/objstm-by-nc-sa.png)

e aqui temo um **body** constituido de **_object_** do tipo _stream_, no qual temos uma invocaçao dos atributos que existem em _stream_ (do tipo `dictionary`), assim como seus filters pra comprimirmos e identificar o bloco de bytes dentro do _stream_ (pode ser um codigo javascript, um grafico, um xml ou qlqr outro dos tipos que sao admitidos)

neste caso podemos conter **objects** dentro de um _stream_)


