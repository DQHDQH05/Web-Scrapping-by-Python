<center>
    <img src="https://gitlab.com/ibm/skills-network/courses/placeholder101/-/raw/master/labs/module%201/images/IDSNlogo.png" width="300" alt="cognitiveclass.ai logo"  />
</center>


# **Web Scraping Lab**


Estimated time needed: **30** minutes


## Objectives


After completing this lab you will be able to:


<h2>Table of Contents</h2>
<div class="alert alert-block alert-info" style="margin-top: 20px">
    <ul>
        <li>
            <a href="https://bso/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkPY0220ENSkillsNetwork23455606-2021-01-01">Beautiful Soup Object</a>
            <ul>
                <li>Tag</li>
                <li>Children, Parents, and Siblings</li>
                <li>HTML Attributes</li>
                <li>Navigable String</li>
            </ul>
        </li>
     </ul>
    <ul>
        <li>
            <a href="https://filter/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkPY0220ENSkillsNetwork23455606-2021-01-01">Filter</a>
            <ul>
                <li>find All</li>
                <li>find </li>
                <li>HTML Attributes</li>
                <li>Navigable String</li>
            </ul>
        </li>
     </ul>
     <ul>
        <li>
            <a href="https://dscw/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkPY0220ENSkillsNetwork23455606-2021-01-01">Downloading And Scraping The Contents Of A Web</a>
    </li>
         </ul>
    <p>
        Estimated time needed: <strong>25 min</strong>
    </p>

</div>

<hr>


For this lab, we are going to be using Python and several Python libraries. Some of these libraries might be installed in your lab environment or in SN Labs. Others may need to be installed by you. The cells below will install these libraries when executed.



```python
!mamba install bs4==4.10.0 -y
!pip install lxml==4.6.4
!mamba install html5lib==1.1 -y
# !pip install requests==2.26.0
```

Import the required modules and functions



```python
from bs4 import BeautifulSoup # this module helps in web scrapping.
import requests  # this module helps us to download a web page
```

<h2 id="BSO">Beautiful Soup Objects</h2>


Beautiful Soup is a Python library for pulling data out of HTML and XML files, we will focus on HTML files. This is accomplished by representing the HTML as a set of objects with methods used to parse the HTML.  We can navigate the HTML as a tree and/or filter out what we are looking for.

Consider the following HTML:



```python
%%html
<!DOCTYPE html>
<html>
<head>
<title>Page Title</title>
</head>
<body>
<h3><b id='boldest'>Lebron James</b></h3>
<p> Salary: $ 92,000,000 </p>
<h3> Stephen Curry</h3>
<p> Salary: $85,000, 000 </p>
<h3> Kevin Durant </h3>
<p> Salary: $73,200, 000</p>
</body>
</html>
```


<!DOCTYPE html>
<html>
<head>
<title>Page Title</title>
</head>
<body>
<h3><b id='boldest'>Lebron James</b></h3>
<p> Salary: $ 92,000,000 </p>
<h3> Stephen Curry</h3>
<p> Salary: $85,000, 000 </p>
<h3> Kevin Durant </h3>
<p> Salary: $73,200, 000</p>
</body>
</html>



We can store it as a string in the variable HTML:



```python
html="<!DOCTYPE html><html><head><title>Page Title</title></head><body><h3><b id='boldest'>Lebron James</b></h3><p> Salary: $ 92,000,000 </p><h3> Stephen Curry</h3><p> Salary: $85,000, 000 </p><h3> Kevin Durant </h3><p> Salary: $73,200, 000</p></body></html>"
```

To parse a document, pass it into the <code>BeautifulSoup</code> constructor, the <code>BeautifulSoup</code> object, which represents the document as a nested data structure:



```python
soup = BeautifulSoup(html, "html.parser")
```

First, the document is converted to Unicode, (similar to ASCII),  and HTML entities are converted to Unicode characters. Beautiful Soup transforms a complex HTML document into a complex tree of Python objects. The <code>BeautifulSoup</code> object can create other types of objects. In this lab, we will cover <code>BeautifulSoup</code> and <code>Tag</code> objects that for the purposes of this lab are identical, and <code>NavigableString</code> objects.


We can use the method <code>prettify()</code> to display the HTML in the nested structure:



```python
print(soup.prettify())
```

    <!DOCTYPE html>
    <html>
     <head>
      <title>
       Page Title
      </title>
     </head>
     <body>
      <h3>
       <b id="boldest">
        Lebron James
       </b>
      </h3>
      <p>
       Salary: $ 92,000,000
      </p>
      <h3>
       Stephen Curry
      </h3>
      <p>
       Salary: $85,000, 000
      </p>
      <h3>
       Kevin Durant
      </h3>
      <p>
       Salary: $73,200, 000
      </p>
     </body>
    </html>


## Tags


Let's say we want the  title of the page and the name of the top paid player we can use the <code>Tag</code>. The <code>Tag</code> object corresponds to an HTML tag in the original document, for example, the tag title.



```python
tag_object=soup.title
print("tag object:",tag_object)
```

    tag object: <title>Page Title</title>


we can see the tag type <code>bs4.element.Tag</code>



```python
print("tag object type:",type(tag_object))
```

    tag object type: <class 'bs4.element.Tag'>


If there is more than one <code>Tag</code>  with the same name, the first element with that <code>Tag</code> name is called, this corresponds to the most paid player:



```python
tag_object=soup.h3
tag_object
```




    <h3><b id="boldest">Lebron James</b></h3>



Enclosed in the bold attribute <code>b</code>, it helps to use the tree representation. We can navigate down the tree using the child attribute to get the name.


### Children, Parents, and Siblings


As stated above the <code>Tag</code> object is a tree of objects we can access the child of the tag or navigate down the branch as follows:



```python
tag_child =tag_object.b
tag_child
```




    <b id="boldest">Lebron James</b>



You can access the parent with the <code> parent</code>



```python
parent_tag=tag_child.parent
parent_tag
```




    <h3><b id="boldest">Lebron James</b></h3>



this is identical to



```python
tag_object
```




    <h3><b id="boldest">Lebron James</b></h3>



<code>tag_object</code> parent is the <code>body</code> element.



```python
tag_object.parent
```




    <body><h3><b id="boldest">Lebron James</b></h3><p> Salary: $ 92,000,000 </p><h3> Stephen Curry</h3><p> Salary: $85,000, 000 </p><h3> Kevin Durant </h3><p> Salary: $73,200, 000</p></body>



<code>tag_object</code> sibling is the <code>paragraph</code> element



```python
sibling_1=tag_object.next_sibling
sibling_1
```




    <p> Salary: $ 92,000,000 </p>



`sibling_2` is the `header` element which is also a sibling of both `sibling_1` and `tag_object`



```python
sibling_2=sibling_1.next_sibling
sibling_2
```




    <h3> Stephen Curry</h3>



<h3 id="first_question">Exercise: <code>next_sibling</code></h3>


Using the object <code>sibling\_2</code> and the property <code>next_sibling</code> to find the salary of Stephen Curry:



```python

```
<details><summary>Click here for the solution</summary>

```
sibling_2.next_sibling

```

</details>

### HTML Attributes


