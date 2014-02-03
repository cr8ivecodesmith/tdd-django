Test Driven Web Development with Python Chapter Notes
=====================================================

## chapter 1

- TDD does not come naturally.
- In TDD, every step has to be tested.
- Development approach is one step at a time.

I tried doing this from a headless server from the get go so I had to change a 
few things in my setup:

- Installed firefox and xvfb
- Added pyvirtualdisplay
- Set up virtualenv

I've also modified the code for the `functional_tests.py` file to use 
pyvirtualdisplay.

I've also pushed this to GitHub:

1. Create an empty repo at GitHub named `tdd-django`.
2. Run the ff. from the command line:

    git remote add origin git@github.com:cr8ivecodesmith/tdd-django.git
    git push -u origin master
