# CS50x Final Project
## CARDANO Stake Pool - Distribution Explorer

For my final project as part of the Harvard CS50x course, I decided to program a web interface using the following tools:
* Django  
* Python
* Javascript
* CSS
* HTML
* SQL

The purpose of this application is to give any interested party a visual overview of the distribution of various parameters in the CARDANO cryptocurrency’s stake pool network. It can also highlight any currently active stake pool and show its position in relation to the rest of the network.

The application runs in a virtual environment with following dependencies installed:

* asgiref==3.3.1
* certifi==2020.12.5
* chardet==3.0.4
* Django==3.1.4
* idna==2.10
* pytz==2020.4
* requests==2.25.0
* sqlparse==0.4.1
* urllib3==1.26.2
* wincertstore==0.2

Following Node.js modules are used:

* Chart.js
* Chartjs-plugin-zoom

Detailed instruction can be found on:

https://www.chartjs.org/ 

https://www.npmjs.com/package/chartjs-plugin-zoom

### Start
If the program is started from scratch the standard django database migrations have to be made first to initialize the database.

`Python manage.py makemigrations`

`Python manage.py migrate`

To start the development server run the command: `python manage.py runserver`.
By opening `http://127.0.0.1:8000/` the user will be presented with the startup page containing the header bar with all the different chart options and control buttons, a search bar which offers to choose an individual stake pool, and the introductory paragraph explaining the tool.

To get the latest data the user can click on the top right “Update” button. This triggers a web query from the publicly available website https://pooltool.io. Whereas the query is fairly instant, the subsequent checking of the current database entries and updating takes a few moments. On the development environment it takes approximately 3 seconds to complete the update.

Once the database is updated through the online api call, the rest of the functionality is given by api calls to the local SQLight database. The user can now discover the various charts by clicking on the respective buttons.

### The Charts

_**Active/Retired**_ shows the relation between currently active running stake pools and those who are already retired on a pie chart.

**_Pool Pledge_** shows the distribution of the pledge each stake pool has committed in a ranked bar chart. Every stake pool is represented as a single bar with its respective pledge as the height of that bar. The order is descending from highest to lowest to represent the fact that a higher pledge is an indication for more “skin in the game” and hence preferable for the overall robustness of the network.
The legend below the chart from “GOOD” to “BAD” also attempts to illustrate that correlation with its color spectrum. It is to be used more as a general reference than as a strict categorization since the assessment of a pool is always dependent on a combination of various factors. 
The next five charts show other important pool parameters in a similar manner ordered by preferability.
The last chart shows the number of newly created pools in each epoch.

### Interaction

Each bar chart can be zoomed in and out with the mouse wheel. Best practice is to have the mouse hover below the x-axis when zooming in and above the x-axis when zooming out. This is to avoid losing the origin out of sight. Further improvement to this behaviour is intended.

### Search Field

The search field can be used optionally to highlight a specific pool within the charts. It provides a selection of all active pools to choose from. The stake pools’ ticker names were chosen for this field for easier readability and recognition. This means that there will be duplicates in that list of options. However, the underlying dataset is the unique pool ID which will be displayed in the respective charts and should be verified in case there is more than one pool with the same ticker name to choose from.
Once a pool is selected and a chart is created, it will show up in the distribution as a red bar in the corresponding position. Also the legend will now show the unique pool ID. This is especially important when the same ticker name is displayed multiple times in the option menu.

The reset button hides the chart and displays the initial index page again. No interaction with the database.

### Functionality

The web application is built within the django web framework.
One `layout.html` page provides the underlying base.
The `index.html` file written in HTML and CSS contains the body of the page and is linked to the Javascript file `index.js` containing most of the logic and creating the functions for the charts.
A separate `views.py` Python file handles the api call to the online data source as well as several api calls to the local SQLight database. 

### Deployment

As of this writing there is a test deployment to be found [here](http://18.194.87.7). The 'Update' button is not displayed in this version since it is not needed for every individual user to update the database themselves.
