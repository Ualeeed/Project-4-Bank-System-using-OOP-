---
DATE: 2025-11-16T17:50:00
DONE: true
---

##### Detailed Explanation of the New Functions added to my Bank project : 


1. `ReadClientInfo` Function : 

```cpp
void ReadClientInfo(clsBankClient& Client)
{
    cout << "\nEnter FirstName: ";
    Client.FirstName = clsInputValidate::ReadString();

    cout << "\nEnter LastName: ";
    Client.LastName = clsInputValidate::ReadString();

    cout << "\nEnter Email: ";
    Client.Email = clsInputValidate::ReadString();

    cout << "\nEnter Phone: ";
    Client.Phone = clsInputValidate::ReadString();

    cout << "\nEnter PinCode: ";
    Client.PinCode = clsInputValidate::ReadString();

    cout << "\nEnter Account Balance: ";
    Client.AccountBalance = clsInputValidate::ReadFloatNumber();
}
```

#### **Purpose:**

This function collects and validates all the necessary information from the user to either create a new client or update an existing client's information.

#### **Key Characteristics:**

**1. Pass-by-Reference Parameter (`clsBankClient& Client`):**

- The function receives a reference to a `clsBankClient` object
- This means any changes made to the `Client` object inside the function will directly affect the original object passed to it
- This is efficient because it avoids creating a copy of the object

**2. User Input Collection:**

- The function prompts the user for each piece of client information one by one
- It uses `clsInputValidate::ReadString()` for text fields (`FirstName, LastName, Email, Phone, PinCode`)
- It uses `clsInputValidate::ReadFloatNumber()` for the `AccountBalance`

**3. Input Validation:**

- By using the `clsInputValidate` class methods, you ensure that the input is properly validated
- `ReadString()` ensures proper string input (handles whitespace correctly)
- `ReadFloatNumber()` ensures the user enters a valid numeric value for the account balance

**4. Direct Property Assignment:**

- The function directly modifies the properties of the `Client` object: `FirstName`, `LastName`, `Email`, `Phone`, `PinCode`, and `AccountBalance`
- This works because these properties are accessible (likely public or have public setters in the `clsBankClient` class)

2. `UpdateClient` Function : 

```cpp
void UpdateClient()
{
    string AccountNumber = "";

    cout << "\nPlease Enter client Account Number: ";
    AccountNumber = clsInputValidate::ReadString();
    
    while (!clsBankClient::IsClientExist(AccountNumber))
    {
        cout << "\nAccount number is not found, choose another one: ";
        AccountNumber = clsInputValidate::ReadString();
    }

    clsBankClient Client1 = clsBankClient::Find(AccountNumber);
    Client1.Print();

    cout << "\n\nUpdate Client Info:";
    cout << "\n____________________\n";

    ReadClientInfo(Client1);

    clsBankClient::enSaveResults SaveResult;

    SaveResult = Client1.Save();

    switch (SaveResult)
    {
    case  clsBankClient::enSaveResults::svSucceeded:
    {
        cout << "\nAccount Updated Successfully :-)\n";
        Client1.Print();
        break;
    }
    case clsBankClient::enSaveResults::svFaildEmptyObject:
    {
        cout << "\nError account was not saved because it's Empty";
        break;
    }
    }
}
```

#### **Purpose:**

This is the main update functionality that allows users to modify existing client information in the bank system.

#### **Detailed Step-by-Step Breakdown:**

**Step 1: Account Number Input**


```cpp
string AccountNumber = "";
cout << "\nPlease Enter client Account Number: ";
AccountNumber = clsInputValidate::ReadString();
```

- Prompts the user to enter the account number of the client they want to update

**Step 2: Account Existence Validation**


```cpp
while (!clsBankClient::IsClientExist(AccountNumber))
{
    cout << "\nAccount number is not found, choose another one: ";
    AccountNumber = clsInputValidate::ReadString();
}
```

- Uses a **validation loop** to ensure the account exists before proceeding
- Calls the static method `IsClientExist()` from the `clsBankClient` class
- If the account doesn't exist, it keeps asking until a valid account number is entered
- This prevents errors from attempting to update a non-existent client

**Step 3: Retrieve Client Object**


```cpp
clsBankClient Client1 = clsBankClient::Find(AccountNumber);
Client1.Print();
```

- Uses the `Find()` static method to retrieve the client object from the data source (likely a file)
- Displays the current client information using the `Print()` method
- This allows the user to see the current data before updating

**Step 4: Update Client Information**

```cpp
cout << "\n\nUpdate Client Info:";
cout << "\n____________________\n";
ReadClientInfo(Client1);
```

- Displays a header for the update section
- Calls `ReadClientInfo()` and passes `Client1` by reference
- The user can now input new values for all client fields

**Step 5: Save Changes**


```cpp
clsBankClient::enSaveResults SaveResult;
SaveResult = Client1.Save();
```

- Calls the `Save()` method on the client object to persist changes to the data source
- The `Save()` method returns an enumeration (`enSaveResults`) indicating success or failure

**Step 6: Display Result**

```cpp
switch (SaveResult)
{
case  clsBankClient::enSaveResults::svSucceeded:
{
    cout << "\nAccount Updated Successfully :-)\n";
    Client1.Print();
    break;
}
case clsBankClient::enSaveResults::svFaildEmptyObject:
{
    cout << "\nError account was not saved because it's Empty";
    break;
}
}
```

- Uses a switch statement to handle different save results
- If successful (`svSucceeded`), displays a success message and prints the updated client info
- If failed due to empty object (`svFaildEmptyObject`), displays an error message

-> `main` : 

