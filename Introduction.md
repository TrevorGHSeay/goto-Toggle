## Welcome to Programming

``For ages the human race has strived for one thing, and one thing only: to control - and conquer - its environment. ``

It may seem like a tiring and fruitless effort without this goal, but to understand the inner workings of digital computation is to do just this - conquer your environment. In a time when so much culture, policy, community and progress is technologically oriented, it seems irresponsible to the writer not to have an understanding of the power that comes with so little knowledge; the power to make machines that create, or destroy, without warrant or care in its own behalf. This is a mastery of our environment, and its power is a privilege.

Now, without anymore preaching to the choir, I'll begin in the best way I know how: explaining what code itself really is, and what's really going on inside your computer. Now know that I am not a master of every component nor assembly myself, so if there are any corrections that need to be made I greatly appreciate the reader appealing to me in professional email via the [contact](https://trevorghseay.github.io/goto-Toggle/Contact) section. So let's get to it.

### What is Code?

Code - in its most core sense - is the typed interface from which a digital computation device derives arguments that it uses to direct its ability to perform work. This means that the machine you are programming on literally 'reads' your commands (at compilation time) for to be directed by it. This is where the saying that 'a computer is only as smart as the people that made it' comes from; it only knows how to do anything inasmuch as the task is outlined using the diction its been programmed to interface with.

This (as you may have seen on the previous page) is performed ordinarily in one step by a compiler program, but C# - and several other languages - have the novel ability in being designed to be first compiled to bytecode before being again compiled, this time into machine code. Because of this difference, C# compiles on most machines second and so has a certain security in the time invested learning it. This, and because it's the language I'm most comfortable with, are why I'm using it in the page's [tutorials](https://trevorghseay.github.io/goto-Toggle/Tutorials)' and [useful snippets](https://trevorghseay.github.io/goto-Toggle/Projects)' pages here on GitHub.

### So how do you control it?

Well, here's the interesting thing: you kind of make it up as you go along, but with an immovable line of rules that need be followed for consistent success. 

An example that might be given is making a machine 'remember' something. Without detailing how or why this works just yet, so as to spark your newfound curiosity, I'll provide a simple snippet that would be used to recall a piece of memory later:

    string Greeting = "Hello, World!";

As you can see, it's far simpler than the engineering of a processor leads the programmer to believe, but with the above line you would now, under the appropriate conditions, be permitted to recall this information and, say, display it in a Command Prompt window, and show itself until someone enters a reply:

    Console.WriteLine(Greeting);
    Console.ReadLine();

As confusing as it might seem, every single character in the lines I've shown are necessary and serve a vital purpose in how a computer might interpret what's been written.

###So how did I know what stuff to write?

Check out the **[next tutorial](https://trevorghseay.github.io/goto-Toggle/Scopes)**!

[Go Back](https://trevorghseay.github.io/goto-Toggle/Tutorials)
