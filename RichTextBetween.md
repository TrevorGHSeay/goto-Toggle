### RichTextBetween

This method is meant to be used as a wrapper for to grab the text string between two locations in a WPF Rich Text Box.

    string RichTextBetween(TextPointer start, Textpointer end)
    {
          return new Textrange(start, end).Text;
    }

[Go Back](https://trevorghseay.github.io/goto-Toggle/UsefulSnippets)
