[Interpreter pattern](https://en.wikipedia.org/wiki/Interpreter_pattern)
-----------------

 The **Interpreter pattern** is a [design pattern](https://en.wikipedia.org/wiki/Design_pattern_(computer_science)) that specifies how to evaluate sentences in a language.

 The basic idea is to have a [class](https://en.wikipedia.org/wiki/Class_(computer_science)) for each symbol ([terminal](https://en.wikipedia.org/wiki/Terminal_symbol) or [nonterminal](https://en.wikipedia.org/wiki/Nonterminal_symbol)).

 The [syntax tree](https://en.wikipedia.org/wiki/Abstract_syntax_tree) of a sentence in the language is an instance of the [composite](https://en.wikipedia.org/wiki/Composite_pattern) pattern and is used to evaluate (interpret) the sentence for a client.

> <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/bc/Interpreter_UML_class_diagram.svg/804px-Interpreter_UML_class_diagram.svg.png" alt="Interpreter UML class diagram.svg" width="511" height="274" />


The following [Backus–Naur form](https://en.wikipedia.org/wiki/Backus%E2%80%93Naur_form) example illustrates the interpreter pattern.

```properties
expression ::= plus | minus | variable | number
plus ::= expression expression '+'
minus ::= expression expression '-'
variable ::= 'a' | 'b' | 'c' | ... | 'z'
digit = '0' | '1' | ... | '9'
number ::= digit | digit number
```

The grammar defines a language that contains [Reverse Polish Notation](https://en.wikipedia.org/wiki/Reverse_Polish_Notation) expressions like:

```properties
a b +
a b c + -
a b + c a - -
```

Following the interpreter pattern there is a class for each grammar rule.

```java
import java.util.Map;

interface Expression {
    public int interpret(final Map<String, Expression> variables);
}

class Number implements Expression {
    private int number;
    public Number(final int number)       { this.number = number; }
    public int interpret(final Map<String, Expression> variables)  { return number; }
}

class Plus implements Expression {
    Expression leftOperand;
    Expression rightOperand;
    public Plus(final Expression left, final Expression right) {
        leftOperand = left;
        rightOperand = right;
    }
		
    public int interpret(final Map<String, Expression> variables) {
        return leftOperand.interpret(variables) + rightOperand.interpret(variables);
    }
}

class Minus implements Expression {
    Expression leftOperand;
    Expression rightOperand;
    public Minus(final Expression left, final Expression right) {
        leftOperand = left;
        rightOperand = right;
    }
		
    public int interpret(final Map<String, Expression> variables) {
        return leftOperand.interpret(variables) - rightOperand.interpret(variables);
    }
}

class Variable implements Expression {
    private String name;
    public Variable(final String name)       { this.name = name; }
    public int interpret(final Map<String, Expression> variables) {
        if (null == variables.get(name)) return 0; // Either return new Number(0).
        return variables.get(name).interpret(variables);
    }
}

```

While the interpreter pattern does not address parsing a parser is provided for completeness.

```java
import java.util.Map;
import java.util.Stack;

class Evaluator implements Expression {
    private Expression syntaxTree;

    public Evaluator(final String expression) {
        final Stack<Expression> expressionStack = new Stack<Expression>();
        for (final String token : expression.split(" ")) {
            if (token.equals("+")) {
                final Expression subExpression = new Plus(expressionStack.pop(), expressionStack.pop());
                expressionStack.push(subExpression);
            } else if (token.equals("-")) {
                // it's necessary remove first the right operand from the stack
                final Expression right = expressionStack.pop();
                // ..and after the left one
                final Expression left = expressionStack.pop();
                final Expression subExpression = new Minus(left, right);
                expressionStack.push(subExpression);
            } else
                expressionStack.push(new Variable(token));
        }
        syntaxTree = expressionStack.pop();
    }

    public int interpret(final Map<String, Expression> context) {
        return syntaxTree.interpret(context);
    }
}

```

Finally evaluating the expression "w x z - +" with w = 5, x = 10, and z = 42.

```java
import java.util.Map;
import java.util.HashMap;

public class InterpreterExample {
    public static void main(final String[] args) {
        final String expression = "w x z - +";
        final Evaluator sentence = new Evaluator(expression);
        final Map<String, Expression> variables = new HashMap<String, Expression>();
        variables.put("w", new Number(5));
        variables.put("x", new Number(10));
        variables.put("z", new Number(42));
        final int result = sentence.interpret(variables);
        System.out.println(result);
    }
}
```
