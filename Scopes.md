## Scopes

``Consider this: I just used a colon; a semi-colon too, and now a comma (but least I ended the sentence with period).``

The above is a lenient example of scoping; but what is it that provides the structures for which to interpret? Well, the answer to that is grammar. 
As you can see, the example provided is comprised of a series of punctual cues; denoting the flow of the statement is required for legibility, 
and so the following might be considered an apt representation for what happens inside your brain to understand it as it's being read.

    Consider this
    {
    
      I just used a colon
      
      a semi-colon too
        
      and now a comma
      {
        but at least I ended the sentence with a period
      }
      
    }
    
Similarly, the process for visualizing statements to be inquired and their corresponding conclusions can be written in like kind:

    "The dog is brown"
    
    If the dog is brown
    {
        remember that the previous statement is true  
    }
    else
    {
        remember that the previous statement is false
    }
    
Not coincidentally, you'll likely notice - if you're already familiar with basic programming - that the above is in fact very similar already to how it would be written in code (particularily C#). To contrast its hypothetical form:

```csharp
    bool the_dog_is_brown;
    
    if (dog.Color == Colors.Brown)
    {
        the_dog_is_brown = true;
    }
    else
    {
        the_dog_is_brown = false;
    }
```

But what does all this jibber-jabber mean? Well, for starters, it goes to show that you already understand coding on a fundamental level; and that level is logic.

Keeping in mind that (ordinarily) computers are, quite literally, slaves that will do exactly as asked - no more, and no less - so only particular 'statements' are performed at certain times, and the timing is also mostly up to you. So why are you learning all this first?

The answer to this question is in how we learn. In a few cases I've seen programmers attempt to learn variable manipulation, method creation or even event raising first, without first knowing what they're doing or why it works. This is why I'm going to run through scoping as briefly as I can: so as to give the reader a "heads-up" for the termage, perspective and mindframe that I myself use to visualize a program before creating it.

So we know that scopes are kinda like guidelines, but that isn't saying much. To understand most C# code, you should first be aware of the most important scopes found at an elemental level:

    Namespaces, classes/structs, interfaces, enumerables, methods, conditionals, and loops.
    
#### Namespaces

Namespaces can be thought of as a group of code's nickname, or alias. Within namespaces lie the mechanics for essentially all executable programs, video games, and blocks of programming that you will be working with in the future. They are the literal 'name space' which is occupied on the computer for to be referenced by seperate programming from its own (or not). You can imagine them as identifiers for groups of concepts that you might call on later.

Because of this, many developers find it useful to use a single namespace for each entire project because it requires extra work to reference a different namespace than the one you're currently in. Don't worry; this is all a little ahead of you right now so I'm not going to rest my lesson on your understanding of this concept yet - just think of namespaces as the name you would use to refer to an outline of action(s) you're explaining how to do. It's also what you'll use to hold the entirety of each of your first few programs. 

```csharp
namespace ProgrammingExplanation
{

}
```

Easy enough. This is in fact all a namespace really is at the text level; if you were to enter this into a code editor, and attempt to compile it, it will work with no errors attached to it. You can imagine it representing a nickname for an idea - or concept - that's yet to be articulated.

#### Classes / Structs

Classes and Structs occupy a special space in the Object-Oriented-Programming ecosphere; at their fundamental level, they let us define objects - or, more specifically, nouns. Classes and Structs, however, are not those nouns. Conceptually, they're something of a blueprint that describes a noun that might be used later in the way you imply with the blueprint itself. There are more similarities between Classes and Structs than there are differences, on the programming level, and should be seen to derive from the aforementioned description: they are a description of a noun.

You can imagine making the blueprint for a house; the blueprint is not the house but, rather, describes what a house will have, do, and - perhaps - why. You may later use said blueprint to *construct* an actual house, and this is the extent of the connection between both blueprint and object. To better understand how this connection and how it applies to the difference between Classes and Structs, you must first understand the difference between a *reference* and *value* type.

###### Reference Types

Reference types *refer* to things. If I tell you that there's a coffee shop on the corner, which we both frequent, then it can be said that we're now both referring to the same cafe. You don't need to be standing next to it, and neither must you touch it; simply by referring to it we can come to some concensus about what we're talking about. Likewise, if the shop is then altered, then both our understandings of this house will change automatically, because we'll both see the changes that were made. This concept is what we refer to as a reference type: an object that sits independently from outside either of points of perception is something we *refer* to.

###### Value Types

A Value type is, unlike a reference type, dependent on the point of perception. Presume we're both holding an apple of the same variety, shape, and colour; these apples are, for practical purposes, identical in every way and so we can go on to describe our own apple and know it'll be understood and agreed upon by the other. If I were then to take a bite from my apple, however, then our points of perception becomes important; because they're two different apples, it can be said they have - implicitly - two difference dimensions of values. Even though they were originally identical (quantitatively), and we were both talking about the same concept, the object we were each describing was no more than our own apple (qualitatively). In this way, it is not a shared perception but, rather, two completely different apples; if I wanted to give you my apple, presuming we defined an apple to be a value type programmatically, then I must first duplicate that apple and give it to you for us to each take part in the consumption of an apple that has the same quantitative experience.

##### Classes

Classes are reference types. This means that when I refer to a coffee shop that was defined (remember, like a blueprint) using class notation, then we will be allowed to pass around by pointing my finger at the actual cafe - which will exist irrespective of either of our points of reference. It need not be copied, or described again; if I want to change it, then I can simply make changes to the original cafe and then my pointing finger need not change because the cafe is still in the same place (albeit itself having been altered). The point is that it's still the same object and still exists at the same location conceptually. This is what classes are for: to be pointed at and agreed upon by many sources or, in other circumstances, to make broad changes to an idea that many other nouns understand or interact with.

```csharp
class Cafe
{

}
```

##### Structs

Structs are value types. This makes the apple we were refering to best suited as a candidate for struct notation so we may duplicate the original object at will without risking altering the original (in case someone wants to take a bite). We can give out as many copies of the original apple as we want, but there will only ever be one of the original apple that you began giving out; we're not pointing at an apple on the table because, then, anyone might take a bite out of our apple (which is clearly undesirable).

```csharp
struct Apple
{

}
```

##### Tying It Together

Namespaces being the nickname for a set of concepts and instructions that are organized as relations, we must take our classes and structs and put them inside a namespace if we want to use those concepts properly:

```csharp
namespace ProgrammingExplanation
{
    class Cafe
    {

    }
    
    struct Apple
    {

    }
}
```

#### Methods

Given that namespaces are nicknames for a group of concepts or ideas, and that classes/structs are blueprints (or descriptions) of any particular noun, we can extend the analogy further by conceptualizing methods as the description of *verbs*. Keeping in mind that concepts and ideas cannot act but, rather, that nouns can, it stands to reason that we may only describe the orders to carry out a verb to those nouns. You can now refer back to a cafe and acknowledge that a cafe may open or close; this means that, if we wish to describe how a cafe might do these things, we need to establish that there are instructions it needs to follow in order to complete those tasks. Likewise, we may also want to describe an apple's proclivity to decompose:

```csharp
namespace ProgrammingExplanation
{
    class Cafe
    {
        void Open()
        {
        
        }
        
        void Close()
        {
        
        }
    }
    
    struct Apple
    {
        void Decompose()
        {
        
        }
    }
    
}
```

It is in this way that we begin to give life to our program: by describing these things as related to each other (in that they both share a space in this tutorial), we give them a place in the same namespace. To do this, we must put those descriptions inside the namespace declaration itself, and nest all things related to those things inside of their own descriptions (like we'd done with the three aforementioned methods). This is the basis for Object-Oriented-Programming (OOP), as it lets us organize our thoughts so we might call on them later to do work.

#### Conditionals

Conditionals are a bridge into the next chapter, where we will be discussing statements. Statements being the actual instructions that a program follows, and not just the overarching structure of it, Conditionals are statements that allow us to take a varying course of action depending on a *condition*. A condition must evaluate to either a true or a false value (technically refered to as a boolean). We'll leave the specifics out of the picture for this chapter, but the most important thing to remember is their function and scoping mechanism; if the condition that a conditional evaluated then happens to be false, the computer does not perform whatever is inside its scope - likewise, when that condition is determined to be true, the computer carries out all instructions that the conditional's scopes has laid out. We can also catch if the condition evaluates as false, and carry out those instructions instead:

```csharp
if(cafe.isOpen)
{
    // Do something that can only be done when the cafe is open
}
else
{
    // Do something that can only be done once the cafe is closed
}
```

Typing it all together again, we can now do something like this:

```csharp
namespace ProgrammingExplanation
{
    class Cafe
    {
        void Open()
        {
            if(isOpen)
            {
                // Do something that can only be done when the cafe is open
            }
            else
            {
                // Do something that can only be done once the cafe is closed
            }
        }
        
        void Close()
        {
            if(isOpen)
            {
                // Do something that can only be done when the cafe is open
            }
            else
            {
                // Do something that can only be done once the cafe is closed
            }
        }
    }
    
    struct Apple
    {
        void Decompose()
        {
            if(isOld)
            {
                // Instruct on how to decompose
            }
        }
    }
}
```

#### Loops

Loops, like conditionals, also require a condition; there are three main types of loops but they all share the same concept, however, and so really only have one main idea behind them that must be understood before using them in code: loops, unlike conditionals, **repeat** the instructions that are inside their scope ``while`` the condition is true. Loops, then, let us do the same thing many times without needing to write it out as many times as we want them to do it, and that makes our lives (as programmers) notably simpler. All we need to do is write the instructions we want to have repeatedly performed once, and stick it inside the scope of a loop:


```csharp
if(cafe.isOpen)
{
    // Do something that can only be done when the cafe is open
}
else
{
    // Do something that can only be done once the cafe is closed
}
```

Putting it all together, we now get something like this:

```csharp
namespace ProgrammingExplanation
{
    class Cafe
    {
        void Open()
        {
            if(isOpen)
            {
                // Do something that can only be done when the cafe is open
            }
            else
            {
                // Do something that can only be done once the cafe is closed
            }
        }
        
        void Close()
        {
            if(isOpen)
            {
                // Do something that can only be done when the cafe is open
            }
            else
            {
                // Do something that can only be done once the cafe is closed
            }
        }
    }
    
    struct Apple
    {
        void Decompose()
        {
            while(isNotOld)
            {
                // Get older
            }
        
            if(isOld)
            {
                // Instruct on how to decompose
            }
        }
    }
}
```
