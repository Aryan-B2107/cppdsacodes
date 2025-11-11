cppdsacodes
______________________________________________________________________________________________________
SEARCHING CODES : 
```
#include <iostream>
#include <algorithm>
using namespace std;

// Linear Search

int linearSearch(int arr[], int n, int target) {
    for (int i = 0; i < n; i++) {
        if (arr[i] == target) {
            return i;
        }
    }
    return -1;
}

// Binary Search
int binarySearch(int arr[], int n, int target) {
    int left = 0;
    int right = n - 1;
    
    while (left <= right) {

        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target) {
            return mid;
        }
        
        if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return -1;
}

int main() {
    int customerIDs[] = {105, 203, 150, 401, 299};
    int n = 5;
    int searchID = 401;
    
    // Linear Search
    cout << "Linear Search:" << endl;
    int result = linearSearch(customerIDs, n, searchID);
    if (result != -1) {
        cout << "Found at index " << result << endl;
    } else {
        cout << "Not found" << endl;
    }
    
    // Sort array for binary search
    sort(customerIDs, customerIDs + n);
    
    // Binary Search
    cout << "\nBinary Search:" << endl;
    result = binarySearch(customerIDs, n, searchID);
    if (result != -1) {
        cout << "Found at index " << result << endl;
    } else {
        cout << "Not found" << endl;
    }
    
    return 0;
}
```
______________________________________________________________________________________________________
HashTABLE Implementation using Chaining as resolution of size 10:
```
#include <iostream>
using namespace std;

struct Node {
    int key;
    int value;
    Node* next;

    Node(int k, int v) {
        key = k;
        value = v;
        next = NULL;
    }
};

class HashTable {
    Node* table[10];   // Array of linked-list heads

public:
    HashTable() {
        for (int i = 0; i < 10; i++)
            table[i] = NULL;
    }

    int hashFunction(int key) {
        return key % 10;
    }

    void insertKey(int key, int value) {
        int index = hashFunction(key);
        Node* temp = table[index];

        // Check if key already exists
        while (temp != NULL) {
            if (temp->key == key) {
                temp->value = value;   // Update value
                return;
            }
            temp = temp->next;
        }

        // Key not found -> insert at head
        Node* newNode = new Node(key, value);
        newNode->next = table[index];
        table[index] = newNode;
    }

    int searchKey(int key) {
        int index = hashFunction(key);
        Node* temp = table[index];

        while (temp != NULL) {
            if (temp->key == key)
                return temp->value;
            temp = temp->next;
        }
        return -1;  // Not found
    }

    bool deleteKey(int key) {
        int index = hashFunction(key);
        Node* temp = table[index];
        Node* prev = NULL;

        while (temp != NULL) {
            if (temp->key == key) {
                if (prev == NULL)
                    table[index] = temp->next;   // Deleting head node
                else
                    prev->next = temp->next;     // Middle or last node

                delete temp;
                return true;
            }
            prev = temp;
            temp = temp->next;
        }
        return false;
    }

    void display() {
        for (int i = 0; i < 10; i++) {
            cout << i << ": ";
            Node* temp = table[i];
            while (temp != NULL) {
                cout << "(" << temp->key << "," << temp->value << ") ";
                temp = temp->next;
            }
            cout << endl;
        }
    }
};

int main() {
    HashTable ht;

    ht.insertKey(10, 100);
    ht.insertKey(20, 200);
    ht.insertKey(25, 250); // collision with index 5
    ht.insertKey(15, 150); // collision with index 5

    cout << "Hash Table:\n";
    ht.display();

    cout << "Search 25 -> " << ht.searchKey(25) << endl;

    ht.deleteKey(15);

    cout << "After Deletion:\n";
    ht.display();

    return 0;
}
```
Student Management System CODE
DLL CODE:

