# Jaggaer Java Developer Homework Assignment
## Supplier Bid Evaluation System

### Overview

Build a command-line Java application that processes supplier bids for a procurement request and ranks them based on weighted evaluation criteria. This exercise is designed to assess your ability to write clean, well-structured Java code using object-oriented principles.

**Time Estimate:** ~2 hours

---

### Scenario

A company is evaluating bids from multiple suppliers for a procurement request. Each supplier can submit one of several pricing models:

| Bid Type | Description |
|----------|-------------|
| **Fixed Price** | A single total price for the entire order, regardless of quantity |
| **Unit Price** | A per-unit price multiplied by the requested quantity |
| **Tiered Price** | Price varies based on quantity—the tier matching the total quantity determines the unit price for all units |

The system must calculate effective prices, score each bid against weighted criteria, and rank the bids.

---

### Requirements

#### Functional Requirements

1. **Parse Input**: Read bid data from a JSON file (path provided as command-line argument)

2. **Calculate Effective Price**: Each bid type calculates price differently:
   - **Fixed Price**: Return the total price as-is
   - **Unit Price**: `unitPrice × quantity`
   - **Tiered Price**: Find the tier where quantity falls, then `tierUnitPrice × quantity`

3. **Score Bids**: Evaluate each bid using three weighted criteria:
   - **Price** (lower is better)
   - **Delivery Days** (fewer is better)
   - **Quality Rating** (higher is better)

4. **Rank & Display**: Sort bids by score (highest first) and output a formatted results table

#### Technical Requirements

- Minimum **Java 11** with **Maven** (we currently use Java 17)
- Must compile and run with Maven commands
- Include **unit tests** (JUnit)
- Use **Jackson** for JSON parsing

---

### Input Format

Your program will receive a JSON file path as a command-line argument. The JSON structure:

```json
{
  "requestedQuantity": 1000,
  "evaluationWeights": {
    "price": 0.5,
    "deliveryDays": 0.3,
    "qualityRating": 0.2
  },
  "bids": [
    {
      "supplierId": "SUP-001",
      "supplierName": "Acme Corp",
      "type": "FIXED_PRICE",
      "totalPrice": 5000.00,
      "deliveryDays": 14,
      "qualityRating": 4.2
    },
    {
      "supplierId": "SUP-002",
      "supplierName": "GlobalSupply Inc",
      "type": "UNIT_PRICE",
      "unitPrice": 4.75,
      "deliveryDays": 10,
      "qualityRating": 3.8
    },
    {
      "supplierId": "SUP-003",
      "supplierName": "ValueSource Ltd",
      "type": "TIERED_PRICE",
      "tiers": [
        { "minQuantity": 1, "maxQuantity": 500, "unitPrice": 6.00 },
        { "minQuantity": 501, "maxQuantity": 2000, "unitPrice": 4.50 }
      ],
      "deliveryDays": 7,
      "qualityRating": 4.5
    }
  ]
}
```

A sample file `bid-sample.json` is provided.

---

### Expected Output

Your program should output a ranked table similar to:

```
=== Bid Evaluation Results ===
Requested Quantity: 1000 units

Rank | Supplier           | Eff. Price    | Delivery    | Quality | Score
-----+--------------------+---------------+-------------+---------+-------
   1 | ValueSource Ltd    |     $4,500.00 |      7 days |     4.5 |  100.0
   2 | GlobalSupply Inc   |     $4,750.00 |     10 days |     3.8 |   87.9
   3 | Acme Corp          |     $5,000.00 |     14 days |     4.2 |   81.2
```

*(Exact scores may vary based on your scoring algorithm—document your approach)*

---

### Scoring Algorithm Guidance

You have flexibility in how you implement scoring. One common approach:

1. For each criterion, calculate how each bid compares to the **best value** among all bids
2. Normalize scores to a 0-100 scale
3. Apply the weights and sum

Example formulas:
- **Price Score**: `(bestPrice / bidPrice) × 100`
- **Delivery Score**: `(bestDeliveryDays / bidDeliveryDays) × 100`
- **Quality Score**: `(bidQuality / bestQuality) × 100`

**Final Score** = `(priceWeight × priceScore) + (deliveryWeight × deliveryScore) + (qualityWeight × qualityScore)`

---

### How to Run

```bash
# Compile
mvn clean compile

# Run tests (if applicable)
mvn test

# Execute the application
mvn exec:java -Dexec.args="bid-sample.json"
```

---

### Deliverables

Please submit:

1. **Complete Maven project** (source code, `pom.xml`, etc.)
2. **Unit tests** demonstrating your code works correctly
3. **README.md** containing:
   - Build and run instructions
   - Brief explanation of your design decisions
   - Any assumptions you made

---

### Evaluation Criteria

| Criteria | What We're Looking For |
|----------|------------------------|
| **Correctness** | Program compiles, runs, and produces correct results |
| **OOP Design** | Appropriate use of abstraction, inheritance, polymorphism, encapsulation |
| **Code Quality** | Readable, well-organized, follows Java conventions |
| **Testing** | Meaningful unit tests with reasonable coverage |
| **Error Handling** | Graceful handling of invalid input |

---

### Tips

- Focus on clean, readable code over clever solutions
- Use meaningful class and method names
- Keep methods focused on a single responsibility
- Don't over-engineer; solve the problem at hand
- Document any assumptions in your README
- AI tools are allowed, but we suggest leaning on your own approach as much as possible so you get the most out of the exercise

---

**Good luck! We look forward to reviewing your solution.**
