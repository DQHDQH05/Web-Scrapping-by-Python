<center>
    <img src="https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/Logos/organization_logo/organization_logo.png" width="300" alt="cognitiveclass.ai logo"  />
</center>


<h1>Extracting Stock Data Using a Web Scraping</h1>


Not all stock data is available via API in this assignment; you will use web-scraping to obtain financial data. You will be quizzed on your results.\
Using beautiful soup we will extract historical share data from a web-page.


<h2>Table of Contents</h2>
<div class="alert alert-block alert-info" style="margin-top: 20px">
    <ul>
        <li>Downloading the Webpage Using Requests Library</li>
        <li>Parsing Webpage HTML Using BeautifulSoup</li>
        <li>Extracting Data and Building DataFrame</li>
    </ul>
<p>
    Estimated Time Needed: <strong>30 min</strong></p>
</div>

<hr>



```python
#!pip install pandas==1.3.3
#!pip install requests==2.26.0
!mamba install bs4==4.10.0 -y
!mamba install html5lib==1.1 -y
!pip install lxml==4.6.4
#!pip install plotly==5.3.1
```

    
                      __    __    __    __
                     /  \  /  \  /  \  /  \
                    /    \/    \/    \/    \
    ███████████████/  /██/  /██/  /██/  /████████████████████████
                  /  / \   / \   / \   / \  \____
                 /  /   \_/   \_/   \_/   \    o \__,
                / _/                       \_____/  `
                |/
            ███╗   ███╗ █████╗ ███╗   ███╗██████╗  █████╗
            ████╗ ████║██╔══██╗████╗ ████║██╔══██╗██╔══██╗
            ██╔████╔██║███████║██╔████╔██║██████╔╝███████║
            ██║╚██╔╝██║██╔══██║██║╚██╔╝██║██╔══██╗██╔══██║
            ██║ ╚═╝ ██║██║  ██║██║ ╚═╝ ██║██████╔╝██║  ██║
            ╚═╝     ╚═╝╚═╝  ╚═╝╚═╝     ╚═╝╚═════╝ ╚═╝  ╚═╝
    
            mamba (0.15.3) supported by @QuantStack
    
            GitHub:  https://github.com/mamba-org/mamba
            Twitter: https://twitter.com/QuantStack
    
    █████████████████████████████████████████████████████████████
    
    
    Looking for: ['bs4==4.10.0']
    
    pkgs/main/noarch         [>                   ] (--:--) No change
    pkgs/main/noarch         [====================] (00m:00s) No change
    pkgs/r/linux-64          [>                   ] (--:--) No change
    pkgs/r/linux-64          [====================] (00m:00s) No change
    pkgs/main/linux-64       [>                   ] (--:--) No change
    pkgs/main/linux-64       [====================] (00m:00s) No change
    pkgs/r/noarch            [>                   ] (--:--) No change
    pkgs/r/noarch            [====================] (00m:00s) No change
    
    Pinned packages:
      - python 3.7.*
    
    
    Transaction
    
      Prefix: /home/jupyterlab/conda/envs/python
    
      All requested packages already installed
    
    
                      __    __    __    __
                     /  \  /  \  /  \  /  \
                    /    \/    \/    \/    \
    ███████████████/  /██/  /██/  /██/  /████████████████████████
                  /  / \   / \   / \   / \  \____
                 /  /   \_/   \_/   \_/   \    o \__,
                / _/                       \_____/  `
                |/
            ███╗   ███╗ █████╗ ███╗   ███╗██████╗  █████╗
            ████╗ ████║██╔══██╗████╗ ████║██╔══██╗██╔══██╗
            ██╔████╔██║███████║██╔████╔██║██████╔╝███████║
            ██║╚██╔╝██║██╔══██║██║╚██╔╝██║██╔══██╗██╔══██║
            ██║ ╚═╝ ██║██║  ██║██║ ╚═╝ ██║██████╔╝██║  ██║
            ╚═╝     ╚═╝╚═╝  ╚═╝╚═╝     ╚═╝╚═════╝ ╚═╝  ╚═╝
    
            mamba (0.15.3) supported by @QuantStack
    
            GitHub:  https://github.com/mamba-org/mamba
            Twitter: https://twitter.com/QuantStack
    
    █████████████████████████████████████████████████████████████
    
    
    Looking for: ['html5lib==1.1']
    
    pkgs/main/linux-64       Using cache
    pkgs/main/noarch         Using cache
    pkgs/r/linux-64          Using cache
    pkgs/r/noarch            Using cache
    
    Pinned packages:
      - python 3.7.*
    
    
    Transaction
    
      Prefix: /home/jupyterlab/conda/envs/python
    
      All requested packages already installed
    
    Requirement already satisfied: lxml==4.6.4 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (4.6.4)



