# C# 7 Features
The following are some of the features introduced with C# 7, which is the default compiler when selecting the .Net Core 2.X framework. These are discussed more in-depth at the following links:

-[Microsoft Docs]: https://docs.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-7


## out variables
You can declare out values inline as arguments to the method where they're used.

    if (int.TryParse(input, out int result)){}
    
    or

    if (int.TryParse(input, out var result)){}


## Tuples
You can create lightweight, unnamed types that contain multiple public fields. Compilers and IDE tools understand the semantics of these types.

    (string Alpha, string Beta) alphabetStart = ("a", "b");

    or

    var alphabetStart = (Alpha: "a", Beta: "b");

    or

    (string Alpha, string Beta) alphabetStart = Foo();
    private static (string Alpha, string Beta) Foo()
    {
        return ("a", "b");
    }

## Discards
Discards are temporary, write-only variables used in assignments when you don't care about the value assigned. They're most useful when deconstructing tuples and user-defined types, 
as well as when calling methods with out parameters. A discard is a write-only variable whose name is _ (the underscore character).

    var (a, _, b) = ("a", "don't-care", "b");

## Pattern Matching
Pattern matching is a feature that allows you to implement method dispatch on properties other than the type of an object. You can create branching logic based on arbitrary types 
and values of the members of those types.

    if (a is string alpha && b is string beta)
        Console.WriteLine(Alpha + " - " + Beta);

    or

    for (int i = 0; i < 10; i++)
        switch (i)
        {
            case int odd when i % 2 != 0:
                Console.WriteLine($"{odd} is odd");
                break;
            case int even when i % 2 == 0:
                Console.WriteLine($"{even} is even");
                break;
        }


## ref locals and returns
Method local variables and return values can be references to other storage.

    string foo =  "foo";
    void func(ref string name) => name = "bar";
    func(ref foo);
    Console.WriteLine(foo);

## Local Functions
You can nest functions inside other functions to limit their scope and visibility.

        static void Main(string[] args)
        {
            string foo = "foo";
            funct(ref foo);
            Console.WriteLine(foo);

            void func(ref string name)
            {
                name = "bar";
            }
        }

## More expression-bodied members
The list of members that can be authored using expressions has grown. In C# 7.0, you can implement constructors, finalizers, and get and set accessors on properties and indexers.

    class FullName
    {
        private string fullName;

        public FullName(string first, string last) => fullName = $"{first} {last}";

        ~FullName() => Console.WriteLine("FullName finalized");
    }

## throw Expressions
You can throw exceptions in code constructs that previously weren't allowed because throw was a statement. For example, a throw expression can now be used within an expression-bodied member
with a null-coalescing operator.

    public string Name
    {
        get => name;
        set => name = value ?? throw new ArgumentNullException(paramName: nameof(value), message: "Name cannot be null");
    }   


## Generalized async return types
Methods declared with the async modifier can return other types in addition to Task and Task<T>. The returned type must still satisfy the async pattern, meaning a GetAwaiter method must be accessible.

    public async ValueTask<int> Func()
    {
        await Task.Delay(100);
        return 5;
    }

## Numeric literal syntax improvements
New tokens improve readability for numeric constants. The 0b at the beginning of the constant indicates that the number is written as a binary number. Long binary numbers can also be broken up using the
_ (underscore) as a separator.

    public const int BinarySixteen =   0b0001_0000;
    Console.WriteLine($"BinarySixteen {BinarySixteen}");
        ==> 16

    public const long BillionsAndBillions = 100_000_000_000;
    Console.WriteLine($"BillionsAndBillions {BillionsAndBillions}");
        ==> 100000000000

    public const double AvogadroConstant = 6.022_140_857_747_474e23;
    Console.WriteLine($"AvogadroConstant {AvogadroConstant}");
        ==> 6.022140857747474E+23

    public const decimal GoldenRatio = 1.618_033_988_749_894_848_204_586_834_365_638_117_720_309_179M;
    Console.WriteLine($"GoldenRatio {GoldenRatio}");
        ==> 1.6180339887498948482045868344

