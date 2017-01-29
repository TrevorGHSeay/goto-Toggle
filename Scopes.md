## Scopes

``Consider this: I just used a colon; a semi-colon too, and now a comma (but least I ended the sentence with period).``

The above is a lenient example of scoping; but what is it that provides the structures for which to interpret? Well, the answer to that is grammar. 
As you can see, the example provided is comprised of a series of punctual cues; denoting the flow of the statement is required for legibility, 
and so the following might be considered an apt representation for what happens inside your brain to understand it as it's being read.

    Consider this
    {
      I just used a colon
      {
        a semi-colon too
        
        and now a comma
        {
          but at least I ended the sentence with a period
        }
      }
    }
    
Similarly, the process for visualizing statements to be answered and their conclusions can be written in like kind:

    "The dog is brown"
    
    If the dog is brown
    {
        the statement is true  
    }
    else
    {
        the statement is false
    }
    
Not coincidentally, you'll likely notice - if you're already familiar with basic programming - that the above is in fact very similar already to how it would be written in code (particularily C#). To contrast its hypothetical form:

    bool the_dog_is_brown;
    
    if (dog.Color == Brushes.Brown)
    {
        the_dog_is_brown = true;
    }
    else
    {
        the_dog_is_brown = false;
    }

But what does all this jibber-jabber mean? Well, for starters, it goes to show that you already understand coding on a fundamental level; and that level is logic.

Keeping in mind that (ordinarily) computers are literal slaves that will do exactly as asked - no more, and no less. This is to say that, as outline above, only particular 'arguments' are performed at certain times, and the timing is also mostly up to you. So why are you learning all this first?

The answer to this question is in how we learn. In many cases I've seen programmers attempt to learn variable manipulation, method creation or even event raising first, without first knowing what they're doing or why it works. This is why I'm going to run through scoping as briefly as I can: so as to give the reader a "heads-up" for the termage, perspective and mindframe that I myself use to visualize a program before creating it.

So we know that scopes are kinda like guidelines, but that isn't saying much. To understand most C# code, you should first be aware of the most common scopes found at an elemental level:

    Namespaces, classes/structs, methods, conditionals, and loops.
    
#### Namespaces

Namespaces can be thought of as a group of code's nickname, or alias. Within namespaces lie the mechanics for essentially all executable programs, video games, and blocks of programming that you will be working with in the future. They are the literal 'name space' which is occupied on the computer for to be referenced by seperate programming from its own (or not).

Because of this, many developers find it useful to use a single namespace for each entire project, because it requires extra work to reference a different namespace than the one you're currently in. Don't worry; this is all a little ahead of you right now so I'm not going to rest my lesson on your understanding of this concept yet - just think of namespaces as the name you would use to perform an action you've already explained how to do before. Easy enough.

#### Classes / Structs

#### Methods

#### Conditionals

#### Loops
