Soluções_sudoku

O algoritmo verifica se o sudoku fornecido tem 0, 1 ou várias soluções. Retorna: "0 soluções", "1 solução" ou "mais de uma solução".


    import copy

    def solucoes_sudoku(tabuleiro):
        # Usa o tabuleiro recebido como parâmetro
        def encontrar_vazio(tab):
            for i in range(9):
                for j in range(9):
                    if tab[i][j] == 0:
                        return (i, j)
            return None

        def eh_valido(tab, num, pos):
            # Verifica linha
            for j in range(9):
                if tab[pos[0]][j] == num and pos[1] != j:
                    return False

            # Verifica coluna
            for i in range(9):
                if tab[i][pos[1]] == num and pos[0] != i:
                    return False

            # Verifica quadrante 3x3
            box_x = pos[1] // 3
            box_y = pos[0] // 3

            for i in range(box_y * 3, box_y * 3 + 3):
                for j in range(box_x * 3, box_x * 3 + 3):
                    if tab[i][j] == num and (i, j) != pos:
                        return False

            return True

        def backtracking(tab, contador_solucoes):
            if contador_solucoes[0] > 1:
                return

            pos_vazio = encontrar_vazio(tab)
            if not pos_vazio:
                contador_solucoes[0] += 1
                return

            row, col = pos_vazio

            for num in range(1, 10):
                if eh_valido(tab, num, (row, col)):
                    tab[row][col] = num
                    backtracking(tab, contador_solucoes)
                    tab[row][col] = 0

                    if contador_solucoes[0] > 1:
                        return

        tabuleiro_copia = copy.deepcopy(tabuleiro)
        contador = [0]
        backtracking(tabuleiro_copia, contador)
        return contador[0]


    def verificar_sudoku(tabuleiro):
        def validar_tabuleiro_inicial(tab):
            # Verifica números inválidos
            for i in range(9):
                for j in range(9):
                    if tab[i][j] != 0:
                        if tab[i][j] < 1 or tab[i][j] > 9:
                            return False

            # Verifica duplicatas de forma mais eficiente
            # Verifica linhas
            for i in range(9):
                linha = [x for x in tab[i] if x != 0]
                if len(linha) != len(set(linha)):
                    return False

            # Verifica colunas
            for j in range(9):
                coluna = [tab[i][j] for i in range(9) if tab[i][j] != 0]
                if len(coluna) != len(set(coluna)):
                    return False

            # Verifica quadrantes 3x3
            for box_i in range(3):
                for box_j in range(3):
                    quadrante = []
                    for i in range(box_i * 3, box_i * 3 + 3):
                        for j in range(box_j * 3, box_j * 3 + 3):
                            if tab[i][j] != 0:
                                quadrante.append(tab[i][j])
                    if len(quadrante) != len(set(quadrante)):
                        return False

            return True

        # Verifica se o tabuleiro inicial é válido
        if not validar_tabuleiro_inicial(tabuleiro):
            return "0 soluções"

        # Conta o número de soluções
        num_solucoes = solucoes_sudoku(tabuleiro)

        if num_solucoes == 0:
            return "0 soluções"
        elif num_solucoes == 1:
            return "1 solução"
        else:
            return "mais de uma solução"


    if __name__ == "__main__":
        sudoku_modelo = [
            [0, 0, 0, 0, 0, 0, 0, 0, 2],
            [0, 0, 0, 3, 0, 0, 0, 6, 7],
            [0, 0, 9, 0, 8, 0, 0, 0, 0],
            [0, 0, 0, 0, 0, 2, 0, 0, 0],
            [0, 0, 8, 0, 0, 0, 5, 0, 0],
            [0, 0, 0, 6, 0, 0, 0, 0, 0],
            [0, 0, 0, 0, 5, 0, 9, 0, 0],
            [2, 7, 0, 0, 0, 1, 0, 0, 0],
            [3, 0, 0, 0, 0, 0, 4, 0, 0]
        ]
        print("Quantidade de soluções:", verificar_sudoku(sudoku_modelo))
        




Para utilizar o algoritmo, basta colá-lo no Google Colab e editar as células do sudoku_modelo. Note que os zeros representam os espaços vazios.
Utilizando o exemplo dado acima, o programa retorna "Quantidade de soluções: 1 solução."