```cpp

#include <iostream>
#include "clsBankClient.h"
#include "clsInputValidate.h"

void ReadClientInfo(clsBankClient& Client)
{
    cout << "\nEnter FirstName: ";
    Client.FirstName = clsInputValidate::ReadString();

    cout << "\nEnter LastName: ";
    Client.LastName = clsInputValidate::ReadString();

    cout << "\nEnter Email: ";
    Client.Email = clsInputValidate::ReadString();

    cout << "\nEnter Phone: ";
    Client.Phone = clsInputValidate::ReadString();

    cout << "\nEnter PinCode: ";
    Client.PinCode = clsInputValidate::ReadString();

    cout << "\nEnter Account Balance: ";
    Client.AccountBalance = clsInputValidate::ReadFloatNumber();
}

void UpdateClient()
{
    string AccountNumber = "";

    cout << "\nPlease Enter client Account Number: ";
    AccountNumber = clsInputValidate::ReadString();
    
    while (!clsBankClient::IsClientExist(AccountNumber))
    {
        cout << "\nAccount number is not found, choose another one: ";
        AccountNumber = clsInputValidate::ReadString();
    }

    clsBankClient Client1 = clsBankClient::Find(AccountNumber);
    Client1.Print();

    cout << "\n\nUpdate Client Info:";
    cout << "\n____________________\n";

  
    ReadClientInfo(Client1);

    clsBankClient::enSaveResults SaveResult;

    SaveResult = Client1.Save();

    switch (SaveResult)
    {
    case  clsBankClient::enSaveResults::svSucceeded:
    {
        cout << "\nAccount Updated Successfully :-)\n";
        Client1.Print();
        break;
    }
    case clsBankClient::enSaveResults::svFaildEmptyObject:
    {
        cout << "\nError account was not saved because it's Empty";
        break;

    }

    }
}

int main()

{
    UpdateClient();
    system("pause>0");
    return 0;
}
```

## **How They Work Together**

1. **UpdateClient()** manages the workflow
2. It validates the account exists
3. Retrieves the client object
4. Calls **ReadClientInfo()** to collect new data
5. Saves the changes
6. Provides feedback to the user

This design is efficient, maintainable, and follows object-oriented principles by encapsulating client data in the `clsBankClient` class and delegating validation to the `clsInputValidate` class.


- Used Library : 

→ `clsInputValidate` : 

```cpp


#pragma once
#include <iostream>
#include <string>
#include "clsString.h"
#include "clsDate.h"

class clsInputValidate
{

public:

	static bool IsNumberBetween(short Number, short From, short To)
	{
		if (Number >= From && Number <= To)
			return true;
		else
			return false;
	}

	static bool IsNumberBetween(int Number, int From, int To)
	{
		if (Number >= From && Number <= To)
			return true;
		else
			return false;

	}

	

	static bool IsNumberBetween(double Number, double From, double To)
	{
		if (Number >= From && Number <= To)
			return true;
		else
			return false;
	}

	static bool IsDateBetween(clsDate Date, clsDate From, clsDate To)
	{
		//Date>=From && Date<=To
		if ((clsDate::IsDate1AfterDate2(Date, From) || clsDate::IsDate1EqualDate2(Date, From))
			&&
			(clsDate::IsDate1BeforeDate2(Date, To) || clsDate::IsDate1EqualDate2(Date, To))
			)
		{
			return true;
		}

		//Date>=To && Date<=From
		if ((clsDate::IsDate1AfterDate2(Date, To) || clsDate::IsDate1EqualDate2(Date, To))
			&&
			(clsDate::IsDate1BeforeDate2(Date, From) || clsDate::IsDate1EqualDate2(Date, From))
			)
		{
			return true;
		}

		return false;
	}

	static int ReadIntNumber(string ErrorMessage = "Invalid Number, Enter again\n")
	{
		int Number;
		while (!(cin >> Number)) {
			cin.clear();
			cin.ignore(numeric_limits<streamsize>::max(), '\n');
			cout << ErrorMessage;
		}
		return Number;
	}

	static int ReadIntNumberBetween(int From, int To, string ErrorMessage = "Number is not within range, Enter again:\n")
	{
		int Number = ReadIntNumber();

		while (!IsNumberBetween(Number, From, To))
		{
			cout << ErrorMessage;
			Number = ReadIntNumber();
		}
		return Number;
	}

	static double ReadFloatNumber(string ErrorMessage = "Invalid Number, Enter again\n")
	{
		float Number;
		while (!(cin >> Number)) {
			cin.clear();
			cin.ignore(numeric_limits<streamsize>::max(), '\n');
			cout << ErrorMessage;
		}
		return Number;
	}

	static double ReadFloatNumberBetween(double From, double To, string ErrorMessage = "Number is not within range, Enter again:\n")
	{
		float Number = ReadFloatNumber();

		while (!IsNumberBetween(Number, From, To)) {
			cout << ErrorMessage;
			Number = ReadDblNumber();
		}
		return Number;
	}

	static double ReadDblNumber(string ErrorMessage = "Invalid Number, Enter again\n")
	{
		double Number;
		while (!(cin >> Number)) {
			cin.clear();
			cin.ignore(numeric_limits<streamsize>::max(), '\n');
			cout << ErrorMessage;
		}
		return Number;
	}

	static double ReadDblNumberBetween(double From, double To, string ErrorMessage = "Number is not within range, Enter again:\n")
	{
		double Number = ReadDblNumber();

		while (!IsNumberBetween(Number, From, To)) {
			cout << ErrorMessage;
			Number = ReadDblNumber();
		}
		return Number;
	}

	static bool IsValideDate(clsDate Date)
	{
		return	clsDate::IsValidDate(Date);
	}

	static string ReadString()
	{
		string  S1="";
		// Usage of std::ws will extract allthe whitespace character
		getline(cin >> ws, S1);
		return S1;
	}
};


```

→ `clsPerson` : 

