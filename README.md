import random
import psycopg2
from collections import Counter
from math import sqrt

# Function to calculate mean
def calculate_mean(data):
    return sum(data) / len(data)

# Function to calculate median
def calculate_median(data):
    sorted_data = sorted(data)
    n = len(sorted_data)
    if n % 2 == 0:
        return (sorted_data[n//2 - 1] + sorted_data[n//2]) / 2
    else:
        return sorted_data[n//2]

# Function to calculate variance
def calculate_variance(data):
    mean = calculate_mean(data)
    return sum((x - mean) ** 2 for x in data) / len(data)

# Function to generate random 4-digit number of 0s and 1s and convert to base 10
def generate_random_binary():
    return int(''.join(str(random.randint(0, 1)) for _ in range(4)), 2)

# Function to sum the first 50 Fibonacci sequence
def sum_first_50_fibonacci():
    a, b = 0, 1
    total_sum = 0
    for _ in range(50):
        total_sum += a
        a, b = b, a + b
    return total_sum

# Function to save colors and their frequencies in PostgreSQL database
def save_to_database(colors):
    conn = psycopg2.connect(database="your_database_name", user="your_username", password="your_password", host="your_host", port="your_port")
    cursor = conn.cursor()
    
    for color, frequency in colors.items():
        cursor.execute("INSERT INTO colors (color, frequency) VALUES (%s, %s)", (color, frequency))
    
    conn.commit()
    conn.close()

  # Recursive searching algorithm
def recursive_search(number, lst, start=0, end=None):
    if end is None:
        end = len(lst) - 1
    if start > end:
        return False
    mid = (start + end) // 2
    if lst[mid] == number:
        return True
    elif lst[mid] < number:
        return recursive_search(number, lst, mid + 1, end)
    else:
        return recursive_search(number, lst, start, mid - 1)

   # Main program
if __name__ == "__main__":
    # Sample data of shirt colors for demonstration purposes
    shirt_colors = ['red', 'blue', 'green', 'blue', 'red', 'yellow', 'red', 'green', 'blue', 'red', 'blue', 'blue']

    # Calculate mean, median, and variance
    mean_color = calculate_mean(shirt_colors)
    median_color = calculate_median(shirt_colors)
    variance = calculate_variance(shirt_colors)

    # Count frequencies of colors
    color_frequencies = Counter(shirt_colors)

    # Save colors and frequencies to PostgreSQL database
    save_to_database(color_frequencies)

    # Probability of choosing red
    total_colors = sum(color_frequencies.values())
    probability_red = color_frequencies.get('red', 0) / total_colors

    # Generate random binary number and convert to base 10
    random_binary = generate_random_binary()

    # Sum of first 50 Fibonacci numbers
    fibonacci_sum = sum_first_50_fibonacci()

    print("Mean color of shirt:", mean_color)
    print("Median color of shirt:", median_color)
    print("Variance of colors:", variance)
    print("Probability of choosing red color:", probability_red)
    print("Random binary number in base 10:", random_binary)
    print("Sum of first 50 Fibonacci numbers:", fibonacci_sum)

    # Example usage of recursive searching algorithm
    numbers = [1, 3, 5, 7, 9, 11, 13, 15, 17, 19]
    search_number = 11
    if recursive_search(search_number, numbers):
        print(f"{search_number} found in the list.")
    else:
        print(f"{search_number} not found in the list.")
