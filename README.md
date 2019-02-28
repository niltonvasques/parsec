# Parsec

A parser for Math equations using a lighter, faster and more complete version of muparserx C++ library

## Usage

#### Add to your `Gemfile`

```ruby
gem 'parsecs'
```

## Tests

```ruby
gem build parsec.gemspec
gem install ./parsecs-VERSION.gem (e.g.: gem install ./parsecs-0.5.1.gem)
ruby -Ilib -Iext/libnativemath test/test_parsec.rb
```

* Use Ruby version 2.5.1

#### You can then eval equations in your code

```ruby
parser = Parsec::Parsec
parser.eval_equation('5 + 1')
parser.eval_equation('(3 + 3) * (5 * 2)')
```

#### And also validate the syntax of the equations

```ruby
parser = Parsec::Parsec
parser.validate_syntax!('4 + 4')               # return => true
parser.validate_syntax!('4 + ')                # raise SyntaxError with message: Unexpected end of expression found at position 4.

parser.validate_syntax!('(4 + 3) + ( 4 + 2)')  # return => true
parser.validate_syntax!('(4 + 3) + ( 4 + 2')   # raise SyntaxError with message: Missing parenthesis.

parser.validate_syntax!('4==4 ? true : false') # return => true
parser.validate_syntax!('4==4 ? true')         # raise SyntaxError with message: If-then-else operator is missing an else clause.
parser.validate_syntax!('4==4 ^ true : false') # raise SyntaxError with message: Misplaced colon at position 12.
```

#### Here are examples of equations which are accepted by the parser
```ruby
parser = Parsec::Parsec

# Simple Math equations
parser.eval_equation('(5 + 1) + (6 - 2)')  # result => 10
parser.eval_equation('4 + 4 * 3')          # result => 16
parser.eval_equation('10.5 / 5.25')        # result => 2
parser.eval_equation('abs(-5)')            # result => 5
parser.eval_equation('sqrt(16) + cbrt(8)') # result => 6
parser.eval_equation('log10(10)')          # result => 1
parser.eval_equation('round(4.4)')         # result => 4
parser.eval_equation('(3^3)^2')            # result => 729
parser.eval_equation('3^(3^(2))')          # result => 19683
parser.eval_equation('10!')                # result => 3628800

# Complex Math equations
parser.eval_equation('log10(10) + ln(e) + log(10)')       # result => 4.30259
parser.eval_equation('sin(1) + cos(0) + tan(0.15722)')    # result => 2.0
parser.eval_equation('max(1, 2) + min(3, 4) + sum(5, 6)') # result => 16
parser.eval_equation('avg(9, 9.8, 10)')                   # result => 9.6
parser.eval_equation('pow(2, 3)')                         # result => 8
parser.eval_equation('round_decimal(4.559, 2)')           # result => 4.56

# IF THEN ELSE equations
parser.eval_equation('4 > 2 ? "bigger" : "smaller"')    # result => "bigger"
parser.eval_equation('2 == 2 ? true : false')           # result => true
parser.eval_equation('2 != 2 ? true : false')           # result => false
parser.eval_equation('"this" == "this" ? "yes" : "no"') # result => "yes"
parser.eval_equation('"this" != "that" ? "yes" : "no"') # result => "yes"

# Logic equations
parser.eval_equation('true and false')    # result => false
parser.eval_equation('true or false')     # result => true
parser.eval_equation('(3==3) and (3!=3)') # result => false
parser.eval_equation('exp(1) == e')       # result => true

# String equations
parser.eval_equation('length("test string")')     # result => 11
parser.eval_equation('toupper("test string")')    # result => "TEST STRING"
parser.eval_equation('tolower("TEST STRING")')    # result => "test string"
parser.eval_equation('concat("Hello ", "World")') # result => "Hello World"
parser.eval_equation('str2number("5")')           # result => 5
parser.eval_equation('left("Hello World", 5)')    # result => "Hello"
parser.eval_equation('right("Hello World", 5)')   # result => "World"

# Date equations
parsec.eval_equation("current_date()"))                        # result => "2018-10-03"
parsec.eval_equation('daysdiff(current_date(), "2018-10-04")') # result => 1
parsec.eval_equation('daysdiff("2018-01-01", "2018-12-31")')   # result => 364
```

### The following functions can be used

* Math trigonometric functions: **sin**, **cos**, **tan**, **sinh**, **cosh**, **tanh**, **asin**, **acos**, **atan**, **asinh**, **acosh**, **atanh**
* Math logarithm functions: **ln**, **log**, **log10**
* Math standard functions: **abs**, **sqrt**, **cbrt**, **pow**, **exp**, **round**, **round_decimal**
* Math constants: **e**, **pi**
* Unlimited number of arguments: **min**, **max**, **sum**, **avg**
* String functions: **concat**, **length**, **toupper**, **tolower**, **left**, **right**, **str2number**
* Complex functions: **real**, **imag**, **conj**, **arg**, **norm**
* Array functions: **sizeof**, **eye**, **ones**, **zeros**