```python
import pandas as pd
import requests
from bs4 import BeautifulSoup
```

## Using Webscraping to Extract Stock Data Example


First we must use the `request` library to downlaod the webpage, and extract the text. We will extract Netflix stock data <https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/netflix_data_webpage.html>.



```python
url = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/netflix_data_webpage.html"

data  = requests.get(url).text
```

Next we must parse the text into html using `beautiful_soup`



```python
soup = BeautifulSoup(data, 'html5lib')
```

Now we can turn the html table into a pandas dataframe



```python
netflix_data = pd.DataFrame(columns=["Date", "Open", "High", "Low", "Close", "Volume"])

# First we isolate the body of the table which contains all the information
# Then we loop through each row and find all the column values for each row
for row in soup.find("tbody").find_all('tr'):
    col = row.find_all("td")
    date = col[0].text
    Open = col[1].text
    high = col[2].text
    low = col[3].text
    close = col[4].text
    adj_close = col[5].text
    volume = col[6].text
    
    # Finally we append the data of each row to the table
    netflix_data = netflix_data.append({"Date":date, "Open":Open, "High":high, "Low":low, "Close":close, "Adj Close":adj_close, "Volume":volume}, ignore_index=True)    
```

We can now print out the dataframe



```python
netflix_data.head()
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
      <th>Date</th>
      <th>Open</th>
      <th>High</th>
      <th>Low</th>
      <th>Close</th>
      <th>Volume</th>
      <th>Adj Close</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Jun 01, 2021</td>
      <td>504.01</td>
      <td>536.13</td>
      <td>482.14</td>
      <td>528.21</td>
      <td>78,560,600</td>
      <td>528.21</td>
    </tr>
    <tr>
      <th>1</th>
      <td>May 01, 2021</td>
      <td>512.65</td>
      <td>518.95</td>
      <td>478.54</td>
      <td>502.81</td>
      <td>66,927,600</td>
      <td>502.81</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Apr 01, 2021</td>
      <td>529.93</td>
      <td>563.56</td>
      <td>499.00</td>
      <td>513.47</td>
      <td>111,573,300</td>
      <td>513.47</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Mar 01, 2021</td>
      <td>545.57</td>
      <td>556.99</td>
      <td>492.85</td>
      <td>521.66</td>
      <td>90,183,900</td>
      <td>521.66</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Feb 01, 2021</td>
      <td>536.79</td>
      <td>566.65</td>
      <td>518.28</td>
      <td>538.85</td>
      <td>61,902,300</td>
      <td>538.85</td>
    </tr>
  </tbody>
</table>
</div>



We can also use the pandas `read_html` function using the url



```python
read_html_pandas_data = pd.read_html(url)
```

Or we can convert the BeautifulSoup object to a string



```python
read_html_pandas_data = pd.read_html(str(soup))
```

Beacause there is only one table on the page, we just take the first table in the list returned



```python
netflix_dataframe = read_html_pandas_data[0]

netflix_dataframe.head()
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
      <th>Date</th>
      <th>Open</th>
      <th>High</th>
      <th>Low</th>
      <th>Close*</th>
      <th>Adj Close**</th>
      <th>Volume</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Jun 01, 2021</td>
      <td>504.01</td>
      <td>536.13</td>
      <td>482.14</td>
      <td>528.21</td>
      <td>528.21</td>
      <td>78560600</td>
    </tr>
    <tr>
      <th>1</th>
      <td>May 01, 2021</td>
      <td>512.65</td>
      <td>518.95</td>
      <td>478.54</td>
      <td>502.81</td>
      <td>502.81</td>
      <td>66927600</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Apr 01, 2021</td>
      <td>529.93</td>
      <td>563.56</td>
      <td>499.00</td>
      <td>513.47</td>
      <td>513.47</td>
      <td>111573300</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Mar 01, 2021</td>
      <td>545.57</td>
      <td>556.99</td>
      <td>492.85</td>
      <td>521.66</td>
      <td>521.66</td>
      <td>90183900</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Feb 01, 2021</td>
      <td>536.79</td>
      <td>566.65</td>
      <td>518.28</td>
      <td>538.85</td>
      <td>538.85</td>
      <td>61902300</td>
    </tr>
  </tbody>
