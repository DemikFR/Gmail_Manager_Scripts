<!-- PROJECT LOGO -->
<br />
<div align="center">
  <a href="https://github.com/othneildrew/Best-README-Template">
    <img src="https://cdn-icons-png.flaticon.com/512/732/732200.png?w=740&t=st=1679934391~exp=1679934991~hmac=747ccfd46c5617f896c97413e77a27397bc70c38a3b6b3f79dba3335dcf346db" alt="Logo" width="80" height="80">
  </a>

  <h3 align="center">Analyze and Delete your Emails</h3>

  <p align="center">
    You can use the scripts to analyze and delete the emails you want, in this document you can know how to use it.
    <br />
    <a href="https://github.com/DemikFR/Gmail_Manager_Scripts"><strong>Explore the docs »</strong></a>
  </p>
</div>



<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li>
      <a href="#usage">Usage</a>
      <ul>
        <li><a href="#get_email_data">get_email_data</a></li>
        <li><a href="#analysis_my_gmail">Analysis_my_Gmail</a></li>
        <li><a href="#delete_emails">delete_emails</a></li>
      </ul>
    </li>
    <li><a href="#roadmap">Roadmap</a></li>
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#license">License</a></li>
    <li><a href="#contact">Contact</a></li>
    <li><a href="#acknowledgments">Acknowledgments</a></li>
  </ol>
</details>



<!-- ABOUT THE PROJECT -->
## About The Project

Noticing that my Gmail inbox had a lot of things, some of which were not even read, I decided to look for a way to delete them all together without the need to select one by one (How long would it take?).

After seeing some articles, I had seen that I could do it through Python and its Imaplib library, which serves precisely to connect to the Gmail API. Since I'm studying Python and there was this need to use it, why not?

I separated 3 Jupyter Notebook scripts (You need to run them in this order):
* <strong>get_email_data</strong>: This script is for connecting and fetching email data such as sender and subject. Additionally, it treats this data and places it in a CSV file. 
* <strong>analysis_my_gmail</strong>: As the name implies, it will do a little analysis with some graphs for interpretation.
* <strong>delete_emails</strong>: Here, the e-mails will be deleted according to the selected senders.

As you can see, a process was followed which will be explained part by part according to the next topics.



## Built With

This project, in addition to being written in Python, used some of its libraries.



* [![Gmail][Gmail.com]][Gmail-url]
* [![Python][Python.py]][Python-url]
* [![Jupyter][Jupyter.ipynb]][Jupyter-url]
* [![Matplotlib][Matplotlib.py]][Matplotlib-url]
* [![Pandas][Pandas.py]][Pandas-url]

In addition to these you will have to use the libs <b>wordcloud</b>, <b>yaml</b> <b>imaplib</b>, <b>email</b> and <b>concurrent.futures</b>




<!-- GETTING STARTED -->
## Getting Started

In addition to installing the libraries as per the prerequisites subtopic below, you will need to enable IMAP in your Gmail and generate an app password, all shown below.



### Prerequisites

Fisrt of all you will need to activate the IMAP and create your APP password:

1. Settings

2. See all settings

3. Fowarding and POP/IMAP

4. At the last, enable the IMAP

[![IMAP][IMAP-Print]]

