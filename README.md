# Bingo-Hexadecimal
Jogo de bingo criado para apresentação na disciplina de laboratório de programação.
<Vou adicionar a descrição detalhada aqui em breve>

--------------------------------------------------------------------------------------------------

import numpy
import random

#ESSA FUNÇÃO CRIA A TABELA EM BASE DECIMAL
def criarCartela():
    cartela = set()
    for i in range(16):
        numero = numpy.random.randint(16, 160)
        while True:
            if (numero not in cartela):
                cartela.add(numero)
                break
            else:
                numero = numpy.random.randint(16, 160)
    return cartela
    
#ESSA FUNÇÃO CONVERTE UM NÚMERO INTEIRO PARA HEXADECIMAL
def converteHex(n):
        x =n//16
        y =n%16
        if(y!=0):
            if (y < 10):
                hex = (str(x)+str(y)) 
            if (y == 10):
                hex = (str(x) + "A")
            if (y == 11):
                hex = (str(x) + "B")
            if (y == 12):
                hex = (str(x) + "C")
            if (y == 13):
                hex = (str(x) + "D")
            if (y == 14):
                hex = (str(x) + "E")
            if (y == 15):
                hex = (str(x) + "F")
        else:
            hex = (' '+str(x))
        return hex
    
    #ESSA FUNÇÃO PEGA UMA TABELA EM BASE DECIMAL, CONVERTE PARA HEXADECIMAL E EXIBE
def exibeCartela(cartela):
    dfhex = {0:'0',1:'0',2:'0',3:'0',4:'0',5:'0',6:'0',7:'0',8:'0',9:'0',10:'0',11:'0',12:'0',13:'0',14:'0',15:'0'}
    n=0
    for i in cartela:
        dfhex[n] = converteHex(i)        
        n +=1
    print(f'{dfhex[0]}   {dfhex[1]}   {dfhex[2]}   {dfhex[3]}\n{dfhex[4]}   {dfhex[5]}   {dfhex[6]}   {dfhex[7]}\n{dfhex[8]}   {dfhex[9]}   {dfhex[10]}   {dfhex[11]}\n{dfhex[12]}   {dfhex[13]}   {dfhex[14]}   {dfhex[15]}')
    print('', end='\n')
    
#ESSA FUNÇÃO SORTEIA PEDRAS SEM REPETIÇÕES. PARA ISSO É NECESSÁRIO UM VETOR PREEXISTENTE COM OS VALORES DE 16 A 159 CONTIDOS NELE
#A FUNÇÃO TAMBÉM GUARDA AS PEDRAS QUE JA FORAM SORTEADAS EM UM VETOR SEPARADO, SEM EXIBIR
def sorteiaPedra(pedras, pedrasSorteadas):
    var = random.choice(pedras)
    index = pedras.index(var)
    pedrasSorteadas.add(converteHex(pedras[index]))
    return pedras.pop(index)

#ESSA FUNÇÃO CONFERE O VALOR DE UMA PEDRA NA CARTELA, FUNCIONANDO FINALMENTE
def conferePedra(pedra, cartela):
    for i in cartela:
        if (pedra in cartela):
            return True
        else:
            return False 
            
#ESSA FUNÇÃO EXIBE A PONTUAÇÃO DE UMA CARTELA. A LÓGICA DE ACUMULAR PONTUAÇÃO FOI IMPLEMENTADA NA MAIN
def exibirPontuacao(pontosJogadores):
    for key in pontosJogadores:
        print(f'O(A) jogador(a) {key} fez {pontosJogadores[key]} pontos.')
        
#ESSA FUNÇÃO EXIBE O VENCEDOR E A PREMIAÇÃO
def exibirPremio(jogadores, pedrasSorteadas, pontosJogadores, maximoCartela):
    for key in pontosJogadores:
        if (pontosJogadores[key] == max(pontosJogadores.values())):
            index = key
    premio = 100 - len(pedrasSorteadas) + maximoCartela[index]
    print(f'O ganhador foi {index} e o valor do prêmio foi de R${premio}.')
    
