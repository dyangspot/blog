title: "Extractor @ use in scala"
date: 2015-12-10 10:03:33
tags: scala
---

Use @ operator to bind the value that is matched to a varibale is very power in Scala. e.g.
Define the User model: 

```
trait User {
  def name: String
  def score: Int
}
class FreeUser(val name: String, val score: Int, val upgradeProbability: Double)
  extends User
class PremiumUser(val name: String, val score: Int) extends User

object FreeUser {
  def unapply(user: FreeUser): Option[(String, Int, Double)] =
    Some((user.name, user.score, user.upgradeProbability))
}
object PremiumUser {
  def unapply(user: PremiumUser): Option[(String, Int)] = Some((user.name, user.score))
}

```

Define a boolean extractor: 
Note: 'premiumCandidate' the first letter use the lower case here 

```
object premiumCandidate {
  def unapply(user: FreeUser): Boolean = user.upgradeProbability > 0.75
}
```

Now, we can apply the @ operator at the premiumCandiate to do the pattern match: 

```
val user: User = new FreeUser("Daniel", 2500, 0.8d)
user match {
  case user @ premiumCandidate() =>  "cool, you gonna upgrade now: " + user.name
  case _ => sendRegularNewsletter(user)
}

```

Finally, we can extract the users with their upgradeProbability value >.75 to do the next job. 

