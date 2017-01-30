### SubstringBetween

This method is intended to in many cases replace the string type's similar method, 'Substring', by allowing the designation of an end index instead of a count from start. If the end is greater than than the length of the string, it defaults to the end being the last index of source.

    string SubstringBetween(string source, int start, int end)
    {
        int end2Use = end;
        int source_max = source.Length - 1;
        if ( end2Use > source_max ) end2Use = source_max;
        return source.Substring(start, end2Use - start);
    }

[Go Back](https://trevorghseay.github.io/goto-Toggle/UsefulSnippets)
