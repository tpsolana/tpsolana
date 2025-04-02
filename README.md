# Projects

## Investment Portfolio Tracker (July 2024)
- Utilized new front end techniques to design an engaging way to display the assets in a treasury.
- Utilized Coin Gecko's free API to fetch current asset prices.
- Displayed a pie graph to visually show the weight of each asset.

## Basic Ecommerce Site (September 2024)
- Designed a database schema for an online retailer and used an ERD to visualize this schema.
  ![image](https://github.com/user-attachments/assets/9a50e7d5-f9ff-41a1-84bf-885486458542)
- Created a SQL database and connected it to a basic website.
  <details>
  <summary>Canada_Database_Full.sql</summary>
  <br>
  (CREATE DATABASE `canada_db`;
    USE `canada_db`;

  CREATE TABLE `User` (
                        `UserID` int NOT NULL,
                        `Username` varchar(255) NOT NULL,
                        `Email` varchar(255) NOT NULL,
                        `Password` varchar(255) NOT NULL,
                        `Phone` char(10) NOT NULL,
                        `FirstName` varchar(255) NOT NULL,
                        `LastName` varchar(255) NOT NULL,
                        PRIMARY KEY (`UserID`),
                        UNIQUE KEY `Email_UNIQUE` (`Email`),
                        UNIQUE KEY `Username_UNIQUE` (`Username`)
  ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

    CREATE TABLE `Card` (
                        `CardID` int NOT NULL,
                        `Provider` varchar(255) NOT NULL,
                        `CardNo` varchar(23) NOT NULL,
                        `CVV` char(3) NOT NULL,
                        `ExpiryDate` date NOT NULL,
                        `FirstName` varchar(255) NOT NULL,
                        `LastName` varchar(255) NOT NULL,
                        PRIMARY KEY (`CardID`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

  CREATE TABLE `Address` (
                           `AddressID` int NOT NULL,
                           `Line1` varchar(255) NOT NULL,
                           `Line2` varchar(255) DEFAULT NULL,
                           `City` varchar(255) NOT NULL,
                           `State` varchar(255) NOT NULL,
                           `PostalCode` varchar(10) NOT NULL,
                           `SalesTaxRate` decimal(10,4) NOT NULL,
                           `Type` varchar(50) NOT NULL DEFAULT 'Shipping',
                           PRIMARY KEY (`AddressID`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

  CREATE TABLE `UserCard` (
                            `UserID` int NOT NULL,
                            `CardID` int NOT NULL,
                            PRIMARY KEY (`UserID`,`CardID`),
                            KEY `fk_UserCard_Card_idx` (`CardID`),
                            CONSTRAINT `fk_UserCard_Card` FOREIGN KEY (`CardID`) REFERENCES   `Card` (`CardID`) ON DELETE RESTRICT ON UPDATE RESTRICT,
                            CONSTRAINT `fk_UserCard_User` FOREIGN KEY (`UserID`) REFERENCES     `User` (`UserID`) ON DELETE RESTRICT ON UPDATE RESTRICT
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

  CREATE TABLE `UserAddress` (
                               `UserID` int NOT NULL,
                               `AddressID` int NOT NULL,
                               PRIMARY KEY (`UserID`,`AddressID`),
                               KEY `fk_UserAddress_Address_idx` (`AddressID`),
                               CONSTRAINT `fk_UserAddress_Address` FOREIGN KEY (`AddressID`)   REFERENCES `Address` (`AddressID`) ON DELETE RESTRICT ON UPDATE RESTRICT,
                               CONSTRAINT `fk_UserAddress_User` FOREIGN KEY (`UserID`) REFERENCES `User` (`UserID`) ON DELETE RESTRICT ON UPDATE RESTRICT
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

  CREATE TABLE `Item` (
                        `ItemID` int NOT NULL,
                        `Name` varchar(255) NOT NULL,
                        `Description` mediumtext NOT NULL,
                        `ImageURL` varchar(255) NOT NULL,
                        `Price` int NOT NULL,
                        `VariantOf` int DEFAULT NULL,
                        PRIMARY KEY (`ItemID`),
                        KEY `fk_Item_Item_idx` (`VariantOf`),
                        CONSTRAINT `fk_Item_Item` FOREIGN KEY (`VariantOf`) REFERENCES `Item` (`ItemID`) ON DELETE RESTRICT ON UPDATE RESTRICT
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

  CREATE TABLE `Category` (
                            `CategoryID` int NOT NULL,
                            `Name` varchar(255) NOT NULL,
                            `Description` mediumtext,
                            PRIMARY KEY (`CategoryID`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

    CREATE TABLE `Tag` (
                       `TagID` int NOT NULL,
                       `Name` varchar(255) NOT NULL,
                       PRIMARY KEY (`TagID`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

  CREATE TABLE `Discount` (
                            `DiscountID` int NOT NULL,
                            `Description` mediumtext,
                            `StartDate` datetime DEFAULT NULL,
                            `EndDate` datetime DEFAULT NULL,
                            `DiscountRate` decimal(6,3) NOT NULL,
                            PRIMARY KEY (`DiscountID`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

  CREATE TABLE `ItemCategory` (
                                `ItemID` int NOT NULL,
                                `CategoryID` int NOT NULL,
                                PRIMARY KEY (`ItemID`,`CategoryID`),
                                KEY `fk_ItemCategory_Category_idx` (`CategoryID`),
                                CONSTRAINT `fk_ItemCategory_Category` FOREIGN KEY (`CategoryID`) REFERENCES `Category` (`CategoryID`) ON DELETE RESTRICT ON UPDATE RESTRICT,
                                CONSTRAINT `fk_ItemCategory_Item` FOREIGN KEY (`ItemID`) REFERENCES `Item` (`ItemID`) ON DELETE RESTRICT ON UPDATE RESTRICT
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

  CREATE TABLE `ItemTag` (
                           `ItemID` int NOT NULL,
                           `TagID` int NOT NULL,
                           PRIMARY KEY (`ItemID`,`TagID`),
                           KEY `fk_ItemTag_Tag_idx` (`TagID`),
                           CONSTRAINT `fk_ItemTag_Item` FOREIGN KEY (`ItemID`) REFERENCES `Item` (`ItemID`) ON DELETE RESTRICT ON UPDATE RESTRICT,
                           CONSTRAINT `fk_ItemTag_Tag` FOREIGN KEY (`TagID`) REFERENCES `Tag` (`TagID`) ON DELETE RESTRICT ON UPDATE RESTRICT
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

  CREATE TABLE `ItemDiscount` (
                                `ItemID` int NOT NULL,
                                `DiscountID` int NOT NULL,
                                PRIMARY KEY (`ItemID`,`DiscountID`),
                                KEY `fk_ItemDiscount_Discount_idx` (`DiscountID`),
                                CONSTRAINT `fk_ItemDiscount_Discount` FOREIGN KEY (`DiscountID`) REFERENCES `Discount` (`DiscountID`) ON DELETE RESTRICT ON UPDATE RESTRICT,
                                CONSTRAINT `fk_ItemDiscount_Item` FOREIGN KEY (`ItemID`) REFERENCES `Item` (`ItemID`) ON DELETE RESTRICT ON UPDATE RESTRICT
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

  CREATE TABLE `List` (
                        `ListID` int NOT NULL,
                        `Name` varchar(255) NOT NULL,
                        `Description` mediumtext,
                        `IsPrivate` tinyint(1) DEFAULT '1',
                        PRIMARY KEY (`ListID`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

  CREATE TABLE `ListItem` (
                            `ListID` int NOT NULL,
                            `ItemID` int NOT NULL,
                            `Quantity` int NOT NULL DEFAULT '1',
                            PRIMARY KEY (`ListID`,`ItemID`),
                            KEY `fk_ListItem_Item_idx` (`ItemID`),
                            CONSTRAINT `fk_ListItem_Item` FOREIGN KEY (`ItemID`) REFERENCES `Item` (`ItemID`) ON DELETE RESTRICT ON UPDATE RESTRICT,
                            CONSTRAINT `fk_ListItem_List` FOREIGN KEY (`ListID`) REFERENCES `List` (`ListID`) ON DELETE RESTRICT ON UPDATE RESTRICT
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

  CREATE TABLE `UserList` (
                            `UserID` int NOT NULL,
                            `ListID` int NOT NULL,
                            PRIMARY KEY (`UserID`,`ListID`),
                            KEY `fk_UserList_List_idx` (`ListID`),
                            CONSTRAINT `fk_UserList_List` FOREIGN KEY (`ListID`) REFERENCES `List` (`ListID`) ON DELETE RESTRICT ON UPDATE RESTRICT,
                            CONSTRAINT `fk_UserList_User` FOREIGN KEY (`UserID`) REFERENCES `User` (`UserID`) ON DELETE RESTRICT ON UPDATE RESTRICT
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

  CREATE TABLE `Cart` (
                        `UserID` int NOT NULL,
                        `CreatedOn` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,
                        `ModifiedOn` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,
                        `AccessedOn` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,
                        PRIMARY KEY (`UserID`),
                        CONSTRAINT `fk_Cart_User` FOREIGN KEY (`UserID`) REFERENCES `User` (`UserID`) ON DELETE RESTRICT ON UPDATE RESTRICT
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

  CREATE TABLE `CartItem` (
                            `UserID` int NOT NULL,
                            `ItemID` int NOT NULL,
                            `Quantity` int NOT NULL DEFAULT '1',
                            PRIMARY KEY (`UserID`,`ItemID`),
                            KEY `fk_CartItem_Item_idx` (`ItemID`),
                            CONSTRAINT `fk_CartItem_Cart` FOREIGN KEY (`UserID`) REFERENCES `Cart` (`UserID`),
                            CONSTRAINT `fk_CartItem_Item` FOREIGN KEY (`ItemID`) REFERENCES `Item` (`ItemID`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

  CREATE TABLE `Order` (
                         `OrderID` int NOT NULL,
                         `UserID` int NOT NULL,
                         `Status` varchar(255) NOT NULL,
                         `PurchasedOn` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,
                         `BillingAddressID` int NOT NULL,
                         `ShippingAddressID` int NOT NULL,
                         `CardID` int NOT NULL,
                         `TotalPrice` decimal(20,2) NOT NULL,
                         PRIMARY KEY (`OrderID`),
                         KEY `fk_Order_User_idx` (`UserID`),
                         KEY `fk_Order_Address1_idx` (`BillingAddressID`),
                         KEY `fk_Order_Address2_idx` (`ShippingAddressID`),
                         KEY `fk_Order_Card_idx` (`CardID`),
                         CONSTRAINT `fk_Order_Address1` FOREIGN KEY (`BillingAddressID`) REFERENCES `Address` (`AddressID`) ON DELETE RESTRICT ON UPDATE RESTRICT,
                         CONSTRAINT `fk_Order_Address2` FOREIGN KEY (`ShippingAddressID`) REFERENCES `Address` (`AddressID`) ON DELETE RESTRICT ON UPDATE RESTRICT,
                         CONSTRAINT `fk_Order_Card` FOREIGN KEY (`CardID`) REFERENCES `Card` (`CardID`) ON DELETE RESTRICT ON UPDATE RESTRICT,
                         CONSTRAINT `fk_Order_User` FOREIGN KEY (`UserID`) REFERENCES `User` (`UserID`) ON DELETE RESTRICT ON UPDATE RESTRICT
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

  CREATE TABLE `OrderItem` (
                             `OrderID` int NOT NULL,
                             `ItemID` int NOT NULL,
                             `Quantity` int NOT NULL DEFAULT '1',
                             `Price` decimal(20,2) NOT NULL,
                             PRIMARY KEY (`OrderID`,`ItemID`),
                             KEY `fk_OrderItem_Item_idx` (`ItemID`),
                             CONSTRAINT `fk_OrderItem_Item` FOREIGN KEY (`ItemID`) REFERENCES `Item` (`ItemID`),
                             CONSTRAINT `fk_OrderItem_Order` FOREIGN KEY (`OrderID`) REFERENCES `Order` (`OrderID`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;)
</details>

- Allowed users to input a CSV files with cart, item, customer, list and order information.

- Users could then generate basic reports of either customer, item, or list information.
  <details>
    <summary>Customer Report Query</summary>
    <br>
     SELECT 
                        u.UserID, 
                        CONCAT(u.FirstName, ' ', u.LastName) AS FullName, 
                        u.Username, 
                        u.Email,
                        GROUP_CONCAT(DISTINCT l.Name ORDER BY l.Name SEPARATOR ', ') AS ListNames,
                        IF(c.UserID IS NOT NULL, 'Yes', 'No') AS CartExists,
                        GROUP_CONCAT(DISTINCT o.OrderID ORDER BY o.PurchasedOn SEPARATOR ', ') AS Orders
                    FROM 
                        canada_db.User u
                    LEFT JOIN 
                        canada_db.UserList ul ON u.UserID = ul.UserID
                    LEFT JOIN 
                        canada_db.List l ON ul.ListID = l.ListID
                    LEFT JOIN 
                        canada_db.Cart c ON u.UserID = c.UserID
                    LEFT JOIN 
                        canada_db.Order o ON u.UserID = o.UserID
                    GROUP BY 
                        u.UserID
                    ORDER BY 
                        u.UserID;
  </details>

## BONK! dApp (February 2025)
- Utilizing the Solana dApp Scaffold I created a basic website that allowed the user to overlay a GIF over their image and receive a new GIF.
- Then I hooked up the button to wait for a successful transaction through our website before outputting the gif for the user.
- I also created a button that allowed the user to have a pre-programmed tweet about their experience sent to twitter.

## Bowling Game (February 2025)
- Basic bowling game made with Java for my Agile Software Development class.
- Allows for 1-4 players to compete against each other, utilizing either manual or random throws.
- Scores each players frames and then determines and displays the winner.

## Senior Capstone (Present)
https://github.com/jslovell/hfh-capstone
- Creating a web app to allow Habit for Humanity's project managers to easily notate issues in elderly housing
- First I designed a simple login page the linked to account creation for new users.
  ![image](https://github.com/user-attachments/assets/927ffa13-fa68-4599-8afd-1c4ce18fbeb8)
- Next the user has two options, create a new assessment or use our catalog page to look up an existing assessment. I created the catalog page using HTML/CSS/SQL to allow users to search based on the name, address, or the assessment status.
  ![image](https://github.com/user-attachments/assets/54de53e0-7425-477d-9d5d-341341c27671)
- Finally the user is greeted with their uploaded floorplan as well as a toolbar with an array of icons. We designed this page to show three levels of priority for each icon and allow for any of these icons to be placed directly on the floorplan.

# Education

## Bradley University | Computer Science (May 2025)
- Database Management Systems
- Intro to Business Analytics
- Agile Software Development
- Artifical Intelligence
- Computer and Network Security
- Computer Architecture

# Technical Skills

- **SQL**: Completed a Database Management Systems class. This class's project was building a database system for an online retailer. We firstly designed the entities and their relationships before creating an ERD to display this. Afterwords, we created this database and implemented it on a skeleton front-end for testing purposes.
- **Microsoft Excel**: Completed Intro to Business Analytics which is focusing on teaching the basics of Excel. This knowledge was further utilized in my Artificial Intelligence building linear regression models.
- **HTML/CSS**: Worked on the front-end for my senior capstone project with Habitat for Humanity. I did various front-end tasks from redesigning the website to implementing a new nav-bar that would be adaptive to their employees iPads.
- **Java**: My Java experience comes from an Agile Programming course which taught heavily how to utilize Object Oriented Programming to perfection. I completed an array of projects in Java ranging from a simple bowling score calculator to a payroll system. While I am not very experienced I have a firm grasp on the fundamentals.
- **JavaScript**: I was not formerly taught JavaScript however I have had exposure with it in some of my projects. I helped implement placing icons on a floor plan with my senior capstone project.

**Developer Tools**: Git, VS Code, Visual Studio, PyCharm, IntelliJ, Eclipse, Anchor
