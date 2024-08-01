#include <stdio.h>
#include <string.h>
#include <curses.h>
#include <time.h>
#include <stdbool.h>

void updateFile(int type);

struct Item
{
    char id[50];
    int price;
    char name[200];
    char category[100];
    int quantity;
} items[100];

struct Client
{
    char id[10];
    char firstName[50];
    char lastName[50];
    int phoneNumber;
} clients[100];

struct Detail
{
    char itemId[50];
    int quantity;
    int price;
    int total;
};

struct Sale
{
    struct Detail details[50];
    char clientId[10];
    int invoiceId;
    char date[30];
    int invoiceValue;
} sales[100];

struct Supply
{
    int id;
    char date[30];
    int invoiceValue;
    int totalQuantity;
    int totalAmount;
    struct Detail details[50];
} supplies[100];

struct ClientPurchaseCount
{
    char id[10];
    int purchaseCount;
} clientPurchaseCounts[100];

struct ClientExpenditure
{
    char id[10];
    int totalExpenditure;
} clientExpenditures[100];

int itemCount = 0;
int clientCount = 0;
int saleCount = 0;
int supplyCount = 0;

char itemFile[50] = "items.dat";
char supplyFile[50] = "supply.dat";
char clientFile[50] = "clients.dat";
char saleFile[50] = "sales.dat";

void loadItems()
{
    FILE *file = fopen(itemFile, "rb");
    if (file == NULL)
    {
        printf("\nError opening file");
        return;
    }

    itemCount = 0;
    while (fread(&items[itemCount], sizeof(struct Item), 1, file))
    {
        itemCount++;
    }
    fclose(file);
}

void loadClients()
{
    FILE *file = fopen(clientFile, "rb");
    if (file == NULL)
    {
        printf("\nError opening file");
        return;
    }

    clientCount = 0;
    while (fread(&clients[clientCount], sizeof(struct Client), 1, file))
    {
        clientCount++;
    }
    fclose(file);
}

void loadSupplies()
{
    FILE *file = fopen(supplyFile, "rb");
    if (file == NULL)
    {
        printf("\nError opening file");
        return;
    }

    supplyCount = 0;
    while (fread(&supplies[supplyCount], sizeof(struct Supply), 1, file))
    {
        supplyCount++;
    }
    fclose(file);
}

void loadSales()
{
    FILE *file = fopen(saleFile, "rb");
    if (file == NULL)
    {
        printf("\nError opening file");
        return;
    }

    saleCount = 0;
    while (fread(&sales[saleCount], sizeof(struct Sale), 1, file))
    {
        saleCount++;
    }
    fclose(file);
}

void updateClientPurchaseCount()
{
    for (int i = 0; i < clientCount; i++)
    {
        int purchases = 0;
        for (int j = 0; j < saleCount; j++)
        {
            if (strcmp(clients[i].id, sales[j].clientId) == 0)
            {
                purchases++;
            }
        }
        strcpy(clientPurchaseCounts[i].id, clients[i].id);
        clientPurchaseCounts[i].purchaseCount = purchases;
    }
}

void updateClientExpenditure()
{
    for (int i = 0; i < clientCount; i++)
    {
        int expenditure = 0;
        for (int j = 0; j < saleCount; j++)
        {
            if (strcmp(clients[i].id, sales[j].clientId) == 0)
            {
                expenditure += sales[j].invoiceValue;
            }
        }
        strcpy(clientExpenditures[i].id, clients[i].id);
        clientExpenditures[i].totalExpenditure = expenditure;
    }
}

void menu()
{
    printf("\n");
    printf("\n     Course Assignment in C");
    printf("\n");
    printf("\n 1 - Register Item");
    printf("\n 2 - Register Client");
    printf("\n 3 - Supply");
    printf("\n 4 - Sale");
    printf("\n 5 - Inventory Status");
    printf("\n 6 - Top 10 Clients by Expenditure");
    printf("\n 7 - Top 20 Clients by Purchase Frequency");
    printf("\n 8 - Search Item by Barcode");
    printf("\n r - Display General Statistics");
    printf("\n d - Exit");
    printf("\n Choice: ");
}

void registerItem()
{
    char id_temp[50];
    printf("\nEnter Item ID: ");
    scanf("%s", id_temp);

    if (strlen(id_temp) != 8)
    {
        printf("\nID must contain exactly 8 characters!");
        return;
    }

    for (int i = 0; i < itemCount; i++)
    {
        if (strcmp(id_temp, items[i].id) == 0)
        {
            printf("Registration cannot proceed because an item with this ID already exists in the system!");
            return;
        }
    }

    strcpy(items[itemCount].id, id_temp);

    printf("Enter Item Name: ");
    scanf(" %[^\n]%*c", items[itemCount].name);

    printf("Enter Category: ");
    scanf(" %[^\n]%*c", items[itemCount].category);

    printf("Enter Price: ");
    scanf("%d", &items[itemCount].price);
    items[itemCount].quantity = 0;

    itemCount++;
    updateFile(1);
}