</table>
</div>



## Using Webscraping to Extract Stock Data Exercise


Use the `requests` library to download the webpage <https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/amazon_data_webpage.html>. Save the text of the response as a variable named `html_data`.



```python
url = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/amazon_data_webpage.html"

html_data  = requests.get(url).text
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    /tmp/ipykernel_65/1500335879.py in <module>
          1 url = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/amazon_data_webpage.html"
          2 
    ----> 3 html_data  = requests.get(url).text
    

    NameError: name 'requests' is not defined


Parse the html data using `beautiful_soup`.



```python
soup = BeautifulSoup(html_data, 'html5lib')
```

<b>Question 1</b> What is the content of the title attribute:



```python
soup.title.string
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    /tmp/ipykernel_65/683123429.py in <module>
    ----> 1 soup.title.string
    

    NameError: name 'soup' is not defined


Using beautiful soup extract the table with historical share prices and store it into a dataframe named `amazon_data`. The dataframe should have columns Date, Open, High, Low, Close, Adj Close, and Volume. Fill in each variable with the correct data from the list `col`.



```python
amazon_data = pd.DataFrame(columns=["Date", "Open", "High", "Low", "Close", "Volume"])

for row in soup.find("tbody").find_all("tr"):
    col = row.find_all("td")
    date = col[0].text
    Open = col[1].text
    high = col[2].text
    low = col[3].text
    close = col[4].text
    adj_close = col[5].text
    volume = col[6].text
    
    amazon_data = amazon_data.append({"Date":date, "Open":Open, "High":high, "Low":low, "Close":close, "Adj Close":adj_close, "Volume":volume}, ignore_index=True)
```

Print out the first five rows of the `amazon_data` dataframe you created.



```python
amazon_data.head()
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
      <th>Date</th>
      <th>Open</th>
      <th>High</th>
      <th>Low</th>
      <th>Close</th>
      <th>Volume</th>
      <th>Adj Close</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Jan 01, 2021</td>
      <td>3,270.00</td>
      <td>3,363.89</td>
      <td>3,086.00</td>
      <td>3,206.20</td>
      <td>71,528,900</td>
      <td>3,206.20</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Dec 01, 2020</td>
      <td>3,188.50</td>
      <td>3,350.65</td>
      <td>3,072.82</td>
      <td>3,256.93</td>
      <td>77,556,200</td>
      <td>3,256.93</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Nov 01, 2020</td>
      <td>3,061.74</td>
      <td>3,366.80</td>
      <td>2,950.12</td>
      <td>3,168.04</td>
      <td>90,810,500</td>
      <td>3,168.04</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Oct 01, 2020</td>
      <td>3,208.00</td>
      <td>3,496.24</td>
      <td>3,019.00</td>
      <td>3,036.15</td>
      <td>116,226,100</td>
      <td>3,036.15</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Sep 01, 2020</td>
      <td>3,489.58</td>
      <td>3,552.25</td>
      <td>2,871.00</td>
      <td>3,148.73</td>
      <td>115,899,300</td>
      <td>3,148.73</td>
    </tr>
  </tbody>
</table>
</div>



<b>Question 2</b> What is the name of the columns of the dataframe



```python
amazon_data.columns
```




    Index(['Date', 'Open', 'High', 'Low', 'Close', 'Volume', 'Adj Close'], dtype='object')



<b>Question 3</b> What is the `Open` of the last row of the amazon_data dataframe?



```python
amazon_data.iloc[-1].loc["Open"]
```




    '656.29'



<h2>About the Authors:</h2> 

<a href="https://www.linkedin.com/in/joseph-s-50398b136/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkPY0220ENSkillsNetwork23455606-2021-01-01">Joseph Santarcangelo</a> has a PhD in Electrical Engineering, his research focused on using machine learning, signal processing, and computer vision to determine how videos impact human cognition. Joseph has been working for IBM since he completed his PhD.

Azim Hirjani


## Change Log

| Date (YYYY-MM-DD) | Version | Changed By | Change Description |
| ----------------- | ------- | ---------- | ------------------ |

```
| 2021-06-09       | 1.2     | Lakshmi Holla|Added URL in question 3 |
```

\| 2020-11-10        | 1.1     | Malika Singla | Deleted the Optional part |
\| 2020-08-27        | 1.0     | Malika Singla | Added lab to GitLab       |

<hr>

## <h3 align="center"> © IBM Corporation 2020. All rights reserved. <h3/>

<p>