If the tag has attributes, the tag <code>id="boldest"</code> has an attribute <code>id</code> whose value is <code>boldest</code>. You can access a tag’s attributes by treating the tag like a dictionary:



```python
tag_child['id']
```




    'boldest'



You can access that dictionary directly as <code>attrs</code>:



```python
tag_child.attrs
```




    {'id': 'boldest'}



You can also work with Multi-valued attribute check out <a href="https://www.crummy.com/software/BeautifulSoup/bs4/doc/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkPY0220ENSkillsNetwork23455606-2021-01-01">\[1]</a> for more.


We can also obtain the content if the attribute of the <code>tag</code> using the Python <code>get()</code> method.



```python
tag_child.get('id')
```




    'boldest'



### Navigable String


A string corresponds to a bit of text or content within a tag. Beautiful Soup uses the <code>NavigableString</code> class to contain this text. In our HTML we can obtain the name of the first player by extracting the sting of the <code>Tag</code> object <code>tag_child</code> as follows:



```python
tag_string=tag_child.string
tag_string
```




    'Lebron James'



we can verify the type is Navigable String



```python
type(tag_string)
```




    bs4.element.NavigableString



A NavigableString is just like a Python string or Unicode string, to be more precise. The main difference is that it also supports some  <code>BeautifulSoup</code> features. We can covert it to sting object in Python:



```python
unicode_string = str(tag_string)
unicode_string
```




    'Lebron James'



<h2 id="filter">Filter</h2>


Filters allow you to find complex patterns, the simplest filter is a string. In this section we will pass a string to a different filter method and Beautiful Soup will perform a match against that exact string.  Consider the following HTML of rocket launchs:



```python
%%html
<table>
  <tr>
    <td id='flight' >Flight No</td>
    <td>Launch site</td> 
    <td>Payload mass</td>
   </tr>
  <tr> 
    <td>1</td>
    <td><a href='https://en.wikipedia.org/wiki/Florida'>Florida</a></td>
    <td>300 kg</td>
  </tr>
  <tr>
    <td>2</td>
    <td><a href='https://en.wikipedia.org/wiki/Texas'>Texas</a></td>
    <td>94 kg</td>
  </tr>
  <tr>
    <td>3</td>
    <td><a href='https://en.wikipedia.org/wiki/Florida'>Florida<a> </td>
    <td>80 kg</td>
  </tr>
</table>
```


<table>
  <tr>
    <td id='flight' >Flight No</td>
    <td>Launch site</td> 
    <td>Payload mass</td>
   </tr>
  <tr> 
    <td>1</td>
    <td><a href='https://en.wikipedia.org/wiki/Florida'>Florida</a></td>
    <td>300 kg</td>
  </tr>
  <tr>
    <td>2</td>
    <td><a href='https://en.wikipedia.org/wiki/Texas'>Texas</a></td>
    <td>94 kg</td>
  </tr>
  <tr>
    <td>3</td>
    <td><a href='https://en.wikipedia.org/wiki/Florida'>Florida<a> </td>
    <td>80 kg</td>
  </tr>
</table>



We can store it as a string in the variable <code>table</code>:



```python
table="<table><tr><td id='flight'>Flight No</td><td>Launch site</td> <td>Payload mass</td></tr><tr> <td>1</td><td><a href='https://en.wikipedia.org/wiki/Florida'>Florida<a></td><td>300 kg</td></tr><tr><td>2</td><td><a href='https://en.wikipedia.org/wiki/Texas'>Texas</a></td><td>94 kg</td></tr><tr><td>3</td><td><a href='https://en.wikipedia.org/wiki/Florida'>Florida<a> </td><td>80 kg</td></tr></table>"
```


```python
table_bs = BeautifulSoup(table, "html.parser")
```

## find All


The <code>find_all()</code> method looks through a tag’s descendants and retrieves all descendants that match your filters.

<p>
The Method signature for <code>find_all(name, attrs, recursive, string, limit, **kwargs)<c/ode>
</p>


### Name


When we set the <code>name</code> parameter to a tag name, the method will extract all the tags with that name and its children.



```python
table_rows=table_bs.find_all('tr')
table_rows
```




    [<tr><td id="flight">Flight No</td><td>Launch site</td> <td>Payload mass</td></tr>,
     <tr> <td>1</td><td><a href="https://en.wikipedia.org/wiki/Florida">Florida<a></a></a></td><td>300 kg</td></tr>,
     <tr><td>2</td><td><a href="https://en.wikipedia.org/wiki/Texas">Texas</a></td><td>94 kg</td></tr>,
     <tr><td>3</td><td><a href="https://en.wikipedia.org/wiki/Florida">Florida<a> </a></a></td><td>80 kg</td></tr>]



The result is a Python Iterable just like a list, each element is a <code>tag</code> object:



```python
first_row =table_rows[0]
first_row
```




    <tr><td id="flight">Flight No</td><td>Launch site</td> <td>Payload mass</td></tr>



The type is <code>tag</code>



```python
print(type(first_row))
```

    <class 'bs4.element.Tag'>


we can obtain the child



```python
first_row.td
```




    <td id="flight">Flight No</td>



If we iterate through the list, each element corresponds to a row in the table:



```python
for i,row in enumerate(table_rows):
    print("row",i,"is",row)
    
```

    row 0 is <tr><td id="flight">Flight No</td><td>Launch site</td> <td>Payload mass</td></tr>
    row 1 is <tr> <td>1</td><td><a href="https://en.wikipedia.org/wiki/Florida">Florida<a></a></a></td><td>300 kg</td></tr>
    row 2 is <tr><td>2</td><td><a href="https://en.wikipedia.org/wiki/Texas">Texas</a></td><td>94 kg</td></tr>
    row 3 is <tr><td>3</td><td><a href="https://en.wikipedia.org/wiki/Florida">Florida<a> </a></a></td><td>80 kg</td></tr>


As <code>row</code> is a <code>cell</code> object, we can apply the method <code>find_all</code> to it and extract table cells in the object <code>cells</code> using the tag <code>td</code>, this is all the children with the name <code>td</code>. The result is a list, each element corresponds to a cell and is a <code>Tag</code> object, we can iterate through this list as well. We can extract the content using the <code>string</code>  attribute.



```python
for i,row in enumerate(table_rows):
    print("row",i)
    cells=row.find_all('td')
    for j,cell in enumerate(cells):
        print('colunm',j,"cell",cell)
```

    row 0
    colunm 0 cell <td id="flight">Flight No</td>
    colunm 1 cell <td>Launch site</td>
    colunm 2 cell <td>Payload mass</td>
    row 1
    colunm 0 cell <td>1</td>
    colunm 1 cell <td><a href="https://en.wikipedia.org/wiki/Florida">Florida<a></a></a></td>
    colunm 2 cell <td>300 kg</td>
    row 2
    colunm 0 cell <td>2</td>
    colunm 1 cell <td><a href="https://en.wikipedia.org/wiki/Texas">Texas</a></td>
    colunm 2 cell <td>94 kg</td>
    row 3
    colunm 0 cell <td>3</td>
    colunm 1 cell <td><a href="https://en.wikipedia.org/wiki/Florida">Florida<a> </a></a></td>
    colunm 2 cell <td>80 kg</td>


If we use a list we can match against any item in that list.



