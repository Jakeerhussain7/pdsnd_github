>**Note**: Please **fork** the current Udacity repository so that you will have a **remote** repository in **your** Github account. Clone the remote repository to your local machine. Later, as a part of the project "Post your Work on Github", you will push your proposed changes to the remote repository in your Github account.

### Date created
feb 8 2022 and README file.

### Project Title
Bikeshare

### Description
this project will give bikeshare report of the cities ie chicago, washington_dc, newyork_city

### Files used
chicago.csv, washington_dc.csv, newyork_city.csv

### Credits
It's important to give proper credit. Add links to any repo that inspired you or blogposts you consulted.

import time
import pandas as pd
import numpy as np

CITY_DATA = { 'chicago': 'chicago.csv',
              'new york city': 'new_york_city.csv',
              'washington': 'washington.csv' }

def get_filters():
    """
    Asks user to specify a city, month, and day to analyze.

    Returns:
        (str) city - name of the city to analyze
        (str) month - name of the month to filter by, or "all" to apply no month filter
        (str) day - name of the day of week to filter by, or "all" to apply no day filter
    """
    print('Hello! Let\'s explore some US bikeshare data!')
    # TO DO: get user input for city (chicago, new york city, washington). HINT: Use a while loop to handle invalid inputs
    
    city=input("Choose a city name from chicago, new york city and washington: ").lower()
    while city  not in CITY_DATA.keys():
        print("Invalid city")
        city=input("Choose a city name from chicago, new york city and washington: ").lower()
    


    # TO DO: get user input for month (all, january, february, ... , june)
    
    months=['all', 'january', 'february','march','april','may','june']
    while True:
        month=input("choose a month: ").lower()
        if month in months:
            break
        else:
            print("Invalid month")
            

    # TO DO: get user input for day of week (all, monday, tuesday, ... sunday)
    days=['all', 'monday', 'tuesday','wednesday','thrusday','friday','saturday','sunday']
    while True:
        day=input("choose a day in a week: ").lower()
        if day in days:
            break
        else:
            print("Invalid day")


    print('-'*40)
    return city, month, day


def load_data(city, month, day):
    """
    Loads data for the specified city and filters by month and day if applicable.

    Args:
        (str) city - name of the city to analyze
        (str) month - name of the month to filter by, or "all" to apply no month filter
        (str) day - name of the day of week to filter by, or "all" to apply no day filter
    Returns:
        df - Pandas DataFrame containing city data filtered by month and day
    """
    df = pd.read_csv(CITY_DATA[city])
    df['Start Time'] = pd.to_datetime(df['Start Time'])
    df['start_hour'] =  df['Start Time'].dt.hour
    df['month'] = df['Start Time'].dt.month
    df['day_of_week'] =  df['Start Time'].dt.weekday_name
    
    
    if month != 'all':
        months=['january', 'february','march','april','may','june']
        month = months.index(month) + 1
        df = df[df['month'] == month]

    if day != 'all':
        df = df[df['day_of_week'] == day.title()]
    
    return df


def time_stats(df):
    """Displays statistics on the most frequent times of travel."""

    print('\nCalculating The Most Frequent Times of Travel...\n')
    start_time = time.time()

    # TO DO: display the most common month
    print("The Most Common month: {} ".format(df['month'].mode()[0]))


    # TO DO: display the most common day of week
    print("The Most Common day of the week: {} ".format(df['day_of_week'].mode()[0]))


    # TO DO: display the most common start hour
    print("The Most Common Start hour: {} ".format(df['start_hour'].mode()[0]))


    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def station_stats(df):
    """Displays statistics on the most popular stations and trip."""

    print('\nCalculating The Most Popular Stations and Trip...\n')
    start_time = time.time()

    # TO DO: display most commonly used start station
    print("The Most Commonly used start station: {} ".format(df['Start Station'].mode()[0]))


    # TO DO: display most commonly used end station
    print("The Most Commonly used end station: {} ".format(df['End Station'].mode()[0]))


    # TO DO: display most frequent combination of start station and end station trip
    df['route']=df['Start Station']+", "+df['End Station']
    print('The most frequent route: {}'.format(df['route'].mode()[0]))


    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def trip_duration_stats(df):
    """Displays statistics on the total and average trip duration."""

    print('\nCalculating Trip Duration...\n')
    start_time = time.time()

    # TO DO: display total travel time
    print("Total Travel Time: ",(df['Trip Duration'].sum()).round())


    # TO DO: display mean travel time
    print("Mean Travel Time: ",(df['Trip Duration'].mean()).round())


    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def user_stats(df,city):
    """Displays statistics on bikeshare users."""

    print('\nCalculating User Stats...\n')
    start_time = time.time()

    # TO DO: Display counts of user types
    print(df['User Type'].value_counts().to_frame())


    # TO DO: Display counts of gender
    if city !='washington':
        print(df['Gender'].value_counts().to_frame())
    # TO DO: Display earliest, most recent, and most common year of birth
        print("The Earliest Year of birth: ",int(df['Birth Year'].min()))
        print("The Most recent Year of birth: ",int(df['Birth Year'].max()))
        print("The Most Common Year of birth: ",int(df['Birth Year'].mode()[0]))
    else:
        print("Data is not found")


    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)
def display_data(df):
    print("Five rows of data is available to check")
    user_input=input("would you like to view 5 rows of individual trip data?, Please type yes or no ").lower()
    if user_input not in ['yes','no']:
        print("Invalid data , Please type yes or no")
        user_input=input("would you like to view 5 rows of individual trip data?, Please type yes or no ").lower()
    elif user_input != 'yes':
        print('Thank you')
    else:
        start_loc=0
        while start_loc+5< df.shape[0]:
            print(df.iloc[start_loc:start_loc+5])
            start_loc+=5
            user_input=input("would you like to view 5 rows of individual trip data?, Please type yes or no ").lower()
            if user_input != 'yes':
                break
    

def main():
    while True:
        city, month, day = get_filters()
        df = load_data(city, month, day)

        time_stats(df)
        station_stats(df)
        trip_duration_stats(df)
        user_stats(df,city)
        display_data(df)

        restart = input('\nWould you like to restart? Enter yes or no.\n')
        if restart.lower() != 'yes':
            break


if __name__ == "__main__":
	main()
