# Loan Qualifier Application
This program allows a customer to input they're financial information to recieve all available loans that they qualify for. This automates the the qualifying process for the lender and saves time for both the cusotmer and the lender.
---

## Technologies
### Languages used:
python 3.7.10 64-bit (conda)
### Libraries used:
sys
fire
csv
pathlib
typing
questionary
---

## Installation Guide
### Before writing the functions the following libraries required installation as so.
'''
import sys
from typing import NewType
import fire
import questionary
from pathlib import Path

#added function
import csv
from pathlib import Path
'''
---

## Usage
This part of the code imports all necassary modules containing the functions and filters needed to complete the program.
'''
import csv
from pathlib import Path

    
from qualifier.utils.fileio import load_csv, save_csv

from qualifier.utils.calculators import (
    calculate_monthly_debt_ratio,
    calculate_loan_to_value_ratio,
)

from qualifier.filters.max_loan_size import filter_max_loan_size
from qualifier.filters.credit_score import filter_credit_score
from qualifier.filters.debt_to_income import filter_debt_to_income
from qualifier.filters.loan_to_value import filter_loan_to_value

'''

This function asks the user to import the necasarry data rate sheet containing all bank loans before they are filtered. If user input is anything but the correct data rate sheet an error will return and the function will close.
'''
def load_bank_data():
    """Ask for the file path to the latest banking data and load the CSV file.

    Returns:
        The bank data from the data rate sheet CSV file.
    """

    csvpath = questionary.text("Enter a file path to a rate-sheet (.csv):").ask()
    csvpath = Path(csvpath)
    if not csvpath.exists():
        sys.exit(f"Oops! Can't find this path: {csvpath}")

    return load_csv(csvpath)
'''
This function provides user dialogue to gather data to be used to filter the data rate sheet. It also turns the answers into int and float type numerical values so that they can be used in expressions.
'''
def get_applicant_info():
    """Prompt dialog to get the applicant's financial information.

    Returns:
        Returns the applicant's financial information.
    """

    credit_score = questionary.text("What's your credit score?").ask()
    debt = questionary.text("What's your current amount of monthly debt?").ask()
    income = questionary.text("What's your total monthly income?").ask()
    loan_amount = questionary.text("What's your desired loan amount?").ask()
    home_value = questionary.text("What's your home value?").ask()

    credit_score = int(credit_score)
    debt = float(debt)
    income = float(income)
    loan_amount = float(loan_amount)
    home_value = float(home_value)

    return credit_score, debt, income, loan_amount, home_value
'''

'''
def find_qualifying_loans(bank_data, credit_score, debt, income, loan, home_value):

    # Calculate the monthly debt ratio
    monthly_debt_ratio = calculate_monthly_debt_ratio(debt, income)
    print(f"The monthly debt to income ratio is {monthly_debt_ratio:.02f}")

    # Calculate loan to value ratio
    loan_to_value_ratio = calculate_loan_to_value_ratio(loan, home_value)
    print(f"The loan to value ratio is {loan_to_value_ratio:.02f}.")

    # Run qualification filters
    bank_data_filtered = filter_max_loan_size(loan, bank_data)
    bank_data_filtered = filter_credit_score(credit_score, bank_data_filtered)
    bank_data_filtered = filter_debt_to_income(monthly_debt_ratio, bank_data_filtered)
    bank_data_filtered = filter_loan_to_value(loan_to_value_ratio, bank_data_filtered)

    print(f"Found {len(bank_data_filtered)} qualifying loans")

    return bank_data_filtered
'''
This function saves the qualifying loans to a csv file of the users choice if they want to. If they dont want to save the results for viewing the program will exit. If there are no qualifing loans the program will exit.
'''
def save_qualifying_loans(qualifying_loans):
    """Saves the qualifying loans to a CSV file.

    Args:
        qualifying_loans (list of lists): The qualifying bank loans.
    """
    # @TODO: Complete the usability dialog for saving the CSV Files.
    # YOUR CODE HERE!
    if len(qualifying_loans) == 0:
        sys.exit("no qualifying loans")
    save_file_confirmation = questionary.confirm("Would you like to save your results to a csv file?").ask()    
    if save_file_confirmation == False:
        sys.exit("Your file is not going to be saved")
    elif save_file_confirmation ==True:
        filepath = questionary.text("enter file path for desired save location.").ask()
        save_csv(filepath, qualifying_loans)
'''
This is the main function of the script which runs the loan qualifier application.
'''
def run():
    """The main function for running the script."""

    # Load the latest Bank data
    bank_data = load_bank_data()

    # Get the applicant's information
    credit_score, debt, income, loan_amount, home_value = get_applicant_info()

    # Find qualifying loans
    qualifying_loans = find_qualifying_loans(
        bank_data, credit_score, debt, income, loan_amount, home_value
    )

    # Save qualifying loans
    save_qualifying_loans(qualifying_loans)

if __name__ == "__main__":
    fire.Fire(run)
'''
---

## Contributors
### Main and only contributor:
Niicholas strohm
### Social Media:
 LinkedIn: Nick strohm
 Email: strohm241@gmail.com
---

## License
MIT License

Copyright (c) [2021] [Nicholas_Strohm]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files, to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
When you share a project on a repository, especially a public one, it's important to choose the right license to specify what others can and can't with your source code and files. Use this section to include the license you want to use.
