# tic-tac-toe_AI

import math

HUMAN = 'O'
AI = 'X'
EMPTY = ' '

def init_board():
    return [EMPTY] * 9

def check_winner(board, player):
    win_conditions = [
        [0, 1, 2], [3, 4, 5], [6, 7, 8], # Horizontal
        [0, 3, 6], [1, 4, 7], [2, 5, 8], # Vertical
        [0, 4, 8], [2, 4, 6]             # Diagonal
    ]
    return any(all(board[pos] == player for pos in condition) for condition in win_conditions)

def is_draw(board):
    return all(cell != EMPTY for cell in board)

def available_moves(board):
    return [i for i, cell in enumerate(board) if cell == EMPTY]

def minimax(board, depth, alpha, beta, is_maximizing):
    if check_winner(board, AI):
        return 1
    if check_winner(board, HUMAN):
        return -1
    if is_draw(board):
        return 0
    
    if is_maximizing:
        max_eval = -math.inf
        for move in available_moves(board):
            board[move] = AI
            eval = minimax(board, depth + 1, alpha, beta, False)
            board[move] = EMPTY
            max_eval = max(max_eval, eval)
            alpha = max(alpha, eval)
            if beta <= alpha:
                break
        return max_eval
    else:
        min_eval = math.inf
        for move in available_moves(board):
            board[move] = HUMAN
            eval = minimax(board, depth + 1, alpha, beta, True)
            board[move] = EMPTY
            min_eval = min(min_eval, eval)
            beta = min(beta, eval)
            if beta <= alpha:
                break
        return min_eval

def get_best_move(board):
    best_score = -math.inf
    best_move = None
    for move in available_moves(board):
        board[move] = AI
        score = minimax(board, 0, -math.inf, math.inf, False)
        board[move] = EMPTY
        if score > best_score:
            best_score = score
            best_move = move
    return best_move

def print_board(board):
    for i in range(0, 9, 3):
        print(board[i:i+3])
    print()

def play_game():
    board = init_board()
    current_player = HUMAN
    while True:
        print_board(board)
        if check_winner(board, HUMAN):
            print("Human wins!")
            break
        if check_winner(board, AI):
            print("AI wins!")
            break
        if is_draw(board):
            print("It's a draw!")
            break
        
        if current_player == HUMAN:
            move = int(input("Enter your move (0-8): "))
            if board[move] == EMPTY:
                board[move] = HUMAN
                current_player = AI
            else:
                print("Invalid move. Try again.")
        else:
            move = get_best_move(board)
            board[move] = AI
            current_player = HUMAN

play_game()
