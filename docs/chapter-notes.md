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
```
git remote add origin git@github.com:cr8ivecodesmith/tdd-django.git
git push -u origin master
```

## chapter 2

#### Functional Tests
Sometimes called _Acceptance test_, _End-to-end test_, _Integration test_. The
main point however is that they test the application fromt a user's perspective.

FTs should have a human-readable story that describes how the user might 
interact with the application and how the application might react.

The story can be written as comments.

FT comments describe the User Story in detail. By writing it we force ourselves
to always test from the user's perspective. This is related to another concept
called _Behavior-Driven-Development_.

#### Useful TDD Concepts
__User story__  
Describes how the application will work from the user's perspective. Used to 
spec out FTs.

__Expected failure__  
When you mean for a test to fail.