```
#include <iostream>
#include <string>
using namespace std;

struct Node {
    int roll;
    string name;
    float marks;
    Node* prev;
    Node* next;

    Node(int r, string n, float m) {
        roll = r;
        name = n;
        marks = m;
        prev = nullptr;
        next = nullptr;
    }
};

class DoublyList {
private:
    Node* head;

public:
    DoublyList() {
        head = nullptr;
    }

    void add(int roll, string name, float marks) {
        Node* temp = new Node(roll, name, marks);

        if (head == nullptr) {
            head = temp;
        }
        else {
            Node* current = head;
            while (current->next != nullptr) {
                current = current->next;
            }
            current->next = temp;
            temp->prev = current;
        }
    }

    Node* search(int roll) {
        Node* current = head;
        while (current != nullptr) {
            if (current->roll == roll) {
                return current;
            }
            current = current->next;
        }
        return nullptr;
    }

    void update(int roll, string newName, float newMarks) {
        Node* temp = search(roll);
        if (temp == nullptr) {
            cout << "Record not found\n";
        }
        else {
            temp->name = newName;
            temp->marks = newMarks;
        }
    }

    void deleteRecord(int roll) {
        Node* temp = search(roll);

        if (temp == nullptr) {
            cout << "Record not found\n";
            return;
        }

        if (temp->prev != nullptr) {
            temp->prev->next = temp->next;
        }
        else {
            head = temp->next;
        }

        if (temp->next != nullptr) {
            temp->next->prev = temp->prev;
        }

        delete temp;
    }

    void sortAscending(bool sortByMarks = false) {
        Node* i = head;

        while (i != nullptr) {
            Node* j = i->next;
            while (j != nullptr) {
                bool condition;

                if (sortByMarks == true) {
                    condition = (i->marks > j->marks);
                } 
                else {
                    condition = (i->roll > j->roll);
                }

                if (condition == true) {
                    int tempRoll = i->roll;
                    string tempName = i->name;
                    float tempMarks = i->marks;

                    i->roll = j->roll;
                    i->name = j->name;
                    i->marks = j->marks;

                    j->roll = tempRoll;
                    j->name = tempName;
                    j->marks = tempMarks;
                }

                j = j->next;
            }
            i = i->next;
        }
    }

    void display() {
        Node* current = head;
        while (current != nullptr) {
            cout << current->roll << " " 
                 << current->name << " "
                 << current->marks << "\n";

            current = current->next;
        }
    }
};

int main() {
    DoublyList dl;

    dl.add(1, "Aryan", 90);
    dl.add(2, "Ravi", 82);
    dl.add(3, "Nikita", 95);

    dl.sortAscending(true);
    dl.display();
}
```
__________________________________________________________________________________________________________
CALL CENTER QUEUE:
```
#include <iostream>
using namespace std;

struct Node {
    int customerID;
    int callTime;
    Node* next;

    Node(int id, int time) {
        customerID = id;
        callTime = time;
        next = nullptr;
    }
};

class CallQueue {
private:
    Node* front;
    Node* rear;

public:
    CallQueue() {
        front = nullptr;
        rear = nullptr;
    }

    void addCall(int customerID, int callTime) {
        Node* temp = new Node(customerID, callTime);

        if (front == nullptr) {
            front = temp;
            rear = temp;
        }
        else {
            rear->next = temp;
            rear = temp;
        }
    }

    void answerCall() {
        if (front == nullptr) {
            cout << "No calls in the queue\n";
            return;
        }

        Node* temp = front;
        front = front->next;

        cout << "Answering Call - Customer ID: "
             << temp->customerID << ", Time: "
             << temp->callTime << " min\n";

        delete temp;

        if (front == nullptr) {
            rear = nullptr;
        }
    }

    void viewQueue() {
        if (front == nullptr) {
            cout << "No calls in the queue\n";
            return;
        }

        Node* current = front;
        while (current != nullptr) {
            cout << "Customer ID: " << current->customerID
                 << ", Time: " << current->callTime << " min\n";
            current = current->next;
        }
    }

    bool isQueueEmpty() {
        return front == nullptr;
    }
};

int main() {
    CallQueue q;

    q.addCall(101, 5);
    q.addCall(102, 3);
    q.addCall(103, 8);

    q.viewQueue();
    q.answerCall();
    q.answerCall();
    q.viewQueue();

    if (q.isQueueEmpty()) {
        cout << "Queue is empty\n";
    }
    else {
        cout << "Queue is not empty\n";
    }
}
```
___________________________________________________________________________________________________
PIZZA SHOP MST:
```
#include <iostream>
#include <climits>
using namespace std;

int main() {
    int numLocations = 6;
    
    // Distance matrix between pizza shop and delivery locations
    int distance[6][6] = {
        {0, 4, 0, 0, 0, 0},
        {4, 0, 8, 0, 0, 0},
        {0, 8, 0, 7, 0, 4},
        {0, 0, 7, 0, 9, 14},
        {0, 0, 0, 9, 0, 10},
        {0, 0, 4, 14, 10, 0}
    };
    
    // Arrays for Prim's MST Algorithm
    int parentNode[6];           // Stores MST structure
    int minDistance[6];          // Minimum distance to include in MST
    bool visited[6];             // Track visited nodes
    
    // Initialize all distances as infinite and visited as false
    for(int i = 0; i < numLocations; i++) {
        minDistance[i] = INT_MAX;
        visited[i] = false;
    }
    
    // Start from pizza shop (node 0)
    minDistance[0] = 0;
    parentNode[0] = -1;
    
    // Build MST with numLocations-1 edges
    for(int edge = 0; edge < numLocations - 1; edge++) {
        
        // Find minimum distance node not yet visited
        int minDist = INT_MAX;
        int currentNode;
        
        for(int node = 0; node < numLocations; node++) {
            if(!visited[node] && minDistance[node] < minDist) {
                minDist = minDistance[node];
                currentNode = node;
            }
        }
        
        visited[currentNode] = true;
        
        // Update distances of adjacent nodes
        for(int neighbor = 0; neighbor < numLocations; neighbor++) {
            if(distance[currentNode][neighbor] && 
               !visited[neighbor] && 
               distance[currentNode][neighbor] < minDistance[neighbor]) {
                parentNode[neighbor] = currentNode;
                minDistance[neighbor] = distance[currentNode][neighbor];
            }
        }
    }
    
    // Display MST result
    cout << "Pizza Delivery Route (MST):\n";
    cout << "----------------------------\n";
    int totalDistance = 0;
    
    for(int i = 1; i < numLocations; i++) {
        cout << "Location " << parentNode[i] << " -> Location " << i 
             << " : " << distance[i][parentNode[i]] << " km\n";
        totalDistance += distance[i][parentNode[i]];
    }
    
    cout << "----------------------------\n";
    cout << "Total Distance: " << totalDistance << " km\n";
    
    return 0;
}
```
____________________________________________________________________________________________________________________
Binary SEARCH TREE :

