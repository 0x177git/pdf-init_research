# o  que sao flaw nativas | hibridas

bem, antes de publicar o restante quero explicar o que eh uma flaw *Nativa*. quis expressar dessa forma pra classificar as flaws ou os conceitos de *_exploiting_* que nascem original e exclusivamente da prova de conceito do pdf, ou seja, pra exploitar essas flaws basta usar mecanismos jah presentes nos pdf, sem necessidades de uma _supplychains_ de flaws externas 

Mais a frente vou tentar explicar o que chamarei de *_flaws hibridas_*, que consistem em flaws jah existentes no sistema que suporta ou no _reader_ que renderiza, alem de outros tipos, nas quais usaremos o arquivo pdf com carga util e exploitar elas ou carregar payloads como meio de infiltraçao ou ataque pra atingir o _target_ 

    pra entender um pouco das flaws **hibridas** eh um pouco mais facil se jah entendermos alguns mecanismos conceituais como:

>
>  entender o mecanismo de heap spraying (*class of stack overflow*)(*estouro do heap*, geralmente utilizando _javascript_ em documentos pdf (o conceito eh bem amplo, porem to me referindo apenas a ideia de flaws conhecidas em espaços que carregam tipos de arquivos pdf)) resumindo, ao estourar o *_heap_* (*memoria alocada dinamicamente*), se repetem codes(nop_slep) ateh encontrar uma parte do *heap* que aponte a outros espaços de memoria ou funçoes fora da alocada e se escrevem bytes arbitrarios (geralmente shellcode ou bypass ( ASLR | nops leds ) pra outras funçoes com acceso privilegiado) |

>
> tmb eh interessante a injeçao de code arbitrario, ou seja, quando um proccesso permite ao usuario ou ao arquivo escrever dentro processo ou thread, se houver vulnerabilidade de injeçao, o usuario pode carregar sequencias arbitrarias de vetores pra injetar code que ative algum recurso ou explore alguma vulnerabilidade local |  caso nao conheça, recomendo dar uma olhada, se acha bastante conteudo e eh soh pra entender o conceito, nao eh nda mto avançado 