#ESSA FUNÇÃO REINICIALIZA AS VARIÁVEIS PARA COMEÇAR UM NOVO JOGO
def reiniciarJogo(jogadores, maximoCartela, pedrasSorteadas, opcaoMenu, pedras, valor):
    jogadores.clear()
    maximoCartela.clear()
    pedrasSorteadas.clear()
    opcaoMenu = 0
    pedras.clear()
    valor = 16
    for x in range(144):
        pedras.append(valor)
        valor += 1
        
jogadores = {} #guarda os nomes:cartelas dos jogadores
maximoCartela = {} #guarda o maior valor de cada cartela, pra calcular o premio no final
pedrasSorteadas = set() #conjunto que vai guardas as pedras que já saíram
opcaoMenu = 0 #inicializando variável do menu
#Armazena todas as pedras do bingo em um vetor. É como se fosse a bolsa de pedras a serem sorteadas
pedras = []
valor = 16
for x in range(144):
    pedras.append(valor)
    valor += 1
#Fim do armazenamento das pedras no vetor---------------
            
#Menu do bingo
print('Bem vindo ao bingo!!!')
while True:
    print('Opção 1: criar nova cartela')
    print('Opção 2: exibir cartelas')
    print('Opção 3: começar o jogo')
    print('Opção 0: encerrar programa')
    opcaoMenu = int(input('Escolha uma opção (0, 1, 2 ou 3): '))
    if (opcaoMenu != 1 and opcaoMenu != 2 and opcaoMenu != 3 and opcaoMenu != 0):
        print('Opção inválida')
    if (opcaoMenu == 0):
        print('O programa foi encerrado. Até a próxima.')
        break
    if (opcaoMenu == 1):
        nomeJogador = input('Digite o nome do jogador:')
        print('', end='\n')
        jogadores[nomeJogador] = criarCartela()
        maximoCartela[nomeJogador] = max(jogadores[nomeJogador])
    if (opcaoMenu == 2):
        print('', end='\n')
        for key in jogadores:
            print(f'Cartela {key}')
            exibeCartela(jogadores[key])
    if (opcaoMenu == 3):
        pontosJogadores = jogadores.copy()
        for key in pontosJogadores:
            pontosJogadores[key] = 0
        print('', end='\n')
        print('Vai começar o jogo!!!')
        print('', end='\n')
        while (max(pontosJogadores.values()) < 16):
            print('Opção 1: sortear pedra')
            print('Opção 2: exibir pontuação')
            print('Opção 0: encerrar programa')
            opcaoMenu = int(input('Escolha uma opção:'))
            if (opcaoMenu != 1 and opcaoMenu != 2 and opcaoMenu != 0):
                print('Opção inválida')
            if (opcaoMenu == 0):
                print('', end='\n')
                break
            if (opcaoMenu == 1):
                print('', end='\n')
                sorteada = sorteiaPedra(pedras, pedrasSorteadas)
                print(f'A pedra sorteada foi: {converteHex(sorteada)}')
                print(f'As pedras que já saíram foram:', pedrasSorteadas, sep=' ')
                for key in jogadores:
                    if (conferePedra(sorteada, jogadores[key]) == True):
                        pontosJogadores[key] += 1
                        print(f'O(A) jogador(a) {key} marcou um ponto')
                        jogadores[key].remove(sorteada)
                        #NESSA PARTE, QUANDO O JOGADOR ACERTA UMA PEDRA, O VALOR DA PEDRA EM SUA CARTELA É REMOVIDO E SUBSTITUÍDO
                        #PELO VALOR 0
                    print(f'Tabela {key}')
                    exibeCartela(jogadores[key])                
            if (opcaoMenu == 2):
                print('', end='\n')
                exibirPontuacao(pontosJogadores)
        print('', end='\n')
        print('BINGO!!! Alguém acaba de completar a cartela. Parabéns!!!')
        print(f'O total de pedras sorteadas foram {len(pedrasSorteadas)}.')
        exibirPremio(jogadores, pedrasSorteadas, pontosJogadores, maximoCartela)
        print('', end='\n')
        reiniciarJogo(jogadores, maximoCartela, pedrasSorteadas, opcaoMenu, pedras, valor)
