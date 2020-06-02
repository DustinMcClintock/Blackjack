#Blackjack
#Playing the game 21 as close as possible to the table game.

import random
import os
deck = [2,3,4,5,6,7,8,9,10,11,12,13,14]*4
print("Welcome to BLACKJACK \n RULES: \n 1) You can only split one time \n 2) Blackjack pays 3/2 \n 3) You cannot hit on split Aces \n 4) Have FUN \n")
money = int(input("How much money would you like to play with? $"))

def play_again():
  again = input("Do you want to play again? [Y]es/[N]o :").lower()
  if again == "y":
    dhand = []
    phand =[]
    deck = [2,3,4,5,6,7,8,9,10,11,12,13,14]*400
    clear()
    game()
  else:
    print("Bye! Your winnings are: $" + str(money))
    exit()

def clear():
  if os.name == 'nt':
    os.system('CLS')
  if os.name == 'posix':
    os.system('clear')
  print("Welcome to BLACKJACK \n RULES: \n 1) You can only split one time \n 2) Blackjack pays 3/2 \n 3) You cannot hit on split Aces \n 4) Have FUN \n")

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

def total(hand):
  total = 0
  NT = 0
  for card in hand:
    if card == "J" or card == "Q" or card == "K":
      total+= 10
    elif card == "A":
      total+= 11
    else:
      total += card
  NT = total
  for card in hand: 
    if card == "A" and total > 21:
      total = NT - 10
      return total
    else:
      total = total
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
  print("\nThe dealer has a " + str(dhand) + "for a total of " + str(total(dhand)))
  print("You have a " + str(phand) + " for a total of " + str(total(phand)))

def score(dhand,phand,bid):
  global money
  if total(phand) == 21:
    presults(dhand,phand)
    money = money + bid
    print("\nCongratulations! You got 21! You now have $ " +str(money) + "\n")
  elif total(dhand) == 21:
    presults(dhand,phand)
    money = money - bid
    print("\nSorry, you lose. The dealer got 21. You now have $ " + str(money) + "\n")
  elif total(phand) > 21:
    presults(dhand,phand)
    money = money - bid
    print("\nSorry, you busted. You lose. You now have $ " + str(money) + "\n")
  elif total(dhand) > 21:
    presults(dhand,phand)
    money = money + bid
    print("\nDealer busts. You win! You now have $" + str(money) + "\n")
  elif total(phand) < total(dhand):
    presults(dhand,phand)
    money = money - bid
    print("\nSorry, Your score is not higher than the dealer. You lose. You now have $" +str(money) + "\n")
  elif total(phand) > total(dhand):
    presults(dhand,phand)
    money = money + bid
    print("\nCongratulations. Your score is higher than the dealer. You win. You now have $" + str(money) + "\n")
  elif total(phand) == total(dhand):
    presults(dhand,phand)
    money = money
    print("\nYou and the dealer have the same total, this is a push. You now have $ " + str(money) + "\n")
    
 