5. Access [Google account](https://myaccount.google.com/)

6. Security

7. App password

8. Save the password in a safe place



### Installation

1. In your Python, install the libraries used in the scripts
   ```py
   pip install wordcloud yaml imaplib email concurrent.futures matplotlib pandas
   ```

2. Clone the repo
   ```sh
   git clone https://github.com/DemikFR/Gmail_Manager_Scripts.git
   ```
3. Open the 'credentials.yaml' to enter your previously generated email and password
   ```yaml
    user : "Your email"
    password : "Your password"
   ```

Now, you will be able to use this project to verify your emails on a large scale, in the next topic you will be able to understand how the scripts work.



<!-- USAGE -->
## Usage

After you complete the installation steps, you will be able to run the scripts.

### get_email_data

You will get the email credentials placed in the YAML file.
   ```py
    # Get the Credentials
    
    my_credentials = yaml.load(credentials, Loader = yaml.FullLoader)
    user, password = my_credentials['user'], my_credentials['password']
   ```
   
With the credentials, you can connect Python with Gmail
   ```py
    # Connect your Gmail
    
    imap_url = 'imap.gmail.com'
    my_email = imaplib.IMAP4_SSL(imap_url)
    my_email.login(user, password)
   ```
 
Now, you will need to know and get the email ids, note that you can filter the search by sender, subjects and others. It will be mentioned in more detail in the 3rd script.
   ```py
    emails = my_email.search(None, 'ALL')
   ```

Finally it is possible to fetch the emails from the IDs. In the code below it will fetch the date, sender (name and email address) and subject. Right after the search is done, each e-mail will be added to a list and then placed in a Pandas dataframe.
   ```py
    #fetch e-mails
    
    def fetch(id):
      with concurrent.futures.ThreadPoolExecutor() as executor:
        data = executor.submit(my_email.fetch, str(id), '(RFC822)') # Get the message informations (Message, emails, ids, if errors, etc...)
        return data.result()[1][0][1] # Return only message and e-mail
        
    # Get the message and the email informations

    emails_list = []
    for i in emails_ids:
        msg = email.message_from_string(str(fetch(i),'ISO-8859-1')) # Transform the e-mails 
        emails_list.append({'Date': msg['Date'], 'From': msg['From'], 'Subject': msg['Subject']})
        
    # Put email data in a dataframe
    
    df = pd.DataFrame(emails_list, columns=['Date', 'From', 'Subject'])
   ```
   
Before leaving the data ready for use, it was necessary to clean the data due to the encoding, in this case, the e-mails are extracted as 'ISO-8859-1', however, I have several e-mails in Portuguese, so it was necessary to convert to UTF-8. 

In addition, to carry out the analysis of the emails, I had to create a new column for the email addresses of the senders, as the same comes in a column along with the name. To do so, it was necessary to use Python's <code>extract</code> method with a Regex expression, as shown below:
   ```py
    # Take the email address and put it in a new column
   
    df1['Email'] = df1['From'].str.extract(r'<(.+)>')
    
    # Delete the e-mail from the name column
    
    df1['Sender'] = df1['From'].str.extract(r'(?:"|^)(.*?)(?:"|\s)(?:\s*<|$)')
   ```
   
With the data ready, you can save a .CSV file for later use in other scripts.
   ```py
    df1.to_csv('Emails_Dataset.csv', sep=';', encoding='utf-8')
   ```
### analysis_my_gmail

In this script, you will be able to generate 4 metrics and a graph for each, so you will have an overview of who and when you receive the emails, in addition to being able to know which are the main words said. 

The analysis itself will be mentioned in the especially analysis topic.

Four questions were asked to carry out the analysis:

1. Who are the senders who send the most emails?
  Using the Pandas values count and a horizontal bar chart, it was already possible to answer this question.
  ```py
   top_10_senders = df['Sender'].value_counts(sort=True).head(10).to_frame()
  ```
 
2. What were the years that the most email arrived?
  This question has been answered grouping by year and counting the years values.
  ```py
   year_values = df.groupby(df['Date'].dt.year, group_keys=False)['Date'].apply(lambda x: x.count()).to_frame()
  ``` 
  
  
3. What were the months that the most email arrived?
  This question has been answered grouping by months and counting the months values.
  ```py
   month_values = df.groupby(df['Date'].dt.month, group_keys=False)['Date'].apply(lambda x: x.count()).to_frame()
   month_values.index = pd.to_datetime(month_values.index, format='%m').strftime('%b') #Tranform month numbers
  ``` 
  Note that the months were numeric and were converted to alphabetic characters directly in the database.
  
4. What are the words that appear the most in emails?
  To perform the word cloud, all the Subject field records were concatenated to a string, in addition to some having to convert the encoding.
  ```py
   Subject = df['Subject'].str.cat(sep=' ')
   Subject = Subject.encode('utf-8').replace(b'\xe2\x80\x8a', b'').replace(b'\r\n', b'').decode('utf-8')
  ``` 
  
After the metrics were made, the graphs were made, which will be discussed in a later topic.
 
### delete_emails
 
This last script will look for the e-mails following a predefined pattern and delete them.
 
For this, you must connect to the email again and use the <code>search</code> command of Imap, it is there where you can define which emails will be deleted.
 
In my case, I created a list with the name of some senders, such as social networks and some stores and with that, I did a search for your email address in the dataframe created in script 1, below you can see how it was done.
```py
 # Create the list 
 
 senders = ['Instagram', 'Twitter', 'Facebook', 'LinkedIn', 'TikTok', 'Pinterest', 'Reddit', 'YouTube',
               'Telegram', 'Discord', 'Flickr', 'Twitch', 'Medium', 'Quora', 'Dafiti']
               
 # Creates a Regex expression to search the dataframe
 
 senders_regex = '|'.join(senders)
 senders_regex
 
 # Search the e-mail adress
 
 df_media = df[df['Sender'].str.contains(senders_regex, regex=True)]['Email'].drop_duplicates()
 df_media
``` 
With the e-mail addresses to be deleted, I was able to carry out the search through a Python code that runs through the series created by searching for each sender.
```py
  emails_ids = []
  for i in df_media:
    emails = my_email.search(None, f'FROM "{i}"')
    emails_ids.extend(emails[1][0].decode().split())
``` 

Note that through the <code>search</code> command you can use different ways to find the emails to delete, in my case, I used the 'From' criterion which is precisely the email address, in others cases, you could, for example, put a pattern that if the message had "Announcement" or "Offer" it would look for it, so using 'From' would be 'Text.' 

You can find all the criteria by [clicking here](https://gist.github.com/martinrusev/6121028).

Finally, you will be able to delete these emails.
```py
  # Move selected emails to the deleted folder
  for i in emails_ids:
    my_email.store(i, '+FLAGS', '\\Deleted')
  # Permanently removing deleted items from the selected mailbox.
  my_email.expunge()
``` 
   
<!-- ROADMAP -->
## Roadmap

- [x] Add Changelog
- [x] Add back to top links
- [ ] Add Additional Templates w/ Examples
- [ ] Add "components" document to easily copy & paste sections of the readme
- [ ] Multi-language Support
    - [ ] Chinese
    - [ ] Spanish

See the [open issues](https://github.com/othneildrew/Best-README-Template/issues) for a full list of proposed features (and known issues).

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- CONTRIBUTING -->
## Contributing

Contributions are what make the open source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement".
Don't forget to give the project a star! Thanks again!

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- LICENSE -->
## License

Distributed under the MIT License. See `LICENSE.txt` for more information.

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- CONTACT -->
## Contact

Your Name - [@your_twitter](https://twitter.com/your_username) - email@example.com

Project Link: [https://github.com/your_username/repo_name](https://github.com/your_username/repo_name)




<!-- ACKNOWLEDGMENTS -->
## Acknowledgments

Use this space to list resources you find helpful and would like to give credit to. I've included a few of my favorites to kick things off!

* [Choose an Open Source License](https://choosealicense.com)
* [GitHub Emoji Cheat Sheet](https://www.webpagefx.com/tools/emoji-cheat-sheet)
* [Malven's Flexbox Cheatsheet](https://flexbox.malven.co/)
* [Malven's Grid Cheatsheet](https://grid.malven.co/)
* [Img Shields](https://shields.io)
* [GitHub Pages](https://pages.github.com)
* [Font Awesome](https://fontawesome.com)
* [React Icons](https://react-icons.github.io/react-icons/search)



<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[Gmail.com]: https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white
[Gmail-url]: https://www.google.com/intl/pt-BR/gmail/about/
[IMAP-Print]: https://user-images.githubusercontent.com/102700735/228278183-25dff5f1-a7cf-4def-9c9d-5fe693c2e762.png
[Google.account]: https://myaccount.google.com/
[Python.py]: https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54
[Python-url]: https://www.python.org/
[license-shield]: https://img.shields.io/github/license/othneildrew/Best-README-Template.svg?style=for-the-badge
[license-url]: https://github.com/othneildrew/Best-README-Template/blob/master/LICENSE.txt
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://linkedin.com/in/othneildrew
[Jupyter.ipynb]: https://img.shields.io/badge/jupyter-%23FA0F00.svg?style=for-the-badge&logo=jupyter&logoColor=white
[Jupyter-url]: https://jupyter.org/
[Matplotlib.py]: https://img.shields.io/badge/Matplotlib-%23ffffff.svg?style=for-the-badge&logo=Matplotlib&logoColor=black
[Matplotlib-url]: https://matplotlib.org/
[Pandas.py]: https://img.shields.io/badge/pandas-%23150458.svg?style=for-the-badge&logo=pandas&logoColor=white
[Pandas-url]: https://pandas.pydata.org/
[imap-search]: https://gist.github.com/martinrusev/6121028
