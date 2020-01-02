# C# 8 Features
The following are some of the features introduced with C# 8, which is the default compiler when selecting the .Net Core 3.X framework. These features are discussed more in-depth at the following links:

-[Microsoft Docs]: https://docs.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-8


## Readonly Members
You can apply the readonly modifier to members of a struct. The compiler enforces the readonly rule by not letting members modify state and generating a compile error if state modification is attempted.

    public struct Point
    {
        public readonly override string ToString() => $"({X}, {Y}) is {Distance} from the origin";
    }


## Default interface methods
You can now define an implementation when you declare a member of an interface. The most common scenario is to safely add members to an interface already released and used by innumerable clients. Interfaces can now include static members, including fields and methods. Different access modifiers are also enabled.

## More patterns in more places
Although C# 7 introduced it, C# 8 expands the vocabulary so you can use more pattern expressions in more places in your code.

### Switch expressions
Eliminate repetitive case and break keywords, and fewer curly braces.

        public enum Color
        {
            Green = 0,
            Yellow = 1,
            Red = 2
        };

        static Color LightColor(int color) =>
            color switch
            {
                0 => Color.Green,
                1 => Color.Yellow,
                2 => Color.Red,
                _ => throw new Exception("invalid color!")
            };


### Property patterns
The property pattern enables you to match on properties of the object examined.
    
    class Address
    {
        public Address(string street, string state, int zip) => (Street, State, Zip) = (street, state, zip);
        public string Street { get; set; }
        public string State { get; set; }
        public string Zip { get; set; }
    }

    static decimal SalesTax(Address address) =>
        address switch
        {
            (State: "MA") => 5.0,
            (State: "NH") => 0.0,
            (_, _) => throw new Exception("state not matched!")
        };

### Tuple patterns
Some algorithms depend on multiple inputs. Tuple patterns allow you to switch based on multiple values expressed as a tuple.

    static bool Validate(string first, string second) =>
        (first, second) switch
        {
            ("a", "b") => true,
            ("c", "d") => true,
            (_, _) => false
        };

### Positional patterns
You can use positional patterns to inspect properties of the object and use those properties for a pattern.

    public class Point
    {
        public int X { get; }
        public int Y { get; }
        public Point(int x, int y) => (X, Y) = (x, y);
        public void Deconstruct(out int x, out int y) =>
            (x, y) = (X, Y);
    }

    static string GetQuadrant(Point point) => 
        point switch
        {
            (0, 0) => "Origin",
            var (x, y) when x > 0 && y > 0 => "One",
            var (x, y) when x < 0 && y > 0 => "Two",
            var (x, y) when x < 0 && y < 0 => "Three",
            var (x, y) when x > 0 && y < 0 => "Four",
            var (_, _) => "OnBorder",
            _ => "Unknown"
        };

## Using declarations
Previously you did:

    using (var file = new System.IO.StreamWriter("WriteLines2.txt")) {...}
    
Now you can do:

    using var file = new System.IO.StreamWriter("WriteLines2.txt");
    ...

## Static local functions
C# 7 introduced Local Functions. C# 8 now allows this Local Functions to be static to ensure that local function doesn't capture (reference) any variables from the enclosing scope.

    int M()
    {
        int y = 5;
        int x = 7;
        return Add(x, y);

        static int Add(int left, int right) => left + right;
    }

## Disposable ref structs

## Nullable reference types

## Asynchronous streams

## Indices and ranges

## Null-coalescing assignment

## Unmanaged constructed types

## Stackalloc in nested expressions

## Enhancement of interpolated verbatim strings

