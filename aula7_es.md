Sistemas Operacionais Básico

Gerenciamento de Entrada/Saída
Prof. José Roberto Bezerra

1

Sumário

 Hardware de E/S
 Software de E/S
 Discos
 RAID
 Formatação

2

Hardware de Entrada/Saída

3

Dispositivos de E/S

 Dispositivos de bloco

 Armazena informações em blocos de tamanho

fixo (512 a 32768 bytes)

 Blocos são escritos e lidos de maneira

independente

 Cada bloco possui seu próprio endereço
 Exemplo: Discos, fita magnética

 Dispositivos de caractere

 Envia e recebe fluxos de caracteres individuais e

independentes

 Não são endereçáveis
 Exemplos: Impressoras, interfaces de rede,

mouses

4

Dispositivos de E/S

 Outros tipos de dispositivos

 Relógios (clock)
 Vídeo

 Devido a grande diversidade na velocidade dos

dispositivos (devido as suas próprias
características) o SO possui a responsabilidade
de disponibilizar o acesso a todos de maneira
eficiente, apesar das diferenças

5

Taxas de Dados e Dispositivos

6

Controladores de Dispositivo
 Componentes dos Dispositivos de E/S

 Mecânicos

● Dispositivo propriamente dito

 Eletrônicos

● Também chamados de controlador de

dispositivo (driver) ou adaptador

● Pode manipular mais de um dispositivo
● Nos computadores pessoais são as chamadas

placas de expansão

7

Funções dos Controladores de
Dispositivo (CD)

 Converter dados binários seriais para dados

de bloco
 O controlador de disco entrega um preâmbulo,

seguido de 4096 bits (bloco de dados)

 Os blocos são armazenados bit a bit em buffers

 Correção de erros

 ECC (error-correcting code)
● Chekcsum, CRC ou similar

 Disponibilização para Memória Principal

 E/S mapeada em memória
 DMA (Direct Memory Access)

8

E/S Mapeada em Memória

 Comunicação entre CPU e CD é feita através

de registradores

 Ações como

 Enviar dados
 Receber dados
 Ligar/Desligar dispositivo
 Determinação de estado do dispositivo (impressora)
 São controladas pelo SO através da leitura dos

valores contidos nos registradores
 Buffers de dados de leitura e escrita

armazenam os dados disponíveis aos
programas e o SO através de portas
específicas

9

Portas de E/S

 Cada registrador é associado a um número

de porta

 Instrução para leitura

 IN REG,PORT

 Instrução para escrita

 OUT PORT,REG

 Nesse caso espaços de endereçamento de

E/S e memória são distintos

10

Mapeamento em Memória

 A proposta do PDP-11 é mapear na MP s
registradores dos dispositivos, criando um
mapeamento único

 Cada registrador é associado a uma posição

de memória única (mapeamento)

 No IBM PC temos um sistema híbrido
 Portas de E/S em endereços de 0 a 64K
 Buffers em endereços de 640K a 1M

11

Funcionamento

 Quando a CPU necessita ler uma palavra (seja

da memória ou de uma porta de E/S), o
endereço é colocado no barramento

 CPU emite um sinal READ e um outro para

sinalizar se a leitura se refere a memória ou a
uma porta de E/S

 No caso de E/S o respectivo dispositivo

responde a requisição
 No caso de E/S mapeada em memória a leitura é
direta e se dá através da seleção dos endereços

 Se um endereço está dentro da faixa de um
determinado dispositivo, este responderá a
requisição

12

Vantagens E/S Mapeada

 Evita o uso de instruções especiais de E/S

em baixo nível (ASSEMBLY)
 IN e OUT não podem ser executadas em C, C++

ou Java, por exemplo

 Para permitir acesso a um dispositivo pelo

usuário, basta que o SO faça o mapeamento
do dispositivo no espaço de endereçamento
do usuário

 Todas as instruções capazes de referenciar

(ler ou escrever) a memória podem
referenciar os registradores de controle dos
dispositivos

13

DMA (Direct Memory Access)

 CPU requisita os dados de um controlador de
E/S, um byte por vez, desperdiçando tempo
 O uso de DMA pelo SO requer um hardware

especial, Controlador DMA

14

Leitura no Disco sem DMA

 Controlador lê um bloco do dispositivo bit a

bit, colocando-o no buffer interno do
controlador

 A Soma de Verificação é executada para a

correção de eventuais erros de leitura
 Controlador causa interrupção que será

atendida pelo SO

 Na rotina de tratamento da interrupção, a

leitura do buffer é feita byte a byte em um
laço

 A cada iteração a leitura é feita do buffer e

copiada para a memória

15

Leitura no Disco com DMA

1.CPU passa valores aos registradores do

controlador DMA (CDMA) para configurá-lo e
iniciar o processo

16

Leitura no Disco com DMA

2.A transferência é requisitada através de um

comando do CDMA para que o controlador do
disco escreva os dados na memória principal
(MP)

17

Leitura no Disco com DMA

