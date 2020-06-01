# Blackjack
Playing the game 21 as close as possible to the table game.

import random
import os
deck = [2,3,4,5,6,7,8,9,10,11,12,13,14]*4
 
money = input("How much money would you like to play with? $")
 
def play_again():
again = input("Do you wnat to play again? [Y]es/[N]o :").lower()
if again == "y":
  dhand = []
  phand =[]
  deck = [2,3,4,5,6,7,8,9,10,11,12,13,14]*4
  game()
else:
  print("Bye! Your winnings are: ")
  exit()
def clear():
 if os.name == 'nt':
   os.system('CLS')
 if os.name == 'posix':
   os.system('clear')
def deal(deck):
hand = []
for i in range(2):
  random.shuffle(deck)
  card = deck.pop()
  if card == 11:card="J"
  if card == 12:card="Q"
  if card == 13:card="K"
  if card == 14:card="A"
  hand.append(card)
return hand
 
def winnings(bid):
 bid = input("How much would you like to bid? $")
 
def total(hand):
total = 0
for card in hand:
   if card == "J" or card == "Q" or card == "K":
     total+= 10
   elif card == "A":
     if total >= 11: total+= 1
     else: total+= 11
   else:
     total += card
return total
def hit(hand):
 card = deck.pop()
 if card == 11:card = "J"
 if card == 12:card = "Q"
 if card == 13:card = "K"
 if card == 14:card = "A"
 hand.append(card)
 return hand
def presults(dhand,phand):
print("The dealer has a " + str(dhand) + "for a total of " + str(total(dhand)))
print("You have a " + str(phand) + " for a total of " + str(total(phand)))
def score(dhand,phand):
if total(phand) == 21:
  presults(dhand,phand)
  print("Congratulations! You got 21! \n")
elif total(dhand) == 21:
  presults(dhand,phand)
  print("Sorry, you lose. The dealer got 21. \n")
elif total(phand) > 21:
  presults(dhand,phand)
  print("Sorry, you busted. You lose. \n")
elif total(dhand) > 21:
  presults(dhand,phand)
  print("Dealer busts. You win! \n")
elif total(phand) < total(dhand):
  presults(dhand,phand)
  print("Sorry, Your score is not higher than the dealer. You lose. \n")
elif total(phand) > total(dhand):
  presults(dhand,phand)
  print("Congratulations. Your score is higher than the dealer. You win. \n")
elif total(phand) == total(dhand):
  presults(dhand,phand)
  print("You and the dealer have the same total, this is a push. \n")
def game():
choice = 0
bid = 0
dhand = deal(deck)
phand = deal(deck)
winnings(bid)
while choice != "stay":
  print("The dealer is showing a " + str(dhand[0]))
  print("You have " + str(phand) + " for a total of " + str(total(phand)))
  choice = input("Do you want to [H]it, [S]plit, [P]ass, [D]ouble Down, or [Q]uit?").lower()
  if choice == "h":
    hit(phand)
    print("You now have " + str(phand) + " for a total of " + str(total(phand)))
    another = input("Do you need to [H]it again or [P]ass?").lower()
    while another != "p":
       if another == "h":
         hit(phand)
       elif another == "p":
         print("You passed")
    while total(dhand) < 17:
      hit(dhand)
    score(dhand,phand)
    play_again()
  elif choice == "p":
    while total(dhand) < 17:
      hit(dhand)
    score(dhand,phand)
    play_again()
  elif choice == "d":
    print("Double Down")
    score(dhand,phand)
    play_again()
  elif choice == "s":
    print("splitting")
    score(dhand,phand)
    play_again()
  else:
    print("bye!")
    exit()
 
 
 
game()
