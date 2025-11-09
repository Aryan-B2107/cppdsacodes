HOLA# cppdsacodes
1ST ASSIGNMENT CODE : 

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
