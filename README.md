# SQl_EX

Number_1

SELECT model, speed, hd 
FROM PC WHERE price < 500

----------------------------------

Number_2

SELECT maker FROM Product 
WHERE type = 'Printer' 
GROUP BY maker

----------------------------------

Number_3

SELECT model, ram, screen 
FROM laptop WHERE price > 1000

----------------------------------

Number_4

SELECT * FROM Printer 
WHERE color = 'y'

----------------------------------

Number_5

SELECT model, speed, hd FROM PC 
WHERE (cd = '12x' OR cd = '24x') 
AND price < 600

----------------------------------

Number_6

SELECT DISTINCT p.maker, l.speed
FROM laptop l
JOIN product p ON p.model = l.model
WHERE l.hd >= 10

----------------------------------

Number_7

SELECT DISTINCT product.model, pc.price 
FROM product JOIN pc ON product.model = pc.model WHERE maker = 'B'
UNION
SELECT DISTINCT product.model, printer.price 
FROM product JOIN printer ON product.model = printer.model WHERE maker = 'B'
UNION
SELECT DISTINCT product.model, laptop.price 
FROM product JOIN laptop ON product.model = laptop.model WHERE maker = 'B'

----------------------------------

Number_8

SELECT DISTINCT maker
FROM product
WHERE type = 'pc' EXCEPT
SELECT DISTINCT product.maker
FROM product
WHERE type = 'laptop'

----------------------------------

Number_9

SELECT DISTINCT product.maker FROM pc
INNER JOIN product ON pc.model = product.model
WHERE pc.speed >= 450

----------------------------------

Number_10

SELECT model, price FROM printer 
WHERE price = (SELECT MAX(price) FROM printer)

----------------------------------

Number_11

SELECT AVG(speed) AS Avg_speed FROM pc

----------------------------------

Number_12

SELECT AVG(speed) AS Avg_speed FROM laptop
WHERE price > 1000

----------------------------------

Number_13

SELECT AVG(pc.speed) AS Avg_speed
FROM pc, product
WHERE product.model = pc.model AND product.maker = 'A'

----------------------------------

Number_14

SELECT s.class, s.name, c.country FROM ships s
LEFT JOIN classes c ON s.class = c.class
WHERE c.numGuns >= 10

----------------------------------

Number_15

SELECT hd FROM pc GROUP BY (hd) 
HAVING COUNT(model) >= 2

----------------------------------

Number_16

SELECT DISTINCT p1.model, p2.model, p1.speed, p1.ram
FROM pc p1, pc p2
WHERE p1.speed = p2.speed AND 
p1.ram = p2.ram AND p1.model > p2.model

----------------------------------

Number_17

SELECT DISTINCT p.type, p.model, l.speed
FROM laptop l JOIN product p ON l.model=p.model
WHERE l.speed<(select MIN(speed) FROM pc)

----------------------------------

Number_18

SELECT DISTINCT product.maker, printer.price
FROM product, printer
WHERE product.model = printer.model AND printer.color = 'y'
AND printer.price = (SELECT MIN(price) FROM printer
WHERE printer.color = 'y')

----------------------------------

Number_19

SELECT product.maker, AVG(screen) FROM laptop
LEFT JOIN product ON product.model = laptop.model
GROUP BY product.maker

----------------------------------

Number_20

SELECT maker, COUNT(model) FROM product
WHERE type = 'pc' GROUP BY product.maker
HAVING COUNT (DISTINCT model) >= 3

----------------------------------

Number_21

SELECT product.maker, MAX(pc.price) FROM product, pc
WHERE product.model = pc.model GROUP BY product.maker

----------------------------------

Number_22

SELECT pc.speed, AVG(pc.price) FROM pc
WHERE pc.speed > 600 GROUP BY pc.speed

----------------------------------

Number_23

SELECT DISTINCT maker
FROM product t1 JOIN pc t2 ON t1.model=t2.model
WHERE speed>=750 AND maker IN (SELECT maker
FROM product t1 JOIN laptop t2 ON t1.model=t2.model WHERE speed>=750)

----------------------------------

Number_24



----------------------------------

Number_25

SELECT DISTINCT maker FROM product 
WHERE model IN (SELECT model FROM pc
WHERE ram = (SELECT MIN(ram) FROM pc)
AND speed = (SELECT MAX(speed) FROM pc
WHERE ram = (SELECT MIN(ram) FROM pc)))
AND maker IN (SELECT maker 
FROM product WHERE type = 'printer')

----------------------------------

Number_26

SELECT sum(s.price)/sum(s.kol) FROM 
(SELECT price,1 kol FROM pc,product
WHERE pc.model=product.model AND product.maker='A'
UNION all SELECT price,1 FROM laptop,product
WHERE laptop.model=product.model AND product.maker='A') as s

----------------------------------

Number_27

SELECT product.maker, AVG(pc.hd)
FROM pc, product WHERE product.model = pc.model
AND product.maker IN ( SELECT DISTINCT maker FROM product
WHERE product.type = 'printer') GROUP BY maker

----------------------------------

Number_28

SELECT COUNT(maker) FROM product
WHERE maker IN (SELECT maker FROM product
GROUP BY maker HAVING COUNT(model) = 1)

----------------------------------

Number_29

