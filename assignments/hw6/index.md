---
layout: assignment 
title: Stimulus Data
due: Tuesday Aug 21th, 8pm
---

Introduction 
------------

This assignment is to help you familiarize yourself with

* Structuring real world data
* Building and manipulating lists with small functions
* Higher order functions (map, filter, list comprehensions)

###Data###

We will use data from the [American Recovery and Reinvestment Act (ARRA)](http://en.wikipedia.org/wiki/American_Recovery_and_Reinvestment_Act_of_2009), part of the 2009 US Stimulus Package to motivate this project. Your repository holds a file, `stimulus.txt` which contains thousands of lines of text, each of which describe a single government funded public project. 

Each line looks like the following:

    North Chicago;IL;Bike Path Extension (Commonwealth to Sheridan);20;100000;Streets/Roads;[page]

The different columns correspond to 
    
    City; State; Description of Project; Jobs created; Cost in Dollars; Project Category

This describes a project that took place in the city North Chicago, IL to extend a bike path from Commonwealth to Sheridan Rd. It employed 20 people and cost $100 000. It is part of the Streets and Roads project category. 
As you can see the different sections in each line are separated by a semi-colon. 

###Useful functions###

Given a file `f` you can use `f.readlines()` to get a list of strings. Each
element of this list is one line of the file.

Given a string `s` you can use `s.split(c)` to split s on the delimiter c. For
example

    "Hello World".split(" ") == ["Hello", "World"]

Tasks
-----

Using this data you will provide answers to the following questions

* How much money total was spent in these projects?
* How many jobs were created in the state of Illinois?
* How much money was spent in a given state?
* How much money was distributed to each state?
* What were the names of all the projects that put computers in schools?

Structuring Data
----------------

*The goal of this section is to build a function to convert a list of strings into a list of dictionaries or objects.*

You could do this entire assignment dealing with strings in all of your functions. This would require you to split your strings and call float many times. This is error prone because you're almost certain to get a few of those times wrong. It also requires you to understand what your data looks like in all parts of the code. 

Instead, lets convert the input list of strings into a more structured format. This will allow us to manipulate it more easily. You should do one of the following

1. *Note - we haven't yet covered this in lecture. You're welcome to try this option but dictionaries might be easier*. Convert your list of strings into a list of Project objects. This will require you to make a very simple class with fields `city`, `state`, `description`, `jobs`, `cost`, `category`. Ensure that the fields jobs and cost hold integers and not strings.

2. Convert your list of strings into a list of dictionaries. Each of your dictionaries should have the keys, `"city"`, `"state"`, `"description"`, `"jobs"`, `"cost"`, `"category"`. Ensure that the values associated to `"jobs"` and `"cost"` hold integers and not strings. 

You should do this conversion by making a function that takes in a single line of text like 

    North Chicago;IL;Bike Path Extension (Commonwealth to Sheridan);20;100000;Streets/Roads;[page]

And produces a single `Project` or `dict` as described above. 

You should then create a function which takes in the filename of the stimulus textfile and returns a list of Project objects/dicts which you will use for the rest of the assignment. 

If you choose to create a Project class then this function might look like the following in use

    In [2]: data = loadFromFile('stimulus.txt')
    In [3]: a_project = data[1000]
    In [3]: a_project.cost
    Out[3]: 1229780

    In [4]: a_project.description
    Out[4]: '67 Avenue and Happy Valley Road Intersection Improvement'

If you choose to create a dictionaries then this function might look like the following in use

    In [2]: data = loadFromFile('stimulus.txt')
    In [3]: a_project = data[1000]
    In [3]: a_project["cost"]
    Out[3]: 1229780

    In [4]: a_project["description"]
    Out[4]: '67 Avenue and Happy Valley Road Intersection Improvement'

It will be much easier to perform the requested tasks with this list of
objects/dicts rather than with a list of strings. All of your other functions
should use this list of objects/dicts as an input. 

The Different Tasks
-------------------

* How much money total was spent in these projects? 

This question is self explanatory. You may find the `sum` function helpful.

* How many jobs were created in the state of Illinois?

This question is self explanatory.

* How much money was distributed to a given state?

This function should take both your list of projects and a state, specified by the two letter abbreviation (like "IL" for Illinois).
    
    In [10]: moneyOfState(data, "CA")
    Out[10]: 23194447835

* How much money was distributed to each state?

In the previous question you computed how much money was given to a particular state. Now we want to know the answer for each of the states. You should use your previous function to accomplish this task (even though this is slightly inefficient).

For this function you should return your answer as a dicitonary so that  
    
    In [10]: result = moneyOfStates(data)
    In [11]: result['CA'] # result is a dict
    Out[11]: 23194447835

You can obtain a list of states from the data. If you get the state from every project you'll have a list of every state with a lot of repeats. If you turn this list into a set (by calling the aptly-named `set` function) it will get rid of the repeats for you. There are 51 states because we include Puerto Rico (PR).

* What were the names of all the projects that put computers in schools?

You should check both that the text "computer" is in the project description
and that the type of the project is "Schools". You should return a list of
project descriptions (a list of strings). 

A note about using strings. The "in" operator can be very helpful to test if some text appears in a string. For example

    In [1]: 'cat' in 'concatenate'
    Out[1]: True
    
This returns True because the text cat appears in the word concatenate. However, consider 

    In [2]: 'cat' in 'Category Theory'
    Out[2]: False

This is false because 'Category Theory' contains 'Cat', not 'cat'. To eliminate case issues a common tactic is to convert all strings to lower-case before comparing them. 

    In [2]: 'Category Theory'.lower()
    Out[2]: 'category theory'
    In [3]: 'cat' in 'Category Theory'.lower()
    Out[3]: True

Challenge Problem 5%
--------------------

We've discussed three different methods of manipulating lists. 

1. One was using for loops to iterate over old lists and build up new lists using append
2. The second was using `map`, `filter` and small functions like 

    def stateIsIllinois(project):
        return project.state=="IL"

3. The third way used [list comprehensions](http://docs.python.org/tutorial/datastructures.html#list-comprehensions)

Complete the last task using all three ways. 


Challenge - 0%
--------------

We won't give you any credit, but if this sort of project interests you you
might want to look into the [pandas library](http://pandas.pydata.org/).

Submission
----------
Submit a three .py files. `stimulus.py`, `test_stimulus.py`, and `print_stimulus.py`

`stimulus.py` should contain 

* A function that takes in a line of the file and returns a Project object or dict
* A function that takes in a filename and returns a list of Project objects or a list of dicts
* Five additional functions - one for each task
* Two additional functions if you do the challenge

`test_stimulus.py` should testing code for the functions in `stimulus.py`. 

`print_stimulus.py` should print out the answers to the above questions. It
should use functions from stimulus.py. `print_stimulus.py` should be a very
small file. As much of the logic as possible should be in `stimulus.py`.

    # print_stimulus.py
    # Author: 

    from stimulus import *

    data = loadFromFile("stimulus.txt")

    print("Question 1")
    print("Total money spent on all projects was: " + str(totalMoney(data)))

    print("Question 2")
    ...

You should not do the following - 

* print anywhere outside of print_stimulus.py - all functions should return lists, not print them
* load the data from the file in every function. Data should only be loaded in in a single `loadFromFile` funciton. Every other function should take the data that is returned by this function as input.
* Have code in your .py file other than function and class definitions. If we run 

    python stimulus.py 
    
then nothing should actually be computed/loaded/printed. Your `stimulus.py` file should only define some functions and possibly a Class.