```python
list_input=table_bs .find_all(name=["tr", "td"])
list_input
```




    [<tr><td id="flight">Flight No</td><td>Launch site</td> <td>Payload mass</td></tr>,
     <td id="flight">Flight No</td>,
     <td>Launch site</td>,
     <td>Payload mass</td>,
     <tr> <td>1</td><td><a href="https://en.wikipedia.org/wiki/Florida">Florida<a></a></a></td><td>300 kg</td></tr>,
     <td>1</td>,
     <td><a href="https://en.wikipedia.org/wiki/Florida">Florida<a></a></a></td>,
     <td>300 kg</td>,
     <tr><td>2</td><td><a href="https://en.wikipedia.org/wiki/Texas">Texas</a></td><td>94 kg</td></tr>,
     <td>2</td>,
     <td><a href="https://en.wikipedia.org/wiki/Texas">Texas</a></td>,
     <td>94 kg</td>,
     <tr><td>3</td><td><a href="https://en.wikipedia.org/wiki/Florida">Florida<a> </a></a></td><td>80 kg</td></tr>,
     <td>3</td>,
     <td><a href="https://en.wikipedia.org/wiki/Florida">Florida<a> </a></a></td>,
     <td>80 kg</td>]



## Attributes


If the argument is not recognized it will be turned into a filter on the tag’s attributes. For example the <code>id</code>  argument, Beautiful Soup will filter against each tag’s <code>id</code> attribute. For example, the first <code>td</code> elements have a value of <code>id</code> of <code>flight</code>, therefore we can filter based on that <code>id</code> value.



```python
table_bs.find_all(id="flight")
```




    [<td id="flight">Flight No</td>]



We can find all the elements that have links to the Florida Wikipedia page:



```python
list_input=table_bs.find_all(href="https://en.wikipedia.org/wiki/Florida")
list_input
```




    [<a href="https://en.wikipedia.org/wiki/Florida">Florida<a></a></a>,
     <a href="https://en.wikipedia.org/wiki/Florida">Florida<a> </a></a>]



If we set the  <code>href</code> attribute to True, regardless of what the value is, the code finds all tags with <code>href</code> value:



```python
table_bs.find_all(href=True)
```




    [<a href="https://en.wikipedia.org/wiki/Florida">Florida<a></a></a>,
     <a href="https://en.wikipedia.org/wiki/Texas">Texas</a>,
     <a href="https://en.wikipedia.org/wiki/Florida">Florida<a> </a></a>]



There are other methods for dealing with attributes and other related methods; Check out the following <a href='https://www.crummy.com/software/BeautifulSoup/bs4/doc/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkPY0220ENSkillsNetwork23455606-2021-01-01#css-selectors'>link</a>


<h3 id="exer_type">Exercise: <code>find_all</code></h3>


Using the logic above, find all the elements without <code>href</code> value



```python

```

<details><summary>Click here for the solution</summary>

```
table_bs.find_all(href=False)

```

</details>


Using the soup object <code>soup</code>, find the element with the <code>id</code> attribute content set to <code>"boldest"</code>.



```python

```

<details><summary>Click here for the solution</summary>

```
soup.find_all(id="boldest")

```

</details>


### string


With string you can search for strings instead of tags, where we find all the elments with Florida:



```python
table_bs.find_all(string="Florida")
```




    ['Florida', 'Florida']



## find


The <code>find_all()</code> method scans the entire document looking for results, it’s if you are looking for one element you can use the <code>find()</code> method to find the first element in the document. Consider the following two table:



```python
%%html
<h3>Rocket Launch </h3>

<p>
<table class='rocket'>
  <tr>
    <td>Flight No</td>
    <td>Launch site</td> 
    <td>Payload mass</td>
  </tr>
  <tr>
    <td>1</td>
    <td>Florida</td>
    <td>300 kg</td>
  </tr>
  <tr>
    <td>2</td>
    <td>Texas</td>
    <td>94 kg</td>
  </tr>
  <tr>
    <td>3</td>
    <td>Florida </td>
    <td>80 kg</td>
  </tr>
</table>
</p>
<p>

<h3>Pizza Party  </h3>
  
    
<table class='pizza'>
  <tr>
    <td>Pizza Place</td>
    <td>Orders</td> 
    <td>Slices </td>
   </tr>
  <tr>
    <td>Domino's Pizza</td>
    <td>10</td>
    <td>100</td>
  </tr>
  <tr>
    <td>Little Caesars</td>
    <td>12</td>
    <td >144 </td>
  </tr>
  <tr>
    <td>Papa John's </td>
    <td>15 </td>
    <td>165</td>
  </tr>

```


<h3>Rocket Launch </h3>

<p>
<table class='rocket'>
  <tr>
    <td>Flight No</td>
    <td>Launch site</td> 
    <td>Payload mass</td>
  </tr>
  <tr>
    <td>1</td>
    <td>Florida</td>
    <td>300 kg</td>
  </tr>
  <tr>
    <td>2</td>
    <td>Texas</td>
    <td>94 kg</td>
  </tr>
  <tr>
    <td>3</td>
    <td>Florida </td>
    <td>80 kg</td>
  </tr>
</table>
</p>
<p>

<h3>Pizza Party  </h3>


<table class='pizza'>
  <tr>
    <td>Pizza Place</td>
    <td>Orders</td> 
    <td>Slices </td>
   </tr>
  <tr>
    <td>Domino's Pizza</td>
    <td>10</td>
    <td>100</td>
  </tr>
  <tr>
    <td>Little Caesars</td>
    <td>12</td>
    <td >144 </td>
  </tr>
  <tr>
    <td>Papa John's </td>
    <td>15 </td>
    <td>165</td>
  </tr>



We store the HTML as a Python string and assign <code>two_tables</code>:



```python
two_tables="<h3>Rocket Launch </h3><p><table class='rocket'><tr><td>Flight No</td><td>Launch site</td> <td>Payload mass</td></tr><tr><td>1</td><td>Florida</td><td>300 kg</td></tr><tr><td>2</td><td>Texas</td><td>94 kg</td></tr><tr><td>3</td><td>Florida </td><td>80 kg</td></tr></table></p><p><h3>Pizza Party  </h3><table class='pizza'><tr><td>Pizza Place</td><td>Orders</td> <td>Slices </td></tr><tr><td>Domino's Pizza</td><td>10</td><td>100</td></tr><tr><td>Little Caesars</td><td>12</td><td >144 </td></tr><tr><td>Papa John's </td><td>15 </td><td>165</td></tr>"
```

We create a <code>BeautifulSoup</code> object  <code>two_tables_bs</code>



```python
two_tables_bs= BeautifulSoup(two_tables, 'html.parser')
```

We can find the first table using the tag name table



```python
two_tables_bs.find("table")
```




    <table class="rocket"><tr><td>Flight No</td><td>Launch site</td> <td>Payload mass</td></tr><tr><td>1</td><td>Florida</td><td>300 kg</td></tr><tr><td>2</td><td>Texas</td><td>94 kg</td></tr><tr><td>3</td><td>Florida </td><td>80 kg</td></tr></table>



We can filter on the class attribute to find the second table, but because class is a keyword in Python, we add an underscore.



