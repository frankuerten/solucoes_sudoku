Solucoes_sudoku

O algoritmo verifica se o sudoku fornecido tem 0, 1 ou várias soluções. Retorna: "0 soluções", "1 solução" ou "mais de uma solução".


    def solucoes_sudoku(tabuleiro):
        # Define um tabuleiro 9x9
        def encontrar_vazio():
            for i in range(9):
                for j in range(9):
                    if tabuleiro[i][j] == 0:
                        return (i, j)
            return None

        def eh_valido(num, pos):
            # Verifica linha
            for j in range(9):
                if tabuleiro[pos[0]][j] == num and pos[1] != j:
                    return False
    
            # Verifica coluna
            for i in range(9):
                if tabuleiro[i][pos[1]] == num and pos[0] != i:
                    return False
    
            # Verifica quadrante 3x3
            box_x = pos[1] // 3
            box_y = pos[0] // 3
    
            for i in range(box_y * 3, box_y * 3 + 3):
                for j in range(box_x * 3, box_x * 3 + 3):
                    if tabuleiro[i][j] == num and (i, j) != pos:
                        return False
    
            return True

        def backtracking(contador_solucoes):
            # Se já encontramos mais de 1 solução, podemos parar
            if contador_solucoes[0] > 1:
                return
    
            pos_vazio = encontrar_vazio()
            if not pos_vazio:
                contador_solucoes[0] += 1
                return
    
            row, col = pos_vazio
    
            for num in range(1, 10):
                if eh_valido(num, (row, col)):
                    tabuleiro[row][col] = num
                    backtracking(contador_solucoes)
                    tabuleiro[row][col] = 0  # Backtrack
            
                    # Se já encontramos mais de 1 solução, podemos parar
                    if contador_solucoes[0] > 1:
                        return

        # Faz uma cópia profunda do tabuleiro para não modificar o original
        import copy
        tabuleiro_copia = copy.deepcopy(tabuleiro)

        contador = [0]  # Usando lista para poder modificar dentro da função
        backtracking(contador)

        return contador[0]

    def verificar_sudoku(tabuleiro):
        # Primeiro, verifica se o tabuleiro inicial é válido
        def validar_tabuleiro_inicial():
            for i in range(9):
                for j in range(9):
                    if tabuleiro[i][j] != 0:
                        num = tabuleiro[i][j]
                        tabuleiro[i][j] = 0  # Remove temporariamente
                        if not eh_valido_rapido(num, (i, j)):
                            return False
                        tabuleiro[i][j] = num
            return True

        def eh_valido_rapido(num, pos):
            # Versão rápida para validação inicial
            for j in range(9):
                if j != pos[1] and tabuleiro[pos[0]][j] == num:
                    return False
            for i in range(9):
                if i != pos[0] and tabuleiro[i][pos[1]] == num:
                    return False
            box_x = pos[1] // 3
            box_y = pos[0] // 3
            for i in range(box_y * 3, box_y * 3 + 3):
                for j in range(box_x * 3, box_x * 3 + 3):
                    if (i, j) != pos and tabuleiro[i][j] == num:
                        return False
            return True

        # Verifica se o tabuleiro inicial é válido
        if not validar_tabuleiro_inicial():
            return "0 soluções"

        # Conta o número de soluções
        num_solucoes = resolver_sudoku(tabuleiro)

        if num_solucoes == 0:
            return "0 soluções"
        elif num_solucoes == 1:
            return "1 solução"
        else:
            return "mais de uma solução"

    if __name__ == "__main__":
        # Preencha as dicas do sudoku
        sudoku_jogo = [
            [0, 0, 1, 0, 0, 0, 0, 0, 2],
            [0, 0, 0, 3, 0, 0, 0, 6, 7],
            [0, 0, 9, 0, 8, 0, 0, 0, 0],
            [0, 0, 0, 0, 0, 2, 0, 0, 0],
            [0, 0, 8, 0, 0, 0, 5, 0, 0],
            [0, 0, 0, 6, 0, 0, 0, 0, 0],
            [0, 0, 0, 0, 5, 0, 9, 0, 0],
            [2, 7, 0, 0, 0, 1, 0, 0, 0],
            [3, 0, 0, 0, 0, 0, 4, 0, 0]
        ]
    
    
        print("Quantidade de soluções:", verificar_sudoku(sudoku_jogo))
        




Para utilizar o algoritmo, basta colá-lo no Google Colab e editar as células do sudoku. Note que os zeros representam os espaços vazios.
Utilizando o exemplo dado acima, o programa retorna "Quantidade de soluções: 1 solução."
