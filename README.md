Medium-Level Questions & Answers:

1. Retrieve Film and Inventory Details
List all films with the store IDs where they are available.

SELECT f.film_id, f.title, i.store_id  
FROM film f  
JOIN inventory i ON f.film_id = i.film_id;
2. List Customers Who Have Rented Movies
Retrieve customer names and emails who have rented at least one movie.

SELECT DISTINCT c.customer_id, c.first_name, c.last_name, c.email  
FROM customers c  
JOIN rental r ON c.customer_id = r.customer_id;
3. Find Rental History for a Specific Film
Get all rental transactions for the movie "Inception" along with customer names.

SELECT r.rental_id, c.first_name, c.last_name, r.rental_date, r.return_date  
FROM rental r  
JOIN customers c ON r.customer_id = c.customer_id  
JOIN inventory i ON r.inventory_id = i.inventory_id  
JOIN film f ON i.film_id = f.film_id  
WHERE f.title = 'Inception';
4. Count Rentals Per Customer
Display the number of rentals made by each customer.

SELECT c.customer_id, c.first_name, c.last_name, COUNT(r.rental_id) AS total_rentals  
FROM customers c  
LEFT JOIN rental r ON c.customer_id = r.customer_id  
GROUP BY c.customer_id, c.first_name, c.last_name  
ORDER BY total_rentals DESC;
5. Find Customers Who Have Not Rented Any Movie
Retrieve customer details who have never rented a movie.

SELECT c.customer_id, c.first_name, c.last_name  
FROM customers c  
LEFT JOIN rental r ON c.customer_id = r.customer_id  
WHERE r.rental_id IS NULL;
6. Find the Most Popular Film
Identify the movie that has been rented the most times.

SELECT f.film_id, f.title, COUNT(r.rental_id) AS rental_count  
FROM rental r  
JOIN inventory i ON r.inventory_id = i.inventory_id  
JOIN film f ON i.film_id = f.film_id  
GROUP BY f.film_id, f.title  
ORDER BY rental_count DESC  
LIMIT 1;
7. Retrieve Customers Who Spent More Than $50 on Rentals
SELECT c.customer_id, c.first_name, c.last_name, SUM(p.amount) AS total_spent  
FROM customers c  
JOIN payment p ON c.customer_id = p.customer_id  
GROUP BY c.customer_id, c.first_name, c.last_name  
HAVING SUM(p.amount) > 50  
ORDER BY total_spent DESC;
8. Find Films That Have Never Been Rented
SELECT f.film_id, f.title  
FROM film f  
LEFT JOIN inventory i ON f.film_id = i.film_id  
LEFT JOIN rental r ON i.inventory_id = r.inventory_id  
WHERE r.rental_id IS NULL;
9. Rank Customers by Their Rental Activity
SELECT c.customer_id, c.first_name, c.last_name,  
       COUNT(r.rental_id) AS total_rentals,  
       RANK() OVER (ORDER BY COUNT(r.rental_id) DESC) AS ranking  
FROM customers c  
LEFT JOIN rental r ON c.customer_id = r.customer_id  
GROUP BY c.customer_id, c.first_name, c.last_name;
10. Find Average Rental Rate of Films
SELECT AVG(rental_rate) AS avg_rental_rate FROM film;
11. Get the Total Amount Collected by Each Staff Member
SELECT staff_id, SUM(amount) AS total_collected  
FROM payment  
GROUP BY staff_id;
12. Find the Most Expensive Movie Rental
SELECT title, rental_rate FROM film  
ORDER BY rental_rate DESC LIMIT 1;
13. Count the Number of Movies Available in Each Store
SELECT store_id, COUNT(inventory_id) AS total_movies  
FROM inventory  
GROUP BY store_id;
14. Find Customers Who Have Made the Most Payments
SELECT c.first_name, c.last_name, COUNT(p.payment_id) AS total_payments  
FROM customers c  
JOIN payment p ON c.customer_id = p.customer_id  
GROUP BY c.customer_id, c.first_name, c.last_name  
ORDER BY total_payments DESC  
LIMIT 1;
15. Find the Highest Payment Amount
SELECT MAX(amount) AS highest_payment FROM payment;
16. Find Customers Who Have Spent More Than the Average Payment Amount
SELECT c.first_name, c.last_name, SUM(p.amount) AS total_spent  
FROM customers c  
JOIN payment p ON c.customer_id = p.customer_id  
GROUP BY c.customer_id, c.first_name, c.last_name  
HAVING SUM(p.amount) > (SELECT AVG(amount) FROM payment);
17. Get the Total Revenue from Rentals in the Last 30 Days
SELECT SUM(amount) AS revenue_last_30_days  
FROM payment  
WHERE payment_date >= DATEADD(DAY, -30, GETDATE());
18. Find the Oldest Movie in the Database
SELECT title, release_year  
FROM film  
ORDER BY release_year ASC  
LIMIT 1;
19. Find Movies That Are Available in More Than One Store
SELECT f.title, COUNT(DISTINCT i.store_id) AS store_count  
FROM film f  
JOIN inventory i ON f.film_id = i.film_id  
GROUP BY f.title  
HAVING COUNT(DISTINCT i.store_id) > 1;
20. Find the Staff Member Who Processed the Most Payments
SELECT staff_id, COUNT(payment_id) AS total_payments  
FROM payment  
GROUP BY staff_id  
ORDER BY total_payments DESC  
LIMIT 1;
21. List the Top 5 Customers Who Have Spent the Most
SELECT c.first_name, c.last_name, SUM(p.amount) AS total_spent  
FROM customers c  
JOIN payment p ON c.customer_id = p.customer_id  
GROUP BY c.customer_id, c.first_name, c.last_name  
ORDER BY total_spent DESC  
LIMIT 5;
22. Find Movies That Have Special Features
SELECT title, special_features  
FROM film  
WHERE special_features IS NOT NULL;
23. Find Customers Who Have Rented the Most in the Last 60 Days
SELECT c.first_name, c.last_name, COUNT(r.rental_id) AS rental_count  
FROM customers c  
JOIN rental r ON c.customer_id = r.customer_id  
WHERE r.rental_date >= DATEADD(DAY, -60, GETDATE())  
GROUP BY c.customer_id, c.first_name, c.last_name  
ORDER BY rental_count DESC  
LIMIT 1;
24. Find the Movie That Generates the Most Revenue
SELECT f.title, SUM(p.amount) AS total_revenue  
FROM film f  
JOIN inventory i ON f.film_id = i.film_id  
JOIN rental r ON i.inventory_id = r.inventory_id  
JOIN payment p ON r.rental_id = p.rental_id  
GROUP BY f.title  
ORDER BY total_revenue DESC  
LIMIT 1;