void registerClient()
{
    char id_temp[10];
    sprintf(id_temp, "%d", clientCount + 1000);

    printf("\nGenerated Client ID: %s", id_temp);

    for (int i = 0; i < clientCount; i++)
    {
        if (strcmp(id_temp, clients[i].id) == 0)
        {
            printf("Registration cannot proceed because a client with this ID already exists in the system.");
            return;
        }
    }

    strcpy(clients[clientCount].id, id_temp);

    printf("\nEnter First Name: ");
    scanf(" %[^\n]%*c", clients[clientCount].firstName);

    printf("Enter Last Name: ");
    scanf(" %[^\n]%*c", clients[clientCount].lastName);

    printf("Enter Phone Number: ");
    scanf("%d", &clients[clientCount].phoneNumber);

    clientCount++;
    updateFile(3);
}

void supply()
{
    int id_temp = supplyCount + 1000;

    printf("\n\nGenerated Supply ID: %d", id_temp);
    supplies[supplyCount].id = id_temp;

    time_t t;
    time(&t);
    printf("\nSupply Date and Time: %s", ctime(&t));
    strcpy(supplies[supplyCount].date, ctime(&t));

    int itemsSupplied = 0, totalValue = 0;
    char itemId[50];
    int price, quantity;
    struct Detail temp;

    while (itemsSupplied < 50)
    {
        bool itemFound = false;
        printf("\n\n+ ITEM %d ", itemsSupplied + 1);
        printf("\nEnter the item code you are supplying: ");
        scanf("%s", itemId);

        for (int i = 0; i < itemCount; i++)
        {
            if (strcmp(itemId, items[i].id) == 0)
            {
                itemFound = true;
                break;
            }
        }

        if (!itemFound)
        {
            printf("\nItem with this ID does not exist!");
            return;
        }

        strcpy(temp.itemId, itemId);

        printf("Enter the purchase price for the item being supplied: ");
        scanf("%d", &price);
        temp.price = price;

        printf("Enter the quantity: ");
        scanf("%d", &quantity);
        temp.quantity = quantity;

        temp.total = quantity * price;

        totalValue += temp.total;
        printf("Total: %d", totalValue);

        for (int i = 0; i < itemCount; i++)
        {
            if (strcmp(itemId, items[i].id) == 0)
            {
                items[i].quantity += quantity;
                break;
            }
        }

        supplies[supplyCount].details[itemsSupplied] = temp;
        itemsSupplied++;

        printf("\nIf you want to close this invoice press P or any other key to continue");
        char ch = getchar();
        getchar();  // Consume newline
        if (ch == 'P' || ch == 'p')
            break;
    }

    printf("\nTotal invoice value: %d", totalValue);

    supplies[supplyCount].invoiceValue = totalValue;
    supplies[supplyCount].totalQuantity = itemsSupplied;
    supplies[supplyCount].totalAmount = totalValue;

    supplyCount++;
    updateFile(2);
    updateFile(1);
}

void sale()
{
    int id_temp = saleCount + 1000;

    sales[saleCount].invoiceId = id_temp;

    time_t t;
    time(&t);
    printf("\nSale Date and Time: %s", ctime(&t));
    strcpy(sales[saleCount].date, ctime(&t));

    printf("\nEnter the Client ID: ");
    char clientId[10];
    scanf("%s", clientId);

    bool clientFound = false;
    for (int i = 0; i < clientCount; i++)
    {
        if (strcmp(clientId, clients[i].id) == 0)
        {
            clientFound = true;
            break;
        }
    }

    if (!clientFound)
    {
        printf("\nClient ID not found!");
        return;
    }
    strcpy(sales[saleCount].clientId, clientId);

    int itemsSold = 0, totalValue = 0;
    char itemId[50];
    int price, quantity;
    struct Detail temp;

    while (itemsSold < 50)
    {
        bool itemFound = false;
        printf("\n\n+ ITEM %d ", itemsSold + 1);
        printf("\nEnter the item code: ");
        scanf("%s", itemId);

        for (int i = 0; i < itemCount; i++)
        {
            if (strcmp(itemId, items[i].id) == 0)
            {
                itemFound = true;
                break;
            }
        }

        if (!itemFound)
        {
            printf("\nItem with this ID does not exist!");
            return;
        }

        strcpy(temp.itemId, itemId);

        printf("Enter the sale price for the item: ");
        scanf("%d", &price);
        temp.price = price;

        printf("Enter the quantity: ");
        scanf("%d", &quantity);
        temp.quantity = quantity;

        temp.total = quantity * price;

        totalValue += temp.total;
        printf("Total: %d", totalValue);

        for (int i = 0; i < itemCount; i++)
        {
            if (strcmp(itemId, items[i].id) == 0)
            {
                if (items[i].quantity < quantity)
                {
                    printf("\nNot enough quantity in stock!");
                    return;
                }
                items[i].quantity -= quantity;
                break;
            }
        }

        sales[saleCount].details[itemsSold] = temp;
        itemsSold++;

        printf("\nIf you want to close this invoice press P or any other key to continue");
        char ch = getchar();
        getchar();  // Consume newline
        if (ch == 'P' || ch == 'p')
            break;
    }

    printf("\nTotal invoice value: %d", totalValue);

    sales[saleCount].invoiceValue = totalValue;
    saleCount++;
    updateFile(4);
    updateFile(1);
}

