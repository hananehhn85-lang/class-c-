#include <iostream>
#include <fstream>
#include <vector>
#include <string>

using namespace std;

class Customer
{
protected:
    string customerName;
};

class Order : public Customer
{
private:
    string foodName;
    double price;
    int quantity;
    double totalPrice;

public:

    void addOrder()
    {
        cout << "\nCustomer Name: ";
        cin >> customerName;

        int choice;

        cout << "\n===== FOOD MENU =====\n";
        cout << "1. Pizza      - 250\n";
        cout << "2. Burger     - 180\n";
        cout << "3. Pasta      - 220\n";
        cout << "4. Salad      - 120\n";
        cout << "5. Sandwich   - 150\n";

        cout << "\nSelect Food Number: ";
        cin >> choice;

        switch(choice)
        {
        case 1:
            foodName = "Pizza";
            price = 250;
            break;

        case 2:
            foodName = "Burger";
            price = 180;
            break;

        case 3:
            foodName = "Pasta";
            price = 220;
            break;

        case 4:
            foodName = "Salad";
            price = 120;
            break;

        case 5:
            foodName = "Sandwich";
            price = 150;
            break;

        default:
            cout << "Invalid Choice!\n";
            foodName = "Unknown";
            price = 0;
        }

        cout << "Quantity: ";
        cin >> quantity;

        totalPrice = price * quantity;
    }

    friend void saveOrderToFile(Order o);
    friend class RestaurantManager;
};

class RestaurantManager
{
public:

    void showOrder(Order o)
    {
        cout << "\n-------------------------\n";
        cout << "Customer : " << o.customerName << endl;
        cout << "Food     : " << o.foodName << endl;
        cout << "Price    : " << o.price << endl;
        cout << "Quantity : " << o.quantity << endl;
        cout << "Total    : " << o.totalPrice << endl;
        cout << "-------------------------\n";
    }
};

void saveOrderToFile(Order o)
{
    ofstream file("orders.txt", ios::app);

    file << o.customerName << " "
         << o.foodName << " "
         << o.price << " "
         << o.quantity << " "
         << o.totalPrice << endl;

    file.close();
}

void readOrders()
{
    ifstream file("orders.txt");

    string line;

    while(getline(file, line))
    {
        cout << line << endl;
    }

    file.close();
}

void showFoodMenu()
{
    cout << "\n===== FOOD MENU =====\n";
    cout << "1. Pizza      - 250\n";
    cout << "2. Burger     - 180\n";
    cout << "3. Pasta      - 220\n";
    cout << "4. Salad      - 120\n";
    cout << "5. Sandwich   - 150\n";
}

int main()
{
    vector<Order> orders;
    RestaurantManager manager;

    int choice;

    do
    {
        cout << "\n========== RESTAURANT MENU ==========\n";
        cout << "1. Show Food Menu\n";
        cout << "2. Add New Order\n";
        cout << "3. Show Current Orders\n";
        cout << "4. Exit\n";

        cout << "\nEnter Choice: ";
        cin >> choice;

        switch(choice)
        {
        case 1:
            showFoodMenu();
            break;

        case 2:
        {
            Order newOrder;

            newOrder.addOrder();

            orders.push_back(newOrder);

            saveOrderToFile(newOrder);

            cout << "\nOrder Added Successfully.\n";
            cout << "Order Saved To File Successfully.\n";

            break;
        }

        case 3:
        {
            if(orders.empty())
            {
                cout << "\nNo Orders Available.\n";
            }
            else
            {
                for(int i = 0; i < orders.size(); i++)
                {
                    manager.showOrder(orders[i]);
                }
            }

            break;
        }

        case 4:
            cout << "\nProgram Ended.\n";
            break;

        default:
            cout << "\nInvalid Choice!\n";
        }

    } while(choice != 4);

    return 0;
}