```python
two_tables_bs.find("table",class_='pizza')
```




    <table class="pizza"><tr><td>Pizza Place</td><td>Orders</td> <td>Slices </td></tr><tr><td>Domino's Pizza</td><td>10</td><td>100</td></tr><tr><td>Little Caesars</td><td>12</td><td>144 </td></tr><tr><td>Papa John's </td><td>15 </td><td>165</td></tr></table>



<h2 id="DSCW">Downloading And Scraping The Contents Of A Web Page</h2> 


We Download the contents of the web page:



```python
url = "http://www.ibm.com"
```

We use <code>get</code> to download the contents of the webpage in text format and store in a variable called <code>data</code>:



```python
data  = requests.get(url).text 
```

We create a <code>BeautifulSoup</code> object using the <code>BeautifulSoup</code> constructor



```python
soup = BeautifulSoup(data,"html.parser")  # create a soup object using the variable 'data'
```

Scrape all links



```python
for link in soup.find_all('a',href=True):  # in html anchor/link is represented by the tag <a>

    print(link.get('href'))

```

    #main-content
    http://www.ibm.com
    https://www.ibm.com/cloud/paks?lnk=ushpv18l1
    https://www.ibm.com/security/executive-order-cybersecurity?lnk=ushpv18f1
    https://www.ibm.com/consulting/technology/?lnk=ushpv18f2
    https://www.ibm.com/training/credentials?lnk=ushpv18f3
    https://www.ibm.com/blogs/blockchain/2021/09/dont-let-the-shipping-container-crisis-ruin-your-holidays-this-year/?lnk=ushpv18f4
    https://www.ibm.com/products/offers-and-discounts?link=ushpv18t5&lnk2=trial_mktpl_MPDISC
    https://www.ibm.com/cloud/cloud-pak-for-automation?lnk=ushpv18t1&lnk2=trial_CloudPakAtm&psrc=none&pexp=def
    https://www.ibm.com/cloud/watson-studio?lnk=ushpv18t2&lnk2=trial_WatStudio&psrc=none&pexp=def
    https://www.ibm.com/cloud/aspera?lnk=ushpv18t3&lnk2=trial_AsperaCloud&psrc=none&pexp=def
    https://www.ibm.com/security/identity-access-management/cloud-identity?lnk=ushpv18t4&lnk2=trial_Verify&psrc=none&pexp=def
    https://www.ibm.com/search?lnk=ushpv18srch&locale=en-us&q=
    https://www.ibm.com/products?lnk=ushpv18p1&lnk2=trial_mktpl&psrc=none&pexp=def
    https://www.ibm.com/cloud/hybrid?lnk=ushpv18pt14
    https://www.ibm.com/watson?lnk=ushpv18pt17
    https://www.ibm.com/it-infrastructure?lnk=ushpv18pt19
    https://www.ibm.com/us-en/products/categories?technologyTopics%5B0%5D%5B0%5D=cat.topic:Blockchain&isIBMOffering%5B0%5D=true&lnk=ushpv18pt4
    https://www.ibm.com/us-en/products/category/technology/security?lnk=ushpv18pt9
    https://www.ibm.com/us-en/products/category/technology/analytics?lnk=ushpv18pt1
    https://www.ibm.com/cloud/automation?lnk=ushpv18ct21
    https://www.ibm.com/quantum-computing?lnk=ushpv18pt16
    https://www.ibm.com/mysupport/s/?language=en_US&lnk=ushpv18ct11
    https://www.ibm.com/training/?lnk=ushpv18ct15
    https://developer.ibm.com/?lnk=ushpv18ct9
    https://www.ibm.com/garage?lnk=ushpv18pt18
    https://www.ibm.com/docs/en?lnk=ushpv18ct14
    https://www.redbooks.ibm.com/?lnk=ushpv18ct10
    https://www-03.ibm.com/employment/technicaltalent/developer/?lnk=ushpv18ct2
    https://www.ibm.com/case-studies/verizon-business/?lnk=ushpv18vn1
    https://www.ibm.com/case-studies/verizon-business/?lnk=ushpv18vn1
    https://www.ibm.com/


## Scrape  all images  Tags