void inventoryStatus()
{
    printf("\n\nList of Items in Inventory");
    printf("\nItem ID\tName\tCategory\tQuantity\tPrice");
    for (int i = 0; i < itemCount; i++)
    {
        printf("\n%s\t%s\t%s\t%d\t%d", items[i].id, items[i].name, items[i].category, items[i].quantity, items[i].price);
    }
}

void top10ClientsByExpenditure()
{
    updateClientExpenditure();
    
    // Bubble sort clients by expenditure in descending order
    for (int i = 0; i < clientCount - 1; i++)
    {
        for (int j = 0; j < clientCount - i - 1; j++)
        {
            if (clientExpenditures[j].totalExpenditure < clientExpenditures[j + 1].totalExpenditure)
            {
                struct ClientExpenditure temp = clientExpenditures[j];
                clientExpenditures[j] = clientExpenditures[j + 1];
                clientExpenditures[j + 1] = temp;
            }
        }
    }

    printf("\nTop 10 Clients by Expenditure");
    printf("\nClient ID\tTotal Expenditure");
    for (int i = 0; i < 10 && i < clientCount; i++)
    {
        printf("\n%s\t%d", clientExpenditures[i].id, clientExpenditures[i].totalExpenditure);
    }
}

void top20ClientsByPurchaseFrequency()
{
    updateClientPurchaseCount();
    
    // Bubble sort clients by purchase frequency in descending order
    for (int i = 0; i < clientCount - 1; i++)
    {
        for (int j = 0; j < clientCount - i - 1; j++)
        {
            if (clientPurchaseCounts[j].purchaseCount < clientPurchaseCounts[j + 1].purchaseCount)
            {
                struct ClientPurchaseCount temp = clientPurchaseCounts[j];
                clientPurchaseCounts[j] = clientPurchaseCounts[j + 1];
                clientPurchaseCounts[j + 1] = temp;
            }
        }
    }

    printf("\nTop 20 Clients by Purchase Frequency");
    printf("\nClient ID\tPurchase Frequency");
    for (int i = 0; i < 20 && i < clientCount; i++)
    {
        printf("\n%s\t%d", clientPurchaseCounts[i].id, clientPurchaseCounts[i].purchaseCount);
    }
}

void searchItemByBarcode()
{
    char barcode[50];
    printf("\nEnter Item Barcode: ");
    scanf("%s", barcode);

    bool itemFound = false;
    for (int i = 0; i < itemCount; i++)
    {
        if (strcmp(barcode, items[i].id) == 0)
        {
            printf("\nItem found:");
            printf("\nItem ID: %s", items[i].id);
            printf("\nName: %s", items[i].name);
            printf("\nCategory: %s", items[i].category);
            printf("\nPrice: %d", items[i].price);
            printf("\nQuantity: %d", items[i].quantity);
            itemFound = true;
            break;
        }
    }

    if (!itemFound)
    {
        printf("\nItem not found!");
    }
}

void displayGeneralStatistics()
{
    printf("\nGeneral Statistics:");
    printf("\nTotal Items: %d", itemCount);
    printf("\nTotal Clients: %d", clientCount);
    printf("\nTotal Sales: %d", saleCount);
    printf("\nTotal Supplies: %d", supplyCount);
}

void updateFile(int type)
{
    FILE *file;

    switch (type)
    {
    case 1:
        file = fopen(itemFile, "wb");
        fwrite(items, sizeof(struct Item), itemCount, file);
        break;
    case 2:
        file = fopen(supplyFile, "wb");
        fwrite(supplies, sizeof(struct Supply), supplyCount, file);
        break;
    case 3:
        file = fopen(clientFile, "wb");
        fwrite(clients, sizeof(struct Client), clientCount, file);
        break;
    case 4:
        file = fopen(saleFile, "wb");
        fwrite(sales, sizeof(struct Sale), saleCount, file);
        break;
    default:
        printf("\nUnknown file type.");
        break;
    }

    fclose(file);
}

int main()
{
    int choice;

    loadItems();
    loadClients();
    loadSupplies();
    loadSales();

    while (1)
    {
        menu();
        choice = getchar();
        getchar();  // Consume newline

        switch (choice)
        {
        case '1':
            registerItem();
            break;
        case '2':
            registerClient();
            break;
        case '3':
            supply();
            break;
        case '4':
            sale();
            break;
        case '5':
            inventoryStatus();
            break;
        case '6':
            top10ClientsByExpenditure();
            break;
        case '7':
            top20ClientsByPurchaseFrequency();
            break;
        case '8':
            searchItemByBarcode();
            break;
        case 'r':
            displayGeneralStatistics();
            break;
        case 'd':
            return 0;
        default:
            printf("\nInvalid choice. Please try again.");
        }
    }

    return 0;
}
