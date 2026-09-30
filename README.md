# Airbnb-Datamart-SQL-Project-


## Airbnb Data Mart — Build a Data Mart in SQL (DLBDSPBDM01)

A normalized relational data mart modelling the Airbnb use
case hosts, guests, listings, bookings, payments, reviews,
messaging, and more built as the portfolio project for IU
Internationale Hochschule's Data Mart SQL course.




### Table of contents

- **[Project Overview](#project-overview)**
- **[Tools](#tools)**
- **[Conception Phase](#conception-phase)**
- **[Entity Relationship Model](#entity-relationship-model)**
- **[Development Phase](#development-phase)**
- **[Test Cases](#test-cases)**
- **[Finalization](#finalization)**
- **[Entity Relationship Diagram](#entity-relationship-diagram)**
- **[Academic Note](#academic-note)**





### Project Overview
Airbnb connects hosts who have space to rent with guests who need short-term accommodation, handling search, booking, payment, and post-stay feedback on a single platform. This project translates that business process into a relational schema: who hosts and guests are, what was listed, how a booking moves from request to payment to review, and how hosts and guests communicate without duplicating data or losing the ability to query it efficiently.


| Fields | Description |
| :--- | :---- |
| Course | DLBDSPBDM01 — Build a Data Mart in SQL |
| Use case | Airbnb (hosts renting listings to guests) |
| DBMS | MySQL 8.0 (SQL Server variant also included) |
| Entities  |  24   |
| Triple (ternary) relationships | Payment , Review , Message |
| Recursive relationships | UserAccount.ReferredByUserID |
| Total dummy data rows | 751 (every table ≥ 20 rows)   |



### Tools
- Database: MySQL 8.0 (SQL Server 2019+ compatible version included)
- Diagramming: MySQL Workbench (EER reverse engineering), Graphviz (design-phase diagrams)
- Documentation: Markdown / Word (.docx) / PDF
- Presentation: PowerPoint (.pptx)







### Conception Phase

- Guest: searches listings, books a stay, pays through the platform, messages the host, leaves and receives reviews, saves listings to a wishlist, applies promo codes.
- Host: lists a property with photos, description, amenities and pricing, uses the income calculator, receives bookings and payouts, messages guests, leaves and receives reviews.

### Entity Relationship Model

Cardinalities are expressed in Crow's Foot notation: 1:1 ( UserAccount → GuestProfile / HostProfile ), 1:N ( HostProfile → Property ), M:N resolved through junction tables ( PropertyAmenity , BookingPromo , WishlistItem ), and one recursive 1:N relationship ( UserAccount.ReferredByUserID ).
Triple relationships (join over three tables) the part of the brief that goes beyond a standard binary ER model:
- Payment: connects Booking , the paying GuestProfile , and the receiving single record. 
- Review: connects Booking , ReviewerUserID , and RevieweeUserID , so both guest → host and host → guest feedback attach to the same stay.
- Message: connects SenderUserID , and the ReceiverUserID , PropertyID the conversation concerns.

| Area | Entities |
| :--- | :--- |
| Identity  |  UserAccount , Identity GuestProfile , HostProfile , SocialNetworkLink    |
| Property | PropertyType , Address , CancellationPolicy , Property Booking & Money Amenity , Property , PropertyPhoto , PropertyAmenity  |
| Booking & Money | PromoCode , Booking , Payment , BookingPromo , Commission , Currency , PayoutMethod     |
| Engagement  |  Review , Message , Engagement Wishlist , WishlistItem , IncomeEstimate , Notification    |

### Development Phase
Mysql code statement and screenshots of the table execution was documented using power point. All codes statement and executions output would be uploaded for overview.

### Test Cases
At least one test query was written and executed per relationship type required by the brief, the three ternary relationships, the recursive relationship, a many-to-many junction, and a business relevant aggregate.


```

SELECT b.BookingID, u1.FirstName AS Guest, u2.FirstName AS Host, p.Amount, p.PaymentStatus
FROM Payment p JOIN Booking b ON p.BookingID = b.BookingID
JOIN GuestProfile g ON p.GuestID = g.GuestID
JOIN UserAccount u1 ON g.UserID = u1.UserID
JOIN HostProfile h ON p.HostID = h.HostID
JOIN UserAccount u2 ON h.UserID = u2.UserID
WHERE p.PaymentStatus = 'Paid';

```

### Finalization

| Metric | Value |
| :--- | :--- |
| Total tables | 24  |
| Total rows | 751  |
| Minimum rows per table | 20 (met on every table)   |
| Triple relationships    |   3  |
| Recursive relationships    |  1   |
| M:N junction tables       |  3  |


### Entity Relationship Diagram

The diagram in this repo was generated from the schema directly. To regenerate it yourself from a live database (recommended before final submission, since it proves the schema was actually built): 
- Run the SQL script in MySQL Workbench.
- Database → Reverse Engineer
- Workbench auto-detects every foreign key including the three ternary tables and the recursive self-FK on UserAccount and lays out the EER diagram.
- File → Export → Export as PNG to save it for documentation.

### Academic Note

This repository documents coursework submitted for DLBDSPBDM01 at IU Internationale Hochschule. Dummy data is synthetic and generated programmatically for schema testing purposes only. it does not represent real Airbnb users, listings, or transactions.









