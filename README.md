# EPAiV5-Session12 - Lazy Evaluation - Generators - CSV File Read

## Overview
This assignment processes large CSV files containing vehicle parking violations data lazily, utilizing Python's lazy evaluation techniques to manage memory efficiently when working with substantial datasets. It parses the CSV data into named tuples, counts occurrences of vehicle manufacturers, and stores these counts in a sorted dictionary. The code is designed to read and process the CSV file line by line rather than loading the entire file into memory at once, making it highly efficient for handling large files.

## Key Features
**Lazy Evaluation:** The core strength of this code lies in its lazy evaluation, which defers the computation of data until it's needed. This helps manage large datasets without consuming excessive memory.
**Namedtuple Usage:** To efficiently handle data rows, we transform each row into a `namedtuple`, which allows us to reference values by attribute names rather than index numbers.
**Type Casting and Cleaning:** The code sanitizes column names and dynamically casts values in each row to specified data types.

## Lazy Evaluation and Its Role
Lazy evaluation is a programming technique where evaluation of expressions is delayed until their values are actually needed. In this code, lazy evaluation is primarily implemented through generators and functions like `yield`, which help avoid loading the entire dataset into memory.

### How Lazy Evaluation is Used:
**Line-by-Line Processing:** The `lazy_car_data_iterator()` function reads the CSV file one line at a time. It uses a generator to yield each processed row, thereby avoiding the need to load the entire file at once into memory.
**Counting Vehicle Manufacturers:** The `lazy_car_make_violations_count()` function counts vehicle manufacturers in a lazy manner. Each time a new row is processed, the count is updated, and the intermediate result is yielded. This enables processing of data as it streams in, rather than after the entire dataset is processed.

### Benefits of Lazy Evaluation:
**Memory Efficiency:** By processing data in chunks and yielding results incrementally, we avoid consuming large amounts of memory. This is particularly useful when dealing with large datasets, like millions of rows in a CSV file.
**Performance:** Lazy evaluation allows the code to start processing and yielding results without waiting for the entire dataset to load, improving the performance of operations on large files.
**Scalability:** With lazy evaluation, the code can easily scale to handle larger datasets without significant changes or performance bottlenecks.

## How Lazy Evaluation Helps in the Code
**1. Memory Optimization:** Both `lazy_car_data_iterator()` and `lazy_car_make_violations_count()` implement lazy evaluation using generators. This avoids loading the entire dataset into memory, making the solution scalable and efficient for processing large datasets. For example, in the following code line instead of holding all rows in memory, it processes one row at a time:


```
for car in lazy_car_data_iterator(file_path):

```

**2. On-demand Execution:** The `type_cast_and_clean_data()` function lazily processes each element in the row, only casting values when needed. This is done using the `yield` keyword to defer execution, as demonstrated here:


```
for value, cast_func in zip(data, cast_type):
    if value != '':
        yield cast_func(value)
    else:
        yield 'N/A'

```

This approach reduces unnecessary computations, improving both memory and CPU efficiency.

**3. Incremental Results:** When counting vehicle manufacturers in `lazy_car_make_violations_count()`, the results are yielded incrementally as new rows are processed, allowing for real-time updates of vehicle counts without waiting for the entire file to be read.

## Code Structure
- `sanitize_column_name(name)`
Replaces invalid characters in column names with underscores to ensure the names are valid for `namedtuple` field names.

- `type_cast_and_clean_data(data, cast_type)`
Lazily casts and cleans the data according to specified types. It handles missing or invalid data by returning `'N/A'` for empty fields.

- `lazy_car_data_iterator(file_path)`
A generator function that lazily reads and processes a CSV file containing vehicle data. It sanitizes the headers and creates namedtuples for each row with casted data types.

- `lazy_car_make_violations_count(file_path)`
This function lazily counts the occurrences of each vehicle manufacturer as it processes the data row by row. It yields the manufacturer and the updated count incrementally.

## Example Usage


```
file_path = 'nyc_parking_tickets_extract-1.csv'
car_make_violations = {}

# Process and count vehicle manufacturers lazily
for vehicle_make, count in lazy_car_make_violations_count(file_path):
    car_make_violations[vehicle_make] = count

# Sort the results by count and print
sorted_vehicle_mfgr = dict(sorted(car_make_violations.items(), key=lambda x: x[1], reverse=True))
for vehicle_make, count in sorted_vehicle_mfgr.items():
    print(f"{vehicle_make}: {count}")

```

## Conclusion
The lazy evaluation techniques implemented in this project improve memory and processing efficiency, particularly when handling large datasets. This approach is ideal for tasks such as analyzing parking ticket data, where the file size can be quite large. By leveraging Python's generators, this code remains efficient and scalable without compromising functionality.

---------------------------------------------------------------------------------------------------------------------------------------------------

**Submission by** - Hema Aparna M

**mail id** - mhema.aprai@gmail.com

---------------------------------------------------------------------------------------------------------------------------------------------------