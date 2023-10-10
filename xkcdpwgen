#!/usr/bin/python3
import argparse
import random
import rstr
import re

wordFile = open('words.txt', 'r')
wordContent = wordFile.readlines()

parser = argparse.ArgumentParser(
    prog='xkcdpwgen',
    description='Generate a secure, memorable password using the XKCD method')
parser.add_argument('-w', '--words', dest='WORDS', type=int, default=4,
                    help='include WORDS words in the password (default=4)')
parser.add_argument('-c', '--caps', dest='CAPS', type=int, default=0,
                    help='capitalize the first letter of CAPS random words (default=0)')
parser.add_argument('-n', '--numbers', dest='NUMBERS', type=int, default=0,
                    help='insert NUMBERS random numbers in the password (default=0)')
parser.add_argument('-s', '--symbols', dest='SYMBOLS', type=int, default=0,
                    help='insert SYMBOLS random symbols in the password (default=0)')

args = parser.parse_args()

# capitalize the first character of a random array of strings for a number of times
def capitalize(chosenWords):
    indexes = [x for x in range(0, len(chosenWords))]
    for x in range(min(len(chosenWords), args.CAPS)):
        randPos = random.randint(0, len(indexes)-1)
        chosenWords[indexes[randPos]] = chosenWords[indexes[randPos]].capitalize()
        del indexes[randPos]

# insert the given character at the front or end of any strings in the given array
def insert(chosenWords, insertChar):
    randPos = random.randint(0, len(chosenWords)-1)
    if(random.randint(0, 100) >= 50):
        chosenWords[randPos] =  insertChar + chosenWords[randPos]
    else:
        chosenWords[randPos] = chosenWords[randPos] + insertChar

# validates the generated password to ensure it meets user's specification
def validate(chosenWords, newPass):
    noOfDigits = len(re.findall('[0-9]', newPass))
    noOfWords = len(chosenWords)
    noOfCaps = len(re.findall('[A-Z]', newPass))
    noOfSymbols = len(re.findall('[^A-Za-z0-9\s]', newPass))
    if (noOfWords != args.WORDS or noOfDigits != args.NUMBERS
        or noOfCaps != args.CAPS or noOfSymbols != args.SYMBOLS):
        raise Exception("Password does not meet specification!")

# generates a XKCD password               
def generatePass():
    newPass = ""
    chosenWords = []
    for num in range(args.WORDS):
        randNum = random.randint(0, len(wordContent)-1)
        if wordContent[randNum][-1] == '\n':
            chosenWords.append(wordContent[randNum][:-1])
        else:
            chosenWords.append(wordContent[randNum])
    capitalize(chosenWords)
    for x in range (0, args.NUMBERS):
        insert(chosenWords, str(random.randint(0, 9)))
    for x in range (0, args.SYMBOLS):
        insert(chosenWords, rstr.xeger("[^A-Za-z0-9\s]"))
    for word in chosenWords:
        newPass = newPass + word
    validate(chosenWords, newPass)
    return newPass

print(generatePass())
