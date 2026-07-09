
Coffee Machine [README.md](https://github.com/user-attachments/files/29829133/README.md)
# Coffee Machine

A command-line coffee vending machine simulator written in Java. The program models a simplified real-world machine that tracks its own resources — water, milk, coffee beans, cups, and cash — and responds to user commands the same way a physical machine would.

## Features

The machine supports six actions, entered as plain text commands at the prompt:

| Command | Description |
|---|---|
| `buy` | Purchase espresso, latte, or cappuccino. Checks water, milk, beans, and cups before brewing; reports the specific missing ingredient if it can't fulfill the order. |
| `fill` | Add water, milk, coffee beans, and cups to the machine. |
| `take` | Collect all cash the machine has earned, resetting the till to zero. |
| `clean` | Clean the machine, resetting the cleaning counter. |
| `remaining` | Display current water, milk, beans, cups, and money. |
| `exit` | Stop the program. |

### Cleaning requirement

The machine tracks a cleaning counter that decreases by 1 each time a coffee is made. Once it reaches 0, the machine refuses to brew any further coffee — printing `I need cleaning!` immediately, without even showing the drink menu — until `clean` is run.

## Recipes

| Drink | Water (ml) | Milk (ml) | Beans (g) | Price ($) |
|---|---|---|---|---|
| Espresso | 250 | 0 | 16 | 4 |
| Latte | 350 | 75 | 20 | 7 |
| Cappuccino | 200 | 100 | 12 | 6 |

## Starting resources

- 400 ml water
- 540 ml milk
- 120 g coffee beans
- 9 disposable cups
- $550 cash
- Cleaning counter: 10

## Design & architecture

The program is organized into three classes to separate data, state, and control:

- **`Coffee`** — An immutable value class representing a single drink recipe (name, water, milk, beans, price). Static factory methods (`espresso()`, `latte()`, `cappuccino()`) construct the three supported drinks, keeping ingredient numbers defined in one place rather than scattered through conditional logic.
- **`CoffeeMachine`** — Owns all mutable machine state (water, milk, beans, cups, money, cleaning counter) and implements the user-facing actions (`buy`, `fill`, `take`, `clean`, `remaining`). A private helper method, `makeCoffee(Coffee)`, contains the shared ingredient-check-and-deduct logic used across all three drink types, so `buy()` doesn't repeat that logic per drink.
- **`Main`** — The entry point. Constructs a `CoffeeMachine` and starts its input loop.

This structure keeps each class responsible for one thing: `Coffee` describes a drink, `CoffeeMachine` manages state and behavior, and `Main` just boots the program — making it straightforward to add new drinks or extend machine behavior without duplicating logic.

## Running the program

```bash
javac machine/*.java
java machine.Main
```

## Example session

```
Write action (buy, fill, take, clean, remaining, exit):
buy
What do you want to buy? 1 - espresso, 2 - latte, 3 - cappuccino, back - to main menu:
1
I have enough resources, making you a coffee!

Write action (buy, fill, take, clean, remaining, exit):
remaining
The coffee machine has:
150 ml of water
540 ml of milk
104 g of coffee beans
8 disposable cups
$554 of money

Write action (buy, fill, take, clean, remaining, exit):
exit
```
