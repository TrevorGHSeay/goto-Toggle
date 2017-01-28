### SubstringTo

This method is intended to in many cases replace the string type's similar method, 'Substring', by allowing the designation of an end index instead of a count from start.


``string SubstringTo(string source, int start, int end)``

``{``

``		int end2Use = end;``

``		int source_max = source.Length - 1;``

``		if ( end2Use > source_max - 1 ) end2Use = source_max;``

``		return source.Substring(start, end2Use - start);``

``}``