```python
for link in soup.find_all('img'):# in html image is represented by the tag <img>
    print(link)
    print(link.get('src'))
```

    <img alt="" aria-hidden="true" role="presentation" src="data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMTA1NSIgaGVpZ2h0PSI1MjcuNSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB2ZXJzaW9uPSIxLjEiLz4=" style="max-width:100%;display:block;margin:0;border:none;padding:0"/>
    data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMTA1NSIgaGVpZ2h0PSI1MjcuNSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB2ZXJzaW9uPSIxLjEiLz4=
    <img alt="leadspace mobile image" class="ibm-resize" decoding="async" src="https://1.dam.s81c.com/public/content/dam/worldwide-content/homepage/ul/g/2f/bc/20211115-ls-cloud-paks-26257-720x360.jpg" style="position:absolute;top:0;left:0;bottom:0;right:0;box-sizing:border-box;padding:0;border:none;margin:auto;display:block;width:0;height:0;min-width:100%;max-width:100%;min-height:100%;max-height:100%"/>
    https://1.dam.s81c.com/public/content/dam/worldwide-content/homepage/ul/g/2f/bc/20211115-ls-cloud-paks-26257-720x360.jpg
    <img alt="" aria-hidden="true" role="presentation" src="data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNDQwIiBoZWlnaHQ9IjMyMCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB2ZXJzaW9uPSIxLjEiLz4=" style="max-width:100%;display:block;margin:0;border:none;padding:0"/>
    data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNDQwIiBoZWlnaHQ9IjMyMCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB2ZXJzaW9uPSIxLjEiLz4=
    <img alt="U.S. Executive Order 14028" class="ibm-resize ibm-ab-image featured-image" decoding="async" src="https://1.dam.s81c.com/public/content/dam/worldwide-content/homepage/ul/g/80/38/20211107-26227%20X-Force-executive-order-444x320.jpg" style="position:absolute;top:0;left:0;bottom:0;right:0;box-sizing:border-box;padding:0;border:none;margin:auto;display:block;width:0;height:0;min-width:100%;max-width:100%;min-height:100%;max-height:100%"/>
    https://1.dam.s81c.com/public/content/dam/worldwide-content/homepage/ul/g/80/38/20211107-26227%20X-Force-executive-order-444x320.jpg
    <img alt="" aria-hidden="true" role="presentation" src="data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNDQwIiBoZWlnaHQ9IjMyMCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB2ZXJzaW9uPSIxLjEiLz4=" style="max-width:100%;display:block;margin:0;border:none;padding:0"/>
    data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNDQwIiBoZWlnaHQ9IjMyMCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB2ZXJzaW9uPSIxLjEiLz4=
    <img alt="IBM Consulting for technology" class="ibm-resize ibm-ab-image featured-image" decoding="async" src="https://1.dam.s81c.com/public/content/dam/worldwide-content/homepage/ul/g/57/06/20211101-f-consulting-technology-26225.jpg" style="position:absolute;top:0;left:0;bottom:0;right:0;box-sizing:border-box;padding:0;border:none;margin:auto;display:block;width:0;height:0;min-width:100%;max-width:100%;min-height:100%;max-height:100%"/>
    https://1.dam.s81c.com/public/content/dam/worldwide-content/homepage/ul/g/57/06/20211101-f-consulting-technology-26225.jpg
    <img alt="" aria-hidden="true" role="presentation" src="data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNDQwIiBoZWlnaHQ9IjMyMCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB2ZXJzaW9uPSIxLjEiLz4=" style="max-width:100%;display:block;margin:0;border:none;padding:0"/>
    data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNDQwIiBoZWlnaHQ9IjMyMCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB2ZXJzaW9uPSIxLjEiLz4=
    <img alt="To accelerate your career, start&amp;nbsp;here" class="ibm-resize ibm-ab-image featured-image" decoding="async" src="https://1.dam.s81c.com/public/content/dam/worldwide-content/homepage/ul/g/fd/39/20211107-26176-credential-experience-444x320.jpg" style="position:absolute;top:0;left:0;bottom:0;right:0;box-sizing:border-box;padding:0;border:none;margin:auto;display:block;width:0;height:0;min-width:100%;max-width:100%;min-height:100%;max-height:100%"/>
    https://1.dam.s81c.com/public/content/dam/worldwide-content/homepage/ul/g/fd/39/20211107-26176-credential-experience-444x320.jpg
    <img alt="" aria-hidden="true" role="presentation" src="data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNDQwIiBoZWlnaHQ9IjMyMCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB2ZXJzaW9uPSIxLjEiLz4=" style="max-width:100%;display:block;margin:0;border:none;padding:0"/>
    data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNDQwIiBoZWlnaHQ9IjMyMCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB2ZXJzaW9uPSIxLjEiLz4=
    <img alt="Demand is up but your supply isn&amp;#8217;t?" class="ibm-resize ibm-ab-image featured-image" decoding="async" src="https://1.dam.s81c.com/public/content/dam/worldwide-content/homepage/ul/g/37/67/20211107-26238-supply-chain-crisis-444x320.jpg" style="position:absolute;top:0;left:0;bottom:0;right:0;box-sizing:border-box;padding:0;border:none;margin:auto;display:block;width:0;height:0;min-width:100%;max-width:100%;min-height:100%;max-height:100%"/>
    https://1.dam.s81c.com/public/content/dam/worldwide-content/homepage/ul/g/37/67/20211107-26238-supply-chain-crisis-444x320.jpg
    <img alt="" aria-hidden="true" role="presentation" src="data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNDQwIiBoZWlnaHQ9IjI2MCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB2ZXJzaW9uPSIxLjEiLz4=" style="max-width:100%;display:block;margin:0;border:none;padding:0"/>
    data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNDQwIiBoZWlnaHQ9IjI2MCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB2ZXJzaW9uPSIxLjEiLz4=
    <img alt="IBM Cloud Pak for Automation" class="ibm-resize ibm-ab-image trials-image" decoding="async" src="https://1.dam.s81c.com/public/content/dam/worldwide-content/homepage/ul/g/ab/f7/Cloud-pak-for-automation-444x260.png" style="position:absolute;top:0;left:0;bottom:0;right:0;box-sizing:border-box;padding:0;border:none;margin:auto;display:block;width:0;height:0;min-width:100%;max-width:100%;min-height:100%;max-height:100%"/>
    https://1.dam.s81c.com/public/content/dam/worldwide-content/homepage/ul/g/ab/f7/Cloud-pak-for-automation-444x260.png
    <img alt="IBM Cloud Pak for Automation" class="ibm-resize ibm-ab-image trials-image" decoding="async" src="data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7" style="position:absolute;top:0;left:0;bottom:0;right:0;box-sizing:border-box;padding:0;border:none;margin:auto;display:block;width:0;height:0;min-width:100%;max-width:100%;min-height:100%;max-height:100%"/>
    data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7
    <img alt="" aria-hidden="true" role="presentation" src="data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNDQwIiBoZWlnaHQ9IjI2MCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB2ZXJzaW9uPSIxLjEiLz4=" style="max-width:100%;display:block;margin:0;border:none;padding:0"/>
    data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNDQwIiBoZWlnaHQ9IjI2MCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB2ZXJzaW9uPSIxLjEiLz4=
    <img alt="IBM Watson Studio" class="ibm-resize ibm-ab-image trials-image" decoding="async" src="https://1.dam.s81c.com/public/content/dam/worldwide-content/homepage/ul/g/58/44/Watson-Studio-Desktop-21039-700x420.png" style="position:absolute;top:0;left:0;bottom:0;right:0;box-sizing:border-box;padding:0;border:none;margin:auto;display:block;width:0;height:0;min-width:100%;max-width:100%;min-height:100%;max-height:100%"/>
    https://1.dam.s81c.com/public/content/dam/worldwide-content/homepage/ul/g/58/44/Watson-Studio-Desktop-21039-700x420.png
    <img alt="IBM Watson Studio" class="ibm-resize ibm-ab-image trials-image" decoding="async" src="data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7" style="position:absolute;top:0;left:0;bottom:0;right:0;box-sizing:border-box;padding:0;border:none;margin:auto;display:block;width:0;height:0;min-width:100%;max-width:100%;min-height:100%;max-height:100%"/>
    data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7
    <img alt="" aria-hidden="true" role="presentation" src="data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNDQwIiBoZWlnaHQ9IjI2MCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB2ZXJzaW9uPSIxLjEiLz4=" style="max-width:100%;display:block;margin:0;border:none;padding:0"/>
    data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNDQwIiBoZWlnaHQ9IjI2MCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB2ZXJzaW9uPSIxLjEiLz4=
    <img alt="IBM Aspera on Cloud" class="ibm-resize ibm-ab-image trials-image" decoding="async" src="https://1.dam.s81c.com/public/content/dam/worldwide-content/homepage/ul/g/e5/32/Aspera-on-Cloud-19783-700x420.png" style="position:absolute;top:0;left:0;bottom:0;right:0;box-sizing:border-box;padding:0;border:none;margin:auto;display:block;width:0;height:0;min-width:100%;max-width:100%;min-height:100%;max-height:100%"/>
    https://1.dam.s81c.com/public/content/dam/worldwide-content/homepage/ul/g/e5/32/Aspera-on-Cloud-19783-700x420.png
    <img alt="IBM Aspera on Cloud" class="ibm-resize ibm-ab-image trials-image" decoding="async" src="data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7" style="position:absolute;top:0;left:0;bottom:0;right:0;box-sizing:border-box;padding:0;border:none;margin:auto;display:block;width:0;height:0;min-width:100%;max-width:100%;min-height:100%;max-height:100%"/>
    data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7
    <img alt="" aria-hidden="true" role="presentation" src="data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNDQwIiBoZWlnaHQ9IjI2MCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB2ZXJzaW9uPSIxLjEiLz4=" style="max-width:100%;display:block;margin:0;border:none;padding:0"/>
    data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNDQwIiBoZWlnaHQ9IjI2MCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB2ZXJzaW9uPSIxLjEiLz4=
    <img alt="IBM Security Verify" class="ibm-resize ibm-ab-image trials-image" decoding="async" src="https://1.dam.s81c.com/public/content/dam/worldwide-content/homepage/ul/g/15/16/cloud-transformation-trial-700x420.png" style="position:absolute;top:0;left:0;bottom:0;right:0;box-sizing:border-box;padding:0;border:none;margin:auto;display:block;width:0;height:0;min-width:100%;max-width:100%;min-height:100%;max-height:100%"/>
    https://1.dam.s81c.com/public/content/dam/worldwide-content/homepage/ul/g/15/16/cloud-transformation-trial-700x420.png
    <img alt="IBM Security Verify" class="ibm-resize ibm-ab-image trials-image" decoding="async" src="data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7" style="position:absolute;top:0;left:0;bottom:0;right:0;box-sizing:border-box;padding:0;border:none;margin:auto;display:block;width:0;height:0;min-width:100%;max-width:100%;min-height:100%;max-height:100%"/>
    data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7


