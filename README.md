## pdf-init_research
# pesquisa sobre pdf e seus vetores de ataque 
esta eh uma pesquisa que fiz ha alguns meses atras sobre pdf, tdo começo como anotaçoes de estudo mas acabou virando uma serie de post no *@rubyofsec*, motivado por ver diversas pessoas implementando ou falando de exploits em arquivos pdf,
mas que infelizmente acabavam sabendo apenas como implementar o exploit, mas nao entendiam a fundo sua natureza ou comportamento e ainda pior, nao conheciam as falhas criticas existentes em diversos arquivos do dia-a-dia

acabei tentando organizar estas notas em varios posts que ficaram um pouco desorganizados, mas tentei organizar e esclarecer algumas questoes um pouco mais abstratas, estea pesquisa tmb pode ser orientada pra dev que desejam trabalhar com 
framework ou tools que implementam alguma API orientada a pdfs

espero ajudar a aqueles que desejam conhecer um pouco mais sobre este assunto, ou que trabalham de alguma forma com algum campo nos quais a segurança voltada pra estes arquivos eh essencial, tdo surgiu como notas de uma pesquisa pessoal, jah que eu msm acabei utilizando exploits existentes 
em pdf, mas sempre fiquei desconfortavel com a falta de compilaçao de informaçao e traduçao no portugues, tdo eh publicado com licença GNU, compartilhem, copiem, modifiquem e se houver algum erro ou alguma questao que desejam melhorar, peço que
me avisem, assim que tiver um tempinho irei corrigir, ou se quisser fazer um repositorio pra melhorar, tmb seria interessante, enfim, sempre eh bom conhecer o objeto de estudo pra entendermos pq existem determinados protocolos de segurança
ou ateh mesmo como surgiram suas falham 

sempre que usarem porfavor referenciem meu canal do telegram *@rubyofsec*, se puderem, uma estrelinha seria dms <3
espero que gostem e mais do que isso, espero que entendam e expandam o que jah sabem

## estrutura elemental (corpo e conteudo)

a principal estrutura abstrata do pdf, contendo quatro segmento, o _header_ (metadados, versao do pdf), o _body_ (estrutura de dados que compoem o conteudo), a tabela de referencias (implementa o indice de itens (_objects & sub-objects_) e funciona como estrutura de lista a qual o processador ira renderizar e mapear, alem de possuir atualizaçoes que permitem indexar multiplos _body_ e carregar grandes blocos de *_streams_*) e o _trailer_ (indica o inicio do pdf desde o header e um resumo dos itens no _cross-reference_) 

## Body (objects_type)
aprofundo na estrutura e os elemento que constroem o conteudo do pdf, alem dos tipos de dados e sua dinamica durante processamento e analise estatico

## vetores de ataque
explico algumas bases conceituais pra entender o contreudo sobre as vulnerabilidades existentes e divido elas em dois tipos

*nativas* : estas sao aquelas que existem em algumas versoes antigas do pdf e permitem exfiltrar dados e carregar payloads, assim como algumas que ainda existem ainda e permitem ofuscar vulnerabilidades hibridasa pra explorar vulnerablidades no reader ou *OS*, porem nao precisam de supplychains pra serem carregadas

*hibridas*: vulnerabilidades que existem nos readers, *OS*, ou qlqr outro elemento externo ao corpo do pdf, porem podem ser carregadas pelo pdf de forma externa pra elevar privilegios e exfiltrar dados
