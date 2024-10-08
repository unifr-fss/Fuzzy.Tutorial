# Fuzzy Control Primer

Based on: https://scikit-fuzzy.github.io/scikit-fuzzy/userguide/fuzzy_control_primer.html

Overview and Terminology
-------------------------

**Fuzzy Logic** is a methodology predicated on the idea that the "truthiness"
of something can be expressed over a continuum.  This is to say that something
isn't *true* or *false* but instead *partially true* or *partially false*.

A **fuzzy variable** has a **crisp value** which takes on some number
over a pre-defined domain (in fuzzy logic terms, called a **universe**).
The crisp value is how we think of the variable using normal mathematics.
For example, if my fuzzy variable was how much to tip someone, its universe
would be 0 to 25% and it might take on a crisp value of 15%.

A fuzzy variable also has several **terms** that are used to describe the
variable.  The terms taken together are the **fuzzy set** which can be used to
describe the "fuzzy value" of a fuzzy variable.  These terms are usually
adjectives like "poor," "mediocre," and "good."  Each term has
a **membership function** that defines how a crisp value maps to the term on a
scale of 0 to 1.  In essence, it describes "how good" something is.

So, back to the tip example, a "good tip" might have a membership function
which has non-zero values between 15% and 25%, with 25% being a "completely
good tip" (ie, it's membership is 1.0) and 15% being a "barely good tip"
(ie, its membership is 0.1).

A **fuzzy control system** links fuzzy variables using a set of **rules**.
These rules are simply mappings that describe how one or more fuzzy variables
relates to another.  These are expressed in terms of an IF-THEN statement;
the IF part is called the **antecedent** and the THEN part is the
**consequent**.  In the tipping example, one rule might be "IF the service
was good THEN the tip will be good."  The exact math related to how
a rule is used to calculate the value of the consequent based on the value
of the antecedent is outside the scope of this primer.


The Tipping Problem
-------------------

Taking the tipping example full circle, if we were to create a controller which
estimates the tip we should give at a restaurant, we might structure it as such:

* Antecedents (Inputs)
   - `service`
      * Universe (ie, crisp value range): How good was the service of the waitress, on a scale of 1 to 10?
      * Fuzzy set (ie, fuzzy value range): poor, acceptable, amazing
   - `food quality`
      * Universe: How tasty was the food, on a scale of 1 to 10?
      * Fuzzy set: bad, decent, great
* Consequents (Outputs)
   - `tip`
      * Universe: How much should we tip, on a scale of 0% to 25%
      * Fuzzy set: low, medium, high
* Rules
   - IF the *service* was good  *or* the *food quality* was good, THEN the tip will be high.
   - IF the *service* was average, THEN the tip will be medium.
   - IF the *service* was poor *and* the *food quality* was poor THEN the tip will be low.
* Usage
   - If I tell this controller that I rated:
      * the service as 9.8, and
      * the quality as 6.5,
   - it would recommend I leave:
      * a 20.2% tip.