## Scrape data from HTML tables



```python
#The below url contains an html table with data about colors and color codes.
url = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DA0321EN-SkillsNetwork/labs/datasets/HTMLColorCodes.html"
```

Before proceeding to scrape a web site, you need to examine the contents, and the way data is organized on the website. Open the above url in your browser and check how many rows and columns are there in the color table.



```python
# get the contents of the webpage in text format and store in a variable called data
data  = requests.get(url).text
```


```python
soup = BeautifulSoup(data,"html.parser")
```


```python
#find a html table in the web page
table = soup.find('table') # in html table is represented by the tag <table>
```


```python
#Get all rows from the table
for row in table.find_all('tr'): # in html table row is represented by the tag <tr>
    # Get all columns in each row.
    cols = row.find_all('td') # in html a column is represented by the tag <td>
    color_name = cols[2].string # store the value in column 3 as color_name
    color_code = cols[3].string # store the value in column 4 as color_code
    print("{}--->{}".format(color_name,color_code))
```

    Color Name--->None
    lightsalmon--->#FFA07A
    salmon--->#FA8072
    darksalmon--->#E9967A
    lightcoral--->#F08080
    coral--->#FF7F50
    tomato--->#FF6347
    orangered--->#FF4500
    gold--->#FFD700
    orange--->#FFA500
    darkorange--->#FF8C00
    lightyellow--->#FFFFE0
    lemonchiffon--->#FFFACD
    papayawhip--->#FFEFD5
    moccasin--->#FFE4B5
    peachpuff--->#FFDAB9
    palegoldenrod--->#EEE8AA
    khaki--->#F0E68C
    darkkhaki--->#BDB76B
    yellow--->#FFFF00
    lawngreen--->#7CFC00
    chartreuse--->#7FFF00
    limegreen--->#32CD32
    lime--->#00FF00
    forestgreen--->#228B22
    green--->#008000
    powderblue--->#B0E0E6
    lightblue--->#ADD8E6
    lightskyblue--->#87CEFA
    skyblue--->#87CEEB
    deepskyblue--->#00BFFF
    lightsteelblue--->#B0C4DE
    dodgerblue--->#1E90FF


## Scrape data from HTML tables into a DataFrame using BeautifulSoup and Pandas



```python
import pandas as pd
```


```python
#The below url contains html tables with data about world population.
url = "https://en.wikipedia.org/wiki/World_population"
```

Before proceeding to scrape a web site, you need to examine the contents, and the way data is organized on the website. Open the above url in your browser and check the tables on the webpage.



```python
# get the contents of the webpage in text format and store in a variable called data
data  = requests.get(url).text
```


```python
soup = BeautifulSoup(data,"html.parser")
```


```python
#find all html tables in the web page
tables = soup.find_all('table') # in html table is represented by the tag <table>
```


```python
# we can see how many tables were found by checking the length of the tables list
len(tables)
```




    26



Assume that we are looking for the `10 most densly populated countries` table, we can look through the tables list and find the right one we are look for based on the data in each table or we can search for the table name if it is in the table but this option might not always work.



```python
for index,table in enumerate(tables):
    if ("10 most densely populated countries" in str(table)):
        table_index = index
print(table_index)
```

    5


See if you can locate the table name of the table, `10 most densly populated countries`, below.



