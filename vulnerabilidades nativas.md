## Observando os detalhes silenciosos (_flaws nativas_)

vamos conhecer alguns elementos que podem ser invocados (do tipo `dictionary`) dentro do corpo de um pdf, geralmente invocados em *_objects_* (ou suas subespécies) tmb podendo ser combinados e possuem uma relaçao simbiotica pra ofuscar ou burlar sua açao, tmb podem ser embebados e comprimidos dentro de blocos de bytes (_streams_ ou _objects-streams_)


### /OpenAction

executa uma funçao quando o pdf for aberto, geralmente libreoffice carrega esse elemento (`dictionary`) pra carregar sempre a primeira pagina ao ser aberto (geralmente servidores de e-mails bem configurados ou com diretrizes estritas bloqueiam este elemento-açao)

### /URI
invoca uma _url_, combinada com outro tipo de elementos permite abrir paginas sem o conhecimento do usuario, baixar arquivos em segundo plano,enfim, cria uma açao relacionada a abertura, chamada, embebeda ou carrega pagina quando declarada e invocada

### /LaunchAction
invoca um parâmetro pra execuçao de comandos ou abertura de recursos do *OS*, pode ser adaptado pra diversos sistemas operacionais segundo a documentaçao original

### /GoToEmbebbed
invoca blocos de streams ou bodys incorporados dentro de outros bodys, pode chamar arquivos pdf encriptados dentro de streams e escondidos pra evitar a deteçao automatica durante a analises estatica 


tmb existem alguns filters e dictionarys que possuim flaws amplamente exploitados, aqui eu coletei apenas alguns dos elementos internos que mais sao usados e bem documentados pela comunidade, porem na documentaçao vc pode se aprofundar neste elementos e seus mecanismo que como falei, geralmente sao integrados (ou seja, combinados com outros metodos de ofuscaçao) pra dificultar a deteçao automatica
