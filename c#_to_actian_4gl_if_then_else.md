# C# to Actian 4GL for OpenROAD: IF-THEN-ELSE Statement FOR OpenROAD

## C# Example

In this example, we have variables representing a person's age, membership status, total purchase amount, and 
eligibility for a discount. The values of these variables will then be used to determine a person's eligibility for a 
discount.

```csharp
int age = 65;
bool isPremiumMember = false;
double totalPurchase = 120.50;

// Determine membership status
string membershipStatus = isPremiumMember ? "Premium" : "Regular";

// Determine eligibility for discount using a complex conditional expression
bool isEligibleForDiscount = 
   isPremiumMember ? true :
   (membershipStatus == "Regular" && (age > 60 || totalPurchase > 100));

// Output the result
Console.WriteLine($"Age: {age}");
Console.WriteLine($"Membership Status: {membershipStatus}");
Console.WriteLine($"Total Purchase: ${totalPurchase}");
Console.WriteLine($"Eligible for Discount: {isEligibleForDiscount}");
```

### Variable Initialization

- Age: The variable `age` is initialized to 65. This represents the age of the person for whom we are determining the 
discount eligibility.

- Membership Status:
  - The variable `isPremiumMember` is a boolean initialized to false. This indicates that the person is not a premium 
  member.
  - The variable `totalPurchase` is initialized to 120.50. This represents the total amount the person has spent.

### Determine Membership Status

Conditional Expression for Membership Status:
- We use a conditional (ternary) operator to determine the membership status. The condition checks the value of 
`isPremiumMember`.
- If `isPremiumMember` is true, the membership status is set to "Premium".
- If `isPremiumMember` is false, the membership status is set to "Regular".
- In this case, since `isPremiumMember` is false, the membership status will be "Regular".

### Determine Eligibility for Discount

Complex Conditional Expression for Discount Eligibility:
- We use another conditional expression to determine if the person is eligible for a discount.
- The first part of the expression checks if the person is a premium member (`isPremiumMember`). If this is true, the 
person is automatically eligible for the discount, and `isEligibleForDiscount` is set to true.
- If the person is not a premium member, the expression further checks two conditions:
  - If the membership status is "Regular".
  - If either the age is greater than 60 or the total purchase amount is greater than $100.
- The logical AND (&&) operator ensures that the membership status must be "Regular" and either of the other conditions 
(age > 60 or total purchase > $100) must be true for the person to be eligible for the discount.
- In this case `isPremiumMember` is false, `membershipStatus` is "Regular", `age` is 65, which is greater than 60.
- Since the age condition is met, `isEligibleForDiscount` is set to true.

### Output the Result

Output Statements:
- We use `Console.WriteLine` to print the values of `age`, `membershipStatus`, `totalPurchase`, and 
`isEligibleForDiscount`.
- The output will display the age of the person (65), the membership status ("Regular"), the total purchase amount 
($120.50), whether the person is eligible for a discount (true).

## Actian 4GL Equivalent

```sql
DEFINE age INTEGER
DEFINE isPremiumMember BOOLEAN
DEFINE totalPurchase DECIMAL(10,2)
DEFINE membershipStatus CHAR(10)
DEFINE isEligibleForDiscount BOOLEAN

LET age = 65
LET isPremiumMember = FALSE
LET totalPurchase = 120.50

-- Determine membership status
IF isPremiumMember THEN
    LET membershipStatus = "Premium"
ELSE
    LET membershipStatus = "Regular"
END IF

-- Determine eligibility for discount using a complex conditional expression
IF isPremiumMember THEN
    LET isEligibleForDiscount = TRUE
ELSE
    IF membershipStatus = "Regular" AND (age > 60 OR totalPurchase > 100) THEN
        LET isEligibleForDiscount = TRUE
    ELSE
        LET isEligibleForDiscount = FALSE
    END IF
END IF

-- Output the result
DISPLAY "Age: ", age
DISPLAY "Membership Status: ", membershipStatus
DISPLAY "Total Purchase: $", totalPurchase
DISPLAY "Eligible for Discount: ", isEligibleForDiscount
```

This Actian 4GL code snippet determines a person's eligibility for a discount based on their age, membership status, 
and total purchase amount. It follows a structured approach to define variables, perform conditional logic, and output 
the results.

### Define Variables:
- `age`: An integer variable to store the person's age.
- `isPremiumMember`: A boolean variable to indicate whether the person is a premium member.
- `totalPurchase`: A decimal variable to store the total amount of the person's purchase, with up to 10 digits and 2 
decimal places.
- `membershipStatus`: A character variable to store the membership status ("Premium" or "Regular").
- `isEligibleForDiscount`: A boolean variable to indicate if the person is eligible for a discount.

### Initialize Variables:
- `age` is set to 65, representing the person's age.
- `isPremiumMember` is set to FALSE, indicating the person is not a premium member.
- `totalPurchase` is set to 120.50, representing the total purchase amount in dollars.

### Conditional Expression for Membership Status:
- The IF statement checks if `isPremiumMember` is TRUE.
- If TRUE, `membershipStatus` is set to "Premium".
- If FALSE, `membershipStatus` is set to "Regular".
- In this case, since `isPremiumMember` is FALSE, `membershipStatus` will be "Regular".

### Complex Conditional Expression for Discount Eligibility:
- The outer IF statement checks if `isPremiumMember` is TRUE.
- If TRUE, `isEligibleForDiscount` is set to TRUE (premium members are always eligible for a discount).
- If `isPremiumMember` is FALSE, it checks further conditions within the nested IF statement:
  - It verifies if `membershipStatus` is "Regular" and either `age` is greater than 60 or `totalPurchase` is greater 
  than 100.
  - If both conditions are met, `isEligibleForDiscount` is set to TRUE.
  - If the conditions are not met, `isEligibleForDiscount` is set to FALSE.
- In this example, `isPremiumMember` is FALSE, `membershipStatus` is "Regular", `age` is 65, which is greater than 60, 
therefore, `isEligibleForDiscount` is set to TRUE.

### Display Results:
- The DISPLAY statements output the values of `age`, `membershipStatus`, `totalPurchase`, and `isEligibleForDiscount` 
to the console or user interface.
- The output will show:
  - The person's age (65).
  - The membership status ("Regular").
  - The total purchase amount ($120.50).
  - The discount eligibility status (TRUE).

The code first defines and initializes the necessary variables. It determines the membership status based on whether 
the person is a premium member. It uses nested conditional expressions to decide if the person is eligible for a 
discount. Finally, it outputs the relevant information to the user.
