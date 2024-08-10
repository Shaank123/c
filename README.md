### Project: Bank Management System

#### **Features**
1. **Create an Account**: Allow users to create a new bank account.
2. **View Account Details**: Display account details based on the account number.
3. **Deposit Money**: Allow users to deposit money into their account.
4. **Withdraw Money**: Allow users to withdraw money if they have sufficient balance.
5. **Delete Account**: Allow users to delete an account.

#### **Project Structure**
```
bank-management-system/
├── main.c
├── account.h
├── account.c
├── Makefile
└── README.md
```

#### **1. main.c**
This is the entry point of your program.

```c
#include <stdio.h>
#include "account.h"

int main() {
    int choice;
    while(1) {
        printf("\nBank Management System\n");
        printf("1. Create Account\n");
        printf("2. View Account\n");
        printf("3. Deposit Money\n");
        printf("4. Withdraw Money\n");
        printf("5. Delete Account\n");
        printf("6. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch(choice) {
            case 1:
                createAccount();
                break;
            case 2:
                viewAccount();
                break;
            case 3:
                depositMoney();
                break;
            case 4:
                withdrawMoney();
                break;
            case 5:
                deleteAccount();
                break;
            case 6:
                return 0;
            default:
                printf("Invalid choice! Please try again.\n");
        }
    }
    return 0;
}
```

#### **2. account.h**
Header file that declares the functions related to account management.

```c
#ifndef ACCOUNT_H
#define ACCOUNT_H

void createAccount();
void viewAccount();
void depositMoney();
void withdrawMoney();
void deleteAccount();

#endif
```

#### **3. account.c**
Implementation of the functions declared in `account.h`.

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include "account.h"

typedef struct {
    int accountNumber;
    char name[50];
    float balance;
} Account;

void createAccount() {
    Account account;
    FILE *file = fopen("accounts.dat", "ab");

    printf("Enter account number: ");
    scanf("%d", &account.accountNumber);
    printf("Enter name: ");
    scanf("%s", account.name);
    account.balance = 0.0;

    fwrite(&account, sizeof(Account), 1, file);
    fclose(file);
    printf("Account created successfully!\n");
}

void viewAccount() {
    int accountNumber;
    Account account;
    FILE *file = fopen("accounts.dat", "rb");

    printf("Enter account number: ");
    scanf("%d", &accountNumber);

    while (fread(&account, sizeof(Account), 1, file)) {
        if (account.accountNumber == accountNumber) {
            printf("Account Number: %d\n", account.accountNumber);
            printf("Name: %s\n", account.name);
            printf("Balance: $%.2f\n", account.balance);
            fclose(file);
            return;
        }
    }

    printf("Account not found!\n");
    fclose(file);
}

void depositMoney() {
    int accountNumber;
    float amount;
    Account account;
    FILE *file = fopen("accounts.dat", "rb+");

    printf("Enter account number: ");
    scanf("%d", &accountNumber);

    while (fread(&account, sizeof(Account), 1, file)) {
        if (account.accountNumber == accountNumber) {
            printf("Enter amount to deposit: ");
            scanf("%f", &amount);
            account.balance += amount;
            fseek(file, -sizeof(Account), SEEK_CUR);
            fwrite(&account, sizeof(Account), 1, file);
            printf("Money deposited successfully!\n");
            fclose(file);
            return;
        }
    }

    printf("Account not found!\n");
    fclose(file);
}

void withdrawMoney() {
    int accountNumber;
    float amount;
    Account account;
    FILE *file = fopen("accounts.dat", "rb+");

    printf("Enter account number: ");
    scanf("%d", &accountNumber);

    while (fread(&account, sizeof(Account), 1, file)) {
        if (account.accountNumber == accountNumber) {
            printf("Enter amount to withdraw: ");
            scanf("%f", &amount);
            if (amount > account.balance) {
                printf("Insufficient balance!\n");
            } else {
                account.balance -= amount;
                fseek(file, -sizeof(Account), SEEK_CUR);
                fwrite(&account, sizeof(Account), 1, file);
                printf("Money withdrawn successfully!\n");
            }
            fclose(file);
            return;
        }
    }

    printf("Account not found!\n");
    fclose(file);
}

void deleteAccount() {
    int accountNumber;
    Account account;
    FILE *file = fopen("accounts.dat", "rb");
    FILE *tempFile = fopen("temp.dat", "wb");

    printf("Enter account number: ");
    scanf("%d", &accountNumber);

    while (fread(&account, sizeof(Account), 1, file)) {
        if (account.accountNumber != accountNumber) {
            fwrite(&account, sizeof(Account), 1, tempFile);
        }
    }

    fclose(file);
    fclose(tempFile);

    remove("accounts.dat");
    rename("temp.dat", "accounts.dat");

    printf("Account deleted successfully!\n");
}
```

#### **4. Makefile**
To simplify compilation, you can create a Makefile.

```makefile
CC = gcc
CFLAGS = -Wall

all: main

main: main.o account.o
	$(CC) main.o account.o -o bank

main.o: main.c account.h
	$(CC) $(CFLAGS) -c main.c

account.o: account.c account.h
	$(CC) $(CFLAGS) -c account.c

clean:
	rm -f *.o bank
```

#### **5. README.md**
Add a `README.md` file to describe your project, how to compile it, and how to use it.

```markdown
# Bank Management System

A simple bank management system implemented in C. The system allows you to create an account, view account details, deposit money, withdraw money, and delete an account.

## Features
- Create an account
- View account details
- Deposit money
- Withdraw money
- Delete an account

## Compilation
Use the provided `Makefile` to compile the program:

```
make
```

## Usage
Run the compiled program:

```
./bank
```
```

### How to Use This Project
1. **Clone the Repository**:
   - If you haven't already, clone your repository to your local machine:
     ```bash
     git clone https://github.com/yourusername/c.git
     ```

2. **Add the Project Files**:
   - Create the project structure and add the files as outlined above.

3. **Commit and Push**:
   - After adding the files, commit your changes and push them to GitHub:
     ```bash
     git add .
     git commit -m "Add Bank Management System project"
     git push origin main
     ```

4. **Compile and Run**:
   - Navigate to the project directory and compile the program using `make`. Run the program to interact with the Bank Management System.

This project will give you hands-on experience with C programming, including working with files, structures, and basic command-line interfaces. If you need further guidance or more advanced features, feel free to ask!
