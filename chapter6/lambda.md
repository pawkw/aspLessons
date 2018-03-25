# Lambda Functions

Unfortunately, the C# book and the ASP.NET books don't have a good explanation of lambda functions. Here is a breakdown.

## The Usual Way of Defining a Function

For the tic-tac-toe console program I had a display like this:
```
 1 | 2 | 3
---+---+---
 4 | 5 | 6
---+---+---
 7 | 8 | 9
```

I needed to convert the number the user entered to a row and a column for the 2d array. In order to do this right, we have to subtract 1 from the square number. Here are the functions in regular form:
``` cs
public static int getColumn(int number)
{
    // The column is the remainder of the number divided by 3.
    return (number - 1) % 3;
}

public static int getRow(int number)
{
    // The row is the number divided by 3.
    return (number - 1) / 3;
}
```

The first `int` on the line says that we are returning an integer. The second `int` says that we are sending an integer named `number` to the function. So we can use these functions to get the row and column:
``` cs
    int row = getRow(userInput);
    int col = getColumn(userInput);
```

If the user entered '5' for the middle square, we would end up with 'row = 1' and 'col = 1'. This is nice, but these functions are so small that it's almost not worth writing. We could just say `row = --userInput / 3;`. The problem with this is that if we change how the board is represented, we have to search through all the code and change every line that we assigned `row` directly.

## Defining a Lambda Function

Lambdas let us write functions quickly without giving up the code encapsulation. Here are the same functions as lambda functions:
``` cs
Func<int, int> getColumn = number => --number % 3;
Func<int, int> getRow = number => --number / 3;
```

When we write functions this way, the first `int` says that we are sending an integer, and the second says we are returning an integer. This is the opposite order of what we did with the normal functions. More on that later. Next we have the function name and then the assignment (the equals sign). After that we have the name of the integer that sending, `number`, then we have a 'double arrow' `=>`, and finally we have what we want to do with `number`.

Notice that there is no return statement. We don't need it for really simple functions. If we had multiple lines, we would need the return statement. If we have multiple parameters, we need to put the names inside round brackets and separated by commas.

Here is a lambda function to change a row and a column into a number:
``` cs
Func<int, int, int> getSquareNumber = (row, col) =>
{
    int result = row * 3;
    result += col;
    return ++result;
};
```

A couple of things to notice here. We have to put a semicolon at the end of a lambda function, even when we are using curly braces. The last `int` is the return type. If we wanted to return a string, we would say `Func<int, int, string>`. The whole multiline function is not needed. Let's return that number as a string:
``` cs
Func<int, int, string> takenError = (row, col) => "Square " + (row * 3 + col + 1).ToString() + " is already taken.";
```

Remember that the **last** variable type listed in a lambda function is the return type.

## Lambdas as Anonymous Functions

Sometimes we need to tell ASP.NET which function is going to handle things. For example, the `@HTML.EditorFor` helper wants a function where it can send a model and get a property back. We could define a function and supply the name of the function, or we could drop in a lambda function without bothering to do a declaration or anything else. Here's how we do it:
``` cs
@Html.EditorFor(user => user.FirstName);
```

This lambda function takes a `user` object and sends back the `FirstName` property. If there was something complicated here, we would have to do a proper function, but all we're doing is returning a single property, so there is no need to waste time and space defining a function.
