import random

def criar_tabuleiro(linhas, colunas, num_minas):
    """Cria o tabuleiro do jogo com minas colocadas aleatoriamente."""
    tabuleiro = [[' ' for _ in range(colunas)] for _ in range(linhas)]
    minas = random.sample(range(linhas * colunas), num_minas)
    for mina in minas:
        linha, coluna = divmod(mina, colunas)
        tabuleiro[linha][coluna] = '*'
    return tabuleiro

def contar_minas_vizinhas(tabuleiro, linha, coluna):
    """Conta o número de minas vizinhas para uma determinada célula."""
    contador = 0
    for i in range(max(0, linha - 1), min(len(tabuleiro), linha + 2)):
        for j in range(max(0, coluna - 1), min(len(tabuleiro[0]), coluna + 2)):
            if tabuleiro[i][j] == '*':
                contador += 1
    return contador

def revelar_celula(tabuleiro, tabuleiro_revelado, linha, coluna):
    """Revela uma célula e suas células vizinhas se não houver minas por perto."""
    if tabuleiro_revelado[linha][coluna] != ' ':
        return
    tabuleiro_revelado[linha][coluna] = str(contar_minas_vizinhas(tabuleiro, linha, coluna))
    if tabuleiro_revelado[linha][coluna] == '0':
        for i in range(max(0, linha - 1), min(len(tabuleiro), linha + 2)):
            for j in range(max(0, coluna - 1), min(len(tabuleiro[0]), coluna + 2)):
                revelar_celula(tabuleiro, tabuleiro_revelado, i, j)

def imprimir_tabuleiro(tabuleiro_revelado):
    """Imprime o tabuleiro do jogo."""
    for linha in tabuleiro_revelado:
        print(' '.join(linha))

def jogar_campo_minado(linhas, colunas, num_minas):
    """Função principal para jogar o jogo Campo Minado."""
    tabuleiro = criar_tabuleiro(linhas, colunas, num_minas)
    tabuleiro_revelado = [[' ' for _ in range(colunas)] for _ in range(linhas)]
    while True:
        imprimir_tabuleiro(tabuleiro_revelado)
        linha, coluna = map(int, input("Digite a linha e coluna (separadas por espaço): ").split())
        if tabuleiro[linha][coluna] == '*':
            print("Você perdeu!")
            imprimir_tabuleiro(tabuleiro)
            break
        revelar_celula(tabuleiro, tabuleiro_revelado, linha, coluna)
        if all(all(celula != ' ' for celula in linha) for linha in tabuleiro_revelado):
            print("Você venceu!")
            imprimir_tabuleiro(tabuleiro)
            break

# Configurações do jogo
linhas = 8
colunas = 8
num_minas = 10

# Inicia o jogo
jogar_campo_minado(linhas, colunas, num_minas)
