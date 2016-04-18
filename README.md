# SnipLime
A collection of design patterns/idioms/snippets for Sublime Text.
<img src="http://blogs.theage.com.au/schembri/rat1.jpg" alt="Python Patterns logo" align="right" />
# Python Patterns snippets 

Design patterns/idioms/snippets for [Sublime Text](http://www.sublimetext.com/) with **good solutions
for regular problems.

> Debugging is twice as hard as writing the code in the first place.
> Therefore, if you write the code as cleverly as possible, you are, by definition,
> not smart enough to debug it.
>
> – Brian W. Kernighan, co-author of The C Programming Language and the "K" in "AWK"

## Installation

**Using Package Control**:

> Bring up the Command Palette(Command+Shift+P on OS X, Control+Shift+P on Linux/Windows)

> Select Package Control: Install Package.

> Select Sublime Text 3 Snippets to install.

**Not Using Package Control**:
Save files to the Packages/Sublime Text 3 Snippets directory, then relaunch Sublime:

> git clone https://github.com/yasintoy/SnipLime.git

> cd SnipLime

> Linux: sudo cp Python/ ~/.config/sublime-text-2/Packages/

> Mac: sudo cp Python/ ~/Library/Application Support/Sublime-Text-3/Packages/

> Windows: cp %APPDATA%/Sublime-Text-2/Packages/

(if you want to install it for sublime-text-3, change sublime-text-2 with sublime-text-3 )

## Snippets 
> (You don't have to memorise triggers because it's really close to english sentences.)
Some Python Patterns snippets in the wild.

<img src="http://cdn1.caiogondim.com/js-patterns-sublime-snippets-preview.gif" alt="Preview" />


## Factorial function

**trigger**: factorial⇥

It works also for bigger numbers, when the result becomes **long**
Reference:
- [StackOverflow: Function for Factorial in Python](http://stackoverflow.com/questions/5136447/function-for-factorial-in-python)

```python
from itertools import imap
def factorial(x):
    return reduce(long.__mul__, imap(long, xrange(1, x + 1))
    
print factorial(1000)
```


## Get Twitter User Timeline

**trigger**: twittergetusertimeline⇥

Just change the api parameters and username that whatever you want!

```python
import twitter

api = twitter.Api(consumer_key='your-consumer-key',
                  consumer_secret='your-consumer-secret',
                  access_token_key='your-access-token-key',
                  access_token_secret='your-access-token-secret')

# set a handle
username = 'anthonydb'

# get a user timeline
statuses = api.GetUserTimeline(screen_name=username, count=1)
print [s.text for s in statuses]
```

## Any Generate

**trigger**: anygenerate⇥

I like using **any** and a generator:

```python
if any(pred(x.item) for x in sequence):
    ...
});
```
instead of code written like this:

```python 
found = False
for x in sequence:
    if pred(x.n):
        found = True
if found:
    ...
```
Reference:
- [this technique from a Peter Norvig](http://norvig.com/sudoku.html)

## Dictionary Comprehension

**trigger:** dictcomp⇥

A faster and pythonic way to create a `dict`.

```python
d = {key: value for (key, value) in iterable}
```

## Open File

An easy and security way to open a file.
**trigger**: readfile⇥

```python
with open('file.txt',"r") as f:
    for line in f:
        ...
```


## Email Verification Pattern

**trigger**: emailverify⇥

```python
import re

addressToVerify ='info@emailhippo.com' # change it
match = re.match('^[_a-z0-9-]+(\.[_a-z0-9-]+)*@[a-z0-9-]+(\.[a-z0-9-]+)*(\.[a-z]{2,4})$', addressToVerify)

if match == None:
	print('Bad Syntax')
```

## Download Stock From Yahoo

Iteratively create a pandas dataframe by column and download stock data from Yahoo.com

**trigger**: downloadstock⇥

```python
import pandas.io.data as web
import pandas as p
from datetime import datetime as dt

start = dt(2011,1,3)
# end = dt.today()
end = dt(2011,6,5)

stocks = [        'XLY',  # XLY Consumer Discrectionary SPDR Fund   
                       'XLF',  # XLF Financial SPDR Fund  
                       'XLK',  # XLK Technology SPDR Fund  
                       'XLE',  # XLE Energy SPDR Fund  
                       'XLV',  # XLV Health Care SPRD Fund  
                       'XLI',  # XLI Industrial SPDR Fund  
                       'XLP',  # XLP Consumer Staples SPDR Fund   
                       'XLB',  # XLB Materials SPDR Fund  
                       'XLU' ]

prices = p.DataFrame()

for stock in stocks:
    prices[stock] = web.DataReader(stock, 'yahoo', start, end)['Adj Close']
```
Reference:
- [What are some nifty python snippets that you have found very useful and would like to share?](https://www.reddit.com/r/Python/comments/2ysd91/what_are_some_nifty_python_snippets_that_you_have/)

## Get Links

**trigger**: getlinks⇥

Get all links from given website and print them.

```python
import urllib
import BeautifulSoup

htmlSource = urllib.urlopen("${1:http://sebsauvage.net/index.html}").read(200000)
soup = BeautifulSoup.BeautifulSoup(htmlSource)
for item in soup.fetch('a'):
    print item['href'] 
```

## Graph Search

**trigger**: graphsearch⇥

Implementation of Graph search with Python.

```python
class GraphSearch:

    """comments"""

    def __init__(self, graph):
        self.graph = graph

    def find_path(self, start, end, path=None):
        self.start = start
        self.end = end
        self.path = path if path else []

        self.path += [self.start]
        if self.start == self.end:
            return self.path
        if self.start not in self.graph:
            return None
        for node in self.graph[self.start]:
            if node not in self.path:
                newpath = self.find_path(node, self.end, self.path)
                if newpath:
                    return newpath
        return None

    def find_all_path(self, start, end, path=None):
        self.start = start
        self.end = end
        _path = path if path else []
        _path += [self.start]
        if self.start == self.end:
            return [_path]
        if self.start not in self.graph:
            return []
        paths = []
        for node in self.graph[self.start]:
            if node not in _path:
                newpaths = self.find_all_path(node, self.end, _path[:])
                for newpath in newpaths:
                    paths.append(newpath)
        return paths

    def find_shortest_path(self, start, end, path=None):
        self.start = start
        self.end = end
        _path = path if path else []

        _path += [self.start]
        if self.start == self.end:
            return _path
        if self.start not in self.graph:
            return None
        shortest = None
        for node in self.graph[self.start]:
            if node not in _path:
                newpath = self.find_shortest_path(node, self.end, _path[:])
                if newpath:
                    if not shortest or len(newpath) < len(shortest):
                        shortest = newpath
        return shortest

"""

# example of graph usage
graph = {'A': ['B', 'C'],
         'B': ['C', 'D'],
         'C': ['D'],
         'D': ['C'],
         'E': ['F'],
         'F': ['C']
         }

# initialization of new graph search object
graph1 = GraphSearch(graph)


print(graph1.find_path('A', 'D'))
print(graph1.find_all_path('A', 'D'))
print(graph1.find_shortest_path('A', 'D'))

### OUTPUT ###
# ['A', 'B', 'C', 'D']
# [['A', 'B', 'C', 'D'], ['A', 'B', 'D'], ['A', 'C', 'D']]
# ['A', 'B', 'D'] 

"""

```


## FTP Send File

**trigger**: ftpsendfile⇥

Send a file by using the FTP.

```python
import ftplib                                          # We import the FTP module

session = ftplib.FTP('adress like blabla.com','login','passord') # Connect to the FTP server
myfile = open('toto.txt','rb')                    # Open the file to send
session.storbinary('STOR toto.txt', myfile)       # Send the file

myfile.close()                                         # Close the file
session.quit()                                         # Close FTP session
```

## Timestamped Filename

**trigger**: timestampedfilename⇥

```python
import datetime, os

def timestamped_filename(file_name, date_time=None):
    date_time = date_time or datetime.datetime.now()
    root, ext = os.path.splitext(file_name)
    return '{}{:_%Y_%m_%d_%H_%M_%S}{}'.format(root, date_time, ext)
```


## Merge Sort

**trigger**: mergesort⇥

```python
def merge(left, right, lt):
    """Assumes left and right are sorted lists.
     lt defines an ordering on the elements of the lists.
     Returns a new sorted(by lt) list containing the same elements
     as (left + right) would contain."""
    result = []
    i,j = 0, 0
    while i < len(left) and j < len(right):
        if lt(left[i], right[j]):
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    while (i < len(left)):
        result.append(left[i])
        i += 1
    while (j < len(right)):
        result.append(right[j])
        j += 1
    return result
            
def sort(L, lt = lambda x,y: x < y):
    """Returns a new sorted list containing the same elements as L"""
    if len(L) < 2:
        return L[:]
    else:
        middle = int(len(L)/2)
        left = sort(L[:middle], lt)
        right = sort(L[middle:], lt)
        #print 'About to merge', left, 'and', right
        return merge(left, right, lt)
}());
```
## Tree Data Structure

**trigger**: tree⇥

```python
class Node:
    """
    Class Node
    """
    def __init__(self, value):
        self.left = None
        self.data = value
        self.right = None

class Tree:
    """
    Class tree will provide a tree as well as utility functions.
    """

    def createNode(self, data):
        """
        Utility function to create a node.
        """
        return Node(data)

    def insert(self, node , data):
        """
        Insert function will insert a node into tree.
        Duplicate keys are not allowed.
        """
        #if tree is empty , return a root node
        if node is None:
            return self.createNode(data)
        # if data is smaller than parent , insert it into left side
        if data < node.data:
            node.left = self.insert(node.left, data)
        elif data > node.data:
            node.right = self.insert(node.right, data)

        return node


    def search(self, node, data):
        """
        Search function will search a node into tree.
        """
        # if root is None or root is the search data.
        if node is None or node.data == data:
            return node

        if node.data < data:
            return self.search(node.right, data)
        else:
            return self.search(node.left, data)



    def deleteNode(self,node,data):
        """
        Delete function will delete a node into tree.
        Not complete , may need some more scenarion that we can handle
        Now it is handling only leaf.
        """

        # Check if tree is empty.
        if node is None:
            return None

        # searching key into BST.
        if data < node.data:
            node.left = self.deleteNode(node.left, data)
        elif data > node.data:
            node.right = self.deleteNode(node.right, data)
        else: # reach to the node that need to delete from BST.
            if node.left is None and node.right is None:
                del node
            if node.left == None:
                temp = node.right
                del node
                return  temp
            elif node.right == None:
                temp = node.left
                del node
                return temp

        return node

    def traverseInorder(self, root):
        """
        traverse function will print all the node in the tree.
        """
        if root is not None:
            self.traverseInorder(root.left)
            print root.data
            self.traverseInorder(root.right)

    def traversePreorder(self, root):
        """
        traverse function will print all the node in the tree.
        """
        if root is not None:
            print root.data
            self.traversePreorder(root.left)
            self.traversePreorder(root.right)

    def traversePostorder(self, root):
        """
        traverse function will print all the node in the tree.
        """
        if root is not None:
            self.traversePreorder(root.left)
            self.traversePreorder(root.right)
            print root.data
```

## And we've much more samples but I think that's enough for now.

The MIT License (MIT)

Copyright (c) 2016 Yasin Toy

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
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
