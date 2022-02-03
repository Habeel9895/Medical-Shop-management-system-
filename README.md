# Medical-Shop-management-system-
#Medical Shop management system using python and MySQL 

#MySQL code
#create database medical store;
#use medicalstore;
#create table medicines(Mcode int primary key, Mname varchar(15), qty int, price int);

#MySQL connection
import mysql.connector as m
c=m.connect(host="localhost",user="root",passwd="habeel9895",database="medicalstore")
cur=c.cursor()

# function definition
def intro():
    print("\033[1m\n\t\t\t_______________________________________________")
    print("\t\t\t\t\tMEDICAL STORE MANAGEMENT SYSTEM")
    print("\t\t\t_______________________________________________\033[0m")
    x=input("\n\n\t\t\tpress any key to continue")

def display():
    print("\033[1m\t\tDISPLAY\033[0m")
    cur.execute("select * from medicines")
    d=cur.fetchall()
    count=cur.rowcount
    print("\nnumber of records:",count)
    if count!= 0:
        print("The Medicine lists are")
        for i in d:
            print(i)

def insert():
    choice="y"
    while choice=="y":
        try:
            print("\033[1m\t\tINSER\033[0mT")
            print("PLEASE ENTER THE DETAILS")
            mcode=int(input("enter the Medicine code: "))
            medicine_name=input("enter the medicine name: ")
            quantity=int(input('enter the quantity: '))
            price=int(input('enter the price: '))
            cur.execute("insert into medicines values({},'{}',{},{})".format(mcode,medicine_name,quantity,price))
            c.commit()
            print("record added successfuly")
        except:
            print(
                "invalid input, Please try again by entering y")  # if the entered Mcode value is already taken or instead of int str is input
        choice=input("do you want to add more records? y/n")
    print("you can view the table with newly added record from the main menu:)")

def search():
    choice="y"
    while choice=="y":
        try:
            print("\033[1m SEARCH\033[0m") #searching using mcode
            mcode=int(input("enter the Mcode of the record to be searched:"))
            cur.execute("select * from medicines where Mcode={}".format(mcode))
            d=cur.fetchone()
            if d!=None:
                print("record found")
                print("Mcode:{},Medicine name:{},Quantity:{},Price:{}".format(d[0],d[1],d[2],d[3]))
            else:
                print("record not found")
        except:
            print("sorry invalid input please try agian")
        choice=input("do you to search any other record? y/n")

def update():
    choice = "y"
    while choice == "y":
        try:
            print("\033[1m\t\tUPDATE\033[0m")
            print("**PLEASE SELECT THE OPTION TO BE UPDATED**")
            print("1: PRICE")
            print("2: QUANTITY")
            print("3: EXIT TO MAIN MENU")
            option=int(input("enter the option to be updated"))
            if option==1:
                print("\033[1m PRICE UPDATION\033[0m")
                mcode=int(input("enter the Medicine code : "))
                cur.execute("select * from medicines where Mcode={}".format(mcode))
                d = cur.fetchone()
                if d != None:
                    print("record to be updated found")
                    print(d)
                    new_price=int(input("enter the new price: "))
                    cur.execute("update medicines set price={} where Mcode={}".format(new_price,mcode))
                    c.commit()
                    print("price updated")
                    print("updated record is")
                    cur.execute("select * from medicines where Mcode={}".format(mcode))
                    d = cur.fetchone()
                    print(d)
                else:
                    print("record to be updated not found")
            elif option==2:
                print("\033[1m QUANTITY UPDATION\033[0m")
                mcode = int(input("enter the Medicine code : "))
                cur.execute("select * from medicines where Mcode={}".format(mcode))
                d = cur.fetchone()
                if d != None:
                    print("record to be updated found")
                    print(d)
                    new_qty = int(input("enter the new quantity: "))
                    cur.execute("update medicines set qty={} where Mcode={}".format(new_qty, mcode))
                    c.commit()
                    print("Quantity updated")
                    print("updated record is")
                    cur.execute("select * from medicines where Mcode={}".format(mcode))
                    d = cur.fetchone()
                    print(d)
                else:
                    print("record to be updated not found")
            elif option==3:
                break
            else:
                print("invalid option")
        except:
            print("sorry invalid input please try again")
        choice=input("do you want to update any more records? y/n")
    print("updated table")
    display()   #optional

def delete():
    choice="y"
    while choice=="y":
        try:
            print("\t\tDELETE")
            m=int(input("ENTER THE Mcode OF THE RECORD TO BE DELETED"))
            cur.execute("select * from medicines where Mcode={}".format(m))
            d=cur.fetchone()
            if d!= None:
                print(d)
                cur.execute("delete from medicines where Mcode={}".format(m))
                c.commit()
                print("is deleted")
            else:
                print("record to be deleted not found")
        except:
            print("sorry invalid input please try again")
        choice=input("do you want to delete more records? y/n")

def EXIT():
    print("""\t\t  THANK YOU
        PROGRAM ENDED""")

#main block
intro()
choice="y"
while choice=="y":
    try:
        print("\033[1m\t\t\tMAIN MENU\033[0m")
        print("***SELECT ANY OPTION FROM 1-6***")
        print("\t1. DISPLAY MEDICINE LIST")
        print("\t2. INSERT MEDICINE DETAILS")
        print("\t3. SEARCH")
        print("\t4. UPDATE PRICE/QTY")
        print("\t5. DELETE RECORD")
        print("\t6. EXIT")
        option=int(input("enter a number from 1-6 "))
        if option==1:
            display()
        elif option==2:
            insert()
        elif option==3:
            search()
        elif option==4:
            update()
        elif option==5:
            delete()
        elif option==6:
            EXIT()
            break
        else:
            print("sorry wrong input")
    except:
        print("sorry invalid input please try again")
    choice=input("press 'y' to continue in the main menu or press 'n' to exit the program")
else:
    EXIT()
c.close()
