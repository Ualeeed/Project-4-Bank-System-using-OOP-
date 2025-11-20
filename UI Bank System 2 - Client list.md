---
DATE: 2025-11-20T18:41:00
DONE: true
---

- Create a new header ` clsClientListScreen` to perform showing client list 


-> ` clsClientListScreen.h` : 

```cpp
#pragma once

#include <iostream>
#include "clsScreen.h"
#include "clsBankClient.h"
#include <iomanip>

class clsClientListScreen:protected clsScreen
{

private:
   static void PrintClientRecordLine(clsBankClient Client)
    {

        cout << setw(8) << left << "" << "| " << setw(15) << left << Client.AccountNumber();
        cout << "| " << setw(20) << left << Client.FullName();
        cout << "| " << setw(12) << left << Client.Phone;
        cout << "| " << setw(20) << left << Client.Email;
        cout << "| " << setw(10) << left << Client.PinCode;
        cout << "| " << setw(12) << left << Client.AccountBalance;

    }

public:
  

   static void ShowClientsList()
    {

        
        vector <clsBankClient> vClients = clsBankClient::GetClientsList();
        string Title = "\t  Client List Screen";
        string SubTitle ="\t    (" + to_string(vClients.size()) + ") Client(s).";

        _DrawScreenHeader(Title, SubTitle);
        

        cout << setw(8) << left << "" << "\n\t_______________________________________________________";
        cout << "_________________________________________\n" << endl;

        cout <<  setw(8) << left << "" << "| " << left << setw(15) << "Accout Number";
        cout << "| " << left << setw(20) << "Client Name";
        cout << "| " << left << setw(12) << "Phone";
        cout << "| " << left << setw(20) << "Email";
        cout << "| " << left << setw(10) << "Pin Code";
        cout << "| " << left << setw(12) << "Balance";
        cout << setw(8) << left << "" << "\n\t_______________________________________________________";
        cout << "_________________________________________\n" << endl;

        if (vClients.size() == 0)
            cout << "\t\t\t\tNo Clients Available In the System!";
        else

            for (clsBankClient Client : vClients)
            {

                PrintClientRecordLine(Client);
                cout << endl;
            }

        cout << setw(8) << left << "" << "\n\t_______________________________________________________";
        cout << "_________________________________________\n" << endl;

    }

};


```


- Called in the `clsMainScreen.h` : 

-> `clsMainScreen.h` : 

```cpp
#pragma once
#include <iostream>
#include "clsScreen.h"
#include "clsInputValidate.h"
#include "clsClientListScreen.h"


using namespace std;

class clsMainScreen:protected clsScreen
{


    private:
        enum enMainMenueOptions {
            eListClients = 1, eAddNewClient = 2, eDeleteClient = 3,
            eUpdateClient = 4, eFindClient = 5, eShowTransactionsMenue = 6,
            eManageUsers = 7, eExit = 8
        };

       static short _ReadMainMenueOption()
        {
            cout <<setw(37) << left << ""<< "Choose what do you want to do? [1 to 8]? ";
            short Choice = clsInputValidate::ReadShortNumberBetween(1,8,"Enter Number between 1 to 8? ");
            return Choice;
        }

       static  void _GoBackToMainMenue()
        {
            cout << setw(37) << left << ""<<"\n\tPress any key to go back to Main Menue...\n";
           
            system("pause>0");
            ShowMainMenue();
        }
       
       static void _ShowAllClientsScreen()
       {
        //  cout << "\nClient List Screen Will be here...\n";
          clsClientListScreen::ShowClientsList();
 

       }

       static void _ShowAddNewClientsScreen()
       {
           cout << "\nAdd New Client Screen Will be here...\n";
         
       }

       static void _ShowDeleteClientScreen()
       {
           cout << "\nDelete Client Screen Will be here...\n";

       }

       static void _ShowUpdateClientScreen()
       {
           cout << "\nUpdate Client Screen Will be here...\n";

       }

       static void _ShowFindClientScreen()
       {
           cout << "\nFind Client Screen Will be here...\n";

       }

       static void _ShowTransactionsMenue()
       {
           cout << "\nTransactions Menue Will be here...\n";

       }

       static void _ShowManageUsersMenue()
       {
           cout << "\nUsers Menue Will be here...\n";

       }

       static void _ShowEndScreen()
           {
               cout << "\nEnd Screen Will be here...\n";

           }

       static void _PerfromMainMenueOption(enMainMenueOptions MainMenueOption)
        {
            switch (MainMenueOption)
            {
            case enMainMenueOptions::eListClients:
            {
                system("cls");
                _ShowAllClientsScreen();
                _GoBackToMainMenue();
                break;
            }
            case enMainMenueOptions::eAddNewClient:
                system("cls");
               _ShowAddNewClientsScreen();
                _GoBackToMainMenue();
                break;

            case enMainMenueOptions::eDeleteClient:
                system("cls");
                _ShowDeleteClientScreen();
                _GoBackToMainMenue();
                break;

            case enMainMenueOptions::eUpdateClient:
                system("cls");
                _ShowUpdateClientScreen();
                _GoBackToMainMenue();
                break;

            case enMainMenueOptions::eFindClient:
                system("cls");
                _ShowFindClientScreen();
                _GoBackToMainMenue();
                break;

            case enMainMenueOptions::eShowTransactionsMenue:
                system("cls");
                _ShowTransactionsMenue();
                break;

            case enMainMenueOptions::eManageUsers:
                system("cls");
                _ShowManageUsersMenue();
                break;

            case enMainMenueOptions::eExit:
                system("cls");
                _ShowEndScreen();
                //Login();

                break;
            }

        }



	public:
       

       static void ShowMainMenue()
        {
           
            system("cls");
            _DrawScreenHeader("\t\tMain Screen");

            cout << setw(37) << left <<""<< "===========================================\n";
            cout << setw(37) << left << "" << "\t\t\tMain Menue\n";
            cout << setw(37) << left << "" << "===========================================\n";
            cout << setw(37) << left << "" << "\t[1] Show Client List.\n";
            cout << setw(37) << left << "" << "\t[2] Add New Client.\n";
            cout << setw(37) << left << "" << "\t[3] Delete Client.\n";
            cout << setw(37) << left << "" << "\t[4] Update Client Info.\n";
            cout << setw(37) << left << "" << "\t[5] Find Client.\n";
            cout << setw(37) << left << "" << "\t[6] Transactions.\n";
            cout << setw(37) << left << "" << "\t[7] Manage Users.\n";
            cout << setw(37) << left << "" << "\t[8] Logout.\n";
            cout << setw(37) << left << "" << "===========================================\n";

            _PerfromMainMenueOption((enMainMenueOptions)_ReadMainMenueOption());
        }

};


```