```python
print(tables[table_index].prettify())
```

    <table class="wikitable sortable" style="text-align:right">
     <caption>
      10 most densely populated countries
      <small>
       (with population above 5 million)
      </small>
     </caption>
     <tbody>
      <tr>
       <th>
        Rank
       </th>
       <th>
        Country
       </th>
       <th>
        Population
       </th>
       <th>
        Area
        <br/>
        <small>
         (km
         <sup>
          2
         </sup>
         )
        </small>
       </th>
       <th>
        Density
        <br/>
        <small>
         (pop/km
         <sup>
          2
         </sup>
         )
        </small>
       </th>
      </tr>
      <tr>
       <td>
        1
       </td>
       <td align="left">
        <span class="flagicon">
         <img alt="" class="thumbborder" data-file-height="600" data-file-width="900" decoding="async" height="15" src="//upload.wikimedia.org/wikipedia/commons/thumb/4/48/Flag_of_Singapore.svg/23px-Flag_of_Singapore.svg.png" srcset="//upload.wikimedia.org/wikipedia/commons/thumb/4/48/Flag_of_Singapore.svg/35px-Flag_of_Singapore.svg.png 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/4/48/Flag_of_Singapore.svg/45px-Flag_of_Singapore.svg.png 2x" width="23"/>
        </span>
        <a href="/wiki/Singapore" title="Singapore">
         Singapore
        </a>
       </td>
       <td>
        5,704,000
       </td>
       <td>
        710
       </td>
       <td>
        8,033
       </td>
      </tr>
      <tr>
       <td>
        2
       </td>
       <td align="left">
        <span class="flagicon">
         <img alt="" class="thumbborder" data-file-height="600" data-file-width="1000" decoding="async" height="14" src="//upload.wikimedia.org/wikipedia/commons/thumb/f/f9/Flag_of_Bangladesh.svg/23px-Flag_of_Bangladesh.svg.png" srcset="//upload.wikimedia.org/wikipedia/commons/thumb/f/f9/Flag_of_Bangladesh.svg/35px-Flag_of_Bangladesh.svg.png 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/f/f9/Flag_of_Bangladesh.svg/46px-Flag_of_Bangladesh.svg.png 2x" width="23"/>
        </span>
        <a href="/wiki/Bangladesh" title="Bangladesh">
         Bangladesh
        </a>
       </td>
       <td>
        171,670,000
       </td>
       <td>
        143,998
       </td>
       <td>
        1,192
       </td>
      </tr>
      <tr>
       <td>
        3
       </td>
       <td align="left">
        <p>
         <span class="flagicon">
          <img alt="" class="thumbborder" data-file-height="600" data-file-width="1200" decoding="async" height="12" src="//upload.wikimedia.org/wikipedia/commons/thumb/0/00/Flag_of_Palestine.svg/23px-Flag_of_Palestine.svg.png" srcset="//upload.wikimedia.org/wikipedia/commons/thumb/0/00/Flag_of_Palestine.svg/35px-Flag_of_Palestine.svg.png 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/0/00/Flag_of_Palestine.svg/46px-Flag_of_Palestine.svg.png 2x" width="23"/>
         </span>
         <a href="/wiki/State_of_Palestine" title="State of Palestine">
          Palestine
         </a>
        </p>
       </td>
       <td>
        5,266,785
       </td>
       <td>
        6,020
       </td>
       <td>
        847
       </td>
      </tr>
      <tr>
       <td>
        4
       </td>
       <td align="left">
        <span class="flagicon">
         <img alt="" class="thumbborder" data-file-height="600" data-file-width="900" decoding="async" height="15" src="//upload.wikimedia.org/wikipedia/commons/thumb/5/59/Flag_of_Lebanon.svg/23px-Flag_of_Lebanon.svg.png" srcset="//upload.wikimedia.org/wikipedia/commons/thumb/5/59/Flag_of_Lebanon.svg/35px-Flag_of_Lebanon.svg.png 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/5/59/Flag_of_Lebanon.svg/45px-Flag_of_Lebanon.svg.png 2x" width="23"/>
        </span>
        <a href="/wiki/Lebanon" title="Lebanon">
         Lebanon
        </a>
       </td>
       <td>
        6,856,000
       </td>
       <td>
        10,452
       </td>
       <td>
        656
       </td>
      </tr>
      <tr>
       <td>
        5
       </td>
       <td align="left">
        <span class="flagicon">
         <img alt="" class="thumbborder" data-file-height="600" data-file-width="900" decoding="async" height="15" src="//upload.wikimedia.org/wikipedia/commons/thumb/7/72/Flag_of_the_Republic_of_China.svg/23px-Flag_of_the_Republic_of_China.svg.png" srcset="//upload.wikimedia.org/wikipedia/commons/thumb/7/72/Flag_of_the_Republic_of_China.svg/35px-Flag_of_the_Republic_of_China.svg.png 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/7/72/Flag_of_the_Republic_of_China.svg/45px-Flag_of_the_Republic_of_China.svg.png 2x" width="23"/>
        </span>
        <a href="/wiki/Taiwan" title="Taiwan">
         Taiwan
        </a>
       </td>
       <td>
        23,604,000
       </td>
       <td>
        36,193
       </td>
       <td>
        652
       </td>
      </tr>
      <tr>
       <td>
        6
       </td>
       <td align="left">
        <span class="flagicon">
         <img alt="" class="thumbborder" data-file-height="600" data-file-width="900" decoding="async" height="15" src="//upload.wikimedia.org/wikipedia/commons/thumb/0/09/Flag_of_South_Korea.svg/23px-Flag_of_South_Korea.svg.png" srcset="//upload.wikimedia.org/wikipedia/commons/thumb/0/09/Flag_of_South_Korea.svg/35px-Flag_of_South_Korea.svg.png 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/0/09/Flag_of_South_Korea.svg/45px-Flag_of_South_Korea.svg.png 2x" width="23"/>
        </span>
        <a href="/wiki/South_Korea" title="South Korea">
         South Korea
        </a>
       </td>
       <td>
        51,781,000
       </td>
       <td>
        99,538
       </td>
       <td>
        520
       </td>
      </tr>
      <tr>
       <td>
        7
       </td>
       <td align="left">
        <span class="flagicon">
         <img alt="" class="thumbborder" data-file-height="720" data-file-width="1080" decoding="async" height="15" src="//upload.wikimedia.org/wikipedia/commons/thumb/1/17/Flag_of_Rwanda.svg/23px-Flag_of_Rwanda.svg.png" srcset="//upload.wikimedia.org/wikipedia/commons/thumb/1/17/Flag_of_Rwanda.svg/35px-Flag_of_Rwanda.svg.png 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/1/17/Flag_of_Rwanda.svg/45px-Flag_of_Rwanda.svg.png 2x" width="23"/>
        </span>
        <a href="/wiki/Rwanda" title="Rwanda">
         Rwanda
        </a>
       </td>
       <td>
        12,374,000
       </td>
       <td>
        26,338
       </td>
       <td>
        470
       </td>
      </tr>
      <tr>
       <td>
        8
       </td>
       <td align="left">
        <span class="flagicon">
         <img alt="" class="thumbborder" data-file-height="600" data-file-width="1000" decoding="async" height="14" src="//upload.wikimedia.org/wikipedia/commons/thumb/5/56/Flag_of_Haiti.svg/23px-Flag_of_Haiti.svg.png" srcset="//upload.wikimedia.org/wikipedia/commons/thumb/5/56/Flag_of_Haiti.svg/35px-Flag_of_Haiti.svg.png 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/5/56/Flag_of_Haiti.svg/46px-Flag_of_Haiti.svg.png 2x" width="23"/>
        </span>
        <a href="/wiki/Haiti" title="Haiti">
         Haiti
        </a>
       </td>
       <td>
        11,578,000
       </td>
       <td>
        27,065
       </td>
       <td>
        428
       </td>
      </tr>
      <tr>
       <td>
        9
       </td>
       <td align="left">
        <span class="flagicon">
         <img alt="" class="thumbborder" data-file-height="600" data-file-width="900" decoding="async" height="15" src="//upload.wikimedia.org/wikipedia/commons/thumb/2/20/Flag_of_the_Netherlands.svg/23px-Flag_of_the_Netherlands.svg.png" srcset="//upload.wikimedia.org/wikipedia/commons/thumb/2/20/Flag_of_the_Netherlands.svg/35px-Flag_of_the_Netherlands.svg.png 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/2/20/Flag_of_the_Netherlands.svg/45px-Flag_of_the_Netherlands.svg.png 2x" width="23"/>
        </span>
        <a href="/wiki/Netherlands" title="Netherlands">
         Netherlands
        </a>
       </td>
       <td>
        17,660,000
       </td>
       <td>
        41,526
       </td>
       <td>
        425
       </td>
      </tr>
      <tr>
       <td>
        10
       </td>
       <td align="left">
        <span class="flagicon">
         <img alt="" class="thumbborder" data-file-height="800" data-file-width="1100" decoding="async" height="15" src="//upload.wikimedia.org/wikipedia/commons/thumb/d/d4/Flag_of_Israel.svg/21px-Flag_of_Israel.svg.png" srcset="//upload.wikimedia.org/wikipedia/commons/thumb/d/d4/Flag_of_Israel.svg/32px-Flag_of_Israel.svg.png 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/d/d4/Flag_of_Israel.svg/41px-Flag_of_Israel.svg.png 2x" width="21"/>
        </span>
        <a href="/wiki/Israel" title="Israel">
         Israel
        </a>
       </td>
       <td>
        9,430,000
       </td>
       <td>
        22,072
       </td>
       <td>
        427
       </td>
      </tr>
     </tbody>
    </table>
    



