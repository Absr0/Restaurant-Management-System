# Restaurant-Management-System
#include <iostream>
#include <vector>
#include <iomanip>
#include <fstream>
#include <algorithm>
#include <string>

using namespace std;

struct MenuItem {
    string name;
    double price;
    string category;
};

// Global menu
vector<MenuItem> menu;

void initializeMenu() {
    MenuItem item1 = {"kitfo", 300, "habesha"};
    MenuItem item2 = {"shero", 150, "fast food"};
    MenuItem item3 = {"burger", 250, "fast food"};
    MenuItem item4 = {"Salad", 420, "Healthy"};
    menu.push_back(item1);
    menu.push_back(item2);
    menu.push_back(item3);
    menu.push_back(item4);
}

// Save to file
void saveToFile() {
    ofstream file("menu.txt");
    if (file.is_open()) {
        for (size_t i = 0; i < menu.size(); ++i) {
            file << menu[i].name << "," << menu[i].price << "," << menu[i].category << endl;
        }
        file.close();
    }
}

// View all items
void viewMenu() {
    cout << left << setw(20) << "Name" << setw(10) << "Price" << "Category" << endl;
    cout << "-------------------------------------------" << endl;
    for (size_t i = 0; i < menu.size(); ++i) {
        cout << left << setw(20) << menu[i].name << setw(10) << menu[i].price << menu[i].category << endl;
    }
}

// Search item
void searchItem() {
    string keyword;
    cout << "Enter name to search: ";
    cin >> keyword;
    bool found = false;
    for (size_t i = 0; i < menu.size(); ++i) {
        if (menu[i].name == keyword) {
            cout << "Found: " << menu[i].name << " - $" << menu[i].price << " - " << menu[i].category << endl;
            found = true;
        }
    }
    if (!found) {
        cout << "Item not found." << endl;
    }
}

// Add new item
void addItem() {
    MenuItem item;
    cout << "Enter name: ";
    cin >> item.name;
    cout << "Enter price: ";
    cin >> item.price;
    cout << "Enter category: ";
    cin >> item.category;
    menu.push_back(item);
    saveToFile();
    cout << "Item added!" << endl;
}

// Update item
void updateItem() {
    string name;
    cout << "Enter item name to update: ";
    cin >> name;
    for (size_t i = 0; i < menu.size(); ++i) {
        if (menu[i].name == name) {
            cout << "New name: ";
            cin >> menu[i].name;
            cout << "New price: ";
            cin >> menu[i].price;
            cout << "New category: ";
            cin >> menu[i].category;
            saveToFile();
            cout << "Item updated." << endl;
            return;
        }
    }
    cout << "Item not found." << endl;
}

// Delete item
void deleteItem() {
    string name;
    cout << "Enter item name to delete: ";
    cin >> name;
    bool found = false;
    for (vector<MenuItem>::iterator it = menu.begin(); it != menu.end(); ++it) {
        if (it->name == name) {
            menu.erase(it);
            saveToFile();
            cout << "Item deleted." << endl;
            found = true;
            break;
        }
    }
    if (!found) {
        cout << "Item not found." << endl;
    }
}

// Menu options
void showMenu() {
    int choice;
    do {
        cout << "\n--- Restaurant Menu System ---\n";
        cout << "1. View Menu\n2. Search Item\n3. Add Item\n4. Update Item\n5. Delete Item\n6. Exit\n";
        cout << "Choose an option: ";
        cin >> choice;

        switch (choice) {
            case 1: viewMenu(); break;
            case 2: searchItem(); break;
            case 3: addItem(); break;
            case 4: updateItem(); break;
            case 5: deleteItem(); break;
            case 6: cout << "Exiting...\n"; break;
            default: cout << "Invalid choice.\n";
        }
    } while (choice != 6);
}

int main() {
    initializeMenu();
    showMenu();
    return 0;
}
