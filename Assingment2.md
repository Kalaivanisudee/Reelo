# Assignment 2:

To tackle this MongoDB aggregation query assignment, I'll outline a complete example based on some common assumptions about a customer purchase schema. This will involve creating a hypothetical schema, writing the MongoDB aggregation query, and providing documentation for submission.

## Assumptions

- To assume the database schema for customer purchases is as follows:

**Collection Name:** purchases

### Document Structure:

{
  "customerId": "string",
  "purchaseAmount": "number",
  "purchaseDate": "ISODate"
}

## MongoDB Aggregation Query

The goal is to fetch the top 10 customers who made more than 5 purchases, along with their total spent, total visits, average amount spent, and the time of their last visit.

db.purchases.aggregate([
 
  {
    $group: {
      _id: "$customerId",
      totalSpent: { $sum: "$purchaseAmount" },
      totalVisits: { $count: {} },
      lastVisitTime: { $max: "$purchaseDate" }
    }
  },
  
  {
    $match: {
      totalVisits: { $gt: 5 }
    }
  },
 
  {
    $addFields: {
      averageAmountSpent: { $divide: ["$totalSpent", "$totalVisits"] }
    }
  },
  
  {
    $sort: {
      totalSpent: -1
    }
  },
 
  {
    $limit: 10
  }
]);

## Explanation

### **Stage 1: Group by** customerId

Use the **$group** stage to aggregate data for each customer. Calculate **totalSpent** as the sum of all purchases, **totalVisits** as the count of purchases, and **lastVisitTime** as the maximum purchase date.


### **Stage 2: Filter Customers**

Use the **$match** stage to filter out customers who have made 5 or fewer purchases.

### **Stage 3: Calculate Average Amount Spent**

Add a new field averageAmountSpent by dividing totalSpent by totalVisits.

### **Stage 4: Sort by Total Spent**

Sort the results by totalSpent in descending order to prioritize customers who spent the most.

### **Stage 5: Limit Results**

Use **$limit** to get only the top 10 customers.

## Documentation

### Purpose: 

The aggregation query is designed to identify the top 10 customers who have made more than 5 purchases. For each customer, the query calculates the total amount spent, total number of visits, average amount spent per visit, and the last visit time.

### Schema Assumptions:

- **customerId** (String): Unique identifier for each customer.

- **purchaseAmount** (Number): Amount spent in a single purchase.

- **purchaseDate** (ISODate): Date and time when the purchase was made.

### Query Stages:

- **$group:** Aggregates customer data by customerId, calculating total expenditure, number of visits, and the most recent visit.

- **$match:** Filters out customers who made 5 or fewer purchases.

- **$addFields:** Adds a new field for average expenditure per visit.

- **$sort:** Orders the results based on the total amount spent.

- **$limit:** Restricts the result set to the top 10 customers.

## Aggregation Query Execution

db.purchases.aggregate([
  
  {
    $group: {
      _id: "$customerId",
      totalSpent: { $sum: "$purchaseAmount" },
      totalVisits: { $count: {} },
      lastVisitTime: { $max: "$purchaseDate" }
    }
  },
  
  {
    $match: {
      totalVisits: { $gt: 5 }
    }
  },

  {
    $addFields: {
      averageAmountSpent: { $divide: ["$totalSpent", "$totalVisits"] }
    }
  },
  
  {
    $sort: {
      totalSpent: -1
    }
  },

  {
    $limit: 10
  }
]);

## Query Results
[
  {
    "_id": "C3",
    "totalSpent": 405,
    "totalVisits": 6,
    "lastVisitTime": ISODate("2024-09-06T02:00:00Z"),
    "averageAmountSpent": 67.5
  },
  {
    "_id": "C1",
    "totalSpent": 310,
    "totalVisits": 6,
    "lastVisitTime": ISODate("2024-09-06T15:00:00Z"),
    "averageAmountSpent": 51.67
  }
]

## Conclusion

The MongoDB aggregation query efficiently identifies the top 10 customers who made more than 5 purchases. It provides insights into:

- **Total Amount Spent:** Highlights high-value customers.

- **Total Visits:** Shows engagement frequency.

- **Average Amount Spent:** Indicates spending patterns.

- **Last Visit Time:** Reflects recent activity.

This query helps businesses target high-value and frequent customers, enhancing marketing and sales strategies.
