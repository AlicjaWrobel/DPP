import string
import random
import itertools

dictionary = open("dic.txt").read().splitlines()
boardString = open("board.txt").read().splitlines()
board = []
word = None
letters = []
bag = [[]]
freeFields = []
sizeOfLetters = 7

def Bag():
    
    bag.append("A", 9)
    bag.append("B", 2)
    bag.append("C", 2)
    bag.append("D", 4)
    bag.append("E", 12)
    bag.append("F", 2)
    bag.append("G", 3)
    bag.append("H", 2)
    bag.append("I", 9)
    bag.append("J", 9)
    bag.append("K", 1)
    bag.append("L", 4)
    bag.append("M", 2)
    bag.append("N", 6)
    bag.append("O", 8)
    bag.append("P", 2)
    bag.append("Q", 1)
    bag.append("R", 6)
    bag.append("S", 4)
    bag.append("T", 6)
    bag.append("U", 4)
    bag.append("V", 2)
    bag.append("W", 2)
    bag.append("X", 1)
    bag.append("Y", 2)
    bag.append("Z", 1)


def randomChar():
    letter = string.ascii_letters.upper()
    return random.choice(letter)

def newLetters():
    letters.clear()
    letter = 0
    while letter < sizeOfLetters:
        letters.append(randomChar())
        letter += 1
    return letters

def convBoard():
    global board
    board = list(''.join(boardString))

def showBoard(board):
    i = 0
    for row in range(15):
        for elem in range(15):
            print((board[i]), end= ' ')
            i += 1
        print('')

def checkWord(word):
    elem = 0
    while elem < len(dictionary):
        if(word in dictionary):
            return word
        else:
            return 0
        elem += 1

def randomWord(letters):
    for sizeOfString in range(len(letters)+1, 2, -1):
        for stringOfSize in itertools.permutations(letters, sizeOfString):
            word = ''.join(stringOfSize)
            if checkWord(word) != 0:
                return word

def searachFreeSpace(board, word):
    for charWord in range(len(word)):
        for charBoard in range(len(board)):
            if board[charBoard] == word[charWord]:
                nextBoardChar = charBoard + 1
                preBoardChar = charBoard - 1
                freeFields.clear()
                while int(board[nextBoardChar] == "-") & int(board[preBoardChar] == '-'):
                    freeFields.append(nextBoardChar)
                    nextBoardChar += 1
                    if len(freeFields) == len(word)-charWord-1:
                        for setChar in range(len(freeFields)):
                            board[charBoard+setChar+1] = word[charWord+setChar+1]
                        for setChar2 in range(len(word)-len(freeFields)-1):
                            board[charBoard-(len(word)-len(freeFields))+setChar2+1] = word[setChar2]
                            break
    return 0

def playScrabble():
    global word
    convBoard()
    print("Startowa plansza")
    showBoard(board)
    while word == None:
        letters = newLetters()
        print("Dostepne litery ", letters)
        word = randomWord(letters)
    print("Utworzone slowo", word)
    searachFreeSpace(board, word)
    if searachFreeSpace(board, word) == 0:
        print("Nie mozna dopasowac wylosowanego slowa")
    else:
        print("Koncowa plansza ")
        showBoard(board)

playScrabble()