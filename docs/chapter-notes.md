Chapter Notes
=============
_**Test Driven Web Development with Python**_

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
**User story**  
Describes how the application will work from the user's perspective. Used to 
spec out FTs.

**Expected failure**  
When you mean for a test to fail.

## chapter 3

FTs drive what we do in a high level while UTs drive what we do in a low
level.

#### Unit Tests
Differs from Functional Tests as it tests the application from the programmer's
point of view.

#### The TDD Approach
It appears that there's several ways of doing TDD but the one taught in this
book is that it uses both FTs and UTs to drive development.

#### TDD Flow
1. Write FT to describe the workflow from the user's point of view.
2. Write first failing FT. We now use UTs to define the internal behavior by
   writing one or more UTs.
3. Write a failing UT. Start writing the smallest amount of code possible. Just
   enough to pass the UT. Iterate between steps 1 and 2 until we think we can 
   write the next FT.
4. Re-run the FT to see if it passes. This will now give us an idea on how to
   proceed further.

#### Unit tests/code cycle
1. Run test in terminal
2. Make minimal code change
3. Repeat

## chapter 4

TDD is a _discipline_. It doesn't come naturally so you have to stick with it
from the beginning.

*refactoring* - Improving code without changing its functionality. A first rule
    in refactoring is that you can't do it without tests.

While doing a step-by-step approach may seem repetitive and tedious, it will
help a lot once you start working on more complex functionality.

You are either working on the tests or the functionality. Never both.

Beware of the tendency to skip a few steps ahead to add some tweaks or change
the behavior. Stick to small steps and keep refactoring and code changes
seperately.

#### Recap: the TDD process
- Functional tests
- Unit tests
- The unit test/code cycle
- Refactoring

#### Overall TDD process
```
      .....................................
      :                                   :
      :                                   N
      v                                   :
write a test ---> run the test -Y-> does it need
                  did it pass?      refactoring?
                       |   ^              :
                       N   |____          Y
                       |        |         :
                       v        |         :
                write minimal code <.......
```

#### Overall TDD process with Functional and Unit tests
```
      .....................................
      :                                   :
      :                                   N
      v                                   :
write an FT ---> run the test -Y-> does it need
                 did it pass?      refactoring?
                       |   ^              :
                       N   |____          Y
                       |        |         :
                       v        |         :
             (A) write minimal code <......

(A)
      .....................................
      :                                   :
      :                                   N
      v                                   :
write a UT ---> run the test -Y-> does it need
                did it pass?      refactoring?
                       |   ^              :
                       N   |____          Y
                       |        |         :
                       v        |         :
              A. write minimal code <......
```

#### modifications

Since I like putting my templates in a central location (rather than inside an
app), I've added this in `settings.py`:
```python
# See: https://docs.djangoproject.com/en/dev/ref/settings/#template-dirs
TEMPLATE_DIRS = (
    os.path.join(BASE_DIR, 'templates'),
)
```
