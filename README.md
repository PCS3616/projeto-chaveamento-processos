# Chaveamento de Processos

## Introdução

Este projeto é uma introdução a Sistemas Operacionais, disciplina que é
continuação direta de Sistemas de Programação. Para entender
corretamente o que deve ser feito deve-se entender o que são processos
em um computador e qual sua dinâmica num sistema real, tudo isso será
aprofundado na disciplina "Sistemas Operacionais" em algum momento no
futuro.

## Base de Sistemas Operacionais

Um processo é uma rotina a ser executada pelo processador que tem posição
local constante e algum tempo de vida. Num sistema real os processos são
identificados por números únicos chamados PID (_process id_). Existem
inúmeras funções que um processo pode executar, desde as operações
lógico-aritméticas convencionais até a criação e destruição de outros
processos, além, é claro do uso de dispositivos externos (tela, teclado,
mouse, portas USB, etc.). Para garantir um bom funcionamento de um
sistema operacional, é definida na memória uma **área reservada**, onde
existem as instruções internas do sistema operacional para lidar as
diversas utilidades diferentes que este tem. A única forma de se entrar
na área reservada é através de uma **interrupção** (deixamos este conceito
para Sistemas Operacionais explicar pra vocês, mas a ideia geral é
intuitiva), sendo impossível um processo de fora da área reservada
chamar um processo de dentro dela.

Em um computador há um número bastante grande de processos ativos
simultaneamente (você pode ver os processos e suas características
usando o comando `top` na sua máquina virtual). Para gerenciar todos
estes processos, já que apenas um processo pode rodar em cada núcleo do
processador por vez, os sistemas operacionais implementam o chaveamento
de processos: existe uma fila de processos que devem ser executados pela
máquina e, a cada intervalo de tempo pré-definido, o processo que está
rodando é parado e um processo da fila é importado para o processador
para poder rodar. Esta pausa do processo, em um processador real, é
chamada de **interrupção de tempo** ou _time interruption_ e, quando ela
ocorre, a execução é redirecionada para um endereço específico dentro da
área reservada que fará a troca dos processos de forma coerente. O
código dentro da área reservada que realiza este procedimento é chamado
de **gerenciador de interrupções** ou _interruption handler_.

## O que será fornecido

Para a execução do trabalho, é necessário que hajam as interrupções de
tempo invocando as rotinas restritas da área reservada. Por isso é
fornecido na própria MVN uma opção de funcionamento que contém uma área
reservada para as rotinas do sistema operacional (entre `0x00` e `0xFF`)
que segue todas as regras de acesso colocadas acima e a implementação de
uma interrupção de tempo que, a cada `NUM` ciclos completos de execução,
faz uma chamada de sub-rotina para o endereço `0x0000`, onde deve estar a
rotina de chaveamento de processos. Para ativar esta funcionalidade,
basta invocar o monitor da MVN com a flag `-i` seguida do tempo de
interrupção, que, por padrão, é de 50 ciclos.

## O trabalho

O trabalho consiste numa simplificação desta dinâmica de chaveamento,
existirão apenas dois processos (chamados rotina 1 e rotina 2), além do
Gerenciador de Interrupções.

Devem ser codificados o gerenciador de interrupções, que faz a troca
entre os dois processos:

1. A rotina 1, que gera números pseudo-aleatórios mod($n_1$) e salva
    eles; e
2. A rotina 2, que que determina se os números gerados pela rotina 1
    são divisíveis por $n_2$ e escreve na tela "SIM!" caso sejam e "NAO!"
    caso contrário, apagando cada um deles a cada conferência.

O trabalho pode ser feito em duplas ou individualmente. Caso a opção
seja por fazer individual, $n_1$ = 5 primeiros dígitos do NUSP e $n_2$ =
3 últimos. Caso seja em dupla, $n_1$ = 5 primeiros dígitos do NUSP de um
aluno e $n_2$ = 3 últimos do outro.

### Desafio

Se tudo funcionar até aqui, existe ainda um desafio: existe a
possibilidade de ocorrer a interrupção de tempo entre as escritas das
duas partes das mensagem na rotina 2 ("SI" e "M!" ou "NA" e
"O!"), encontre uma forma de resolver esse problema.

## Perguntas

Seguem algumas perguntas a serem respondidas depois da codificação:

1.  Os algoritmos propostos podem ser mais eficientes? Como? Se sim, por
    que a forma menos eficiente foi escolhida?

2.  Os códigos escritos podem ser mais eficientes? Como? Se sim, por que
    a forma menos eficiente foi escolhida?

3.  Qual foi a maior dificuldade em implementar as 3 rotinas?

4.  O que teria que ser mudado no seu código para comportar 3 rotinas
    sendo chaveadas (além da implementação da terceira rotina)? E para
    um número arbitrário $k$? Neste último caso, quais seriam as restrições
    de $k$?

5.  O que aconteceria com a execução do seu código se NUM for alterado?
    Qual valor mínimo que ele pode assumir? E o máximo?

## Entrega

Quatro ou mais arquivos devem ser entregues:

1.  **Gerenciador de Interrupções:** um arquivo em ASM chamado
    `interruption_handler.asm` contendo a rotina que chaveia entre dois
    processos.

2.  **Rotina 1:** um arquivo ASM chamado `rotina_1.asm` contendo o arquivo
    para a rotina 1 como descrita acima.

3.  **Rotina 2:** um arquivo ASM chamado `rotina_2.asm` contendo o arquivo
    para a rotina 2 como descrita acima.

4.  **Relatório:** um arquivo chamado `relatorio_<NUSP1>_<NUSP2>.pdf` para
    trabalhos realizados em dupla, ou `relatorio_<NUSP>.pdf` para individuais
    contendo uma descrição resumida do problema e dos conceitos e uma descrição
    detalhada das etapas de resolução, da estratégia utilizada em cada módulo
    e outras informações que julgarem úteis. O relatório pode conter imagens
    das execuções de teste;

5.  Outros arquivos que julgar necessário.
