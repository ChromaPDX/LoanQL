# LoanQL
Loan Query Language

LoanQL is an [unstructured query language](https://en.wikipedia.org/wiki/NoSQL) and a superset of JSON that attempts to help with the creation of smart contracts between buyers and sellers of risk capital.

# Inspiration


* MongoDB:

	LoanQL has logical operators inspired by [MongoDB Query Language](https://docs.mongodb.com/manual/reference/operator/query/)

* CSS:

  LoanQL has a number of [selectors](https://en.wikipedia.org/wiki/Cascading_Style_Sheets#Selector) that perform control flow instead of using imperative 'if/then' statements. 

  Selectors are defined globally.

  Logical branches are chosen hierarchically according to the number of selectors matched by the comparable document.

# Example
### Query

syndicate specified
```
maxInterest: 24
minInterest: 6
```
```coffeescript
amount: 5000
principle: $gte: 10000000
rate:
	weight: [
      [70,
      value: $logMap: [
      600, 750,
      "{syndicate.maxInterest}", "{syndicate.minInterest}",
      "{organization.fico}"
      ]
    ,
      [30,
      value: $logMap: [
      1.6, 1.1,
      "#{syndicate.maxInterest}", "#{syndicate.minInterest}",
      "{organization.debtToServiceCreditRatio}"
      ]
    ]
term:
	$and: [
      $gt: $logMap: [
          "{syndicate.maxInterest}", "{syndicate.minInterest}",
          12, 60,
          "{rate}"
      ]
    ,
      $lt: 72
    ]
sort: [
	"{organization.employees.totalMinority}": -1
    $linearDistance: [[45.5083, 122.6660],"{organization.location}"]: -1
]
```

### Match

```coffeescript
principle: $gte: 12000000
rate: $lte: 12
term: $gte: 60
organization:
	fico: 700
    debtToServiceCreditRatio: 1.2
    location: [45.5262325, 122.6769999]
```

### Resulting Agreement
```
amount: 5000
rate: 12
term: 60
```
