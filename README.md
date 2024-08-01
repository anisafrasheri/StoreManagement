# Inventory Management System

## Description

The Inventory Management System is a C application designed to manage inventory, clients, supplies, and sales. It provides functionalities for registering items and clients, processing supplies and sales, viewing inventory status, and generating reports on client expenditures and purchase frequencies.

## Features

- **Register Items**: Add new items with unique IDs, names, categories, prices, and quantities.
- **Register Clients**: Add new clients with unique IDs, names, and phone numbers.
- **Supply Management**: Record supplies, including details on purchased items and total invoice value.
- **Sales Management**: Process sales transactions, updating inventory and generating sales reports.
- **Inventory Status**: View the current status of all items in inventory.
- **Top Clients Reports**: Generate reports on top clients by expenditure and purchase frequency.
- **Search Items**: Find items by barcode.
- **General Statistics**: Display overall statistics of the inventory system.

## Getting Started

### Prerequisites

- C Compiler (I used DEV C++)

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/anisafrasheri/StoreManagement.git

2. Navigate to the project directory:
   ```bash
   cd inventory-management-system


4. Compile the code:
   ```bash
   gcc -o StoreSourceCode main.c -lcurses


6. Run the application:
   ```bash
   ./StoreSourceCode



## Usage
Register Item: Option 1 to add new items.
Register Client: Option 2 to add new clients.
Supply: Option 3 to manage item supplies.
Sale: Option 4 to process sales transactions.
Inventory Status: Option 5 to view current inventory.
Top Clients by Expenditure: Option 6 to see top 10 clients based on spending.
Top Clients by Purchase Frequency: Option 7 to view top 20 clients based on purchase frequency.
Search Item by Barcode: Option 8 to search for items by barcode.
Display General Statistics: Option r to view total items, clients, sales, and supplies.
Exit: Option d to close the application.


## License
This project is licensed under the MIT License. See the LICENSE file for details.
