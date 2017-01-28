## Print 2D Array
This method is used to print a 2D array (of any object-inherited data type that implements the IConvertible interface) to a string, where the result is cleanly legible from from console or editor formats.

    string Print2DArray (Array[object] array_to_print)
    {
        int array_placeholder = 0;
        int array_length = array_to_print.Length - 1;
        string toReturn = "";
        foreach (object element in array_to_print)
        {
            if ( (element as iConvertible) != null )
                toReturn += array_placeholder.ToString() + ". " + element.ToString();
            else 
                toReturn += array_placeholder.ToString() + ". *Value Inconvertible*";

            if (array_placeholder < array_length) toReturn += Environment.NewLine;
            ++ array_placeholder;
        }
        return toReturn;
    }
    
[Go Back](https://trevorghseay.github.io/goto-Toggle/UsefulSnippets)
