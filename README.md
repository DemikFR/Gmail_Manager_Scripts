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
    <li><a href="#usage">Usage</a></li>
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
* <strong>get_email_data</strong>: This script is for connecting and fetching email data such as sender, subject and message. Additionally, it treats this data and places it in a CSV file. 
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

In addition to these you will have to use the wordcloud <b>lib</b>, <b>yaml</b> <b>imaplib</b>, <b>email</b> and <b>concurrent.futures</b>




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

1. Clone the repo
   ```sh
   git clone https://github.com/DemikFR/Gmail_Manager_Scripts.git
   ```
2. Open the 'credentials.yaml' to enter your previously generated email and password
   ```yaml
    user : "Your email"
    password : "Your password"
   ```

Now, you will be able to use this project to verify your emails on a large scale, in the next topic you will be able to understand how the scripts work.



<!-- USAGE EXAMPLES -->
## Usage

Use this space to show useful examples of how a project can be used. Additional screenshots, code examples and demos work well in this space. You may also link to more resources.

_For more examples, please refer to the [Documentation](https://example.com)_

<p align="right">(<a href="#readme-top">back to top</a>)</p>



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
