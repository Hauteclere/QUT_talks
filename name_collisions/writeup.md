# Risk of Name+Birthday Collisions

This risk refers to the risk of false-positive matches when identifying people based on their given name, family name and their birthday. 

> I.e., how likely is it that there's some other Oliver Lavers out there who shares my birthday, and whose identity could be incorrectly merged with mine if that's the only evidence we use to determine identity?

There are a few ways to measure this risk. Each of them will give slightly different answers because each is based on slightly different assumptions. We'll look at one scenario which is quite generalised, and hopefully helps us build a reliable intuition around the risks.

First, let's look at the data individually...

## Birthday Distribution

![A scatter plot depicting the count of records associated with each birthday in our data.](./img/birthday_scatter.png) 

> Fig 1.1: Number of records by birthday. Each dot here represents a birthday on the x-axis, and the number of records which list that birthday on the y-axis. 

This isn't too surprising - the people in our records have a relatively concentrated spread of birthdays, with a peak around 1990. It reflects the national age distribution relatively closely, with a sharp cut-off on birthdays more recent than ~2006, since those people aren't university-age yet.

## Given Name Distribution

![A scatter plot depicting the count of the most common 1000 given names in our data, ordered from least common to most common.](./img/top_1000_given_names.png) 

> Fig 2.1: The count of the most common 1000 given names in our data, ordered from least common to most common.

This also isn't surprising - most names aren't very popular, but there are a few absolute standouts. Note the pattern of gaps at the high end of this curve - each international culture has a characteristic "fingerprint" of name popularity in the most common names - QUT's is a blend of several cultures!

```diff
- insert a subgraph here that calls out what those most popular names are!
```

## Family Name Distribution

![A scatter plot depicting the count of the most common 1000 family names in our data, ordered from least common to most common.](./img/top_1000_family_names.png)

> Fig 3.1: The count of the most common 1000 family names in our data, ordered from least common to most common.

Also nothing groundbreaking here, but note! The count of the most common given/family names is extremely high! What does this bode for full names?

## Full Name Distribution

![A scatter plot depicting the count of the most common 1000 full names in our data, ordered from least common to most common.](./img/top_1000_full_names_by_discrete_birthday.png)

> Fig 3.1: The count of the most common 1000 full names in our data, ordered from least common to most common. For the purposes of this graph, we have made the (conservative!) assumption that any pair of records with the same name and birthday relate to the same person, and have counted them as a single record.

Right, as we suspected, there are some full names that are extremely common (shared by over 100 individuals!), but the curve is significantly shallower than what we saw for given name or family name alone.

## HOLD UP

Wait a second... That last graph made the assumption that two records with the same name and birthday were duplicates of one another. 

> **Question**: Wasn't the whole point of this walkthrough to demonstrate that that is a dangerous assumption?
> 
> **Answer**: Yes. Thank you for paying attention.

We are going to perform a statistical trick. We are now going to try and **disprove** that assumption with statistics.

### Some Maths

Here's how this works. 

- There are a finite number of birthdays available in the last ~100 years.
- Birthdays are pretty much randomly assigned.

That means that we can pick the top 10 names in our data, look at the number of people with each name, and then calculate the odds that each of them randomly happened to have a different birthday. 

They might **not** have all had different birthdays. There might be two people in our data who share the same name and the same birthday - a collision. That would be hard to prove. But if we can prove that it's **unlikely** that we ended up with a scenario where there are no collisions, then that's proof that it's quite **likely** we have at least one collision!

### Ok Let's Do It

Here are our top ten names, and their counts:

| Name           | Count |
| -------------- | ----- |
| manpreet kaur  | 108   |
| amandeep kaur  | 83    |
| harpreet singh | 77    |
| harpreet kaur  | 75    |
| gurpreet singh | 74    |
| sandeep kaur   | 70    |
| manpreet singh | 69    |
| ramandeep kaur | 61    |
| mandeep kaur   | 57    |
| jaspreet kaur  | 55    |

> Sharp-eyed members of the audience may be looking at this list and drawing some demographic conclusions. But wait! This is definitely good evidence that we have a lot of students of South Asian descent, but remember what we said earlier about cultural fingerprints! 
> 
> Some of you may be aware that Singh and Kaur are Sikh last names (Singh for men, and Kaur for women). These names take the place of western-style "family" names, but are near-universal. Almost all Sikh men carry the last name Singh, and almost all Sikh women carry the last name Kaur. No wonder we see more commonality in these names!  

There are 32850 possible birthdays between 01/01/1920 and 31/12/2000. Let's calculate the odds that none of these people share a birthday!

