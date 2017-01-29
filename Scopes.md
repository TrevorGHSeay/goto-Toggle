## Scopes

``Consider this: I just used a colon; and a semi-colon, and now a comma (but least I ended the sentence with period).``

The above is a lenient example of scoping; but what is it that provides the structures for which to interpret? Well, the answer to that is grammar. 
As you can see, the example provided is comprised of a series of punctual cues; denoting the flow of the statement is required for legibility, 
and so the following might be considered an apt representation for what happens inside your brain to understand it as it's being read.

    Consider this
    {
      I just used a colon
      {
        but now I just used a semi-colon
        {
          and now a comma
          {
            but at least I ended the sentence with a period
          }
        }
      }
    }
    
Similarly, the process for visualizing questions and their conclusions can be written in like kind:

    Is the dog brown?
    
    If so 
    {
        the dog is brown   
    }
    else
    {
        the dog's colour is not brown
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