```cpp
#pragma once
#include <iostream>
#include <string>
using namespace std;

class clsPerson
{

private:
   
    string _FirstName;
    string _LastName;
    string _Email;
    string _Phone;

public:
 
    clsPerson( string FirstName, string LastName, string Email, string Phone)
    {
        
        _FirstName = FirstName;
        _LastName = LastName;
        _Email = Email;
        _Phone = Phone;
    }

    //Property Set
    void SetFirstName(string FirstName)
    {
        _FirstName = FirstName;
    }

    //Property Get
    string GetFirstName()
    {
        return _FirstName;
    }
    __declspec(property(get = GetFirstName, put = SetFirstName)) string FirstName;

    //Property Set
    void SetLastName(string LastName)
    {
        _LastName = LastName;
    }

    //Property Get
    string GetLastName()
    {
        return _LastName;
    }
    __declspec(property(get = GetLastName, put = SetLastName)) string LastName;

    //Property Set
    void SetEmail(string Email)
    {
        _Email = Email;
    }

    //Property Get
    string GetEmail()
    {
        return _Email;
    }
    __declspec(property(get = GetEmail, put = SetEmail)) string Email;

    //Property Set
    void SetPhone(string Phone)
    {
        _Phone = Phone;
    }

    //Property Get
    string GetPhone()
    {
        return _Phone;
    }
    __declspec(property(get = GetPhone, put = SetPhone)) string Phone;

    string FullName()
    {
        return _FirstName + " " + _LastName;
    }


};


```

→ `clsString` : 

```cpp


#pragma once

#include <iostream>
#include <vector>

using namespace std;

class clsString
{

private:
    string _Value;

public:

    clsString()
    {
        _Value = "";
    }

    clsString(string Value)
    {
        _Value = Value;
    }

    void SetValue(string Value) {
        _Value = Value;
    }

    string GetValue() {
        return _Value;
    }

    __declspec(property(get = GetValue, put = SetValue)) string Value;


    static short Length(string S1)
    {
        return S1.length();
    };

    short Length()
    {
        return _Value.length();
    };

    static short CountWords(string S1)
    {

        string delim = " "; // delimiter  
        short Counter = 0;
        short pos = 0;
        string sWord; // define a string variable  

        // use find() function to get the position of the delimiters  
        while ((pos = S1.find(delim)) != std::string::npos)
        {
            sWord = S1.substr(0, pos); // store the word   
            if (sWord != "")
            {
                Counter++;
            }

            //erase() until positon and move to next word.
            S1.erase(0, pos + delim.length());
        }

        if (S1 != "")
        {
            Counter++; // it counts the last word of the string.
        }

        return Counter;

    }

    short CountWords()
    {
        return CountWords(_Value);
    };

    static string  UpperFirstLetterOfEachWord(string S1)
    {

        bool isFirstLetter = true;

        for (short i = 0; i < S1.length(); i++)
        {

            if (S1[i] != ' ' && isFirstLetter)
            {
                S1[i] = toupper(S1[i]);

            }

            isFirstLetter = (S1[i] == ' ' ? true : false);

        }

        return S1;
    }

    void  UpperFirstLetterOfEachWord()
    {
        // no need to return value , this function will directly update the object value  
        _Value = UpperFirstLetterOfEachWord(_Value);
    }

    static string  LowerFirstLetterOfEachWord(string S1)
    {

        bool isFirstLetter = true;

        for (short i = 0; i < S1.length(); i++)
        {

            if (S1[i] != ' ' && isFirstLetter)
            {
                S1[i] = tolower(S1[i]);

            }

            isFirstLetter = (S1[i] == ' ' ? true : false);

        }

        return S1;
    }

    void  LowerFirstLetterOfEachWord()
    {


        // no need to return value , this function will directly update the object value  
        _Value = LowerFirstLetterOfEachWord(_Value);
    }

    static string  UpperAllString(string S1)
    {
        for (short i = 0; i < S1.length(); i++)
        {
            S1[i] = toupper(S1[i]);
        }
        return S1;
    }

    void  UpperAllString()
    {
        _Value = UpperAllString(_Value);
    }

    static string  LowerAllString(string S1)
    {
        for (short i = 0; i < S1.length(); i++)
        {
            S1[i] = tolower(S1[i]);
        }
        return S1;
    }

    void  LowerAllString()
    {
        _Value = LowerAllString(_Value);
    }

    static char  InvertLetterCase(char char1)
    {
        return isupper(char1) ? tolower(char1) : toupper(char1);
    }

    static string  InvertAllLettersCase(string S1)
    {
        for (short i = 0; i < S1.length(); i++)
        {
            S1[i] = InvertLetterCase(S1[i]);
        }
        return S1;
    }

    void  InvertAllLettersCase()
    {
        _Value = InvertAllLettersCase(_Value);
    }

    enum enWhatToCount { SmallLetters = 0, CapitalLetters = 1, All = 3 };

    static short CountLetters(string S1, enWhatToCount WhatToCount = enWhatToCount::All)
    {


        if (WhatToCount == enWhatToCount::All)
        {
            return S1.length();
        }

        short Counter = 0;

        for (short i = 0; i < S1.length(); i++)
        {

            if (WhatToCount == enWhatToCount::CapitalLetters && isupper(S1[i]))
                Counter++;


            if (WhatToCount == enWhatToCount::SmallLetters && islower(S1[i]))
                Counter++;


        }

        return Counter;

    }

    static short  CountCapitalLetters(string S1)
    {

        short Counter = 0;

        for (short i = 0; i < S1.length(); i++)
        {

            if (isupper(S1[i]))
                Counter++;

        }

        return Counter;
    }

    short  CountCapitalLetters()
    {
        return CountCapitalLetters(_Value);
    }

    static short  CountSmallLetters(string S1)
    {

        short Counter = 0;

        for (short i = 0; i < S1.length(); i++)
        {

            if (islower(S1[i]))
                Counter++;

        }

        return Counter;
    }

    short  CountSmallLetters()
    {
        return CountSmallLetters(_Value);
    }

    static short  CountSpecificLetter(string S1, char Letter, bool MatchCase = true)
    {

        short Counter = 0;

        for (short i = 0; i < S1.length(); i++)
        {

            if (MatchCase)
            {
                if (S1[i] == Letter)
                    Counter++;
            }
            else
            {
                if (tolower(S1[i]) == tolower(Letter))
                    Counter++;
            }

        }

        return Counter;
    }

    short  CountSpecificLetter(char Letter, bool MatchCase = true)
    {
        return CountSpecificLetter(_Value, Letter, MatchCase);
    }

    static bool IsVowel(char Ch1)
    {
        Ch1 = tolower(Ch1);

        return ((Ch1 == 'a') || (Ch1 == 'e') || (Ch1 == 'i') || (Ch1 == 'o') || (Ch1 == 'u'));

    }

    static short  CountVowels(string S1)
    {

        short Counter = 0;

        for (short i = 0; i < S1.length(); i++)
        {

            if (IsVowel(S1[i]))
                Counter++;

        }

        return Counter;
    }

    short  CountVowels()
    {
        return CountVowels(_Value);
    }

    static vector<string> Split(string S1, string Delim)
    {

        vector<string> vString;

        short pos = 0;
        string sWord; // define a string variable  

        // use find() function to get the position of the delimiters  
        while ((pos = S1.find(Delim)) != std::string::npos)
        {
            sWord = S1.substr(0, pos); // store the word   
           // if (sWord != "")
           // {
                vString.push_back(sWord);
            //}

            S1.erase(0, pos + Delim.length());  /* erase() until positon and move to next word. */
        }

        if (S1 != "")
        {
            vString.push_back(S1); // it adds last word of the string.
        }

        return vString;

    }

    vector<string> Split(string Delim)
    {
        return Split(_Value, Delim);
    }

    static string TrimLeft(string S1)
    {


        for (short i = 0; i < S1.length(); i++)
        {
            if (S1[i] != ' ')
            {
                return S1.substr(i, S1.length() - i);
            }
        }
        return "";
    }

    void TrimLeft()
    {
        _Value = TrimLeft(_Value);
    }

    static string TrimRight(string S1)
    {


        for (short i = S1.length() - 1; i >= 0; i--)
        {
            if (S1[i] != ' ')
            {
                return S1.substr(0, i + 1);
            }
        }
        return "";
    }

    void TrimRight()
    {
        _Value = TrimRight(_Value);
    }

    static string Trim(string S1)
    {
        return (TrimLeft(TrimRight(S1)));

    }

    void Trim()
    {
        _Value = Trim(_Value);
    }

    static string JoinString(vector<string> vString, string Delim)
    {

        string S1 = "";

        for (string& s : vString)
        {
            S1 = S1 + s + Delim;
        }

        return S1.substr(0, S1.length() - Delim.length());


    }

    static string JoinString(string arrString[], short Length, string Delim)
    {

        string S1 = "";

        for (short i = 0; i < Length; i++)
        {
            S1 = S1 + arrString[i] + Delim;
        }

        return S1.substr(0, S1.length() - Delim.length());

    }

    static string ReverseWordsInString(string S1)
    {

        vector<string> vString;
        string S2 = "";

        vString = Split(S1, " ");

        // declare iterator
        vector<string>::iterator iter = vString.end();

        while (iter != vString.begin())
        {

            --iter;

            S2 += *iter + " ";

        }

        S2 = S2.substr(0, S2.length() - 1); //remove last space.

        return S2;
    }

    void ReverseWordsInString()
    {
        _Value = ReverseWordsInString(_Value);
    }

    static string ReplaceWord(string S1, string StringToReplace, string sRepalceTo, bool MatchCase = true)
    {

        vector<string> vString = Split(S1, " ");

        for (string& s : vString)
        {

            if (MatchCase)
            {
                if (s == StringToReplace)
                {
                    s = sRepalceTo;
                }

            }
            else
            {
                if (LowerAllString(s) == LowerAllString(StringToReplace))
                {
                    s = sRepalceTo;
                }

            }

        }

        return JoinString(vString, " ");
    }

    string ReplaceWord(string StringToReplace, string sRepalceTo)
    {
        return ReplaceWord(_Value, StringToReplace, sRepalceTo);
    }

    static string RemovePunctuations(string S1)
    {

        string S2 = "";

        for (short i = 0; i < S1.length(); i++)
        {
            if (!ispunct(S1[i]))
            {
                S2 += S1[i];
            }
        }

        return S2;

    }

    void RemovePunctuations()
    {
        _Value = RemovePunctuations(_Value);
    }


};


```