SELECT t1.point, t1.date, inc, out
FROM income_o t1 LEFT JOIN outcome_o t2 
ON t1.point = t2.point AND t1.date = t2.date UNION
SELECT t2.point, t2.date, inc, out
FROM income_o t1 RIGHT JOIN outcome_o t2 
ON t1.point = t2.point AND t1.date = t2.date

----------------------------------

Number_30

SELECT point, date, SUM(sum_out), SUM(sum_inc)
FROM (select point, date, SUM(inc) AS sum_inc, null AS sum_out 
FROM Income GROUP BY point, date UNION
SELECT point, date, null AS sum_inc, SUM(out) AS sum_out 
FROM Outcome GROUP BY point, date ) AS t
GROUP BY point, date ORDER BY point

----------------------------------

Number_31

SELECT DISTINCT class, country
FROM classes WHERE bore >= 16

----------------------------------

Number_32



----------------------------------

Number_33

SELECT o.ship FROM BATTLES b LEFT JOIN outcomes o 
ON o.battle = b.name
WHERE b.name = 'North Atlantic' AND o.result = 'sunk'

----------------------------------

Number_34

SELECT name FROM classes,ships 
WHERE launched >=1922 AND displacement>35000 
AND type='bb' AND ships.class = classes.class

----------------------------------

Number_35

SELECT model, type FROM product
WHERE UPPER(model) NOT like '%[^A-Z]%' 
OR model NOT LIKE '%[^0-9]%'

----------------------------------

Number_36

SELECT name FROM ships WHERE class = name UNION
SELECT ship AS name FROM classes, outcomes 
WHERE classes.class = outcomes.ship

----------------------------------

Number_37

SELECT c.class FROM classes c LEFT JOIN (
SELECT class, name FROM ships UNION
SELECT ship, ship FROM outcomes) AS s ON s.class = c.class
GROUP BY c.class HAVING COUNT(s.name) = 1

----------------------------------

Number_38

SELECT country FROM classes
GROUP BY country HAVING COUNT(DISTINCT type) = 2

----------------------------------

Number_39

WITH b_s AS (SELECT o.ship, b.name, b.date, o.result
FROM outcomes o LEFT JOIN battles b ON o.battle = b.name )
SELECT DISTINCT a.ship FROM b_s a
WHERE UPPER(a.ship) IN
(SELECT UPPER(ship) FROM b_s b
WHERE b.date < a.date AND b.result = 'damaged')

----------------------------------

Number_40

SELECT maker, type FROM product
WHERE maker in (SELECT t.maker
FROM (SELECT maker, type
FROM product GROUP BY maker, type) 
t GROUP BY t.maker HAVING count(t.maker) = 1)
GROUP BY maker, type HAVING count(*) > 1

----------------------------------

Number_41



----------------------------------

Number_42

SELECT ship, battle FROM Outcomes 
WHERE result = 'sunk'

----------------------------------

Number_43

SELECT name FROM battles
WHERE year(date) NOT IN (select launched
FROM ships WHERE launched IS NOT null)

----------------------------------

Number_44

SELECT name from Ships
WHERE name LIKE 'R%' UNION
SELECT Ship FROM Outcomes
WHERE Ship LIKE 'R%'

----------------------------------

Number_45

SELECT name FROM ships
WHERE name LIKE '% % %' UNION
SELECT ship FROM outcomes
WHERE ship LIKE '% % %'

----------------------------------

Number_46

SELECT o.ship, displacement, numGuns 
FROM (SELECT name AS ship, displacement, numGuns
FROM Ships s JOIN Classes c ON c.class=s.class UNION
SELECT class AS ship, displacement, numGuns
FROM Classes c) AS a RIGHT JOIN Outcomes o ON o.ship=a.ship
WHERE battle = 'Guadalcanal'

----------------------------------

Number_47



----------------------------------

Number_48

SELECT cl.class FROM Classes cl
LEFT JOIN Ships s ON s.class = cl.class
WHERE cl.class IN 
(SELECT ship FROM Outcomes WHERE result = 'sunk') OR
s.name IN (SELECT ship FROM Outcomes WHERE result = 'sunk')
GROUP BY cl.class

----------------------------------

Number_49

SELECT Ships.name FROM Classes JOIN
Ships ON Classes.class = ships.class
WHERE bore = 16 UNION
SELECT Outcomes.ship FROM Outcomes JOIN
Classes ON Classes.class = Outcomes.ship
WHERE bore = 16

----------------------------------

Number_50

SELECT DISTINCT battle FROM outcomes
WHERE ship IN(SELECT name FROM ships WHERE class = 'kongo')

----------------------------------

Number_51



----------------------------------

Number_52



----------------------------------

Number_53

SELECT cast (AVG (numguns*1.0) AS numeric(4,2)) 
AS Avg_Guns FROM classes WHERE type='bb'

----------------------------------

Number_54



----------------------------------

Number_55

SELECT C.class, MIN(launched) FROM ships S RIGHT JOIN classes 
AS C ON s.class=c.class GROUP BY C.class

----------------------------------

Number_56



----------------------------------

Number_62

SELECT (SELECT SUM(inc) FROM income_o 
WHERE '20010415' > date) - 
(SELECT SUM(out) FROM outcome_o 
WHERE '20010415' > date)  AS remainS

----------------------------------
