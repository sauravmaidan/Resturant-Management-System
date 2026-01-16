# Resturant-Management-System
import csv
import os
import datetime
from colorama import Fore, Style

def view_menu():
    print("\n========== MENU ==========")
    with open("view_menu.csv", "r") as f:
        reader = csv.DictReader(f)
        cats=[]
        for row in reader:
            cat = row["Category"]
            dish = row["Dish"]
            price = row["Price"]

            if cat not in cats:
                print(f"\n{cat}:")
                cats.append(cat)
            print(f"- {dish} (₹{price})")

def search_dish():
    dish_name=input("Enter Dish Name = ").strip().lower()
    with open("view_menu.csv","r") as f:
        reader=csv.DictReader(f)
        for row in reader:
            if dish_name in row["Dish"].lower():
                print(Fore.GREEN + f"\n✅ Found: {row['Dish']} in {row['Category']} - ₹{row['Price']}" + Style.RESET_ALL)
                return
    print(Fore.RED + "\n❌ Sorry, this dish is not available in the menu." + Style.RESET_ALL)
        
def place_order():
    order_place = []
    with open("view_menu.csv", "r") as f: 
        reader = csv.DictReader(f)
        menu = list(reader)

    while True:
        dish = input("Enter Dish Name (or 'done' to finish): ").lower()
        if dish == "done":
            break

        for row in menu:
            if dish in row["Dish"].lower():
                try:
                    qty = int(input("Quantity: "))
                except ValueError:
                    print(Fore.RED + "❌ Invalid quantity!" + Style.RESET_ALL)
                    continue
                total = int(row["Price"]) * qty
                order_place.append([row["Dish"], qty, total])   
                print(f"{row['Dish']} x{qty} = ₹{total}")      
                break
        else:
            print(Fore.RED + "❌ Not Found!" + Style.RESET_ALL)   

    return order_place

def bill_generate(order_place, customer_name="Customer"):
    if not order_place:
        print(Fore.RED + "❌ No Item Ordered Found!" + Style.RESET_ALL)   
        return
    print("\n========== 🧾 BILL ==========")
    print(f"Customer Name : {customer_name}")   
    print("Date & Time   :", datetime.datetime.now().strftime("%d-%m-%Y %H:%M:%S"))
    print("-"*40)

    total_bill = 0
    for dish, qty, total in order_place:
        print(f"{dish} x{qty} = ₹{total}")
        total_bill += total
    
    print("-"*40)
    print(f"TOTAL BILL = ₹{total_bill }")
    print(Fore.YELLOW + "🙏 Thank You For Dining With Us!" + Style.RESET_ALL)
    print("="*40)


def give_review(customer_name):
    print("\n========== Customer Review ==========")
    print(f"Hello {customer_name}, Please Share Your Feedback!")
    review=input("Write Your Review =")
    rating=int(input("Rate Us (1/5) ="))
    print(Fore.YELLOW + f"✅ Thank {customer_name}, Your Review Has Been Saved" + Style.RESET_ALL) 
               
    
def main():
    order_place = []   
    customer_name = input("Enter Your Name = ")   

    while True:
        print("="*100)
        print(f"\t\t\t\t 🍴 Welcome {customer_name} To Saurav Da Dhaba 🍴 ")  
        print("="*100)
        print("1. View Menu\n2. Search Dish\n3. Place Order\n4. Generate Bill\n5. Review\n6. Logout")
        print("-"*100)

        try:
            customer_choice = int(input("Enter Your Choice = "))
        except ValueError:
            print(Fore.RED + "❌ Invalid input, please enter a number!" + Style.RESET_ALL)
            continue

        if customer_choice == 1:
            print("-"*100)
            view_menu()
            
        elif customer_choice == 2:
            print("-"*100)
            search_dish()

        elif customer_choice == 3:
            print("-"*100)
            order_place = place_order()

        elif customer_choice==4:
            print("="*100)
            print("Bill")
            bill_generate(order_place, customer_name)

        elif customer_choice==5:
            print("="*100)
            print("Give Review")
            give_review(customer_name)

        elif customer_choice==6:
            print("="*100)
            print(Fore.YELLOW + f"🙏 Dhanaywad {customer_name}, Apka Yaha Ane Ke Leye" + Style.RESET_ALL)
            break
        else:
            print(Fore.RED + "❌ Invalid choice, try again!" + Style.RESET_ALL)

[view_menu.csv](https://github.com/user-attachments/files/24675597/view_menu.csv)
Category,Dish,Price

Veg,Lababdar Paneer Butter Masala,180

Veg,Desi Thali,150

Veg,Kurkuri Bhindi Masala,99

Veg,Dilli-6 Chatkara Chole ,149

Non-Veg,Chicken 440 Volt ,230

Non-Veg,Murgh Matwale Tangdi ,250

Non-Veg,Biryani-e-Bewafa ,300

Starters,Chatori Kurkuri Kachori ,70

Starters,Chatori Chatkara Rolls,200

Drinks,Khatta-Meetha Imli Shot ,50

Drinks,Pyaar Mohabbat Sharbat,80


main()
