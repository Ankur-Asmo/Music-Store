# Overview
This project explores a Music Store relational database using SQL queries. It demonstrates how to analyze business insights such as customer behavior, sales trends, and artist contributions. The dataset includes tables like Employee, Customer, Invoice, Track, Album, Artist, and Genre.

## Features & Queries
The repository contains SQL scripts that answer real-world business questions:

**Employee Insights**

Find the senior-most employee based on job title.

**Sales & Invoices**

Identify which country has the most invoices.

Retrieve the top 3 invoice totals.

Discover which city generates the highest revenue (ideal for promotional events).

**Customer Analysis**

Determine the best customer (highest spender).

List all Rock music listeners with their email, first name, and last name.

**Artist & Music Trends**

Invite the top 10 artists who have written the most Rock songs.

Find tracks longer than the average song length, ordered by duration.

## Queries

Who is the Senior Most employee Based on Job Title?
```
SELECT * FROM employee
ORDER BY levels DESC
LIMIT 1;
```
Which Countries Have the most Invoices?
```
SELECT billing_country, COUNT(billing_country) AS most_Invoice
FROM invoice
GROUP BY billing_country
ORDER BY most_Invoice DESC
LIMIT 1;
```
What is the top 3 values of total Invoices?
```
SELECT * FROM invoice
ORDER BY total DESC
LIMIT 3;
```
```
SELECT total FROM invoice
ORDER BY total DESC
LIMIT 3;
```
Which city has the best customers we would like to throw a party of promotional music festival in which city
we make the most money write the query that returns the city that has highest sum of invoice total
return both city and sum of total invoices?
```
SELECT billing_city, SUM(total) AS invoice_total
FROM invoice
GROUP BY billing_city
ORDER BY invoice_total DESC
LIMIT 1;
```
Who is the best customer, who has spent the most money will be declared the best customer?
```
SELECT c.customer_id, c.first_name, c.last_name, SUM(i.total) AS most_spent
FROM customer c
JOIN invoice i ON c.customer_id=i.customer_id
GROUP BY c.customer_id
ORDER BY most_spent DESC
LIMIT 1;
```
Write query to return the email, first name, last name, and genre of all Rock music listners return list in order email alphabetical?
```
SELECT DISTINCT c.email, c.first_name, c.last_name
FROM customer c
JOIN invoice i ON c.customer_id = i.invoice_id
JOIN invoice_line il ON i.invoice_id = il.invoice_id
WHERE track_id IN(
	SELECT track_id FROM track t
	JOIN genre g ON t.genre_id = g.genre_id
	WHERE g.name LIKE 'Rock'
	)
ORDER BY c.email;
```
Lets invite the 10 Artists who have written most rock music in dataset?
```
SELECT a.artist_id, a.name, COUNT(a.artist_id) AS number_of_songs
FROM track t
JOIN album al ON al.album_id = t.album_id
JOIN artist a ON a.artist_id = al.artist_id
JOIN genre g ON g.genre_id =t.genre_id
WHERE g.name LIKE 'Rock'
GROUP BY a.artist_id
ORDER BY number_of_songs DESC
LIMIT 10;
```
Written all the track names that have a song lenghth longer than the avg song length,
written the name and MS order by longest song first?
```
SELECT name, milliseconds
FROM track
WHERE milliseconds > (
	SELECT AVG(milliseconds) AS avg_track_length
	FROM track)
ORDER BY milliseconds DESC;
```
