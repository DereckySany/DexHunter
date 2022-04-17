# DexHunter

## AVISO
O recurso de string é muito importante. Pode ser alterado com a evolução dos [serviços de hardening](https://g.co/kgs/51oYgZ). Se estiver incorreto, o processo de descompactação não pode ser acionado. DexHunter aproveita "[fwrite](http://wiki.icmc.usp.br/images/8/82/Manipulacao_arquivos.pdf)" e outras funções libc para manipular arquivos. Mas essas funções são fisgadas por serviços de proteção, resultando na falha do processo. Como resultado, você não pode utilizar a imagem fornecida para descompactar os serviços de proteção mais recentes. É melhor substituir essas funções pelas chamadas diretas do sistema para evitar falhas.

DexHunter visa descompactar o arquivo dex automaticamente.

O DexHunter é baseado no código-fonte do tempo de execução do Android. É composto de tempo de execução ART e DVM modificado. Você pode usar o runtime modificado para substituir o conteúdo original nos códigos-fonte do Android (Android 4.4.3). A modificação é principalmente em `"art/runtime/class_linker.cc"` (ART) e `"dalvik/vm/native/dalvik_system_DexFile.cpp"` (DVM).

## Uso:

Se você deseja descompactar um aplicativo, você precisa enviar o arquivo `"dexname"` para `"/data/"` no celular antes de iniciar o aplicativo. A primeira linha em `"dexname"` é a string de recurso (referindo-se a `"slide.pptx"`). A segunda linha é o caminho de dados do aplicativo de destino (por exemplo, `"/data/data/com.test.test/"`). Seu final de linha deve estar no estilo `Unix/Linux`. Você pode observar o log usando `"logcat"` para determinar se o procedimento de descompactação foi concluído. Uma vez feito, o arquivo `"whole.dex"` gerado é o resultado desejado que está localizado no diretório de dados do aplicativo.

## Dicas:

1) DexHunter simplesmente reutiliza o conteúdo antes da seção `"class_def"` em vez de analisá-los para mais eficiência. Se houver alguns problemas, você pode analisá-los e montá-los novamente ou alterá-los estaticamente.

2) Vale ressaltar que alguns campos `"annotation_off"` ou `"debug_info_off"` podem ser inválidos no resultado. Esses arquivos não têm nada a ver com execução apenas para dificultar a descompilação. Não lidamos especificamente com essa situação no momento. Você pode simplesmente programar alguns scripts para definir os campos inválidos com 0x00000000.

3) Como se sabe, alguns serviços de proteção podem proteger vários métodos no arquivo dex, restaurando as instruções antes de serem executadas e limpando-as logo após o término. Portanto, você também precisa modificar a função `"DoInvoke"` (ART) ou `"dvmMterp_invokeMethod"` (DVM) para extrair a instrução protegida durante a execução.

4) O recursos de string pode ser alterada com a evolução dos [serviços de hardening](https://g.co/kgs/51oYgZ).

5) Se o `"fwrite"` e outras funções da libc falharem, talvez essas funções estejam conectadas por serviços de proteção. Como resultado, você não pode despejar a memória através deles. Você pode ignorar essa limitação chamando diretamente as chamadas de sistema relevantes.

DexHunter tem sua própria limitação. À medida que os serviços de proteção se desenvolvem, o DexHunter pode não ser eficaz no futuro. Se estiver interessado, você pode alterar o DexHunter para acompanhar os serviços de proteção continuamente.

## Descrição do arquivos:

> "slide.pptx" é o material de apresentação do [HITCON 2015](https://hitcon.org/2015/ENT/Activities-Enterprise-Agenda.html#zyq) que descreve o design e a implementação do DexHunter.

> "demo.mp4" é o vídeo de demonstração de descompactação de um aplicativo reforçado por @Ali.

> "test.apk" é o exemplo usado no vídeo.

> "dexname" é o arquivo de configuração usado no vídeo.

> O diretório "art" é o tempo de execução modificado para ART.

> O diretório "dalvik" é o tempo de execução modificado para DVM.

> Os arquivos "imagem" 7z contêm os arquivos de imagem do sistema usados no vídeo.

> Se você tiver alguma dúvida, entre em contato comigo por e-mail para zyq8709@gmail.com._(Desatualizado??)_

### Se você usar este código, cite o artigo a seguir. Obrigado!
```
Yueqian Zhang, Xiapu Luo e Haoyang Yin, DexHunter: rumo à extração de código oculto de aplicativos Android compactados,
20º Simpósio Europeu sobre Pesquisa em Segurança de Computadores (ESORICS), Viena, Áustria, setembro de 2015.

@inprocedings{DexHunter15,
Title = {DexHunter: para extrair código oculto de aplicativos Android compactados},
Autor = {Yueqian Zhang e Xiapu Luo e Haoyang Yin},
Título do livro = {Proc. ESÓRICO},
Ano = {2015}}
```
## comentário: 

Eu testei as amostras de 360 em julho sob DVM. A string do recurso é alterada para `"/data/app/XXX.apk"` (referindo-se a `"silde.pptx"`). Esta string é muito importante. Se estiver incorreta, o processo de descompactação falhará.

### Isenção de responsabilidade
_Todo o contéudo acima mencionado é apenas uma tradução para melhor entedimento.Todas as fontes e citações permanecem originais_