def game():
  choice = bid = bid1 = bid2 = answer = again = 0
  global money
  dhand = deal(deck)
  phand = deal(deck)
  print("You have $" + str(money))
  bid = int(input("How much would you like to bid? $" )) #Why does restarting keep old bid?
  if bid > money:
    print("You do not have that much money, we will start again")
    game()
  if bid <= money:
    bid = bid
  while choice != "stay":
    print(" \nThe dealer is showing a " + str(dhand[0]))
    print("You have " + str(phand) + " for a total of " + str(total(phand)) + "\n")
    if str(dhand[0]) == "A":
      ins = input("Would you like to buy insurance [Y]es/[N]o? ").lower()
      if ins == "n" and total(dhand) == 21:
        print("Dealer has BLACKJACK and you do not have insurance")
        score(dhand,phand,bid)
        play_again()
      if ins == "y" and total(dhand) == 21:
        bid = int(input("How much would you like to bet the dealer has BLACKJACK? " ))
        print("Dealer has BLACKJACK and you have insurance")
        money = money + bid + bid
        print("You won...kinda, your new total is " + str(money))
        play_again()
      if ins =="y" and total(dhand) != 21:
        bid = int(input("How much would you like to bet the dealer has BLACKJACK? " )) 
        money = money - bid
        print("Dealer does not have blackjack")
        print("\nYou now have $ " +str(money))
        print("The dealer is showing a " + str(dhand[0]))
        print("You have " + str(phand) + " for a total of " + str(total(phand)) + "\n")
      else:
        print("\nDealer does not have blackjack")
        print("You now have $ " +str(money))
        print("The dealer is showing a " + str(dhand[0]))
        print("You have " + str(phand) + " for a total of " + str(total(phand)) + "\n")
    if total(phand) == 21:
      bid = (1.5 * bid)
      score(dhand,phand,bid)
      play_again()
    choice = input("Do you want to [H]it, [S]plit, [P]ass, [D]ouble Down or [Q]uit? ").lower()
    if choice == "h" and total(phand) < 21:
      while choice == "h" and total(phand) < 21:
        hit(phand)
        print("\nYou now have " + str(phand) + " for a total of " + str(total(phand)))
        if total(phand) > 21:
          score(dhand,phand,bid)
          play_again()
        else:
          choice = input("\nDo you need to [H]it again or [P]ass? ").lower()
      while total(dhand) < 17:
        hit(dhand)
      score(dhand,phand,bid)
      play_again()
    elif choice == "p":
      while total(dhand) < 17:
        hit(dhand)
      score(dhand,phand,bid)
      play_again()
    elif choice == "d":
      print("Double Down")
      bid = bid + bid
      hit(phand)
      score(dhand,phand,bid)
      play_again()
    elif choice == "s" and phand[0] == phand[1]:
      print("Splitting " + str(phand[0]) + "\'s \n")
      phand1 = [phand[0]]
      phand2 = [phand[0]]
      bid = bid1 = bid2
      hit(phand1)
      #First split
      print("Your first split is " + str(phand1) + " for a total of " + str(total(phand1)))
      print("The dealer is showing a " + str(dhand[0]))
      answer = input("Do you want to [H]it, [P]ass, [D]ouble Down? ").lower()
      if answer == "h" and total(phand1) < 21:
        while answer == "h" and total(phand1) < 21:
          hit(phand1)
          print("You now have " + str(phand1) + " for a total of " + str(total(phand1)))
          if total(phand) > 21:
            while total(dhand) < 17:
              hit(dhand)
          else:
            answer = input("Do you need to [H]it again or [P]ass? ").lower()
        while total(dhand) < 17:
          hit(dhand)
      elif answer == "p":
        while total(dhand) < 17:
          hit(dhand)
      elif answer == "d":
        print("Double Down")
        bid = bid1 + bid1
        hit(phand1)
      hit(phand2)

      #second split
      print("\nYour second split is " + str(phand2) + " for a total of " + str(total(phand2)))
      print("The dealer is showing a " + str(dhand[0]))
      again = input("Do you want to [H]it, [P]ass, [D]ouble Down? \n").lower()
      if again == "h" and total(phand2) < 21:
        while again == "h" and total(phand2) < 21:
          hit(phand2)
          print("You now have " + str(phand2) + " for a total of " + str(total(phand2)))
          if total(phand) > 21:
            while total(dhand) < 17:
              hit(dhand)
          else:
            again = input("Do you need to [H]it again or [P]ass? ").lower()
        while total(dhand) < 17:
          hit(dhand)
      elif again == "p":
        while total(dhand) < 17:
          hit(dhand)
      elif again == "d":
        print("Double Down")
        bid = bid2 + bid2
        hit(phand2)
      score(dhand,phand1,bid1) #Why doesnt money get updated each time?
      score(dhand,phand2,bid2)
      play_again()
    else:
      print("bye!")
      exit()

game()
