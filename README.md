# Description

A very simple ledger-parser for ledger-cli journal files. It probably does not parse everything and it is probably buggy. Handle with care!

# Usage

Simply import this module either by putting the `ledger-parse.py` file into the folder of your python script, or into the pythons modules path (maybe something like `/usr/lib/python2.7`). Then just use `import ledger-parse` to import the module.

A good workflow for me is: generating a symbolic link to my python2.7 lib path so that I can import the module even if it's not in the actual pythons script path, while using always the latest version of the script. You can make such a symbolic link by entering the ledger-parse folder and firing this command: `sudo ln -s $PWD/ledgerparse.py /usr/lib/python2.7/ledgerparse.py`.