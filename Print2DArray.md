## Print 2D Array
This method is used to print a 2D array (of any object-inherited data type that implements the IConvertible interface) to a string, where the result is cleanly legible from from console or editor formats.

    string Print2DArray (Array[object] array_to_print)
    {
        string toReturn = "";
        foreach (object element in array_to_print)
        {
            if ( (element as iConvertible) != null )
                toReturn += element.ToString();
            else 
                toReturn += "* Value Inconvertible *";

            toReturn += Environment.NewLine;
        }
        return toReturn;
    }
