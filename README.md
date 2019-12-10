# PDI
# ATIVIDADE 1 - SISTEMA DE MANIPULAÇÃO DE IMAGENS
# RESUMO
O trabalho 01 tem por finalidade desenvolver um sistema para abrir, exibir, manipular
e salvar imagens RGB com 24 bits/pixel (8 bits/componente/pixel). O sistema deve ter a
seguinte funcionalidade:

1. Conversão RGB-YIQ-RGB (cuidado com os limites de R, G e B na volta!);

2. Exibição de bandas individuais (R, G e B) como imagens monocromáticas ou
coloridas (em tons de R, G ou B, respectivamente);

3. Negativo;

4. Controle de brilho multiplicativo (s = r.c, c real não negativo) (cuidado com os limites
de R, G e B);

5. Convolução m x n com máscara especificada pelo usuário em arquivo texto. Testar
com filtros Média e Sobel;

6. Filtro mediana e moda m x n.

O sistema deve ser desenvolvido em uma linguagem de programação a escolha da
equipe. Não pode ser utilizado bibliotecas ou funções dedicadas de processamento de
imagens. Para os itens 3 e 4 devem ser testadas em RGB e na banda Y com posterior
conversão para RGB.

# MATERIAIS E MÉTODOS
O trabalho de implementação foi realizado em linguagem de programação PYTHON,
mais precisamente na versão 3.8.0, com o auxílio de uma plataforma de desenvolvimento
chamada GOOGLE COLAB.

O Google Colab é um serviço de hospedagem gratuito do Google destinado a
incentivar a pesquisa e o aprendizado. É um serviço que disponibiliza ao usuário o suporte a
linguagem de programação, como python, juntamente com toda uma bagagem necessária que
um desenvolvedor precisa para executar seu código, aceleração de GPU, bibliotecas já
instaladas, recurso de colaboração (assim todos podemos participar a distância do
desenvolvimento do trabalho), e etc, resumidamente, são máquinas virtuais onde utilizaremos
do aparato da empresa como se fosse nossas máquinas pessoais.

O processo de desenvolvimento do trabalho se deu primeiramente com a leitura de
uma imagem e em seguida a sua conversão para uma matriz de pixels a qual a partir daí
iremos trabalhar em cima, utilizando fórmulas e métodos de manipulação de matriz para
conseguir gerar como resultado uma outra imagem atingindo os respectivos resultados
esperados para cada etapa.

As etapas de desenvolvimento seguiram uma ordem pré-estabelecida na descrição do
trabalho.

● Conversão RGB - YIQ - RGB;

● Exibição de bandas R, G e B (monocromática e colorida);

● Filtro negativo;

● Controle de brilho multiplicativo;

● Convolução (média e sobel);

● Filtro da mediana;

● Filtro da moda;

Nas próximas etapas do relatório serão descritas cada uma das etapas citadas acima.

# CONVERSÃO RGB - YIQ - RGB
O sistema RGB (R/Red/Vermelho - G/Green/Verde - B/Blue/Azul) foi desenvolvido
com propósito de reproduzir as cores em dispositivos eletrônicos como telas de monitores. As
cores RGB quando combinadas conseguem reproduzir um gamute de cores variadas que mais
se aproxima do que os cones conseguem enxergar. Sistema padrão para a comissão de
iluminação internacional - CIE.

O modelo YIQ (Y/Luminância - I e Q/crominância) também possui a proposta de
reproduzir as cores, porém, utiliza uma combinação linear das diferenças entre os valores
RGB.

Para realizar a conversão RGB-YIQ-RGB é necessário fazer a divisão das imagens
nas bandas R,G,B. Após isso, é realizada a conversão desse array para a banda YIQ. Essa
conversão é feita através da aplicação das fórmulas:

Y = 0.299R + 0.587G + 0.114B

I = 0.596R – 0.274G –0.322B

Q = 0.211R – 0.523G + 0.312B

Assim iremos obter um array dos elementos convertidos em YIQ. Por fim, para a
conversão YIQ-RGB é necessária aplicar no array tais fórmulas:

R = 1.000 Y + 0.956 I + 0.621 Q

G = 1.000 Y – 0.272 I – 0.647 Q

B = 1.000 Y – 1.106 I + 1.703 Q

Ao final do processo, iremos obter uma nova imagem.

# EXIBIÇÃO DE BANDAS INDIVIDUAIS
# MONOCROMÁTICAS
A exibição de bandas monocromáticas é a radiação de um comprimento de onda
único, que pode ser obtido através da seguinte operação aritmética (variando a constante
da multiplicação para as demais bandas): r * 0.299 + r * 0.587 + r * 0.114. A partir daí
repetimos a operação descrita alterando os valores para o que consta em G e em seguida
em B, veja o exemplo abaixo:

(43, 72, 53) - pixel original

(43, 43, 43) - tom de cinza atingido pela repetição do valor de R.

(72, 72, 72) - tom de cinza atingido pela repetição do valor de G.

