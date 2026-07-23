# Restaurant Ratings Analysis

## Table of Content

* [Case Study](#case-study)
* [How to Run This Project](#how-to-run-this-project)
* [Dataset Description](#dataset-description)
* [ER Diagram](#er-diagram)
* [Data Cleaning](#data-cleaning)
* [Data Analysis](#data-analysis)
* [Dashboard](#dashboard)

## Case Study
An analysis of restaurant reviews submitted by real diners across Mexico in 2012, combined with supporting details on each restaurant's cuisine offerings and each consumer's personal preferences.

## How to Run This Project
This project is built in Microsoft Power BI, so you'll need Power BI Desktop installed to open and explore it.

1. **Install Power BI Desktop** - Download and install the free [Power BI Desktop](https://powerbi.microsoft.com/desktop/) app if you don't already have it (Windows only).
2. **Clone or download this repository** so you have the `Datasets` folder and the `Restaurant Ratings Analysis.pbix` file on your machine.
3. **Open the report** - Launch `Restaurant Ratings Analysis.pbix` directly in Power BI Desktop. The visuals and data model should load automatically.
4. **Reconnect the data source (if prompted)** - If Power BI can't locate the source files, point it to the local `Datasets` folder:
   - Go to **Home -> Transform Data -> Data Source Settings**
   - Select the folder source and click **Change Source**
   - Browse to the `Datasets` folder in your local copy of this repository, then click **OK**
   - Click **Refresh** to reload the data
5. **Explore the dashboard** - Use the report pages and slicers to filter by city, state, cuisine, price tier, and more.

To build the report from scratch instead of opening the existing `.pbix` file, follow the [Data Cleaning](#data-cleaning) steps below to import the CSV files from the `Datasets` folder and recreate the calculated fields.

## Dataset Description
The data is organized into the following tables:

#### Consumers
- **Consumer_ID** - A unique ID assigned to each consumer
- **City** - The consumer's city of residence
- **State** - The consumer's state of residence
- **Country** - The consumer's country of residence
- **Latitude** - The latitude of the consumer's residence
- **Longitude** - The longitude of the consumer's residence
- **Smoker** - Indicates whether or not the consumer smokes
- **Drink_Level** - Categorizes the consumer's drinking habits as abstemious, casual, or social
- **Transportation_Method** - The consumer's usual mode of transport: on foot, public transit, or car
- **Marital_Status** - Whether the consumer is single or married
- **Children** - Notes whether the consumer has dependent or independent children
- **Age** - The consumer's age
- **Occupation** - The consumer's occupation status: student, employed, or unemployed
- **Budget** - The consumer's spending budget: low, medium, or high

#### Consumer_Preferences
- **Consumer_ID** - A unique ID assigned to each consumer
- **Preferred_Cuisine** - The cuisine types favored by the consumer

#### Ratings
- **Consumer_ID** - A unique ID assigned to each consumer
- **Restaurant_ID** - A unique ID assigned to each restaurant
- **Overall_Rating** - The consumer's overall rating of the restaurant (0 = Unsatisfactory, 1 = Satisfactory, 2 = Highly Satisfactory)
- **Food_Rating** - The consumer's rating of the food (0 = Unsatisfactory, 1 = Satisfactory, 2 = Highly Satisfactory)
- **Service_Rating** - The consumer's rating of the service (0 = Unsatisfactory, 1 = Satisfactory, 2 = Highly Satisfactory)

#### Restaurants
- **Restaurant_ID** - A unique ID assigned to each restaurant
- **Name** - The name of the restaurant
- **City** - The city the restaurant is located in
- **State** - The state the restaurant is located in
- **Country** - The country the restaurant is located in
- **Zip_Code** - The restaurant's postal code
- **Latitude** - The latitude of the restaurant's location
- **Longitude** - The longitude of the restaurant's location
- **Alcohol_Service** - The type of alcohol service offered: none, wine & beer, or full bar
- **Smoking_Allowed** - Indicates whether smoking is permitted anywhere, including bars or designated sections
- **Price** - The restaurant's price tier: low, medium, or high
- **Franchise** - Whether the restaurant is part of a franchise
- **Area** - Whether the restaurant occupies an open or closed space
- **Parking** - The type of parking offered: none, available, public, or valet

#### Restaurant_Cuisines
- **Restaurant_ID** - A unique ID assigned to each restaurant
- **Cuisine** - The type of cuisine served by the restaurant

## ER Diagram
![Restaurant-ratings drawio](https://github.com/aksingh2908/assits/blob/main/344537311-90bef193-a0bb-4211-96ca-a7587b16f2d1.png)

## Data Cleaning
### Steps for importing the data as a folder
1. Go to Get Data -> More -> All -> Folder -> Connect -> select the path to the dataset folder -> click OK
2. Open Transform Data -> duplicate the file -> expand the dataset via the Binary column (repeat for each dataset)
3. Create the calculated fields below

#### Age Group
```
AgeGroup = 
SWITCH(
    TRUE(),
    consumers[Age] <= 18, "Children and Adolescents",
    consumers[Age] <= 30, "Young Adults",
    consumers[Age] <= 45, "Adults",
    consumers[Age] <= 60, "Middle-aged Adults",
    "Seniors"
)
```
#### Service Rating Category
```
Service_Rating_Category = SWITCH(
    TRUE(),
    ratings[Service_Rating] = 0, "Unsatisfactory",
    ratings[Service_Rating] = 1, "Satisfactory",
    "Highly Satisfactory"
)
```
#### Overall Rating Category
```
Overall_Rating_Category = SWITCH(
    TRUE(),
    ratings[Overall_Rating] = 0, "Unsatisfactory",
    ratings[Overall_Rating] = 1, "Satisfactory",
    "Highly Satisfactory"
)
```
#### Food Rating Category
```
Food_Rating_Category = SWITCH(
    TRUE(),
    ratings[Food_Rating] = 0, "Unsatisfactory",
    ratings[Food_Rating] = 1, "Satisfactory",
    "Highly Satisfactory"
)
```

## Data Analysis
### Local Insights:
- How are consumers distributed across cities and states?

    San Luis Potosí, San Luis Potosí is home to the largest share of consumers, followed by Cuernavaca, Morelos, in second place.

- How does consumer age distribution differ between states?

    Young adults under 30 make up the majority in all three states. In San Luis Potosí and Morelos, seniors over 60 form the second-largest age group.

- What share of consumers smoke versus don't, by city?

    Non-smokers dominate in every city, with Jiutepec reporting a fully non-smoking consumer base. Cuernavaca has the highest smoker share at 25%.

- How available is restaurant parking across different cities?
    
    Most restaurants across all cities have no parking, though some do offer it. Valet parking is available at two restaurants in both San Luis Potosí and Cuernavaca, while public parking can be found in San Luis Potosí, Ciudad Victoria, and Cuernavaca.

### Dining Insights:
- Is there a link between parking availability and restaurant price tier?
    
    Among the 16 high-priced restaurants, parking is available at all of them: 3 offer valet, 1 offers public parking, and 5 have none. Medium and low-priced restaurants never offer valet parking, though some do provide public or general parking, while others offer none at all.

- How are restaurants distributed by state?

    San Luis Potosí leads with 84 restaurants, while Morelos and Tamaulipas each have 23.

- How do franchise and non-franchise restaurants compare on consumer ratings?

    Non-franchise restaurants make up the majority and are spread fairly evenly across the unsatisfactory, satisfactory, and highly satisfactory categories. The smaller group of franchise restaurants shows a similarly even split.

- What cuisines do consumers prefer, based on their demographics?

    Mexican cuisine ranks first in popularity, with American cuisine close behind.


### Hospitality Insights:
- How does alcohol service vary by city?

    Across the four cities — Jiutepec, San Luis Potosí, Cuernavaca, and Ciudad Victoria — 66.92% of restaurants serve no alcohol, 26.15% offer wine and beer, and 6.93% run a full bar.

- Which transportation methods do consumers rely on most?

    61% of consumers get around by public transportation, 27% drive, and 11% walk.

- Does alcohol service affect how consumers rate their experience?

    Among non-drinkers, 303 rated their visit highly satisfactory, 289 satisfactory, and 170 unsatisfactory. Wine and beer drinkers gave 146 highly satisfactory, 105 satisfactory, and 68 unsatisfactory ratings. At full bars, the split was 37 highly satisfactory, 27 satisfactory, and 16 unsatisfactory.

- What proportion of restaurants permit smoking, by state?

    Roughly 73% of restaurants are entirely smoke-free. Only about 1.5% of restaurants in San Luis Potosí and Morelos allow smoking in bar areas, and about 7% allow smoking overall, with roughly 18.46% providing designated smoking sections.

### Behavior Insights:
- What occupations are most common among consumers in each state?

    In San Luis Potosí, students account for 93% of consumers, with the rest split between employed and unemployed individuals. Morelos is nearly evenly split between employed consumers and students. In Tamaulipas, students make up 94%, with the remaining 6% employed.

- How does drinking behavior (abstemious, casual, social) differ by state?

    In San Luis Potosí, roughly 40% of consumers are social drinkers, 36% casual, and 23% abstemious. In Morelos, 45% are abstemious, 41% casual, and 12% social. In Tamaulipas, 52% are abstemious, 31% casual, and 15% social.

- Is there a connection between marital status and smoking or drinking habits?

    Of 88 single consumers, none smoke, and they skew toward abstemious, then casual, then social drinking in decreasing order. Among married non-smokers, 2 are abstemious and 5 are social drinkers. Separately, 23 single consumers who smoke are split between social and casual drinkers, and 2 married smokers are also social drinkers.

- Does occupation relate to budget level?

    Among students, 67 report a medium budget, 33 a low budget, and 4 a high budget. Additionally, 15 employed consumers and 1 unemployed consumer report a medium budget.

### Review Insights: 
- Which 5 restaurants rank highest for food rating?

    Tortas Locas Hipocampo and Puesto de Tacos lead the pack, with most consumers rating them highly satisfactory. Cafeteria y Restaurante El Pacífico received 9 highly satisfactory food ratings, and Gorditas Doa Gloria received 10. La Cantina Restaurante earned 11 highly satisfactory ratings, with the rest split between satisfactory and unsatisfactory.

- Which 5 restaurants rank highest for service rating?

    Tortas Locas Hipocampo tops the list, with most consumers satisfied with the service. Puesto de Tacos received 12 satisfactory ratings, matched by Cafeteria y Restaurante El Pacífico's 12 and Gorditas Doña Gloria's equal count. La Cantina Restaurante received 7 satisfactory ratings, with the remainder split between highly satisfactory and unsatisfactory.

- Which 5 restaurants rank highest for overall rating?

    Tortas Locas Hipocampo leads with the most highly satisfied consumers, followed by Puesto de Tacos with 30 highly satisfactory ratings. Cafeteria y Restaurante El Pacífico follows with 24, La Cantina Restaurante with 28, and Restaurant la Chalita rounds out the top five with 20 highly satisfactory ratings.
  
## Dashboard
![Restaurant Ratings Analysis_page-0001](https://github.com/aksingh2908/assits/blob/main/344538796-d60cc2b1-5067-4806-8163-bad81914dbd8.jpg)
![Restaurant Ratings Analysis_page-0002](https://github.com/aksingh2908/assits/blob/main/344538806-d31ff0e5-fe29-4d03-80b7-c86f6ee4a665.jpg)
![Restaurant Ratings Analysis_page-0003](https://github.com/aksingh2908/assits/blob/main/344538817-e5d81101-0b31-4969-8556-904dcf398737.jpg)
![Restaurant Ratings Analysis_page-0004](https://github.com/aksingh2908/assits/blob/main/344538835-800542ef-077e-4ed1-947e-b3e3cd7b825d.jpg)
![Restaurant Ratings Analysis_page-0005](https://github.com/aksingh2908/assits/blob/main/344538853-55afce3c-8178-4868-9269-0c4e716d8110.jpg)
