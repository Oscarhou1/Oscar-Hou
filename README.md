#Program Name: (Oscar bank)


#Program Description: to let user to use oscar bank ,sign in ,show balance,withdraw,diposit,and more

#-----------------------------------------------------------------------------
#References:

#(put a link to your reference here but also add a comment in the code below where you used the reference)

#-----------------------------------------------------------------------------
#Additional Libraries/Extensions:

#(put a list of required extensions so that the user knows that they need to download extra features)

#-----------------------------------------------------------------------------




#Known bugs:

#(put a list of known bugs here, if you have any)

#----------------------------------------------------------------------------


#Program Reflection:
#I think this project deserves a leve4+ because ...

# Level 3 Requirements Met:
#• Accept the github invite link
#• Use the randint command (or another command from the random library) to generate a random starting balance between $500-$2500.
#• The program has no bugs.
#• Coding decisions should make sense and not include grossly inefficient code.
#•  Use appropriate data types (int, String, long).


#Features Added Beyond Level 4 Requirements:
#• use library
#• Github commits more than daily with every major coding change
#• user can change password
#•  save the mount of money
#-----------------------------------------------------------------------------




import random
from PIL import Image

myImage = Image.open("inkpx-word-art.png")

def process_image():
    # Correct usage: resize((width, height), resample)
    process = myImage.resize((300, 300), Image.Resampling.LANCZOS)
    return process

processed_img = process_image()
processed_img.show()   # shows the resized image

# main progarm

def main():
    # Define valid user accounts
    accounts = {
        "swindrunner": "1234",
        "istormrage": "abcd", 
        "tpgallywix": "qwerty"
    }
    
    # Generate random starting balance between $500-$2500 also to save the mount of money
    account_money = random.randint(500, 2500)
    
    # Display welcome message
    print("=" * 50)
    print("WELCOME TO OSCAR BANK ")
    print("=" * 50)
    
    # Login system
    login_successful = False
    current_user = None
    
    while not login_successful:
        print("\nPlease login to your account")
        username = input("Username: ").strip()
        password = input("Password: ").strip()
        
        # Check if credentials are valid
        if username in accounts and accounts[username] == password:
            print(f"\nLogin successful! Welcome {username}!")
            login_successful = True
            current_user = username
        else:
            print("Invalid username or password. Please try again.")
    
    # Display login success message with balance
    print(f"Your current balance: ${account_money}")
    
    # Menu system
    running = True
    
    # Define menu options
    menu_options = {
        "1": {"name": "Display Balance", "function": "display_balance"},
        "2": {"name": "Withdraw", "function": "withdraw"},
        "3": {"name": "Deposit", "function": "deposit"},
        "4": {"name": "Exit", "function": "exit"},
        "5": {"name": "change password", "function": "change_password"}
    }
    # branch functions
    def display_balance():
        """Display the current account balance"""
        print(f"\nCurrent Balance: ${account_money}")
        return account_money
    
    def withdraw():
        """Handle withdrawal operation with overdraft protection"""
        nonlocal account_money
        
        print("\n--- WITHDRAW FUNDS ---")
        
        # Get withdrawal amount with validation
        while True:
            try:
                withdraw_amount = int(input("Enter amount to withdraw: $"))
                
                # Validate withdrawal amount
                if withdraw_amount <= 0:
                    print("Withdrawal amount must be positive. Please try again.")
                    continue
                
                # Check if withdrawal would exceed overdraft limit
                if account_money - withdraw_amount < -500:
                    print("Transaction denied: Overdraft limit exceeded.")
                    print("You cannot withdraw beyond $500 overdraft.")
                    return account_money
                
                # Process the withdrawal
                account_money -= withdraw_amount
                print(f"Successfully withdrew ${withdraw_amount}")
                
                # Apply overdraft fee if balance is negative also need fee for over draft
                if account_money < 0:
                    overdraft_fee = abs(account_money) * 0.20
                    account_money -= overdraft_fee
                    print(f"Overdraft fee applied: ${overdraft_fee:.2f}")
                    print(f"Your account is now overdrawn.")
                
                return account_money
                
            except ValueError:
                print("Invalid input. Please enter a numeric value.")
    
    def deposit():
        """Handle deposit operation"""
        nonlocal account_money
        
        print("\n--- DEPOSIT FUNDS ---")
        
        # Get deposit amount with validation
        while True:
            try:
                deposit_amount = int(input("Enter amount to deposit: $"))
                
                # Validate deposit amount
                if deposit_amount <= 0:
                    print("Deposit amount must be positive. Please try again.")
                    continue
                
                # Process the deposit
                account_money += deposit_amount
                print(f"Successfully deposited ${deposit_amount}")
                return account_money
                
            except ValueError:
                print("Invalid input. Please enter a numeric value.")
    
    def exit_program():
        """Exit the program"""
        nonlocal running
        print("\n" + "=" * 50)
        print("Thank you for using oscar bank")
        print("Have a great day!")
        print("=" * 50)
        running = False
        return None
    
    def change_password():
        """Allow the current user to change their password"""
        nonlocal accounts, current_user

        print("\n--- CHANGE PASSWORD ---")

        # Ask for old password
        old_pw = input("Enter your current password: ")

        if accounts[current_user] != old_pw:
            print("Incorrect password. Password not changed.")
            return

        # Ask for new password twice
        new_pw = input("Enter your new password: ").strip()
        confirm_pw = input("Re-enter your new password: ").strip()

        if new_pw == confirm_pw and new_pw != "":
            accounts[current_user] = new_pw
            print("Password successfully changed!")
        else:
            print("Passwords did not match or cannot be empty.")

    
    # Main program loop
    while running:
        # Display menu options
        print("\n" + "=" * 40)
        print("Please select an option:")
        for key, value in menu_options.items():
            print(f"{key}. {value['name']}")
        print("=" * 40)
        
        # Get user choice
        choice = input("Enter your choice (1-5): ").strip()
        
        # Process user choice
        if choice in menu_options:
            option_function = menu_options[choice]["function"]
            
            if option_function == "display_balance":
                display_balance()
            elif option_function == "withdraw":
                withdraw()
            elif option_function == "deposit":
                deposit()
            elif option_function == "change_password":
                change_password()
            elif option_function == "exit":
                exit_program()
        else:
            print("Invalid choice. Please enter a number between 1-4.")

# to let the main function run
if __name__ == "__main__":
    main()
