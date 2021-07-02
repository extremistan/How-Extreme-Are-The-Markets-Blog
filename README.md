# How Extreme Are The Markets Blog

All of our analysis and results can be found in the blog on our [website](https://extremistanresearch.com). In this README, we will broadly explain our code, as   well as our thought process in our methodology. 
  
We used 2 python programs to gather the data for our blog. This was neccesaary because we wanted to make sure we got the most accurate data available online for the specific industries we were looking into. For the S&P 500, Gold, and Oil, we used Yahoo Finance. However, we found that the Yahoo Finance data was not too accurate for Bitcoin, Dogecoin, EURUSD, and the Yen, so for these we used data from Investing.com.


## Programs

Both programs were very similar, so I will explain them together first, then the individual differences after. Our program that uses Yahoo Finace Data is called "extreme_market_yahoo.py" and our program that uses investing.com is called "extreme_market_investing.py."

The first step in both programs was to create a list of the symbols and names of the symbols we would be using in that program. Here is the list for extreme_market_yahoo.py:
```
names_list = ["S&P 500 Index", "Gold", "Oil", "Oil Pre 2020"]
symbols_list = ["^GSPC", "XAU=F", "CL=F", "CL=F"]
```
And here is the list for extreme_market_investing.py. Note, we had to download this data as a .xls file before using it in our code.
```
names_list = ["Bitcoin", "Dogecoin", "EURUSD", "Yen"]
symbols_list = ["Bitcoin.xls", "Doge_1.xls", "EURUSD2.xls", "Yen.xls"]
```
After creating these lists, we looped through the lists so that we could run the rest of the program once for each symbol. The method for the two programs slightly differ here due to the format the data is in, so I will explain them seperately now.


## extreme_data_yahoo.py

Here is the code that loops through the symbols, imports daily data, calculates the percent change for each day, and saves that to a temporary list.
```
# Loop through the list of names/symbols
for j in range(len(names_list)):

    # If the name is this specific one, change the end date to before 2020
    if names_list[j] == "Oil Pre 2020":
        data = yf.download(symbols_list[j], start="1993-01-01", end="2019-05-10", interval = "1d")
    # Otherwise, keep the end date in 2021
    else:
        data = yf.download(symbols_list[j], start="1995-01-01", end="2021-05-10", interval = "1d")

    # Create and empty this new list in each loop
    dif = []

    # Loop through the data for the given symbol
    for i in range(len(data["Open"])):
        # Make sure the data exists
        if data["Close"][i] != "" and data["Open"][i] != "":
            # Add the percent change from the day to the list
            dif.append(((data["Close"][i]/data["Open"][i]) - 1) * 100)
```
Note, the "Oil Pre 2020" data is there because oil took a major hit during 2020 which scewed the graph scale, so we wanted to analyize the past data both with and without this extreme. 

After getting the percent change for each symbol, we simply graphed the data and saved the figure.


## extreme_data_investing.py

This code is very similar to the previous program with a few syntactical changes. Here is the code that loops through the data and stores the daily percent change.
```
# Loop through the list of names/symbols
for j in range(len(symbols_list)):

    # Read in our data from an excel spreadsheet
    data = pd.read_excel(symbols_list[j], encoding='utf-16')

    # Create and empty this new list in each loop
    dif = []

    # Loop through the data for the given symbol
    for i in range(len(data["Open"])):
        # Make sure the data exists
        if data["Price"][i] != "" and data["Open"][i] != "":\
            # Add the percent change from the day to the list
            dif.append(((data["Price"][i]/data["Open"][i]) - 1) * 100)
            if dif[len(dif) - 1] > 300:
                print(data["Price"][i])
                print(data["Open"][i])
                print(data["Date"][i])
```
After getting this data, we simply graph and save the figure.
