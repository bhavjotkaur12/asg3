```
// InterpreterPattern.cs: Code for interpretting boollean expressions using interpreter pattern
// created by Samay Sehgal - October 22, 2023
// Editted by Bhavjot Kaur Pal - October 23,2023

using System;
using System.Collections.Generic;

// Abstract Expression
abstract class AbstractExpression
{
    public abstract bool Interpret(Context context);
}

// Terminal Expression
class TerminalExpression : AbstractExpression
{
    private string value;

    public TerminalExpression(string value)
    {
        this.value = value;
    }

    public override bool Interpret(Context context)
    {
        return context.Lookup(value);
    }
}

// Non-terminal Expression
class NonTerminalExpression : AbstractExpression
{
    private AbstractExpression left;
    private AbstractExpression right;

    public NonTerminalExpression(AbstractExpression left, AbstractExpression right)
    {
        this.left = left;
        this.right = right;
    }

    public override bool Interpret(Context context)
    {
        return left.Interpret(context) || right.Interpret(context);
    }
}

// Context
class Context
{
    private Dictionary<string, bool> variables = new Dictionary<string, bool>();

    public bool Lookup(string name)
    {
        if (variables.ContainsKey(name))
        {
            return variables[name];
        }
        return false;
    }

    public void Assign(string name, bool value)
    {
        variables[name] = value;
    }
}

class Program
{
    static void Main(string[] args)
    {
        Context context = new Context();
        context.Assign("A", true);
        context.Assign("B", false);

        AbstractExpression expression = new NonTerminalExpression(
            new TerminalExpression("A"),
            new TerminalExpression("B")
        );

        bool result = expression.Interpret(context);
        Console.WriteLine("Result: " + result); // Output: Result: True
    }
}
```
