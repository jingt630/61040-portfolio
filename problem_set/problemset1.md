# Problem Set 1

## Exercise 1

Each user has only one registry, one to one, is unique.

1. The first invariant is the number of items a purchases can buy is at most of the requested item count.

The second invariant is the purchaser can only buy existing items in existing registry. Purchase only works if the given item, if existing, is in the given registry, if also existing.

The more important one is the second invariant because it has to do with the purpose/concept of this whole design. It's mean for gifting the right thing to the right person. Allowing the purchaser to buy more than the requested item count isn't that bad, since it meant extra copies of gift for the user. However, buying a gift for someone else is money lost to the purchaser, and also defeating the purpose of this application. The action most affected by the second invariant would be the purchase feature. The purchaser can be buying none existing thing, buying for the wrong user, because each user can only has one registry, and as well as. 
