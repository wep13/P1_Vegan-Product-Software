# BioMarket — Inventory & Sales Management CLI

A simple command-line inventory management system built in Python for **BioMarket s.a.s.**, a vegan products store. The program lets users manage stock, register sales, and track gross/net profit, with all data persisted to CSV files between sessions.

## Features

1. **Add products** — add a new product with name, quantity, selling price, and buying price
2. **List inventory** — view all products currently in stock
3. **Register sales** — record a sale for an existing product
4. **Track profit** — view cumulative gross and net profit from all sales
5. **Help menu** — display all available commands

## How it works

- The program runs an interactive command loop in the console: the user types a command, the corresponding action runs, and the loop repeats until `close` is entered.
- **Persistence**: inventory data is stored in `inventory.csv`; sales data is appended to `profits.csv`. Both files are created automatically on first run if they don't already exist.
- **Input validation**: all numeric inputs (quantity, prices) are validated through the `is_positive()` helper, which loops until the user enters a valid non-negative number, converting commas to decimal points for locale flexibility. Invalid commands are caught and rejected via an `assert` check against a whitelist.
- **Adding logic**: when adding a product, the program checks if it already exists in the inventory (case-insensitive match). If it does, the new quantity is simply added to the existing stock. If not, the user is prompted for selling and buying price and a new row is created.
- **Selling logic**: when registering a sale, the program checks the product exists and that enough quantity is available (raises an assertion error otherwise). It then computes gross profit (`quantity × sell price`) and net profit (`gross profit − quantity × buy price`) and appends the result to `profits.csv`. Note: the sold quantity is **not** subtracted from the inventory stock.

## Commands

| Command | Description |
|---|---|
| `add` | Add a product to the inventory (or increase its quantity if it already exists) |
| `inventory_list` | Show the full list of products in the inventory |
| `sell` | Register a sale for a product |
| `profit` | Show total gross and net profit accumulated so far |
| `help` | Show the list of available commands |
| `close` | Exit the program |

## Data files

### `inventory.csv`
| Column | Description |
|---|---|
| `Item` | Product name |
| `Quantity` | Quantity currently in stock |
| `Sell Price` | Unit selling price |
| `Buy Price` | Unit buying (cost) price |

### `profits.csv`
| Column | Description |
|---|---|
| `Gross Profit` | Gross profit from a single sale |
| `Net Profit` | Net profit from a single sale (gross profit minus cost) |

## Function reference

| Function | Purpose |
|---|---|
| `help()` | Returns a string listing all available commands |
| `is_positive()` | Prompts the user for input and loops until a valid positive number is entered |
| `inventory_list()` | Reads and prints the contents of `inventory.csv` |
| `add()` | Adds a new product or increases the quantity of an existing one |
| `sell()` | Registers a sale, validates stock availability, and logs profit |
| `profit()` | Reads `profits.csv` and returns total gross and net profit |

## Dependencies

Only Python standard library modules are used:

```
csv
os
```

## How to run it

The notebook is designed to run on **Google Colab**, but since it only relies on the standard library, it can be run anywhere Python 3 is installed — as a notebook or converted into a standalone `.py` script. Simply run all cells (or the script) and interact with the program via the console prompts.

```bash
python biomarket.py
```

## Known limitations

- Selling a product does not decrease its quantity in `inventory.csv` — the inventory count stays static after a sale is registered.
- When updating the quantity of an existing product via `add()`, the selling and buying prices cannot be updated — only the original values are kept.
- Product matching is case-insensitive on input, but the case originally used for `add` is what gets stored.
