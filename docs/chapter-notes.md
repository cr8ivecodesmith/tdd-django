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

#### Modifications

Since I like putting my templates in a central location (rather than inside an
app), I've added this in `settings.py`:
```python
# See: https://docs.djangoproject.com/en/dev/ref/settings/#template-dirs
TEMPLATE_DIRS = (
    os.path.join(BASE_DIR, 'templates'),
)
```

## chapter 5

The chapter attempts to demonstrate how TDD can support an iterative
development style. Instead of skipping ahead and creating the various
templates, models, and tests, we'll go through it one step at a time.

Debugging the initial (403) error says that a CSRF verification failed. CSRF
also known as _Cross-Site Request Forgery_, is a kind of exploit. The author
goes on and recommends the book _Ross Anderson’s Security Engineering_ as a
good read on different ways technology is used in unexpected ways.

Best practice tells us to [always redirect after a POST](https://en.wikipedia.org/wiki/Post/Redirect/Get).
So when a POST request is made and processed, your _views_ should make a _302_.


#### Debugging an unexpected failure
- Add print statements to display the output
- Improve the _error message_ to show more info
- Visit the site manually
- Use time.sleep to pause the test during execution

#### Red / Green / Refactor and Triangulation
**Red** - Write a test that fails
**Green** - Write simplest code that passes even if it means we're _cheating_!
**Refactor** - To have a code that makes more sense
**Triangulation** - A specific test case for existing code to justify
    generalising the implementation.

A refactor is justifed when there's code duplication _(i.e. using constants in
templates to pass a test)_

#### Dealing with Code Smell
When a [code smell](http://en.wikipedia.org/wiki/Code_smell) is detected, the
normal way of dealing with it is to take a note of it in the code base and deal
with it later.

But that's not always the case.

It will have to depend on whether the problem is too big to ignore.

#### Useful TDD concepts

**Regression** - When new code breaks current application behaviour.

**Unexpected failure** - When the test produces an error the wasn't intentional.
    Either there's something wrong in our test or it helped us find a
    regression that we need to fix.

**3 strikes and a refactor** - A rule-of-thumb that tells us when to refactor
    code duplication.

**Scratchpad to-do list** - A place to write things we wanted to work on next or
    later.

#### Modifications
Since I was doing these exercises from a headless environment, I modified the
code to make use of a virtual display server where Firefox can run.

At some point my application keeps on exiting for some weird reason. I tried to
see if I did something wrong in my code but can't seem to find it. Then I
suspected that there might be something wrong with VM. I ran `htop` and true
enough my RAM is at max. Firefox can no longer run.

I then saw that I have several instances of my virtual display server running
all consuming about 2.5% of RAM. There's my culprit!

I rebooted the VM and fixed my `functional_tests.py` so that the instantiation
of the virtual display is included in the `setUp` and `tearDown` of the test.
Hopefully this fixes the problem.