3.É feita a transferência do buffer do controlador

de disco para a MP

18

Leitura no Disco com DMA

4.Ao final da transferência, o controlador do disco
informa ao CDMA a conclusão através de um
sinal de confirmação (Ack). Os passos 2 a 4 se
repetem até que todo o buffer seja copiado para
a MP

19

Leitura no Disco com DMA

 Ao final do processo o CDMA interrompe a
CPU para informar que a transferência foi
concluída

 Quando o SO iniciar o tratamento da

interrupção não há mais necessidade de
copiar o bloco, pois o mesmo já está na MP

20

Algumas Características
dos Controladores DMA

 Podem tratar uma ou várias transferências

simultâneas (canais DMA)

 Cada canal possui conjuntos de registradores

próprios

 Normalmente operam em dois modos:

 Palavra (word-at-a-time mode)

● CDMA ocupa o barramento e o libera a cada byte que

é copiado para memória
 Bloco (modo surto - burst)

● CDMA ocupa o barramento e o libera apenas após a

transferência de vários bytes (bloco)

21

Por que Controladores de Disco
(CD) utilizam buffers internos?

Os CD não poderiam simplesmente copiar o
contéudo dos buffers internos diretamente para
a MP a medida que são lidos do disco?

22

Por que Controladores de Disco
(CD) utilizam buffers internos?

1.Ao armazenar os dados em buffer é possível

para o CD realizar a soma de verificação
(checksum) e detectar possíveis erros antes
da transferência para a MP

2.O CD ocuparia o barramento do sistema a
cada palavra, tornando o processo lento. A
cópia do disco para o buffer interno do CD
tem um ritmo próprio, não poderia esperar
pela disponibilidade do barramento. Com o
buffer preenchido a tarefa de transferência é
feita pelo CDMA, que é mais eficiente

23

Software de Entrada/Saída

24

Propriedades

 Independência de Dispositivos

 Programas devem ter acesso a qualquer

dispositivo de E/S de maneira similar (CD-ROM,
disco, etc), independente da sua natureza

 Nomeação Uniforme

 Utilização de strings ou inteiros

 Tratamento de Erros
 Sincronismo

 Síncrona (bloqueante)
 Assíncrona (orientada a interrupção)

 Buffering
 Dispositivos Compartilhados versus

Dedicados

25

Formas de realizar E/S

 E/S Programada

 Espera ociosa (busy waiting)
 Fácil de ser implementada
 Aumenta a ociosidade da CPU

 E/S Orientada a Interrupção

 Alternativa a E/S Programada utilizando

interrupções
 E/S com DMA

 DMA reduz o número de interrupções

necessárias para transferências entre dispositivos
e MP

 Ao invés de uma interrupção para cada byte, uma

para cada buffer completo

26

Discos

27

Tipos de Discos

 Magnéticos
 Estado Sólido
 Híbridos
 Ópticos

28

Tipos de Discos

 Magnéticos

 Principal uso: armazenamento confiável
 Flexíveis (disquetes)
 Rígidos (Hard Disks Drives - HDD)

 Estado Sólido
 Tem a mesma aplicação dos discos magnéticos

convencionais (HDD)
 Solid State Drives (SSD)
 Não há partes móveis (disco, cabeças, braços)
 Utiliza
 Baixo consu



29

Tipos de Discos

 Híbridos

 Utiliza ambas tecnologias: HDD e SSD
 Uma pequena parte da capacidade do disco

(4Gb) utiliza memórias e o restante é
complementado com um disco rígido (496Gb)



 Ópticos

 Principal uso: distribuição de informação (som,

vídeo, programas)

 CD-ROMs
 CD-Rs
 DVDs

30

Discos Magnéticos

 São organizados internamente em cilindros,

trilhas e setores

 Cada cilindro contém diversas trilhas, que

por sua vez são divididas em setores

 A Geometria do disco é definida durante a
fabricação dos mesmos (formatação física)

31

Parâmetros de disco

32

Interfaces de Acesso

 IDE (Integrated Drive Electronics)
 ATA (Advanced Technology Attachment)
 SCSI

33

IDE (Integrated Drive Electronics)

 Desenvolvido pela Western Digital e Compaq

em 1986

 Eletrônica reduzida
 Conectores de 40 pinos
 Velocidades de 5400 a 7200rpm
 Cada controlador IDE pode gerenciar até dois

discos: Master e Slave

 Taxas de transferência da ordem de 133MB/s



34

UltraDMA/66

 Protocolo de transferência de dados entre os

discos e a Memória RAM

 Desenvolvido pela Intel e Seagate
 Taxa de transferência de 66MB/s
 Compatibilidade com discos IDE convencional



35

SATA (Serial Advanced
Technology Attachment)

 Criado para substituir o padrão IDE
 Dados são transmitidos serialmente em altas

taxas de clock

 Taxas de transferência da ordem de 150

(SATA-150) a 300MB/s (SATA II)

36

SCSI (Small Computer System
Interface)

 Suporta diversos tipos de dispositivos, além

