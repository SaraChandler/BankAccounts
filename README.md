# Bank Accounts

We will be working with the concept of bank accounts in order to explore more object-oriented code as well as a few other new topics.

## Baseline Setup

1. This is an individual project.
1. Fork this repo to your Github account
1. Clone the forked repo into `~/C5/projects`: `$ git clone [YOUR FORKED REPO URL]`
1. Enter the created directory: `$ cd BankAccounts`
1. Run `git status` to verify the branch you are on
1. Run `git remote -v` to verify the folder you are in corresponds to the fork you have created.

## Wave 1
### Primary Functionality
1. Create a `Bank` module which will contain your `Account` class and any future bank account logic.
1. Create an `Account` class which should have the following functionality:
  - A new account should be created with an __ID__ and an __initial balance__
  - Should have a `withdraw` method that accepts a single parameter which represents the amount of money that will be withdrawn. This method should return the updated account balance.
  - Should have a `deposit` method that accepts a single parameter which represents the amount of money that will be deposited. This method should return the updated account balance.
  - Should be able to access the current `balance` of an account at any time.

### Error handling
1. A new account cannot be created with initial negative balance - this will `raise` an `ArgumentError` (Google this)
1. The `withdraw` method does not allow the account to go negative - Will `puts` a warning message and then return the original un-modified balance

### Bonus Optional Fun Time:
- Create an `Owner` class which will store information about those who own the `Accounts`.
  - This should have info like name and address and any other identifying information that an account owner would have.
- Add an `owner` property to each Account to track information about who owns the account.
  - The `Account` can be created with an `owner`, OR you can create a method that will add the `owner` after the `Account` has already been created.


## Wave 2: CSV Files!
### Primary Requirements
1. Update the `Account` class to be able to handle all of these fields from the CSV file used as input.
  - For example, manually choose the data from the first line of the CSV file and ensure you can create a new instance of your Account using that data
1. Add the following **class** methods to your existing `Account` class
  - `self.all` - returns a collection of `Account` instances, representing all of the Accounts described in the CSV. See below for the CSV file specifications
  - `self.find(id)` - returns an instance of `Account` where the value of the id field in the CSV matches the passed parameter

#### CSV Data File for `Bank::Account`
The data, in order in the CSV, consists of:

1. **ID** - (Fixnum) a unique identifier for that Account  
1. **Balance** - (Fixnum) the account balance amount, in cents (i.e., 150 would be $1.50)  
1. **OpenDate** - (Datetime) when the account was opened

### Bonus Optional Fun Times:
- Implement the optional requirement from Wave 1
- Add the following **class** methods to your existing `Owner` class
  - `self.all` - returns a collection of `Owner` instances, representing all of the Owners described in the CSV. See below for the CSV file specifications
  - `self.find(id)` - returns an instance of `Owner` where the value of the id field in the CSV matches the passed parameter

#### CSV Data File for `Bank::Owner`  
The data, in order in the CSV, consists of:  

1. **ID** - (Fixnum) a unique identifier for that Owner  
1. **Last Name** - (String) the owner's last name   
1. **First Name** - (String) the owner's first name  
1. **Street Addess** - (String) the owner's street address  
1. **City** - (String) the owner's city  
1. **State** - (String) the owner's state  

- Add the instance method `accounts` to the `Owner` class. This method should return a collection of `Account` instances that belong to the specific owner.  To create the relationship between the accounts and the owners use an `account_owners.csv` file. The data for this file, in order in the CSV, consists of:

1. **Account ID** - (Fixnum) a unique identifier corresponding to an `Account` instance.
1. **Owner ID** - (Fixnum) a unique identifier corresponding to an `Owner` instance.

## Wave 3: Inheritance
### Primary Requirements
1. Create a `SavingsAccount` class which should inherit behavior from the `Account` class.
  1. It should include the following updated functionality:
    - The initial balance cannot be less than $10. If it is, this will `raise` an `ArgumentError`
    - Updated withdrawal functionality:
      - Each withdrawal 'transaction' incurs a fee of $2 that is taken out of the balance.
      - Does not allow the account to go below the $10 minimum balance - Will output a warning message and return the original un-modified balance
  1. It should include the following new methods:
    - `#add_interest(rate)`: Calculate the interest on the balance and add the interest to the balance. Return the **interest** that was calculated and added to the balance (not the updated balance).
      - The parameter `rate` is assumed to be a percentage (i.e. 0.25).
      - The formula for calculating interest is `balance * rate/100`
        - Example: If the interest rate is 0.25% and the balance is $10,000, then the interest that is returned is $25 and the new balance becomes $10,025.
1. Create a `CheckingAccount` class which should inherit behavior from the `Account` class.
  1. It should include the following updated functionality:
    - Updated withdrawal functionality:
      - Each withdrawal 'transaction' incurs a fee of $1 that is taken out of the balance. Returns the updated account balance.
      - Does not allow the account to go negative. Will output a warning message and return the original un-modified balance.
    - `#withdraw_using_check(amount)`: The input amount gets taken out of the account as a result of a check withdrawal. Returns the updated account balance.
      - Allows the account to go into overdraft up to -$10 but not any lower
      - The user is allowed three free check uses in one month, but any subsequent use adds a $2 transaction fee
    - `#reset_checks`: Resets the number of checks used to zero

### Bonus Optional Fun Times:
- Create a `MoneyMarketAccount` class which should inherit behavior from the `Account` class.
  - A maximum of 6 transactions (deposits or withdrawals) are allowed per month on this account type
  - The initial balance cannot be less than $10,000 - this will `raise` an `ArgumentError`
  - Updated withdrawal logic:
    - If a withdrawal causes the balance to go below $10,000, a fee of $100 is imposed and no more transactions are allowed until the balance is increased using a deposit transaction.
    - Each transaction will be counted against the maximum number of transactions
  - Updated deposit logic:
    - Each transaction will be counted against the maximum number of transactions
    - Exception to the above: A deposit performed to reach or exceed the minimum balance of $10,000 is not counted as part of the 6 transactions.
  - `#add_interest(rate)`: Calculate the interest on the balance and add the interest to the balance. Return the interest that was calculated and added to the balance (not the updated balance).
    - **Note** This is the same as the `SavingsAccount` interest.
  - `#reset_transactions`: Resets the number of transactions to zero
