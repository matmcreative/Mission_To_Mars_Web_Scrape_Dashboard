# Web Scraping Challenge - Mission to Mars

<img src="mission_to_mars/Images/mission_to_mars.png" width=350 align=right>

## Table of Contents
* [Objective](#Objective)
* [Technologies](#Technologies)
* [Process](#Technologies)
* [Visualization](#Visualization)
* [Troubleshooting](#Troubleshooting)

# Objective | Web Scrape and Display
Build a web application that scrapes various websites for data related to the Mission to Mars and displays the information in a single HTML page.

# Technologies
* HTML5
* CSS
* BeautifulSoup
* Pandas
* Splinter
* MongoDB

# Process

## Step 1 - Scraping
Complete initial scraping using Jupyter Notebook, BeautifulSoup, Pandas, and Requests/Splinter.

* Create a Jupyter Notebook file called `mission_to_mars.ipynb` and use this to complete all of your scraping and analysis tasks. The following outlines what you need to scrape.

### NASA Mars News

* Scrape the [NASA Mars News Site](https://mars.nasa.gov/news/) and collect the latest News Title and Paragraph Text. Assign the text to variables that you can reference later.

```
# results are returned as an iterable list
titles = soup.find_all('div', class_="content_title")
#print(titles[0])
news_titles=titles[0].text.strip()
print(news_titles)
```
```
# results are returned as an iterable list
descs = soup.find_all('div', class_="rollover_description_inner")
#print(descs[0])
news_p=descs[0].text.strip()
news_p
```

### JPL Mars Space Images - Featured Image

* Visit the url for JPL Featured Space Image [here](https://www.jpl.nasa.gov/spaceimages/?search=&category=Mars).

* Use splinter to navigate the site and find the image url for the current Featured Mars Image and assign the url string to a variable called `featured_image_url`.

```
#cleaning up the URL for the most recent featured image from the webpage.
mars='?search=&category=Mars'
# Use Base URL to Create Absolute URL
feimg_url = f"https://www.jpl.nasa.gov{image}"
print(feimg_url)
```

### Mars Weather

* Visit the Mars Weather twitter account [here](https://twitter.com/marswxreport?lang=en) and scrape the latest Mars weather tweet from the page. Save the tweet text for the weather report as a variable called `mars_weather`.

```
#Looking for all paragraph statements in returned object
mars_weather_tweet = tw_soup.find_all('p', class_ = "")
twite=mars_weather_tweet[0].text.strip()


results = tw_soup.find_all('div', class_="")
#tweet = results.find("p", class_="TweetTextSize TweetTextSize--normal js-tweet-text tweet-text")
#tweet_split = tweet.rsplit("pic")
#mars_weather = tweet_split[0]
#print(mars_weather)
print(results)
```

### Mars Facts

* Visit the Mars Facts webpage [here](https://space-facts.com/mars/) and use Pandas to scrape the table containing facts about the planet including Diameter, Mass, etc.
```
#facts table setup
fact_url = 'https://space-facts.com/mars/'
facts_table = pd.read_html(fact_url)
factual_df = facts_table[0]
factual_df.columns = ["Category", "Measurement"]
factual_df = factual_df.set_index("Category")
factual_df
```
* Use Pandas to convert the data to a HTML table string.

```
tablefacts=factual_df.to_html()
tablefacts
```

### Mars Hemispheres

* Visit the USGS Astrogeology site [here](https://astrogeology.usgs.gov/search/results?q=hemisphere+enhanced&k1=target&v1=Mars) to obtain high resolution images for each of Mar's hemispheres.

* You will need to click each of the links to the hemispheres in order to find the image url to the full resolution image.

* Save both the image url string for the full resolution hemisphere image, and the Hemisphere title containing the hemisphere name. Use a Python dictionary to store the data using the keys `img_url` and `title`.

* Append the dictionary with the image url string and the hemisphere title to a list. This list will contain one dictionary for each hemisphere.

```python
# Example:
hemisphere_image_urls = [
    {"title": "Valles Marineris Hemisphere", "img_url": "..."},
    {"title": "Cerberus Hemisphere", "img_url": "..."},
    {"title": "Schiaparelli Hemisphere", "img_url": "..."},
    {"title": "Syrtis Major Hemisphere", "img_url": "..."},
]
```

- - -

## Step 2 - MongoDB and Flask Application

Use MongoDB with Flask templating to create a new HTML page that displays all of the information that was scraped from the URLs above.

* Start by converting your Jupyter notebook into a Python script called `scrape_mars.py` with a function called `scrape` that will execute all of your scraping code from above and return one Python dictionary containing all of the scraped data.

* Next, create a route called `/scrape` that will import your `scrape_mars.py` script and call your `scrape` function.

  * Store the return value in Mongo as a Python dictionary.

* Create a root route `/` that will query your Mongo database and pass the mars data into an HTML template to display the data.

* Create a template HTML file called `index.html` that will take the mars data dictionary and display all of the data in the appropriate HTML elements. Use the following as a guide for what the final product should look like, but feel free to create your own design.

![final_app_part1.png](mission_to_mars/Images/final_app_part1.png)
![final_app_part2.png](mission_to_mars/Images/final_app_part2.png)
