/**************************************************************
 *
 * GROUP #6
 * Group Members: Jasen Clerisier, Anika Patel, Markeisa Graham
 * Date Completed: tba
 * Filename: shoppingCart.cpp
 * 
 **************************************************************/


#include <iostream>
#include <fstream>
#include <cstdlib>
#include <cstring>
#include <ctime>
#include <sstream>
#include <vector> 
#include <string>
#include <iomanip>
#include <stdlib.h>
#include <ctype.h>
#include <stdio.h>

using namespace std;

struct Address { string streetAddress;        //struct for address data
                 string city;  
                 string state;  
                 string zipCode;   
                };    

struct Customer { string customerNum;         //struct for customer data 
                  string customerName; 
                  int lineOfCredit; 
                  Address * corporateAddress;
                 };

struct Product{ int itemNo;                    //struct for product data 
                string description; 
                double price; };

struct Cart{ int itemNo;                       //struct for product data 
                string description;  
                int quant; 
                double total;
           };

void Prodsort(Product eList[], int arrLen) // Sorts Product struct, Reference: https://www.daniweb.com/programming/software-development/threads/415633/c-selection-sort-with-array-of-structures#
{
     int i, j, min_idx; 
  
    for (i = 0; i < arrLen-1; i++) 
    { 
        min_idx = i; 
        
        for (j = i+1; j < arrLen; j++) 
          if (eList[j].itemNo < eList[min_idx].itemNo)
            min_idx = j; 
      
    Product temp = eList[min_idx]; //temporary employee variable to hold the old value during swap
    eList[min_idx] = eList[i]; // assign the min_idx(element to be moved) to it's position
    eList[i] = temp; //assign the element previously at the now sorted postion
}
}

void Custsort(Customer eList[], int arrLen) //Sorts Customer struct, Reference: https://www.daniweb.com/programming/software-development/threads/415633/c-selection-sort-with-array-of-structures#
{
    
     int i, j, min_idx; 
  
    for (i = 0; i < arrLen-1; i++) 
    { 
        min_idx = i; 
        
        for (j = i+1; j < arrLen; j++) 
          if (eList[j].customerNum.compare(eList[min_idx].customerNum) < 0)
            min_idx = j; 
      
    Customer temp = eList[min_idx]; //temporary employee variable to hold the old value during swap
    eList[min_idx] = eList[i]; // assign the min_idx(element to be moved) to it's position
    eList[i] = temp; //assign the element previously at the now sorted postion
}
}

void Addsort(Address eList[], int arrLen) // sorts Address struct
{
    
     int i, j, min_idx; 
  
    for (i = 0; i < arrLen-1; i++) 
    { 
        min_idx = i; 
        
        for (j = i+1; j < arrLen; j++) 
          if (eList[j].zipCode.compare(eList[min_idx].zipCode) < 0)
            min_idx = j; 
      
    Address temp = eList[min_idx]; //temporary employee variable to hold the old value during swap
    eList[min_idx] = eList[i]; // assign the min_idx(element to be moved) to it's position
    eList[i] = temp; //assign the element previously at the now sorted postion
}
}

string upper(string str) //returns uppercase version of passed string, Reference: http://www.cplusplus.com/forum/beginner/70692/
{
	locale settings;
	string converted;
	for(short i = 0; i < str.size(); ++i)
		converted += (std::toupper(str[i], settings));
	return converted;
}

int CustNumBinarySearch(Customer eList[], int numElems, string num) //Binary search for customer number, Reference: textbook
{    
 int first = 0,                                           // First array element                        
     last = numElems - 1,                                 // Last array element      
     middle,                                              // Last array element 
     position = -1;                                       // Position of search value       
 bool found = false;                                      // Flag
 while (!found && first <= last) 
 { 
      middle = (first + last) / 2;                        // Calculate midpoint              
     if (eList[middle].customerNum == num)                // If value is found at mid    
     {           
         found = true;           
         position = middle;        
     }        
     else if (eList[middle].customerNum > num)            // If value is in lower half 
         last = middle - 1;        
     else           
                     first = middle + 1;                  // If value is in upper half
 }    
    return position;  
}  


int CustNameLinearSearch(Customer eList[], int numElems, string name) //Linear search for the customer name
{ 
 int last = numElems - 1, 
     position = -1; 
for(int first = 0 ; first <= last ; first++)   
 { 
    if (upper(eList[first].customerName) == upper(name))      
     {                     
         position = first;        
     }        
 }              
    return position;  
}  

int AddLinearSearch(vector <Address> a, string street, string zipCode) //Linear seach for the Address
{ 
 int last = a.size()-1, 
     position = -1; 
for(int first = 0 ; first <= last ; first++)   
 { 
    if ((a[first].streetAddress == street)&&(a[first].zipCode == zipCode))
     
     {                     
         position = first;        
     }        
 }              
    return position;  
}  