```python
population_data = pd.DataFrame(columns=["Rank", "Country", "Population", "Area", "Density"])

for row in tables[table_index].tbody.find_all("tr"):
    col = row.find_all("td")
    if (col != []):
        rank = col[0].text
        country = col[1].text
        population = col[2].text.strip()
        area = col[3].text.strip()
        density = col[4].text.strip()
        population_data = population_data.append({"Rank":rank, "Country":country, "Population":population, "Area":area, "Density":density}, ignore_index=True)

population_data
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Rank</th>
      <th>Country</th>
      <th>Population</th>
      <th>Area</th>
      <th>Density</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Singapore</td>
      <td>5,704,000</td>
      <td>710</td>
      <td>8,033</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Bangladesh</td>
      <td>171,670,000</td>
      <td>143,998</td>
      <td>1,192</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>\n Palestine\n\n</td>
      <td>5,266,785</td>
      <td>6,020</td>
      <td>847</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Lebanon</td>
      <td>6,856,000</td>
      <td>10,452</td>
      <td>656</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Taiwan</td>
      <td>23,604,000</td>
      <td>36,193</td>
      <td>652</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>South Korea</td>
      <td>51,781,000</td>
      <td>99,538</td>
      <td>520</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>Rwanda</td>
      <td>12,374,000</td>
      <td>26,338</td>
      <td>470</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>Haiti</td>
      <td>11,578,000</td>
      <td>27,065</td>
      <td>428</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>Netherlands</td>
      <td>17,660,000</td>
      <td>41,526</td>
      <td>425</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>Israel</td>
      <td>9,430,000</td>
      <td>22,072</td>
      <td>427</td>
    </tr>
  </tbody>
</table>
</div>



## Scrape data from HTML tables into a DataFrame using BeautifulSoup and read_html


Using the same `url`, `data`, `soup`, and `tables` object as in the last section we can use the `read_html` function to create a DataFrame.

Remember the table we need is located in `tables[table_index]`

We can now use the `pandas` function `read_html` and give it the string version of the table as well as the `flavor` which is the parsing engine `bs4`.



```python
pd.read_html(str(tables[5]), flavor='bs4')
```




    [   Rank      Country  Population  Area(km2)  Density(pop/km2)
     0     1    Singapore     5704000        710              8033
     1     2   Bangladesh   171670000     143998              1192
     2     3    Palestine     5266785       6020               847
     3     4      Lebanon     6856000      10452               656
     4     5       Taiwan    23604000      36193               652
     5     6  South Korea    51781000      99538               520
     6     7       Rwanda    12374000      26338               470
     7     8        Haiti    11578000      27065               428
     8     9  Netherlands    17660000      41526               425
     9    10       Israel     9430000      22072               427]



The function `read_html` always returns a list of DataFrames so we must pick the one we want out of the list.



```python
population_data_read_html = pd.read_html(str(tables[5]), flavor='bs4')[0]

population_data_read_html
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Rank</th>
      <th>Country</th>
      <th>Population</th>
      <th>Area(km2)</th>
      <th>Density(pop/km2)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Singapore</td>
      <td>5704000</td>
      <td>710</td>
      <td>8033</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Bangladesh</td>
      <td>171670000</td>
      <td>143998</td>
      <td>1192</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Palestine</td>
      <td>5266785</td>
      <td>6020</td>
      <td>847</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Lebanon</td>
      <td>6856000</td>
      <td>10452</td>
      <td>656</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Taiwan</td>
      <td>23604000</td>
      <td>36193</td>
      <td>652</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>South Korea</td>
      <td>51781000</td>
      <td>99538</td>
      <td>520</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>Rwanda</td>
      <td>12374000</td>
      <td>26338</td>
      <td>470</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>Haiti</td>
      <td>11578000</td>
      <td>27065</td>
      <td>428</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>Netherlands</td>
      <td>17660000</td>
      <td>41526</td>
      <td>425</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>Israel</td>
      <td>9430000</td>
      <td>22072</td>
      <td>427</td>
    </tr>
  </tbody>
</table>
</div>



## Scrape data from HTML tables into a DataFrame using read_html


We can also use the `read_html` function to directly get DataFrames from a `url`.



```python
dataframe_list = pd.read_html(url, flavor='bs4')
```

We can see there are 25 DataFrames just like when we used `find_all` on the `soup` object.



```python
len(dataframe_list)
```




    26



Finally we can pick the DataFrame we need out of the list.



```python
dataframe_list[5]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Rank</th>
      <th>Country</th>
      <th>Population</th>
      <th>Area(km2)</th>
      <th>Density(pop/km2)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Singapore</td>
      <td>5704000</td>
      <td>710</td>
      <td>8033</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Bangladesh</td>
      <td>171670000</td>
      <td>143998</td>
      <td>1192</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Palestine</td>
      <td>5266785</td>
      <td>6020</td>
      <td>847</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Lebanon</td>
      <td>6856000</td>
      <td>10452</td>
      <td>656</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Taiwan</td>
      <td>23604000</td>
      <td>36193</td>
      <td>652</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>South Korea</td>
      <td>51781000</td>
      <td>99538</td>
      <td>520</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>Rwanda</td>
      <td>12374000</td>
      <td>26338</td>
      <td>470</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>Haiti</td>
      <td>11578000</td>
      <td>27065</td>
      <td>428</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>Netherlands</td>
      <td>17660000</td>
      <td>41526</td>
      <td>425</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>Israel</td>
      <td>9430000</td>
      <td>22072</td>
      <td>427</td>
    </tr>
  </tbody>
</table>
</div>



We can also use the `match` parameter to select the specific table we want. If the table contains a string matching the text it will be read.



```python
pd.read_html(url, match="10 most densely populated countries", flavor='bs4')[0]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Rank</th>
      <th>Country</th>
      <th>Population</th>
      <th>Area(km2)</th>
      <th>Density(pop/km2)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Singapore</td>
      <td>5704000</td>
      <td>710</td>
      <td>8033</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Bangladesh</td>
      <td>171670000</td>
      <td>143998</td>
      <td>1192</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Palestine</td>
      <td>5266785</td>
      <td>6020</td>
      <td>847</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Lebanon</td>
      <td>6856000</td>
      <td>10452</td>
      <td>656</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Taiwan</td>
      <td>23604000</td>
      <td>36193</td>
      <td>652</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>South Korea</td>
      <td>51781000</td>
      <td>99538</td>
      <td>520</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>Rwanda</td>
      <td>12374000</td>
      <td>26338</td>
      <td>470</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>Haiti</td>
      <td>11578000</td>
      <td>27065</td>
      <td>428</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>Netherlands</td>
      <td>17660000</td>
      <td>41526</td>
      <td>425</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>Israel</td>
      <td>9430000</td>
      <td>22072</td>
      <td>427</td>
    </tr>
  </tbody>
</table>
</div>



## Authors


Ramesh Sannareddy


### Other Contributors


Rav Ahuja


## Change Log


| Date (YYYY-MM-DD) | Version | Changed By                                               | Change Description |
| ----------------- | ------- | -------------------------------------------------------- | ------------------ |
| 2021-08-04        | 0.2     | Made changes to markdown of nextsibling                  |                    |
| 2020-10-17        | 0.1     | Joseph Santarcangelo  Created initial version of the lab |                    |


Copyright © 2020 IBM Corporation. This notebook and its source code are released under the terms of the [MIT License](https://cognitiveclass.ai/mit-license?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkPY0220ENSkillsNetwork23455606-2021-01-01&cm_mmc=Email_Newsletter-\_-Developer_Ed%2BTech-\_-WW_WW-\_-SkillsNetwork-Courses-IBMDeveloperSkillsNetwork-PY0101EN-SkillsNetwork-19487395&cm_mmca1=000026UJ&cm_mmca2=10006555&cm_mmca3=M12345678&cvosrc=email.Newsletter.M12345678&cvo_campaign=000026UJ).



```python

```


```python

```
