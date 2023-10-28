```
using System;
using System.Collections.Generic;

// Abstract Expression
abstract class Expression
{
    public abstract double Interpret();
}

// Terminal Expression for numbers
class NumberExpression : Expression
{
    private double value;

    public NumberExpression(double value)
    {
        this.value = value;
    }

    public override double Interpret()
    {
        return value;
    }
}

// Non-terminal Expression for binary operations
class BinaryOperationExpression : Expression
{
    private Expression left;
    private Expression right;
    private char operation;

    public BinaryOperationExpression(Expression left, Expression right, char operation)
    {
        this.left = left;
        this.right = right;
        this.operation = operation;
    }

    public override double Interpret()
    {
        switch (operation)
        {
            case '+':
                return left.Interpret() + right.Interpret();
            case '-':
                return left.Interpret() - right.Interpret();
            case '*':
                return left.Interpret() * right.Interpret();
            case '/':
                double divisor = right.Interpret();
                if (Math.Abs(divisor) < 1e-6)
                {
                    throw new DivideByZeroException("Division by zero is not allowed.");
                }
                return left.Interpret() / divisor;
            default:
                throw new InvalidOperationException("Invalid operator: " + operation);
        }
    }
}

// Context for storing variables and parsing expressions
class Context
{
    private Dictionary<string, double> variables = new Dictionary<string, double>();

    public void Assign(string variable, double value)
    {
        variables[variable] = value;
    }

    public double Lookup(string variable)
    {
        if (variables.ContainsKey(variable))
        {
            return variables[variable];
        }
        throw new ArgumentException("Variable not found: " + variable);
    }
}

class Program
{
    static void Main(string[] args)
    {
        // Create a context and assign variables
        Context context = new Context();
        context.Assign("x", 5.0);
        context.Assign("y", 2.0);

        // Parse and evaluate expressions
        Expression expression1 = new BinaryOperationExpression(new NumberExpression(5), new NumberExpression(2), '+');
        Expression expression2 = new BinaryOperationExpression(new NumberExpression(5), new NumberExpression(2), '*');
        Expression expression3 = new BinaryOperationExpression(new NumberExpression(5), new NumberExpression(0), '/');

        Console.WriteLine("Result 1: " + expression1.Interpret()); // Output: Result 1: 7
        Console.WriteLine("Result 2: " + expression2.Interpret()); // Output: Result 2: 10
        try
        {
            Console.WriteLine("Result 3: " + expression3.Interpret());
        }
        catch (Exception ex)
        {
            Console.WriteLine("Exception: " + ex.Message); // Output: Exception: Division by zero is not allowed.
        }
    }
}
```