int ProdLinearSearch(Product eList[], int numElems, int item) //Linear search for products, Reference: textbook
{ 
 int last = numElems - 1, 
     position = -1; 
for(int first = 0 ; first <= last ; first++)   
 { 
    if (eList[first].itemNo == item)    
     {                     
         position = first;        
     }        
 }              
    return position;  
}  

/**************************************************************
 *                             parse                          *
 * Called by: <fill in>                                       *
 * Passed   : 2 arguments: a string, a character that is the  *
 *            delimiter                                       *
 * Purpose  : Split a given string by its delimiter           *
 * Returns  : String vector                                   *
 **************************************************************/
vector<string> parse(string line, char delimiter){
    
    vector<string> parsedLine;
    istringstream stringStream( line ); //create string stream from the line
    //iterate through the stream
    while (stringStream)
    {
        string s;
        //uses getline to get the values from the stream before the delimiting character
        if (getline( stringStream, s, delimiter))
          parsedLine.push_back(s);
    }
    return parsedLine;
}

/**************************************************************
 *                  generateOrderNum                          *
 * Called by: <fill in>                                       *                                       
 * Purpose  : Generate an order number based on epoch         *
 * Returns  : String order number                             *
 **************************************************************/
string generateOrderNum(){
    
    time_t now = time(0); //current time object
    
    string oNum;
    stringstream strstream;
    
    strstream << now; //read the date/time in epoch into the string stream
    strstream >> oNum; //output a string from the string stream
    return oNum;
}

