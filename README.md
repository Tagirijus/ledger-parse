# Description

A very simple ledger-parser for ledger-cli journal files. It probably does not parse everything and it is probably buggy. Handle with care!

# Installation

Simply import this module either by putting the `ledger-parse.py` file into the folder of your python script, or into the pythons modules path (maybe something like `/usr/lib/python2.7`). Then just use `import ledgerparse` to import the module.

A good workflow for me is: generating a symbolic link to my python2.7 lib path so that I can import the module even if it's not in the actual pythons script path, while using always the latest version of the script. You can make such a symbolic link by entering the ledger-parse folder and firing this command: `sudo ln -s $PWD/ledgerparse.py /usr/lib/python2.7/ledgerparse.py`.

# Usage

Load a ledger formatted journal into a variable - get the content as a simple string. With this string you use the `ledgerparse.string_to_ledger()` function to generate a list of `ledgerparse.ledger_transaction` objects. Example:

```python
ìmport ledgerparse

# get the content of the file into a variable
f = open('ledger.journal')
JOURNAL_STRING = f.read()
f.close()

# convert the string into a list with transaction-objects
JOURNAL = ledgerparse.string_to_ledger(JOURNAL_STRING)
```

Then you have all the transaction entries from the ledger journal in a list and can access them for example like this:

```python
# print the first transaction - this will print a string
print JOURNAL[0]

# get the payee of the first transaction
print JOURNAL[0].payee

# print the accounts with their amount of the first transaction
for account in JOURNAL[0].accounts:
    print account
```

These variables are accessable:

```python
# the transaction
JOURNAL[0].date         # a datetime object
JOURNAL[0].aux_date     # a datetime object
JOURNAL[0].state        # string, e.g. '!' or '*'
JOURNAL[0].code         # string
JOURNAL[0].payee        # string
JOURNAL[0].comments     # list of strings
JOURNAL[0].accounts     # list of ledgerparse.ledger_account objects
JOURNAL[0].original     # original string of the transaction

# the account
JOUNRAL[0].accounts[0].name         # string
JOUNRAL[0].accounts[0].commodity    # string
JOUNRAL[0].accounts[0].amount       # ledgerparse.Money object
JOUNRAL[0].accounts[0].comments     # list of strings
```

# Why not the native ledger module?

Simple: teh native ledger module which comes with the ledger-cli programm gets all the ledger journal trasactions as `postings`. A posting can have the connected transaction payee etc. ... but it does not really parse the transactions. I wanted a parser which is closer to the journal like you are probably write: a list of transactions.

IMPORTANT: This module is NOT for calculating like ledger is (at least not YET).

# Example

If you would like to sort a non-sorted ledger-journal, you could use this script like this:

```python
import ledgerparse

f = open('ledger.journal')
JOURNAL_STRING = f.read()
f.close()

SORTED_JOURNAL_STRING = '\n\n'.join([x.get_original() for x in sorted(ledgerparse.string_to_ledger(JOURNAL_STRING), key=lambda y: y.date)])

f = open('ledger_sorted.journal', 'w')
f.write(SORTED_JOURNAL_STRING)
f.close()
```