```
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node *left, *right;
};

// Create new node
Node* newNode(int value) {
    Node* node = new Node();
    node->data = value;
    node->left = node->right = NULL;
    return node;
}

// Insert
Node* insert(Node* root, int value) {
    if (root == NULL) return newNode(value);
    if (value < root->data)
        root->left = insert(root->left, value);
    else if (value > root->data)
        root->right = insert(root->right, value);
    return root;
}

// Search
bool search(Node* root, int key) {
    if (root == NULL) return false;
    if (root->data == key) return true;
    if (key < root->data)
        return search(root->left, key);
    else
        return search(root->right, key);
}

// Inorder Display
void inorder(Node* root) {
    if (root != NULL) {
        inorder(root->left);
        cout << root->data << " ";
        inorder(root->right);
    }
}

// Find minimum node
Node* minValue(Node* root) {
    while (root && root->left != NULL)
        root = root->left;
    return root;
}

// Delete
Node* deleteNode(Node* root, int key) {
    if (root == NULL) return root;
    if (key < root->data)
        root->left = deleteNode(root->left, key);
    else if (key > root->data)
        root->right = deleteNode(root->right, key);
    else {
        if (root->left == NULL) return root->right;
        else if (root->right == NULL) return root->left;
        Node* temp = minValue(root->right);
        root->data = temp->data;
        root->right = deleteNode(root->right, temp->data);
    }
    return root;
}

// Main
int main() {
    Node* root = NULL;
    root = insert(root, 50);
    insert(root, 30);
    insert(root, 70);
    insert(root, 20);
    insert(root, 40);
    insert(root, 60);
    insert(root, 80);

    cout << "Inorder Traversal: ";
    inorder(root);

    cout << "\nSearch 40: " << (search(root, 40) ? "Found" : "Not Found");

    root = deleteNode(root, 20);
    cout << "\nAfter deleting 20: ";
    inorder(root);

    return 0;
}
```
HASHING function 2, using LINEAR PROBING
```
#include <iostream>
using namespace std;

#define SIZE 10   // Fixed size of hash table
#define EMPTY -1  // Empty cell marker
#define DELETED -2 // Deleted cell marker

int hashTable[SIZE];

// Hash function (Division method)
int hashFunc(int key) {
    return key % SIZE;
}

// Insert a key
void insertKey(int key) {
    int index = hashFunc(key);
    int startIndex = index;

    while (hashTable[index] != EMPTY && hashTable[index] != DELETED) {
        index = (index + 1) % SIZE;
        if (index == startIndex) {
            cout << "Hash Table is full!\n";
            return;
        }
    }
    hashTable[index] = key;
    cout << "Inserted " << key << " at index " << index << endl;
}

// Search a key
void searchKey(int key) {
    int index = hashFunc(key);
    int startIndex = index;

    while (hashTable[index] != EMPTY) {
        if (hashTable[index] == key) {
            cout << "Key " << key << " found at index " << index << endl;
            return;
        }
        index = (index + 1) % SIZE;
        if (index == startIndex) break;
    }
    cout << "Key " << key << " not found.\n";
}

// Delete a key
void deleteKey(int key) {
    int index = hashFunc(key);
    int startIndex = index;

    while (hashTable[index] != EMPTY) {
        if (hashTable[index] == key) {
            hashTable[index] = DELETED;
            cout << "Key " << key << " deleted from index " << index << endl;
            return;
        }
        index = (index + 1) % SIZE;
        if (index == startIndex) break;
    }
    cout << "Key " << key << " not found.\n";
}

// Display the hash table
void displayTable() {
    cout << "\nHash Table:\n";
    for (int i = 0; i < SIZE; i++) {
        cout << i << " --> ";
        if (hashTable[i] == EMPTY) cout << "EMPTY";
        else if (hashTable[i] == DELETED) cout << "DELETED";
        else cout << hashTable[i];
        cout << endl;
    }
}

// Main function
int main() {
    for (int i = 0; i < SIZE; i++) hashTable[i] = EMPTY;

    int choice, key;

    while (true) {
        cout << "\n--- Hash Table Menu ---\n";
        cout << "1. Insert\n2. Search\n3. Delete\n4. Display\n5. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter key to insert: ";
                cin >> key;
                insertKey(key);
                break;

            case 2:
                cout << "Enter key to search: ";
                cin >> key;
                searchKey(key);
                break;

            case 3:
                cout << "Enter key to delete: ";
                cin >> key;
                deleteKey(key);
                break;

            case 4:
                displayTable();
                break;

            case 5:
                cout << "Exiting...";
                return 0;

            default:
                cout << "Invalid choice!";
        }
    }
}

```