int main()
{
    int const max_c = 21;
	int const max_p = 20;               
    string base;                        //stores an entire line from the input file
    string convert_lineOfCredit;        //temporary variable to converts customer lineOfCredit string to integer
    string convert_itemNo;              //temporary variable to converts product itemNo string to integer
    string convert_price;               //temporary variable to converts product price string to double
    ifstream inFile1("customers.dat");  //declaration and opening of customers input file
    ifstream inFile2("inventory.dat");  //declaration and opening of inventory input file
    int i = 0,                          //increment variable to store data being read from input from customers.dat
    j = 0;                              //increment variable to store data being read from input from inventory.dat
    Customer cust;                      //temporary placeholder to store for Customer struct parts
    Address add;                        //temporary placeholder to store for Address struct parts
    Product prod;                       //temporary placeholder to store for Product struct parts
    Address output;                     //stores the address converted back from pointer
    Customer Cust_arr[max_c];           //Customer array: holds Customer struct data
	Address Add_arr[max_c];             //Address array: holds Address struct data
	Product Prod_arr[max_p];            //Product array: holds Product struct data
    Cart Cart_arr[max_p];               //Cart array: holds info for ordered items
    int Loc[max_c];                     //array storing position of addresses in vector
    vector <Address> Add;               //vectors storing locations of addresses
    int bc = 0;                         //stores last instance of "|" in customers.dat                      
    int custholder,                     //stores position for customer info in Cust_arr
    itemholder;                         //stores position for item info in Prod_arr
    int choice1,                        //choice for first menu
    choice2;                            //choice for second menu 
    string cust_num;                    //stores customer number
    string cust_name;                   //stores customer name
    string associate_nam;               //stores associate name
    double balance,                     //stores customer balance
    temptotal,                          //stores temporary total
    ordtotal,                           //stores entire total
    price;                              //stores price of chosen item
    int itemNo;                         //stores item number of item
    int counter = 0;                    //increment variable for adding items to Cart_arr
    int quantity;                       //stores quantity of the chosen item
    char last;                          //decides whether code terminates or continues
    bool foundname = false,             //flag for do-while, first menu
    foundnum = false;                   //flag for do-while, first menu
    int addpos;                         //stores address position for iterator
    string filename;                    //stores ordernumber and filename
    ofstream cart_order;                //declartion for output file
      
    
        while(getline(inFile1, base))                                //storing customers.dat into arrays Cust_arr and Add_arr
    {
        vector <string> CparsedLine = parse(base,'|');               //parses string of line from file
            
        cust.customerNum = CparsedLine[0];                           //storing the customer number into a temporary struct location
        cust.customerName = CparsedLine[1];                          //storing the customer name into a temporary struct location
        convert_lineOfCredit = CparsedLine[2];                       //storing the customer credit into a temporary struct location
        istringstream (convert_lineOfCredit) >> cust.lineOfCredit;   //Reference: https://stackoverflow.com/questions/7663709/how-can-i-convert-a-stdstring-to-int
      
        bc = base.find_last_of("|");                                 //finds last instance of the | symbol
        base.erase(0,bc+1);                                          //deletes everything in the string up to the symbol
            
        vector <string> AparsedLine = parse(base ,',');               //parses string of line from file
        add.streetAddress = AparsedLine[0];
        add.city = AparsedLine[1];                                    //storing the city into a temporary struct location
        add.state = AparsedLine[2];                                   //storing the state into a temporary struct location
        add.zipCode = AparsedLine[3];                                 //storing the zipcode into a temporary struct location

       addpos = AddLinearSearch(Add, add.streetAddress, add.zipCode); //searches if address is already saved
       if(addpos == -1)                                               //if address is not stored, it adds it to the vector
          {
             Add.push_back(add);
             addpos = Add.size()-1;
          }    
        Loc[i] = addpos;                                              //puts vector addresses into Loc array

        Cust_arr[i] = cust;                                           //putting all the parts of the struct into one array location
        i++;
    }
    
    for(int a = 0 ; a < 21 ; a++)                                     //iterator that places all Loc array locations into Cust_arr           
    {
        vector <Address>::iterator ptr = Add.begin();
        advance(ptr, Loc[a]);
        Cust_arr[a].corporateAddress = &* ptr;
    }
             
        while(getline(inFile2, base))                                 //storing inventory.dat into array Prod_arr
    { 
        vector <string> PparsedLine = parse(base,',');                
            
        convert_itemNo = PparsedLine[0];                              //storing item number into temporary struct location, and converting to an integer in the next line
        istringstream (convert_itemNo) >> prod.itemNo;                //Reference: https://stackoverflow.com/questions/7663709/how-can-i-convert-a-stdstring-to-int
        prod.description = PparsedLine[1];                            //storing item description int temporary struct location
        convert_price = PparsedLine[2];                               //storing item price into temporary struct location, and converting to a double in the next line
        istringstream (convert_price) >> prod.price;                  //Reference: https://stackoverflow.com/questions/7663709/how-can-i-convert-a-stdstring-to-int
        
        Prod_arr[j] = prod;                                           //putting all the parts of the struct into one array location
        j++;
    }

    Custsort(Cust_arr, 21);                                             //sorts Cust_arr, customers array           
    Addsort(Add_arr, 21);                                               //sorts Add_arr, addresses array
    Prodsort(Prod_arr, 20);                                             //storts Prod_arr, products array
        

        cout << endl << "Associate, please enter your name: ";          //input before menu begins, associate name entry
        getline(cin, associate_nam);
        cout << endl << endl;
           
        do
        {
            
        while(foundnum == false && foundnum == false)                   //first menu, customer name/number entry
        {
        cout << endl << endl;
        cout << "|------------------------------------|" << endl;        
        cout << "| 1 - Search By Number               |" << endl;
        cout << "| 2 - Search By Name                 |" << endl;
        cout << "|------------------------------------|" << endl;
        cout << "Selection: ";
        cin >> choice1;
        
        if(choice1 == 1)                                                //first choice, takes number input and searches Cust_arr             
            
          {
            cout << endl << "Enter customer number: ";
                cin >> cust_num;
                custholder = CustNumBinarySearch(Cust_arr, 21, cust_num);
                cout << endl;
                if(custholder == -1)                                    //customer name is not found, goes back to first menu
                    cout << "Customer not found.";
            else
                    foundnum = true;                                    //customer name is found, moves onto next menu 
          }
        
        else if(choice1 == 2)                                           //second choice, takes name input and searches Cust_arr  
          {
            cout << endl << "Enter customer name: ";
                cin.ignore();
                getline(cin, cust_name);
                custholder = CustNameLinearSearch(Cust_arr, 21, cust_name);
                cout << endl;
                if(custholder == -1)                                    //customer name is not found, goes back to first menu
                    cout << "Customer not found.";
                else
                    foundnum = true;                                    //customer name is found, moves onto next menu                  
          }
            else                                                        //invalid choice, goes back to first menu                  
                cout << endl << "Bad choice.";
        }
        cout << "Customer Found: Now ordering for " << Cust_arr[custholder].customerName << endl;
        filename = generateOrderNum();
        do
        {
                balance = Cust_arr[custholder].lineOfCredit;                                   //second menu, enter products, view/display them                
                cout << endl << endl;
                cout << "|------------------------------------|" << endl;
                cout << "| 1 - Add Product to Cart            |" << endl;
                cout << "| 2 - View Cart                      |" << endl;
                cout << "| 3 - Checkout                       |" << endl; 
                cout << "|------------------------------------|" << endl;
                cout << "Selection: ";
                cin >> choice2;
            
                switch (choice2)
                {
                    case 1: cout << endl << "Enter Product Number: ";                          //allows user to input item number of item they want to order
                            cin >> itemNo;
                            itemholder = ProdLinearSearch(Prod_arr, 20, itemNo);
                            if(itemholder == -1)                                               //entered item is not found, goes back to second menu
                              {
                                cout << "Item not found." << endl;
                                break;
                              }
                                                                                                    
                            price = Prod_arr[itemholder].price;
                            cout << "Enter Quantity: ";                                       //item is found, propmts for quantity          
                            cin >> quantity;
                            cout << endl;
                            temptotal = price * quantity;                                      //temporary total for current item
                            if((balance - ordtotal - temptotal) < 0)                           //checks if the temporary total can be covered by (remaining) balance
                              {
                                cout << "Total exceeds balance. Unable to add.";
                                break;
                              }
                            ordtotal+=temptotal;                                               //item can successfully be added              
                            //stores item information in Cart_arr
                            Cart_arr[counter].itemNo = itemNo;
                            Cart_arr[counter].description = Prod_arr[itemholder].description;
                            Cart_arr[counter].quant = quantity;
                            Cart_arr[counter].total = temptotal;
                            counter++;
                            cout << quantity << " " << Prod_arr[itemholder].description << " added." << endl;
                    break;
                        
                    case 2: cout << endl << "---------------------------------------------------" << endl    
                                 << "Item No         " << "Description       " << "Qty    " << "Total" << endl
                                 << "---------------------------------------------------" << endl;
  
                                //For loop that outputs items' name ,quantity, and total cost
                                for (int index = 0; index < counter; index++)                          
                                    {
                                        //Item No
                                        cout << "   "<< right << setfill(' ') << setw(4) << Cart_arr[index].itemNo << "  "
                                       //Item Description
                                       << setfill(' ') << setw(23) << Cart_arr[index].description << "  "
                                       //Item Quantity
                                       << setw(6) << setfill('0') << right << Cart_arr[index].quant << "  " 
                                       //Item Total
                                       << right << fixed << showpoint << setprecision(2) << Cart_arr[index].total << endl;
                                    }
                            cout << "---------------------------------------------------" << endl;
                    break;
                        
                    case 3: cart_order.open(filename.c_str()); //opens output file with name of order number                                                    
                        
                        //stores address from pointers into Address output
                        output.streetAddress = Cust_arr[custholder].corporateAddress->streetAddress;
                        output.city = Cust_arr[custholder].corporateAddress->city;
                        output.state = Cust_arr[custholder].corporateAddress->state;
                        output.zipCode = Cust_arr[custholder].corporateAddress->zipCode;
                        
                        //Prints order header information to ouput file
                        cart_order << "---------------------------------------------------"<< endl
                        << "B2B Shopping Cart" << endl
                        << "---------------------------------------------------" << endl << endl
                        << "Order Number: " << filename << endl << "Associate: " << associate_nam << endl
                        << "Customer Number: " << Cust_arr[custholder].customerNum << endl
                        << "Customer Name: " << Cust_arr[custholder].customerName << endl
                        << "Address: " << output.streetAddress << endl 
                        << "        " << output.city << " ," << output.state << output.zipCode << endl
                        << "---------------------------------------------------" << endl 
                        << "Item No         " << "Description       " << "Qty    " << "Total" << endl
                        << "---------------------------------------------------" << endl;

                        //For loop that outputs items' name ,quantity, and total cost                                                 
                        for (int index = 0; index < counter; index++)                          
                            {
                                   //Item No 
                                   cart_order << "   "<< right << setfill(' ') << setw(4) << Cart_arr[index].itemNo << "  "
                                   //Item Description
                                   << setfill(' ') << setw(23) << Cart_arr[index].description << "  "
                                   //Item Quantity
                                   << setw(6) << setfill('0') << right << Cart_arr[index].quant << "  " 
                                   //Item Total
                                   << right << fixed << showpoint << setprecision(2) << Cart_arr[index].total << endl;
                            }
                        
                                   //Outputs total and remaining credit
                                   cart_order << "---------------------------------------------------" << endl
                                   << "Total           " << right << setfill(' ') << setw(35) 
                                   << fixed << showpoint << setprecision(2) << ordtotal << endl
                                   << "---------------------------------------------------" << endl
                                   << "Remaining Credit" << right << setfill(' ') << fixed 
                                   << showpoint << setprecision(2) << setw(35) << balance - ordtotal << endl
                                   << "---------------------------------------------------" << endl;
                            break;

                    default: cout << endl << "Bad choice.";
                            break;
                }//end of switch
        }//end of do-while for choice2
        while (choice2 != 3);
    cout << endl << endl << "Would you like to enter an order for another customer? (Y/N) ";                    //last part of menu, ask if another customer entry wants to be made
    cin >> last;
    foundnum = false;  //resets boolean for if customer number was found
    foundname = false; //resets boolean for if customer name was found
}//end of entire do-while
while (toupper(last) == 'Y'); //decides whether or not to proceed with code
return 0;
}//end of main
