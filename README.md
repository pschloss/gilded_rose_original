# Gilded Rose Refactoring Kata

As I understand it a "Kata" is an exercise that those learning martial arts will go through to hone their skills. A variety of coding Katas have been developed to practice different aspects of software engineering. While they are fun to go through on your own, doing it as a paired or "mob" programming exercise should be a lot of fun. After all, we all have different experiences, skills, needs, and interests.

You can find out more about this particular Kata in Emily Bache's YouTube video [Why Developers LOVE The Gilded Rose Kata](https://youtu.be/Mt4XpGxigT4). She has a [GitHub repository](https://github.com/emilybache/GildedRose-Refactoring-Kata) that includes the R code I have included here as well as many other translations into different programming languages. As [her README](https://github.com/emilybache/GildedRose-Refactoring-Kata/blob/main/README.md) describes

> This Kata was originally created by Terry Hughes (http://twitter.com/TerryHughes). It is already on GitHub [here](https://github.com/NotMyself/GildedRose). See also [Bobby Johnson's description of the Kata](https://iamnotmyself.com/refactor-this-the-gilded-rose-Kata/).  
> 
> I translated the original C# into a few other languages, (with a little help from my friends!), and slightly changed the starting position. This means I've actually done a small amount of refactoring already compared with the original form of the Kata, and made it easier to get going with writing tests by giving you one failing unit test to start with. I also added test fixtures for Text-Based approval testing with TextTest (see [the TextTests](https://github.com/emilybache/GildedRose-Refactoring-Kata/tree/main/texttests))  
> 
> As Bobby Johnson points out in his article ["Why Most Solutions to Gilded Rose Miss The Bigger Picture"](https://iamnotmyself.com/why-most-solutions-to-gilded-rose-miss-the-bigger-picture/), it'll actually give you better practice at handling a legacy code situation if you do this Kata in the original C#. However, I think this Kata is also really useful for practicing writing good tests using different frameworks and approaches, and the small changes I've made help with that. I think it's also interesting to compare what the refactored code and tests look like in different programming languages.


## Requirements Specification

Hi and welcome to team Gilded Rose. As you know, we are a small inn with a prime location in a prominent city ran by a friendly innkeeper named Allison. We also buy and sell only the finest goods.

Unfortunately, our goods are constantly degrading in quality as they approach their sell by date. We have a system in place that updates our inventory for us. It was developed by a no-nonsense type named Leeroy, who has moved on to new adventures. Your task is to add the new feature to our system so that we can begin selling a new category of items. First an introduction to our system:

- All items have a `sell_in` value which denotes the number of days we have to sell the item
- All items have a `quality` value which denotes how valuable the item is
- At the end of each day our system lowers both values for every item

Pretty simple, right? Well this is where it gets interesting:

- Once the sell by date has passed, `quality` degrades twice as fast
- The `quality` of an item is never negative
- "Aged Brie" actually increases in `quality` the older it gets
- The `quality` of an item is never more than 50
- "Sulfuras", being a legendary item, never has to be sold or decreases in `quality`
- "Backstage passes", like aged brie, increases in `quality` as its `sell_in` value approaches; `quality` increases by 2 when there are 10 days or less and by 3 when there are 5 days or less but `quality` drops to 0 after the concert

We have recently signed a supplier of conjured items. This requires an update to our system:

- "Conjured" items degrade in `quality` twice as fast as normal items

Feel free to make any changes to the `update_quality` method and add any new code as long as everything still works correctly. However, do not alter the Item class or Items property as those belong to the goblin in the corner who will insta-rage and one-shot you as he doesn't believe in shared code ownership (you can make the `update_quality` method and Items property static if you like, we'll cover for you).

Just for clarification, an item can never have its `quality` increase above 50, however "Sulfuras" is a legendary item and as such its `quality` is 80 and it never alters.


## How to use this Kata

There are two important files to make the code run in this repository: `item.R` and `gilded_rose.R`. We're told that we can't change `item.R` in the specifications above, but we are to modify `gilded_rose.R` to add "conjured" items. Looking at `gilded_rose.R` you'll quickly notice that it's a mess and that adding the logic for conjured items is going to be a headache.

We need to refactor our code so that we can easily add the conjured items. Before stepping into this mess, it would be a good idea to understand how the code works. One way of doing this is to use tests to assess whether our changes give the right results. Remember that although the current code is a disaster, it works.

There are already some R scripts in this repository that are used for testing the code using several methods. The first uses the `RUnit` package and has one test, which fails. The files using `RUnit` are `test_setup.R` and `runit_gilded_rose.R` - run `test_setup.R` to run the test in `runit_gilded_rose.R`. The second set of scripts are for performing "Approval Testing". If you run the R code in `texttest_fixture.R` you should get the output in `texttest_fixture.gold`. To confirm this, I'd suggest running the following bash commands...

```
Rscript texttest_fixture.R > texttest_fixture.refactor
diff texttest_fixture.refactor texttest_fixture.gold
```

If you don't get any output, that's a great sign!

## Suggestions

I don't want to be too overbearing on how you approach this Kata. Here are some suggestions to get you going

1. Fork the repository. To practice your git and GitHub skills, I'm going to suggest that you practice using GitFlow
2. Take in the code and see if you can relate the specifications to what Leeroy wrote. Can you come up with some initial issues?
3. Write some unit tests to capture the current functionality of the code. To do this, you may want to use a different testing suite, which may require you to convert the R code to a package. Do this in an issue branch and merge it with the main branch once you're happy with things
4. Take on the next issue that looks interesting to you! 

A great video you might watch is Jenny Bryan's talk at useR!2018 in Brisbane on "[Code Smells](https://www.youtube.com/watch?v=7oyiPBjLAWY&ab_channel=RConsortium)". It's really fantastic and will give you some other ideas for things to think about for future refactoring steps