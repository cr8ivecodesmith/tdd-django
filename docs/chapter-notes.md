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
_User story_ - Describes how the application will work from the user's
perspective. Used to spec out FTs.

_Expected failure_ - When you mean for a test to fail.

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
_Red_ - Write a test that fails

_Green_ - Write simplest code that passes even if it means we're _cheating_!

_Refactor_ - To have a code that makes more sense

_Triangulation_ - A specific test case for existing code to justify
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

_Regression_ - When new code breaks current application behaviour.

_Unexpected failure_ - When the test produces an error the wasn't intentional.
    Either there's something wrong in our test or it helped us find a
    regression that we need to fix.

_3 strikes and a refactor_ - A rule-of-thumb that tells us when to refactor
    code duplication.

_Scratchpad to-do list_ - A place to write things we wanted to work on next or
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

## chapter 6

This chapter will address some of the issues that was raised in the previous
one.

To run tests against a specific app just do `python manage.py test <app_name>`.

Introduced Django's _reverse lookup_ feature. It was used in `list.html`
template file:
```html
...
<table id="id_list_table">
        {% for item in list.item_set.all %}
...
```

The author also introduced this practice of having anything that has to do with
the app contained within the app. In this light, I'm rethinking my stance on 
the directory structure I've come to adapt.

#### Ensuring functional tests isolation
A big one is that our FT writes to the main database and leaves test data
on it. To address that the we'll use a feature introduced in Django 1.6 called
_LiveServerTestCase_, so go ahead to that upgrade if you haven't already.

#### Hand-rolled implementation of test database cleanup
In case you you're not using Django and don't have a feature similar to the
_LiveServerTestCase_ the author suggest a custom implementation. Here's my
slightly modified _(framework agnostic)_ steps:
- Use a different settings file that simple overrides the _databases_
configuration.
- On the `setUp` method, use `subprocess.Popen` to sync your databases and run
run the web server.
- On the `tearDown` method, kill the server and delete the database files if
your using something like sqlite.

#### Design when necessary: YAGNI
TDD is closely tied to the Agile development practice. Normally, a lengthy
requirements meeting is followed by a lengthy design meeting to determine how
the software is gonna look like to meet it. However in the Agile world, we want
to get our application out there the soonest possible time _(think MVP)_ and
let design evolve based on real world experience and usage.

Most of the time, as software engineers, we tend to be over enthusiastic and
begin to sprout ideas of things we _might_ need. This goes on and eventually
you end up with a long list of items/features to work on. All of them seem good
but most of them will be unnecessary for the MVP. To avoid this we follow the
mantra: YAGNI - _You Ain't Gonna Need It_.

#### REST-inspired design
The [REpresentational State Transfer](http://en.wikipedia.org/wiki/Representational_state_transfer) will provide a good inspiration when
dealing with user-facing applications. According to this we'll use the
the following URL structure that matches our data structure:

`/lists/<list_id>/`

Uses a GET request to give us the to-do items fromt the specified list id.

`/lists/new`

Uses a POST request to create a new list.

`/lists/<list_id>/add_item`

Uses a POST request to add a new item to the list.

#### Chapter scratch-pad
- ~~Get FTs to cleanup after themselves~~
- ~~Adjust model to accommodate different kinds of lists~~
- ~~Add unique url for each list~~
- ~~Add a url for creating a new list via POST~~
- ~~Add URLs for adding new items to an existing list via POST~~

#### Implementing the new design via TDD
Basically, we'll be using a combo of adding new functionality (via extending 
FTs and writing new code) and refactoring our application using aspects of the
new design. The UTs will test that the new functionalities we added doesn't
break new, modified, and untouched tests.

#### Test isolation
Each test must not affect other tests. Every test must run in a self-contained
manner and any permanent state must be reset.

#### The Testing Goat vs. The Refactoring Cat
We must avoid the tendency to fix everything at once and end up with a
non-working application. Take one step a time. Get your app from working state
to working state.

## chapter 7

This chapter introduces using static files and Twitter Bootstrap for styling the
application.

Testing layout and styling is a lot like testing a constant and any test written
will most likely be brittle.

However, a minimal _Smoke Test_ is in order just to ensure that your CSS and JS
files are working. Also, if a certain process requires JS processing you'll want
to test that too.

This is a dangerous area but simply remember to leave yourself with room to
change your design without having to change your test all the time.

## chapter 8

In this chapter we'll tackle deployment. As in putting our app in a live web
server for the world to see.

The author urges the readers to send him a note if they are able to put it out
there: obeythetestinggoat@gmail.com

Configuring DNS records so that my app subdomains point to my Digital Ocean
server without switching over to DO's nameservers was quite tricky.

On my DNS manager in my domain registrar, I created _A records_ for the app
subdomains with the IP pointing to my DO server's IP. Then on DO's DNS manager,
I added my domain with A records for the app subdomain exactly the same as how
I configured it on my domain registrar's.

I'm not sure if it's just me, but it seems that the author made switcheroo on us
from using a root account _(p128)_ to a non-root account in the staging server _(p134)_.
If you're just happily following along the book and don't have much experience
in this configuration, you'll get lost.



#### TDD and Danger Areas of Deployment

**Isses**

- Static files: Production servers need special permissions/configurations to
                have access to these.
- The database: Path/configuration issues and making sure data is preserved.
- Dependencies: Other applications we need to run our app.

**Solutions**

- Use a _staging site_ that is as close to the production environment as possible.
- Run the _FTs against the staging._ This will also cover the smoke tests we
  created for our CSS to ensure that it has loaded properly.
- _Virtualenvs_ to manage packages and dependencies on a machine that may be used
  to serve several applications.
- _Automation_. Using a script to deploy our application on both staging and
  production to ensure that both environments are much alike.

#### Chapter overview / To-Do's

This chapter is going to be another marathon so here's a list of the items we'll
be covering:

- [x] Adapt our FTs so it runs against a staging server.
- [x] Spin up a server, install all required software and point our staging and
      live urls to it.
- [x] Upload our code to the server using git.
- [x] Get a quick & dirty version of our site running on the staging domain
      using the Django dev server.
- [x] Learn to use virtualenv to manage dependencies.
- [x] As we go, we keep on running our FT to tell us what's running and what's
      not.
- [ ] Move from our quick & dirty setup to a production-ready setup using
      gunicorn, upstart, and domain sockets.
- [ ] Once we have a working config, we write a script to automate the manual
      process we were doing previously.
- [ ] Run the script to automate the deployment to the production server.