(53, 53, 53) - tom de cinza atingido pela repetição do valor de B.


# COLORIDAS
Para a criação de uma nova imagem colorida, é preciso ignorar as outras bandas em
prol do tom em questão.
Exemplo: banda R = (R, 0, 0).

# NEGATIVO
O negativo se dá pela inversão de cores na imagem, o contraste ocorre de modo que
as áreas escuras se tornam claras e as áreas claras se tornam escuras. Um pixel de cor branca
tem os valores de RGB em (255, 255, 255) e a partir desta informação utilizamos a seguinte
fórmula para subtrair o valor do pixel original do branco:

R = (-1*R + 255)

G = (-1*G + 255)

B = (-1*B + 255)

Fazendo o cálculo do inverso da banda + 255, iremos obter o negativo do pixel em
questão.

# CONTROLE DE BRILHO MULTIPLICATIVO
No controle de brilho multiplicativo é realizada uma multiplicação, de um parâmetro
qualquer, um valor inteiro que chamamos de “d”, pelas bandas R, G, e B. Essa multiplicação
pode ser realizada tanto na imagem original (R, G, B), como nas bandas de modo individual
(R ou G ou B).

Imagem = (R*d, G*d, B*d)

No desenvolvimento do controle do brilho multiplicativo pode-se obter valores abaixo
de 0 e acima de 255 (que é o valor máximo ao qual o sistema RGB consegue reproduzir),
com isso é preciso desenvolver um controle para que os valores abaixo de 0 fiquem em 0 e os
valores acima de 255 permaneçam em 255 visto que temos um espaço amostral de 0~255
para trabalhar.


# CONVOLUÇÃO
Para tratarmos de convolução primeiramente citamos correlação. Correlação é a
utilização de uma máscara, composta por valores aleatórios, para tratamento de uma
determinada imagem, seja uma máscara de identificação de algum padrão, quanto para
suavização, onde a máscara é deslizante, percorrendo por toda a extensão da imagem.
Convolução é a correlação com uma máscara rebatida. O rebatimento de uma máscara
é o espelhamento dela.

Quanto maior a máscara mais suavização a imagem irá receber, assim perdendo em
qualidade. No trabalho utilizamos a máscara de tamanho 3x3 demonstrada abaixo:

[0.1 0.2 0. ]

[0. 0. 0. ]

[0. 0. 0.4]


# FILTRO DA MÉDIA
O filtro da média é um filtro linear de suavização, que elimina ruídos da imagem,
onde todos os valores na máscara aplicada são definidos por 1 / N, sendo N o número de
pixels total da máscara. Por exemplo, a máscara que utilizamos foi de dimensão 3x3, ou seja,
todos os valores da máscara do filtro serão 1/9, pois N = 9. O tamanho do N determina o grau
de suavização e perda de detalhes.

# FILTRO DE SOBEL
O filtro de Sobel é uma utilização de convolução com uma máscara com valores pré
definidos. O filtro de Sobel é utilizado para detecção de bordas e pode ser encontrado com
duas variações, Sobel Vertical e Sobel Horizontal como apresentado abaixo.
 Sobel Vertical Sobel Horizontal
 
[ -1 0 1 ] [ -1 -2 -1 ]

[ -2 0 2 ] [ 0 0 0 ]

[ -1 0 1 ] [ 1 2 1 ]


# FILTRO MEDIANA
Filtro da mediana é uma transformação bastante usada para suavizar ruído do tipo
impulsivo em sinais e imagens digitais. Uma imagem digital pode ser representada por uma
matriz. Dada uma matriz A de inteiros positivos, com m linhas e n colunas, e dados dois
inteiros positivos e ímpares p e q, o filtro da mediana produz uma matriz transformada M,
com as mesmas dimensões que A, definida da seguinte maneira: para cada par de índices (i,j),
o elemento M(i,j) da matriz transformada é a mediana dos elementos de Aij (a vizinhança p ×
q em torno de (i, j)).

Ao contrário dos outros filtros, o filtro da mediana preserva bem as bordas, pois é
insensível aos valores extremos

# FILTRO MODA
O filtro da moda seleciona o valor que ocorre com maior frequência na vizinhança.
Todos os pixels sob a máscara terão seus valores substituídos pelo valor da moda. Assim, o
filtro da moda irá eliminar valores discrepantes, dando uma suavização à imagem.

# CONCLUSÃO
Ao fim do trabalho pode-se concluir que, o conhecimento a respeito do
Processamento Digital de Imagem foi posto em prática, pois, foram replicadas, as técnicas
consideradas pertencentes ao assunto ministrado em aula pelo professor, agregando valor,
tanto para o grupo, quanto para a geração de conhecimento para a respectiva área.

Também foi possível o aprofundamento do conhecimento da linguagem Python e na
utilização da ferramenta Google Colab, juntamente com a fixação do aprendizado sobre os
conceitos de operações pixel a pixel, operações convolucionais e manipulação de imagens.