SORTING CODE:
```
ARYAN:

#include <iostream>
using namespace std;

void selection_sort(double arr[], int n) {
    for (int i = 0; i < n; i++) {
        int min_idx = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[min_idx]) {
                min_idx = j;
            }
        }
        double temp = arr[i];
        arr[i] = arr[min_idx];
        arr[min_idx] = temp;
    }
}

void bubble_sort(double arr[], int n) {
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                double temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}

int main() {
    double salaries[] = {
        45000.50, 32000.00, 78000.75, 51000.20, 29000.00,
        66000.40, 88000.00, 73000.55, 91000.90, 54000.60
    };

    int n = sizeof(salaries) / sizeof(salaries[0]);

    double s1[10];
    double s2[10];

    for (int i = 0; i < n; i++) {
        s1[i] = salaries[i];
        s2[i] = salaries[i];
    }

    bubble_sort(s1, n);
    selection_sort(s2, n);

    cout << "Bubble Sorted: ";
    for (int i = 0; i < n; i++) cout << s1[i] << " ";
    cout << "\n";

    cout << "Selection Sorted: ";
    for (int i = 0; i < n; i++) cout << s2[i] << " ";
    cout << "\n";

    cout << "Top 5: ";
    for (int i = n - 5; i < n; i++) cout << s1[i] << " ";
    cout << "\n";

    return 0;
}
----------------------------------------------------------------------
HIMANSHU:

#include <iostream>
using namespace std;

// Function to display array
void displaySalaries(float salaries[], int n) {
    for (int i = 0; i < n; i++) {
        cout << salaries[i] << " ";
    }
    cout << endl;
}

// Selection Sort Algorithm
void selectionSort(float salaries[], int n) {
    for (int i = 0; i < n - 1; i++) {
        int minIndex = i;
        
        // Find minimum element in unsorted part
        for (int j = i + 1; j < n; j++) {
            if (salaries[j] < salaries[minIndex]) {
                minIndex = j;
            }
        }
        
        // Swap minimum element with first element
        if (minIndex != i) {
            float temp = salaries[i];
            salaries[i] = salaries[minIndex];
            salaries[minIndex] = temp;
        }
    }
}

// Bubble Sort Algorithm
void bubbleSort(float salaries[], int n) {
    for (int i = 0; i < n - 1; i++) {
        // Flag to optimize (stop if no swaps occur)
        bool swapped = false;
        
        // Compare adjacent elements
        for (int j = 0; j < n - i - 1; j++) {
            if (salaries[j] > salaries[j + 1]) {
                // Swap if they are in wrong order
                float temp = salaries[j];
                salaries[j] = salaries[j + 1];
                salaries[j + 1] = temp;
                swapped = true;
            }
        }
        
        // If no swaps occurred, array is sorted
        if (!swapped) {
            break;
        }
    }
}

// Function to display top 5 highest salaries
void displayTop5(float salaries[], int n) {
    cout << "\nTop 5 Highest Salaries:" << endl;
    
    // Start from end (highest) and go backwards
    int count = (n < 5) ? n : 5;  // Handle if less than 5 employees
    
    for (int i = n - 1; i >= n - count; i--) {
        cout << i - (n - count) + 1 << ". $" << salaries[i] << endl;
    }
}
-
int main() {
    int n;
    
    cout << "Enter number of employees: ";
    cin >> n;
    
    float salaries1[n], salaries2[n];
    
    // Input salaries
    cout << "Enter " << n << " employee salaries:" << endl;
    for (int i = 0; i < n; i++) {
        cout << "Salary " << i + 1 << ": ";
        cin >> salaries1[i];
        salaries2[i] = salaries1[i];  // Copy for second algorithm
    }
    
    cout << "\n--- Original Salaries ---" << endl;
    displaySalaries(salaries1, n);
    
    // Selection Sort
    cout << "\n--- Using Selection Sort ---" << endl;
    selectionSort(salaries1, n);
    cout << "Sorted Salaries: ";
    displaySalaries(salaries1, n);
    displayTop5(salaries1, n);
    
    // Bubble Sort
    cout << "\n--- Using Bubble Sort ---" << endl;
    bubbleSort(salaries2, n);
    cout << "Sorted Salaries: ";
    displaySalaries(salaries2, n);
    displayTop5(salaries2, n);
    
    return 0;
}
```