dos discos

 Comunicação peer-to-peer
 Qualquer dispositivo pode ser o

iniciador(exchange initiator) ou alvo (target)
 O barramento é utilizado apenas durante a

transferência de dados
 Cabos de 50 ou 68 vias
 Taxas de transferência da ordem de 80MB/s

37

RAID (redundant array of
inexpensive disks)

 Constitui-se basicamente de um agrupamento
de vários discos para obter benefícios como
maior velocidade de acesso e/ou tornar o
sistema mais confiável operando como se
fosse um único disco

 Requer o uso de controladores RAID especiais



38

Níveis de RAID

 Nível 0
 Nível 1
 Nível 2
 Nível 3
 Nível 4
 Nível 5

39

Benefícios

 Melhoria no desempenho (tempos de acesso

menores)

 Aumento da confiabilidade através de

redundância

 Facilitar a restauração de informações em

caso de desastres

40

RAID Nível 0

 Dados são divididos em

segmentos sequencialmente
(stripping)
 Vantagens

 Tempo de leitura menor (50%)

 Desvantagem

 Em caso de perda de um
segmento o arquivo fica
comprometido

41

RAID Nível 1

 Implementa o chamado
espelhamento de disco

 Vantagens

 Possibilidade de recuperação de
um setor a partir do outro disco

 Redundância
 Desvantagem

 Tempo de escrita maior

42

RAID Nível 4

 Um dos discos guarda os

dados em si e o outro guarda
as informações de paridade

 Em caso de perda, as

informações podem ser
reconstituídas a partir da
paridade
 Vantagens

 Leitura rápida
 Desvantagem
 Escrita lenta
 Recuperação complexa em caso

de falha

43

RAID Nível 2

 Implementa o controle de erros
ECC (Error Correction Code)

 Similar ao RAID 4
 Está obsoleto
 Desvantagem

 Discos atuais já utilizam

correção de erro

44

RAID Nível 3

 Versão simplificada do RAID 2
 Cada palavra de dados recebe

um bit de paridade que
armazenado em um segundo
disco

 Similar ao RAID 4
 Vantagens

 Leitura e escrita rápidas

 Desvantagem

 Montagem do RAID complexa e

feita via software

45

RAID Nível 5

 Similar ao RAID 4
 As informações de paridade
são distribuídas através de
todos os discos

 Vantagens

 Leitura rápida
 Maior confiabilidade em relação

ao RAID 4
 Desvantagem
 Escrita lenta
 Recuperação complexa em caso

de falha

46

Formatação de Discos

47

Setores do disco

Preâmbulo

Dados

ECC

 Preâmbulo contém um padrão binário,

indicando o início do setor

 Dados contém as informações propriamente

ditas, normalmente 512 bytes

 ECC contém informações para recuperação de

erros de leitura

 O espaço do disco não é totalmente utilizado

com dados
 Parte do espaço é ocupado pelo preâmbulo e ECC
 Para os fabricantes 1GB=1000x1000x1000 bytes
 Para o SO 1GB=1024x1024x1024 bytes

48

Formatação Lógica

 Feita via software tornando o disco

reconhecível pelo SO

 O disco é preparado para receber dados, é
feita a inclusão de um sistema de arquivos
 Praticamente cada SO possui um tipo de

sistema de arquivos próprio

 Exemplos de sistemas de arquivos

 FAT16 ou FAT32 (File Alocation Table)
 NTFS
 FFS (Fast File System – BSD)
 Ext2 ou Ext3

49

FAT

 Utiliza alocação por lista encadeada
 FAT32 utiliza clusters de 4KB, FAT16 de

32KB

50

FAT

 Vantagens

 Facilita o acesso aleatório ao

disco

 Deixa todo espaço do bloco

para dados

 Desvantagem

 A tabela de alocação deve

estar em memória

 Para um disco de 2GB e

blocos de 1KB são
necessárias 20 milhões de
entradas. Se cada entrada
possui 3 a 4 bytes, a tabela
ocuparia 60 a 80 MB da
memória principal

51

Second Extended File System
(ext2)

 Arquivos são guardados
em blocos do mesmo
tamanho

 Cada arquivo é descrito
por um único i-node que
contém as informações
de quais blocos compõe
o arquivo

52

Ext2

 Vantagem

 Cada i-node é carregado apenas quando o

arquivo correspondente está aberto

 A quantidade de memória ocupada é proporcional

a quantidade de arquivos abertos
simultaneamente

 Desvantagem

 A estrutura para manipular arquivos com

endereços de blocos que os indicados no i-node
pode se tornar complexa para arquivos grandes

53

Bibliografia

 SISTEMAS OPERACIONAIS MODERNOS.

Andrew Tanenbaum



OBSERVAÇÃO

A disponibilização das notas de aula através de slides serve
apenas como apoio aos estudos. Para um bom aproveitamento
e aprendizado é necessário a leitura das referências (livro
texto) e estar atento às aulas

54

FIM

55