→ `clsDate` : 

```cpp

#pragma warning(disable : 4996)
#pragma once

#include<iostream>
#include<string>
#include "clsString.h"

using namespace std;

class clsDate
{

private:

	short _Day = 1;
	short _Month = 1;
	short _Year = 1900;

public:

	clsDate()
	{
		time_t t = time(0);
		tm* now = localtime(&t);
		_Day = now->tm_mday;
		_Month = now->tm_mon + 1;
		_Year = now->tm_year + 1900;
	}

	clsDate(string sDate)
	{

		vector <string> vDate;
		vDate = clsString::Split(sDate, "/");

		_Day = stoi(vDate[0]);
		_Month = stoi(vDate[1]);
		_Year = stoi(vDate[2]);

	}

	clsDate(short Day, short Month, short Year)
	{

		_Day = Day;
		_Month = Month;
		_Year = Year;

	}

	clsDate(short DateOrderInYear, short Year)
	{
		//This will construct a date by date order in year
		clsDate Date1 = GetDateFromDayOrderInYear(DateOrderInYear, Year);
		_Day = Date1.Day;
		_Month = Date1.Month;
		_Year = Date1.Year;
	}

	void SetDay(short Day) {
		_Day = Day;
	}

	short GetDay() {
		return _Day;
	}
	__declspec(property(get = GetDay, put = SetDay)) short Day;

	void SetMonth(short Month) {
		_Month = Month;
	}

	short GetMonth() {
		return _Month;
	}
	__declspec(property(get = GetMonth, put = SetMonth)) short Month;


	void SetYear(short Year) {
		_Year = Year;
	}

	short GetYear() {
		return _Year;
	}
	__declspec(property(get = GetYear, put = SetYear)) short Year;

	void Print()
	{
		cout << DateToString() << endl;
	}

	static clsDate GetSystemDate()
	{
		//system date
		time_t t = time(0);
		tm* now = localtime(&t);

		short Day, Month, Year;

		Year = now->tm_year + 1900;
		Month = now->tm_mon + 1;
		Day = now->tm_mday;

		return clsDate(Day, Month, Year);
	}

	static	bool IsValidDate(clsDate Date)
	{

		if (Date.Day < 1 || Date.Day>31)
			return false;

		if (Date.Month < 1 || Date.Month>12)
			return false;

		if (Date.Month == 2)
		{
			if (isLeapYear(Date.Year))
			{
				if (Date.Day > 29)
					return false;
			}
			else
			{
				if (Date.Day > 28)
					return false;
			}
		}

		short DaysInMonth = NumberOfDaysInAMonth(Date.Month, Date.Year);

		if (Date.Day > DaysInMonth)
			return false;

		return true;

	}

	bool IsValid()
	{
		return IsValidDate(*this);
	}

	static string DateToString(clsDate Date)
	{
		return  to_string(Date.Day) + "/" + to_string(Date.Month) + "/" + to_string(Date.Year);
	}

	string DateToString()
	{
		return  DateToString(*this);
	}

	static bool isLeapYear(short Year)
	{

		// if year is divisible by 4 AND not divisible by 100
	  // OR if year is divisible by 400
	  // then it is a leap year
		return (Year % 4 == 0 && Year % 100 != 0) || (Year % 400 == 0);
	}

	bool isLeapYear()
	{
		return isLeapYear(_Year);
	}

	static short NumberOfDaysInAYear(short Year)
	{
		return  isLeapYear(Year) ? 365 : 364;
	}

	short NumberOfDaysInAYear()
	{
		return  NumberOfDaysInAYear(_Year);
	}

	static short NumberOfHoursInAYear(short Year)
	{
		return  NumberOfDaysInAYear(Year) * 24;
	}

	short NumberOfHoursInAYear()
	{
		return  NumberOfHoursInAYear(_Year);
	}

	static int NumberOfMinutesInAYear(short Year)
	{
		return  NumberOfHoursInAYear(Year) * 60;
	}

	int NumberOfMinutesInAYear()
	{
		return  NumberOfMinutesInAYear(_Year);
	}

	static int NumberOfSecondsInAYear(short Year)
	{
		return  NumberOfMinutesInAYear(Year) * 60;
	}

	int NumberOfSecondsInAYear()
	{
		return  NumberOfSecondsInAYear();
	}

	static short NumberOfDaysInAMonth(short Month, short Year)
	{

		if (Month < 1 || Month>12)
			return  0;

		int days[12] = { 31,28,31,30,31,30,31,31,30,31,30,31 };
		return (Month == 2) ? (isLeapYear(Year) ? 29 : 28) : days[Month - 1];

	}

	short NumberOfDaysInAMonth()
	{
		return NumberOfDaysInAMonth(_Month, _Year);
	}

	static short NumberOfHoursInAMonth(short Month, short Year)
	{
		return  NumberOfDaysInAMonth(Month, Year) * 24;
	}

	short NumberOfHoursInAMonth()
	{
		return  NumberOfDaysInAMonth(_Month, _Year) * 24;
	}

	static int NumberOfMinutesInAMonth(short Month, short Year)
	{
		return  NumberOfHoursInAMonth(Month, Year) * 60;
	}

	int NumberOfMinutesInAMonth()
	{
		return  NumberOfHoursInAMonth(_Month, _Year) * 60;
	}

	static int NumberOfSecondsInAMonth(short Month, short Year)
	{
		return  NumberOfMinutesInAMonth(Month, Year) * 60;
	}

	int NumberOfSecondsInAMonth()
	{
		return  NumberOfMinutesInAMonth(_Month, _Year) * 60;
	}

	static short DayOfWeekOrder(short Day, short Month, short Year)
	{
		short a, y, m;
		a = (14 - Month) / 12;
		y = Year - a;
		m = Month + (12 * a) - 2;
		// Gregorian:
		//0:sun, 1:Mon, 2:Tue...etc
		return (Day + y + (y / 4) - (y / 100) + (y / 400) + ((31 * m) / 12)) % 7;
	}

	short DayOfWeekOrder()
	{
		return DayOfWeekOrder(_Day, _Month, _Year);
	}

	static string DayShortName(short DayOfWeekOrder)
	{
		string arrDayNames[] = { "Sun","Mon","Tue","Wed","Thu","Fri","Sat" };

		return arrDayNames[DayOfWeekOrder];

	}

	static string DayShortName(short Day, short Month, short Year)
	{

		string arrDayNames[] = { "Sun","Mon","Tue","Wed","Thu","Fri","Sat" };

		return arrDayNames[DayOfWeekOrder(Day, Month, Year)];

	}

	string DayShortName()
	{

		string arrDayNames[] = { "Sun","Mon","Tue","Wed","Thu","Fri","Sat" };

		return arrDayNames[DayOfWeekOrder(_Day, _Month, _Year)];

	}

	static string MonthShortName(short MonthNumber)
	{
		string Months[12] = { "Jan", "Feb", "Mar",
						   "Apr", "May", "Jun",
						   "Jul", "Aug", "Sep",
						   "Oct", "Nov", "Dec"
		};

		return (Months[MonthNumber - 1]);
	}

	string MonthShortName()
	{

		return MonthShortName(_Month);
	}

	static void PrintMonthCalendar(short Month, short Year)
	{
		int NumberOfDays;

		// Index of the day from 0 to 6
		int current = DayOfWeekOrder(1, Month, Year);

		NumberOfDays = NumberOfDaysInAMonth(Month, Year);

		// Print the current month name
		printf("\n  _______________%s_______________\n\n",
			MonthShortName(Month).c_str());

		// Print the columns
		printf("  Sun  Mon  Tue  Wed  Thu  Fri  Sat\n");

		// Print appropriate spaces
		int i;
		for (i = 0; i < current; i++)
			printf("     ");

		for (int j = 1; j <= NumberOfDays; j++)
		{
			printf("%5d", j);


			if (++i == 7)
			{
				i = 0;
				printf("\n");
			}
		}

		printf("\n  _________________________________\n");

	}

	void PrintMonthCalendar()
	{
		PrintMonthCalendar(_Month, _Year);
	}

	static void PrintYearCalendar(int Year)
	{
		printf("\n  _________________________________\n\n");
		printf("           Calendar - %d\n", Year);
		printf("  _________________________________\n");


		for (int i = 1; i <= 12; i++)
		{
			PrintMonthCalendar(i, Year);
		}

		return;
	}

	void PrintYearCalendar()
	{
		printf("\n  _________________________________\n\n");
		printf("           Calendar - %d\n", _Year);
		printf("  _________________________________\n");


		for (int i = 1; i <= 12; i++)
		{
			PrintMonthCalendar(i, _Year);
		}

		return;
	}

	static short DaysFromTheBeginingOfTheYear(short Day, short Month, short Year)
	{


		short TotalDays = 0;

		for (int i = 1; i <= Month - 1; i++)
		{
			TotalDays += NumberOfDaysInAMonth(i, Year);
		}

		TotalDays += Day;

		return TotalDays;
	}

	short DaysFromTheBeginingOfTheYear()
	{


		short TotalDays = 0;

		for (int i = 1; i <= _Month - 1; i++)
		{
			TotalDays += NumberOfDaysInAMonth(i, _Year);
		}

		TotalDays += _Day;

		return TotalDays;
	}

	static clsDate GetDateFromDayOrderInYear(short DateOrderInYear, short Year)
	{

		clsDate Date;
		short RemainingDays = DateOrderInYear;
		short MonthDays = 0;

		Date.Year = Year;
		Date.Month = 1;

		while (true)
		{
			MonthDays = NumberOfDaysInAMonth(Date.Month, Year);

			if (RemainingDays > MonthDays)
			{
				RemainingDays -= MonthDays;
				Date.Month++;
			}
			else
			{
				Date.Day = RemainingDays;
				break;
			}

		}

		return Date;
	}

	void AddDays(short Days)
	{


		short RemainingDays = Days + DaysFromTheBeginingOfTheYear(_Day, _Month, _Year);
		short MonthDays = 0;

		_Month = 1;

		while (true)
		{
			MonthDays = NumberOfDaysInAMonth(_Month, _Year);

			if (RemainingDays > MonthDays)
			{
				RemainingDays -= MonthDays;
				_Month++;

				if (_Month > 12)
				{
					_Month = 1;
					_Year++;

				}
			}
			else
			{
				_Day = RemainingDays;
				break;
			}

		}


	}

	static bool IsDate1BeforeDate2(clsDate Date1, clsDate Date2)
	{
		return  (Date1.Year < Date2.Year) ? true : ((Date1.Year == Date2.Year) ? (Date1.Month < Date2.Month ? true : (Date1.Month == Date2.Month ? Date1.Day < Date2.Day : false)) : false);
	}

	bool IsDateBeforeDate2(clsDate Date2)
	{
		//note: *this sends the current object :-) 
		return  IsDate1BeforeDate2(*this, Date2);

	}

	static bool IsDate1EqualDate2(clsDate Date1, clsDate Date2)
	{
		return  (Date1.Year == Date2.Year) ? ((Date1.Month == Date2.Month) ? ((Date1.Day == Date2.Day) ? true : false) : false) : false;
	}

	bool IsDateEqualDate2(clsDate Date2)
	{
		return  IsDate1EqualDate2(*this, Date2);
	}

	static bool IsLastDayInMonth(clsDate Date)
	{

		return (Date.Day == NumberOfDaysInAMonth(Date.Month, Date.Year));

	}

	bool IsLastDayInMonth()
	{

		return IsLastDayInMonth(*this);

	}

	static bool IsLastMonthInYear(short Month)
	{
		return (Month == 12);
	}

	static clsDate AddOneDay(clsDate Date)
	{
		if (IsLastDayInMonth(Date))
		{
			if (IsLastMonthInYear(Date.Month))
			{
				Date.Month = 1;
				Date.Day = 1;
				Date.Year++;
			}
			else
			{
				Date.Day = 1;
				Date.Month++;
			}
		}
		else
		{
			Date.Day++;
		}

		return Date;
	}

	void AddOneDay()

	{
		*this = AddOneDay(*this);
	}

	static void  SwapDates(clsDate& Date1, clsDate& Date2)
	{

		clsDate TempDate;
		TempDate = Date1;
		Date1 = Date2;
		Date2 = TempDate;

	}

	static int GetDifferenceInDays(clsDate Date1, clsDate Date2, bool IncludeEndDay = false)
	{
		//this will take care of negative diff
		int Days = 0;
		short SawpFlagValue = 1;

		if (!IsDate1BeforeDate2(Date1, Date2))
		{
			//Swap Dates 
			SwapDates(Date1, Date2);
			SawpFlagValue = -1;

		}

		while (IsDate1BeforeDate2(Date1, Date2))
		{
			Days++;
			Date1 = AddOneDay(Date1);
		}

		return IncludeEndDay ? ++Days * SawpFlagValue : Days * SawpFlagValue;
	}

	int GetDifferenceInDays(clsDate Date2, bool IncludeEndDay = false)
	{
		return GetDifferenceInDays(*this, Date2, IncludeEndDay);
	}

	static short CalculateMyAgeInDays(clsDate DateOfBirth)
	{
		return GetDifferenceInDays(DateOfBirth, clsDate::GetSystemDate(), true);
	}
	//above no need to have nonstatic function for the object because it does not depend on any data from it.

	static clsDate IncreaseDateByOneWeek(clsDate& Date)
	{

		for (int i = 1; i <= 7; i++)
		{
			Date = AddOneDay(Date);
		}

		return Date;
	}

	void IncreaseDateByOneWeek()
	{
		IncreaseDateByOneWeek(*this);
	}

	clsDate IncreaseDateByXWeeks(short Weeks, clsDate& Date)
	{

		for (short i = 1; i <= Weeks; i++)
		{
			Date = IncreaseDateByOneWeek(Date);
		}
		return Date;
	}

	void IncreaseDateByXWeeks(short Weeks)
	{
		IncreaseDateByXWeeks(Weeks, *this);
	}

	clsDate IncreaseDateByOneMonth(clsDate& Date)
	{

		if (Date.Month == 12)
		{
			Date.Month = 1;
			Date.Year++;
		}
		else
		{
			Date.Month++;
		}

		//last check day in date should not exceed max days in the current month
		// example if date is 31/1/2022 increasing one month should not be 31/2/2022, it should
		// be 28/2/2022
		short NumberOfDaysInCurrentMonth = NumberOfDaysInAMonth(Date.Month, Date.Year);
		if (Date.Day > NumberOfDaysInCurrentMonth)
		{
			Date.Day = NumberOfDaysInCurrentMonth;
		}

		return Date;
	}

	void IncreaseDateByOneMonth()
	{

		IncreaseDateByOneMonth(*this);

	}

	clsDate IncreaseDateByXDays(short Days, clsDate& Date)
	{

		for (short i = 1; i <= Days; i++)
		{
			Date = AddOneDay(Date);
		}
		return Date;
	}

	void IncreaseDateByXDays(short Days)
	{

		IncreaseDateByXDays(Days, *this);
	}

	clsDate IncreaseDateByXMonths(short Months, clsDate& Date)
	{

		for (short i = 1; i <= Months; i++)
		{
			Date = IncreaseDateByOneMonth(Date);
		}
		return Date;
	}

	void IncreaseDateByXMonths(short Months)
	{
		IncreaseDateByXMonths(Months, *this);
	}

	static clsDate IncreaseDateByOneYear(clsDate& Date)
	{
		Date.Year++;
		return Date;
	}

	void IncreaseDateByOneYear()
	{
		IncreaseDateByOneYear(*this);
	}

	clsDate IncreaseDateByXYears(short Years, clsDate& Date)
	{
		Date.Year += Years;
		return Date;

	}

	void IncreaseDateByXYears(short Years)
	{
		IncreaseDateByXYears(Years);
	}

	clsDate IncreaseDateByOneDecade(clsDate& Date)
	{
		//Period of 10 years
		Date.Year += 10;
		return Date;
	}

	void IncreaseDateByOneDecade()
	{
		IncreaseDateByOneDecade(*this);
	}

	clsDate IncreaseDateByXDecades(short Decade, clsDate& Date)
	{
		Date.Year += Decade * 10;
		return Date;
	}

	void IncreaseDateByXDecades(short Decade)
	{
		IncreaseDateByXDecades(Decade, *this);
	}

	clsDate IncreaseDateByOneCentury(clsDate& Date)
	{
		//Period of 100 years
		Date.Year += 100;
		return Date;
	}

	void IncreaseDateByOneCentury()
	{
		IncreaseDateByOneCentury(*this);
	}

	clsDate IncreaseDateByOneMillennium(clsDate& Date)
	{
		//Period of 1000 years
		Date.Year += 1000;
		return Date;
	}

	clsDate IncreaseDateByOneMillennium()
	{
		IncreaseDateByOneMillennium(*this);
	}

	static clsDate DecreaseDateByOneDay(clsDate Date)
	{
		if (Date.Day == 1)
		{
			if (Date.Month == 1)
			{
				Date.Month = 12;
				Date.Day = 31;
				Date.Year--;
			}
			else
			{

				Date.Month--;
				Date.Day = NumberOfDaysInAMonth(Date.Month, Date.Year);
			}
		}
		else
		{
			Date.Day--;
		}

		return Date;
	}

	void DecreaseDateByOneDay()
	{
		DecreaseDateByOneDay(*this);
	}

	static clsDate DecreaseDateByOneWeek(clsDate& Date)
	{

		for (int i = 1; i <= 7; i++)
		{
			Date = DecreaseDateByOneDay(Date);
		}

		return Date;
	}

	void DecreaseDateByOneWeek()
	{
		DecreaseDateByOneWeek(*this);
	}

	static clsDate DecreaseDateByXWeeks(short Weeks, clsDate& Date)
	{

		for (short i = 1; i <= Weeks; i++)
		{
			Date = DecreaseDateByOneWeek(Date);
		}
		return Date;
	}

	void DecreaseDateByXWeeks(short Weeks)
	{
		DecreaseDateByXWeeks(Weeks, *this);
	}

	static clsDate DecreaseDateByOneMonth(clsDate& Date)
	{

		if (Date.Month == 1)
		{
			Date.Month = 12;
			Date.Year--;
		}
		else
			Date.Month--;


		//last check day in date should not exceed max days in the current month
	   // example if date is 31/3/2022 decreasing one month should not be 31/2/2022, it should
	   // be 28/2/2022
		short NumberOfDaysInCurrentMonth = NumberOfDaysInAMonth(Date.Month, Date.Year);
		if (Date.Day > NumberOfDaysInCurrentMonth)
		{
			Date.Day = NumberOfDaysInCurrentMonth;
		}


		return Date;
	}

	void DecreaseDateByOneMonth()
	{
		DecreaseDateByOneMonth(*this);
	}

	static clsDate DecreaseDateByXDays(short Days, clsDate& Date)
	{

		for (short i = 1; i <= Days; i++)
		{
			Date = DecreaseDateByOneDay(Date);
		}
		return Date;
	}

	void DecreaseDateByXDays(short Days)
	{
		DecreaseDateByXDays(Days, *this);
	}

	static clsDate DecreaseDateByXMonths(short Months, clsDate& Date)
	{

		for (short i = 1; i <= Months; i++)
		{
			Date = DecreaseDateByOneMonth(Date);
		}
		return Date;
	}

	void DecreaseDateByXMonths(short Months)
	{
		DecreaseDateByXMonths(Months, *this);
	}

	static clsDate DecreaseDateByOneYear(clsDate& Date)
	{

		Date.Year--;
		return Date;
	}

	void DecreaseDateByOneYear()
	{
		DecreaseDateByOneYear(*this);
	}

	static clsDate DecreaseDateByXYears(short Years, clsDate& Date)
	{

		Date.Year -= Years;
		return Date;
	}

	void DecreaseDateByXYears(short Years)
	{
		DecreaseDateByXYears(Years, *this);
	}

	static clsDate DecreaseDateByOneDecade(clsDate& Date)
	{
		//Period of 10 years
		Date.Year -= 10;
		return Date;
	}

	void DecreaseDateByOneDecade()
	{
		DecreaseDateByOneDecade(*this);
	}

	static clsDate DecreaseDateByXDecades(short Decades, clsDate& Date)
	{

		Date.Year -= Decades * 10;
		return Date;
	}

	void DecreaseDateByXDecades(short Decades)
	{
		DecreaseDateByXDecades(Decades, *this);
	}

	static clsDate DecreaseDateByOneCentury(clsDate& Date)
	{
		//Period of 100 years
		Date.Year -= 100;
		return Date;
	}

	void DecreaseDateByOneCentury()
	{
		DecreaseDateByOneCentury(*this);
	}

	static clsDate DecreaseDateByOneMillennium(clsDate& Date)
	{
		//Period of 1000 years
		Date.Year -= 1000;
		return Date;
	}

	void DecreaseDateByOneMillennium()
	{
		DecreaseDateByOneMillennium(*this);
	}


	static short IsEndOfWeek(clsDate Date)
	{
		return  DayOfWeekOrder(Date.Day, Date.Month, Date.Year) == 6;
	}

	short IsEndOfWeek()
	{
		return IsEndOfWeek(*this);
	}

	static bool IsWeekEnd(clsDate Date)
	{
		//Weekends are Fri and Sat
		short DayIndex = DayOfWeekOrder(Date.Day, Date.Month, Date.Year);
		return  (DayIndex == 5 || DayIndex == 6);
	}

	bool IsWeekEnd()
	{
		return  IsWeekEnd(*this);
	}

	static bool IsBusinessDay(clsDate Date)
	{
		//Weekends are Sun,Mon,Tue,Wed and Thur

	   /*
		short DayIndex = DayOfWeekOrder(Date.Day, Date.Month, Date.Year);
		return  (DayIndex >= 5 && DayIndex <= 4);
	   */

	   //shorter method is to invert the IsWeekEnd: this will save updating code.
		return !IsWeekEnd(Date);

	}

	bool IsBusinessDay()
	{
		return  IsBusinessDay(*this);
	}

	static short DaysUntilTheEndOfWeek(clsDate Date)
	{
		return 6 - DayOfWeekOrder(Date.Day, Date.Month, Date.Year);
	}

	short DaysUntilTheEndOfWeek()
	{
		return  DaysUntilTheEndOfWeek(*this);
	}

	static short DaysUntilTheEndOfMonth(clsDate Date1)
	{

		clsDate EndOfMontDate;
		EndOfMontDate.Day = NumberOfDaysInAMonth(Date1.Month, Date1.Year);
		EndOfMontDate.Month = Date1.Month;
		EndOfMontDate.Year = Date1.Year;

		return GetDifferenceInDays(Date1, EndOfMontDate, true);

	}

	short DaysUntilTheEndOfMonth()
	{
		return DaysUntilTheEndOfMonth(*this);
	}

	static short DaysUntilTheEndOfYear(clsDate Date1)
	{

		clsDate EndOfYearDate;
		EndOfYearDate.Day = 31;
		EndOfYearDate.Month = 12;
		EndOfYearDate.Year = Date1.Year;

		return GetDifferenceInDays(Date1, EndOfYearDate, true);

	}

	short DaysUntilTheEndOfYear()
	{
		return  DaysUntilTheEndOfYear(*this);
	}

	//i added this method to calculate business days between 2 days
	static short CalculateBusinessDays(clsDate DateFrom, clsDate DateTo)
	{

		short Days = 0;
		while (IsDate1BeforeDate2(DateFrom, DateTo))
		{
			if (IsBusinessDay(DateFrom))
				Days++;

			DateFrom = AddOneDay(DateFrom);
		}

		return Days;

	}

	static short CalculateVacationDays(clsDate DateFrom, clsDate DateTo)
	{
		/*short Days = 0;
		while (IsDate1BeforeDate2(DateFrom, DateTo))
		{
			if (IsBusinessDay(DateFrom))
				Days++;

			DateFrom = AddOneDay(DateFrom);
		}*/

		return CalculateBusinessDays(DateFrom, DateTo);

	}
	//above method is eough , no need to have method for the object

	static clsDate CalculateVacationReturnDate(clsDate DateFrom, short VacationDays)
	{

		short WeekEndCounter = 0;

		for (short i = 1; i <= VacationDays; i++)
		{

			if (IsWeekEnd(DateFrom))
				WeekEndCounter++;

			DateFrom = AddOneDay(DateFrom);
		}
		//to add weekends 
		for (short i = 1; i <= WeekEndCounter; i++)
			DateFrom = AddOneDay(DateFrom);

		return DateFrom;
	}

	static bool IsDate1AfterDate2(clsDate Date1, clsDate Date2)
	{
		return (!IsDate1BeforeDate2(Date1, Date2) && !IsDate1EqualDate2(Date1, Date2));

	}

	bool IsDateAfterDate2(clsDate Date2)
	{
		return IsDate1AfterDate2(*this, Date2);
	}

	enum enDateCompare { Before = -1, Equal = 0, After = 1 };

	static enDateCompare CompareDates(clsDate Date1, clsDate Date2)
	{
		if (IsDate1BeforeDate2(Date1, Date2))
			return enDateCompare::Before;

		if (IsDate1EqualDate2(Date1, Date2))
			return enDateCompare::Equal;

		/* if (IsDate1AfterDate2(Date1,Date2))
			 return enDateCompare::After;*/

			 //this is faster
		return enDateCompare::After;

	}

	enDateCompare CompareDates(clsDate Date2)
	{
		return CompareDates(*this, Date2);
	}



};


```


