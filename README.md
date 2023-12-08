# RestaurantReviewAPPfinal
A website that allows users to review restaurants they've eaten at, give ratings for the menu or specific food items, location, the price levels etc.

These were the scripts to create the mysql server:

show databases;
create database project1CS348;
use project1cs348;
show tables;
-- Restaurant Table
CREATE TABLE Restaurant (
    restaurantId INT AUTO_INCREMENT PRIMARY KEY,
    restaurantName VARCHAR(255) NOT NULL,
    streetAddress VARCHAR(255),
    zipCode VARCHAR(10),
    Email VARCHAR(255) UNIQUE NOT NULL,
    phoneNumber VARCHAR(15),
    website VARCHAR(255),
    cuisineType VARCHAR(255)
);

-- User Table
CREATE TABLE User (
    username VARCHAR(255) PRIMARY KEY,
    firstName VARCHAR(255) NOT NULL,
    lastName VARCHAR(255) NOT NULL,
    Email VARCHAR(255) UNIQUE NOT NULL,
    Password VARCHAR(255) NOT NULL
);

-- Review Table
CREATE TABLE Review (
    reviewId INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(255),
    restaurantId INT,
    rating INT NOT NULL CHECK (rating BETWEEN 1 AND 5),
    textReview TEXT,
    dateTime TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (username) REFERENCES User(username),
    FOREIGN KEY (restaurantId) REFERENCES Restaurant(restaurantId)
);


TO CREATE THE INDEXES:

Index 1:
create index zipcode_cusine_index  on restaurant(zipCode,cuisineType) using BTREE;
Index 2:
create index review_username_index  on review(username) using BTREE;



TO CREATE THE STORED PROCEDURES

Stored Procedure 1:

DELIMITER $$

DROP PROCEDURE IF EXISTS `delete_restaurant`$$


CREATE PROCEDURE delete_restaurant(IN name varchar(255))
BEGIN
delete from review where restaurantId IN (select restaurantId from restaurant where restaurantName=name);
    delete from restaurant  where restaurantName=name;

END $$
DELIMITER ;

Stored Procedure 2:

DELIMITER $$

DROP PROCEDURE IF EXISTS `delete_user`$$


CREATE PROCEDURE delete_user(IN name varchar(255))
BEGIN
delete from review where username = name;
    	delete from user  where username=name;

END $$
DELIMITER ;


